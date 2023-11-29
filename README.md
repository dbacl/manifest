# notes
* `other` user/group has value 0 (all users are implicitly included in this group)
* `other` can be used for anonymous users (not logged-in)
* admin user has value 1
* upg = user private group

# prerequisite
* users have a unique integer id

# table modifications
* add a `groups` table: uid x gid x private (uid can show up in multiple rows, private is a boolean flagging if the group is a upg)
* add boolean columns to all tables with non-internal data: `owner`, `userR`, `userW`, `group`, `groupR`, `groupW`, `otherR`, `otherW`

# queries
these return data along with true/false on success/failure
* select: if set intersection of read-allowed user/groups of all rows
* update: if write permissions
* delete: if write permissions

# functions
* addUser: user is assigned a user private group
* addGroup: returns new gid

* deleteGroup: scan every row and set group to 1 if needed

assert user != 0 for these functions
* addToGroup: add to `groups` table
* removeFromGroup: assert not upg, remove from `groups` table


* allOwned: join of all owned 
* allReadable: join of all readable
* allWritable: join of all writable
* clearData: deletes all owned
* deleteId: assert not 0 or 1, delete all rows from `groups`, delete all owned, if matches gid for a row, set gid to 1

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
