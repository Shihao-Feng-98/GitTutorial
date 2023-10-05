# Git Tutorial
A basic tutorial on using git for **version control**, focusing on **git native commands**.  There are some easy-to-use tools that can be an assistance, such as `Git Graph` (vscode extension) and `Source Control` (vscode built in tool).

Some reference:
[Official Tutorial](https://git-scm.com/book/en/v2),
[Docs Tutorial](https://www.liaoxuefeng.com/wiki/896043488029600),
[Video Tutorial](https://www.bilibili.com/video/BV1w14y1C7oi/?spm_id_from=333.999.0.0)

---

### 1. Create and Submit
#### 1.1 Clone a remote repo to local 
```shell
git clone https://github.com/Shihao-Feng-98/GitTutorial.git
```

#### 1.2 Initialize the local repo 
```shell
git config --global user.email "you@example.com"
git config --global user.name "Your Name"
```
**Note**: Up to date, you can coding locally.

#### 1.3 Put the current files into **Stage**
```shell
git add . # add all modified files
git add file_name # add specific file
```

#### 1.4 Submit the current contents of **Stage** to **Branch**
```shell
git commit -m "some descriptions of submission" # add some descriptions
```

#### 1.5 Push to remote ([Github](https://github.com/) or [Gitlab](https://gitlab.com/))
```shell
git push origin main # push to the corresponding remote branch, e.g. main
```
**Note**: You can `add`, `commit`, or `push` multiple times as you need.

---


### 2. Version Control
#### 2.1 Version rollback
If you want to rollback to the previous version, run
```shell
git log # find the commit_id that you want to rollback
git reset --hard commit_id
```

Now, if you want to rollback to the future version of the current version, run
```shell
git reflog # show the command you have been used, find the commit_id here
git reset --hard commit_id
```

#### 2.2 Undo changes
If you want to undo the changes on the working directory, run 
```shell
git checkout -- file_name
```

If you want to undo the changes that have been add to stage, run 
```shell
git reset HEAD file_name
git checkout -- file_name
```

If you want to undo the canges that have been commit, run
```shell
git reset --hard commit_id
git reset HEAD file_name
git checkout -- file_name
```

---


### 3. Use Feature branch 
When you developing a new feature in a project, the following process should be followed.

#### 3.1 Create a new branch
```shell
git checkout dev # switch to desired parent branch, e.g. dev
git branch dev/feat_a # create a branch, e.g. dev/feat_a
git checkout dev/feat_a # switch to new branch, e.g. dev/feat_a
```

#### 3.2 Follow 1.3, 1.4, and 1.5   
**Note**: The "dev/feat_a" branch would created on the remote after implementing 1.5.

#### 3.3 Use pull request  
When you cooperate with others, a pull request should be submitted. Before merging your branch, you should modify your code after review. 

#### 3.4 Deal with conflict  
When new changes have been applied to the dev branch, the current merge may fail. Sometimes conflicts should be resolved before the current merge. <font color=yellow>TODO: conflict</font>

#### 3.5 Merge to desired branch
```shell
git checkout dev # switch to desired branch
git merge dev/feat_a # merge feature branch to desired branch
```

#### 3.6 Delete the old branch
```shell
git branch -d dev/feat_a # delete the local branch
git push origin --delete dev/feat_a # delete the remote branch
```

----


### 4 Use Bug branch
If there is a bug needs to be fixed when you developing a new feature in a project, the following process should be followed.

#### 4.1 Stash the current code
```shell
git stash # save the current code
```

#### 4.2 Create new branch and fix the bug  
Please follow 3.1.

#### 4.3 Recovery the previous code  
```shell
git stash pop # recovery code and delete the stash
```

#### 4.4 Fix the same bug on current branch
```shell
git log # find the commit_id for bug fixing
git cherry-pick commit_id
```

---


### 5 Some Useful Skills
#### 5.1 Get information
```shell
git log # list the commit history

git status # check the status of the current local branch

git remote show origin # check the infomation of remote branch

git branch # list local branch
git branch -r # list remote branch
git branch -a # list all branch

git tag # list tag
git show tag_name # check the infomation of the current tag 
```

#### 5.2 Combine commits
During development, you would add many commits, but you should combine 
them into one commit before Pull request to keep it clear.

Use `git reset --soft`:
```shell
git log -5 # show last 5 commits
git reset --soft commit_id # restore files from repo to cache without changing the local work area
git add -u # add tracked files
git commit -m "new description"
git push --force
```

Use `git rebase`:
```shell
git log -5
git rebase -i HEAD~5 # combine last 5 commits into one commit
git push --force
```
**Notes**: 
- Run `git rebase -i HEAD~5` would enter a editor, change the reserved commit as `pick`, change the discarded commit as `fixup`.
- `git rebase` would create a new branch. If you make a mistake, you can `git checkout` to the previous branch and `git rebase` again. 
