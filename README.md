# Testing submodules
  Learning how to include and pin specific submodules.
  We'll be utilizing the biotools suite to do this

## Create submodules
  ```bash
  git submodule add --name bcftools -b master git@github.com:samtools/bcftools.git bcftools
  git submodule add --name samtools -b master git@github.com:samtools/samtools.git samtools
  ```
Check the status
```bash
(main) % git submodule add --name samtools -b master git@github.com:samtools/samtools.git samtools
Cloning into './samtools'...
remote: Enumerating objects: 12626, done.
remote: Counting objects: 100% (809/809), done.
remote: Compressing objects: 100% (312/312), done.
remote: Total 12626 (delta 563), reused 672 (delta 494), pack-reused 11817
Receiving objects: 100% (12626/12626), 13.45 MiB | 1.85 MiB/s, done.
Resolving deltas: 100% (8482/8482), done.

submodules (main 2S-0U) % git submodule add --name bcftools -b master git@github.com:samtools/bcftools.git bcftools
Cloning into './bcftools'...
remote: Enumerating objects: 14943, done.
remote: Counting objects: 100% (1440/1440), done.
remote: Compressing objects: 100% (787/787), done.
remote: Total 14943 (delta 878), reused 1104 (delta 625), pack-reused 13503
Receiving objects: 100% (14943/14943), 17.51 MiB | 1.60 MiB/s, done.
Resolving deltas: 100% (9840/9840), done.

submodules (main 4S-0U) % git status
On branch main
Your branch is up to date with 'origin/main'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	new file:   .gitmodules
	new file:   bcftools
	new file:   samtools
	new file:   singularity

submodules (main 4S-0U) % git submodule status
 5f1bf7a1b016c24d38657bdde5fd2ca27e6954e9 bcftools (1.14)
 c29621d3ae075573fce83e229a5e02348d4e8147 samtools (1.14)
 2f16701e33f5e54824429d196b239cff30e208be singularity (v3.8.3-222-g2f16701e3)

submodules (main 4S-0U) % cat .gitmodules
[submodule "samtools"]
	path = samtools
	url = git@github.com:samtools/samtools.git
	branch = master
[submodule "bcftools"]
	path = bcftools
	url = git@github.com:samtools/bcftools.git
	branch = master
[submodule "singularity"]
	path = singularity
	url = https://github.com/apptainer/singularity.git
	branch = master
```


## Cloning
Just doing a git clone will *not* pull the submodules
Running `git clone git@github.com:helix-clyde/submodules.git` will pull the repository, but will not checkout the submodules e.g.

```bash
git clone git@github.com:helix-clyde/submodules.git submodules_bare
Cloning into 'submodules_bare'...
remote: Enumerating objects: 13, done.
remote: Counting objects: 100% (13/13), done.
remote: Compressing objects: 100% (11/11), done.
remote: Total 13 (delta 1), reused 7 (delta 1), pack-reused 0
Receiving objects: 100% (13/13), 14.95 KiB | 464.00 KiB/s, done.
Resolving deltas: 100% (1/1), done.
$ cd submodules_bare/

submodules_bare (main) $ ll
total 52
-rw-r--r-- 1 clyde.jones docker 35149 Jan 11 12:57 LICENSE
-rw-r--r-- 1 clyde.jones docker  2176 Jan 11 12:57 README.md
drwxr-xr-x 2 clyde.jones docker  4096 Jan 11 12:57 bcftools
drwxr-xr-x 2 clyde.jones docker  4096 Jan 11 12:57 samtools
drwxr-xr-x 2 clyde.jones docker  4096 Jan 11 12:57 singularity

submodules_bare (main) $ git submodule status
-5f1bf7a1b016c24d38657bdde5fd2ca27e6954e9 bcftools
-c29621d3ae075573fce83e229a5e02348d4e8147 samtools
-2f16701e33f5e54824429d196b239cff30e208be singularity

submodules_bare (main) $ cd bcftools/
submodules_bare/bcftools (main) $ ll
total 0
submodules_bare/bcftools (main) $
```

To pull the submodule code if you've done a plain clone, do:
```bash
git submodule update --init samtools/
Submodule 'samtools' (git@github.com:samtools/samtools.git) registered for path 'samtools'
Cloning into 'submodules_bare/samtools'...
Submodule path 'samtools': checked out 'c29621d3ae075573fce83e229a5e02348d4e8147'
```

or you can run this bash snippet
```
for subm in $(grep submodule .gitmodules  | cut -d \" -f 2);
  do
    git submodule init $subm
  done
```
run `git clone --recursive git@github.com:helix-clyde/submodules.git` to do the full checkout
