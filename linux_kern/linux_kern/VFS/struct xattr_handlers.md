On filesystems that support extended attributes (xattrs), the s_xattr
superblock field points to a NULL-terminated array of xattr handlers.
Extended attributes are name:value pairs.

[[name]]
[[prefix]]
[[list]]
[[get]]
[[set]]

When none of the xattr handlers of a filesystem match the specified attribute name or when a filesystem doesn't support extended attributes, the various ``*xattr(2)`` system calls return -EOPNOTSUPP.