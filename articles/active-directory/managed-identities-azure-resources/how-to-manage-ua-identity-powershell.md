---
title: Het maken, weergeven en verwijderen van een gebruiker toegewezen beheerde identiteit met Azure PowerShell
description: Stap voor stap beheerde instructies voor het maken, weergeven en verwijderen door de gebruiker toegewezen identiteit met Azure PowerShell.
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
ms.openlocfilehash: c7191f60b8780e8ccee9b330aa21d8174f0f0148
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47106842"
---
# <a name="create-list-or-delete-a-user-assigned-managed-identity-using-azure-powershell"></a>Maken, weergeven of verwijderen van een gebruiker toegewezen beheerde identiteit met Azure PowerShell

[!INCLUDE [preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Beheerde identiteiten voor Azure-resources biedt Azure-services met een beheerde identiteit in Azure Active Directory. U kunt deze identiteit gebruiken voor verificatie bij services die ondersteuning bieden voor Azure AD-verificatie, zonder de referenties in uw code. 

In dit artikel leert u hoe u kunt maken, weergeven en verwijderen van een gebruiker toegewezen beheerde identiteit met Azure PowerShell.

## <a name="prerequisites"></a>Vereisten

- Als u niet bekend met beheerde identiteiten voor Azure-resources bent, lees de [overzichtssectie](overview.md). **Lees de [verschil tussen een beheerde identiteit door het systeem is toegewezen en de gebruiker toegewezen](overview.md#how-does-it-work)**.
- Als u nog geen Azure-account hebt, [registreer u dan voor een gratis account](https://azure.microsoft.com/free/) voordat u verdergaat.
- Installeer [de meest recente versie van Azure PowerShell](https://www.powershellgallery.com/packages/AzureRM) als u dat nog niet gedaan hebt.
- Als u ervoor kiest om PowerShell lokaal te installeren en gebruiken, is voor deze zelfstudie versie 5.7.0 of hoger van de Azure PowerShell-module vereist. Voer ` Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps). 
- Als u PowerShell lokaal uitvoert, moet u ook het volgende doen: 
    - Voer `Login-AzureRmAccount` uit om een verbinding op te zetten met Azure.
    - Installeer de [nieuwste versie van PowerShellGet](/powershell/gallery/installing-psget#for-systems-with-powershell-50-or-newer-you-can-install-the-latest-powershellget).
    - Voer `Install-Module -Name PowerShellGet -AllowPrerelease` uit om de voorlopige versie van de `PowerShellGet`-module op te halen (mogelijk moet u met `Exit` de huidige PowerShell-sessie afsluiten nadat u met deze opdracht de `AzureRM.ManagedServiceIdentity`-module hebt geïnstalleerd).
    - Voer `Install-Module -Name AzureRM.ManagedServiceIdentity -AllowPrerelease` voor het installeren van de voorlopige versie van de `AzureRM.ManagedServiceIdentity` module om uit te voeren van de gebruiker toegewezen identiteit bewerkingen in dit artikel worden beheerd.
- Als u wilt de beheerbewerkingen in dit artikel uitvoert, moet uw account de volgende roltoewijzingen:
    - [Beheerde identiteit Inzender](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) (lijst) rol te maken, lezen, bijwerken en verwijderen van een gebruiker toegewezen beheerde identiteit.
    - [Beheerde identiteit Operator](/azure/role-based-access-control/built-in-roles#managed-identity-operator) rol te lezen (lijst) de eigenschappen van een gebruiker toegewezen beheerde identiteit.

## <a name="create-a-user-assigned-managed-identity"></a>Een door de gebruiker toegewezen beheerde identiteit maken

Gebruik voor het maken van een gebruiker toegewezen beheerde identiteit het [New-AzureRmUserAssignedIdentity](/powershell/module/azurerm.managedserviceidentity/new-azurermuserassignedidentity) opdracht. De `ResourceGroupName` parameter geeft u de resourcegroep waar moet worden gemaakt van de gebruiker toegewezen beheerde identiteit, en de `-Name` parameter geeft u de naam ervan. Vervang de `<RESOURCE GROUP>` en `<USER ASSIGNED IDENTITY NAME>` parameterwaarden door uw eigen waarden:

[!INCLUDE [ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

 ```azurepowershell-interactive
New-AzureRmUserAssignedIdentity -ResourceGroupName <RESOURCEGROUP> -Name <USER ASSIGNED IDENTITY NAME>
```
## <a name="list-user-assigned-managed-identities"></a>Lijst met door de gebruiker toegewezen identiteiten beheren

Aan de lijst van beheerde identiteiten gebruiker toegewezen, gebruikt u de [Get-AzureRmUserAssigned](/powershell/module/azurerm.managedserviceidentity/get-azurermuserassignedidentity) opdracht.  De `-ResourceGroupName` parameter geeft u de resourcegroep waarin de gebruiker toegewezen beheerde identiteit is gemaakt. Vervang de `<RESOURCE GROUP>` door uw eigen waarde:

```azurepowershell-interactive
Get-AzureRmUserAssignedIdentity -ResourceGroupName <RESOURCE GROUP>
```
In het antwoord door de gebruiker toegewezen beheerde identiteiten hebben `"Microsoft.ManagedIdentity/userAssignedIdentities"` waarde geretourneerd voor de sleutel, `Type`.

`Type :Microsoft.ManagedIdentity/userAssignedIdentities`

## <a name="delete-a-user-assigned-managed-identity"></a>Verwijderen van een gebruiker toegewezen beheerde identiteit

Als u wilt verwijderen van een gebruiker toegewezen beheerde identiteit, gebruikt u de [Remove-AzureRmUserAssignedIdentity](/powershell/module/azurerm.managedserviceidentity/remove-azurermuserassignedidentity) opdracht.  De `-ResourceGroupName` parameter geeft u de resourcegroep waarin de gebruiker toegewezen identiteit is gemaakt en de `-Name` parameter geeft u de naam ervan. Vervang de `<RESOURCE GROUP>` en de `<USER ASSIGNED IDENTITY NAME>` parameterwaarden door uw eigen waarden:

 ```azurepowershell-interactive
Remove-AzurRmUserAssignedIdentity -ResourceGroupName <RESOURCE GROUP> -Name <USER ASSIGNED IDENTITY NAME>
```
> [!NOTE]
> Verwijderen van een gebruiker toegewezen beheerde identiteit, wordt de verwijzing van een willekeurige resource die deze is toegewezen aan niet verwijderen. Toewijzingen van de identiteit moeten afzonderlijk worden verwijderd.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer details van de Azure PowerShell en een volledige lijst met beheerde identiteiten voor opdrachten van Azure-resources, [AzureRM.ManagedServiceIdentity](/powershell/module/azurerm.managedserviceidentity#managed_service_identity).
