apple notes
shm = shared memory file
wal = write ahead log files

### these files are untouched 

```
Notes   1008 apresswa  txt       REG                1,4      32768  1470466 /Users/apresswa/Library/Containers/com.apple.Notes/Data/Library/Notes/NotesV7.storedata-shm
Notes   1008 apresswa    3u      REG                1,4     155648  1470452 /Users/apresswa/Library/Containers/com.apple.Notes/Data/Library/Notes/NotesV7.storedata
Notes   1008 apresswa    4u      REG                1,4     395552  1470465 /Users/apresswa/Library/Containers/com.apple.Notes/Data/Library/Notes/NotesV7.storedata-wal
Notes   1008 apresswa    5u      REG                1,4      32768  1470466 /Users/apresswa/Library/Containers/com.apple.Notes/Data/Library/Notes/NotesV7.storedata-shm
```

There's a lot of crap in the filenames you get by the lsof of the Notes processes, both in sqlite temporary files and other shit. So I put a bunch of stuff into an inclusion list (it's in `notes-grep-exclude.txt`- hopefully i'm not filtering out anything important)

And then grepped them:
```
5677 ◯  {lsof -p 966; lsof -p 1008} | grep -v -f notes-grep-exclude.txt | awk '{ $1=$2=""; $3=$4=""; $5=$6=""; $7=$8="" ; print $0}' | cut -d ' ' -f 9-
/Users/apresswa/Library/Containers/com.apple.Notes.datastore/Data
/System/Library/PrivateFrameworks/NotesShared.framework/Versions/A/XPCServices/com.apple.Notes.datastore.xpc/Contents/MacOS/com.apple.Notes.datastore
/Users/apresswa/Library/Group Containers/group.com.apple.notes/NoteStore.sqlite
/Users/apresswa/Library/Group Containers/group.com.apple.notes/NoteStore.sqlite
/Users/apresswa/Library/Containers/com.apple.Notes/Data
/Applications/Notes.app/Contents/MacOS/Notes
/Users/apresswa/Library/Containers/com.apple.Notes/Data/Library/Notes/NotesV7.storedata
```

(that command string was tough to iterate, i ended up making a poor-coders-watch with `while true; do clear; that-command | and | shit; sleep 2; dnone`)

So these files 
* `/Users/apresswa/Library/Containers/com.apple.Notes.datastore/Data` is an empty directory structure. Symlinks are probably just to get config files and shit from outside the sandbox.\*
* `/System/Library/PrivateFrameworks/NotesShared.framework/Versions/A/XPCServices/com.apple.Notes.datastore.xpc/Contents/MacOS/com.apple.Notes.datastore` is an executable and seems to be what persists data to iCloud and local storage.
* /Users/apresswa/Library/Containers/com.apple.Notes/Data has a lot of stuff, it's all of my shit. Going through this directory structure\* it looks like apart from all my uploaded file blobs there's also a lot of other metadata stuff.
```/Users/apresswa/Library/Containers/com.apple.Notes/Data/CloudKit/8dfaed7cb02f131324842fa27d653c460d733303/Records/pcs.db:                                                                                        SQLite 3.x database, user version 8, last written using SQLite version 3019003
/Users/apresswa/Library/Containers/com.apple.Notes/Data/CloudKit/8dfaed7cb02f131324842fa27d653c460d733303/Records/Records.db:                                                                                    SQLite 3.x database, user version 165930232, last written using SQLite version 3019003
/Users/apresswa/Library/Containers/com.apple.Notes/Data/CloudKit/8dfaed7cb02f131324842fa27d653c460d733303/MMCS/.cs/ChunkStoreDatabase:                                                                           SQLite 3.x database, last written using SQLite version 3019003
/Users/apresswa/Library/Containers/com.apple.Notes/Data/CloudKit/8dfaed7cb02f131324842fa27d653c460d733303/AssetsDb/Assets.db:                                                                                    SQLite 3.x database, user version 17957640, last written using SQLite version 3019003
/Users/apresswa/Library/Containers/com.apple.Notes/Data/Library/Notes/NotesV7.storedata:                                                                                                                         SQLite 3.x database, last written using SQLite version 3019003
/Users/apresswa/Library/Containers/com.apple.Notes/Data/Library/Caches/com.apple.Notes/Cache.db:                                                                                                                 SQLite 3.x database, last written using SQLite version 3019003```

Those are all the SQL dbs

and these are all the rest:
```/Users/apresswa/Library/Containers/com.apple.Notes/Data/Library/Preferences/com.xerox.xeroxfeatures.pde.plist:                                                                                                   XML 1.0 document text, ASCII text
/Users/apresswa/Library/Containers/com.apple.Notes/Data/Library/Preferences/ByHost/com.apple.Notes.A17F494D-C16A-5E62-A7EE-836E5FE9202E.plist:                                                                   Apple binary property list
/Users/apresswa/Library/Containers/com.apple.Notes/Data/Library/Preferences/ByHost/com.apple.Notes.SharingExtension.A17F494D-C16A-5E62-A7EE-836E5FE9202E.plist:                                                  Apple binary property list
/Users/apresswa/Library/Containers/com.apple.Notes/Data/Library/Preferences/com.apple.Notes.plist:                                                                                                               Apple binary property list
/Users/apresswa/Library/Containers/com.apple.Notes/Data/Library/Cookies/Cookies.binarycookies:                                                                                                                   data
/Users/apresswa/Library/Containers/com.apple.Notes/Data/Library/Caches/com.apple.Notes/fsCachedData/1AFC6CE5-585A-4D01-AE7F-A0F52D19E691:                                                                        ASCII text, with very long lines, with no line terminators
/Users/apresswa/Library/Containers/com.apple.Notes/Data/Library/Caches/RemoteConfiguration.plist:                                                                                                                XML 1.0 document text, ASCII text```



\* (Disclaimer i have no fuckin clue how  apple sandboxing works). I ran thru these files by doing `find $1 | xargs file | egrep -v 'directory$' | egrep -v "broken sym" | egrep -v "No such"` but the broken symlinks are probably where it gets the important stuff.