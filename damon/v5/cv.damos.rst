Implement Data Access Monitoring-based Memory Operation Schemes

DAMON[1] can be used as a primitive for data access monitoring-based memory
operation schemes optimizations.  That said, users who want such optimizations
should run DAMON, read the monitoring results, analyze it, plan a new memory
management scheme, and apply the new scheme by themselves.  Such efforts will
be inevitable for some complicated optimizations.

However, in many other cases, the users would just want the system to apply an
memory management action to a memory region of a specific size having a
specific access frequency for a specific time.  For example, "page out a memory
region larger than 100 MiB keeping only rare accesses more than 10 minutes", or
"Use THP for a memory region larger than 2 MiB maintaing frequent accesses for
more than 5 seconds".

This RFC patchset makes DAMON to handle such requests.  All the things users
need to do for such simple cases is only specifying their requests to DAMON in
a simple form.


Sequence Of Patches
===================

The patches are based on the v5.5 plus v5 DAMON patchset[1] and Minchan's
``madvise()`` factor-out patch[2].  Minchan's patch was necessary for reuse of
``madvise()`` code in DAMOS.  You can also clone the complete git tree:

    $ git clone git://github.com/sjp38/linux -b damos/rfc/v2

The web is also available:
https://github.com/sjp38/linux/releases/tag/damos/rfc/v2

The first patch allows DAMON to reuse ``madvise()`` code for the actions.  The
second patch implements the handling of the schemes in DAMON and exports a
kernel space programming interface for it.  Finally, the third patch implements
a debugfs interface for privileged user space people and programs.

[1] !!!UPDATE THE URL!!!
[2] https://lore.kernel.org/linux-mm/20200128001641.5086-2-minchan@kernel.org/


Patch History
=============

Changes from RFC v1
(https://lore.kernel.org/linux-mm/20200210150921.32482-1-sjpark@amazon.com/)
 - Reset age of the region after applying an action of a scheme
 - Rename rules to schemes
