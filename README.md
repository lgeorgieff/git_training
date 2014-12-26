# Description
This repository is thought for learning git and therefore contains the most important commands for git in a very basic way.
The author most often doesn't use prosa text but heavily uses some basic git commands in favour of providing lean examples.

# Configuration
1. Set the user name locally (for this repository)
2. Set the email address locally (for this repository)
3. Set the default editor globally (for the current user)
4. List all configurations

```sh
# details: man git-config
1> git config --local user.name "John Doe"
2> git config --local user.email john.doe@email.com
3> git config --global core.editor emacs
4> git config --list
```

# Getting a Repository
1. Initialize the working directory as an empty git repository
2. Clone the repository _git\_training_ into the subfolder _git\_training_
3. Clone the repository _git\_training_ into the subfolder _training_
4. Clone the branch _my\_branch_ of the repository _git_training_ into the subfolder _git\_training_ (the _--branch_ switch also allows to clone tags)

```sh
# details: man git-init
1> git init
# details: man git-clone
2> git clone git@github.com:lgeorgieff/git_training.git
3> git clone git@github.com:lgeorgieff/git_training.git training
4> git clone --branch my_branch git@github.com:lgeorgieff/git_training.git
```

# Working with Files
1. Request the current status of the repository
2. Request the current status of the repository in short form
3. Add the file _README.md_ to the repository stage the file _README.md_ and add its last changes to the next commit
4. Check unstaged changes
5. Check all staged changes that will go into the next commit
6. Identify whitespace errors (whitespace errors are defined by _core.whitespace_)
7. Remove the file _README.md_ from the repository and the file system
8. Remove the file _README.md_ from the repository but leave it on the file system
9. Moves the file _README.md_ to _README.txt_ in the repository and on the file system
10. Clean the working directory and all sub-directories except files matched by _.gitignore_
11. Show what would be done if `git clean -d` is executed
12. Clean the working directory and all sub-directories inclusive files matched by _.gitignore_

```sh
# details: man git-status
 1> git status
 2> git status -s
# details: man git-add
 3> git add README.md
# details: man git-diff
 4> git diff
 5> git diff --staged
 6> git diff --check
# details: man git-rm
 7> git rm README.md
 8> git rm --cached README.md
# details: man git-mv
 9> git mv README.md README.txt
# details: man git-clean
10> git clean -d
11> git clean -d -n
12> git clean -d -n -x
```

## .gitignore
1. Ignore all files that ends with _~_
2. Ignore the folder _/tmp_
3. Ignore the file _/TODO_
4. Ignore all files with the name _TODOs_
5. Ignore all _.docx_ files in the folder _/doc/*/_
6. Track the file _important.docx_. This will also track files with the name _important.docx_ in _/doc/*/_.

```sh
# details: man gitignore
1> *~
2> /tmp/
3> /TODO
4> TODOs
5> /doc/*/*.docx
6> !important.docx
```

# Committing Changes
1. Commit all staged changes to the local repository
2. Stage all unstaged changes and commit it along with the comment _"..."_
3. Show the log of the repository
4. Show the log of the repository in the form of a commit per line
5. Show the log of the repository including a diff for each commit
6. Show the log of the repository including a compact diff for each commit
7. Show the log of the repository formatted as "short hash - author name, author date : subject"
8. Show the log of the repository in the form of a commit per line and displaying the committs and branches as a graph
9. Show the log of the repository for the last two days
10. Show all logs with a short SHA-1 value
11. Show the log message of the commit _940c980_

```sh
# details: man git-commit
 1> git commit
 2> git commit -a -m "..."
# details: man git-log
 3> git log
 4> git log --oneline
 5> git log -p
 6> git log --stat
 7> git log --pretty=format:"%h - %an, %ar : %s"
 8> git log --oneline --graph
 9> git log --since=2.days
10> git log --abbrev-commit
# details: man git-show
11> git show 940c980
```

# Changing the History
1. Change the last commit
2. Reset all unstaged changes of the file _README.md_ to the version of the last commit
3. Reset the local repository to the commit _36b01dc_
4. Checkout the last version of _README.md_ and replace the local file on the file system by it
5. Change the history (commits) on top of the commit _36b01dc_
6. List all SHA-1 values for commits where the _HEAD_ and branch references pointed to. This can be very helpful after rewriting the history by using _git rebase_, since the overwritten references are still tracked in the _reflog_
7. Show the hash of the commit on which the master reference is pointing
8. Create a new commit that undoes the changes made in the commit _2c1e336_

