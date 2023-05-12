This describes the filesystem. The following members are defined:
```C
struct file_system_type {
        const char *name;
        int fs_flags;
        int (*init_fs_context)(struct fs_context *);
        const struct fs_parameter_spec *parameters;
        struct dentry *(*mount) (struct file_system_type *, int,
                const char *, void *);
        void (*kill_sb) (struct super_block *);
        struct module *owner;
        struct file_system_type * next;
        struct hlist_head fs_supers;

        struct lock_class_key s_lock_key;
        struct lock_class_key s_umount_key;
        struct lock_class_key s_vfs_rename_key;
        struct lock_class_key s_writers_key[SB_FREEZE_LEVELS];

        struct lock_class_key i_lock_key;
        struct lock_class_key i_mutex_key;
        struct lock_class_key invalidate_lock_key;
        struct lock_class_key i_mutex_dir_key;
};

```

*  [[file_system_type:name | name]] the name of the filesystem type, such as "ext2", "vfat" and so n
* [[fs_flags]] various flags(i.e. FS_REQUIRES_DEV, FS_NO_DCACHE, etc)
* [[init_fs_context]]  Initializes 'struct fs_context' ->ops and ->fs_private fields with filesystem-specific data.