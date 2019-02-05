---
title: Systeem- en gebruiker toegewezen beheerde identiteiten op een Azure-VMSS met behulp van Azure CLI configureren
description: Voor stap door stap instructies voor het configureren van systeem en de gebruiker toegewezen beheerde identiteiten op een Azure-VMSS, met behulp van Azure CLI.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/15/2018
ms.author: priyamo
ms.openlocfilehash: 42b0ab15f43d301629b9fbb3208ba24eae8c227e
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/04/2019
ms.locfileid: "55694598"
---
# <a name="configure-managed-identities-for-azure-resources-on-a-virtual-machine-scale-set-using-azure-cli"></a>Configureren van beheerde identiteiten voor Azure-resources op een VM-schaalset met behulp van Azure CLI

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Beheerde identiteiten voor Azure-resources biedt Azure-services met een automatisch beheerde identiteit in Azure Active Directory. U kunt deze identiteit gebruiken om te verifiëren bij een service die ondersteuning biedt voor Azure AD-verificatie, zonder referenties in uw code. 

In dit artikel leert u hoe u de volgende beheerde identiteiten voor bewerkingen van de Azure-resources op een Azure Virtual Machine Scale instellen (VMSS), met de Azure CLI uitvoert:
- In- en uitschakelen van de door het systeem toegewezen beheerde identiteit in een Azure VMSS
- Toevoegen en verwijderen van een gebruiker toegewezen beheerde identiteit in een Azure VMSS


## <a name="prerequisites"></a>Vereisten

