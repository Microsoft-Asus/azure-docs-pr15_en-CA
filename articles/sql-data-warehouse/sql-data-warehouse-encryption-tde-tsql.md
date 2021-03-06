<properties
   pageTitle="Transparent Data Encryption in SQL Data Warehouse (T-SQL)| Microsoft Azure"
   description="Transparent Data Encryption (TDE) in SQL Data Warehouse (T-SQL)"
   services="sql-data-warehouse"
   documentationCenter=""
   authors="ronortloff"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.workload="data-management"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="09/24/2016"
   ms.author="rortloff;barbkess;sonyama"/>

# <a name="get-started-with-transparent-data-encryption-tde"></a>Get started with Transparent Data Encryption (TDE)


> [AZURE.SELECTOR]
- [Security Overview](sql-data-warehouse-overview-manage-security.md)
- [Authentication](sql-data-warehouse-authentication.md)
- [Encryption (Portal)](sql-data-warehouse-encryption-tde.md)
- [Encryption (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)

## <a name="required-permssions"></a>Required Permssions

To enable Transparent Data Encryption (TDE), you must be an administrator or a member of the dbmanager role.

## <a name="enabling-encryption"></a>Enabling Encryption

Follow these steps to enable TDE for a SQL Data Warehouse:

1. Connect to the *master* database on the server hosting the database using a login that is an administrator or a member of the **dbmanager** role in the master database
2. Execute the following statement to encrypt the database.

```sql
ALTER DATABASE [AdventureWorks] SET ENCRYPTION ON;
```

## <a name="disabling-encryption"></a>Disabling Encryption

Follow these steps to disable TDE for a SQL Data Warehouse:

1. Connect to the *master* database using a login that is an administrator or a member of the **dbmanager** role in the master database
2. Execute the following statement to encrypt the database.

```sql
ALTER DATABASE [AdventureWorks] SET ENCRYPTION OFF;
```

> [AZURE.NOTE] A paused SQL Data Warehouse must be resumed before making changes to the TDE settings.

## <a name="verifying-encryption"></a>Verifying Encryption

To verify encryption status for a SQL Data Warehouse, follow the steps below:

1. Connect to the *master* or instance database using a login that is an administrator or a member of the **dbmanager** role in the master database
2. Execute the following statement to encrypt the database.

```sql
SELECT
    [name],
    [is_encrypted]
FROM
    sys.databases;
```

A result of ```1``` indicates an encrypted database, ```0``` indicates a non-encrypted database.

## <a name="encryption-dmvs"></a>Encryption DMVs  

- [sys.databases][] 
- [sys.dm_pdw_nodes_database_encryption_keys][]


<!--Anchors-->
[Transparent Data Encryption (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx
[sys.databases]: http://msdn.microsoft.com/library/ms178534.aspx  
[sys.dm_pdw_nodes_database_encryption_keys]: https://msdn.microsoft.com/library/mt203922.aspx  

<!--Image references-->

<!--Link references-->