```sh
# details: man git-commit
1> git commit --amend
# details: man git-reset
2> git reset HEAD README.md
3> git reset 36b01dc --hard
# details: man git-checkout
4> git checkout -- README.md
# details: man git-rebase
5> git rebase -i 36b01dc
# details: man git-reflog
6> git reflog
# details: man git-rev-parse
7> git rev-parse master
# details: git-revert
8> git revert 2c1e336
```

## Rebasing
Rebasing allows to integrate two branches into a "single line" (after the rebase it seems that there existed only one branch). The results of rebasing are similar to merging except that there is only one final branch. An example rebase works as follows:

1. Checkout the branch _my\_branch_ which changes should be put on top of the branch _master_
2. Rebase the current branch on top of the branch _master_
3. Checkout the rebased branch _master_
4. Perform a _fast-forward_ merge (since the _master_ pointer is behind the _my\_branch_ pointer)

```sh
1> git checkout my_branch
2> git rebase master
3> git checkout master
4> git merge
```

## Interactive Rebasing
Interactive rebasing allows to change the entire history of the git repository.
With _git rebase -i <commit hash>_ it is possible to select the start point from which the history is changed, i.e. the first changeable commit is the commit that follows after the specified one.
Afterwards the default editor appears with all commits listed (on top of the specified one). It is possible to
* __reorder__: by changing the order of the commits
* __pick__: (default) each commit that is annotated as _pick_ is used in the history
* __reword__: the commit message is changed
* __edit__: the entire commit can be changed (sources and commit message)
* __squash__: the commit is melt into the previous commit, the commit message can be changed
* __fixup__: the commit is melt into the previous commit, but the commit message of the current commit is discarded
* __exec__: the specified command at the end of the line is executed

the commits.

Note the history is rewritten, i.e. there are used different commit IDs and usually a _git push --force_ is required to write the changes into the remote repository. Also take into account, that others working with the current version of the repository may get in real trouble since their version of the repository won't exist after you commit the rebased changes.

1. Start the rebase process on top of the commit _9fc7ad5_
2. Stage the file _README.md_
 * Add the latest changes of the file _README.md_ of the current commit to the staging area
 * Or resolve a conflict of the file _README.md_ caused by the changes already done and the next commit
4. Change the currently selected commit in the rebase process
5. Go to the next commit during the rebase process (is required after amending a commit or resolving a conflict)
6. Abort the entire rebase process and revert all done changes during the rebase process

```sh
1> git rebase -i 9fc7ad5
2> git add README.md
3> git commit --amend
4> git rebase --continue
5> git rebase --abort
```

# Branching
1. Create a new local branch named _new\_branch_
2. Create a new local branch named _very\_new\_branch_ based on the branch _new\_branch_
3. List all local branches
4. List all local and remote branches
5. List all branches that were merged into the current branch
6. List all brnaches that were not merged into the current branch
7. Switch into the branch _new\_branch_ (tis branch must already exist)
8. Create the branch _new\_branch_ and immediately switch to it
9. Merges the branch _new\_branch_ into the current branch
10. Show the common ancestor version of the file _README.md_ when your repository is in a conflict state
11. Show the version of the current branch of the file _README.md_ when your repository is in a conflict state
12. Show the version of the file _README.md_ from the branch your are merging in when your repository is in a conflict state
13. Try to revert the merge and go back to the state before the merge operation
14. Merge the branch _origin/master_ into the current branch while using the _theirs_ merge strategy, i.e. the changes from _origin/master_ are privileged
15. Merge the branch _origin/master_ into the current branch while using the _ours_ merge strategy, i.e. the changes from the current branch are privileged
16. Delete the local branch _new\_branch_
17. Delete the remote branch _new\_branch_ (the local branch remains)
18. Push the current branch to _origin/new\_branch_
19. Push the branch _new\_branch_ to _origin/hotfix\_1_
20. Push the branch _new\_branch_ to _origin/other\_branch_ and directly set the upstream branch for _new\_branch_ to _origin/other\_branch_
21. Checkout the remote branch _origin/hotfix\_2_ into the local (and tracking) branch _hotfix\_2_
22. Checkout the remote branch _origin/hotfix\_3_ into the local (and tracking) branch _very\_new\_branch_
23. Set the upstream branch of the current branch to _origin/hotfix\_4_
24. Rename the current branch to _new\_branch\_name_

