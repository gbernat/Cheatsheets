# Git cheatsheet

## Normal flow:
- Status
```
$ git status
```

- Create a local git repository.   
Inside the project folder:
```
$ git init
```

- Add a file to staging  
```
$ git add <filename>
$ git add .  (all files)
```

- Create a commit
```
$ git commit -m "Your message about the commit"
Add and Commit in one step: 
$ git commit -a -m "your commit message here"
```

- Create a new branch  
This command will automatically create a new branch and then 'check you out' on it, meaning git will move you to that branch, off of the primary branch.
```
$ git checkout -b <my branch name>
```

- List branches
```
$ git branch
```
  
(..work..work..work.. local commit)  
  
- Push a branch and local commits to GitHub (synchronize)  
```
$ git push origin my-new-branch
```
  
- Create a pull request (PR) (**On GitHub page**)  
"Create a Pull request", select Banch From and Brach To (generally, master) where to merge.  
"Merge pull request", This will merge your changes into the primary branch.  
Now you can delete the other Brach ("Delete branch" button).

- Get changes on GitHub back to your computer  
Gets the most recent changes that you or others have merged on GitHub
```
$ git pull origin master
```
  
- Delete local branch  
You have to be in another branch to delete it. -D forces deletion.  
```
$ git branch -d my-new-branch
```  


## Remote:
- Push a local repo to a new empty GitHub repo (synchronize)
```
$ git remote add origin https://github.com/gbernat/mynewrepository.git
$ git push -u origin master
```

## Other commands:
- switch branches
```
$ git checkout <branch-name>
```

- Show all commits in the current branch’s history
```
$ git log
```

- diff of what is changed but not staged
```
$ git diff
```

- diff of what is staged but not yet commited
```
$ git diff --staged
```

## References
https://product.hubspot.com/blog/git-and-github-tutorial-for-beginners
