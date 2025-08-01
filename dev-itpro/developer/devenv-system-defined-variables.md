---
title: System-defined variables
description: Describes the system-defined variables that are automatically declared and initialized in AL for Business Central.
ms.date: 04/26/2024
ms.update-cycle: 1095-days
ms.reviewer: solsen
ms.topic: article
ms.custom: evergreen
author: SusanneWindfeldPedersen
ms.author: solsen
ms.collection: get-started
---

# System-defined variables

[!INCLUDE [prod_short](includes/prod_short.md)] automatically declares and initializes several variables that you can use when you develop applications. The following table describes the system-defined variables.  

|System-defined variable|[!INCLUDE[bp_tabledescription](includes/bp_tabledescription_md.md)]|  
|------------------------------|---------------------------------------|  
|`Rec`|When a record is modified, this variable specifies the current record, including the changes that are made.|  
|`xRec`|When a record is modified, this variable specifies the original values of the record before the changes.|  
|`CurrPage`|This variable specifies the current page.|  
|`CurrReport`|This variable specifies the current report.|  
|`RequestOptionsPage`|This variable specifies the request options page for the current report.|  
|`CurrFieldNo`|This variable specifies the field number of the current field in the current table. Retained for compatibility reasons.| 

## Using Rec and xRec

The following example shows how to use the `Rec` and `xRec` pair of records.  

In an application, data is stored in two tables; a header table and a line table. The header table contains general information about, for example, sales orders, while the line table contains the specific order lines. The page that you use to enter information into the header table has fields that contain the customer’s address. These fields are related to the **Customer** table, and can be filled by using a lookup method in the field that establishes the relationship. In the header table, only the customer number is stored, and the other fields that have customer information, such as name and address, are retrieved from the **Customer** table when the **Customer No.** field is validated.  

In some situations, the user should be able to change the customer number, and in other situations the user shouldn't be able to change it. For example, if the order has already been shipped, the user shouldn't be able to change the customer number. If there's an incorrect number on an order that hasn't been processed completely, the user should be able to correct the error.  

You can use the `Rec` and `xRec` variables to design your application in the following way:  

- When validating the customer number field, check whether the order has shipped.  
- If the order has shipped, compare the customer number fields in the Rec and xRec records. If they differ, reject the change.

> [!NOTE]
> Avoid modifications to the xRec variable because the record might share some of the underlying state with the Rec variable for performance and compatibility reasons and changes can unexpectedly propagate to the Rec variable.

## Using `CurrPage`

You can access the controls of the page through the `CurrPage` variable and set the dynamic properties of the page and its controls. The `CurrPage.Editable` variable reflects the runtime value of the `Editable` property, which can be changed at design time, programmatically, or by the user when switching view modes on a page. The `CurrPage.Update([SaveRecord])` variable can be used to save the current record and then update the controls on the page. When the View mode on a page is **false**, then the Edit, New, and Delete modes are **true**.

## Using CurrReport

You can access properties of a report through the `CurrReport` variable and set them dynamically. For example, by using `CurrReport.Preview`, you can determine if the report is being run in preview mode.  

## Using RequestOptionsPage

You can access properties of the request page through the RequestOptionsPage variable and set them dynamically. 

## Related information

[AL variables](devenv-al-variables.md)  
[AL method reference](methods-auto/library.md)  
[Properties](properties/devenv-properties.md)  
