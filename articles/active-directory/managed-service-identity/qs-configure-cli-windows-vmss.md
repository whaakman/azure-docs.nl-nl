---
title: Het configureren van systeem en de gebruiker toegewezen identiteiten op een Azure-VMSS met behulp van Azure CLI
description: Stap voor stap toegewezen instructies voor het configureren van systeem- en identiteiten op een VMSS Azure met behulp van Azure CLI.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/15/2018
ms.author: daveba
ms.openlocfilehash: 36df9d00d41f3c092320fa88772b41c9a41c6d8e
ms.sourcegitcommit: 194789f8a678be2ddca5397137005c53b666e51e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39237278"
---
# <a name="configure-a-virtual-machine-scale-set-managed-service-identity-msi-using-azure-cli"></a>Een virtuele machine configureren Virtual Machine scale sets Managed Service Identity (MSI) met behulp van Azure CLI

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Beheerde Service-identiteit biedt Azure-services met een automatisch beheerde identiteit in Azure Active Directory. U kunt deze identiteit gebruiken om te verifiëren bij een service die ondersteuning biedt voor Azure AD-verificatie, zonder referenties in uw code. 

In dit artikel leert u hoe u de volgende bewerkingen voor beheerde Service-identiteit op een Azure Virtual Machine Scale instellen (VMSS), met de Azure CLI uitvoeren:
- In- en uitschakelen van het systeem toegewezen identiteit in een Azure VMSS
- Toevoegen en verwijderen van een gebruiker toegewezen identiteit in een Azure VMSS


## <a name="prerequisites"></a>Vereisten

