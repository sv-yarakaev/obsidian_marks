
To register and unregister a filesystem, use the following API function:
```C
#include <linux/fs.h>

extern int register_filesystem(struct file_system_type *);
extern int unregister_filesystem(struct file_system_type *);
```

The passed struct file_system_type describes your filesystem. When a request is made to mount a filesystem onto a directory in your namespace, the VFS will call the appropriate mount() method for the specific filesystem. New vfsmount referring to the tree returned by ->mount() will be attached to the mountpoint, so that when pathname resolution reaches the mountpoint it will jump into the root of that vfsmount.

You can see all filesystems that are registered to the kernel in the file /proc/filesystems.

### struct file_system_type [[struct file_system_type]]

This describes the filesystem.  As of kernel 2.6.39, the following
members are defined:

```C
struct file_system_operations {
		const char *name;
		int fs_flags;
		struct dentry *(*mount) (struct file_system_type *, int,
					 const char *, void *);
		void (*kill_sb) (struct super_block *);
		struct module *owner;
		struct file_system_type * next;
		struct list_head fs_supers;
		struct lock_class_key s_lock_key;
		struct lock_class_key s_umount_key;
};
```

``name``
	the name of the filesystem type, such as "ext2", "iso9660",
	"msdos" and so on

``fs_flags``
	various flags (i.e. FS_REQUIRES_DEV, FS_NO_DCACHE, etc.)

``mount``
	the method to call when a new instance of this filesystem should
	be mounted

``kill_sb``
	the method to call when an instance of this filesystem should be
	shut down


``owner``
	for internal VFS use: you should initialize this to THIS_MODULE
	in most cases.

``next``
	for internal VFS use: you should initialize this to NULL

  s_lock_key, s_umount_key: lockdep-specific

The mount() method has the following arguments:

``struct file_system_type *fs_type``
	describes the filesystem, partly initialized by the specific
	filesystem code

``int flags``
	mount flags

``const char *dev_name``
	the device name we are mounting.

``void *data``
	arbitrary mount options, usually comes as an ASCII string (see
	"Mount Options" section)

The mount() method must return the root dentry of the tree requested by
caller.  An active reference to its superblock must be grabbed and the
superblock must be locked.  On failure it should return ERR_PTR(error).

The arguments match those of mount(2) and their interpretation depends
on filesystem type.  E.g. for block filesystems, dev_name is interpreted
as block device name, that device is opened and if it contains a
suitable filesystem image the method creates and initializes struct
super_block accordingly, returning its root dentry to caller.

->mount() may choose to return a subtree of existing filesystem - it
doesn't have to create a new one.  The main result from the caller's
point of view is a reference to dentry at the root of (sub)tree to be
attached; creation of new superblock is a common side effect.

The most interesting member of the superblock structure that the mount()
method fills in is the "s_op" field.  This is a pointer to a "struct
super_operations" which describes the next level of the filesystem
implementation.

Usually, a filesystem uses one of the generic mount() implementations
and provides a fill_super() callback instead.  The generic variants are:

``mount_bdev``
	mount a filesystem residing on a block device

``mount_nodev``
	mount a filesystem that is not backed by a device

``mount_single``
	mount a filesystem which shares the instance between all mounts

A fill_super() callback implementation has the following arguments:

``struct super_block *sb``
	the superblock structure.  The callback must initialize this
	properly.

``void *data``
	arbitrary mount options, usually comes as an ASCII string (see
	"Mount Options" section)

``int silent``
	whether or not to be silent on error

