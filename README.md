# Git Tutorial
A basic tutorial on using git for **version control**, focusing on **git native commands**.  There are some easy-to-use tools that can be an assistance, such as `Git Graph` (vscode extension) and `Source Control` (vscode built in tool).

Some reference:
[Official Tutorial](https://git-scm.com/book/en/v2),
[Docs Tutorial](https://www.liaoxuefeng.com/wiki/896043488029600),
[Video Tutorial](https://www.bilibili.com/video/BV1w14y1C7oi/?spm_id_from=333.999.0.0)


## Standard Usage
### 1. Create and Submit
1.1 Clone a remote repo to local 
```shell
git clone https://github.com/Shihao-Feng-98/GitTutorial.git
```

1.2 Initialize the local repo 
```shell
git config --global user.email "you@example.com"
git config --global user.name "Your Name"
```
**Note**: Up to date, you can coding locally.

1.3 Put the current files into **Stage**
```shell
git add . # add all modified files
git add file_name # add specific file
```

1.4 Submit the current contents of **Stage** to **Branch**
```shell
git commit -m "some descriptions of submission" # add some descriptions
```

1.5 Push to remote ([Github](https://github.com/) or [Gitlab](https://gitlab.com/))
```shell
git push origin main # push to the corresponding remote branch, e.g. main
```
**Note**: You can `add`, `commit`, or `push` multiple times as you need.

### 2. Version Control

### 3. Use Branches 
#### 3.1 Feature branch
When you developing a new feature in a project, the following process should be followed.

3.1.1 Create a new branch
```shell
git checkout dev # switch to desired parent branch, e.g. dev
git branch dev/feat_a # create a branch, e.g. dev/feat_a
git checkout dev/feat_a # switch to new branch, e.g. dev/feat_a
```
**Note**: Coding locally.

3.1.2 Follow 1.3, 1.4, and 1.5  
**Note**: The dev/feat_a branch would created on the remote after implementing 1.5.

3.1.3 Merge to desired branch
```shell
git checkout dev # switch to desired branch
git merge dev/feat_a # merge feature branch to desired branch
```
**Deal with conflict**  
When new changes have been applied to the dev branch, the current merge may fail. Sometimes conflicts should be resolved before the current merge. <font color=yellow>TODO: conflict</font>

**Use pull request**  
When you cooperate with others, a pull request should be submitted. Before merging your branch, you should modify your code after review. 

3.1.4 Delete the old branch
```shell
git branch -d dev/feat_a # delete the local branch
git push origin --delete dev/feat_a # delete the remote branch
```

#### 3.2 Bug branch

### 5. Release a project


### 2. Coding locally and updating to remote
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


---
### 3. Release a project 
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
- Click `Merge requests`
- Choose the branch you want to merge
- Write some descriptions
- Wait for review

---

## note
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