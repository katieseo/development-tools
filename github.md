## Basic

#### Download Git & congif
```
> git --verrsion
> git config --global user.name "name name"
> git config --global user.email "email@emial.com"
```
#### create folder, file
#### initialize, add, commit
```
> git init
> git status
> git add . (start tracking)
> git status (green)
> git commit -m "message"
> git log   (end: q)

change file

> git diff filename
> git add filename
> git commit -m "message)

OR
>git commit -am "message" (add+commit)
```

#### create a branch
```
> git branch develop    OR.    git checkout -b feature/new-feature
> git branch  (check branches)
> git checkout develop
```

#### merge
```
> git merge feature/new-feature
> git log
> git show 6eq139a

master --> production
develop --> staging
```

#### git clone

#### git fetch

#### Pull Request



#### …or create a new repository on the command line
````
echo "# test" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin [URL]
git push -u origin main
````

#### …or push an existing repository from the command line
````
git remote add origin {URL}
git branch -M main
git push -u origin main
````

#### github to netlify
Project using react-route:

create \_redirects file in public folder
```
/* /index.html 200
```
