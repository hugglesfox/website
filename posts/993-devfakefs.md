# Faking /dev for rootless build environments
tags: linux, containers

_Published: 2026/07/09_

I've been working on stuffing [kiss linux](https://kisscommunity.org/) into the
[BIOS flash of a Thinkpad X220](https://codeberg.org/huggles/arctic/src/branch/kiss-linux).

The cliff notes for that project for anyone interested:

- Uses CoreBoot to replace the BIOS and boots directly into the Linux environment after initializing the hardware
- Minimal Linux environment (with networking) that can then load in further applications from the disk
- Though doesn't require a disk to be present for basic things

For this I need to create a separated build environment (think container) for
constructing the initramfs which the kernel sets up on boot. Now the typical
way of setting this up would use chroot and bind mounts in the host's pseudo
file systems such as `/dev`. And that would've worked just fine, though I
really wanted the build script to not require escalating to root privileges.

Why? Because I thought it would be an interesting experiment and if I'm being
honest a little piece of me cringes every time a script asks for a password
because it's trying to run sudo. 

Though to be fair I could've just used sudo to run chroot and things would've
been fine. Whilst there some security use-cases where going to the effort to
avoid privilege escalation is required, generally as long as you're comfortable
with what a script is doing (always make sure to read scripts you download from
the internet before running them) there isn't a huge threat to a script to use
sudo. 

## Unshare

So to replace chroot I looked at the [unshare](https://www.man7.org/linux/man-pages/man1/unshare.1.html)
tool. Unshare is used for unsharing namespaces from a process.

[Linux namespaces](https://manpages.debian.org/stretch/manpages/namespaces.7.en.html)
are a way of partitioning resources. There's namespaces for mounts, networking,
PID, users, etc. In this context it gives us a way of separating the subsystem
for the build container from the host. E.g.
Also
- Unsharing the mount namespace allows specifying a different root partition for the process
- Unsharing the user namespace allows mapping a regular user as 'root' (uid 0) for the process
- Unsharing the PID namespace detaches the process IDs from the rest of the system (e.g. the process becomes PID 0)

And with [user namespaces](https://www.man7.org/linux/man-pages/man7/user_namespaces.7.html)
enabled, regular users are allowed to create namespaces without requiring extra
permissions. This means you can achieve the same functionality of chroot using

```shell
$ unshare --fork --user --map-root-user --mount --pid --root $ROOTFS
```

Without requiring root.

## Devfakefs

Now we have a different problem. It's common for scripts and tools to utilize
files such as `/dev/null` or `/dev/urandom` which aren't present in our
container. Usually one would bind mount the host's `/dev`

```shell
# mount --bind /dev $ROOTFS/dev
```

Though bind mounting requires root permissions.

Another option would be to use
[mknod](https://www.man7.org/linux/man-pages/man2/mknod.2.html)
to create the required character devices

```shell
# mknod $ROOTFS/dev/null c 1 3
```

But again it requires root permissions.

We could try being cheeky and just creating a regular file for `/dev/null`.

```shell
$ touch $ROOTFS/dev/null
```

This doesn't require root privileges though it only works if you only ever
write to `/dev/null`, things tend to freak out when they read `/dev/null` and
gets something in return (as I found out when trying to compile busybox which
throws all sorts of weird errors when `/dev/null` isn't null)

```shell
$ echo "hi" > $ROOTFS/dev/null
$ cat $ROOTFS/dev/null
hi
```


However [FUSE](https://www.kernel.org/doc/html/next/filesystems/fuse.html)
file systems don't require root permissions to mount as the FUSE mount utility
uses a SUID binary. So if there was a FUSE file system which could mimic the
behaviour of common `/dev` character devices, our scripts would work without
needing to use sudo to setup the build environment.

That's what [devfakefs](https://codeberg.org/huggles/devfakefs) is.

```shell
$ devfakefs $ROOTFS/dev
$ echo "hi" > $ROOTFS/dev/null
$ cat $ROOTFS/dev/null
<nothing>
```
