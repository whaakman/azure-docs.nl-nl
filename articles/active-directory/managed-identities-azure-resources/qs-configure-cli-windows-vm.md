---
title: Het configureren van systeem en de gebruiker toegewezen beheerde identiteiten op een Azure-VM met behulp van Azure CLI
description: Voor stap door stap instructies voor het configureren van systeem en de gebruiker toegewezen beheerde identiteiten op een Azure-VM met behulp van Azure CLI.
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
ms.date: 09/14/2017
ms.author: daveba
ms.openlocfilehash: 094ee070a3eae9d102bd137e1a0ed7299b2b45a3
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/11/2018
ms.locfileid: "44345893"
---
# <a name="configure-managed-identities-for-azure-resources-on-an-azure-vm-using-azure-cli"></a>Configureren van beheerde identiteiten voor Azure-resources op een Azure-VM met behulp van Azure CLI

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Beheerde identiteiten voor Azure-resources biedt Azure-services met een automatisch beheerde identiteit in Azure Active Directory. U kunt deze identiteit gebruiken om te verifiëren bij een service die ondersteuning biedt voor Azure AD-verificatie, zonder referenties in uw code. 

In dit artikel, leert met behulp van de Azure CLI, u hoe u voor het uitvoeren van de volgende beheerde identiteiten voor bewerkingen van de Azure-resources op een Azure-VM:

- In- en uitschakelen van de door het systeem toegewezen beheerde identiteit op een Azure VM
- Toevoegen en verwijderen van een gebruiker toegewezen beheerde identiteit op een Azure VM

## <a name="prerequisites"></a>Vereiste onderdelen

