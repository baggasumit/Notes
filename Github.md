# Github 

Initial Steps
+ Move to the directory where the files are
+ git init
+ git add MarkDownTest.md
+ git commit -m "first commit"
+ git remote add origin https://github.com/baggasumit/Notes.git
+ git push [-u origin master]

## Adding and commiting in one step
+ git commit -a -m "first commit"
+ and then
+ git push

## Summary:

### Part 1
+ **git init** creates a new Git repository
+ **git status** inspects the contents of the working directory and staging area
+ **git add** adds files from the working directory to the staging area
+ **git diff filename** shows the difference between the working directory and the staging area
+ **git commit** permanently stores file changes from the staging area in the repository
+ **git log** shows a list of all previous commits

### Part 2
+ **git checkout HEAD filename**: Discards changes in the working directory.
+ **git reset HEAD filename**: Unstages file changes in the staging area.
+ **git reset commit_SHA**: Resets to a previous commit in your commit history. 

### Part 3
+ **git branch**: Lists all a Git project's branches.
+ **git branch branch_name**: Creates a new branch.
+ **git checkout branch_name**: Used to switch from one branch to another.
+ **git merge branch_name**: Used to join file changes from one branch to another.
+ **git branch -d branch_name**: Deletes the branch specified.

### Part 4
+ **git clone**: Creates a local copy of a remote.
+ **git remote -v**: Lists a Git project's remotes.
+ **git fetch**: Fetches work from the remote into the local copy.
+ **git merge origin/master**: Merges origin/master into your local branch.
+ **git push origin <branch_name>**: Pushes a local branch to the origin remote.

### Git Workflow

+ Fetch and merge changes from the remote
+ Create a branch to work on a new project feature
+ Develop the feature on your branch and commit your work
+ Fetch and merge from the remote again (in case new commits were made while you were working)
+ Push your branch up to the remote for review
