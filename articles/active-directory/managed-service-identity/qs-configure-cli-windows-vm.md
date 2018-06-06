---
title: Het configureren van systeem en de gebruiker toegewezen identiteiten op een virtuele machine in Azure met Azure CLI
description: Stap voor stap toegewezen instructies voor het configureren van een systeem- en identiteiten op een virtuele machine van Azure met Azure CLI.
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
ms.date: 09/14/2017
ms.author: daveba
ms.openlocfilehash: 09ee4dfc403bf570631f64b0b13d1592a03eed17
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34698965"
---
# <a name="configure-managed-service-identity-msi-on-an-azure-vm-using-azure-cli"></a>Managed Service identiteit (MSI) configureren op een virtuele machine in Azure met Azure CLI

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Beheerde Service-identiteit biedt Azure-services met een automatisch beheerde identiteit in Azure Active Directory. U kunt deze identiteit gebruiken om alle services die Azure AD-verificatie ondersteunt, zonder referenties in uw code te verifiëren. 

In dit artikel leert u hoe op de volgende Service-identiteit beheerd bewerkingen uitvoeren op een virtuele machine van Azure, met de Azure CLI:
- Inschakelen en uitschakelen van het systeem toegewezen identiteit op een virtuele machine in Azure
- Toevoegen en verwijderen van een gebruiker met de identiteit op een virtuele machine in Azure

## <a name="prerequisites"></a>Vereisten

