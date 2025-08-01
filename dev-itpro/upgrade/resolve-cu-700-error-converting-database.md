---
title: "Resolving Coduenit 700 compilation error"
description: Explains how to resolve the compilation error that you get for Codeunit 700 when converting a database from Dynamics NAV to Business Central.
ms.custom: evergreen
ms.date: 04/18/2024
ms.update-cycle: 1095-days
ms.topic: concept-article
author: jswymer
---
# Resolving Codeunit 700 compilation error 
This article explains how to resolve the compilation error that you get for codeunit **700 Page Management** when converting a [!INCLUDE[nav2018_md](../developer/includes/nav2018_md.md)] database to  [!INCLUDE[prodhort](../developer/includes/prod_short.md)].

## Resolution

In `VerifyPageID` function, replace the following code: 

```
EXIT(PageMetadata.GET(PageID) AND (PageMetadata.EntitySetName = TableID));
```

With the following code:

```
EXIT(PageMetadata.GET(PageID) AND (PageMetadata.SourceTable = TableID));
```


## Related information  
 [Converting a Database](Converting-a-Database.md)  
 [Resolving Compilation Errors When Converting a Dynamics NAV 2018 Database](Resolve-Compile-Errors-When-Converting-Dynamics-NAV-2018-Database.md)  
