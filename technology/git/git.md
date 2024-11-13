---
permalink: /git/
---

# Git

[Back to Software Docs](/docs/)

Git is a version management tool used by our team to keep track of code changes. Specifically, our repos are hosted on [GitHub](https://github.com/MissouriMRR). On our GitHub, you will find code for various projects and competitions.

## What is Git?
- Version Control System (VCS) for tracking changes in files
- Work independently on different features
- Merge your code with everyone else’s code
- Visit or revert back to any previous version at any time

## What is GitHub?
- Online platform for hosting VCS
- Collaborate on a repository
- Intuitive UI
- Additional features for software development (issues, projects, etc.)

## Repositories
- Essentially a specific folder (either remote or local) with version tracking
- Commits
    - A “checkpoint” that denotes a specific point in the repo
- Branches
    - Offshoot of the main code base with its own version history

## Local vs. Remote Repository
Local Repository:
- Files stored on your computer drive.
- Your own “version” of the files.
- Changes here don’t affect the outside world

Remote Repository:
- Where you “push” or upload your code for review
- Anyone and everyone can see these changes
- Merged changes here are part of the latest release code

## Git

### Installation - Command Line

Linux (Debian/Ubuntu): `sudo apt install git`

Linux (Fedora): `sudo dnf install git` or `sudo yum install git`

Mac: [https://git-scm.com/download/mac](https://git-scm.com/download/mac)

Windows: [https://git-scm.com/download/win](https://git-scm.com/download/win)

### Installation - GUIs

You will still need command line!

- GitHub Desktop
    - Windows/Mac: [https://desktop.github.com/](https://desktop.github.com/)
    - Linux: [https://github.com/shiftkey/desktop](https://github.com/shiftkey/desktop)
- Tons more options
    - [https://git-scm.com/downloads/guis](https://git-scm.com/downloads/guis)

### Basic Commands

- `git add` - Add files to Index, “stage” for commit
- `git commit` - Create a “checkpoint” in the repository
- `git log` - Show log of commits
- `git diff` - Show differences between commits
- `git branch` - Create or manage branches
- `git checkout` - Create and navigate between branches
- `git switch` - Switch to a different branch
- `git rm` - Remove files
- `git merge` - Merges other branch into currently checked out branch
- `git status` - Check status of working tree
- `git push` - Push local repository to remote repository
- `git pull` - Update local repo with the latest changes from remote repo

### Need help with a git command?

Use `git help [COMMAND]`

This lists out how to use the command and possible flags to use alongside the command for different functionality.

Example Usage:
- Wanna know how to use `git clone`? Then, use `git help clone`

Note: `git help` can also be used as a standalone command or as `git help --all` to list the most common or all available commands, respectively

### Commits

Commits create a snapshot of the current state of your code.

`git add <file>`

`git commit -m “YOUR COMMIT MESSAGE”`

How to write commit messages: [https://chris.beams.io/posts/git-commit/](https://chris.beams.io/posts/git-commit/)

### Branching

Branches are a set of code changes that can operate without affecting the main codebase.

Essentially, a branch has a separate history from the main codebase.

`git branch <branch_name>`

`git checkout <branch_name>`

To create a new branch, use `git checkout -b “feature/<branch_name>”`

Note: the “feature/” part is there as our naming system for our branches. "feature/" is the most common type of naming convention you will use; it indicates a new feature will be added to the branch. You can also use "bugfix/" to indicate your branch is for smaller updates and fixes to the current code.

### Pushing

Pushing updates the remote branch with the current commit on your local branch.

`git push` or `git push origin master`

If you’re working on a local checked out branch you’ll have to set your remote counterpart with `git push --set-upstream-to origin <branch_name>`

### Merging

Merging unifies the code of two different branches.

`git merge <other_branch>`

Note that this will merge other_branch into the branch you’re currently on.
Only do this after you’ve committed, pushed, and been approved for your current branch’s code.

### Merge Conflicts

Sometimes the changes on two different branches are conflicting. Git doesn’t know how to decide between them, so you must choose manually.

### Pulling

This is how you can update your local repo with the latest changes from the remote repo

`git pull`

Pulling will merge changes from the remote branch into your local branch.

This may happen automatically or could result in merge conflicts. 

## GitHub

### Pull Requests

Pull requests are mainly for merging a remote branch into the master branch.

Leave comments, request reviews, make new commits until it’s ready.

### Issues

Issues track problems/to-dos in a repository.

They can be given description, to do actions, and be assigned to specific people.

### Projects

Project boards show what tasks need to be done, who is working on which tasks, and what has been completed.

Can be linked to address certain Issues!


## More Resources

- Official Github documentation: [https://docs.github.com/en](https://docs.github.com/en)
- More about VCS: [https://www.atlassian.com/git/tutorials/what-is-version-control](https://www.atlassian.com/git/tutorials/what-is-version-control)
- Visualizing Git, animated for each git command: [https://onlywei.github.io/explain-git-with-d3/](https://onlywei.github.io/explain-git-with-d3/)
- Interactive visual tutorial with animations: [https://learngitbranching.js.org/](https://learngitbranching.js.org/)
    - Level 1 on the Main and Remote categories should be sufficient
- How to write commit messages: [https://chris.beams.io/posts/git-commit/](https://chris.beams.io/posts/git-commit/)
