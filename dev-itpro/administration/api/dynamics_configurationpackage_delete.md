---
title: (automation API) Delete configurationPackage
description: Deletes a configuration package object in Dynamics 365 Business Central.
author: SusanneWindfeldPedersen
ms.topic: reference
ms.devlang: al
ms.date: 05/31/2024
ms.author: solsen
ms.reviewer: solsen
---

<!-- NOTE: This article is an auto-generated stub from the metadata file. -->
<!-- The sections marked with an EDIT_IS_REQUIRED require manual editing. -->
# (automation API) Delete configurationPackage

Deletes a configuration package from [!INCLUDE[d365fin_long_md](../../includes/d365fin_long_md.md)].

## HTTP request

Replace the URL prefix for [!INCLUDE [prod_short](../../includes/prod_short.md)] depending on environment following the [guideline](../../api-reference/v2.0/enabling-apis-for-dynamics-nav.md).

```
DELETE /microsoft/automation/v2.0/companies({companyId})/configurationPackages({packageId})
```

## Request headers

|Header|Value|
|------|-----|
|Authorization  |Bearer {token}. Required. |
|If-Match       |Required. When this request header is included and the eTag provided does not match the current tag on the **configurationPackage**, the **configurationPackage** will not be updated. |


## Request body

Do not supply a request body for this method.

## Response

If successful, this method returns ```204 No Content``` response code and deletes the configurationPackage. It does not return anything in the response body.

## Example

**Request**

Here is an example of the request.
```json
DELETE https://api.businesscentral.dynamics.com/v2.0/{environment name}/api/microsoft/automation/v2.0/companies({companyId})/configurationPackages({packageId})
```

**Response**

Here is an example of the response.

```json
HTTP/1.1 204 No Content
```

## Related information

[Tips for working with the APIs](../../developer/devenv-connect-apps-tips.md)  
[configurationPackage](../resources/dynamics_configurationPackage.md)