---
title: Een MSI toegang toewijzen aan een Azure-resource, met behulp van PowerShell
description: Stapsgewijze toegang instructies voor het toewijzen van een MSI-bestand op een resource, tot een andere bron, met behulp van PowerShell.
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
ms.date: 09/14/2017
ms.author: daveba
ms.openlocfilehash: 6d503526ed97dd1f61269acd83810cd44598d72f
ms.sourcegitcommit: d211f1d24c669b459a3910761b5cacb4b4f46ac9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/06/2018
ms.locfileid: "44028150"
---
# <a name="assign-a-managed-service-identity-msi-access-to-a-resource-using-powershell"></a>Een Managed Service Identity (MSI) toegang toewijzen aan een resource met behulp van PowerShell

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Wanneer u een Azure-resource hebt geconfigureerd met een MSI-bestand, kunt u het MSI-toegang geven tot een andere resource, net als bij alle beveiligings-principal. In dit voorbeeld ziet u hoe u een virtuele machine van Azure van MSI toegang geven tot Azure storage-account, met behulp van PowerShell.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

Ook installeren [Azure PowerShell versie 4.3.1](https://www.powershellgallery.com/packages/AzureRM/4.3.1) als u dat nog niet gedaan hebt.

## <a name="use-rbac-to-assign-the-msi-access-to-another-resource"></a>RBAC gebruiken voor het toewijzen van de toegang tot de MSI-bestand naar een andere resource

Nadat u MSI-bestand hebt ingeschakeld op een Azure-resource, [, zoals een Azure-VM](qs-configure-powershell-windows-vm.md):

1. Aanmelden bij Azure met de `Connect-AzureRmAccount` cmdlet. Gebruik een account dat is gekoppeld aan het Azure-abonnement waarin u het MSI-bestand hebt geconfigureerd:

   ```powershell
   Connect-AzureRmAccount
   ```
2. In dit voorbeeld geeft we een Azure VM-toegang om een storage-account. Eerst gebruiken we [Get-AzureRMVM](/powershell/module/azurerm.compute/get-azurermvm) om op te halen van de service-principal voor de virtuele machine met de naam 'myVM', die is gemaakt toen we MSI ingeschakeld. Vervolgens gebruiken we [New-AzureRmRoleAssignment](/powershell/module/AzureRM.Resources/New-AzureRmRoleAssignment) te geven van de virtuele machine 'Lezer' toegang tot een opslagaccount met de naam 'myStorageAcct':

    ```powershell
    $spID = (Get-AzureRMVM -ResourceGroupName myRG -Name myVM).identity.principalid
    New-AzureRmRoleAssignment -ObjectId $spID -RoleDefinitionName "Reader" -Scope "/subscriptions/<mySubscriptionID>/resourceGroups/<myResourceGroup>/providers/Microsoft.Storage/storageAccounts/<myStorageAcct>"
    ```

## <a name="troubleshooting"></a>Problemen oplossen

Als het MSI-bestand voor de resource niet in de lijst met beschikbare identiteiten weergegeven wordt, controleert u of het MSI-bestand correct is ingeschakeld. In ons geval we teruggaan naar de Azure-VM in de [Azure-portal](https://portal.azure.com) en:

- Kijken naar de pagina "Configuratie" en zorg ervoor dat MSI ingeschakeld = "Yes".
- Kijken naar de pagina 'Uitbreidingen' en zorg ervoor dat de MSI-extensie is geïmplementeerd.

Als een onjuist is, moet u mogelijk het MSI-bestand voor uw resource opnieuw implementeren of problemen met de implementatie mislukt.

## <a name="related-content"></a>Gerelateerde inhoud

- Zie voor een overzicht van MSI [overzicht van de beheerde Service-identiteit](overview.md).
- Zie voor het inschakelen van MSI-bestand op een Azure VM [configureren van een Azure VM Managed Service Identity (MSI) met behulp van PowerShell](qs-configure-powershell-windows-vm.md).

Gebruik de volgende sectie met opmerkingen uw feedback en help ons verfijnen en vorm van onze inhoud.

