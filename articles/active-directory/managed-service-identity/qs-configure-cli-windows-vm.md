---
title: Het configureren van systeem en de gebruiker toegewezen identiteiten op een Azure-VM met behulp van Azure CLI
description: Stap voor stap instructies voor het configureren van een systeem- en aan toegewezen identiteiten op een Azure-VM met behulp van Azure CLI.
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
ms.openlocfilehash: db32f56c55f189001e51a727ca6b5703e82dafe4
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/07/2018
ms.locfileid: "37903994"
---
# <a name="configure-managed-service-identity-msi-on-an-azure-vm-using-azure-cli"></a>Managed Service Identity (MSI) configureren op een Azure-VM met behulp van Azure CLI

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Beheerde Service-identiteit biedt Azure-services met een automatisch beheerde identiteit in Azure Active Directory. U kunt deze identiteit gebruiken om te verifiëren bij een service die ondersteuning biedt voor Azure AD-verificatie, zonder referenties in uw code. 

In dit artikel leert u hoe u de volgende bewerkingen van de beheerde Service-identiteit op een Azure-VM, met de Azure CLI uitvoeren:
- In- en uitschakelen van het systeem toegewezen identiteit op een Azure VM
- Toevoegen en verwijderen van een gebruiker toegewezen identiteit op een Azure VM

## <a name="prerequisites"></a>Vereisten

