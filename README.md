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

### 1.1.4. Git Legit

#### 1.1.4.1. Speaker

Speaker: Pauline Vos

- Software Engineer, Werkspot

#### 1.1.4.2. Notes

##### "Atomic" commits

1. So small you can't divide it. Pertains to one fix or feature.
2. Everything works. Don't break builds.
3. Clear and concise

Problem? See http://www.commitlogsfromlastnight.com/

##### Exercise

Clone & run: https://github.com/paulinevos/money

```bash
$ git checkout 1_amend
Branch '1_amend' set up to track remote branch '1_amend' from 'origin'.
Switched to a new branch '1_amend'
```

Make a simple change and stage the change w/ add, then:

```bash
git commit --amend
[1_amend 239402f] Update README with setup instructions
 Author: Pauline Vos <pvos88@gmail.com>
 Date: Sat Jan 26 10:37:00 2019 +0100
 1 file changed, 36 insertions(+)
```

With `--amend`, you must force push because you're rewriting history.

```bash
$ git rebase -i HEAD~4

pick 44fa399 Fix division causing unnecessary fractional parts (#495)
pick 8655b0f Update change log
pick 4990791 Change Docker setup for Git workshop
pick 239402f Update README with setup instructions

# Rebase 1ec3b8e..239402f onto 1ec3b8e (4 commands)
#
# Commands:
# p, pick <commit> = use commit
```

Make different changes to previous commits:

```bash
$ git checkout 2_rebase
Branch '2_rebase' set up to track remote branch '2_rebase' from 'origin'.
Switched to a new branch '2_rebase'

$ code README.md  # making changes to a file

$ git stash

Saved working directory and index state WIP on 2_rebase: d2c0cef Updated currencies (#479)

$ git rebase -i HEAD~8

Stopped at 43ad4f8...  Fix risky tests
You can amend the commit now, with

  git commit --amend

Once you are satisfied with your changes, run

  git rebase --continue

$  git rebase --continue

[detached HEAD 6449588] Update change log 123
 Author: Mark Sagi-Kazar <mark.sagikazar@gmail.com>
 Date: Wed Sep 12 17:09:03 2018 +0200
 1 file changed, 5 insertions(+)
Stopped at 74cdd03...  Fix annotations (#491)
You can amend the commit now, with

  git commit --amend

Once you are satisfied with your changes, run

  git rebase --continue

$ git rebase --continue

Successfully rebased and updated refs/heads/2_rebase.
```

Clean-up from previous "late night" commits, making into atomic commits:

```bash
$ git checkout 3_cleanup

Branch '3_cleanup' set up to track remote branch '3_cleanup' from 'origin'.
Switched to a new branch '3_cleanup'

$ git log --oneline

$git reset HEAD~

Unstaged changes after reset:
M	composer.json
M	src/Currencies/BitcoinCurrencies.php
M	src/Currencies/CurrencyList.php
M	tests/Exchange/IndirectExchangeTest.php

$ git status

On branch 3_cleanup
Your branch is behind 'origin/3_cleanup' by 1 commit, and can be fast-forwarded.
  (use "git pull" to update your local branch)

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   composer.json
	modified:   src/Currencies/BitcoinCurrencies.php
	modified:   src/Currencies/CurrencyList.php
	modified:   tests/Exchange/IndirectExchangeTest.php

no changes added to commit (use "git add" and/or "git commit -a")

$ git log --oneline

$ git add composer.json

$ git commit -m "dependencies"

[3_cleanup 8bdbb4c] dependencies
 1 file changed, 1 insertion(+), 1 deletion(-)

$ git add tests/Exchange/IndirectExchangeTest.php

$ git commit -m "fix risky tests"

[3_cleanup 5045f96] fix risky tests
 1 file changed, 1 insertion(+), 1 deletion(-)

$ git add src/Currencies/BitcoinCurrencies.php

$ git add src/Currencies/CurrencyList.php

$ git commit -m "update currencies"

[3_cleanup 3ca5e77] update currencies
 2 files changed, 2 insertions(+), 2 deletions(-)

$ git rebase -i HEAD~10

Successfully rebased and updated refs/heads/3_cleanup.
```

