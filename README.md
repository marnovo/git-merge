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

## 1.2. Main Event

February 01, 2019
