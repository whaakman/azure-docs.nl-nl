---
title: Het configureren van systeem en de gebruiker toegewezen identiteiten op een Azure-VMSS met Azure CLI
description: Stap voor stap toegewezen instructies voor het configureren van systeem- en identiteiten op een VMSS Azure met Azure CLI.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/15/2018
ms.author: daveba
ms.openlocfilehash: 376f1edab0d7babeb895f600fd2db1b447eeaa14
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/04/2018
---
# <a name="configure-a-virtual-machine-scale-set-managed-service-identity-msi-using-azure-cli"></a>Een virtuele machine configureren schaalset beheerde Service identiteit (MSI) met Azure CLI

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Beheerde Service-identiteit biedt Azure-services met een automatisch beheerde identiteit in Azure Active Directory. U kunt deze identiteit gebruiken om alle services die Azure AD-verificatie ondersteunt, zonder referenties in uw code te verifiëren. 

In dit artikel leert u hoe de volgende bewerkingen voor Service-identiteit beheerd op een Azure virtuele Machine schaal instellen (VMSS), met de Azure CLI uit te voeren:
- Inschakelen en uitschakelen van het systeem toegewezen identiteit op een Azure-VMSS
- Toevoegen en verwijderen van een gebruiker met de identiteit op een Azure-VMSS


## <a name="prerequisites"></a>Vereisten

