# github-webhooks-test

This is an example repository to show how to set up Gitlab-CI on Github via mirroring. Check out `.gitlab-ci.yml` in master for how to run the CI, or in the `mirror` branch for mirroring magic. To learn how to set this up in your own project, read on.

## Step One: Set up GitLab
- Create an empty repository in Gitlab.
- Create a new ssh key pair

```
ssh-keygen -t rsa -b 2048
```

- Add the public key to the Gitlab repository as a deploy key _with write access_.
- Add the private key to the CI variables as `MIRROR_KEY`
- Add the host keys for GitHub and Gitlab to the CI variable `SSH_HOSTKEYS`
- Create a pipeline trigger token for the project and copy the webhook URL. Replace `REF` with master and token with the newly generated token.

## Step Two: Set up GitHub
- Create an orphan branch named `mirror`. Add the `.gitlab-ci.yml` from the `mirror` branch in this repository.
    - Make sure to update the necessary variables
        - `GITLAB_REPO_URL`
        - `GITHUB_REPO_URL`
        - `GITHUB_ORG`
        - `GITHUB_REPO_NAME`
- Add the `.gitlab-ci.yml` file from `master` to the GitHub repository branch on which the CI should run. Update the variables here as well.
    - **Note:** If `.gitlab-ci.yml` already exists and cannot be discarded, manually merge in the status steps.
- Create an access token with permissions to change statuses. Add it to the gitlab CI variables as `GITHUB_TOKEN`.
- Create a new webhook using the pipeline trigger URL from Gitlab.

## Step Three: Initial Sync
- Clone the GitHub repository to your computer git clone --bare git@github.com:<org>/<repo>.git
- Perform a mirror push to Gitlab `git push --mirror git@<Gitlab URL>:<org>/<repo>

## Step Four: Watch it work
- Remember, NEVER commit to the gitlab repository.
