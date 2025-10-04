# funcwords-gh-actions
Github Actions config for Funcwords project + a tutorial

## Setup Github Secrets
In your production repository (e.g. `funcwords-prod`), click "Settings" tab > "Secrets and variables" in "Security" section on the left > "Actions". Then add the following 4 repository secrets by clicking "New repository secret" button (*NAME* - *description*):

* APP_PATH - your name for the directory inside home (~) that will contain the repo (e.g., `the-app`)
* HOST - IP address of VPS
* USER - user of VPS (e.g., `root`)
* PASS - SSH password

## Setup Workflow
In your production repository, click "Actions" tab > "New workflow" > "set up a workflow yourself" > Copy the contents of `main.yaml` > "Commit changes..." > Edit commit message if you like > Commit changes.

## Setup Personal Access Token (PAT)
Click the profile picture of your account at top right > "Settings" > "Developer Settings" (bottom of the left section) > "Personal Access Tokens" > "Fine-grained tokens" > "Generate new token" > Confirm access via TOTP/Password.

* Set your name, and description if you like.
* Expiration = "No expiration".
* Repository access = "Only select repositories" > select your production repository.
* Permissions = select only "Contents" > Access = Read-only.

Then click "Generate Token". Follow the "copy your personal access token now".

## Setup Repo Directory in VPS
SSH to your VPS. After login, you'll be in home directory. Then perform the git clone using PAT:
```bash
git clone https://<username>:<github_pat_your_copied_token>@github.com/<username>/funcwords-prod.git
```
Finally rename the new directory to the value of APP_PATH (e.g., `mv funcwords-prod the-app`).

## Sparse Checkout
Do this to exclude files or directories when doing git pull.
```bash
git config core.sparsecheckout true
```
Then create the file `.git/info/sparse-checkout` with this content:
```
/*
!other/
```
The first line says to include everything, and the second line excludes the `other` directory.