- Als u niet bekend met de Service-identiteit beheerd bent, bekijk de [overzichtssectie van](overview.md). **Lees de [verschil tussen een systeem dat is toegewezen en de gebruiker toegewezen identiteit](overview.md#how-does-it-work)**.
- Als u al een Azure-account niet hebt [aanmelden voor een gratis account](https://azure.microsoft.com/free/) voordat u doorgaat.

De CLI script als voorbeelden wilt uitvoeren, hebt u drie opties:

- Gebruik [Azure Cloud Shell](../../cloud-shell/overview.md) vanuit de Azure-portal (Zie volgende sectie).
- Gebruik de ingesloten Azure Cloud Shell via de 'Deze probeer' knop, zich in de rechterbovenhoek van elk codeblok.
- [Installeer de nieuwste versie van CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.13 of hoger) als u liever een lokale CLI-console te gebruiken. 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="system-assigned-identity"></a>Automatisch toegewezen identiteit

Informatie over het inschakelen en uitschakelen van het systeem toegewezen identiteit voor een Azure-VMSS met Azure CLI in deze sectie.

### <a name="enable-system-assigned-identity-during-creation-of-an-azure-virtual-machine-scale-set"></a>Automatisch toegewezen identiteit inschakelen tijdens het maken van een Azure virtuele-machineschaalset

Als u wilt maken van een virtuele-machineschaalset ingesteld met het systeem toegewezen identiteit ingeschakeld:

1. Als u de Azure CLI in een lokale console, eerst aanmelden bij het gebruik van Azure [az aanmelding](/cli/azure/reference-index#az_login). Gebruik een account dat is gekoppeld aan het Azure-abonnement waarmee u wilt implementeren, de virtuele-machineschaalset:

   ```azurecli-interactive
   az login
   ```

2. Maak een [resourcegroep](../../azure-resource-manager/resource-group-overview.md#terminology) voor containment en implementatie van uw virtuele-machineschaalset en de bijbehorende resources, met behulp van [az groep maken](/cli/azure/group/#az_group_create). U kunt deze stap overslaan als u al een resourcegroep die u wilt gebruiken in plaats daarvan:

   ```azurecli-interactive 
   az group create --name myResourceGroup --location westus
   ```

3. Maken van een virtuele-machineschaalset ingesteld met [az vmss maken](/cli/azure/vmss/#az_vmss_create) . Het volgende voorbeeld wordt een virtuele-machineschaalset set met de naam *myVMSS* automatisch toegewezen identiteit, zoals aangevraagd door de `--assign-identity` parameter. De `--admin-username` en `--admin-password` parameters geeft u het account voor gebruiker met beheerdersrechten en het wachtwoord voor aanmelding bij de virtuele machine. Bijwerken van deze waarden naargelang nodig is voor uw omgeving: 

   ```azurecli-interactive 
   az vmss create --resource-group myResourceGroup --name myVMSS --image win2016datacenter --upgrade-policy-mode automatic --custom-data cloud-init.txt --admin-username azureuser --admin-password myPassword12 --assign-identity --generate-ssh-keys
   ```

### <a name="enable-system-assigned-identity-on-an-existing-azure-virtual-machine-scale-set"></a>Automatisch toegewezen identiteit op een bestaande virtuele machine van Azure-scale set inschakelen

Als u de identiteit van de automatisch toegewezen op een bestaande virtuele machine van Azure-scale set inschakelen moet:

1. Als u de Azure CLI in een lokale console, eerst aanmelden bij het gebruik van Azure [az aanmelding](/cli/azure/reference-index#az_login). Gebruik een account dat is gekoppeld aan het Azure-abonnement met de virtuele-machineschaalset.

   ```azurecli-interactive
   az login
   ```

2. Gebruik [az vmss identiteit toewijzen](/cli/azure/vmss/identity/#az_vmss_identity_assign) opdracht uit om de identiteit van een automatisch toegewezen naar een bestaande virtuele machine:

   ```azurecli-interactive
   az vmss identity assign -g myResourceGroup -n myVMSS
   ```

### <a name="disable-system-assigned-identity-from-an-azure-virtual-machine-scale-set"></a>Automatisch toegewezen identiteit van een Azure virtuele-machineschaalset uitschakelen

> [!NOTE]
> Managed Service-identiteit van een virtuele-Machineschaalset uitschakelen is momenteel niet ondersteund. In de tussentijd kunt u schakelen tussen het gebruik van het systeem toegewezen en toegewezen gebruikersidentiteiten. Controleer regelmatig op updates.

Als u een virtuele-machineschaalset die niet langer een systeem toegewezen identiteit, maar nog steeds moet gebruiker toegewezen identiteiten hebt, voer de volgende opdracht:

```azurecli-interactive
az vmss update -n myVMSS -g myResourceGroup --set identity.type='UserAssigned' 
```

U kunt de MSI-VM-extensie verwijderen met [az vmss identiteit verwijderen](/cli/azure/vmss/identity/#az_vmss_remove_identity) opdracht verwijderen van de systeemidentiteit toegewezen uit een VMSS:

   ```azurecli-interactive
   az vmss extension delete -n ManagedIdentityExtensionForWindows -g myResourceGroup -vmss-name myVMSS
   ```

## <a name="user-assigned-identity"></a>Identiteit toegewezen door gebruiker

Informatie over het inschakelen en het verwijderen van de identiteit van een gebruiker aan zijn toegewezen met Azure CLI in deze sectie.

### <a name="assign-a-user-assigned-identity-during-the-creation-of-an-azure-vmss"></a>Een gebruiker toegewezen identiteit tijdens het maken van een Azure-VMSS toewijzen

Deze sectie helpt u bij het maken van een VMSS en toewijzing van een gebruiker identiteit toegewezen aan de VMSS. Als u al een VMSS hebt die u wilt gebruiken, kunt u deze sectie overslaan en doorgaan met de volgende.

1. U kunt deze stap overslaan als u al een resourcegroep die u wilt gebruiken. Maak een [resourcegroep](~/articles/azure-resource-manager/resource-group-overview.md#terminology) voor containment en implementatie van uw identiteit gebruiker toegewezen met behulp van [az groep maken](/cli/azure/group/#az_group_create). Zorg ervoor dat u de `<RESOURCE GROUP>` en `<LOCATION>` parameterwaarden met uw eigen waarden. :

   ```azurecli-interactive 
   az group create --name <RESOURCE GROUP> --location <LOCATION>
   ```

2. Maken van een gebruiker toegewezen identiteit met [az identiteit maken](/cli/azure/identity#az-identity-create).  De `-g` parameter geeft u de resourcegroep waar de gebruiker toegewezen identiteit is gemaakt, en de `-n` parameter geeft u de naam ervan. Zorg ervoor dat u de `<RESOURCE GROUP>` en `<USER ASSIGNED IDENTITY NAME>` parameterwaarden met uw eigen waarden:

    > [!IMPORTANT]
    > Maken van toegewezen gebruikers-id's ondersteunt alleen alfanumerieke en het koppelteken (0-9 of a-z of A-Z of -) tekens. Bovendien moeten worden beperkt tot 24 tekens voor de toewijzing aan een VM/VMSS goed te laten werken. Controleer regelmatig op updates. Zie voor meer informatie [Veelgestelde vragen en bekende problemen](known-issues.md)


    ```azurecli-interactive
    az identity create -g <RESOURCE GROUP> -n <USER ASSIGNED IDENTITY NAME>
    ```
Het antwoord bevat de details voor de gebruiker toegewezen identiteit gemaakt, ziet er als volgt. De resource `id` waarde wordt toegewezen aan de toegewezen gebruikers-id wordt gebruikt in de volgende stap.

   ```json
   {
        "clientId": "73444643-8088-4d70-9532-c3a0fdc190fz",
        "clientSecretUrl": "https://control-westcentralus.identity.azure.net/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>/credentials?tid=5678&oid=9012&aid=73444643-8088-4d70-9532-c3a0fdc190fz",
        "id": "/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>",
        "location": "westcentralus",
        "name": "<USER ASSIGNED IDENTITY NAME>",
        "principalId": "e5fdfdc1-ed84-4d48-8551-fe9fb9dedfll",
        "resourceGroup": "<RESOURCE GROUP>",
        "tags": {},
        "tenantId": "733a8f0e-ec41-4e69-8ad8-971fc4b533bl",
        "type": "Microsoft.ManagedIdentity/userAssignedIdentities"    
   }
   ```

3. Maak een VMSS met [az vmss maken](/cli/azure/vmss/#az-vmss-create). Het volgende voorbeeld wordt een VMSS die zijn gekoppeld aan de nieuwe gebruiker toegewezen identiteit, zoals opgegeven door de `--assign-identity` parameter. Zorg ervoor dat u de `<RESOURCE GROUP>`, `<VMSS NAME>`, `<USER NAME>`, `<PASSWORD>`, en `<USER ASSIGNED IDENTITY ID>` parameterwaarden met uw eigen waarden. Voor `<USER ASSIGNED IDENTITY ID>`, gebruik van de identiteit van de gebruiker toegewezen resource `id` eigenschap in de vorige stap hebt gemaakt: 

   ```azurecli-interactive 
   az vmss create --resource-group <RESOURCE GROUP> --name <VMSS NAME> --image UbuntuLTS --admin-username <USER NAME> --admin-password <PASSWORD> --assign-identity <USER ASSIGNED IDENTITY ID>
   ```

### <a name="assign-a-user-assigned-identity-to-an-existing-azure-vm"></a>Toewijzen van een gebruiker identiteit toegewezen aan een bestaande virtuele machine in Azure

1. Maken van een gebruiker toegewezen identiteit met [az identiteit maken](/cli/azure/identity#az-identity-create).  De `-g` parameter geeft u de resourcegroep waar de gebruiker toegewezen identiteit is gemaakt, en de `-n` parameter geeft u de naam ervan. Zorg ervoor dat u de `<RESOURCE GROUP>` en `<USER ASSIGNED IDENTITY NAME>` parameterwaarden met uw eigen waarden:

    > [!IMPORTANT]
    > Toegewezen identiteiten met speciale tekens (dat wil zeggen liggend streepje) in de naam van de gebruiker maken wordt momenteel niet ondersteund. Gebruik alfanumerieke tekens. Controleer regelmatig op updates.  Zie voor meer informatie [Veelgestelde vragen en bekende problemen](known-issues.md)

    ```azurecli-interactive
    az identity create -g <RESOURCE GROUP> -n <USER ASSIGNED IDENTITY NAME>
    ```
Het antwoord bevat de details voor de gebruiker toegewezen identiteit gemaakt, ziet er als volgt. De resource `id` waarde wordt toegewezen aan de toegewezen gebruikers-id wordt gebruikt in de volgende stap.

   ```json
   {
        "clientId": "73444643-8088-4d70-9532-c3a0fdc190fz",
        "clientSecretUrl": "https://control-westcentralus.identity.azure.net/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY >/credentials?tid=5678&oid=9012&aid=73444643-8088-4d70-9532-c3a0fdc190fz",
        "id": "/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY>",
        "location": "westcentralus",
        "name": "<USER ASSIGNED IDENTITY>",
        "principalId": "e5fdfdc1-ed84-4d48-8551-fe9fb9dedfll",
        "resourceGroup": "<RESOURCE GROUP>",
        "tags": {},
        "tenantId": "733a8f0e-ec41-4e69-8ad8-971fc4b533bl",
        "type": "Microsoft.ManagedIdentity/userAssignedIdentities"    
   }
   ```

2. Toewijzen van de identiteit van de gebruiker toegewezen aan uw VMSS met [az vmss identiteit toewijzen](/cli/azure/vmss/identity#az_vm_assign_identity). Zorg ervoor dat u de `<RESOURCE GROUP>` en `<VM NAME>` parameterwaarden met uw eigen waarden. De `<USER ASSIGNED IDENTITY ID>` zijn van de identiteit van de gebruiker toegewezen resource `id` eigenschap, in de vorige stap hebt gemaakt:

    ```azurecli-interactive
    az vmss assign-identity -g <RESOURCE GROUP> -n <VM NAME> --identities <USER ASSIGNED IDENTITY ID>
    ```

### <a name="remove-a-user-assigned-identity-from-an-azure-vmss"></a>Een gebruiker toegewezen identiteit van een Azure-VMSS verwijderen

> [!NOTE]
>  Verwijderen van alle toegewezen gebruikersidentiteiten uit een virtuele-Machineschaalset is momenteel niet ondersteund, tenzij u een systeem toegewezen identiteit hebt. 

Als uw VMSS meerdere identiteiten van de gebruiker die is toegewezen heeft, kunt u alles behalve het laatste teken met behulp van [az vmss identiteit verwijderen](/cli/azure/vmss/identity#az-vmss-identity-remove). Zorg ervoor dat u de `<RESOURCE GROUP>` en `<VM NAME>` parameterwaarden met uw eigen waarden. De `<MSI NAME>` is de eigenschap name van de identiteit van de gebruiker die is toegewezen, die kan worden gevonden door in de sectie van de identiteit van het gebruik van de VM `az vm show`:

```azurecli-interactive
az vmss identity remove -g <RESOURCE GROUP> -n <VM NAME> --identities <MSI NAME>
```
Als uw VMSS heeft zowel automatisch toegewezen als de gebruiker toegewezen identiteiten, kunt u alle gebruikers identiteiten toegewezen door over te schakelen voor het gebruik van alleen automatisch toegewezen. Gebruik de volgende opdracht: 

```azurecli-interactive
az vmss update -n myVM -g myResourceGroup --set identity.type='SystemAssigned' identity.identityIds=null
```

## <a name="next-steps"></a>Volgende stappen

- [Overzicht van beheerde Service-identiteit](overview.md)
- Voor de volledige Azure virtuele-machineschaalset maken Quick Start, Zie: 

  - [Een virtuele-Machineschaalset maken met CLI](../../virtual-machines/linux/tutorial-create-vmss.md#create-a-scale-set)

















