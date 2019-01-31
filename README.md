# 1. Git Merge 2019

- Date: Jan 31–Feb 01, 2019
- Venue: [The EGG](https://goo.gl/maps/uKWqWu5V3sF2)
- [Schedule](https://git-merge.com/#schedule)
- Wi-Fi network/password: `Git Merge` / `gitexcited`

## 1.1. Workshops

January 31, 2019

### 1.1.1. Visualizing Git

#### 1.1.1.1. Speaker

Katrina Uychaco

- Software Engineer, GitHub
- github.com/kuychaco
- kuychaco@github.com

#### 1.1.1.2. Notes

Branches are refs and commits are not deleted if you delete a branch like `master`.

- [Git visualization tool](https://git-school.github.io/visualizing-git/)

- Command `head~2` point to 2 commits back.

No fast-forward:

```bash
$ git checkout -b feature
$ git commit
$ git checkout master
$ git merge feature-branch --no-ff
```

Move commits from a feature branch to the master branch:

```bash
$ git commit
$ git checkout -b feature
$ git commit
$ git checkout master
$ git checkout feature
$ git merge master
$ git rebase master
$ git reflog
```

Pushing to and pulling from remote: basically merges and rebases between branches.

Pull = fetch + merge (fast fwd)

Force pushing to rewrite history on remote:

```bash
$ git push --force
```

Amend commit message:

```bash
$ git commit --amend -m "fixed message"
```

Revert a commit: creates a new commit reverting changes from previous commit:

```bash
$ git revert head
```

Cherry-picking commits

### 1.1.2. Git hooks, aliases, and bears oh my

#### 1.1.2.1. Speaker

Mike Corsaro

- Windows Developer, Atlassian
- @F0DA

#### 1.1.2.2. Notes

- [Repo with all content](https://bitbucket.org/MikeCorsaro/git-scripts/src/master/)

##### Aliases

- [Aliases folder](https://bitbucket.org/MikeCorsaro/git-scripts/src/a9238748c6a891fdab447b5b58af8542a39a8ad7/aliases/?at=master)
- [Other Useful aliases (not from the talk)](https://github.com/lee-dohm/dotfiles/blob/8d3c59004154571578c2b32df2cdebb013517630/gitconfig#L8)

In aliases, you can add a `!` to run bash commands in the alias, e.g.:  `!git status`

You can add a script folder (and version it) and add to your `$PATH`.

##### Hooks

Automate things on git events. Client-side and server-side hooks possible.

Hooks are harder to version, because they live inside the `.git/hooks` folder.

## 1.2. Main Event

February 01, 2019
