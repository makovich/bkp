# bkp

Configuration management helper for Alpine Linux [`lbu`](https://wiki.alpinelinux.org/wiki/Alpine_local_backup) that let commit every update to the Git. You may think of it as an [`etckeeper`](https://etckeeper.branchable.com/)'s brother.

## Requirements

* sh
* Git

## Usage

### Local backup (enhanced `lbu commit`)
Initialize repository and make first commit from Alpine itself:
```sh
$ uname -v
#1-Alpine SMP Wed, 17 Feb 2021 08:17:06 UTC
$ git --version
git version 2.30.2
$ wget -O /usr/local/sbin/bkp https://raw.githubusercontent.com/makovich/bkp/master/bkp
$ chmod +x /usr/local/sbin/bkp
$ bkp
LBU_BACKUPDIR is not defined in /etc/lbu/lbu.conf
$ echo "LBU_BACKUPDIR=/root/config-backups" >> /etc/lbu/lbu.conf
$ bkp
Backing up to /root/config-backups...
Initialized empty shared Git repository in /root/config-backups/.git/
warning: You appear to have cloned an empty repository.
[master (root-commit) b21ae0e] auto
 31 files changed, 111 insertions(+)
 create mode 100644 etc/apk/arch
 create mode 100644 etc/apk/keys/alpine-devel@lists.alpinelinux.org-4a6a0840.rsa.pub
 create mode 100644 etc/apk/keys/alpine-devel@lists.alpinelinux.org-5243ef4b.rsa.pub
 create mode 100644 etc/apk/keys/alpine-devel@lists.alpinelinux.org-5261cecb.rsa.pub
 create mode 100644 etc/apk/protected_paths.d/ca-certificates.list
 create mode 100644 etc/apk/repositories
 create mode 100644 etc/apk/world
 create mode 100644 etc/hostname
 create mode 100644 etc/hosts
 create mode 100644 etc/inittab
 create mode 100644 etc/lbu/lbu.conf
 create mode 100644 etc/network/interfaces
 create mode 100644 etc/resolv.conf
 create mode 120000 etc/runlevels/boot/bootmisc
 create mode 120000 etc/runlevels/boot/hostname
 create mode 120000 etc/runlevels/boot/hwclock
 create mode 120000 etc/runlevels/boot/modules
 create mode 120000 etc/runlevels/boot/networking
 create mode 120000 etc/runlevels/boot/sysctl
 create mode 120000 etc/runlevels/boot/syslog
 create mode 120000 etc/runlevels/boot/urandom
 create mode 120000 etc/runlevels/default/acpid
 create mode 120000 etc/runlevels/default/crond
 create mode 120000 etc/runlevels/shutdown/killprocs
 create mode 120000 etc/runlevels/shutdown/mount-ro
 create mode 120000 etc/runlevels/shutdown/savecache
 create mode 120000 etc/runlevels/sysinit/devfs
 create mode 120000 etc/runlevels/sysinit/dmesg
 create mode 120000 etc/runlevels/sysinit/hwdrivers
 create mode 120000 etc/runlevels/sysinit/mdev
 create mode 120000 etc/runlevels/sysinit/modloop
Enumerating objects: 44, done.
Counting objects: 100% (44/44), done.
Compressing objects: 100% (18/18), done.
Writing objects: 100% (44/44), 4.22 KiB | 143.00 KiB/s, done.
Total 44 (delta 0), reused 0 (delta 0), pack-reused 0
To /root/config-backups
 * [new branch]      master -> master
```

If you'd like to store files permissions as well, install lbu's `pre-package` hook:
```sh
$ bkp -i
File save_lbu_perms successfully installed into /etc/lbu/pre-package.d directory.
$ bkp -m "lbu hook and perms.bkp"
Backing up to /root/config-backups...
[master 8cdcc6e] lbu hook and perms.bkp
 2 files changed, 88 insertions(+)
 create mode 100644 etc/lbu/perms.bkp
 create mode 100755 etc/lbu/pre-package.d/save_lbu_perms
Enumerating objects: 10, done.
Counting objects: 100% (10/10), done.
Compressing objects: 100% (5/5), done.
Writing objects: 100% (7/7), 1.69 KiB | 433.00 KiB/s, done.
Total 7 (delta 1), reused 0 (delta 0), pack-reused 0
To /root/config-backups
   b21ae0e..7274d56  master -> master
$ cd /root/config-backups
$ git ls-tree -r HEAD
100644 blob 1c09346681a674c5830116f760375ef17db8dd93    etc/apk/arch
100644 blob bb4bdc80fd10d0609f67076f05e17aafdff75415    etc/apk/keys/alpine-devel@lists.alpinelinux.org-4a6a0840.rsa.pub
100644 blob 6cbfad7441d0193d6f2f64dfb47797975bcc54b1    etc/apk/keys/alpine-devel@lists.alpinelinux.org-5243ef4b.rsa.pub
100644 blob 83f0658e9ca16c4bad20a219318497313a45b7bb    etc/apk/keys/alpine-devel@lists.alpinelinux.org-5261cecb.rsa.pub
100644 blob e23dae84ce22348b03e915cdebf496965254b049    etc/apk/protected_paths.d/ca-certificates.list
100644 blob 409553dfab16b241dc6f4de7764090e3596a4b89    etc/apk/repositories
100644 blob c807c75f6c0742152d426a2ccf534a5aae986baa    etc/apk/world
100644 blob e0b8ab2104103cc0a280114f0c1c2f86a2419565    etc/hostname
100644 blob 269b51fc070be5c6c7cc519a6392b565c339310e    etc/hosts
100644 blob 597331e856c8757e8a6510a1a4026c045dd27239    etc/inittab
100644 blob fa7d0876bce40d5ba21e39692339836f78e787f2    etc/lbu/lbu.conf
100644 blob b984d24c9529d8f8795d25c265c5e10e9b75f401    etc/lbu/perms.bkp
100755 blob 96346d8e610cfe8063317e37f72d93b8c18f079a    etc/lbu/pre-package.d/save_lbu_perms
100644 blob dd1973bb5a370c2575dc4a349c32b5cd4b3482d8    etc/network/interfaces
100644 blob 27a94c07e56a76a9723549aabfe9cd903c683c8b    etc/resolv.conf
120000 blob 01ec1a7e6f8c8aa007e2fd0c9a43f31222b22e44    etc/runlevels/boot/bootmisc
120000 blob 2920d24f68d1d36b7fdee09d2257957a9c944690    etc/runlevels/boot/hostname
120000 blob 84d9672162630272293bab0f1bff46804b17e9ab    etc/runlevels/boot/hwclock
120000 blob 4886563ab2b76604473422e880c38c9ecb153887    etc/runlevels/boot/modules
120000 blob 79fa8db22d1081e43a813b4eadd978fdab9ea42d    etc/runlevels/boot/networking
120000 blob b4ac535e9157932ba0f60da9313d277a1c723b14    etc/runlevels/boot/sysctl
120000 blob 0e9aa2b563bde63389730cd2bc9e232e926a5612    etc/runlevels/boot/syslog
120000 blob 0bae59f211044a3374d147552b9b53f3a997ea6e    etc/runlevels/boot/urandom
120000 blob 94ab152937dbb1029838c2bab3aa4d0bcc410b19    etc/runlevels/default/acpid
120000 blob c9fe428f7d46df5b85c1994b029c20610e507cbc    etc/runlevels/default/crond
120000 blob a26adc1ed1fb5486b9ebed5414c055b69a89d8f0    etc/runlevels/shutdown/killprocs
120000 blob 87cc2564b91802843d7a7f9ac7930284aa9be9a5    etc/runlevels/shutdown/mount-ro
120000 blob 6235c7e5645ef759933f75b785c178bfeb493528    etc/runlevels/shutdown/savecache
120000 blob 73fcdfc42ad9fae2750862a62a8c1aafa187abe4    etc/runlevels/sysinit/devfs
120000 blob d23267e6b0ec4c587299b5f1c9dc5cf22303f780    etc/runlevels/sysinit/dmesg
120000 blob 10c6e5c00714ca8bacf21aa5ca7480d0aaeff5ee    etc/runlevels/sysinit/hwdrivers
120000 blob 8ed346f5a604037064d5dc21d1accd2c6b1c170f    etc/runlevels/sysinit/mdev
120000 blob 77173fb3e0d689745e46ebc350eff80320a9a66f    etc/runlevels/sysinit/modloop
```

Notice `/etc/lbu/perms.bkp` file. It's a self executable shell script that re/stores all permissions of every `lbu` file. By the way, [`alp`](https://github.com/makovich/alp) is aware of it.

Hint: you might want to add `bkp` to the crontab and it'll become more `etckeeper`'y.

### Backup remote machine from local *nix

However, you don't have to install `bkp` at your remote machine and this is another way of usage. Copy script at a machine where you're going to store backup. From here you can install `pre-package` hook:
```sh
# temporary allow `remuser` run sh as a root
# whith doas it looks as follows:
root@remote $ echo "permit nopass remuser as root cmd sh" >> /etc/doas.conf

# here you pipe `bkp -i`'s output to the remote shell interpreter
you@home $ bkp -i | ssh remuser@remote doas sh
File save_lbu_perms successfully installed into /etc/lbu/pre-package.d directory.

# edit doas.conf againg by changing `sh` to `lbu pkg -`
root@remote $ cat /etc/doas.conf
permit nopass remuser as root cmd lbu args pkg -

# now make your commit
you@home $ ssh remuser@remote doas lbu pkg - | bkp -m "init"
you@home $ ls -al
drwxr-xr-x   7 morse  staff   224B Feb  9 01:04 ./
drwxr-xr-x   9 morse  staff   288B Feb  9 01:05 ../
drwxr-xr-x  15 morse  staff   480B Feb 17 15:57 .git/
drwxr-xr-x  40 morse  staff   1.3K Dec 16 17:55 etc/
```

Now you can use [`alp`](https://github.com/makovich/alp) and run in few seconds your remote machine configuration in-memory:
```sh
# you@home
$ cd ~/backups/remote
$ ls -al
drwxr-xr-x   7 morse  staff   224B Feb  9 01:04 ./
drwxr-xr-x   9 morse  staff   288B Feb  9 01:05 ../
drwxr-xr-x  15 morse  staff   480B Feb 17 15:57 .git/
drwxr-xr-x  40 morse  staff   1.3K Dec 16 17:55 etc/
$ alp
# ... hack-hack-hack ...
$ ls -al
drwxr-xr-x   7 morse  staff   224B Feb  9 01:04 ./
drwxr-xr-x   9 morse  staff   288B Feb  9 01:05 ../
drwxr-xr-x   5 morse  staff   160B Feb  1 16:37 .alp/
drwxr-xr-x  15 morse  staff   480B Feb 17 15:57 .git/
drwxr-xr-x  40 morse  staff   1.3K Dec 16 17:55 etc/
$ ls -al .alp/
drwxr-xr-x   5 morse  staff      160 Feb  1 16:37 .
drwxr-xr-x   7 morse  staff      224 Feb  9 01:04 ..
-rw-r--r--   1 morse  staff  8315500 Feb  1 18:06 alp.apkovl.tar.gz
drwxr-xr-x  61 morse  staff     1952 Feb  1 19:05 cache
```

## License

MIT/Unlicense
