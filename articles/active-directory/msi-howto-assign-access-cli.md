---
title: Een MSI-toegang toewijzen aan een Azure-resource met Azure CLI
description: Stapsgewijze toegang instructies voor het toewijzen van een MSI-bestand op een resource, tot een andere bron, met Azure CLI.
services: active-directory
documentationcenter: 
author: bryanla
manager: mtillman
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/25/2017
ms.author: bryanla
ms.openlocfilehash: 7d817a90277a1320ccc028822032916c98cae4b4
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="assign-a-managed-service-identity-msi-access-to-a-resource-using-azure-cli"></a>Een beheerde Service identiteit (MSI) toegang toewijzen aan een resource met Azure CLI

[!INCLUDE[preview-notice](../../includes/active-directory-msi-preview-notice.md)]

Wanneer u een Azure-resource hebt geconfigureerd met een MSI-bestand, kunt u de MSI-toegang geven tot een andere resource, net als elke beveiligings-principal. In dit voorbeeld ziet u hoe een virtuele machine van Azure van MSI toegang te verlenen tot een Azure storage-account met Azure CLI.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [msi-qs-configure-prereqs](../../includes/active-directory-msi-qs-configure-prereqs.md)]

De CLI script als voorbeelden wilt uitvoeren, hebt u drie opties:

- Gebruik [Azure Cloud Shell](../cloud-shell/overview.md) vanuit de Azure-portal (Zie volgende sectie).
- Gebruik de ingesloten Azure Cloud Shell via de 'Deze probeer' knop, zich in de rechterbovenhoek van elk codeblok.
- [Installeer de nieuwste versie van CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.13 of hoger) als u liever een lokale CLI-console te gebruiken. 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="use-rbac-to-assign-the-msi-access-to-another-resource"></a>Gebruikmaken van RBAC de MSI-toegang toewijzen aan een andere bron

Nadat u MSI hebt ingeschakeld op een Azure-resource [zoals een Azure VM](msi-qs-configure-cli-windows-vm.md): 

1. Als u de Azure CLI in een lokale console, eerst aanmelden bij het gebruik van Azure [az aanmelding](/cli/azure/#login). Gebruik een account dat is gekoppeld aan het Azure-abonnement waarmee u wilt implementeren, de virtuele machine:

   ```azurecli-interactive
   az login
   ```

2. In dit voorbeeld geeft wordt een virtuele machine van Azure toegang tot een opslagaccount. Eerst gebruiken we [az resourcelijst](/cli/azure/resource/#list) ophalen van de service-principal voor de virtuele machine met de naam 'myVM', die is gemaakt bij MSI op de virtuele machine is ingeschakeld:

   ```azurecli-interactive
   spID=$(az resource list -n myVM --query [*].identity.principalId --out tsv)
   ```

3. Wanneer we de service-principal-ID hebt, gebruiken we [az roltoewijzing maken](/cli/azure/role/assignment#az_role_assignment_create) geven van de virtuele machine 'Lezer' toegang tot een opslagaccount 'myStorageAcct' genoemd:

   ```azurecli-interactive
   az role assignment create --assignee $spID --role 'Reader' --scope /subscriptions/<mySubscriptionID>/resourceGroups/<myResourceGroup>/providers/Microsoft.Storage/storageAccounts/myStorageAcct
   ```

## <a name="troubleshooting"></a>Problemen oplossen

Als het MSI-bestand voor de resource niet in de lijst met beschikbare identiteiten weergegeven wordt, moet u controleren of het MSI-bestand juist is ingeschakeld. In ons geval we kunt teruggaan naar de Azure VM in de [Azure-portal](https://portal.azure.com) en:

- Bekijk de pagina 'Configuration' en zorg ervoor dat MSI ingeschakeld = "Yes".
- Bekijk de pagina 'Extensies' en zorg ervoor dat de MSI-extensie die is geïmplementeerd.

Als een onjuist is, moet u wellicht de MSI van uw resources opnieuw te implementeren of problemen met de implementatie is mislukt.

## <a name="related-content"></a>Gerelateerde inhoud

- Zie voor een overzicht van MSI [overzicht van de Service-identiteit beheerd](msi-overview.md).
- Zie voor het inschakelen van MSI op een Azure VM [configureren van een Azure VM beheerde Service identiteit (MSI) met Azure CLI](msi-qs-configure-cli-windows-vm.md).

Gebruik de volgende sectie met opmerkingen uw feedback en help ons verfijnen en onze content vorm.

