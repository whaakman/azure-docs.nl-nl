---
title: Het configureren van MSI op een virtuele machine in Azure met Azure CLI
description: Stap door stapsgewijze instructies voor het configureren van een beheerde Service identiteit (MSI) op een virtuele machine van Azure, met Azure CLI.
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
ms.date: 09/14/2017
ms.author: bryanla
ms.openlocfilehash: 70500ab572be9902c040388ee31a3fbed601445f
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="configure-a-vm-managed-service-identity-msi-using-azure-cli"></a>Een VM beheerde Service identiteit (MSI) met Azure CLI configureren

[!INCLUDE[preview-notice](../../includes/active-directory-msi-preview-notice.md)]

Beheerde Service-identiteit biedt Azure-services met een automatisch beheerde identiteit in Azure Active Directory. U kunt deze identiteit gebruiken om alle services die Azure AD-verificatie ondersteunt, zonder referenties in uw code te verifiëren. 

In dit artikel leert u het inschakelen en het verwijderen van MSI voor een virtuele machine van Azure, met Azure CLI.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [msi-qs-configure-prereqs](../../includes/active-directory-msi-qs-configure-prereqs.md)]

De CLI script als voorbeelden wilt uitvoeren, hebt u drie opties:

- Gebruik [Azure Cloud Shell](../cloud-shell/overview.md) vanuit de Azure-portal (Zie volgende sectie).
- Gebruik de ingesloten Azure Cloud Shell via de 'Deze probeer' knop, zich in de rechterbovenhoek van elk codeblok.
- [Installeer de nieuwste versie van CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.13 of hoger) als u liever een lokale CLI-console te gebruiken. 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="enable-msi-during-creation-of-an-azure-vm"></a>MSI bij het maken van een virtuele machine van Azure inschakelen

Maken van een VM MSI ingeschakeld:

1. Als u de Azure CLI in een lokale console, eerst aanmelden bij het gebruik van Azure [az aanmelding](/cli/azure/#login). Gebruik een account dat is gekoppeld aan het Azure-abonnement waarmee u wilt implementeren, de virtuele machine:

   ```azurecli-interactive
   az login
   ```

2. Maak een [resourcegroep](../azure-resource-manager/resource-group-overview.md#terminology) voor containment en implementatie van uw virtuele machine en de bijbehorende resources, met behulp van [az groep maken](/cli/azure/group/#create). U kunt deze stap overslaan als u al resourcegroep die u wilt gebruiken in plaats daarvan:

   ```azurecli-interactive 
   az group create --name myResourceGroup --location westus
   ```

3. Maak een VM met [az vm maken](/cli/azure/vm/#create). Het volgende voorbeeld wordt een virtuele machine met de naam *myVM* met een MSI-bestand, zoals aangevraagd door de `--assign-identity` parameter. De `--admin-username` en `--admin-password` parameters geeft u het account voor gebruiker met beheerdersrechten en het wachtwoord voor aanmelding bij de virtuele machine. Bijwerken van deze waarden naargelang nodig is voor uw omgeving: 

   ```azurecli-interactive 
   az vm create --resource-group myResourceGroup --name myVM --image win2016datacenter --generate-ssh-keys --assign-identity --admin-username azureuser --admin-password myPassword12
   ```

## <a name="enable-msi-on-an-existing-azure-vm"></a>MSI op een bestaande virtuele machine van Azure inschakelen

Als u inschakelen, MSI op een bestaande virtuele Machine wilt:

1. Als u de Azure CLI in een lokale console, eerst aanmelden bij het gebruik van Azure [az aanmelding](/cli/azure/#login). Gebruik een account dat is gekoppeld aan het Azure-abonnement met de virtuele machine. Controleer ook of uw account behoort tot een functie waarmee u schrijfmachtigingen heeft op de virtuele machine, zoals 'Virtual Machine Contributor':

   ```azurecli-interactive
   az login
   ```

2. Gebruik [az vm toewijzen-identity](/cli/azure/vm/#az_vm_assign_identity) met de `--assign-identity` -parameter voor het toevoegen van een MSI-bestand naar een bestaande virtuele machine:

   ```azurecli-interactive
   az vm assign-identity -g myResourceGroup -n myVm
   ```

## <a name="remove-msi-from-an-azure-vm"></a>Verwijder de MSI van een Azure VM

Als u een virtuele Machine die niet langer een MSI-bestand hebt:

1. Als u de Azure CLI in een lokale console, eerst aanmelden bij het gebruik van Azure [az aanmelding](/cli/azure/#login). Gebruik een account dat is gekoppeld aan het Azure-abonnement met de virtuele machine. Controleer ook of uw account behoort tot een functie waarmee u schrijfmachtigingen heeft op de virtuele machine, zoals 'Virtual Machine Contributor':

   ```azurecli-interactive
   az login
   ```

2. Gebruik de `-n ManagedIdentityExtensionForWindows` of `-n ManagedIdentityExtensionForLinux` overschakelen (afhankelijk van het type van de virtuele machine) met [az vm-extensie verwijderen](https://docs.microsoft.com/cli/azure/vm/#assign-identity) het MSI-bestand te verwijderen:

   ```azurecli-interactive
   az vm extension delete --resource-group myResourceGroup --vm-name myVm -n ManagedIdentityExtensionForWindows
   ```

## <a name="related-content"></a>Gerelateerde inhoud

- [Overzicht van beheerde Service-identiteit](msi-overview.md)
- Voor het volledige virtuele machine in Azure maken snelstartgidsen, Zie: 

  - [Een Windows-machine maken met CLI](../virtual-machines/windows/quick-create-cli.md)  
  - [Een virtuele Linux-machine maken met CLI](../virtual-machines/linux/quick-create-cli.md) 

Gebruik de volgende sectie met opmerkingen uw feedback en help ons verfijnen en onze content vorm.
















