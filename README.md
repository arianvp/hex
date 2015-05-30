# Hex

## Setting up

You will need a system that runs at least `systemd 205` and has `btrfs` installed. You won't need to have a btrfs partition



## Lets go!

Lets first create a store where all our images will reside. This will be a BTRFS filesystem. 

```bash
$ truncate -s 10G store.img
```
