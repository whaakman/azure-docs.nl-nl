---
title: Een beheerde identiteit toegang toewijzen aan een Azure-resource met behulp van PowerShell
description: Stapsgewijze toegang instructies voor het toewijzen van een beheerde identiteit op een resource, tot een andere bron, met behulp van PowerShell.
services: active-directory
documentationcenter: ''
author: daveba
manager: daveba
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/06/2018
ms.author: daveba
ms.openlocfilehash: 72e05af92e88dc04f470d8be9a65347672777556
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54427658"
---
# <a name="assign-a-managed-identity-access-to-a-resource-using-powershell"></a>Een beheerde identiteit toegang toewijzen aan een resource met behulp van PowerShell

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Wanneer u een Azure-resource hebt geconfigureerd met een beheerde identiteit, kunt u de toegang tot de beheerde identiteit geven naar een andere resource, net als bij elke beveiligings-principal. In dit voorbeeld ziet u hoe u een virtuele machine van Azure van beheerde identiteit toegang geven tot Azure storage-account met behulp van PowerShell.

## <a name="prerequisites"></a>Vereisten

- Als u niet bekend met beheerde identiteiten voor Azure-resources bent, lees de [overzichtssectie](overview.md). **Lees de [verschil tussen een beheerde identiteit door het systeem is toegewezen en de gebruiker toegewezen](overview.md#how-does-it-work)**.
- Als u nog geen Azure-account hebt, [registreer u dan voor een gratis account](https://azure.microsoft.com/free/) voordat u verdergaat.
- Installeer [de meest recente versie van Azure PowerShell](https://www.powershellgallery.com/packages/AzureRM) als u dat nog niet gedaan hebt.

## <a name="use-rbac-to-assign-a-managed-identity-access-to-another-resource"></a>RBAC gebruiken voor het toewijzen van een beheerde identiteit toegang naar een andere resource

Nadat u de beheerde identiteit op een Azure-resource hebt ingeschakeld [, zoals een Azure-VM](qs-configure-powershell-windows-vm.md):

1. Aanmelden bij Azure met de `Connect-AzureRmAccount` cmdlet. Gebruik een account dat is gekoppeld aan het Azure-abonnement waarin u de beheerde identiteit hebt geconfigureerd:

   ```powershell
   Connect-AzureRmAccount
   ```
2. In dit voorbeeld geeft we een Azure VM-toegang om een storage-account. Eerst gebruiken we [Get-AzureRMVM](/powershell/module/azurerm.compute/get-azurermvm) om op te halen van de service-principal voor de virtuele machine met de naam `myVM`, die is gemaakt toen we ingeschakeld beheerde identiteit. Vervolgens gebruikt u [New-AzureRmRoleAssignment](/powershell/module/AzureRM.Resources/New-AzureRmRoleAssignment) te geven van de virtuele machine **lezer** toegang tot een opslagaccount met de naam `myStorageAcct`:

    ```powershell
    $spID = (Get-AzureRMVM -ResourceGroupName myRG -Name myVM).identity.principalid
    New-AzureRmRoleAssignment -ObjectId $spID -RoleDefinitionName "Reader" -Scope "/subscriptions/<mySubscriptionID>/resourceGroups/<myResourceGroup>/providers/Microsoft.Storage/storageAccounts/<myStorageAcct>"
    ```

## <a name="next-steps"></a>Volgende stappen

- [Beheerde identiteit voor Azure-resources-overzicht](overview.md)
- Zie voor het inschakelen van beheerde identiteiten op een Azure VM [configureren beheerde identiteiten voor een Azure-resources op een Azure-VM met behulp van PowerShell](qs-configure-powershell-windows-vm.md).
