# Git knowledge

## Checkout specific tag

### first, need to make sure you fetch all the tags locally

```
# --all will fetch all the remotes.
# --tags will fetch all tags as well
git fetch --all --tags --prune
```

### Then check out the tag by running

```
git checkout tags/<tag_name> -b <branch_name>
```

### sync the remote branch

```
git checkout master
git pull
git fetch upstream
git merge upstream/master
git push
```
