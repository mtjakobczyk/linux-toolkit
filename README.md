## Linux recipes and reusable code

#### Show directory tree
Display `terraform-aws-eks` directory tree including directories (except `examples`,`docs` and `tools`) and files with `*.tf` and `*.tpl` extensions:
```bash
tree -P '*.tf|*.tpl' -I 'examples|docs|tools' terraform-aws-eks
```

#### Find all lines that begin with a particular pattern in selected files

```bash
grep "^resource " terraform-aws-eks/*.tf  terraform-aws-eks/modules/*/*.tf
```

#### Inspect the storage in use across directories
This `du` command estimates file space usage in the root directory (`/*`), excluding the `/proc` pseudo-filesystem directory. It displays the grand total (`-c`) and individual totals (`-s`) for each subdirectory. The totals are displayed in kilobytes, megabytes or gigabytes (`-h`).

    du -shc --exclude=proc /*

The output can look like this:
```
0	bin
147M	boot
0	dev
19M	etc
24K	home
0	lib
0	lib64
0	media
0	mnt
0	opt
32K	root
1.1M	run
0	sbin
0	srv
0	sys
0	tmp
1.1G	usr
456M	var
1.7G	total
```

#### Prevent mounting /tmp in memory
Programs can create large files in `/tmp`. For some distros it will actually occupy space in memory rather than disk. 
To prevent systemd from mounting `/tmp` as tmpfs (in-memory), use this command:
```bash
systemctl mask tmp.mount # followed by reboot
```

#### Coloring Terminal output
```bash
# Add to .profile
export CLICOLOR=1
export LSCOLORS=Exfxcxdxbxegedabagacad
```