- Als u niet bekend met beheerde Service-identiteit bent, bekijk dan de [overzichtssectie](overview.md). **Lees de [verschil tussen een systeem toegewezen en een gebruiker toegewezen identiteit](overview.md#how-does-it-work)**.
- Als u nog een Azure-account hebt [zich registreren voor een gratis account](https://azure.microsoft.com/free/) voordat u doorgaat.
- Als u wilt de CLI-scriptvoorbeelden uitvoeren, hebt u drie opties:

    - Gebruik [Azure Cloud Shell](../../cloud-shell/overview.md) vanuit Azure portal (Zie volgende sectie).
    - Gebruik de ingesloten Azure Cloud Shell via het 'Try It' de knop, zich in de rechterbovenhoek van elk codeblok.
    - [Installeer de nieuwste versie van CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.13 of hoger) als u liever een lokale CLI-console. 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="system-assigned-identity"></a>Systeem toegewezen identiteit

In deze sectie leert u hoe u inschakelen en uitschakelen van het systeem toegewezen identiteit op een Azure-VM met behulp van Azure CLI.

### <a name="enable-system-assigned-identity-during-creation-of-an-azure-vm"></a>Systeem toegewezen identiteit tijdens het maken van een virtuele Azure-machine inschakelen

Voor het maken van een Azure-VM met het systeem toegewezen identiteit ingeschakeld:

1. Als u de Azure CLI in een lokale console gebruikt, eerst aanmelden bij Azure met [az login](/cli/azure/reference-index#az_login). Gebruik een account dat is gekoppeld aan het Azure-abonnement waarin u wilt de virtuele machine implementeren:

   ```azurecli-interactive
   az login
   ```

2. Maak een [resourcegroep](../../azure-resource-manager/resource-group-overview.md#terminology) voor insluiting en implementatie van de virtuele machine en de bijbehorende resources, met behulp van [az-groep maken](/cli/azure/group/#az_group_create). U kunt deze stap overslaan als u al resourcegroep die u wilt gebruiken in plaats daarvan:

   ```azurecli-interactive 
   az group create --name myResourceGroup --location westus
   ```

3. Maak een VM met [az vm maken](/cli/azure/vm/#az_vm_create). Het volgende voorbeeld wordt een virtuele machine met de naam *myVM* met een systeem toegewezen identiteit, zoals aangevraagd door de `--assign-identity` parameter. De `--admin-username` en `--admin-password` parameters geeft u het account voor gebruiker met beheerdersrechten en het wachtwoord voor aanmelding bij de virtuele machine. Deze waarden als geschikt is voor uw omgeving bijwerken: 

   ```azurecli-interactive 
   az vm create --resource-group myResourceGroup --name myVM --image win2016datacenter --generate-ssh-keys --assign-identity --admin-username azureuser --admin-password myPassword12
   ```

### <a name="enable-system-assigned-identity-on-an-existing-azure-vm"></a>Systeem toegewezen identiteit van een bestaande VM in Azure inschakelen

Als u nodig hebt om in te schakelen van het systeem toegewezen identiteit van een bestaande VM:

1. Als u de Azure CLI in een lokale console gebruikt, eerst aanmelden bij Azure met [az login](/cli/azure/reference-index#az_login). Gebruik een account dat is gekoppeld aan het Azure-abonnement met de virtuele machine. Ook voor zorgen dat uw account deel uitmaakt van een rol waarmee u beschikt over machtigingen voor schrijven op de virtuele machine, zoals 'Inzender voor virtuele machines':

   ```azurecli-interactive
   az login
   ```

2. Gebruik [az vm identiteit toewijzen](/cli/azure/vm/identity/#az_vm_identity_assign) met de `identity assign` opdracht inschakelen in het systeem toegewezen identiteit aan een bestaande virtuele machine:

   ```azurecli-interactive
   az vm identity assign -g myResourceGroup -n myVm
   ```

### <a name="disable-the-system-assigned-identity-from-an-azure-vm"></a>Uitschakelen van het systeem toegewezen identiteit van een Azure VM

> [!NOTE]
> Uitschakelen van de beheerde Service-identiteit van een virtuele Machine wordt momenteel niet ondersteund. In de tussentijd kunt u overschakelen tussen het gebruik van het systeem toegewezen en de gebruiker toegewezen identiteiten.

Als u een virtuele Machine die niet meer nodig is op het systeem toegewezen identiteit maar nog steeds moet gebruiker toegewezen identiteiten hebt, gebruikt u de volgende opdracht uit:

```azurecli-interactive
az vm update -n myVM -g myResourceGroup --set identity.type='UserAssigned' 
```
Verwijderen van de MSI-VM-extensie, gebruiker `-n ManagedIdentityExtensionForWindows` of `-n ManagedIdentityExtensionForLinux` overschakelen (afhankelijk van het type virtuele machine) met [az vm extension delete](https://docs.microsoft.com/cli/azure/vm/#assign-identity):

```azurecli-interactive
az vm identity --resource-group myResourceGroup --vm-name myVm -n ManagedIdentityExtensionForWindows
```

## <a name="user-assigned-identity"></a>Gebruiker toegewezen identiteit

In deze sectie leert u hoe u kunt toevoegen en verwijderen van een gebruiker toegewezen identiteit van een Azure-VM, met behulp van Azure CLI.

### <a name="assign-a-user-assigned-identity-during-the-creation-of-an-azure-vm"></a>Toewijzen van een gebruiker toegewezen identiteit tijdens het maken van een Azure-VM

In deze sectie helpt u bij het maken van een virtuele machine met de toewijzing van een gebruiker toegewezen identiteit. Als u al een virtuele machine die u wilt gebruiken, kunt u deze sectie overslaan en doorgaan met de volgende.

1. U kunt deze stap overslaan als u al een resourcegroep die u wilt gebruiken. Maak een [resourcegroep](~/articles/azure-resource-manager/resource-group-overview.md#terminology) voor insluiting en implementatie van de MSI-bestand, met behulp van [az-groep maken](/cli/azure/group/#az_group_create). Vervang de `<RESOURCE GROUP>` en `<LOCATION>` parameterwaarden door uw eigen waarden. :

   ```azurecli-interactive 
   az group create --name <RESOURCE GROUP> --location <LOCATION>
   ```

2. Een gebruiker toegewezen identiteit maken [az-identiteit maken](/cli/azure/identity#az_identity_create).  De `-g` parameter geeft u de resourcegroep waar de gebruiker toegewezen identiteit is gemaakt, en de `-n` parameter geeft u de naam ervan.    
    
[!INCLUDE[ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]


```azurecli-interactive
az identity create -g myResourceGroup -n myUserAssignedIdentity
```
Het antwoord bevat details voor de gebruiker toegewezen identiteit gemaakt, vergelijkbaar met de volgende. De waarde van de resource-id toegewezen aan de gebruiker toegewezen identiteit wordt gebruikt in de volgende stap.

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

3. Maak een VM met [az vm maken](/cli/azure/vm/#az_vm_create). Het volgende voorbeeld wordt een virtuele machine die is gekoppeld aan de nieuwe gebruiker toegewezen identiteit, zoals opgegeven door de `--assign-identity` parameter. Vervang de `<RESOURCE GROUP>`, `<VM NAME>`, `<USER NAME>`, `<PASSWORD>`, en `<MSI ID>` parameterwaarden door uw eigen waarden. Voor `<MSI ID>`, gebruik van de identiteit van de gebruiker toegewezen resource `id` eigenschap in de vorige stap hebt gemaakt: 

```azurecli-interactive 
az vm create --resource-group <RESOURCE GROUP> --name <VM NAME> --image UbuntuLTS --admin-username <USER NAME> --admin-password <PASSWORD> --assign-identity <MSI ID>
```

### <a name="assign-a-user-assigned-identity-to-an-existing-azure-vm"></a>Een gebruiker toegewezen identiteit aan een bestaande VM in Azure toewijzen

1. Een gebruiker toegewezen identiteit maken [az-identiteit maken](/cli/azure/identity#az-identity-create).  De `-g` parameter geeft u de resourcegroep waar de gebruiker toegewezen identiteit is gemaakt, en de `-n` parameter geeft u de naam ervan. Vervang de `<RESOURCE GROUP>` en `<MSI NAME>` parameterwaarden door uw eigen waarden:

    > [!IMPORTANT]
    > Het maken van de gebruiker toegewezen identiteiten met speciale tekens (dat wil zeggen onderstrepingstekens) in de naam is momenteel niet ondersteund. Gebruik alfanumerieke tekens. Kijk binnenkort voor updates.  Zie voor meer informatie [Veelgestelde vragen en bekende problemen](known-issues.md)

    ```azurecli-interactive
    az identity create -g <RESOURCE GROUP> -n <MSI NAME>
    ```
Het antwoord bevat details voor de gebruiker toegewezen MSI-bestand gemaakt, vergelijkbaar met de volgende. De resource `id` waarde die is toegewezen aan de gebruiker toegewezen identiteit wordt gebruikt in de volgende stap.

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

2. De identiteit van de gebruiker toegewezen toewijzen aan uw virtuele machine met [az vm identiteit toewijzen](/cli/azure/vm#az-vm-identity-assign). Vervang de `<RESOURCE GROUP>` en `<VM NAME>` parameterwaarden door uw eigen waarden. De `<MSI ID>` is van de identiteit van de gebruiker toegewezen resource `id` eigenschap, zoals in de vorige stap hebt gemaakt:

    ```azurecli-interactive
    az vm identity assign -g <RESOURCE GROUP> -n <VM NAME> --identities <MSI ID>
    ```

### <a name="remove-a-user-assigned-identity-from-an-azure-vm"></a>Een gebruiker toegewezen identiteit van een Azure VM verwijderen

> [!NOTE]
> Alle gebruiker toegewezen identiteiten verwijderen uit een virtuele Machine wordt momenteel niet ondersteund, tenzij er een systeem toegewezen identiteit.

Als uw virtuele machine meerdere gebruiker toegewezen identiteiten, kunt u alles behalve de laatste via [verwijderen van de identiteit van de az vm](/cli/azure/vm#az-vm-identity-remove). Vervang de `<RESOURCE GROUP>` en `<VM NAME>` parameterwaarden door uw eigen waarden. De `<MSI NAME>` is van de gebruiker toegewezen identiteit `name` eigenschap, die kan worden gevonden door in de sectie van de identiteit van het gebruik van de virtuele machine `az vm show`:

```azurecli-interactive
az vm identity remove -g <RESOURCE GROUP> -n <VM NAME> --identities <MSI NAME>
```
Als uw virtuele machine is zowel de systeem toegewezen als de gebruiker toegewezen identiteiten, kunt u alle gebruiker toegewezen identiteiten door over te schakelen voor het gebruik van alleen systeem toegewezen verwijderen. Gebruik de volgende opdracht:

```azurecli-interactive
az vm update -n myVM -g myResourceGroup --set identity.type='SystemAssigned' identity.identityIds=null 
```

## <a name="related-content"></a>Gerelateerde inhoud
- [Overzicht van de beheerde Service-identiteit](overview.md)
- Zie voor het volledige Azure-VM maken snelstartgidsen: 
  - [Een Windows-machine maken met CLI](../../virtual-machines/windows/quick-create-cli.md)  
  - [Een virtuele Linux-machine maken met CLI](../../virtual-machines/linux/quick-create-cli.md) 

















