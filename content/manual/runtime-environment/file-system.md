---
title: "File System"
weight: 20
---

The File System interface defines CORBA operations that exists to provide a runtime abstraction of an Operating System's (OS) real file system. It gives REDHAWK the ability to have a single interface for reading and writing individual files within a file system regardless of the underlying implementation in the OS.

Files that are stored on the File System may be either plain files or directories.

Characters and symbols that are valid in directories and file names consist of:

  - Upper and lowercase letters
  - Numbers
  - “\_” (underscore)
  - “-” (hyphen)
  - “.” (period)
      - The file names “.” and “..” are invalid in the context of a File System.

Path names are structured according to the POSIX specification where the “/” (forward slash) is a valid character that acts as the separator between directories.

Additionally, the File System interface provides implementation of many standard functions such as:

  - `remove()`
  - `copy()`
  - `exists()`
  - `list()`
  - `create()`
  - `open()`
  - `mkdir()`
  - `rmdir()`
  - `query()`

File System attached to the Domain Manager mounts with `$SDRROOT/dom` as the root. Each Device Manager mounts a File System with `$SDRROOT/dev` as the root.

### File Manager

The File Manager exists to manage multiple distributed file systems. This interface allows these file systems to act as a single entity, though they may span multiple physical file systems on different pieces of hardware. This provides for a distributed file system that functions as a single file system across multiple Device Managers and the Domain Manager.

The File Manager inherits the IDL interface of a File System. It then delegates tasks from the Core Framework based off of the path names to the correct mounted File System, depending on where that File System is mounted. It is also responsible for copying the appropriate component files into the specific Device Manager’s File System as applications are installed and launched.
