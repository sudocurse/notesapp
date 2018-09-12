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
/Users/apresswa/Library/Group Containers/group.com.apple.notes/NoteStore.sqlite-shm
/Users/apresswa/Library/Group Containers/group.com.apple.notes/NoteStore.sqlite
/Users/apresswa/Library/Group Containers/group.com.apple.notes/NoteStore.sqlite-wal
/Users/apresswa/Library/Group Containers/group.com.apple.notes/NoteStore.sqlite-shm
/Users/apresswa/Library/Group Containers/group.com.apple.notes/NoteStore.sqlite
/Users/apresswa/Library/Group Containers/group.com.apple.notes/NoteStore.sqlite-wal
/Users/apresswa/Library/Containers/com.apple.Notes/Data
/Applications/Notes.app/Contents/MacOS/Notes
/Users/apresswa/Library/Containers/com.apple.Notes/Data/Library/Notes/NotesV7.storedata-shm
/Users/apresswa/Library/Containers/com.apple.Notes/Data/Library/Caches/com.apple.Notes/Cache.db-shm
/Users/apresswa/Library/Containers/com.apple.Notes/Data/Library/Notes/NotesV7.storedata
/Users/apresswa/Library/Containers/com.apple.Notes/Data/Library/Notes/NotesV7.storedata-wal
/Users/apresswa/Library/Containers/com.apple.Notes/Data/Library/Notes/NotesV7.storedata-shm
/Users/apresswa/Library/Containers/com.apple.Notes/Data/Library/Saved Application State/com.apple.Notes.savedState/data.data

/Users/apresswa/Library/Containers/com.apple.Notes/Data/Library/Caches/com.apple.Notes/Cache.db
/Users/apresswa/Library/Containers/com.apple.Notes/Data/Library/Caches/com.apple.Notes/Cache.db-wal
/Users/apresswa/Library/Containers/com.apple.Notes/Data/Library/Caches/com.apple.Notes/Cache.db-shm
/Users/apresswa/Library/Containers/com.apple.Notes/Data/Library/Saved Application State/com.apple.Notes.savedState/data.data
/Users/apresswa/Library/Containers/com.apple.Notes/Data/Library/Saved Application State/com.apple.Notes.savedState/windows.plist
/Users/apresswa/Library/Containers/com.apple.Notes/Data/Library/Saved Application State/com.apple.Notes.savedState/window_1.data
/Users/apresswa/Library/Containers/com.apple.Notes/Data/Library/Saved Application State/com.apple.Notes.savedState/window_3.data
```


