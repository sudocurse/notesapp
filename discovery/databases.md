Databases

# CloudKit sandbox filesystem
This is all the shit inside the sandboxed directory. everything's empty except:

```
~/Library/Containers/com.apple.Notes/Data/CloudKit/hash redacted03/Records/pcs.db:              SQLite 3.x database, user version 8, last written using SQLite version 3019003
~/Library/Containers/com.apple.Notes/Data/CloudKit/hash redacted03/Records/Records.db:             SQLite 3.x database, user version 165930232, last written using SQLite version 3019003
~/Library/Containers/com.apple.Notes/Data/CloudKit/hash redacted03/MMCS/.cs/ChunkStoreDatabase:    SQLite 3.x database, last written using SQLite version 3019003
~/Library/Containers/com.apple.Notes/Data/CloudKit/hash redacted03/AssetsDb/Assets.db:             SQLite 3.x database, user version 17957640, last written using SQLite version 3019003
~/Library/Containers/com.apple.Notes/Data/Library/Notes/NotesV7.storedata:                                                  SQLite 3.x database, last written using SQLite version 3019003
~/Library/Containers/com.apple.Notes/Data/Library/Caches/com.apple.Notes/Cache.db:                                          SQLite 3.x database, last written using SQLite version 3019003
```

### pcs.db 
There's identifiers that either are UUIDs, some are prepended with Share. Maybe this is all my shared notes. I only have about 8 here. 
Columns include `accountID` `zoneID`  which i suppose points to the owner of the note, `shareID` and `parentID` which likely links to folders. 

assets and records didn't have much in them.

* `/Users/apresswa/Library/Group Containers/group.com.apple.notes/NoteStore.sqlite`
* `/Users/apresswa/Library/Containers/com.apple.Notes/Data/Library/Notes/NotesV7.storedata`
* `/Users/apresswa/Library/Containers/com.apple.Notes/Data/Library/Caches/com.apple.Notes/Cache.db`

All of these files seem to just be skeleton...

# NoteStore.sqlite

* `ZICCLOUDSTATE` some config state table?
* `ZICCLOUDSYNCINGOBJECT` COLUMN SPACE IS T H I C C, lots of crypto stuff, lots of "marked for deletion" or "ZNESTEDTITLEFORSORTING", shit like that. `ZTITLE` has all of my note titles!
* `ZICLOCATION`
* `ZICNOTEDATA`
* `ZICSEARCHINDEXTRANSACTION`
* `ZICSERVERCHANGETOKEN`
* `ZNEXTID`
* `Z_PRIMARYKEY`
* `Z_METADATA`
* `Z_MODELCACHE`
* `ACHANGE` transactions i guess
* `ATRANSACTION` timestamps, author, "bundleid" which is usually com.apple.Notes but in one instance it's AAACCOUNTS.com.apple.accountsd
space in the fucking filepath, but the containing folder, `/Users/apresswa/Library/Group Containers/group.com.apple.notes/`, seems to have all of the media i have up there. 

## NotesV7.storedata
iirc probably where the notes actually are. 

## Cache.db
has a cfurl_cache_response table that seems to have... cached thumbnails? URLs from all the sites i visit. now.... these things also exist in the same folder as this db under `fsCachedData` and probably the Group Containers one