```sh
# details: man git-branch
 1> git branch new_branch
 2> git branch very_new_branch origin/new_branch
 3> git branch
 4> git branch -a
 5> git branch --merged
 6> git branch --no-merged
 7> git checkout new_branch
 8> git checkout -b new_branch
 9> git merge new_branch
10> git show :1:README.md > README.md.common
11> git show :2:README.md > README.md.ours
12> git show :3:README.md > README.md.theirs
13> git merge --strategy-option=theirs origin/master
14> git merge --strategy-option=ours origin/master
15> git merge --abort
16> git branch --delete new_branch
17> git push origin --delete new_branch
18> git push origin new_branch
19> git push origin new_branch:hotfix_1
20> git push --set-upstream origin new_branch:other_branch
21> git checkout --track origin/hotfix_2
22> git checkout -b very_new_branch origin/hotfix_3
23> git branch --set-upstream origin hotfix_4
24> git branch -m new_branch_name
```

# Tagging
1. List all existing tags.
2. List all existing tags starting with _v2._
3. Create an annotated tag named _v2.5_ with the commit message _..._
4. Show information for the tag _v2.5_
5. Create a lightweight tag named _v2.5.1_
6. Create an annotated tag named _v2.6_ for the commit _bc8bc0c_
7. Push the tag _v2.5_ to _origin_ (by default tags are not pushed to remote repositories)
8. Push all tags to _origin_
9. Checkout the tag _v2.5_
10. Checkout the tag _v2.5_
11. Produce a human readable (version) name based on the nearest tag and the number of commits on top of it followed by a short SHA-1 value
12. Generate an archive of the current state of the branch _master_ (for those who have no git and cannot use _git tag_)

```sh
# details: man git-tag
 1> git tag
 2> git tag -l "v2.*"
 3> git tag -a "v2.5" -m "..."
 4> git show v2.5
 5> git tag v2.5.1
 6> git tag -a v2.6 bc8bc0c
 7> git push origin v2.5
 8> git push origin --tags
 9> git checkout v2.5
10> git checkout tags/v2.5
# details: man git-decribe
11> git describe master
# details: man git-archive
12> git archive master | gzip > $(git describe master).tar.gz
```

# Remote Repositories
1. List all configured remote entities
2. Add the remote entity _<uri>_ as _remote_entity_ in addition to _origin_
3. Show additional information of the remote entity _remote_entity_
4. Rename the remote entity _remote_entity_ to _re_
5. Remove the remote entity _re_
6. Fetch data from the remote entity _remote_entity_
7. Fetch data from the remote entity _origin_ (default)
8. Merge the fetched _master_ branch from _remote_entity_ into the current branch
9. Pull (fetch and merge) data from the remote entity _remote_entity_
10. Pull data from the remote entity _origin_ (default)
11. Push the local commits to the remote entity _remote_entity_
12. Push the local commits to the remote entity _origin_ (default)
13. Generates output that summarizes the forked work inside the local branch _forked\_branch_ which can be sent to the maintainer of the remote repository _origin/master_. Finally, the maintainer of _origin/master_ can pull the changes from _forked\_branch_ and integrate them into _origin/master_

```sh
# details: man git-remote
 1> git remote -v
 2> git remote add remote_entity <uri>
 3> git remote show remote_entity
 4> git remote rename remote_entity re
 5> git remote rm re
# details: man git-fetch
 6> git fetch remote_entity
 7> git fetch
 8> git merge remote_entity/master
# details: man git-pull
 9> git pull remote_entity
10> git pull
# details: man git-push
11> git push remote_entity
12> git push
# details: man git-request-pull
13> git request-pull origin/master forked_branch
```

# Patches
1. Check whether the patch _/tmp/hotfix_1.patch_ which was generated with the command _git diff_ applies cleanly to the current branch
2. Apply the patch _/tmp/hotfix_1.patch_ which was generated with the command _git diff_ to the current branch
3. Apply the patch(es) stored in _/tmp/hotfix_1.patch_ which are in the _mbox_ format and may contain several patches (generated with _git diff_). In addition meta data is also applied, such as commit message, author, ... (_git am_ uses _git apply_ in the background)
4. Set the current state to resolved after a conflict resulting from applying a patch with _git am_ was manually resolved