- Als u niet bekend met beheerde Service-identiteit bent, bekijk dan de [overzichtssectie](overview.md). **Lees de [verschil tussen een systeem toegewezen en een gebruiker toegewezen identiteit](overview.md#how-does-it-work)**.
- Als u nog geen Azure-account hebt, [registreer u dan voor een gratis account](https://azure.microsoft.com/free/) voordat u verdergaat.
- Als u wilt de beheerbewerkingen in dit artikel uitvoert, moet uw account de volgende roltoewijzingen:
    - [Inzender voor virtuele machines](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) instellen voor een virtuele-machineschaalset maken en inschakelen en remove-systeem en/of gebruiker toegewezen beheerde identiteit van een virtuele-machineschaalset.
    - [Beheerde identiteit Inzender](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rol te maken van de identiteit van een gebruiker toegewezen.
    - [Beheerde identiteit Operator](/azure/role-based-access-control/built-in-roles#managed-identity-operator) rol die u wilt toewijzen en verwijderen van een gebruiker toegewezen identiteit van en naar een virtuele-machineschaalset.
- Als u wilt de CLI-scriptvoorbeelden uitvoeren, hebt u drie opties:
    - Gebruik [Azure Cloud Shell](../../cloud-shell/overview.md) vanuit Azure portal (Zie volgende sectie).
    - Gebruik de ingesloten Azure Cloud Shell via het 'Try It' de knop, zich in de rechterbovenhoek van elk codeblok.
    - [Installeer de nieuwste versie van CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.13 of hoger) als u liever een lokale CLI-console. 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="system-assigned-identity"></a>Systeem toegewezen identiteit

In deze sectie leert u hoe u inschakelen en uitschakelen van het systeem toegewezen identiteit voor een Azure-VMSS met behulp van Azure CLI.

### <a name="enable-system-assigned-identity-during-creation-of-an-azure-virtual-machine-scale-set"></a>Systeem toegewezen identiteit inschakelen tijdens het maken van een schaalset voor virtuele Azure-machine

Maken van een virtuele-machineschaalset met het systeem toegewezen identiteit is ingeschakeld:

1. Als u de Azure CLI in een lokale console gebruikt, meldt u zich eerst aan bij Azure met [az login](/cli/azure/reference-index#az_login). Gebruik een account dat is gekoppeld aan het Azure-abonnement waarin u wilt implementeren van de virtuele-machineschaalset:

   ```azurecli-interactive
   az login
   ```

2. Maak een [resourcegroep](../../azure-resource-manager/resource-group-overview.md#terminology) voor insluiting en implementatie van uw virtuele-machineschaalset en de bijbehorende resources, met behulp van [az-groep maken](/cli/azure/group/#az_group_create). U kunt deze stap overslaan als u al een resourcegroep die u wilt gebruiken in plaats daarvan:

   ```azurecli-interactive 
   az group create --name myResourceGroup --location westus
   ```

3. Maak een VM-schaalset met behulp van [az vmss maken](/cli/azure/vmss/#az_vmss_create) . Het volgende voorbeeld wordt een virtuele-machineschaalset met de naam *myVMSS* met een systeem toegewezen identiteit, zoals aangevraagd door de `--assign-identity` parameter. Met de parameters `--admin-username` en `--admin-password` worden de naam van de gebruiker met beheerdersrechten en het wachtwoord van het account voor aanmelding bij de virtuele machine opgegeven. Werk deze waarden bij met waarden die geschikt zijn voor uw omgeving: 

   ```azurecli-interactive 
   az vmss create --resource-group myResourceGroup --name myVMSS --image win2016datacenter --upgrade-policy-mode automatic --custom-data cloud-init.txt --admin-username azureuser --admin-password myPassword12 --assign-identity --generate-ssh-keys
   ```

### <a name="enable-system-assigned-identity-on-an-existing-azure-virtual-machine-scale-set"></a>Systeem toegewezen identiteit op een bestaande schaalset voor virtuele Azure-machine inschakelen

Als u nodig hebt om in te schakelen van het systeem toegewezen identiteit op een bestaande schaalset voor virtuele Azure-machine:

1. Als u de Azure CLI in een lokale console gebruikt, meldt u zich eerst aan bij Azure met [az login](/cli/azure/reference-index#az_login). Gebruik een account dat is gekoppeld aan het Azure-abonnement met de virtuele-machineschaalset.

   ```azurecli-interactive
   az login
   ```

2. Gebruik [az vmss-identiteit toewijzen](/cli/azure/vmss/identity/#az_vmss_identity_assign) opdracht uit om een systeem toegewezen identiteit aan een bestaande virtuele machine:

   ```azurecli-interactive
   az vmss identity assign -g myResourceGroup -n myVMSS
   ```

### <a name="disable-system-assigned-identity-from-an-azure-virtual-machine-scale-set"></a>Systeem toegewezen identiteit van een schaalset voor virtuele Azure-machine uitschakelen

Als u een virtuele-machineschaalset die niet langer moet het systeem toegewezen identiteit, maar nog steeds moet gebruiker toegewezen identiteiten hebt, gebruikt u de volgende opdracht uit:

```azurecli-interactive
az vmss update -n myVM -g myResourceGroup --set identity.type='UserAssigned' 
```

Als u een virtuele machine die systeem toegewezen identiteit niet meer nodig hebt en er geen gebruiker toegewezen identiteiten, gebruikt u de volgende opdracht uit:

> [!NOTE]
> De waarde `none` is hoofdlettergevoelig. Er moet een kleine letter. 

```azurecli-interactive
az vmss update -n myVM -g myResourceGroup --set identity.type="none"
```

U kunt de MSI-VM-extensie verwijderen [az vmss-identiteit verwijderen](/cli/azure/vmss/identity/#az_vmss_remove_identity) opdracht om te verwijderen van het systeem toegewezen identiteit van een VMSS:

```azurecli-interactive
az vmss extension delete -n ManagedIdentityExtensionForWindows -g myResourceGroup -vmss-name myVMSS
```

## <a name="user-assigned-identity"></a>Door gebruiker toegewezen identiteit

In deze sectie leert u hoe u om te schakelen en te verwijderen van een gebruiker toegewezen identiteit met Azure CLI.

### <a name="assign-a-user-assigned-identity-during-the-creation-of-an-azure-vmss"></a>Een gebruiker toegewezen identiteit tijdens het maken van een Azure-VMSS toewijzen

In deze sectie helpt u bij het maken van een VMSS en de toewijzing van een gebruiker toegewezen identiteit voor de VMSS. Als u al een VMSS hebt die u wilt gebruiken, kunt u deze sectie overslaan en doorgaan met de volgende.

1. U kunt deze stap overslaan als u al een resourcegroep die u wilt gebruiken. Maak een [resourcegroep](~/articles/azure-resource-manager/resource-group-overview.md#terminology) voor insluiting en implementatie van de gebruiker toegewezen identiteit, met behulp van [az-groep maken](/cli/azure/group/#az_group_create). Vervang de parameterwaarden `<RESOURCE GROUP>` en `<LOCATION>` door uw eigen waarden. :

   ```azurecli-interactive 
   az group create --name <RESOURCE GROUP> --location <LOCATION>
   ```

2. Een gebruiker toegewezen identiteit maken [az-identiteit maken](/cli/azure/identity#az-identity-create).  De `-g` parameter geeft u de resourcegroep waar de gebruiker toegewezen identiteit is gemaakt, en de `-n` parameter geeft u de naam ervan. Vervang de parameterwaarden `<RESOURCE GROUP>` en `<USER ASSIGNED IDENTITY NAME>` door uw eigen waarden:

   [!INCLUDE[ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

   ```azurecli-interactive
   az identity create -g <RESOURCE GROUP> -n <USER ASSIGNED IDENTITY NAME>
   ```
   Het antwoord bevat details voor de gebruiker toegewezen identiteit gemaakt, vergelijkbaar met de volgende. De resource `id` waarde die is toegewezen aan de gebruiker toegewezen identiteit wordt gebruikt in de volgende stap.

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

3. Maak een VMSS met [az vmss maken](/cli/azure/vmss/#az-vmss-create). Het volgende voorbeeld wordt een VMSS die zijn gekoppeld aan de nieuwe gebruiker toegewezen identiteit, zoals opgegeven door de `--assign-identity` parameter. Vervang de parameterwaarden `<RESOURCE GROUP>`, `<VMSS NAME>`, `<USER NAME>`, `<PASSWORD>` en `<USER ASSIGNED IDENTITY ID>` door uw eigen waarden. Voor `<USER ASSIGNED IDENTITY ID>`, gebruik van de identiteit van de gebruiker toegewezen resource `id` eigenschap in de vorige stap hebt gemaakt: 

   ```azurecli-interactive 
   az vmss create --resource-group <RESOURCE GROUP> --name <VMSS NAME> --image UbuntuLTS --admin-username <USER NAME> --admin-password <PASSWORD> --assign-identity <USER ASSIGNED IDENTITY ID>
   ```

### <a name="assign-a-user-assigned-identity-to-an-existing-azure-vm"></a>Een gebruiker toegewezen identiteit aan een bestaande VM in Azure toewijzen

1. Een gebruiker toegewezen identiteit maken [az-identiteit maken](/cli/azure/identity#az-identity-create).  De `-g` parameter geeft u de resourcegroep waar de gebruiker toegewezen identiteit is gemaakt, en de `-n` parameter geeft u de naam ervan. Vervang de parameterwaarden `<RESOURCE GROUP>` en `<USER ASSIGNED IDENTITY NAME>` door uw eigen waarden:

    > [!IMPORTANT]
    > Het maken van de gebruiker toegewezen identiteiten met speciale tekens (dat wil zeggen onderstrepingstekens) in de naam is momenteel niet ondersteund. Gebruik alfanumerieke tekens. Controleer later op updates.  Zie voor meer informatie [Veelgestelde vragen en bekende problemen](known-issues.md)

    ```azurecli-interactive
    az identity create -g <RESOURCE GROUP> -n <USER ASSIGNED IDENTITY NAME>
    ```
Het antwoord bevat details voor de gebruiker toegewezen identiteit gemaakt, vergelijkbaar met de volgende. De resource `id` waarde die is toegewezen aan de gebruiker toegewezen identiteit wordt gebruikt in de volgende stap.

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

2. Toewijzen van de identiteit van de gebruiker toegewezen aan uw VMSS met [az vmss-identiteit toewijzen](/cli/azure/vmss/identity#az_vm_assign_identity). Vervang de parameterwaarden `<RESOURCE GROUP>` en `<VMSS NAME>` door uw eigen waarden. De `<USER ASSIGNED IDENTITY ID>` is van de identiteit van de gebruiker toegewezen resource `id` eigenschap, zoals in de vorige stap hebt gemaakt:

    ```azurecli-interactive
    az vmss identity assign -g <RESOURCE GROUP> -n <VMSS NAME> --identities <USER ASSIGNED IDENTITY ID>
    ```

### <a name="remove-a-user-assigned-identity-from-an-azure-virtual-machine-scale-set"></a>Een gebruiker toegewezen identiteit verwijderen uit een schaalset voor virtuele Azure-machine

Verwijderen van een toegewezen gebruikers-id van het gebruik van een virtuele machine scale set [az vmss-identiteit verwijderen](/cli/azure/vmss/identity#az-vmss-identity-remove). Vervang de parameterwaarden `<RESOURCE GROUP>` en `<VMSS NAME>` door uw eigen waarden. De `<MSI NAME>` is van de gebruiker toegewezen identiteit `name` eigenschap, die kan worden gevonden door in de sectie van de identiteit van het gebruik van de virtuele machine `az vmss identity show`:

```azurecli-interactive
az vmss identity remove -g <RESOURCE GROUP> -n <VMSS NAME> --identities <MSI NAME>
```

Als uw virtuele-machineschaalset heeft geen een systeem toegewezen identiteit en u wilt verwijderen van alle gebruiker toegewezen identiteiten uit, gebruikt u de volgende opdracht uit:

> [!NOTE]
> De waarde `none` is hoofdlettergevoelig. Er moet een kleine letter.

```azurecli-interactive
az vmss update -n myVMSS -g myResourceGroup --set identity.type="none" identity.identityIds=null
```

Als uw virtuele-machineschaalset is zowel de systeem toegewezen als de gebruiker toegewezen identiteiten, kunt u alle gebruiker toegewezen identiteiten door over te schakelen voor het gebruik van alleen systeem toegewezen verwijderen. Gebruik de volgende opdracht:

```azurecli-interactive
az vmss update -n myVMSS -g myResourceGroup --set identity.type='SystemAssigned' identity.identityIds=null 
```

## <a name="next-steps"></a>Volgende stappen

- [Overzicht van de beheerde Service-identiteit](overview.md)
- Voor de volledige Azure virtuele-machineschaalset maken Quick Start, Zie: 

  - [Een virtuele-Machineschaalset maken met CLI](../../virtual-machines/linux/tutorial-create-vmss.md#create-a-scale-set)

















