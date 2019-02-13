---
title: Het maken, weergeven en verwijderen van een gebruiker toegewezen beheerde identiteit met Azure PowerShell
description: Stap voor stap beheerde instructies voor het maken, weergeven en verwijderen door de gebruiker toegewezen identiteit met Azure PowerShell.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/16/2018
ms.author: priyamo
ms.collection: M365-identity-device-management
ms.openlocfilehash: a0d1fee9526c308f025abaf1ded1f02ee0617b2f
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56202572"
---
# <a name="create-list-or-delete-a-user-assigned-managed-identity-using-azure-powershell"></a>Maken, weergeven of verwijderen van een gebruiker toegewezen beheerde identiteit met Azure PowerShell

[!INCLUDE [preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Beheerde identiteiten voor Azure-resources biedt Azure-services met een beheerde identiteit in Azure Active Directory. U kunt deze identiteit gebruiken voor verificatie bij services die ondersteuning bieden voor Azure AD-verificatie, zonder de referenties in uw code. 

In dit artikel leert u hoe u kunt maken, weergeven en verwijderen van een gebruiker toegewezen beheerde identiteit met Azure PowerShell.

[!INCLUDE [az-powershell-update](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Vereisten

- Als u niet bekend met beheerde identiteiten voor Azure-resources bent, lees de [overzichtssectie](overview.md). **Lees de [verschil tussen een beheerde identiteit door het systeem is toegewezen en de gebruiker toegewezen](overview.md#how-does-it-work)**.
- Als u nog geen Azure-account hebt, [registreer u dan voor een gratis account](https://azure.microsoft.com/free/) voordat u verdergaat.
- Installeer [de meest recente versie van Azure PowerShell](/powershell/azure/install-az-ps) als u dat nog niet gedaan hebt.
- Als u PowerShell lokaal uitvoert, moet u ook het volgende doen: 
    - Voer `Connect-AzAccount` uit om een verbinding op te zetten met Azure.
    - Installeer de [nieuwste versie van PowerShellGet](/powershell/gallery/installing-psget#for-systems-with-powershell-50-or-newer-you-can-install-the-latest-powershellget).
    - Voer `Install-Module -Name PowerShellGet -AllowPrerelease` uit om de voorlopige versie van de `PowerShellGet`-module op te halen (mogelijk moet u met `Exit` de huidige PowerShell-sessie afsluiten nadat u met deze opdracht de `Az.ManagedServiceIdentity`-module hebt geïnstalleerd).
    - Voer `Install-Module -Name Az.ManagedServiceIdentity -AllowPrerelease` voor het installeren van de voorlopige versie van de `Az.ManagedServiceIdentity` module om uit te voeren van de gebruiker toegewezen identiteit bewerkingen in dit artikel worden beheerd.

## <a name="create-a-user-assigned-managed-identity"></a>Een door de gebruiker toegewezen beheerde identiteit maken

Voor het maken van een gebruiker toegewezen beheerde identiteit, uw account moet de [beheerde identiteit Inzender](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) roltoewijzing.

Gebruik voor het maken van een gebruiker toegewezen beheerde identiteit het `New-AzUserAssignedIdentity` opdracht. De `ResourceGroupName` parameter geeft u de resourcegroep waar moet worden gemaakt van de gebruiker toegewezen beheerde identiteit, en de `-Name` parameter geeft u de naam ervan. Vervang de `<RESOURCE GROUP>` en `<USER ASSIGNED IDENTITY NAME>` parameterwaarden door uw eigen waarden:

[!INCLUDE [ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

 ```azurepowershell-interactive
New-AzUserAssignedIdentity -ResourceGroupName <RESOURCEGROUP> -Name <USER ASSIGNED IDENTITY NAME>
```
## <a name="list-user-assigned-managed-identities"></a>Lijst met door de gebruiker toegewezen identiteiten beheren

Als u wilt lezen/lijst met een gebruiker toegewezen beheerde identiteit, uw account moet de [beheerde identiteit Operator](/azure/role-based-access-control/built-in-roles#managed-identity-operator) of [beheerde identiteit Inzender](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) roltoewijzing.

Beheerde lijst door de gebruiker toegewezen identiteiten, gebruik de [Get-AzUserAssigned] opdracht.  De `-ResourceGroupName` parameter geeft u de resourcegroep waarin de gebruiker toegewezen beheerde identiteit is gemaakt. Vervang de `<RESOURCE GROUP>` door uw eigen waarde:

```azurepowershell-interactive
Get-AzUserAssignedIdentity -ResourceGroupName <RESOURCE GROUP>
```
In het antwoord door de gebruiker toegewezen beheerde identiteiten hebben `"Microsoft.ManagedIdentity/userAssignedIdentities"` waarde geretourneerd voor de sleutel, `Type`.

`Type :Microsoft.ManagedIdentity/userAssignedIdentities`

## <a name="delete-a-user-assigned-managed-identity"></a>Verwijderen van een gebruiker toegewezen beheerde identiteit

Als u wilt verwijderen van een gebruiker toegewezen beheerde identiteit, uw account moet de [beheerde identiteit Inzender](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) roltoewijzing.

Als u wilt verwijderen van een gebruiker toegewezen beheerde identiteit, gebruikt u de `Remove-AzUserAssignedIdentity` opdracht.  De `-ResourceGroupName` parameter geeft u de resourcegroep waarin de gebruiker toegewezen identiteit is gemaakt en de `-Name` parameter geeft u de naam ervan. Vervang de `<RESOURCE GROUP>` en de `<USER ASSIGNED IDENTITY NAME>` parameterwaarden door uw eigen waarden:

 ```azurepowershell-interactive
Remove-AzUserAssignedIdentity -ResourceGroupName <RESOURCE GROUP> -Name <USER ASSIGNED IDENTITY NAME>
```
> [!NOTE]
> Verwijderen van een gebruiker toegewezen beheerde identiteit, wordt de verwijzing van een willekeurige resource die deze is toegewezen aan niet verwijderen. Toewijzingen van de identiteit moeten afzonderlijk worden verwijderd.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer details van de Azure PowerShell en een volledige lijst met beheerde identiteiten voor opdrachten van Azure-resources, [Az.ManagedServiceIdentity](/powershell/module/az.managedserviceidentity#managed_service_identity).
