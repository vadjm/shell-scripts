
GIT commands
=====

Git global setup
-----
```sh
git config --global user.name "Vadim Shapka"
git config --global user.email "vadym.shapka@gmail.com"
```

Show changes
-----
```sh
gitk
gitk --all -d
git diff
git status
git log
git log --oneline -5
```

Commit changes
-----
```sh
git commit -am 'comment'
#or
git add fileName
git add .
git commit -m 'comment'
```

Revert file
-----
```sh
git checkout "path to file"
#revert all changes
git checkout .
```

Reset before indicated commit
-----
```sh
git log
git reset --hard commintNumber
git reset --hard g8b0a17e1745ace90b3532284167f7e42d322451
```
Create Repository
-----
```sh
mkdir projectName
cd projectName
git init
git status
touch README
git add README
#git add .
git status
git commit -m 'initial commit'
git remote add origin https://github.com/vadjee/projectName.git
git push -u origin master
```

Clone Repository
-----
```sh
clone https://github.com/vadjee/projectName.git
cd projectName
git fetch
```

Create breanch
-----
```sh
git checkout -b newBreabnchNameFromCurrent
git push --set-upstream origin newBreabnchNameFromCurrent 
# add all changes
git add .
git push origin newBreabnchNameFromCurrent
```

Show branches
-----
```sh
git branch
git branch -av	
```

Delete a local/remote branch
-----
```sh
# local
git branch -d the_local_branch
# remote
git push origin :the_remote_branch
```
Clean(update) old branches after fetch
-----
```sh
git fetch
git remote prune origin
```
Merge with Stash
-----
```sh
git stash
#merge commands ...
#gitk --all -d
git stash apply
git stash drop
git status
```

Merge branchName to master
-----
```sh
git branch -av
git checkout master
#git pull --rebase origin master
git pull origin master
git status
git checkout branchName
git pull
git status
git checkout master
# --no-ff (no fast forward)
#git merge origin/branchName --no-ff (merge to master) (no fast forward)
git merge branchName --no-ff
#Change last comment, insert ... insert :wq
#git commit --amend
git push
#delete branch if need
```


Merge branchName to dev (IDEA)
-----
```sh
-- 1.  update with dev -->
git checkout branchName
git rebase origin/dev
--or--> git pull --rebase origin dev
-- fix conflicts in IDEA
git status
-- if need -->
git add .
git rebase --continue
-- or if wrong ...
-- git rebase --abort 
git push origin
git push origin branchName
-- or
git push origin branchName --force-with-lease 
-- 2. merge to dev
git checkout dev
git pull --rebase
git pull --rebase origin branchName
git push origin dev
```

Squash commits of task2
-----
```sh
git log
git rebase -i HEAD~9
#pick task1 comment
#pick task2 comment1
#f task2 comment2 --> change (pick) to (f) comment2 will be lost in history
#s task2 comment3 --> change (pick) to (s) comment3 will be in history (squashed from down to up); 
#pick task3 comment
#(s) squash with stay comment in history
#(f) squash without save this comment
#(d) drop commit
#squashed from down to up! stay pick on first comment1
git push origin <branchName> -f
git log

#fg
#git rebase --abort
```

Delete commit
-----
```sh
git log
git rebase -i HEAD~9
#change (pick) to (d) drop commit
git push origin <branchName> -f
git log
```

Delete commit. rebase to last_normal_commit
-----
```sh
git log
# rebase to last_normal_commit
git rebase -i aaa84e022469fdee887783f4f4c2de0afbbb
git log
git push --force
```

Delete all local changes in dev branch
-----
```sh
#git checkout .
git log
git reset --hard aaa84e022469fdee887783f4f4c2de0afbbb
git pull --rebase origin dev
```


HOTFIX in new branch based on master
-----
```sh
git checkout master
git pull --rebase origin master
git checkout -b '<branchName>'
git cherry-pick aaa84e022469fdee887783f4f4c2de0afbbb
git push origin <branchName>
```

Rebase hotfix branchName to master
-----
```sh
git checkout master
git pull --rebase origin master
git status
git pull --rebase origin <branchName_BasedOnMaster>
git push origin master
```

Rebase old branch to master
-----
```sh
#1. Update old branch --> Reabase master to <branchName>. 
git checkout <branchName>
git pull --rebase origin <branchName>
git status
git pull --rebase origin master
# double check if all ok. "git status; gitk; git log ..."
git log
# "Squash commits" (s,f) 
#git rebase -i HEAD~9
#git rebase --abort
git push origin <branchName> -f


#2. Rebase updated branch to master
git checkout master
git fetch
git pull --rebase origin master
git status
git pull --rebase origin <branchName>
git push origin master
```

Several Remote
-----
```sh
# show remote
git remote -v

#Project in github, development in <companyName> Git

#1. Clone github project
git clone git@github.com:Totaltel-International/<projectName>.git

#2. Rename github repository from "origin" to "github"
git remote rename origin github

#3. add <companyName> repository
#add <companyName> repository as origin
git remote add origin git@git.<companyName>.com:novatti/<projectName>.git
#push your local repository
git push -u origin master
#Sets the default branch for the 
git remote set-head origin -a

#switch local master to <companyName> git. (!) all un-commited \ un-pushed changes could be lost
git checkout  -B master --track origin/master

#create github master into local github_master
git branch github_master --track github/master
git checkout github_master 

#merge changes from local master as one commit
git merge --squash master --strategy=recursive -Xtheirs
git commit -m "merged test commit"
git push github HEAD:master

#----
#git remote add github git@github.com:Totaltel-International/<projectName>.git
#git push github Morocco_adapter
```

Several Remote. Example of merging master from companyName repo into remote git repository
-----
```sh
git remote add a9 git@git.a9xxx...<remote git repository>.git
git fetch
git checkout -B a9_master --track a9/master

# Merge master into a9_master
git merge --squash master --strategy=recursive -Xtheirs
git commit -m "merged test commit"
git push a9 HEAD:master
```