- Als u niet bekend met de Service-identiteit beheerd bent, bekijk de [overzichtssectie van](overview.md). **Lees de [verschil tussen een systeem dat is toegewezen en de gebruiker toegewezen identiteit](overview.md#how-does-it-work)**.
- Als u al een Azure-account niet hebt [aanmelden voor een gratis account](https://azure.microsoft.com/free/) voordat u doorgaat.
- De CLI script als voorbeelden wilt uitvoeren, hebt u drie opties:

    - Gebruik [Azure Cloud Shell](../../cloud-shell/overview.md) vanuit de Azure-portal (Zie volgende sectie).
    - Gebruik de ingesloten Azure Cloud Shell via de 'Deze probeer' knop, zich in de rechterbovenhoek van elk codeblok.
    - [Installeer de nieuwste versie van CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.13 of hoger) als u liever een lokale CLI-console te gebruiken. 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="system-assigned-identity"></a>Automatisch toegewezen identiteit

Informatie over het inschakelen en uitschakelen van het systeem toegewezen identiteit op een virtuele machine in Azure met Azure CLI in deze sectie.

### <a name="enable-system-assigned-identity-during-creation-of-an-azure-vm"></a>Automatisch toegewezen identiteit bij het maken van een virtuele machine van Azure inschakelen

Als u een virtuele machine in Azure maken met het systeem toegewezen identiteit ingeschakeld:

1. Als u de Azure CLI in een lokale console, eerst aanmelden bij het gebruik van Azure [az aanmelding](/cli/azure/reference-index#az_login). Gebruik een account dat is gekoppeld aan het Azure-abonnement waarmee u wilt implementeren, de virtuele machine:

   ```azurecli-interactive
   az login
   ```

2. Maak een [resourcegroep](../../azure-resource-manager/resource-group-overview.md#terminology) voor containment en implementatie van uw virtuele machine en de bijbehorende resources, met behulp van [az groep maken](/cli/azure/group/#az_group_create). U kunt deze stap overslaan als u al resourcegroep die u wilt gebruiken in plaats daarvan:

   ```azurecli-interactive 
   az group create --name myResourceGroup --location westus
   ```

3. Maak een VM met [az vm maken](/cli/azure/vm/#az_vm_create). Het volgende voorbeeld wordt een virtuele machine met de naam *myVM* automatisch toegewezen identiteit, zoals aangevraagd door de `--assign-identity` parameter. De `--admin-username` en `--admin-password` parameters geeft u het account voor gebruiker met beheerdersrechten en het wachtwoord voor aanmelding bij de virtuele machine. Bijwerken van deze waarden naargelang nodig is voor uw omgeving: 

   ```azurecli-interactive 
   az vm create --resource-group myResourceGroup --name myVM --image win2016datacenter --generate-ssh-keys --assign-identity --admin-username azureuser --admin-password myPassword12
   ```

### <a name="enable-system-assigned-identity-on-an-existing-azure-vm"></a>Automatisch toegewezen identiteit op een bestaande virtuele machine van Azure inschakelen

Als u de identiteit van de automatisch toegewezen op een bestaande virtuele machine inschakelen moet:

1. Als u de Azure CLI in een lokale console, eerst aanmelden bij het gebruik van Azure [az aanmelding](/cli/azure/reference-index#az_login). Gebruik een account dat is gekoppeld aan het Azure-abonnement met de virtuele machine. Controleer ook of uw account behoort tot een functie waarmee u schrijfmachtigingen heeft op de virtuele machine, zoals 'Virtual Machine Contributor':

   ```azurecli-interactive
   az login
   ```

2. Gebruik [toewijzen van de identiteit van de vm az](/cli/azure/vm/identity/#az_vm_identity_assign) met de `identity assign` inschakelen met de identiteit van de automatisch toegewezen aan een bestaande VM opdracht:

   ```azurecli-interactive
   az vm identity assign -g myResourceGroup -n myVm
   ```

### <a name="disable-the-system-assigned-identity-from-an-azure-vm"></a>Het systeem toegewezen identiteit van een Azure-virtuele machine uitschakelen

> [!NOTE]
> Het uitschakelen van de Service-identiteit beheerd vanaf een virtuele Machine is momenteel niet ondersteund. In de tussentijd kunt u schakelen tussen het gebruik van het systeem toegewezen en toegewezen gebruikersidentiteiten.

Als u een virtuele Machine die niet langer nodig heeft het systeem toegewezen identiteit maar nog steeds moet identiteiten toegewezen door gebruiker hebt, gebruikt u de volgende opdracht:

```azurecli-interactive
az vm update -n myVM -g myResourceGroup --set identity.type='UserAssigned' 
```
Verwijderen van de MSI-VM-extensie, gebruiker `-n ManagedIdentityExtensionForWindows` of `-n ManagedIdentityExtensionForLinux` overschakelen (afhankelijk van het type van de virtuele machine) met [az vm-extensie verwijderen](https://docs.microsoft.com/cli/azure/vm/#assign-identity):

```azurecli-interactive
az vm identity --resource-group myResourceGroup --vm-name myVm -n ManagedIdentityExtensionForWindows
```

## <a name="user-assigned-identity"></a>Identiteit van de toegewezen gebruiker

In deze sectie leert u hoe toevoegen en verwijderen van een gebruiker met de identiteit van een virtuele machine van Azure, met Azure CLI.

### <a name="assign-a-user-assigned-identity-during-the-creation-of-an-azure-vm"></a>Een gebruiker toegewezen identiteit tijdens het maken van een Azure VM toewijzen

Deze sectie helpt u bij het maken van een virtuele machine met de toewijzing van een gebruiker toegewezen identiteit. Als u al een virtuele machine die u wilt gebruiken, kunt u deze sectie overslaan en doorgaan met de volgende.

1. U kunt deze stap overslaan als u al een resourcegroep die u wilt gebruiken. Maak een [resourcegroep](~/articles/azure-resource-manager/resource-group-overview.md#terminology) voor de implementatie van uw MSI en containment met [az groep maken](/cli/azure/group/#az_group_create). Zorg ervoor dat u de `<RESOURCE GROUP>` en `<LOCATION>` parameterwaarden met uw eigen waarden. :

   ```azurecli-interactive 
   az group create --name <RESOURCE GROUP> --location <LOCATION>
   ```

2. Maken van een gebruiker toegewezen identiteit met [az identiteit maken](/cli/azure/identity#az_identity_create).  De `-g` parameter geeft u de resourcegroep waar de gebruiker toegewezen identiteit is gemaakt, en de `-n` parameter geeft u de naam ervan.    
    
[!INCLUDE[ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]


```azurecli-interactive
az identity create -g myResourceGroup -n myUserAssignedIdentity
```
Het antwoord bevat de details voor de gebruiker toegewezen identiteit gemaakt, ziet er als volgt. De waarde van de resource-id toegewezen aan de gebruiker toegewezen identiteit wordt gebruikt in de volgende stap.

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

3. Maak een VM met [az vm maken](/cli/azure/vm/#az_vm_create). Het volgende voorbeeld wordt een virtuele machine die is gekoppeld aan de nieuwe gebruiker toegewezen identiteit, zoals opgegeven door de `--assign-identity` parameter. Zorg ervoor dat u de `<RESOURCE GROUP>`, `<VM NAME>`, `<USER NAME>`, `<PASSWORD>`, en `<MSI ID>` parameterwaarden met uw eigen waarden. Voor `<MSI ID>`, gebruik van de identiteit van de gebruiker toegewezen resource `id` eigenschap in de vorige stap hebt gemaakt: 

```azurecli-interactive 
az vm create --resource-group <RESOURCE GROUP> --name <VM NAME> --image UbuntuLTS --admin-username <USER NAME> --admin-password <PASSWORD> --assign-identity <MSI ID>
```

### <a name="assign-a-user-assigned-identity-to-an-existing-azure-vm"></a>Toewijzen van een gebruiker identiteit toegewezen aan een bestaande virtuele machine in Azure

1. Maken van een gebruiker toegewezen identiteit met [az identiteit maken](/cli/azure/identity#az-identity-create).  De `-g` parameter geeft u de resourcegroep waar de gebruiker toegewezen identiteit is gemaakt, en de `-n` parameter geeft u de naam ervan. Zorg ervoor dat u de `<RESOURCE GROUP>` en `<MSI NAME>` parameterwaarden met uw eigen waarden:

    > [!IMPORTANT]
    > Toegewezen identiteiten met speciale tekens (dat wil zeggen liggend streepje) in de naam van de gebruiker maken wordt momenteel niet ondersteund. Gebruik alfanumerieke tekens. Controleer regelmatig op updates.  Zie voor meer informatie [Veelgestelde vragen en bekende problemen](known-issues.md)

    ```azurecli-interactive
    az identity create -g <RESOURCE GROUP> -n <MSI NAME>
    ```
Het antwoord bevat de details voor de gebruiker die is gemaakt, vergelijkbaar met de volgende MSI toegewezen. De resource `id` waarde wordt toegewezen aan de toegewezen gebruikers-id wordt gebruikt in de volgende stap.

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

2. De identiteit van de gebruiker toegewezen aan uw virtuele machine met toewijzen [toewijzen van de identiteit van de vm az](/cli/azure/vm#az-vm-identity-assign). Zorg ervoor dat u de `<RESOURCE GROUP>` en `<VM NAME>` parameterwaarden met uw eigen waarden. De `<MSI ID>` zijn van de identiteit van de gebruiker toegewezen resource `id` eigenschap, in de vorige stap hebt gemaakt:

    ```azurecli-interactive
    az vm identity assign -g <RESOURCE GROUP> -n <VM NAME> --identities <MSI ID>
    ```

### <a name="remove-a-user-assigned-identity-from-an-azure-vm"></a>Verwijderen van een gebruiker met de identiteit van een Azure VM

> [!NOTE]
> Alle toegewezen gebruikersidentiteiten verwijderen uit een virtuele Machine is momenteel niet ondersteund, tenzij u een systeem toegewezen identiteit hebt.

Als uw virtuele machine meerdere identiteiten van de gebruiker die is toegewezen heeft, kunt u alles behalve het laatste teken met behulp van [verwijderen van de identiteit van de vm az](/cli/azure/vm#az-vm-identity-remove). Zorg ervoor dat u de `<RESOURCE GROUP>` en `<VM NAME>` parameterwaarden met uw eigen waarden. De `<MSI NAME>` zijn van de identiteit van de gebruiker toegewezen `name` -eigenschap die u door in de sectie van de identiteit van het gebruik van de VM vinden kunt `az vm show`:

```azurecli-interactive
az vm identity remove -g <RESOURCE GROUP> -n <VM NAME> --identities <MSI NAME>
```
Als uw virtuele machine heeft zowel automatisch toegewezen als de gebruiker toegewezen identiteiten, kunt u alle gebruikers identiteiten toegewezen door over te schakelen voor het gebruik van alleen automatisch toegewezen. Gebruik de volgende opdracht:

```azurecli-interactive
az vm update -n myVM -g myResourceGroup --set identity.type='SystemAssigned' identity.identityIds=null 
```

## <a name="related-content"></a>Gerelateerde inhoud
- [Overzicht van beheerde Service-identiteit](overview.md)
- Voor het volledige virtuele machine in Azure maken snelstartgidsen, Zie: 
  - [Een Windows-machine maken met CLI](../../virtual-machines/windows/quick-create-cli.md)  
  - [Een virtuele Linux-machine maken met CLI](../../virtual-machines/linux/quick-create-cli.md) 

















