# Hex

## Setting up

You will need a system that runs at least `systemd 205` and has `btrfs` installed. You won't need to have a btrfs partition



## Lets go!

Lets first create a store where all our hex images will reside. This will be a BTRFS filesystem. 

```bash
$ truncate -s 10G store.img
```

Don't worry. it's a sparse file. It won't take up as much space as 10G. only What is needed.

Lets mount the btrfs filesystem so that we can do stuff with it.

```bash
$ sudo mount -o loop store.img store
```



