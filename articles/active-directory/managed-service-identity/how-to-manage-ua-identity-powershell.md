---
title: Het maken, weergeven en verwijderen van een gebruiker toegewezen (MSI) met behulp van Azure PowerShell
description: Stapsgewijze instructies over het maken, weergeven en verwijderen van gebruiker toegewezen beheerde Service-identiteit met Azure PowerShell.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/16/2018
ms.author: daveba
ms.openlocfilehash: def5788b83116ce0843f1fdd86933830cabc9ee2
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/20/2018
ms.locfileid: "39187993"
---
# <a name="create-list-or-delete-a-user-assigned-identity-using-azure-powershell"></a>Maken, weergeven of verwijderen van een gebruiker toegewezen identiteit met Azure PowerShell

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Beheerde Service-identiteit biedt Azure-services met een beheerde identiteit in Azure Active Directory. U kunt deze identiteit gebruiken voor verificatie bij services die ondersteuning bieden voor Azure AD-verificatie, zonder de referenties in uw code. 

In dit artikel leert u hoe u kunt maken, weergeven en verwijderen van een gebruiker toegewezen identiteit met Azure PowerShell.

## <a name="prerequisites"></a>Vereisten

- Als u niet bekend met beheerde Service-identiteit bent, bekijk dan de [overzichtssectie](overview.md). **Lees de [verschil tussen een systeem toegewezen en een gebruiker toegewezen identiteit](overview.md#how-does-it-work)**.
- Als u nog geen Azure-account hebt, [registreer u dan voor een gratis account](https://azure.microsoft.com/free/) voordat u verdergaat.
- Installeer [de meest recente versie van Azure PowerShell](https://www.powershellgallery.com/packages/AzureRM) als u dat nog niet gedaan hebt.
- Als u wilt installeren en gebruiken van PowerShell lokaal, in deze zelfstudie moduleversie 5.7.0 van Azure PowerShell vereist of hoger. Voer ` Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook `Login-AzureRmAccount` uitvoeren om verbinding te kunnen maken met Azure.
- Als u wilt de beheerbewerkingen in dit artikel uitvoert, moet uw account de volgende roltoewijzingen:
    - [Beheerde identiteit Inzender](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) (lijst) rol te maken, lezen, bijwerken en verwijderen van de identiteit van een gebruiker toegewezen.
    - [Beheerde identiteit Operator](/azure/role-based-access-control/built-in-roles#managed-identity-operator) rol te lezen (lijst) de eigenschappen van de identiteit van een gebruiker toegewezen.

## <a name="create-a-user-assigned-identity"></a>Door de gebruiker toegewezen identiteit maken

Gebruik voor het maken van een gebruiker toegewezen identiteit het [New-AzureRmUserAssignedIdentity](/powershell/module/azurerm.managedserviceidentity/new-azurermuserassignedidentity) opdracht. De `ResourceGroupName` parameter geeft u de resourcegroep waar moet worden gemaakt van de identiteit van de gebruiker toegewezen, en de `-Name` parameter geeft u de naam ervan. Vervang de `<RESOURCE GROUP>` en `<USER ASSIGNED IDENTITY NAME>` parameterwaarden door uw eigen waarden:

[!INCLUDE[ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

 ```azurepowershell-interactive
New-AzureRmUserAssignedIdentity -ResourceGroupName <RESOURCEGROUP> -Name <USER ASSIGNED IDENTITY NAME>
```
## <a name="list-user-assigned-identities"></a>Lijst met door gebruiker toegewezen identiteiten

Aan de lijst van een gebruiker toegewezen identiteiten, gebruikt u de [Get-AzureRmUserAssigned](/powershell/module/azurerm.managedserviceidentity/get-azurermuserassignedidentity) opdracht.  De `-ResourceGroupName` parameter geeft u de resourcegroep waarin de gebruiker toegewezen identiteit is gemaakt. Vervang de `<RESOURCE GROUP>` door uw eigen waarde:

```azurepowershell-interactive
Get-AzureRmUserAssignedIdentity -ResourceGroupName <RESOURCE GROUP>
```
In het antwoord, gebruikers-id's hebben `"Microsoft.ManagedIdentity/userAssignedIdentities"` waarde geretourneerd voor de sleutel, `Type`.

`Type :Microsoft.ManagedIdentity/userAssignedIdentities`

## <a name="delete-a-user-assigned-identity"></a>Verwijderen van een gebruiker toegewezen identiteit

Als u wilt verwijderen een gebruikers-id, gebruikt u de [Remove-AzureRmUserAssignedIdentity](/powershell/module/azurerm.managedserviceidentity/remove-azurermuserassignedidentity) opdracht.  De `-ResourceGroupName` parameter geeft u de resourcegroep waarin de gebruiker toegewezen identiteit is gemaakt en de `-Name` parameter geeft u de naam ervan. Vervang de `<RESOURCE GROUP>` en de `<USER ASSIGNED IDENTITY NAME>` parameterwaarden door uw eigen waarden:

 ```azurecli-interactive
Remove-AzurRmUserAssignedIdentity -ResourceGroupName <RESOURCE GROUP> -Name <USER ASSIGNED IDENTITY NAME>
```
> [!NOTE]
> Verwijderen van een gebruiker toegewezen identiteit, wordt de verwijzing van een willekeurige resource die deze is toegewezen aan niet verwijderen. Toewijzingen van de identiteit moeten afzonderlijk worden verwijderd.

## <a name="related-content"></a>Gerelateerde inhoud

Zie voor een volledige lijst en meer details van de Azure PowerShell-MSI-opdrachten, [AzureRM.ManagedServiceIdentity](/powershell/module/azurerm.managedserviceidentity#managed_service_identity).


 
