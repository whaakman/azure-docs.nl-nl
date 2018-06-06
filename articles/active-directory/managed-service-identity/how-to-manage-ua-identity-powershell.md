---
title: Het maken, weergeven en verwijderen van een gebruiker toegewezen (MSI) met Azure PowerShell
description: Stapsgewijze instructies voor het maken, weergeven en verwijderen van de gebruiker toegewezen beheerde Service-identiteit met Azure PowerShell.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/16/2018
ms.author: daveba
ms.openlocfilehash: d413e5821aff8cf26dfc9ba03e6ec9d4134af76e
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34698941"
---
# <a name="create-list-or-delete-a-user-assigned-identity-using-azure-powershell"></a>Maken, weergeven of verwijderen van de identiteit van een gebruiker aan zijn toegewezen met Azure PowerShell

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Beheerde Service-identiteit biedt Azure-services met een beheerde identiteit in Azure Active Directory. U kunt deze identiteit te verifiëren bij services die ondersteuning bieden voor Azure AD-verificatie, zonder referenties in uw code. 

In dit artikel leert u hoe u kunt maken, weergeven en verwijderen van de identiteit van een gebruiker aan zijn toegewezen met Azure PowerShell.

## <a name="prerequisites"></a>Vereisten

- Als u niet bekend met de Service-identiteit beheerd bent, bekijk de [overzichtssectie van](overview.md). **Lees de [verschil tussen een systeem dat is toegewezen en de gebruiker toegewezen identiteit](overview.md#how-does-it-work)**.
- Als u al een Azure-account niet hebt [aanmelden voor een gratis account](https://azure.microsoft.com/free/) voordat u doorgaat.
- Installeer [de meest recente versie van Azure PowerShell](https://www.powershellgallery.com/packages/AzureRM) als u dat nog niet gedaan hebt.
- Als u wilt installeren en gebruiken van PowerShell lokaal, in deze zelfstudie vereist de Azure PowerShell-moduleversie 5.7.0 of hoger. Voer ` Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook `Login-AzureRmAccount` uitvoeren om verbinding te kunnen maken met Azure.

## <a name="create-a-user-assigned-identity"></a>Een gebruiker toegewezen identiteit maken

Gebruik voor het maken van een toegewezen gebruikers-id de [nieuw AzureRmUserAssignedIdentity](/powershell/module/azurerm.managedserviceidentity/new-azurermuserassignedidentity) opdracht. De `ResourceGroupName` parameter geeft u de resourcegroep waar moet worden gemaakt van de identiteit van de gebruiker die is toegewezen, en de `-Name` parameter geeft u de naam ervan. Vervang de `<RESOURCE GROUP>` en `<USER ASSIGNED IDENTITY NAME>` parameterwaarden met uw eigen waarden:

[!INCLUDE[ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

 ```azurepowershell-interactive
New-AzureRmUserAssignedIdentity -ResourceGroupName <RESOURCEGROUP> -Name <USER ASSIGNED IDENTITY NAME>
```
## <a name="list-user-assigned-identities"></a>Identiteiten toegewezen lijst door gebruiker

Aan de lijst met toegewezen gebruikers-id's, gebruikt u de [Get-AzureRmUserAssigned](/powershell/module/azurerm.managedserviceidentity/get-azurermuserassignedidentity) opdracht.  De `-ResourceGroupName` parameter geeft u de resourcegroep waar de gebruiker toegewezen identiteit is gemaakt.  Vervang de `<RESOURCE GROUP>` met uw eigen waarde:

```azurepowershell-interactive
Get-AzureRmUserAssignedIdentity -ResourceGroupName <RESOURCE GROUP>
```
In het antwoord, gebruikers-id's hebben `"Microsoft.ManagedIdentity/userAssignedIdentities"` waarde geretourneerd voor de sleutel, `Type`.

`Type :Microsoft.ManagedIdentity/userAssignedIdentities`

## <a name="delete-a-user-assigned-identity"></a>Een gebruiker toegewezen identiteit verwijderen

Gebruik voor het verwijderen van de identiteit van een gebruiker de [verwijderen AzureRmUserAssignedIdentity](/powershell/module/azurerm.managedserviceidentity/remove-azurermuserassignedidentity) opdracht.  De `-ResourceGroupName` parameter geeft u de resourcegroep waar de gebruiker toegewezen identiteit is gemaakt en de `-Name` parameter geeft u de naam ervan.  Vervang de `<RESOURCE GROUP>` en de `<USER ASSIGNED IDENTITY NAME>` parameterwaarden door uw eigen waarden:

 ```azurecli-interactive
Remove-AzurRmUserAssignedIdentity -ResourceGroupName <RESOURCE GROUP> -Name <USER ASSIGNED IDENTITY NAME>
```
> [!NOTE]
> Verwijdering van een gebruiker toegewezen identiteit, worden de verwijzing van een bron die deze is toegewezen aan niet verwijderd. Id-toewijzingen moeten afzonderlijk worden verwijderd.

## <a name="related-content"></a>Gerelateerde inhoud

Zie voor een volledige lijst en meer details van de MSI van Azure PowerShell-opdrachten [AzureRM.ManagedServiceIdentity](/powershell/module/azurerm.managedserviceidentity#managed_service_identity).


 
