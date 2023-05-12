[[superblock object]]

A superblock object represents a mounted filesystem.

### struct super_operation [[struct super_operation]]

This describes how the VFS can manipulate the superblock of your
filesystem.  As of kernel 2.6.22, the following members are defined:
```C
struct super_operations {
		struct inode *(*alloc_inode)(struct super_block *sb);
		void (*destroy_inode)(struct inode *);

		void (*dirty_inode) (struct inode *, int flags);
		int (*write_inode) (struct inode *, int);
		void (*drop_inode) (struct inode *);
		void (*delete_inode) (struct inode *);
		void (*put_super) (struct super_block *);
		int (*sync_fs)(struct super_block *sb, int wait);
		int (*freeze_fs) (struct super_block *);
		int (*unfreeze_fs) (struct super_block *);
		int (*statfs) (struct dentry *, struct kstatfs *);
		int (*remount_fs) (struct super_block *, int *, char *);
		void (*clear_inode) (struct inode *);
		void (*umount_begin) (struct super_block *);

		int (*show_options)(struct seq_file *, struct dentry *);

		ssize_t (*quota_read)(struct super_block *, int, char *, size_t, loff_t);
		ssize_t (*quota_write)(struct super_block *, int, const char *, size_t, loff_t);
		int (*nr_cached_objects)(struct super_block *);
		void (*free_cached_objects)(struct super_block *, int);
};

```
All methods are called without any locks being held, unless otherwise
noted.  This means that most methods can block safely.  All methods are
only called from a process context (i.e. not from an interrupt handler
or bottom half).

[[alloc_inode]]  

[[destroy_inode]] 
[[dirty_inode]] 





