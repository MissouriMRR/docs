---
permalink: /simulation/containers/publishing/
---

# Publishing Container Images

[Back to Simulation Docs](/docs/simulation/)

**Most people do not need this page.** If you just want to run the simulation, go to [Installing and Configuring Containers](/docs/simulation/containers/), the images are already published and `run_container.sh` downloads them for you.

This page is for whoever maintains the images: how to build them, push them to the GitHub Container Registry, and republish them when something changes.

## Table of Contents

- [What gets published](#what-gets-published)
- [How a registry actually works](#how-a-registry-actually-works)
- [Publishing a new image for the first time](#publishing-a-new-image-for-the-first-time)
- [Building the images](#building-the-images)
- [Pushing to GHCR](#pushing-to-ghcr)
- [Things that go wrong](#things-that-go-wrong)

## What gets published

Two images, both defined in the `simulation/` folder:

| Image | Built from | Contains |
| --- | --- | --- |
| `ghcr.io/missourimrr/multirotor-env` | `Env.Containerfile` | Python 3.10, the pinned Project AirSim client, `pre-commit` |
| `ghcr.io/missourimrr/multirotor-sim` | `Sim.Containerfile` | ArduPilot compiled from source, plus the SITL launch scripts |

The `sim` image is the reason this exists at all: building it compiles ArduPilot, which takes roughly 20 minutes. Publishing it once means nobody else pays that cost.

> Both images build from the `simulation/` folder alone. They do not read anything from the competition repository around them, so a bare clone of the simulation repo is a complete build context.

## How a registry actually works
```
ghcr.io  /missourimrr/multirotor-sim:latest
└registry┘└─ owner ─┘└─── name ────┘└ tag ┘
```

Tagging an image with a name starting `ghcr.io/...` allows normal git commands (like push, pull) to function normally. Podman builds images through layers, so only the changes you make to each layer are pushed to ghcr.io, vice versa with pulling

> The owner segment **must be lowercase**. `ghcr.io/MissouriMRR/...` will not work; `ghcr.io/missourimrr/...` will.

## Publishing a new image for the first time

Use this when you are adding a *third* image, not when republishing one of the two above. Steps 1 and 2 are one-time setup for your machine and account; everything after that is per image.

### 1. Get a token and log in

Create a **classic** personal access token at [github.com/settings/tokens](https://github.com/settings/tokens) with the `write:packages` scope, then log in from WSL:

```bash
echo "YOUR_PAT_TOKEN" | podman login ghcr.io -u YOUR_GITHUB_USERNAME --password-stdin
```

The password field takes the token, not your GitHub account password. You only have to do this once per machine.

You also need permission to create packages in our organization. If you do not have it and need it, contact your project lead or the CSE for help.

### 2. Write the Containerfile

Put it in the `simulation/` folder alongside `Env.Containerfile` and `Sim.Containerfile`, and keep it self-contained. It must not read anything outside that folder, or it cannot be built from a bare clone of the simulation repo.

End the file with a source label so the published package links back to the repository:

```dockerfile
LABEL org.opencontainers.image.source=https://github.com/MissouriMRR/Simulation-2026
```

Put it at the *bottom*. A label near the top invalidates every cached layer below it.

### 3. Build it with its registry name

The name is the address, so tag it as you build rather than tagging afterwards. Pick a lowercase name that says what the image is:

```bash
podman build -f My.Containerfile -t ghcr.io/missourimrr/multirotor-mything:latest .
```

### 4. Push it

```bash
podman push ghcr.io/missourimrr/multirotor-mything:latest
```

Pushing is what creates the package. There is no separate "create package" step on GitHub.

### 5. Make the package public

**New packages are private by default**, and this is the step people forget. The symptom is confusing: teammates get a "not found" error rather than a permissions error.

1. Go to [github.com/orgs/MissouriMRR/packages](https://github.com/orgs/MissouriMRR/packages).
2. Open the package, then **Package settings**.
3. Under **Danger Zone**, choose **Change visibility** and set it to **Public**.

Visibility is per package, so a new image needs this even though the existing two are already public. You only do it once, not on every push.

### 6. Wire it into the scripts

An image nobody references does nothing. Add it in two places:

- the `image:` key for its service in `compose.yml`
- the image variables at the top of `run_container.sh`

There is a comment in each file pointing at the other. If the two disagree, `run_container.sh` pulls a name nothing uses and compose quietly builds instead.

### 7. Verify it the way a teammate will see it

Log out and pull with nothing cached:

```bash
podman logout --all
```

```bash
podman rmi ghcr.io/missourimrr/multirotor-mything:latest
```

```bash
podman pull ghcr.io/missourimrr/multirotor-mything:latest
```

A successful pull while logged out means the package is genuinely public. Deleting the local copy first is safe: your build cache is untouched, so rebuilding is quick if something is wrong.

## Building the images

Run these from inside the `simulation/` folder. Tag them with their final registry names as you build, so there is no separate tagging step:

```bash
podman build -f Env.Containerfile -t ghcr.io/missourimrr/multirotor-env:latest .
```

```bash
podman build -f Sim.Containerfile -t ghcr.io/missourimrr/multirotor-sim:latest .
```

The `sim` build is the slow one. But the layers are cached, so if you have built it before and only changed something near the bottom of the Containerfile, it will finish in seconds.

> Both Containerfiles carry an `org.opencontainers.image.source` label, deliberately placed *after* the expensive build steps. A label near the top of the file would invalidate the cached ArduPilot build and turn every rebuild into a 20-minute recompile. If you add labels, add them at the bottom.

## Pushing to GHCR

```bash
podman push ghcr.io/missourimrr/multirotor-env:latest
```

```bash
podman push ghcr.io/missourimrr/multirotor-sim:latest
```

Push the `env` image first when you are testing something — it is smaller, so a mistake surfaces faster.

> Pushing requires being logged in. If you have not done that on this machine, see [Get a token and log in](#1-get-a-token-and-log-in).

## Things that go wrong

| Symptom | Cause                                                                                                                                                       |
| --- |-------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `unable to retrieve auth token: invalid username/password: unauthorized` | A stale token in podman's saved credentials. Run `podman logout --all` and try again — public packages need no login at all                                 |
| Teammates get "not found" on pull | Go to the repository settings and change the visibility to public                                                                                           |
| `denied` on push | Your token lacks `write:packages`, or you lack package-create permission in the organization                                                                |
| Push or pull rejects the name | The owner segment is capitalized. It must be `missourimrr`, all lowercase                                                                                   |
| A trivial change triggers a 20-minute rebuild | Something was inserted near the top of `Sim.Containerfile`, invalidating the cached ArduPilot build. Move it below the `waf` step                           |
| `podman-compose` builds instead of pulling | Expected. With both `image:` and `build:` set, compose builds when an image is missing rather than pulling. That is why `run_container.sh` pulls explicitly |
