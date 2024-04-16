---
permalink: /app/compakt_cloudflare/
---

[Back to App Home](/app/)

# Cloudflare

## Introduction

Cloudflare is the service used to host the Compakt app. It manages all of the branch deployments, user authentication, and the log database. This page outlines how to use the Cloudflare dashboard.

### Accessing the Dashboard

1) Navigate to the cloudflare [homepage](https://www.cloudflare.com/).

2) If you already have a cloudflare account associated with your school account, log in. Otherwise, go to `Log In` in the top right corner, then `Sign Up` in the login options. Create your account from there, preferably using your @mst.edu school email. Successful account creation should land you on the welcome page when you log in for the first time.

3) You will have to verify your cloudflare account by going to your school email and clicking the verification link sent by cloudflare.

4) You will have to notify your lead to add you to the MultiRotor cloudflare page. Once that is done, you should receive an invite email from cloudflare. Accept the invite by going to the link and pressing `Join`.

5) Navigate to the [cloudflare dashboard](https://dash.cloudflare.com/) if you aren't already there. If prompted with accounts to select, choose `multirotor`. This will allow you to see the Multirotor cloudflare dashboard.

### Accessing Deployments

Cloudflare deploys the production site at [compakt.pages.dev](https://compakt.pages.dev/) which can be viewed by anyone. It also deploys from specific branches (and specific commits) on the remote repository.

A branch deployment can be viewed by appending the branch name followed by a dot (.) to the beginning of the primary url. (As an example, the `feature/frontend` branch can be viewed at [feature-frontend.compakt.pages.dev](https://feature-frontend.compakt.pages.dev/)).

Commit deployments can be found by navigating to the Multirotor cloudflare dashboard and going to `Workers & Pages` -> `compakt`.

When navigating to a branch or commit deployment, you will need to enter an email to receive an authentication code. Enter the email you used to sign up with cloudflare. You will then be prompted for a 6-digit code that will be sent to your email. Enter that in to access the site.

*Note: If you are using a personal email address, branch and commit deployments are only viewable if you have been given access by your lead*

### Interacting with the Database

1) From the dashboard, press the dropdown arrow for `Workers & Pages` on the left sidebar, and select the `D1` option.

2) Click the compakt database. From here, you can see all of the compakt tables and prompt SQL statements to the console.

3) Go to the console tab and test out some the following queries:

\
See all of the tables in the database
```sql
PRAGMA table_list
```
\
See information about the testlogs columns
```sql
PRAGMA table_info(testlogs)
```
\
See everything in the testlogs table
```sql
SELECT * FROM testlogs
```
\
Preview some of the testlogs table data
```sql
SELECT id, start_time, stop_time, location FROM testlogs ORDER BY start_time LIMIT 50 OFFSET 0
```
