# implementation notes
`other` group has value 0 (all users are implicitly included in this group)

# prerequisite
users have a unique integer id

# table modifications
add a `groups` table: uid x gid (uid can show up in multiple rows)  
add boolean columns to all tables with non-internal data: `owner`, `userR`, `userW`, `groupR`, `groupW`, `otherR`, `otherW`

# functions
addUser: all users are assigned a gid, user has a 
addGroup: returns new gid

addToGroup
removeFromGroup:
deleteGroup


allUser
allVisible
allWritable
accepts a user, joins all tables with rows where user has access by user or group

clearData
allOwned

# pros
* stict read/write checking, even for self-owned data, if no write access, it is guarenteed to be immutable.

# todo
* conduct space/time analysis
* retrospective vulnerability mitigation count
