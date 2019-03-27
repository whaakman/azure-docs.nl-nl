---
title: Azure PowerShell gebruiken voor het beheren van Azure AD-toegangsrechten tot blob- en wachtrijservices gegevens met RBAC - Azure Storage
description: Azure PowerShell gebruiken voor toegang tot containers en wachtrijen met op rollen gebaseerd toegangsbeheer (RBAC) toewijzen. Azure Storage biedt ondersteuning voor ingebouwde en aangepaste RBAC-rollen voor verificatie via Azure AD.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 03/21/2019
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: f14c6625a36356a6882e1596db13c1749a9a292a
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58449967"
---
# <a name="grant-access-to-azure-blob-and-queue-data-with-rbac-using-powershell"></a>Toegang verlenen tot Azure blob- en wachtrijservices gegevens met RBAC met behulp van PowerShell

Azure Active Directory (Azure AD) machtigt toegangsrechten tot beveiligde bronnen via [op rollen gebaseerd toegangsbeheer (RBAC)](../../role-based-access-control/overview.md). Azure Storage definieert een aantal ingebouwde RBAC-rollen die algemene sets machtigingen die wordt gebruikt voor toegang tot containers of wachtrijen omvatten. 

Wanneer een RBAC-rol is toegewezen aan een beveiligings-principal voor Azure AD, wordt de status van Azure verleent toegang tot deze resources voor deze beveiligings-principal. Toegang kan worden gericht op het niveau van het abonnement, de resourcegroep, de storage-account of een afzonderlijke container of de wachtrij. Een beveiligings-principal voor Azure AD kan een gebruiker, een groep, een service-principal van toepassing zijn of een [beheerde identiteit voor de Azure-resources](../../active-directory/managed-identities-azure-resources/overview.md).