##### How to write good commit messages

https://chris.beams.io/posts/git-commit/

> 1. Separate subject from body with a blank line
> 2. Limit the subject line to 50 characters
> 3. Capitalize the subject line
> 4. Do not end the subject line with a period
> 5. Use the imperative mood in the subject line
> 6. Wrap the body at 72 characters
> 7. Use the body to explain what and why vs. how

##### Git bisect

- [A beginner's guide to GIT BISECT - The process of elimination](https://www.metaltoad.com/blog/beginners-guide-git-bisect-process-elimination)

With the PHP Composer from the Docker image open:

```bash
$ docker run --rm -it -v "$PWD":/money -w /money synon/git-legit:latest

Unable to find image 'synon/git-legit:latest' locally
latest: Pulling from synon/git-legit
5e6ec7f28fb7: Pull complete
cf165947b5b7: Pull complete
7bd37682846d: Pull complete
99daf8e838e1: Pull complete
cd02dfcf7539: Pull complete
209221f5bdfd: Pull complete
5200ddc081c5: Pull complete
f60e885ce860: Pull complete
d9c1a7b321fd: Pull complete
fedde42ee51a: Pull complete
cc30044c7bfe: Pull complete
Digest: sha256:63fac4799a6c700d8b583da5204b39506d555c5d7aceef24dc36319c88119202
Status: Downloaded newer image for synon/git-legit:latest

root@943a1b6361cb:/money$ git checkout 4_bisect

Branch 4_bisect set up to track remote branch 4_bisect from origin.
Switched to a new branch '4_bisect'

root@943a1b6361cb:/money$ git log --oneline

root@943a1b6361cb:/money$ git bisect good c0be76c

root@943a1b6361cb:/money$ git bisect bad fa21802

root@943a1b6361cb:/money$ git bisect reset
```

##### Cheat sheet

- [Cheat-sheet](http://www.pauline-vos.nl/git-legit-cheatsheet/)

### 1.1.5. Ten Git problems & how to fix them

#### 1.1.5.1. Speaker

Briana Swift

- Trainer, GitHub
- @brianamarie

#### 1.1.5.2. Notes

Slides: https://brianamarie.github.io/10-git-problems/

Clone: https://github.com/brianamarie/10-git-problems

##### Problem 1: Commit is too big

[Instructions](https://brianamarie.github.io/10-git-problems/#5)

```bash
$ 01-committed-too-big

$ ./setup.sh
[master b280591] creating files

 4 files changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 01-committed-too-big/file1.md
 create mode 100644 01-committed-too-big/file2.md
 create mode 100644 01-committed-too-big/file3.md
 create mode 100644 01-committed-too-big/file4.md
[master a5d608e] committing content for file1
 1 file changed, 15 insertions(+)
[master 587bf0c] add markdown to file1
 1 file changed, 11 insertions(+), 5 deletions(-)

$ ls

README.md   file1.md    file2.md    file3.md    file4.md    next.sh     reset.sh    setup.sh    tutorial.md

$ git lol

* 587bf0c (HEAD -> master) add markdown to file1
* a5d608e (tag: reset-here) committing content for file1
* b280591 creating files
* cf2cf33 (tag: v1.0, tag: before-activity, origin/master, origin/HEAD) update force-pull activity
* aec5579 add activity files
* c588ca1 initial commit with draft README

$ git reset reset-here

Unstaged changes after reset:
M	01-committed-too-big/file1.md

$ git status
On branch master
Your branch is ahead of 'origin/master' by 2 commits.
  (use "git push" to publish your local commits)

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   file1.md

no changes added to commit (use "git add" and/or "git commit -a")

$ git add -p
diff --git a/01-committed-too-big/file1.md b/01-committed-too-big/file1.md
index e60f5da..0f75f38 100644
--- a/01-committed-too-big/file1.md
+++ b/01-committed-too-big/file1.md
@@ -1,15 +1,21 @@
+# Git
+
 Git is a free and open source distributed version control system designed to handle everything from small to very large projects with speed and efficiency.

 Git is easy to learn and has a tiny footprint with lightning fast performance. It outclasses SCM tools like Subversion, CVS, Perforce, and ClearCase with features like cheap local branching, convenient staging areas, and multiple workflows.

-Branching and Merging
+### Branching and Merging
 The Git feature that really makes it stand apart from nearly every other SCM out there is its branching model.

 Git allows and encourages you to have multiple local branches that can be entirely independent of each other. The creation, merging, and deletion of those lines of development takes seconds.

 This means that you can do things like:

-Frictionless Context Switching. Create a branch to try out an idea, commit a few times, switch back to where you branched from, apply a patch, switch back to where you are experimenting, and merge it in.
-Role-Based Codelines. Have a branch that always contains only what goes to production, another that you merge work into for testing, and several smaller ones for day to day work.
-Feature Based Workflow. Create new branches for each new feature you're working on so you can seamlessly switch back and forth between them, then delete each branch when that feature gets merged into your main line.
-Disposable Experimentation. Create a branch to experiment in, realize it's not going to work, and just delete it - abandoning the work—with nobody else ever seeing it (even if you've pushed other branches in the meantime).
+- Frictionless Context Switching
+- Create a branch to try out an idea, commit a few times, switch back to where you branched from, apply a patch, switch back to where you are experimenting, and merge it in.
+Role-Based Codelines
+- Have a branch that always contains only what goes to production, another that you merge work into for testing, and several smaller ones for day to day work
+- Feature Based Workflow
+- Create new branches for each new feature you're working on so you can seamlessly switch back and forth between them, then delete each branch when that feature gets merged into your main line
+- Disposable Experimentation
+- Create a branch to experiment in, realize it's not going to work, and just delete it - abandoning the work—with nobody else ever seeing it (even if you've pushed other branches in the meantime)
Stage this hunk [y,n,q,a,d,s,e,?]?
```

##### Problem 2: Accidental Commit

[Instructions](https://brianamarie.github.io/10-git-problems/#6)

##### Problem 3: I'm in a detached head state

[Instructions](https://brianamarie.github.io/10-git-problems/#7)

##### Problem 4: What broke my code?

[Instructions](https://brianamarie.github.io/10-git-problems/#8)

##### Problem 5: I want to merge two repositories

[Instructions](https://brianamarie.github.io/10-git-problems/#9)

##### Problem 6: I want to undo a recursive merge (without GitHub)

[Instructions](https://brianamarie.github.io/10-git-problems/#10)

##### Problem 7: I need Git to ignore a binary file

[Instructions](https://brianamarie.github.io/10-git-problems/#11)

##### Problem 8: How do I remove a submodule?

[Instructions](https://brianamarie.github.io/10-git-problems/#12)

##### Problem 9: I want to undo a rebase

[Instructions](https://brianamarie.github.io/10-git-problems/#13)

##### Problem 10: Can I force pull from the remote?

[Instructions](https://brianamarie.github.io/10-git-problems/#14)

### 1.1.6. Git Workflows

#### 1.1.5.1. Speaker

Johan Abildskov

- Trainer & Consultant, Praqma
- @randomsort

#### 1.1.5.2. Notes

Survey with participants on git pros/cons.

##### Comparing Git Workflows

- [Centralized workflow]() (a.k.a. "push to master")
- [Git Common Flow](https://commonflow.org/) (based on [GitHub flow](https://guides.github.com/introduction/flow/))
- [Gitflow workflow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow)
- [Git Phlow]() (from [Praqma repo](https://github.com/Praqma/git-phlow))

|                      	| Difficulty 	| Tooling 	| Problems 	|
|----------------------	|:----------:	|:-------:	|:--------:	|
| Centralized Git Flow 	|      ✅     	|    ✅    	|     🆚    	|
| Git Common Flow      	|      ✅     	|    ✅    	|     ✅    	|
| Gitflow              	|      🆚     	|    🛑    	|     🆚    	|
| Git Phlow            	|      🛑     	|    🆚    	|     🛑    	|

Obs: - [Atlassian comparison](https://www.atlassian.com/git/tutorials/comparing-workflows).

## 1.2. Main Event

February 01, 2019
