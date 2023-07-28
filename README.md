# Git usage tutorial


## 1. Simple usage
### 1.1 Clone the github repo to the local 
```shell
git clone https://github.com/Shihao-Feng-98/Github_usage_tutorial.git
```

### 1.2 Initialize the local repo 
```shell
git config --global user.email "you@example.com"
git config --global user.name "Your Name"

```

### 1.3 Submit the modified files
```shell
git add . # add all modified files
git add file_name # add specific file
git commit -m "some descriptions of submission" # add some descriptions
```
Multiple revisions can be submitted multiple times until the current function is developed and tested.

### 1.4 Submit to github repo
```shell
git push origin main # push to the github main branch
```


## 2. Standard usage
### 2.1 Create a new branch 
```shell
git branch dev # create a branch named dev
git checkout dev # switch to dev branch for development
```
Now the new branch is exist in local. If you want to create this branch to github repo, run
```shell
git push origin dev # pull dev branch to github repo
```
You can develop in this branch, then submit(see 1.3) the code, and push to github repo by the above cmd.

### 2.2 Merge the dev branch into the main branch
```shell
git checkout main # switch to main branch
git merge dev # merge the code of dev branch to main branch
```
After merging the dev branch, then you can delete this branch that if you want
```shell
git branch -d dev # delete local branch
git branch origin:dev # delete github branch
```


## 3. Publish the complete project 
### 3.1 Use tag to label the milestone
```shell
git tag -a tag1.0 -m "tag descriptions" # create a tag
git push origin tag1.0 # push the tag to remote(github)
git checkout tag1.0 # checkout tag when rollback is required
```

### 3.2 Fix the bug of historical version
```shell
git checkout tag1.0 # checkout the tag
git branch fix_bug_tag1.0 # create a new branch from tag1.0
git checkout fix_bug_tag1.0 # checkout branch
# after fixing the bug
git tag -a tag1.1 -m "tag descriptions" # create a new tag
git push origin tag1.1 
git checkout main
```

### 3.3 Modified the historical version and merge to the main branch
```shell
git branch merge_bug_fix # create a new branch from tag1.0
git checkout merge_bug_fix
# after fixing the bug
git merge tag1.1
# solve the conflict and push to the remote
```


## 4. Use pull request for project development
* Click <kbd>Fork</kbd> of the targt repo page and you would find the repo in your github.

* Git clone the forked repo to the local.

* Create a new branch, then finish the development.

* Add commit, then merge to main branch, and push to github.

* Go to the target repo and click <kbd>New pull request</kbd>.


## Some useful cmd 
### Fetch code from github and merge local version
```shell
git pull origin main # merge the remote(github) main branch to local main
git pull origin main:dev # merge the remote(github) main branch to local dev
```

### Check some information
```shell
git status # check the status of the current branch

git tag # list tag
git show tag_name # check the infomation of the current tag 

git branch # list local branch
git branch -a # list all branch
git branch -r # list remote(github) branch
git remote show origin # check the infomation of remote(github) branch
# if you find that a remote deleted branch appears 
# in the local branch information, run the following line
git remote prune origin
```
