# notes
* `other` user/group has value 0 (all users are implicitly included in this group)
* `other` can be used for anonymous users (not logged-in)

# prerequisite
* users have a unique integer id

# table modifications
* add a `groups` table: uid x gid x private (uid can show up in multiple rows, private is a boolean flagging if the group is a user private group)
* add boolean columns to all tables with non-internal data: `owner`, `userR`, `userW`, `group`, `groupR`, `groupW`, `otherR`, `otherW`

# queries
these return data along with true/false on success/failure
* select: true if set intersection of read-allowed groups of all rows
* update: writes if owner
* delete: deletes if write permissions

* query: assert not select. assert that the changes are to internal tables that are not owned by dbacl

# functions
* addUser: user is assigned a user private group
* addGroup: returns new gid

assert user != 0 for these functions
* addToGroup: adds to `groups` table
* removeFromGroup: removes from `groups` table

* deleteGroup: removes from `groups` table, scan every table with non-internal data for 

* allUser
* accepts a user, joins all tables with rows where user has access by user or group

* clearData
* allOwned

# pros
* stict read/write checking, even for self-owned data, if no write access anywhere, it is guaranteed to be immutable.
* shared ownership
* can detect information leakage through set intersection
* can easily check all readable data, all writable data, all public data

# cons
* space/time
* new vulnerability attack vectors?

# todo
* conduct space/time analysis
* retrospective vulnerability mitigation count
