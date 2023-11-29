# notes
* admin user has value 0
* `other` user/group has value 1 (all users are implicitly included in this group)
* `other` can be used for anonymous users (not logged-in)
* upg = user private group

# prerequisite
* users have a unique integer id

# table modifications
* add a `groups` table: id x gid x private (uid can show up in multiple rows, private is a boolean flagging if the group is a upg)
* add boolean columns to all tables with non-internal data: `owner`, `userR`, `userW`, `group`, `groupR`, `groupW`, `otherR`, `otherW`

# queries
these return data along with true/false on success/failure
* select: if set intersection of read-allowed user/groups of all rows
* update: if write permissions
* insert: if write permissions
* delete: if write permissions

# functions
* addUser: user is assigned a user private group
* addGroup: returns new gid

these 2 functions take an id. assert id != 1 for these functions
1. addToGroup: add to `groups` table
2. removeFromGroup: assert not upg, remove from `groups` table


* deleteId: delete all rows from `groups`, delete all owned
* deleteGroup: delete from `groups`

# pros
* stict read/write checking, even for self-owned data, if no write access anywhere, it is guaranteed to be immutable.
* shared ownership
* can detect information leakage through set intersection

# cons
* space/time
* new vulnerability attack vectors?

# todo
* conduct space/time analysis
* retrospective vulnerability mitigation count