In dit artikel wordt beschreven hoe u Azure PowerShell gebruiken om te lijst van ingebouwde RBAC-rollen en deze toewijzen aan gebruikers. Zie voor meer informatie over het gebruik van Azure PowerShell [overzicht van Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="rbac-roles-for-blobs-and-queues"></a>RBAC-rollen voor blobs en wachtrijen

[!INCLUDE [storage-auth-rbac-roles-include](../../../includes/storage-auth-rbac-roles-include.md)]

## <a name="determine-resource-scope"></a>Resource-bereik bepalen 

[!INCLUDE [storage-auth-resource-scope-include](../../../includes/storage-auth-resource-scope-include.md)]

## <a name="list-available-rbac-roles"></a>Lijst met beschikbare RBAC-rollen

Als u beschikbare ingebouwde RBAC-rollen met Azure PowerShell, gebruikt u de [Get-AzRoleDefinition](/powershell/module/az.resources/get-azroledefinition) opdracht:

```powershell
Get-AzRoleDefinition | FT Name, Description
```

Hier ziet u de ingebouwde Azure Storage-gegevens rollen weergegeven, samen met andere ingebouwde rollen voor Azure:

```Example
Storage Blob Data Contributor             Allows for read, write and delete access to Azure Storage blob containers and data
Storage Blob Data Owner                   Allows for full access to Azure Storage blob containers and data, including assigning POSIX access control.
Storage Blob Data Reader                  Allows for read access to Azure Storage blob containers and data
Storage Queue Data Contributor            Allows for read, write, and delete access to Azure Storage queues and queue messages
Storage Queue Data Message Processor      Allows for peek, receive, and delete access to Azure Storage queue messages
Storage Queue Data Message Sender         Allows for sending of Azure Storage queue messages
Storage Queue Data Reader                 Allows for read access to Azure Storage queues and queue messages
```

## <a name="assign-an-rbac-role-to-a-user"></a>Een RBAC-rol toewijzen aan een gebruiker

Als u wilt een RBAC-rol toewijzen aan een gebruiker, gebruikt u de [New-AzRoleAssignment](/powershell/module/az.resources/new-azroleassignment) opdracht. De indeling van de opdracht kan verschillen op basis van het bereik van de toewijzing. De volgende voorbeelden laten zien hoe een rol toewijzen aan een gebruiker op verschillende bereiken.

### <a name="container-scope"></a>Bereik van de container

Als u wilt toewijzen van een rol binnen het bereik van een container, Geef een tekenreeks met het bereik van de container voor de `--scope` parameter. Het bereik voor een container heeft de notatie:

```
/subscriptions/<subscription>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>/blobServices/default/containers/<container-name>
```

In het volgende voorbeeld wordt de **Gegevensbijdrager voor Blob** rol voor een gebruiker, binnen het bereik van een container met de naam *voorbeeldcontainer*. Zorg ervoor dat u de voorbeeldwaarden en de tijdelijke aanduiding voor waarden tussen vierkante haken vervangen door uw eigen waarden: 

```powershell
New-AzRoleAssignment -SignInName <email> `
    -RoleDefinitionName "Storage Blob Data Contributor" `
    -Scope  "/subscriptions/<subscription>/resourceGroups/sample-resource-group/providers/Microsoft.Storage/storageAccounts/<storage-account>/blobServices/default/containers/sample-container"
```

### <a name="queue-scope"></a>Wachtrijbereik

Als u wilt toewijzen van een rol binnen het bereik van een wachtrij, Geef een tekenreeks met het bereik van de wachtrij voor de `--scope` parameter. Het bereik voor een wachtrij is in het formulier:

```
/subscriptions/<subscription>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>/queueServices/default/queues/<queue-name>
```

In het volgende voorbeeld wordt de **Gegevensbijdrager voor wachtrij** rol voor een gebruiker, binnen het bereik van een wachtrij met de naam *voorbeeld-wachtrij*. Zorg ervoor dat u de voorbeeldwaarden en de tijdelijke aanduiding voor waarden tussen vierkante haken vervangen door uw eigen waarden: 

```powershell
New-AzRoleAssignment -SignInName <email> `
    -RoleDefinitionName "Storage Queue Data Contributor" `
    -Scope  "/subscriptions/<subscription>/resourceGroups/sample-resource-group/providers/Microsoft.Storage/storageAccounts/<storage-account>/queueServices/default/queues/sample-queue"
```

### <a name="storage-account-scope"></a>Storage-accountbereik

Als u wilt toewijzen van een rol binnen het bereik van de storage-account, geef het bereik van de resource van het opslagaccount voor de `--scope` parameter. Het bereik voor een storage-account is in het formulier:

```
/subscriptions/<subscription>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>
```

Het volgende voorbeeld wordt het bereik de **gegevenslezer voor Opslagblob** rol aan een gebruiker op het niveau van het storage-account. Zorg ervoor dat u de voorbeeldwaarden door uw eigen waarden vervangt: 

```powershell
New-AzRoleAssignment -SignInName <email> `
    -RoleDefinitionName "Storage Blob Data Reader" `
    -Scope  "/subscriptions/<subscription>/resourceGroups/sample-resource-group/providers/Microsoft.Storage/storageAccounts/<storage-account>"
```

### <a name="resource-group-scope"></a>Groepsbereik van de resource

Als u wilt toewijzen van een rol binnen het bereik van de resourcegroep, geef de naam van de resourcegroep of de ID voor de `--resource-group` parameter. In het volgende voorbeeld wordt de **gegevenslezer voor Opslagwachtrij** rol aan een gebruiker op het niveau van de resourcegroep. Zorg ervoor dat u de voorbeeldwaarden en de tijdelijke aanduiding voor waarden tussen vierkante haken vervangen door uw eigen waarden: 

```powershell
New-AzRoleAssignment -SignInName <email> `
    -RoleDefinitionName "Storage Queue Data Reader" `
    -ResourceGroupName "sample-resource-group"
```

### <a name="subscription-scope"></a>Bereik van abonnement

Als u wilt toewijzen van een rol binnen het bereik van het abonnement, het bereik opgeven voor het abonnement voor de `--scope` parameter. Het bereik voor een abonnement heeft de notatie:

```
/subscriptions/<subscription>
```

Het volgende voorbeeld laat zien hoe om toe te wijzen de **gegevenslezer voor Opslagblob** rol aan een gebruiker op het niveau van het storage-account. Zorg ervoor dat u de voorbeeldwaarden door uw eigen waarden vervangt: 

```powershell
New-AzRoleAssignment -SignInName <email> `
    -RoleDefinitionName "Storage Blob Data Reader" `
    -Scope  "/subscriptions/<subscription>"
```

## <a name="next-steps"></a>Volgende stappen

- [Toegang tot Azure-resources beheren met behulp van RBAC en Azure PowerShell](../../role-based-access-control/role-assignments-powershell.md)
- [Toegang verlenen tot Azure blob- en wachtrijservices gegevens met RBAC met behulp van Azure CLI](storage-auth-aad-rbac-cli.md)
- [Toegang verlenen tot Azure blob- en wachtrijservices gegevens met RBAC in Azure portal](storage-auth-aad-rbac-portal.md)
