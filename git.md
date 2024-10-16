# Git Cheat Sheet

## Initialize a Git Repository in your project folder
```bash
git init
```

## Stage and Commit Files
```bash
git add .
git commit -m 'commit name'
```

## Connect to GitHub repo
```bash
git remote add origin git@github.com:<username>/<repository>.git
```

## Check SSH Connection
### Start the SSH agent in the background
```bash
eval "$(ssh-agent -s)"
```
### Add your SSH private key to the agent
```bash
ssh-add ~/.ssh/<id>
```

## Push Changes to GitHub
### Push changes to the master branch
```bash
git push origin HEAD:master
```
### Alternatively, push to a specific branch
```bash
git push origin branch
```

## Create and Switch Branches

### Create a New Branch
```bash
git checkout -b branchname
```

### Switch to an main Branch
```bash
git checkout main
```

### Or switch to another branch
```bash
git checkout branchname
```

---
