---
description: "Learn more about: JetCreateDatabase Function"
title: JetCreateDatabase Function
TOCTitle: JetCreateDatabase Function
ms:assetid: 2b13b038-1694-46d8-b903-9be64384cb06
ms:mtpsurl: https://msdn.microsoft.com/library/Gg269212(v=EXCHG.10)
ms:contentKeyID: 32765515
ms.date: 04/11/2016
ms.topic: reference
api_name: 
- JetCreateDatabase
- JetCreateDatabaseA
- JetCreateDatabaseW
topic_type: 
- apiref
- kbArticle
api_type: 
- DLLExport
- COM
api_location: 
- ESENT.DLL
ROBOTS: INDEX,FOLLOW

---

# JetCreateDatabase Function


_**Applies to:** Windows | Windows Server_

## JetCreateDatabase Function

The **JetCreateDatabase** function creates and attaches a database file to be used with the ESE database engine. Calling [JetCreateDatabase2](./jetcreatedatabase2-function.md) with *cpgDatabaseSizeMax* set to zero is identical to calling **JetCreateDatabase** with *szConnect* set to NULL. Currently, up to seven databases can be created per instance.

```cpp
    JET_ERR JET_API JetCreateDatabase(
      __in          JET_SESID sesid,
      __in          JET_PCSTR szFilename,
      __in_opt      JET_PCSTR szConnect,
      __out         JET_DBID* pdbid,
      __in          JET_GRBIT grbit
    );
```

### Parameters

*sesid*

The database session context to use for the API call.

*szFilename*

The name of the database to be created.

*szConnect*

Reserved for future use. Set to NULL.

*pdbid*

Pointer to a buffer that, on a successful call, contains the identifier of the database. On failure, the value is undefined.

*grbit*

A group of bits specifying zero or more of the following options.


| <p>Value</p> | <p>Meaning</p> | 
|--------------|----------------|
| <p>JET_bitDbOverwriteExisting</p> | <p>By default, if <strong>JetCreateDatabase</strong> is called and the database already exists, the API call will fail and the original database will not be overwritten. JET_bitDbOverwriteExisting changes this behavior, and the old database will be overwritten with a new one. Windows XP and later.</p> | 
| <p>JET_bitDbRecoveryOff</p> | <p>JET_bitDbRecoveryOff turns off logging. Setting this bit loses the ability to replay log files and recover the database to a consistent usable state after a catastrophic event.</p> | 
| <p>JET_bitDbShadowingOff</p> | <p>Setting JET_bitDbShadowingOff will disable the duplication of some internal database structures (shadowing). The duplication of these structures is done for resiliency, so setting JET_bitDbShadowingOff will remove that resiliency.</p> | 



### Return Value

This function returns the [JET_ERR](./jet-err.md) datatype with one of the following return codes. For more information about the possible ESE errors, see [Extensible Storage Engine Errors](./extensible-storage-engine-errors.md) and [Error Handling Parameters](./error-handling-parameters.md).


| <p>Return code</p> | <p>Description</p> | 
|--------------------|--------------------|
| <p>JET_errSuccess</p> | <p>The operation completed successfully.</p> | 
| <p>JET_errDatabaseDuplicate</p> | <p>The database named in <em>szFilename</em> already exists. When this error is returned, the database does not get attached.</p> | 
| <p>JET_errDatabaseInUse</p> | <p>Can be returned if exclusive access was requested, but could not be granted, or if another session has already opened the database exclusively.</p> | 
| <p>JET_errDatabaseInvalidPages</p> | <p>Returned when <em>cpgDatabaseSizeMax</em> is larger than the maximum number of pages allowed in a database. The current maximum is 2147483646 (0x7ffffffe).</p> | 
| <p>JET_errDatabaseInvalidPath</p> | <p>An invalid path was given in <em>szFilename</em>. <em>szFilename</em> must be non-NULL. By default, <em>szFilename</em> must point to a directory that exists. The path will be created if <em>JET_paramCreatePathIfNotExist</em> is set (See <a href="gg294044(v=exchg.10).md">JetSetSystemParameter</a>).</p> | 
| <p>JET_errDatabaseLocked</p> | <p>Indicates that another session has already opened the database exclusively (using JET_bitDbExclusive).</p> | 
| <p>JET_errDatabaseNotFound</p> | <p>The database was not previously attached (See <a href="gg294074(v=exchg.10).md">JetAttachDatabase</a>).</p> | 
| <p>JET_errDatabaseSharingViolation</p> | <p>The database file has already been attached by a different session.</p> | 
| <p>JET_errInTransaction</p> | <p>An attempt was made to create a database while in a transaction.</p> | 
| <p>JET_errInvalidDatabase</p> | <p>An attempt was made to open a file that is not a valid database file.</p> | 
| <p>JET_errOneDatabasePerSession</p> | <p>An attempt was made to open more than one database, and JET_paramOneDatabasePerSession was set. See <a href="gg269337(v=exchg.10).md">Database Parameters</a>.</p> | 
| <p>JET_errOutOfMemory</p> | <p>The operation failed because memory could not be allocated.</p> | 
| <p>JET_errTooManyAttachedDatabases</p> | <p>Only a finite number of database can be attached per instance. The limit is currently seven databases per instance.</p> | 
| <p>JET_wrnDatabaseAttached</p> | <p>A nonfatal warning indicating that the database file has already been attached by this session.</p> | 
| <p>JET_wrnFileOpenReadOnly</p> | <p>JET_wrnFileOpenReadOnly indicates that the file was attached read-only, but <strong>JetCreateDatabase</strong> did not pass JET_bitDbReadOnly. The database is still opened with read-only access.</p> | 



#### Remarks

If the database specified in *szFilename* exists and JET_bitDbOverwriteExisting was not passed in, the API call will fail. If JET_bitDbOverwriteExisting was passed in, the old database file will be deleted first.

If the API creates a database file and then hits another error, it will clean up and delete the file.

**JetCreateDatabase** will implicitly open the database. It is not necessarily to subsequently call [JetOpenDatabase](./jetopendatabase-function.md).

#### Requirements


| 
|
| <p><strong>Client</strong></p> | <p>Requires Windows Vista, Windows XP, or Windows 2000 Professional.</p> | 
| <p><strong>Server</strong></p> | <p>Requires Windows Server 2008, Windows Server 2003, or Windows 2000 Server.</p> | 
| <p><strong>Header</strong></p> | <p>Declared in Esent.h.</p> | 
| <p><strong>Library</strong></p> | <p>Use ESENT.lib.</p> | 
| <p><strong>DLL</strong></p> | <p>Requires ESENT.dll.</p> | 
| <p><strong>Unicode</strong></p> | <p>Implemented as <strong>JetCreateDatabaseW</strong> (Unicode) and <strong>JetCreateDatabaseA</strong> (ANSI).</p> | 



#### See Also

[Extensible Storage Engine Files](./extensible-storage-engine-files.md)  
[JET_ERR](./jet-err.md)  
[JET_DBID](./jet-dbid.md)  
[JET_GRBIT](./jet-grbit.md)  
[JET_SESID](./jet-sesid.md)  
[JetAttachDatabase](./jetattachdatabase-function.md)  
[JetCloseDatabase](./jetclosedatabase-function.md)  
[JetCreateDatabase2](./jetcreatedatabase2-function.md)  
[JetOpenDatabase](./jetopendatabase-function.md)  
[JetSetSystemParameter](./jetsetsystemparameter-function.md)  
[System Parameters](./extensible-storage-engine-system-parameters.md)
