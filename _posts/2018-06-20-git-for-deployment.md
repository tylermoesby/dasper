---
title: Git for Deployment
date: 2018-06-20 00:00:00 +0000
layout: article
---

## Server

Initialize a bare repository outside of the live server (hub repo).

```bash
mkdir -p /var/git/myProject.git
cd /var/git/myProject.git
git init --bare
```

Initialize a repository inside of the live server (prime repo). And add hub as a remote.

```
cd /var/www/html/myProject
git init
git remote add hub /var/git/myProject.git
git add .
git commit -m "Initial import of pre-existing web files."
git push hub master
```

Open `vim /var/git/myProject.git/hooks/post-update` and paste this.

```
#!/bin/sh

echo
echo "*** Pulling changes into Live [Hub's post-update hook]"
echo

cd /var/www/html/myProject || exit
unset GIT_DIR
git pull hub master

branch=$(git rev-parse --symbolic --abbrev-ref $1)
if [ "$branch" = "master" ]
then
  # run other commands here like `yarn build`
fi

exec git-update-server-info
```

Open `vim /var/www/html/myProject/.git/hooks/post-commit` and paste this.

```
#!/bin/sh

echo
echo "*** Pushing changes to Hub [Live's post-commit hook]"
echo

git push hub
```

Make hooks executable.

```
chmod +x /var/git/myProject.git/hooks/post-update
chmod +x /var/www/html/myProject/.git/hooks/post-commit
```

## Local

Clone repo to the local computer.

```
cd ~/Desktop
git clone username@ipAddress:/var/git/myProject.git
cd myProject
```

Ready to development.

```
echo "test" >> index.html
git add .
git commit -m "First local change."
git push origin master
```
