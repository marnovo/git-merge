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

### 1.1.3. Deep clean your Git repos (with LFS)

#### 1.1.3.1. Speaker

Lars Schneider

- Service Account Engineer, Github

Prem Kumar Ponuthorai

- Trainer, GitHub


#### 1.1.3.2. Notes

Content:

1. How Git works internally (also w/ LFS)
2. How to find large files/dirs in Git repos
3. How to purge files from Git repos

##### Why and how git stores things

Why make git repos smaller? Storage, performance, improve workflows.

Hashing. SHA1 explanation.

Example:

```bash
$ echo 'some text' | git hash-object --stdin

7b57bd29ea8afbdeb9bac64cf7074f4b531492a8
```

Git default deduplication using hashes.

##### How to handle large files

Example with changes in binary files such as media:

- Code: 1MB
- Media: 100MB

Even tiny changes to the binary file add another 100MB to the size of the repo. Two changes mean 301MB repo.

Problem really happens in large-scale settings: 
a 301MB repo cloned by 100 devs: 30GB network usage.

LFS server:

```bash
$ git lfs install
$ git lfs track "*.mp4"
$ git lfs ls-files
```

Goes to `.gitattributes` file.

Git lfs migrate.

##### Exercises

###### Exercise 1

- Fork and clone: https://github.com/universeworkshops/deep-clean-exercise1
  - Even though just one text file in the latest commit, repo has 1MB.

```bash
$ du -hs .git/objects

1.0M	.git/objects

$ git cat-file --batch-all-objects --batch-check

03f04d1a50eeb4c831e925b9de9983e3547b496e blob 106
12b95dbeb2e6a8a75fdb12ce9c86707e5658f1f3 tree 70
130e75bcec86843476a7caadf460add6820ee360 tree 70
7c2b6b0c5faad8cd3f6231b6abbfa87b2882e93b commit 239
b26d812218ce88965b56fa5a5112fc67f941283b blob 1024000  <-- big blob!
b69a1bd969b4b6a581d62a0a43a0d3d71983f3fa blob 96
b88ca904dac19a19e90ac0a0764d2cc6dc8414ec tree 34
e73f2010475f2bec4a483d8d78cc49da0d678353 commit 190
fddc5e0867190869523329b0a3690e6d4bc85358 commit 246

$ git rev-list --all --objects

fddc5e0867190869523329b0a3690e6d4bc85358
7c2b6b0c5faad8cd3f6231b6abbfa87b2882e93b
e73f2010475f2bec4a483d8d78cc49da0d678353
b88ca904dac19a19e90ac0a0764d2cc6dc8414ec
03f04d1a50eeb4c831e925b9de9983e3547b496e main.c
12b95dbeb2e6a8a75fdb12ce9c86707e5658f1f3
b26d812218ce88965b56fa5a5112fc67f941283b tool.exe
130e75bcec86843476a7caadf460add6820ee360
b69a1bd969b4b6a581d62a0a43a0d3d71983f3fa main.c
```

- Clone: https://github.com/larsxschneider/git-repo-analysis
- Run: `git-find-large-files`

```bash
$ ./git-find-large-files

1000  Deleted  tool.exe
```

###### Exercise 2

Remove cached via git:

```bash
$ git filter-branch --index-filter 'git rm --cached --ignore-unmatch tool.exe' -- --all

Rewrite e73f2010475f2bec4a483d8d78cc49da0d678353 (1/3) (0 seconds passed, remaining 0 predicted)    rm 'tool.exe'
Rewrite 7c2b6b0c5faad8cd3f6231b6abbfa87b2882e93b (2/3) (0 seconds passed, remaining 0 predicted)    rm 'tool.exe'
Rewrite fddc5e0867190869523329b0a3690e6d4bc85358 (3/3) (0 seconds passed, remaining 0 predicted)
Ref 'refs/heads/master' was rewritten
Ref 'refs/remotes/origin/master' was rewritten
WARNING: Ref 'refs/remotes/origin/master' is unchanged
```

That above is slow. Alternative from git-repo-analysis:

`git-purge-files '/tool\.exe$'`

Tip: always enable branch protection.

Another tool: https://github.com/github/git-sizer

###### Exercise 3

```bash
$ git clone https://github.com/$YOUR-USERNAME/deep-clean-exercise1.git

$ git lfs migrate info --everything

migrate: Sorting commits: ..., done
migrate: Examining commits: 100% (3/3), done
*.exe	1.0 MB	1/1 files(s)	100%
*.c  	202 B 	2/2 files(s)	100%

$ git lfs migrate import --everything --include="*.exe"

migrate: Sorting commits: ..., done
migrate: Rewriting commits: 100% (3/3), done
  master	fddc5e0867190869523329b0a3690e6d4bc85358 -> 0401299ea1603d42641a29d9f060ff5a1d59c75f
migrate: Updating refs: ..., done
migrate: checkout: ..., done
```

###### Q&A

In GitHub, if the original repo is deleted, content gets copied to fork.

## 1.2. Main Event

February 01, 2019