```sh
# details: man git-apply
1> git apply --check /tmp/hotfix_1.patch
2> git apply /tmp/hotfix_1.patch
# details: man git-am
3> git am /tmp/hotfix_1.patch
4> git am --resolved
```

# Advanced Syntax

## Ancestry References
With the _^_ character at the end of a reference it is possible to get the first parent of the specified reference. _^_ is a shothand for _^1_. It is also possible to select the second parent of a reference by specifying _^2_. This syntax is useful for merge commits only.

With the _~_ character at the end of a reference it is possible to get the first parent of the specified reference. _~_ is a shorthand for _~1_. It is also possible to select an arbitrary parent of a reference by specifying a number after the _~_ character, e.g. _~4_ for the fourth (first) parent.


1. Show the first parent of _HEAD_
2. Show the first parent of _HEAD_
3. Show the second parent of _8d8e119_
4. Show the first parent of _HEAD_
5. Show the first parent of _HEAD_
6. Show the first parent of the first parent of _8d8e119_
7. Show the first parent of the second parent of _HEAD_

```sh
1> git show HEAD^
2> git show HEAD^1
3> git show 8d8e119^2
4> git show HEAD~
5> git show HEAD~1
6> git show 8d8e119~2
7> git show HEAD^2~1
```

## Commit Ranges
1. Show all committs between the commit _1822178_ and _HEAD_
2. Show all committs between the commit _1822178_ and _HEAD_
3. Show all committs between the commit _1822178_ and _b9e0daa_
4. Show all commits that belong to the current branch which is referenced by _HEAD_ but are not merged into the _origin/master_ branch

```sh
1> git log 1822178..HEAD
2> git log 1822178
3> git log 1822178..b9e0daa
4> git log origin/master..HEAD
```

# Stash
1. Add the uncommitted changes from tracked files to the stash area
2. List all changes stored in the stash
3. Apply the stash _stash@{0}_ to the working directory
4. Apply the stash _stash@{2}_ to the working directory
5. Apply the stash _stash@{0}_ to the working directory and to the staging area
6. Drop the changes stored in _stash@{0}_ from the stash area without applying it
7. Apply the changes stored in _stash@{2}_ to the working directory and drop it finally
8. Add the uncommitted changes of tracked and untracked files to the stash area
9. Create the new branch _new\_branch_, checkout the commit the stashed work depends on and apply the stashed work on the new branch
10. Show a summary of the changes stored in the stash _stash@{0}_
11. Show a summary and a diff of the changes stored in the stash _stash@{2}_

```sh
# details: man git-stash
 1> git stash
 2> git stash list
 3> git stash apply
 4> git stash apply stash@{2}
 5> git stash apply --index
 6> git stash drop stash@{0}
 7> git stash pop stash@{2}
 8> git stash --include-untracked
 9> git stash branch new_branch
10> git stash show
11> git stash show stash@{2} -p
```

# Searching
1. Search for all occurrences of ".\*stash.\*" in all tracked files while ignoring lower and upper case and displaying the line of each occurrence
2. Search for all occurrences of ".\*stash.\*" that most probably are used as functions or methods in all tracked files
3. Search for all occurrences of ".\*stash.\*" that exist within files ending with _.md_
4. Search for all log messages that contain the substring _stash_ while ignoring upper and lower case
5. Show what revision and author last modified each line of the file _README.md_
6. Show what revision and author last modified the lines _10_ to _15_ of the file _README.md_
7. Show what revision and author last modified the lines _10_ to _15_ of the file _README.md_
8. Show what revision and author last modified the lines _10_ to _15_ of the file _README.md_ inside the branch _master_
9. Show what revision and author last modified the lines _10_ to _15_ of the file _README.md_ starting from the commit _affd0ba_
10. Show what revision and author last modified the lines _10_ to _15_ of the file _README.md_. In addition try to find out where the lines came from, i.e. when copied from other files into _README.md_

```sh
# details: man git-grep
 1> git grep -n -i stash
 2> git grep -p -i stash
 3> git grep stash *.md
 4> git log -Sstash
# details: man git-blame
 5> git blame README.md
 6> git blame -L 10,15 README.md
 7> git blame -L 10,+6 README.md
 8> git blame -L 10,+6 master README.md
 9> git blame -L 10,+6 affd0ba README.md
10> git blame -C -L 10,+6 README.md
```
