# notes
* admin group has value 0
* `other` group has value 1 (all users are implicitly included in this group)
* `other` can be used for anonymous users (not logged-in)
* upg = user private group

# prerequisite
* users have a unique identifier

# table modifications
* add a `groups` table: id x gid x private (uid can show up in multiple rows, private is a boolean flagging if the group is a upg)
* add columns to all tables with non-internal data: `user`, `userR`, `userW`, `group`, `groupR`, `groupW`, `otherR`, `otherW` (RW are booleans, others are ints)

# queries
these return data along with true/false on success/failure
* select: if set intersection of read-allowed user/groups of all rows
* update: if write permissions
* insert: allowed (no table permissions)
* delete: if write permissions

# functions
* addUser: user is assigned a user private group
* addGroup: returns new gid

* addToGroup: add to `groups` table
* removeFromGroup: assert not upg, remove from `groups` table

* deleteUser: delete all rows from `groups`, delete all (partially) owned
* deleteGroup: delete from `groups`

# pros
* stict read/write checking, even for self-owned data, if no write access anywhere, it is guaranteed to be immutable.
* shared ownership
* can detect information leakage through set intersection (let's say there is a table of users, user's address, and stores. You only have access to see stores and users. If there was a query that showed which users can visit which stores, this would implicitly leak their general location.)
* robustly delete all owned and partially owned data
* easy to understand
* simple well-defined primitives allow for more complex hierarchies
* implements gdpr's right to be forgotten, right to be informed
* due to permissions per row nature, information will be seperated into different tables based on sensitivity of data.

# cons
* space/time overhead
* new vulnerability attack vectors?

# todo
* conduct space/time analysis

# further work
* use in a larger, more complex project
* table permissions
* retrospective vulnerability mitigation count
