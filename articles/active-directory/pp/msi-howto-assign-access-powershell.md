---
title: Een MSI-toegang toewijzen aan een Azure-resource met behulp van PowerShell
description: Stapsgewijze toegang instructies voor het toewijzen van een MSI-bestand op een resource, tot een andere bron, met behulp van PowerShell.
services: active-directory
documentationcenter: 
author: BryanLa
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/15/2017
ms.author: bryanla
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 8b3b000b4dc708e50c7e96fe04646837c96a555a
ms.sourcegitcommit: a648f9d7a502bfbab4cd89c9e25aa03d1a0c412b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/22/2017
---
# <a name="assign-a-managed-service-identity-msi-access-to-a-resource-using-powershell"></a>Een beheerde Service identiteit (MSI) toegang toewijzen aan een resource met behulp van PowerShell

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Wanneer u een Azure-resource hebt geconfigureerd met een MSI-bestand, kunt u de MSI-toegang geven tot een andere resource, net als elke beveiligings-principal. In dit voorbeeld ziet u hoe een virtuele machine van Azure van MSI toegang te verlenen tot een Azure storage-account met behulp van PowerShell.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

Ook installeren [Azure PowerShell versie 4.3.1](https://www.powershellgallery.com/packages/AzureRM/4.3.1) als u dat nog niet gedaan hebt.

## <a name="use-rbac-to-assign-the-msi-access-to-another-resource"></a>Gebruikmaken van RBAC de MSI-toegang toewijzen aan een andere bron

Nadat u MSI hebt ingeschakeld op een Azure-resource [zoals een Azure VM](msi-qs-configure-powershell-windows-vm.md):

1. Aanmelden bij Azure worden verkregen met de `Login-AzureRmAccount` cmdlet. Gebruik een account dat is gekoppeld aan het Azure-abonnement waaronder u het MSI-bestand hebt geconfigureerd:

   ```powershell
   Login-AzureRmAccount
   ```
2. In dit voorbeeld geeft wordt een virtuele machine van Azure toegang tot een opslagaccount. Eerst gebruiken we [Get-AzureRMVM](/powershell/module/azurerm.compute/get-azurermvm) ophalen van de service-principal voor de virtuele machine met de naam 'myVM', is gemaakt toen we MSI ingeschakeld. Vervolgens gebruiken we [New AzureRmRoleAssignment](/powershell/module/AzureRM.Resources/New-AzureRmRoleAssignment) geven van de virtuele machine 'Lezer' toegang tot een opslagaccount 'myStorageAcct' genoemd:

    ```powershell
    $spID = (Get-AzureRMVM -ResourceGroupName myRG -Name myVM).identity.principalid
    New-AzureRmRoleAssignment -ObjectId $spID -RoleDefinitionName "Reader" -Scope "/subscriptions/<mySubscriptionID>/resourceGroups/<myResourceGroup>/providers/Microsoft.Storage/storageAccounts/<myStorageAcct>"
    ```

## <a name="troubleshooting"></a>Problemen oplossen

Als het MSI-bestand voor de resource niet in de lijst met beschikbare identiteiten weergegeven wordt, moet u controleren of het MSI-bestand juist is ingeschakeld. In ons geval we kunt teruggaan naar de Azure VM in de [Azure-portal](https://portal.azure.com) en:

- Bekijk de pagina 'Configuration' en zorg ervoor dat MSI ingeschakeld = "Yes".
- Bekijk de pagina 'Extensies' en zorg ervoor dat de MSI-extensie die is geïmplementeerd.

Als een onjuist is, moet u wellicht de MSI van uw resources opnieuw te implementeren of problemen met de implementatie is mislukt.

## <a name="related-content"></a>Gerelateerde inhoud

- Zie voor een overzicht van MSI [overzicht van de Service-identiteit beheerd](msi-overview.md).
- Zie voor het inschakelen van MSI op een Azure VM [configureren van een Azure VM beheerde Service identiteit (MSI) met behulp van PowerShell](msi-qs-configure-powershell-windows-vm.md).

Gebruik de volgende sectie met opmerkingen uw feedback en help ons verfijnen en onze content vorm.

