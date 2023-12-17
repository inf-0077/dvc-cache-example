# dvc-cache-example

This is a repository with an example of how to make a dvc-cache with symbolic links. 

This is part of INF-0077 course from IC/UNICAMP.

We are versioning locally the dvc remotes here in order to show a very straight-forward example of how to make the symbolic links.

Also, we demonstrate on this repository how to switch and relink the cache.

# Reproducing the scenario of this repository

```sh
# creating our dirs
git init .
mkdir remote
mkdir cache
mkdir new-cache
mkdir project
cd project

# creating dvc repo
dvc init --subdir
dvc remote add --default remote ../remote
dvc cache dir ../cache
dvc config cache.shared. group
dvc config cache.type symlink
git add .
git commit -m "dvc init"

# creating dataset & versioning
mkdir ds
cd ds
echo a > a.txt
echo b > b.txt
echo c > c.txt
echo d > d.txt
echo e > e.txt
cd ..
dvc add ds
git add .
git commit -m "dvc add ds"
dvc push

# Testing ds symlinks
ls -lah ds/a.txt
# [..] project/ds/a.txt -> /[..]/dvc-test/cache/files/md5/60/b725f10c9c85c70d97880dfe8191b3

# Changing cache dir
dvc cache dir ../new-cache
dvc checkout --relink

# Testing ds symlinks after relink
ls -lah ds/a.txt
# [..] project/ds/a.txt -> /[..]/dvc-test/new-cache/files/md5/60/b725f10c9c85c70d97880dfe8191b3
```

# References
[1] https://dvc.org/doc/use-cases/fast-data-caching-hub
[2] https://dvc.org/doc/command-reference/cache
[3] https://dvc.org/doc/user-guide/how-to/share-a-dvc-cache