- Als u niet bekend met beheerde identiteiten voor Azure-resources bent, lees de [overzichtssectie](overview.md). **Lees de [verschil tussen een beheerde identiteit door het systeem is toegewezen en de gebruiker toegewezen](overview.md#how-does-it-work)**.
- Als u nog geen Azure-account hebt, [registreer u dan voor een gratis account](https://azure.microsoft.com/free/) voordat u verdergaat.
- Als u wilt de beheerbewerkingen in dit artikel uitvoert, moet uw account de volgende Azure op basis van rollen access control-toewijzingen:

    > [!NOTE]
    > Er zijn geen extra Azure AD directory-roltoewijzingen is vereist.

    - [Inzender voor virtuele machines](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) aan een virtuele-machineschaalset maken en inschakelen en verwijderen van systeem en/of de gebruiker toegewezen beheerde identiteit van een virtuele-machineschaalset.
    - [Beheerde identiteit Inzender](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rol te maken van een gebruiker toegewezen beheerde identiteit.
    - [Beheerde identiteit Operator](/azure/role-based-access-control/built-in-roles#managed-identity-operator) rol die u wilt toewijzen en verwijderen van een gebruiker toegewezen beheerde identiteit van en naar een virtuele-machineschaalset.
- Als u wilt de CLI-scriptvoorbeelden uitvoeren, hebt u drie opties:
    - Gebruik [Azure Cloud Shell](../../cloud-shell/overview.md) vanuit Azure portal (Zie volgende sectie).
    - Gebruik de ingesloten Azure Cloud Shell via het 'Try It' de knop, zich in de rechterbovenhoek van elk codeblok.
    - [Installeer de nieuwste versie van de Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.13 of hoger) als u liever een lokale CLI-console. 
      
      > [!NOTE]
      > De opdrachten zijn bijgewerkt in overeenstemming met de nieuwste versie van de [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli).

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="system-assigned-managed-identity"></a>Het systeem toegewezen beheerde identiteit

In deze sectie leert u hoe u inschakelen en uitschakelen van de door het systeem toegewezen beheerde identiteit voor een Azure-VMSS met behulp van Azure CLI.

### <a name="enable-system-assigned-managed-identity-during-creation-of-an-azure-virtual-machine-scale-set"></a>Beheerde identiteit systeem toegewezen tijdens het maken van een schaalset voor virtuele Azure-machine inschakelen

Een virtuele-machineschaalset met de ingeschakeld door het systeem toegewezen beheerde identiteit maken:

1. Als u de Azure CLI in een lokale console gebruikt, meldt u zich eerst aan bij Azure met [az login](/cli/azure/reference-index#az-login). Gebruik een account dat is gekoppeld aan het Azure-abonnement waarin u wilt implementeren van de virtuele-machineschaalset:

   ```azurecli-interactive
   az login
   ```

2. Maak een [resourcegroep](../../azure-resource-manager/resource-group-overview.md#terminology) voor insluiting en implementatie van uw virtuele-machineschaalset en de bijbehorende resources, met behulp van [az-groep maken](/cli/azure/group/#az-group-create). U kunt deze stap overslaan als u al een resourcegroep die u wilt gebruiken in plaats daarvan:

   ```azurecli-interactive 
   az group create --name myResourceGroup --location westus
   ```

3. Maak een VM-schaalset met behulp van [az vmss maken](/cli/azure/vmss/#az-vmss-create) . Het volgende voorbeeld wordt een virtuele-machineschaalset met de naam *myVMSS* beheerd met een door het systeem toegewezen identiteit, zoals aangevraagd door de `--assign-identity` parameter. Met de parameters `--admin-username` en `--admin-password` worden de naam van de gebruiker met beheerdersrechten en het wachtwoord van het account voor aanmelding bij de virtuele machine opgegeven. Werk deze waarden bij met waarden die geschikt zijn voor uw omgeving: 

   ```azurecli-interactive 
   az vmss create --resource-group myResourceGroup --name myVMSS --image win2016datacenter --upgrade-policy-mode automatic --custom-data cloud-init.txt --admin-username azureuser --admin-password myPassword12 --assign-identity --generate-ssh-keys
   ```

### <a name="enable-system-assigned-managed-identity-on-an-existing-azure-virtual-machine-scale-set"></a>De beheerde inschakelen door het systeem toegewezen identiteit op een bestaande schaalset voor virtuele machine van Azure

Als u nodig hebt om in te schakelen door het systeem toegewezen beheerde identiteit van de op een bestaande schaalset voor virtuele Azure-machine:

1. Als u de Azure CLI in een lokale console gebruikt, meldt u zich eerst aan bij Azure met [az login](/cli/azure/reference-index#az-login). Gebruik een account dat is gekoppeld aan het Azure-abonnement met de virtuele-machineschaalset.

   ```azurecli-interactive
   az login
   ```

2. Gebruik [az vmss-identiteit toewijzen](/cli/azure/vmss/identity/#az-vmss-identity-assign) opdracht uit om een systeem toegewezen identiteit aan een bestaande VM beheerd:

   ```azurecli-interactive
   az vmss identity assign -g myResourceGroup -n myVMSS
   ```

### <a name="disable-system-assigned-managed-identity-from-an-azure-virtual-machine-scale-set"></a>Beheerde identiteit van een schaalset voor virtuele machine van Azure door het systeem toegewezen uitschakelen

Als u een virtuele-machineschaalset die niet langer moet het systeem toegewezen beheerde identiteit, maar nog steeds moet gebruiker toegewezen identiteiten op beheerde hebt, gebruikt u de volgende opdracht uit:

```azurecli-interactive
az vmss update -n myVM -g myResourceGroup --set identity.type='UserAssigned' 
```

Als u een virtuele machine die niet langer nodig door het systeem toegewezen beheerde identiteit heeft en er geen beheerde gebruiker toegewezen identiteiten, gebruikt u de volgende opdracht uit:

> [!NOTE]
> De waarde `none` is hoofdlettergevoelig. Er moet een kleine letter. 

```azurecli-interactive
az vmss update -n myVM -g myResourceGroup --set identity.type="none"
```

Als u wilt verwijderen van de beheerde identiteiten voor VM-extensie van een Azure-resources (gepland voor de afschaffing in januari 2019), gebruikt u [az vmss-identiteit verwijderen](/cli/azure/vmss/identity/) opdracht om te verwijderen door het systeem toegewezen identiteit beheerd vanaf een VMSS:

```azurecli-interactive
az vmss extension delete -n ManagedIdentityExtensionForWindows -g myResourceGroup -vmss-name myVMSS
```

## <a name="user-assigned-managed-identity"></a>de gebruiker toegewezen beheerde identiteit

In deze sectie leert u hoe u om te schakelen en te verwijderen van een gebruiker toegewezen beheerde identiteit met Azure CLI.

### <a name="assign-a-user-assigned-managed-identity-during-the-creation-of-a-virtual-machine-scale-set"></a>Een gebruiker toegewezen beheerde identiteit toewijzen tijdens het maken van een virtuele-machineschaalset

In deze sectie helpt u bij het maken van een VMSS en de toewijzing van een beheerde identiteit voor de gebruiker toegewezen aan de VMSS. Als u al een VMSS hebt die u wilt gebruiken, kunt u deze sectie overslaan en doorgaan met de volgende.

1. U kunt deze stap overslaan als u al een resourcegroep die u wilt gebruiken. Maak een [resourcegroep](~/articles/azure-resource-manager/resource-group-overview.md#terminology) voor insluiting en implementatie van de gebruiker toegewezen beheerde identiteit, met behulp van [az-groep maken](/cli/azure/group/#az-group-create). Vervang de parameterwaarden `<RESOURCE GROUP>` en `<LOCATION>` door uw eigen waarden. :

   ```azurecli-interactive 
   az group create --name <RESOURCE GROUP> --location <LOCATION>
   ```

2. Maak een door de gebruiker toegewezen beheerde identiteit met [az identity create](/cli/azure/identity#az-identity-create).  De parameter `-g` geeft de resourcegroep aan waarin de door de gebruiker toegewezen beheerde identiteit wordt gemaakt en de parameter `-n` geeft de naam ervan aan. Vervang de parameterwaarden `<RESOURCE GROUP>` en `<USER ASSIGNED IDENTITY NAME>` door uw eigen waarden:

   [!INCLUDE [ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

   ```azurecli-interactive
   az identity create -g <RESOURCE GROUP> -n <USER ASSIGNED IDENTITY NAME>
   ```
   Het antwoord bevat details voor de gebruiker toegewezen beheerde identiteit gemaakt, vergelijkbaar met de volgende. De resource `id` waarde is toegewezen aan de gebruiker toegewezen beheerde identiteit wordt gebruikt in de volgende stap.

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

3. Maak een VMSS met [az vmss maken](/cli/azure/vmss/#az-vmss-create). Het volgende voorbeeld wordt een VMSS die zijn gekoppeld aan de nieuwe gebruiker toegewezen beheerde identiteit, zoals opgegeven door de `--assign-identity` parameter. Vervang de parameterwaarden `<RESOURCE GROUP>`, `<VMSS NAME>`, `<USER NAME>`, `<PASSWORD>` en `<USER ASSIGNED IDENTITY>` door uw eigen waarden. 

   ```azurecli-interactive 
   az vmss create --resource-group <RESOURCE GROUP> --name <VMSS NAME> --image UbuntuLTS --admin-username <USER NAME> --admin-password <PASSWORD> --assign-identity <USER ASSIGNED IDENTITY>
   ```

### <a name="assign-a-user-assigned-managed-identity-to-an-existing-virtual-machine-scale-set"></a>Een gebruiker toegewezen beheerde identiteit toewijzen aan een bestaande virtuele-machineschaalset

1. Maak een door de gebruiker toegewezen beheerde identiteit met [az identity create](/cli/azure/identity#az-identity-create).  De parameter `-g` geeft de resourcegroep aan waarin de door de gebruiker toegewezen beheerde identiteit wordt gemaakt en de parameter `-n` geeft de naam ervan aan. Vervang de parameterwaarden `<RESOURCE GROUP>` en `<USER ASSIGNED IDENTITY NAME>` door uw eigen waarden:

    > [!IMPORTANT]
    > Het maken van beheerde identiteiten gebruiker toegewezen met speciale tekens (dat wil zeggen onderstrepingstekens) in de naam is momenteel niet ondersteund. Gebruik alfanumerieke tekens. Controleer later op updates.  Zie voor meer informatie [Veelgestelde vragen en bekende problemen](known-issues.md)

    ```azurecli-interactive
    az identity create -g <RESOURCE GROUP> -n <USER ASSIGNED IDENTITY NAME>
    ```
Het antwoord bevat details voor de gebruiker toegewezen beheerde identiteit gemaakt, vergelijkbaar met de volgende.

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

2. De gebruiker toegewezen beheerde identiteit toewijzen voor het gebruik van uw VMSS [az vmss-identiteit toewijzen](/cli/azure/vmss/identity). Vervang de parameterwaarden `<RESOURCE GROUP>` en `<VMSS NAME>` door uw eigen waarden. De `<USER ASSIGNED IDENTITY>` is de bron van de gebruiker toegewezen identiteit `name` eigenschap, zoals in de vorige stap hebt gemaakt:

    ```azurecli-interactive
    az vmss identity assign -g <RESOURCE GROUP> -n <VMSS NAME> --identities <USER ASSIGNED IDENTITY>
    ```

### <a name="remove-a-user-assigned-managed-identity-from-an-azure-virtual-machine-scale-set"></a>Een gebruiker toegewezen beheerde identiteit verwijderen uit een schaalset voor virtuele Azure-machine

Verwijderen van een gebruiker toegewezen beheerde identiteit van het gebruik van een virtuele machine scale set [az vmss-identiteit verwijderen](/cli/azure/vmss/identity#az-vmss-identity-remove). Als dit de enige gebruiker toegewezen identiteit toegewezen aan de virtuele-machineschaalset beheerd `UserAssigned` wordt verwijderd uit de waarde van het type identiteit.  Vervang de parameterwaarden `<RESOURCE GROUP>` en `<VMSS NAME>` door uw eigen waarden. De `<USER ASSIGNED IDENTITY>` is de gebruiker toegewezen beheerde identiteit van `name` eigenschap, die kan worden gevonden in de sectie van de identiteit van de VM-schaalset met behulp van `az vmss identity show`:

```azurecli-interactive
az vmss identity remove -g <RESOURCE GROUP> -n <VMSS NAME> --identities <USER ASSIGNED IDENTITY>
```

Als uw virtuele-machineschaalset beschikt niet over een systeem toegewezen beheerd wilt identiteits- en u alle beheerde identiteiten die door de gebruiker toegewezen van het apparaat verwijderen, gebruikt u de volgende opdracht uit:

> [!NOTE]
> De waarde `none` is hoofdlettergevoelig. Er moet een kleine letter.

```azurecli-interactive
az vmss update -n myVMSS -g myResourceGroup --set identity.type="none" identity.userAssignedIdentities=null
```

Als uw virtuele-machineschaalset beide door het systeem is toegewezen en gebruiker toegewezen beheerde identiteiten, kunt u alle gebruiker toegewezen identiteiten verwijderen door over te schakelen voor het gebruik van alleen het systeem toegewezen beheerde identiteit. Gebruik de volgende opdracht:

```azurecli-interactive
az vmss update -n myVMSS -g myResourceGroup --set identity.type='SystemAssigned' identity.userAssignedIdentities=null 
```

## <a name="next-steps"></a>Volgende stappen

- [Beheerde identiteiten voor een overzicht van Azure-resources](overview.md)
- Voor de volledige Azure virtuele-machineschaalset maken Quick Start, Zie: 

  - [Een virtuele-Machineschaalset maken met CLI](../../virtual-machines/linux/tutorial-create-vmss.md#create-a-scale-set)

















