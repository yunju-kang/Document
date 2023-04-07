### Access to the private team's repo

`git clone https://<user_name>:<token>@github.com/<repo_directory>`

You can get token at the github's developer setting page.

(reconnect with a new token)

`git remote remove origin`

`git remote add origin https://<token>@github.com/<repo-owner>/<repo-name>`

`git push`

### Create a new branch

1. ##### Create a new branch

   `git branch <new_branch_name>`

   Branch names can contain slash and hyphen as separator.

   (fatal:not a valid object name: 'census')

   `git checkout -b <new_branch_name>`

2. ##### Change working branch

   `git checkout <new_branch>`

3. ##### Set remote branch for local branch

   `git push --set-upstream origin <new_branch_name>`

### Trouble Shooting

1. ##### LF will be replaced by CRLF

   `git config --global core.autocrlf false` 

2. ##### Check remote repository info

   `git remote -v`


#### Push an existing repository

```
git remote add origin https://github.com/yunju-kang/Document.git
git branch -M master
git push -u origin master
```

#### Create a new repository

```
echo "# Document" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M master
git remote add origin https://github.com/yunju-kang/Document.git
git push -u origin master
```

