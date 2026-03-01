Show git logs

`git log <--oneline>/<--stat>`

`git reflog`

Remove staged changes

`git restore --staged <file>`

Restore the file to the previous state

`git restore <file>`

Stop tracking a previous tracked file

`git rm --cached <file>`

Show commit details

`git show <commit_hash>`

Show diff between commits

`git diff <commit_hash_1> <commit_hash_2>`

Revert by adding a new commit

`git revert <commit_hash>`

Return to previous commit

`git reset --hard <commit_hash>`

Create new branch

`git branch <branch_name>`

Switch branch

`git switch <branch_name>`

Create and switch in one step

`git switch -c <branch_name>`

Merge into main branch (on main branch)

`git merge <branch_name>`

Delete branch

`git branch -d (-D for force deleting) <branch_name>`

Use stach to keep current code changes

```bash
git stash
git switch other
git stash pop

git stash
git reset --hard
git stash pop

git stash
git pull
git stash pop
```

