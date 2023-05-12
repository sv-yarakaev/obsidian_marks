called by the rename(2) system call to rename the object to have the parent and name given by the second inode and dentry. 	The filesystem must return -EINVAL for any unsupported or unknown flags.  Currently the following flags are implemented: 
(1) RENAME_NOREPLACE: this flag indicates that if the target of 	the rename exists the rename should fail with -EEXIST instead of replacing the target.  The VFS already checks for existence, so for local filesystems the RENAME_NOREPLACE implementation is  equivalent to plain rename.
(2) RENAME_EXCHANGE: exchange source and target.  Both must exist; this is checked by the VFS.  Unlike plain rename, source and target may be of different type.