---
title: Het configureren van een gebruiker toegewezen MSI voor een virtuele machine in Azure met Azure CLI
description: Stap door stapsgewijze instructies voor het configureren van een gebruiker toegewezen beheerde Service identiteit (MSI) voor een virtuele machine van Azure, met Azure CLI.
services: active-directory
documentationcenter: 
author: BryanLa
manager: mtillman
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/22/2017
ms.author: bryanla
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 4b6f4e2b0e42724276448fd4726c8326de8ea6ee
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2018
---
# <a name="configure-a-user-assigned-managed-service-identity-msi-for-a-vm-using-azure-cli"></a>Een gebruiker toegewezen beheerde Service identiteit (MSI) configureren voor een virtuele machine met Azure CLI

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Beheerde Service-identiteit biedt Azure-services met een beheerde identiteit in Azure Active Directory. U kunt deze identiteit te verifiëren bij services die ondersteuning bieden voor Azure AD-verificatie, zonder referenties in uw code. 

In dit artikel leert u het inschakelen en het verwijderen van een gebruiker toegewezen MSI voor een virtuele machine van Azure, met Azure CLI.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

Als u wilt uitvoeren in de CLI scriptvoorbeelden in deze zelfstudie, hebt u twee opties:

- Gebruik [Azure Cloud Shell](~/articles/cloud-shell/overview.md) vanuit de Azure-portal of via de knop 'Deze probeer' zich in de rechterbovenhoek van elk codeblok.
- [Installeer de nieuwste versie van CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.23 of hoger) als u liever een lokale CLI-console te gebruiken. Meld u aan met behulp van Azure [az aanmelding](/cli/azure/#login). Gebruik een account dat is gekoppeld aan het Azure-abonnement waarmee u wilt de gebruiker toegewezen MSI en VM implementeren:

   ```azurecli
   az login
   ```

## <a name="enable-msi-during-creation-of-an-azure-vm"></a>MSI bij het maken van een virtuele machine van Azure inschakelen

Deze sectie helpt u bij het maken van de virtuele machine en de toewijzing van de gebruiker toegewezen MSI naar de virtuele machine. Als u al een virtuele machine die u wilt gebruiken, kunt u deze sectie overslaan en doorgaan met de volgende.

1. U kunt deze stap overslaan als u al een resourcegroep die u wilt gebruiken. Maak een [resourcegroep](~/articles/azure-resource-manager/resource-group-overview.md#terminology) voor de implementatie van uw MSI en containment met [az groep maken](/cli/azure/group/#create). Zorg ervoor dat u de `<RESOURCE GROUP>` en `<LOCATION>` parameterwaarden met uw eigen waarden. :

   ```azurecli-interactive 
   az group create --name <RESOURCE GROUP> --location <LOCATION>
   ```

2. Maak een gebruiker toegewezen MSI met [az identiteit maken](/cli/azure/identity#az_identity_create).  De `-g` parameter geeft u de resourcegroep waar het MSI-bestand is gemaakt, en de `-n` parameter geeft u de naam ervan. Zorg ervoor dat u de `<RESOURCE GROUP>` en `<MSI NAME>` parameterwaarden met uw eigen waarden:

    ```azurecli-interactive
    az identity create -g <RESOURCE GROUP> -n <MSI NAME>
    ```
Het antwoord bevat de details voor de gebruiker toegewezen MSI gemaakt, ziet er als volgt. De resource `id` waarde wordt toegewezen aan het MSI-bestand wordt gebruikt in de volgende stap.

   ```json
   {
        "clientId": "73444643-8088-4d70-9532-c3a0fdc190fz",
        "clientSecretUrl": "https://control-westcentralus.identity.azure.net/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>/credentials?tid=5678&oid=9012&aid=73444643-8088-4d70-9532-c3a0fdc190fz",
        "id": "/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>",
        "location": "westcentralus",
        "name": "<MSI NAME>",
        "principalId": "e5fdfdc1-ed84-4d48-8551-fe9fb9dedfll",
        "resourceGroup": "<RESOURCE GROUP>",
        "tags": {},
        "tenantId": "733a8f0e-ec41-4e69-8ad8-971fc4b533bl",
        "type": "Microsoft.ManagedIdentity/userAssignedIdentities"    
   }
   ```

3. Maak een VM met [az vm maken](/cli/azure/vm/#create). Het volgende voorbeeld wordt een virtuele machine die is gekoppeld aan de nieuwe gebruiker toegewezen MSI, zoals opgegeven door de `--assign-identity` parameter. Zorg ervoor dat u de `<RESOURCE GROUP>`, `<VM NAME>`, `<USER NAME>`, `<PASSWORD>`, en `<`MSI-ID >` parameter values with your own values. For `<MSI ID>`, use the user-assigned MSI's resource `id-eigenschap in de vorige stap hebt gemaakt: 

   ```azurecli-interactive 
   az vm create --resource-group <RESOURCE GROUP> --name <VM NAME> --image UbuntuLTS --admin-username <USER NAME> --admin-password <PASSWORD> --assign-identity <MSI ID>
   ```

## <a name="enable-msi-on-an-existing-azure-vm"></a>MSI op een bestaande virtuele machine van Azure inschakelen

1. Maak een gebruiker toegewezen MSI met [az identiteit maken](/cli/azure/identity#az_identity_create).  De `-g` parameter geeft u de resourcegroep waar het MSI-bestand is gemaakt, en de `-n` parameter geeft u de naam ervan. Zorg ervoor dat u de `<RESOURCE GROUP>` en `<MSI NAME>` parameterwaarden met uw eigen waarden:

    ```azurecli-interactive
    az identity create -g <RESOURCE GROUP> -n <MSI NAME>
    ```
Het antwoord bevat de details voor de gebruiker toegewezen MSI gemaakt, ziet er als volgt. De resource `id` waarde wordt toegewezen aan het MSI-bestand wordt gebruikt in de volgende stap.

   ```json
   {
        "clientId": "73444643-8088-4d70-9532-c3a0fdc190fz",
        "clientSecretUrl": "https://control-westcentralus.identity.azure.net/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>/credentials?tid=5678&oid=9012&aid=73444643-8088-4d70-9532-c3a0fdc190fz",
        "id": "/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>",
        "location": "westcentralus",
        "name": "<MSI NAME>",
        "principalId": "e5fdfdc1-ed84-4d48-8551-fe9fb9dedfll",
        "resourceGroup": "<RESOURCE GROUP>",
        "tags": {},
        "tenantId": "733a8f0e-ec41-4e69-8ad8-971fc4b533bl",
        "type": "Microsoft.ManagedIdentity/userAssignedIdentities"    
   }
   ```

2. De gebruiker toegewezen MSI toewijzen voor het gebruik van uw VM [az vm toewijzen-identity](/cli/azure/vm#az_vm_assign_identity). Zorg ervoor dat u de `<RESOURCE GROUP>` en `<VM NAME>` parameterwaarden met uw eigen waarden. De `<MSI ID>` wordt de gebruiker toegewezen MSI-resource worden `id` eigenschap, in de vorige stap hebt gemaakt:

    ```azurecli-interactive
    az vm assign-identity -g <RESOURCE GROUP> -n <VM NAME> --identities <MSI ID>
    ```

## <a name="remove-msi-from-an-azure-vm"></a>Verwijder de MSI van een Azure VM

1. Verwijderen van de gebruiker toegewezen MSI uit uw virtuele machine met [az vm remove-identity](/cli/azure/vm#az_vm_remove_identity). Zorg ervoor dat u de `<RESOURCE GROUP>` en `<VM NAME>` parameterwaarden met uw eigen waarden. De `<MSI NAME>` worden de gebruiker toegewezen MSI `name` eigenschap die is opgegeven tijdens de `az identity create` opdracht (Zie voorbeelden in de vorige secties):

   ```azurecli-interactive
   az vm remove-identity -g <RESOURCE GROUP> -n <VM NAME> --identities <MSI NAME>
   ```

## <a name="next-steps"></a>Volgende stappen

- [Overzicht van beheerde Service-identiteit](msi-overview.md)
- Voor het volledige virtuele machine in Azure maken snelstartgidsen, Zie: 

  - [Een Windows-machine maken met CLI](~/articles/virtual-machines/windows/quick-create-cli.md)  
  - [Een virtuele Linux-machine maken met CLI](~/articles/virtual-machines/linux/quick-create-cli.md) 

Gebruik de volgende sectie met opmerkingen uw feedback en help ons verfijnen en onze content vorm.
