- Als u niet bekend met beheerde identiteiten voor Azure-resources bent, lees de [overzichtssectie](overview.md). **Lees de [verschil tussen een beheerde identiteit door het systeem is toegewezen en de gebruiker toegewezen](overview.md#how-does-it-work)**.
- Als u nog geen Azure-account hebt, [registreer u dan voor een gratis account](https://azure.microsoft.com/free/) voordat u verdergaat.
- Als u wilt de beheerbewerkingen in dit artikel uitvoert, moet uw account de volgende Azure op basis van rollen access control-toewijzingen:
    > [!NOTE]
    > Er zijn geen extra Azure AD directory-roltoewijzingen is vereist.
    - [Inzender voor virtuele machines](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) aan een virtuele machine maken en inschakelen en het systeem en/of de gebruiker toegewezen beheerde identiteit verwijderen uit een Azure-VM.
    - [Beheerde identiteit Inzender](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rol te maken van een gebruiker toegewezen beheerde identiteit.
    - [Beheerde identiteit Operator](/azure/role-based-access-control/built-in-roles#managed-identity-operator) rol die u wilt toewijzen en verwijderen van een gebruiker toegewezen beheerde identiteit van en naar een virtuele machine.
- Als u wilt de CLI-scriptvoorbeelden uitvoeren, hebt u drie opties:
    - Gebruik [Azure Cloud Shell](../../cloud-shell/overview.md) vanuit Azure portal (Zie volgende sectie).
    - Gebruik de ingesloten Azure Cloud Shell via het 'Try It' de knop, zich in de rechterbovenhoek van elk codeblok.
    - [Installeer de nieuwste versie van Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) als u liever een lokale CLI-console.
      
      > [!NOTE]
      > De opdrachten zijn bijgewerkt in overeenstemming met de nieuwste versie van de [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli).     
        

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="system-assigned-managed-identity"></a>Het systeem toegewezen beheerde identiteit

In deze sectie leert u hoe u inschakelen en uitschakelen van de door het systeem toegewezen beheerde identiteit op een Azure-VM met behulp van Azure CLI.

### <a name="enable-system-assigned-managed-identity-during-creation-of-an-azure-vm"></a>Het systeem toegewezen beheerde identiteit inschakelen tijdens het maken van een Azure VM

Een Azure-VM maken met de door het systeem toegewezen beheerde identiteit ingeschakeld:

1. Als u de Azure CLI in een lokale console gebruikt, meldt u zich eerst aan bij Azure met [az login](/cli/azure/reference-index#az-login). Gebruik een account dat is gekoppeld aan het Azure-abonnement waaronder u de VM wilt implementeren:

   ```azurecli-interactive
   az login
   ```

2. Maak met [az group create](/cli/azure/group/#az-group-create) een [resourcegroep](../../azure-resource-manager/resource-group-overview.md#terminology) voor insluiting en implementatie van uw VM en de bijbehorende bronnen. U kunt deze stap overslaan als u al een resourcegroep hebt die u in plaats daarvan wilt gebruiken:

   ```azurecli-interactive 
   az group create --name myResourceGroup --location westus
   ```

3. Maak een VM met [az vm create](/cli/azure/vm/#az-vm-create). Het volgende voorbeeld wordt een virtuele machine met de naam *myVM* beheerd met een door het systeem toegewezen identiteit, zoals aangevraagd door de `--assign-identity` parameter. Met de parameters `--admin-username` en `--admin-password` worden de naam van de gebruiker met beheerdersrechten en het wachtwoord van het account voor aanmelding bij de virtuele machine opgegeven. Werk deze waarden bij met waarden die geschikt zijn voor uw omgeving: 

   ```azurecli-interactive 
   az vm create --resource-group myResourceGroup --name myVM --image win2016datacenter --generate-ssh-keys --assign-identity --admin-username azureuser --admin-password myPassword12
   ```

### <a name="enable-system-assigned-managed-identity-on-an-existing-azure-vm"></a>Het systeem toegewezen beheerde identiteit van een bestaande VM in Azure inschakelen

Als u nodig hebt om in te schakelen door het systeem toegewezen beheerde identiteit van de op een bestaande virtuele machine:

1. Als u de Azure CLI in een lokale console gebruikt, meldt u zich eerst aan bij Azure met [az login](/cli/azure/reference-index#az-login). Gebruik een account dat is gekoppeld aan het Azure-abonnement met de virtuele machine.

   ```azurecli-interactive
   az login
   ```

2. Gebruik [az vm identiteit toewijzen](/cli/azure/vm/identity/#az-vm-identity-assign) met de `identity assign` opdracht de identiteit van de door het systeem is toegewezen aan een bestaande virtuele machine inschakelen:

   ```azurecli-interactive
   az vm identity assign -g myResourceGroup -n myVm
   ```

### <a name="disable-system-assigned-identity-from-an-azure-vm"></a>Uitschakelen van het systeem toegewezen identiteit van een Azure VM

Als u een virtuele Machine die niet langer moet het systeem toegewezen identiteit, maar nog steeds moet gebruiker toegewezen identiteiten hebt, gebruikt u de volgende opdracht uit:

```azurecli-interactive
az vm update -n myVM -g myResourceGroup --set identity.type='UserAssigned' 
```

Als u een virtuele machine die door het systeem toegewezen identiteit niet meer nodig hebt en er geen gebruiker toegewezen identiteiten, gebruikt u de volgende opdracht uit:

> [!NOTE]
> De waarde `none` is hoofdlettergevoelig. Er moet een kleine letter. 

```azurecli-interactive
az vm update -n myVM -g myResourceGroup --set identity.type="none"
```

Verwijderen van de beheerde identiteit voor Azure-resources VM-extensie (gepland voor de afschaffing in januari 2019), gebruiker `-n ManagedIdentityExtensionForWindows` of `-n ManagedIdentityExtensionForLinux` overschakelen (afhankelijk van het type virtuele machine) met [az vm extension delete](https://docs.microsoft.com/cli/azure/vm/#assign-identity):

```azurecli-interactive
az vm identity --resource-group myResourceGroup --vm-name myVm -n ManagedIdentityExtensionForWindows
```

## <a name="user-assigned-managed-identity"></a>de gebruiker toegewezen beheerde identiteit

In deze sectie leert u hoe u kunt toevoegen en verwijderen van een gebruiker toegewezen beheerde identiteit van een Azure-VM met behulp van Azure CLI.

### <a name="assign-a-user-assigned-managed-identity-during-the-creation-of-an-azure-vm"></a>Een gebruiker toegewezen beheerde identiteit toewijzen tijdens het maken van een Azure-VM

In deze sectie helpt u bij het maken van een virtuele machine met de toewijzing van een gebruiker toegewezen beheerde identiteit. Als u al een virtuele machine die u wilt gebruiken, kunt u deze sectie overslaan en doorgaan met de volgende.

1. U kunt deze stap overslaan als u al een resourcegroep die u wilt gebruiken. Maak een [resourcegroep](~/articles/azure-resource-manager/resource-group-overview.md#terminology) voor insluiting en implementatie van de gebruiker toegewezen beheerde identiteit, met behulp van [az-groep maken](/cli/azure/group/#az-group-create). Vervang de parameterwaarden `<RESOURCE GROUP>` en `<LOCATION>` door uw eigen waarden. :

   ```azurecli-interactive 
   az group create --name <RESOURCE GROUP> --location <LOCATION>
   ```

2. Maak een beheerde identiteit gebruiker toegewezen met [az-identiteit maken](/cli/azure/identity#az-identity-create).  De `-g` parameter geeft u de resourcegroep waarin de gebruiker toegewezen beheerde identiteit wordt gemaakt, en de `-n` parameter geeft u de naam ervan.    
    
   [!INCLUDE [ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

   ```azurecli-interactive
   az identity create -g myResourceGroup -n myUserAssignedIdentity
   ```
   Het antwoord bevat details voor de gebruiker toegewezen beheerde identiteit gemaakt, vergelijkbaar met de volgende. De waarde van de resource-id toegewezen aan de gebruiker toegewezen beheerde identiteit wordt gebruikt in de volgende stap.

   ```json
   {
       "clientId": "73444643-8088-4d70-9532-c3a0fdc190fz",
       "clientSecretUrl": "https://control-westcentralus.identity.azure.net/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<myUserAssignedIdentity>/credentials?tid=5678&oid=9012&aid=73444643-8088-4d70-9532-c3a0fdc190fz",
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

3. Maak een VM met [az vm create](/cli/azure/vm/#az-vm-create). Het volgende voorbeeld wordt een virtuele machine die is gekoppeld aan de nieuwe gebruiker toegewezen identiteit, zoals opgegeven door de `--assign-identity` parameter. Vervang de parameterwaarden `<RESOURCE GROUP>`, `<VM NAME>`, `<USER NAME>`, `<PASSWORD>` en `<USER ASSIGNED IDENTITY NAME>` door uw eigen waarden. 

   ```azurecli-interactive 
   az vm create --resource-group <RESOURCE GROUP> --name <VM NAME> --image UbuntuLTS --admin-username <USER NAME> --admin-password <PASSWORD> --assign-identity <USER ASSIGNED IDENTITY NAME>
   ```

### <a name="assign-a-user-assigned-managed-identity-to-an-existing-azure-vm"></a>Een gebruiker toegewezen beheerde identiteit toewijzen aan een bestaande VM in Azure

1. Maak een door de gebruiker toegewezen identiteit met [az identity create](/cli/azure/identity#az-identity-create).  De `-g` parameter geeft u de resourcegroep waarin de gebruiker toegewezen identiteit wordt gemaakt, en de `-n` parameter geeft u de naam ervan. Vervang de parameterwaarden `<RESOURCE GROUP>` en `<USER ASSIGNED IDENTITY NAME>` door uw eigen waarden:

    > [!IMPORTANT]
    > Het maken van beheerde identiteiten gebruiker toegewezen met speciale tekens (dat wil zeggen onderstrepingstekens) in de naam is momenteel niet ondersteund. Gebruik alfanumerieke tekens. Controleer later op updates.  Zie voor meer informatie [Veelgestelde vragen en bekende problemen](known-issues.md)

    ```azurecli-interactive
    az identity create -g <RESOURCE GROUP> -n <USER ASSIGNED IDENTITY NAME>
    ```
   Het antwoord bevat details voor de gebruiker toegewezen beheerde identiteit gemaakt, vergelijkbaar met de volgende. 

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

2. De gebruiker toegewezen identiteit voor het gebruik van uw virtuele machine toewijzen [az vm identiteit toewijzen](/cli/azure/vm#az-vm-identity-assign). Vervang de parameterwaarden `<RESOURCE GROUP>` en `<VM NAME>` door uw eigen waarden. De `<USER ASSIGNED IDENTITY NAME>` is van de gebruiker toegewezen beheerde identiteit van resource `name` eigenschap, zoals in de vorige stap hebt gemaakt:

    ```azurecli-interactive
    az vm identity assign -g <RESOURCE GROUP> -n <VM NAME> --identities <USER ASSIGNED IDENTITY>
    ```

### <a name="remove-a-user-assigned-managed-identity-from-an-azure-vm"></a>Verwijderen van een gebruiker toegewezen beheerde identiteit van een Azure VM

Een gebruiker toegewezen beheerde identiteit verwijderen uit een VM-gebruik [verwijderen van de identiteit van de az vm](/cli/azure/vm#az-vm-identity-remove). Als dit de enige gebruiker toegewezen identiteit toegewezen aan de virtuele machine, beheerde `UserAssigned` wordt verwijderd uit de waarde van het type identiteit.  Vervang de parameterwaarden `<RESOURCE GROUP>` en `<VM NAME>` door uw eigen waarden. De `<USER ASSIGNED IDENTITY>` is van de gebruiker toegewezen identiteit `name` eigenschap, die kan worden gevonden in de sectie van de identiteit van de virtuele machine met `az vm identity show`:

```azurecli-interactive
az vm identity remove -g <RESOURCE GROUP> -n <VM NAME> --identities <USER ASSIGNED IDENTITY>
```

Als uw virtuele machine heeft geen een beheerde identiteit voor het systeem is toegewezen en u wilt alle gebruiker toegewezen identiteiten van het apparaat verwijderen, gebruikt u de volgende opdracht uit:

> [!NOTE]
> De waarde `none` is hoofdlettergevoelig. Er moet een kleine letter.

```azurecli-interactive
az vm update -n myVM -g myResourceGroup --set identity.type="none" identity.userAssignedIdentities=null
```

Als uw virtuele machine is zowel systeem toegewezen en gebruiker toegewezen identiteiten, kunt u alle gebruiker toegewezen identiteiten verwijderen door over te schakelen voor het gebruik van alleen het systeem is toegewezen. Gebruik de volgende opdracht:

```azurecli-interactive
az vm update -n myVM -g myResourceGroup --set identity.type='SystemAssigned' identity.userAssignedIdentities=null 
```

## <a name="next-steps"></a>Volgende stappen
- [Beheerde identiteiten voor een overzicht van Azure-resources](overview.md)
- Zie voor het volledige Azure-VM maken snelstartgidsen: 
  - [Een Windows-machine maken met CLI](../../virtual-machines/windows/quick-create-cli.md)  
  - [Een virtuele Linux-machine maken met CLI](../../virtual-machines/linux/quick-create-cli.md) 

















