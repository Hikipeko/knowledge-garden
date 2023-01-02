```
git init
git status
git log
git add
# if you don't want the staged file, git reset
git reset <filename>
git checkout <branch-name>
git commit -m <message>
git branch <branch-name>
# merge current branch to the target branch
git merge <branch-name>
#remote commands
git clone
git fetch
git pull
git push
git remote
```

**commit message style**

Title + Body

Fix xyz: Issue #22772

**git reset**

Scenario: make a commit and want to "uncommit"

```shell
git reset HEAD~1
--soft # leaves the changed files staged
--mixed # leaves the changed files unstaged
```

**git commit --amend**

forget to add a file/ want to edit the commit message

**git rebase**

Command that allows you to rewrite history.

```shell
git rebase -i <base tip> # e.g. HEAD~3
```

pick：保留该commit（缩写:p）

reword：保留该commit，但我需要修改该commit的注释（缩写:r）

edit：保留该commit, 但我要停下来修改该提交(不仅仅修改注释)（缩写:e）

squash：将该commit和前一个commit合并（缩写:s）

fixup：将该commit和前一个commit合并，但我不要保留该提交的注释信息（缩写:f）

exec：执行shell命令（缩写:x）

drop：我要丢弃该commit（缩写:d）

**git stash**

Effect: save the state of your Index and Working Directory into the "stash" and rolls you back to a clean Working Directory.

Scenario: need to jump around different branches while having some modified files.

```shell
git stash
git stash pop # brin the top entry of the stash's stack
git stash drop
```

**remote**

```shell
git reset --hard origin/master 
git pull --rebase origin m
```

