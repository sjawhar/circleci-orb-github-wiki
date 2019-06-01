## Purpose
The `sjawhar/github-wiki` orb automates the building and deployment of documentation to the wiki repo available every GitHub repository.

What? You didn't know that the wiki tab was controlled by a git repo? Well, it's true! And this orb will help you make the most of it.

One cool feature is the ability to automatically populate your sidebar files with links to page headers. See the sidebar on the side of this page? That was generated using this orb! Just pop a placeholder string (default is `{{SIDEBAR_POPULATE}}`) into your `__Sidebar.md` files and let'er rip. Check out the [docs folder](../tree/master/docs) of this repo to see more.

## Setup
1. Go to the wiki tab of you GitHub repo and create the first page. Content doesn't matter.
2. Add a deploy key **with write access** to your GitHub repo.
3. Add the private key to CircleCI under SSH permissions with hostname "github.com"
4. Add the `sjawhar/github-wiki` orb to your CircleCI config.yml. See the examples below.

## Jobs
### build-and-deploy
Populate sidebar files with page header links, commit to wiki repo, and deploy.

**Parameters**
* `commit-user-name`: User name with which wiki changes should be commited. Default: `CircleCI`
* `commit-user-email`: User email with which wiki changes should be commited. Default: `circleci@wiki`
* `deploy-branch-filter`: Only push to wiki repo on this branch. Default: all branches
* `sidebar-placeholder`: String in sidebar files to replace with page header links. Default: `{{SIDEBAR_POPULATE}}`
* `ssh-key-fingerprint`: Fingerprint of the SSH key with push access to your repo.
* `wiki-folder-path`: Path to directory containing wiki files. Default: `docs`

## Commands
### populate-sidebars
Populate sidebar files with page header links.

**Parameters**
* `sidebar-placeholder`
* `wiki-folder-path`

### deploy-wiki-repo
Copy files to wiki repo and commit.

**Parameters**
* `commit-user-name`
* `commit-user-email`
* `deploy-branch-filter`
* `ssh-key-fingerprint`
* `wiki-folder-path`

## Examples
### Build and deploy on master
```yaml
version: 2.1

orbs:
  github-wiki: sjawhar/github-wiki@0.2.1

workflows:
  deploy-wiki:
    jobs:
      - github-wiki/build-and-deploy:
          commit-user-email: circleci+orb-github-wiki@thecybermonk.com
          ssh-key-fingerprint: YOUR_SSH_JEY_FINGERPRINT_HERE
          filters:
            branches:
              only: master
```

### Build on branches, deploy on master
```yaml
version: 2.1

orbs:
  github-wiki: sjawhar/github-wiki@0.2.1

workflows:
  deploy-wiki:
    jobs:
      - github-wiki/build-and-deploy:
          commit-user-email: circleci+orb-github-wiki@thecybermonk.com
          ssh-key-fingerprint: YOUR_SSH_KEY_FINGERPRINT_HERE
          deploy-branch-filter: master
```
