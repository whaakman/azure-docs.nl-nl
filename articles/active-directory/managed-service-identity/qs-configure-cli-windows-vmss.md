---
title: Configureren van MSI op de schaal van een virtuele machine van Azure met Azure CLI instellen
description: Stap door stapsgewijze instructies voor het configureren van een beheerde Service identiteit (MSI) op een Azure virtuele-Machineschaalset, met Azure CLI.
services: active-directory
documentationcenter: 
author: daveba
manager: mtillman
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/15/2018
ms.author: daveba
ms.openlocfilehash: a64733b1969e2deae665f7a5f24a6653b216d94d
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/08/2018
---
# <a name="configure-a-virtual-machine-scale-set-managed-service-identity-msi-using-azure-cli"></a>Een virtuele machine configureren schaalset beheerde Service identiteit (MSI) met Azure CLI

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Beheerde Service-identiteit biedt Azure-services met een automatisch beheerde identiteit in Azure Active Directory. U kunt deze identiteit gebruiken om alle services die Azure AD-verificatie ondersteunt, zonder referenties in uw code te verifiëren. 

In dit artikel leert u het inschakelen en het verwijderen van MSI voor de schaal van een virtuele machine van Azure met Azure CLI instellen.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

De CLI script als voorbeelden wilt uitvoeren, hebt u drie opties:

- Gebruik [Azure Cloud Shell](../../cloud-shell/overview.md) vanuit de Azure-portal (Zie volgende sectie).
- Gebruik de ingesloten Azure Cloud Shell via de 'Deze probeer' knop, zich in de rechterbovenhoek van elk codeblok.
- [Installeer de nieuwste versie van CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.13 of hoger) als u liever een lokale CLI-console te gebruiken. 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="enable-msi-during-creation-of-an-azure-virtual-machine-scale-set"></a>MSI inschakelen tijdens het maken van een Azure virtuele-machineschaalset

Als u wilt maken van een virtuele machine MSI ingeschakeld schaal instellen:

1. Als u de Azure CLI in een lokale console, eerst aanmelden bij het gebruik van Azure [az aanmelding](/cli/azure/#az_login). Gebruik een account dat is gekoppeld aan het Azure-abonnement waarmee u wilt implementeren, de virtuele-machineschaalset:

   ```azurecli-interactive
   az login
   ```

2. Maak een [resourcegroep](../../azure-resource-manager/resource-group-overview.md#terminology) voor containment en implementatie van uw virtuele-machineschaalset en de bijbehorende resources, met behulp van [az groep maken](/cli/azure/group/#az_group_create). U kunt deze stap overslaan als u al resourcegroep die u wilt gebruiken in plaats daarvan:

   ```azurecli-interactive 
   az group create --name myResourceGroup --location westus
   ```

3. Maken van een virtuele-machineschaalset ingesteld met [az vmss maken](/cli/azure/vmss/#az_vmss_create) . Het volgende voorbeeld wordt een virtuele-machineschaalset set met de naam *myVMSS* met een MSI-bestand, zoals aangevraagd door de `--assign-identity` parameter. De `--admin-username` en `--admin-password` parameters geeft u het account voor gebruiker met beheerdersrechten en het wachtwoord voor aanmelding bij de virtuele machine. Bijwerken van deze waarden naargelang nodig is voor uw omgeving: 

   ```azurecli-interactive 
   az vmss create --resource-group myResourceGroup --name myVMSS --image win2016datacenter --upgrade-policy-mode automatic --custom-data cloud-init.txt --admin-username azureuser --admin-password myPassword12 --assign-identity --generate-ssh-keys
   ```

## <a name="enable-msi-on-an-existing-azure-virtual-machine-scale-set"></a>MSI inschakelen op een bestaande virtuele machine van Azure-scale set

Als u het inschakelen van MSI op een bestaande virtuele machine van Azure-scale set moet:

1. Als u de Azure CLI in een lokale console, eerst aanmelden bij het gebruik van Azure [az aanmelding](/cli/azure/#az_login). Gebruik een account dat is gekoppeld aan het Azure-abonnement met de virtuele-machineschaalset.

   ```azurecli-interactive
   az login
   ```

2. Gebruik [az vmss toewijzen-identity](/cli/azure/vm/#az_vmss_assign_identity) met de `--assign-identity` -parameter voor het toevoegen van een MSI-bestand naar een bestaande virtuele machine:

   ```azurecli-interactive
   az vmss assign-identity -g myResourceGroup -n myVMSS
   ```

## <a name="remove-msi-from-an-azure-virtual-machine-scale-set"></a>MSI uit een Azure virtuele-machineschaalset verwijderen

Als u een virtuele-machineschaalset die een MSI-bestand niet meer nodig hebt:

1. Als u de Azure CLI in een lokale console, eerst aanmelden bij het gebruik van Azure [az aanmelding](/cli/azure/#az_login). Gebruik een account dat is gekoppeld aan het Azure-abonnement met de virtuele-machineschaalset.

   ```azurecli-interactive
   az login
   ```

2. Gebruik de `--identities` overschakelen met [az vmss remove-identity](/cli/azure/vmss/#az_vmss_remove_identity) het MSI-bestand te verwijderen:

   ```azurecli-interactive
   az vmss remove-identity -g myResourceGroup -n myVMSS --identities readerID writerID
   ```

## <a name="next-steps"></a>Volgende stappen

- [Overzicht van beheerde Service-identiteit](overview.md)
- Voor de volledige Azure virtuele-machineschaalset maken Quick Start, Zie: 

  - [Een virtuele-Machineschaalset maken met CLI](../../virtual-machines/linux/tutorial-create-vmss.md#create-a-scale-set)

Gebruik de volgende sectie met opmerkingen uw feedback en help ons verfijnen en onze content vorm.
















