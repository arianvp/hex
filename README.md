# Hex

## Setting up

You will need a system that runs at least `systemd 205` and has `btrfs` installed. You won't need to have a btrfs partition



## Lets go!

Lets first create a store where all our hex images will reside. This will be a BTRFS filesystem. 

```bash
$ truncate -s 10G store.img
$ mkfs.btrfs -f store.img
```

Don't worry. it's a sparse file. It won't take up as much space as 10G. only What is needed.

Lets mount the btrfs filesystem so that we can do stuff with it.

```bash
$ mkdir -p store
$ sudo mount -o loop store.img store
```


Okay. Now we need an image. I'm gonna assume we're running ArchLinux for now. But you could you `deboostrap` or `yum` to populate your own container.

We will install an ArchLinux base system in the container. This is actually too much as we don't need stuff like a Linux kernel etc. We borrow those from the host as we're running containers, not VMs.

If you want to know how to maker smaller base images that throw away all the cruft we don't need. Check out the `image-gen/' folder in this repo to find a plethora of base-image generators for different platforms.

Lets create a container on our filesystem and populate it with an Arch Linux base install

```bash
$ sudo btrfs subvolume create store/arch-base
$ sudo pacstrap -d -G -c -i store/arch-base
```

We actually got a working container now already! Though not with all the fancy features of docker yet.

```bash
$ sudo systemd-nspawn --boot --machine=arch-base --directory=store/arch-base/
```
And you'll be prompted with a login prompt (the user is root and the password is empty). Hurray!





## Creating snapshots

Okay this stuff can't be this easy right? Why are we using docker? Oh yeah! to download insecure layers of images and combine them into one using their union file system. Well... BTRFS can do that too! 

Lets first create a read-only snapshot of our container

```
$ sudo btrfs snapshot store/arch-base store/arch-base-snapshot
```

We can now use this snapshot as a base image for other containers and the filesystem will only store the diffs! pretty nifty huh?

 And move it out of the btrfs filesystem

```
$ sudo btrfs send store/arch-base-snapshot -f ./arch-base-img
```

We can now share this snapshot with others.


...  Coming soon:

* Layered snapshots -  How to send diffs of base images to reduce bandwidth when sharing containers































