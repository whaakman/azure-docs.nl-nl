---
title: Een beheerde identiteit toegang toewijzen aan een Azure-resource met behulp van PowerShell
description: Stapsgewijze toegang instructies voor het toewijzen van een beheerde identiteit op een resource, tot een andere bron, met behulp van PowerShell.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/06/2018
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: ff36be7f87d0dd9e5cac5ee7f788eec0cda5a9fd
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60290682"
---
# <a name="assign-a-managed-identity-access-to-a-resource-using-powershell"></a>Een beheerde identiteit toegang toewijzen aan een resource met behulp van PowerShell

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Wanneer u een Azure-resource hebt geconfigureerd met een beheerde identiteit, kunt u de toegang tot de beheerde identiteit geven naar een andere resource, net als bij elke beveiligings-principal. In dit voorbeeld ziet u hoe u een virtuele machine van Azure van beheerde identiteit toegang geven tot Azure storage-account met behulp van PowerShell.

[!INCLUDE [az-powershell-update](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Vereisten

- Als u niet bekend met beheerde identiteiten voor Azure-resources bent, lees de [overzichtssectie](overview.md). **Lees de [verschil tussen een beheerde identiteit door het systeem is toegewezen en de gebruiker toegewezen](overview.md#how-does-it-work)** .
- Als u nog geen Azure-account hebt, [registreer u dan voor een gratis account](https://azure.microsoft.com/free/) voordat u verdergaat.
- Installeer [de meest recente versie van Azure PowerShell](/powershell/azure/install-az-ps) als u dat nog niet gedaan hebt.

## <a name="use-rbac-to-assign-a-managed-identity-access-to-another-resource"></a>RBAC gebruiken voor het toewijzen van een beheerde identiteit toegang naar een andere resource

Nadat u de beheerde identiteit op een Azure-resource hebt ingeschakeld [, zoals een Azure-VM](qs-configure-powershell-windows-vm.md):

1. Aanmelden bij Azure met de `Connect-AzAccount` cmdlet. Gebruik een account dat is gekoppeld aan het Azure-abonnement waarin u de beheerde identiteit hebt geconfigureerd:

   ```powershell
   Connect-AzAccount
   ```
2. In dit voorbeeld geeft we een Azure VM-toegang om een storage-account. Eerst gebruiken we [Get-AzVM](/powershell/module/az.compute/get-azvm) om op te halen van de service-principal voor de virtuele machine met de naam `myVM`, die is gemaakt toen we ingeschakeld beheerde identiteit. Vervolgens gebruikt u [New-AzRoleAssignment](/powershell/module/Az.Resources/New-AzRoleAssignment) te geven van de virtuele machine **lezer** toegang tot een opslagaccount met de naam `myStorageAcct`:

    ```powershell
    $spID = (Get-AzVM -ResourceGroupName myRG -Name myVM).identity.principalid
    New-AzRoleAssignment -ObjectId $spID -RoleDefinitionName "Reader" -Scope "/subscriptions/<mySubscriptionID>/resourceGroups/<myResourceGroup>/providers/Microsoft.Storage/storageAccounts/<myStorageAcct>"
    ```

## <a name="next-steps"></a>Volgende stappen

- [Beheerde identiteit voor Azure-resources-overzicht](overview.md)
- Zie voor het inschakelen van beheerde identiteiten op een Azure VM [configureren beheerde identiteiten voor een Azure-resources op een Azure-VM met behulp van PowerShell](qs-configure-powershell-windows-vm.md).
