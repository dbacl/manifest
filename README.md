# notes
* admin group has value 0
* `other` user/group has value 1 (all users are implicitly included in this group)
* `other` can be used for anonymous users (not logged-in)
* upg = user private group

# prerequisite
* users have a unique identifier

# table modifications
* add a `groups` table: id x gid x private (uid can show up in multiple rows, private is a boolean flagging if the group is a upg)
* add columns to all tables with non-internal data: `owner`, `userR`, `userW`, `group`, `groupR`, `groupW`, `otherR`, `otherW` (RW are booleans, others are ints)

# queries
these return data along with true/false on success/failure
* select: if set intersection of read-allowed user/groups of all rows
* update: if write permissions
* insert: allowed (no table permissions)
* delete: if write permissions

# functions
* addUser: user is assigned a user private group
* addGroup: returns new gid

these 2 functions take an id. assert id != 1 for these functions
1. addToGroup: add to `groups` table
2. removeFromGroup: assert not upg, remove from `groups` table

assert id != 0 or 1 for these
* deleteUser: delete all rows from `groups`, delete all (partially) owned
* deleteGroup: delete from `groups`

# pros
* stict read/write checking, even for self-owned data, if no write access anywhere, it is guaranteed to be immutable.
* shared ownership
* can detect information leakage through set intersection
* robustly delete all owned and partially owned data
* easy to understand
* simple well-defined primitives allow for more complex hierarchies
* implements gdpr's right to be forgotten, right to be informed

# cons
* space/time overhead
* new vulnerability attack vectors?

# todo
* conduct space/time analysis

# further work
* use in a larger, more complex project
* table permissions
* retrospective vulnerability mitigation count