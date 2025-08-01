---
title: (v1.0) Create purchaseInvoices
description: (v1.0) Creates a purchase invoice object in Dynamics 365 Business Central.
author: SusanneWindfeldPedersen
ms.custom: evergreen
ms.topic: reference
ms.devlang: al
ms.date: 05/01/2024
ms.update-cycle: 1095-days
ms.author: solsen
ms.reviewer: solsen
---

# Create purchaseInvoices (v1.0)
Create a purchase invoice report object in [!INCLUDE[prod_short](../../../includes/prod_short.md)].

## HTTP request (v1.0)
Replace the URL prefix for [!INCLUDE[prod_short](../../../includes/prod_short.md)] depending on environment following the [guideline](../../v1.0/endpoints-apis-for-dynamics.md).

```
POST businesscentralPrefix/companies({id})/purchaseInvoices
```

## Request headers (v1.0)

|Header         |Value                        |
|---------------|-----------------------------|
|Authorization  |Bearer {token}. Required.    |
|Content-Type   |application/json             |

## Request body (v1.0)
In the request body, supply a JSON representation of a **purchaseInvoices** object.
## Response (v1.0)
If successful, this method returns ```201 Created``` response code and a **purchaseInvoices** object in the response body.

## Example (v1.0)

**Request**

Here is an example of a request.

```json
POST https://{businesscentralPrefix}/api/v1.0/companies({id})/purchaseInvoices
Content-type: application/json

{
  "id": "id-value",
  "number": "1009",
  "invoiceDate": "2015-12-31",
  "vendorNumber": "GL00000008",
  "currencyCode": "GBP"
}
```

## Related information
[Tips for working with the APIs](../../../developer/devenv-connect-apps-tips.md)  

[Purchase Invoice](../resources/dynamics_purchaseinvoice.md)  
[Get Purchase Invoice](../api/dynamics_purchaseinvoice_get.md)  
[Update Purchase Invoice](../api/dynamics_purchaseinvoice_update.md)  
[Delete Purchase Invoice](../api/dynamics_purchaseinvoice_delete.md)  