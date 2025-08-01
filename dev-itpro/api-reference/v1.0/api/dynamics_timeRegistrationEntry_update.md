---
title: (v1.0) Patch timeRegistrationEntries| Microsoft Docs
description: (v1.0) Patch a timeRegistrationEntries in Dynamics 365 Business Central.
 
author: SusanneWindfeldPedersen
ms.custom: evergreen
ms.topic: reference
ms.devlang: al
ms.date: 05/01/2024
ms.update-cycle: 1095-days
ms.author: solsen
ms.reviewer: solsen
---

# Update timeRegistrationEntries (v1.0)
Update a timeRegistrationEntry in [!INCLUDE[prod_short](../../../includes/prod_short.md)].


## HTTP request
Replace the URL prefix for [!INCLUDE[prod_short](../../../includes/prod_short.md)] depending on environment following the [guideline](../../v1.0/endpoints-apis-for-dynamics.md).
```
PATCH businesscentralPrefix/companies({companyId})/timeRegistrationEntries({timeregistrationId})
```

## Request headers (v1.0)

|Header        |Value                    |
|--------------|-------------------------|
|Authorization |Bearer {token}. Required.|
|Content-Type  |application/json         |
|If-Match  |*application/json*         |


## Example (v1.0)

**Request**

Here is an example of the request.
```json
PATCH https://{businesscentralPrefix}/api/v1.0/companies({id})/timeRegistrationEntries({timeregistrationId})
Content-type: application/json

{
	"quantity": 8
}
```

**Response**

Here is an example of the response. 

> [!NOTE]  
>   The response object shown here may be truncated for brevity. All of the properties will be returned from an actual call.

```json
HTTP/1.1 200 OK
Content-type: application/json

{
    "id": "967b7868-5e15-4adc-9698-cd88e509395b",
    "employeeId": "dd6ce54a-d14f-40dc-a10d-1d4d18758c76",
    "employeeNumber": "RL",
    "lineNumber": 10000,
    "date": "2019-01-02",
    "quantity": 8,
    "status": "Open",
    "unitOfMeasureId": "a7ec16cb-014f-446a-a084-bc31d5d7e742",
    "lastModfiedDateTime": "2019-03-25T14:26:55.027Z",
    "unitOfMeasure": {
        "code": "HOUR",
        "displayName": "Hour",
        "symbol": null,
        "unitConversion": null
    }
}
```

## Related information
[Tips for working with the APIs](../../../developer/devenv-connect-apps-tips.md)  
[Error Codes](../dynamics_error_codes.md)  
[timeRegistrationEntries](../resources/dynamics_timeRegistrationEntry.md)  
[Get timeRegistrationEntries](../api/dynamics_timeRegistrationEntry_get.md)  
[Post timeRegistrationEntries](../api/dynamics_timeRegistrationEntry_create.md)  
[Patch timeRegistrationEntries](../api/dynamics_timeRegistrationEntry_update.md)  
[Delete timeRegistrationEntries](../api/dynamics_timeRegistrationEntry_delete.md)  
