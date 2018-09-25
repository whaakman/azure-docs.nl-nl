---
title: Een MSI toegang toewijzen aan een Azure-resource, met behulp van Azure CLI
description: Stapsgewijze toegang instructies voor het toewijzen van een MSI-bestand op een resource, tot een andere bron, met behulp van Azure CLI.
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
ms.date: 09/25/2017
ms.author: daveba
ms.openlocfilehash: 2e3b85251b9dabd6efd23e5b41372703a237d227
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46949074"
---
# <a name="assign-a-managed-service-identity-msi-access-to-a-resource-using-azure-cli"></a>Een Managed Service Identity (MSI) toegang toewijzen aan een resource met behulp van Azure CLI

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Wanneer u een Azure-resource hebt geconfigureerd met een MSI-bestand, kunt u het MSI-toegang geven tot een andere resource, net als bij alle beveiligings-principal. In dit voorbeeld ziet u hoe u een Azure-machine of VM-schaalset van MSI toegang geven tot Azure storage-account, met behulp van Azure CLI.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

Als u wilt de CLI-scriptvoorbeelden uitvoeren, hebt u drie opties:

- Gebruik [Azure Cloud Shell](../../cloud-shell/overview.md) vanuit Azure portal (Zie volgende sectie).
- Gebruik de ingesloten Azure Cloud Shell via het 'Try It' de knop, zich in de rechterbovenhoek van elk codeblok.
- [Installeer de nieuwste versie van Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) als u liever een lokale CLI-console. 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="use-rbac-to-assign-the-msi-access-to-another-resource"></a>RBAC gebruiken voor het toewijzen van de toegang tot de MSI-bestand naar een andere resource

Nadat u hebt MSI ingeschakeld op een Azure-resource, zoals een [virtuele Azure-machine](qs-configure-cli-windows-vm.md) of [Azure virtuele-machineschaalset](qs-configure-cli-windows-vmss.md): 

1. Als u de Azure CLI in een lokale console gebruikt, meldt u zich eerst aan bij Azure met [az login](/cli/azure/reference-index#az-login). Gebruik een account dat is gekoppeld aan het Azure-abonnement waarin u wilt implementeren van de virtuele machine of VM-schaalset:

   ```azurecli-interactive
   az login
   ```

2. In dit voorbeeld zijn we geeft u de toegang van een virtuele machine van Azure naar een opslagaccount. Eerst gebruiken we [az resource list](/cli/azure/resource/#az-resource-list) om op te halen van de service-principal voor de virtuele machine met de naam 'myVM':

   ```azurecli-interactive
   spID=$(az resource list -n myVM --query [*].identity.principalId --out tsv)
   ```
   Een schaalset voor virtuele Azure-machine, de opdracht is hetzelfde, behalve hier, krijgt u de service-principal voor de virtuele-machineschaalset met de naam 'DevTestVMSS':
   
   ```azurecli-interactive
   spID=$(az resource list -n DevTestVMSS --query [*].identity.principalId --out tsv)
   ```

3. Zodra u de service-principal-ID hebt, gebruikt u [az roltoewijzing maken](/cli/azure/role/assignment#az-role-assignment-create) instellen om te geven van de virtuele machine of virtuele-machineschaalset 'Lezer' toegang tot een opslagaccount met de naam 'myStorageAcct':

   ```azurecli-interactive
   az role assignment create --assignee $spID --role 'Reader' --scope /subscriptions/<mySubscriptionID>/resourceGroups/<myResourceGroup>/providers/Microsoft.Storage/storageAccounts/myStorageAcct
   ```

## <a name="troubleshooting"></a>Problemen oplossen

Als het MSI-bestand voor de resource niet in de lijst met beschikbare identiteiten weergegeven wordt, controleert u of het MSI-bestand correct is ingeschakeld. In ons geval we terugkeren naar de Azure-machine of virtuele-machineschaalset de [Azure-portal](https://portal.azure.com) en:

- Kijken naar de pagina "Configuratie" en zorg ervoor dat MSI ingeschakeld = "Yes".
- Kijken naar de pagina 'Uitbreidingen' en zorg ervoor dat de MSI-extensie geïmplementeerd (**extensies** pagina is niet beschikbaar voor een schaalset voor virtuele machine van Azure).

Als een onjuist is, moet u mogelijk het MSI-bestand voor uw resource opnieuw implementeren of problemen met de implementatie mislukt.

## <a name="related-content"></a>Gerelateerde inhoud

- Zie voor een overzicht van MSI [overzicht van de beheerde Service-identiteit](overview.md).
- Zie voor meer informatie over het inschakelen van MSI-bestand op een Azure-machine [configureren van een Azure VM Managed Service Identity (MSI) met behulp van Azure CLI](qs-configure-cli-windows-vm.md).
- Zie voor meer informatie over het inschakelen van MSI in een schaalset voor virtuele Azure-machine [configureren van een Azure Virtual Machine Scale ingesteld Managed Service Identity (MSI) met behulp van de Azure portal](qs-configure-portal-windows-vmss.md)

Gebruik de volgende sectie met opmerkingen uw feedback en help ons verfijnen en vorm van onze inhoud.

