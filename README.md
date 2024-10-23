# virtfs module for FreeBSD

This is a fork of <https://github.com/Juniper/virtfs> stripping the outdated freebsd-src tree and re-arranging the files to be built out-of-tree using `LOCAL_MODULES_DIR`.

Additionaly changes are applied to compile the source against recent FreeBSD 14.1.

## Use

On the host, enable VirtFS shares. Under FreeBSD, load the kernel module and issue

```sh
mount -t virtfs -o trans=virtio the_tag_to_use /var/mountpoint
```

### QEMU

Launch QEMU with ```-virtfs local,path=the/path/on/the/host,mount_tag=the_tag_to_use,security_model=none```.

## Build

Clone the repository and apply the following `make.conf` stanzas to include it in your kernel build.

```make
LOCAL_MODULES_DIR="wherever_your_git_is/virtfs"
LOCAL_MODULES="9pfs 9pnet"
```

See <https://man.freebsd.org/cgi/man.cgi?query=build> for reference.

## History

After forking the work of Juniper was isolated by

```sh
git filter-repo  --path 'sys/dev/virtio/9pfs' --path 'sys/dev/virtio/9pnet' --path-glob 'sys/dev/virtio/virtio_fs_*' --path 'sys/modules/virtio/9pfs’ --path 'sys/modules/virtio/9pnet’
```

This retained all their work on 9pfs and the underlying 9p protocol implementation.
Changes to ```sys/dev/virtio/network/if_vtnet.c``` were discarded.
Hierarchy was flattened for ease of use.
FreeBSD kernel version changes were applied.