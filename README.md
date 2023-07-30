# Git Tutorial

## Standard usage
---
### 1. Get a repositories(repo in short)

#### 1.1 New(or fork) a repo in [Github](https://github.com/) (or [Gitlab](https://gitlab.com/))

#### 1.2 Clone remote repo to local 
```shell
git clone https://github.com/Shihao-Feng-98/GitTutorial.git
```

#### 1.3 Initialize the local repo 
```shell
git config --global user.email "you@example.com"
git config --global user.name "Your Name"
```

---

### 2. Coding locally and updating in git
We need some branches when developing our codes. Generally, the release code in `main` branch, the develop code in `dev` branch. 

#### 2.1 Create a new branch from the current branch
For example, we create a new branch named `dev` from `main` branch:
```shell
git branch dev # create a branch named dev
git checkout dev # switch to dev branch
```
After creating a new branch locally, you also should update this branch to remote:
```shell
git push origin dev
```
Before creating a new branch, make sure you are in the branch you want. And we should create a new branch for each new feature (like `dev/AddXxxFeat`).

#### 2.2 Push to remote (Github or Gitlab)
You can commit and push multiple times until your feature development and testing is complete.
```shell
git add . # add all modified files
git add file_name # add specific file
```

```shell
git commit -m "some descriptions of submission" # add some descriptions
```

```shell
git push origin branch_name # push to the corresponding remote branch
```

#### 2.3 Merge to the important branch
For example, now you are in `dev/AddXxxFeat` branch. After develop the new feature, you should merge to `dev` branch:
```shell
git checkout dev # switch to dev branch
git merge dev/AddXxxFeat # merge the code of dev/AddXxxFeat branch to dev branch
```

When multiple `dev/AddXxxFeat` branc merged to `dev` branch, you should merge the `dev` branch to `main` branch:
```shell
git checkout main # switch to main branch
git merge dev # merge the code of dev branch to main branch
```

#### 2.4 Delete the redundant branch
Delete the local branch:
```shell
git branch -d dev/AddXxxFeat # has been pushed or merged
git branch -D dev/AddXxxFeat # haven't been pushed or merged
```
Delete the remote branch:
```shell
git push origin --delete dev/AddXxxFeat
```

---
### 3. Release the project 
#### 3.1 Use tag to label the milestone
In general, the project version that needs to be released will be labeled with tag:
```shell
git tag -a version1.0 -m "tag descriptions" # create a tag
git push origin version1.0 # push the tag to remote
```

#### 3.2 Fix the bug of historical version
For example, you found a bug in `version1.0`, but you are currently developing in `version2.0`; You need to create a new branch and fix the bug of `version1.0`:
```shell
git checkout version1.0 # checkout the tag
git branch fix_bug_version1.0 # create a new branch from version1.0
git checkout fix_bug_version1.0 # checkout branch
```

After fixing the bug, you should create a new tag:
```shell
git tag -a version1.1 -m "tag descriptions" # create a new tag
git push origin version1.1
```

If you found this bug also exists in `version2.0`, you should merge the code from `version1.1`: 
```shell
git checkout version2.0 
git checkout dev
git branch merge_bug_fix # create a new branch from version2.0 
git checkout merge_bug_fix
git merge version1.1
```
Then solve the conflict, push to remote, and merge to `dev` branch.

---
### 4. Use pull requests (merge requests)
When you are collaborating with others, you should use `Pull requests` or `Merge requests` to submit code.
* Click `Merge requests`
* Choose the branch you want to merge
* Write some descriptions
* Wait for review

---
### Some useful command
#### Fetch code from remote branch and merge to local branch 
```shell
git pull origin main # git pull origin main:main, merge the remote main branch to local main branch
git pull origin main:dev # merge the remote main branch to local dev branch
```

#### Get information
```shell
git log # list the commit history
```

```shell
git status # check the status of the current local branch
git remote show origin # check the infomation of remote branch
git branch # list local branch
git branch -r # list remote branch
git branch -a # list all branch
```

```shell
git tag # list tag
git show tag_name # check the infomation of the current tag 
```

If you find that a remote deleted branch appears in the local branch information, run:
```shell
git remote prune origin
```

#### Combine commits
During development, you would add many commits, but you should combine 
them into one commit before `Pull requests` to keep it clear.

Use `git reset --soft`:
```shell
git log -5 # show last 5 commits
git reset --soft commit_id # restore files from repo to cache without changing the local work area
git add -u # add tracked files
git commit -m "new description"
git push --force
```

Or use `git rebase`:
```shell
git log -5
git rebase -i HEAD~5 # combine last 5 commits into one commit
git push --force
```
Notes: 
1. Run `git rebase -i HEAD~5` would enter a editor, change the reserved commit as `pick`, change the discarded commit as `fixup`.
2. `git rebase` would create a new branch. If you make a mistake, you can `git checkout` to the previous branch and `git rebase` again. 


#### Push or pull hangs
When `git pull` and `git push` hangs, try:
```shell
git fsck # verifies the connectivity and validity of the objects in the database
git gc --prune=now # cleanup unnecessary files and optimize the local repository
```