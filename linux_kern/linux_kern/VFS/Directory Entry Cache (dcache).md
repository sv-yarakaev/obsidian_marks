[[dcache]]

The VFS implements the open(2), stat(2), chmod(2), and similar system calls. The pathname argument that is passed to them is used by the VFS to search through the directory entry cache (also known as the dentry cache or dcache). This provides a very fast look-up mechanism to translate a pathname (filename) into a specific dentry. Dentries live in RAM and are never saved to disc: they exist only for performance.

The dentry cache is meant to be a view into your entire filespace.  As
most computers cannot fit all dentries in the RAM at the same time, some
bits of the cache are missing.  In order to resolve your pathname into a
dentry, the VFS may have to resort to creating dentries along the way,
and then loading the inode.  This is done by looking up the inode.