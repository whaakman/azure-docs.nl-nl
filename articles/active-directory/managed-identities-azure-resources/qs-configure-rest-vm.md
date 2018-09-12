---
title: Het configureren van systeem en de gebruiker toegewezen beheerde identiteiten op een Azure-VM met behulp van REST
description: Stapsgewijze instructies voor het configureren van een systeem en de gebruiker toegewezen beheerde identiteiten op een Azure-VM met CURL om de REST-API-aanroepen.
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
ms.date: 06/25/2018
ms.author: daveba
ms.openlocfilehash: 45f322f46ec26da9304acfad1c96a3978cac9cb7
ms.sourcegitcommit: 5a9be113868c29ec9e81fd3549c54a71db3cec31
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/11/2018
ms.locfileid: "44378789"
---
# <a name="configure-managed-identities-for-azure-resources-on-an-azure-vm-using-rest-api-calls"></a>Configureren van beheerde identiteiten voor een Azure-resources op een Azure-VM met behulp van REST-API aanroepen

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Beheerde identiteiten voor Azure-resources biedt Azure-services met een automatisch beheerde identiteit in Azure Active Directory. U kunt deze identiteit gebruiken om te verifiëren bij een service die ondersteuning biedt voor Azure AD-verificatie, zonder referenties in uw code. 

In dit artikel leert met CURL om aanroepen naar het Azure Resource Manager REST-eindpunt, u hoe u de volgende beheerde identiteiten voor bewerkingen van de Azure-resources op een Azure-VM uitvoeren:

- In- en uitschakelen van de door het systeem toegewezen beheerde identiteit op een Azure VM
- Toevoegen en verwijderen van een gebruiker toegewezen beheerde identiteit op een Azure VM

## <a name="prerequisites"></a>Vereisten

- Als u niet bekend met beheerde identiteiten voor Azure-resources bent, lees de [overzichtssectie](overview.md). **Lees de [verschil tussen een beheerde identiteit door het systeem is toegewezen en de gebruiker toegewezen](overview.md#how-does-it-work)**.
- Als u nog geen Azure-account hebt, [registreer u dan voor een gratis account](https://azure.microsoft.com/free/) voordat u verdergaat.
- Als u wilt de beheerbewerkingen in dit artikel uitvoert, moet uw account de volgende Azure op basis van rollen access control-toewijzingen:

    > [!NOTE]
    > Er zijn geen extra Azure AD directory-roltoewijzingen is vereist.

    - [Inzender voor virtuele machines](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) aan een virtuele machine maken en inschakelen en het systeem en/of de gebruiker toegewezen beheerde identiteit verwijderen uit een Azure-VM.
    - [Beheerde identiteit Inzender](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rol te maken van een gebruiker toegewezen identiteit.
    - [Beheerde identiteit Operator](/azure/role-based-access-control/built-in-roles#managed-identity-operator) rol die u wilt toewijzen en verwijderen van een gebruiker toegewezen beheerde identiteit van en naar een virtuele machine.
- Als u Windows gebruikt, installeert u de [Windows-subsysteem voor Linux](https://msdn.microsoft.com/commandline/wsl/about) of gebruik de [Azure Cloud Shell](../../cloud-shell/overview.md) in Azure portal.
- [Installeer de Azure CLI lokale console](/cli/azure/install-azure-cli), als u de [Windows-subsysteem voor Linux](https://msdn.microsoft.com/commandline/wsl/about) of een [Linux-distributie](/cli/azure/install-azure-cli-apt?view=azure-cli-latest).
- Als u van Azure CLI lokale console gebruikmaakt, aanmelden bij Azure met `az login` met een account dat is gekoppeld aan het Azure-abonnement u wilt beheren, systeem- of beheerde identiteiten gebruiker toegewezen.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="system-assigned-managed-identity"></a>Het systeem toegewezen beheerde identiteit

In deze sectie leert u hoe u inschakelen en uitschakelen van het systeem toegewezen beheerde identiteit op een Azure-VM met CURL aanroepen met de Azure Resource Manager REST-eindpunt.

### <a name="enable-system-assigned-managed-identity-during-creation-of-an-azure-vm"></a>Het systeem toegewezen beheerde identiteit inschakelen tijdens het maken van een Azure VM

Voor het maken van een Azure-VM met beheerde identiteit systeem toegewezen ingeschakeld, moet u een virtuele machine maken en ophalen van een toegangstoken om te gebruiken we CURL om aan te roepen van het Resource Manager-eindpunt met de waarde van het type beheerde identiteit door het systeem is toegewezen.

1. Maak met [az group create](/cli/azure/group/#az-group-create) een [resourcegroep](../../azure-resource-manager/resource-group-overview.md#terminology) voor insluiting en implementatie van uw VM en de bijbehorende bronnen. U kunt deze stap overslaan als u al een resourcegroep hebt die u in plaats daarvan wilt gebruiken:

   ```azurecli-interactive 
   az group create --name myResourceGroup --location westus
   ```

2. Maak een [netwerkinterface](/cli/azure/network/nic?view=azure-cli-latest#az-network-nic-create) voor uw virtuele machine:

   ```azurecli-interactive
    az network nic create -g myResourceGroup --vnet-name myVnet --subnet mySubnet -n myNic
   ```

3.  Een Bearer-toegangstoken, waarmee u uw virtuele machine maken met een door het systeem toegewezen beheerde identiteit gebruikt in de volgende stap in de autorisatie-header ophalen.

   ```azurecli-interactive
   az account get-access-token
   ``` 

4. Een virtuele machine met CURL om aan te roepen van het Azure Resource Manager REST-eindpunt maken. Het volgende voorbeeld wordt een virtuele machine met de naam *myVM* beheerd met een door het systeem toegewezen identiteit, zoals vermeld in de hoofdtekst van de aanvraag door de waarde `"identity":{"type":"SystemAssigned"}`. Vervang `<ACCESS TOKEN>` met de waarde u hebt ontvangen in de vorige stap wanneer u een Bearer-toegangstoken aangevraagd en de `<SUBSCRIPTION ID>` waarde als geschikt is voor uw omgeving.
 
    ```bash
    curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2018-06-01' -X PUT -d '{"location":"westus","name":"myVM","identity":{"type":"SystemAssigned"},"properties":{"hardwareProfile":{"vmSize":"Standard_D2_v2"},"storageProfile":{"imageReference":{"sku":"2016-Datacenter","publisher":"MicrosoftWindowsServer","version":"latest","offer":"WindowsServer"},"osDisk":{"caching":"ReadWrite","managedDisk":{"storageAccountType":"Standard_LRS"},"name":"myVM3osdisk","createOption":"FromImage"},"dataDisks":[{"diskSizeGB":1023,"createOption":"Empty","lun":0},{"diskSizeGB":1023,"createOption":"Empty","lun":1}]},"osProfile":{"adminUsername":"azureuser","computerName":"myVM","adminPassword":"myPassword12"},"networkProfile":{"networkInterfaces":[{"id":"/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic","properties":{"primary":true}}]}}}' -H "Content-Type: application/json" -H "Authorization: Bearer <ACCESS TOKEN>"
    ```

### <a name="enable-system-assigned-identity-on-an-existing-azure-vm"></a>Het systeem toegewezen identiteit van een bestaande VM in Azure inschakelen

Om in te schakelen door het systeem toegewezen identiteit op een bestaande virtuele machine, moet u een toegangstoken verkrijgen en vervolgens gebruiken we CURL om aan te roepen van het Resource Manager REST-eindpunt voor het bijwerken van de id-type.

1. Een Bearer-toegangstoken, waarmee u uw virtuele machine maken met een door het systeem toegewezen beheerde identiteit gebruikt in de volgende stap in de autorisatie-header ophalen.

   ```azurecli-interactive
   az account get-access-token
   ```

2. Gebruik de volgende CURL-opdracht om aan te roepen van het Azure Resource Manager REST-eindpunt om in te schakelen door het systeem toegewezen beheerde identiteit op de virtuele machine, zoals vermeld in de hoofdtekst van de aanvraag door de waarde `{"identity":{"type":"SystemAssigned"}` voor een virtuele machine met de naam *myVM*.  Vervang `<ACCESS TOKEN>` met de waarde u hebt ontvangen in de vorige stap wanneer u een Bearer-toegangstoken aangevraagd en de `<SUBSCRIPTION ID>` waarde als geschikt is voor uw omgeving.
   
   > [!IMPORTANT]
   > Als u wilt zorgen dat u geen bestaande gebruiker toegewezen beheerde identiteiten die zijn toegewezen aan de virtuele machine niet verwijderen, moet u de beheerde gebruiker toegewezen identiteiten weergeven door met de volgende CURL-opdracht: `curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachines/<VM NAME>?api-version=2018-06-01' -H "Authorization: Bearer <ACCESS TOKEN>"`. Als een gebruiker toegewezen identiteiten die zijn toegewezen aan de virtuele machine, zoals vermeld in beheerde de `identity` waarde in het antwoord, gaat u naar stap 3 waarin wordt uitgelegd hoe u beheerde identiteiten gebruiker toegewezen behouden tijdens het inschakelen van het systeem toegewezen beheerde identiteit op de virtuele machine.

   ```bash
    curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"SystemAssigned"}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

3. Om in te schakelen door het systeem toegewezen beheerde identiteit op een virtuele machine met een bestaande gebruiker toegewezen beheerde identiteiten, moet u toevoegen `SystemAssigned` naar de `type` waarde.  
   
   Bijvoorbeeld, als uw virtuele machine heeft de gebruiker toegewezen beheerde identiteiten `ID1` en `ID2` zijn toegewezen en u wilt toevoegen van beheerde identiteit systeem toegewezen aan de virtuele machine, gebruikt u de volgende CURL-aanroep. Vervang `<ACCESS TOKEN>` en `<SUBSCRIPTION ID>` met waarden die geschikt is voor uw omgeving.

   API-versie `2018-06-01` worden opgeslagen door de gebruiker toegewezen beheerde identiteiten in de `userAssignedIdentities` waarde in een woordenlijst indeling plaats de `identityIds` waarde in de indeling van een matrix die wordt gebruikt in API-versie `2017-12-01` en eerdere versies.
   
   **API-VERSIE 2018-06-01**

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"SystemAssigned, UserAssigned", "userAssignedIdentities":{"/subscriptions/<<SUBSCRIPTION ID>>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1":{},"/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2":{}}}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

   **API-versie 2017-12-01 en oudere versies**

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"SystemAssigned, UserAssigned", "identityIds":["/subscriptions/<<SUBSCRIPTION ID>>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1","/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2"]}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

### <a name="disable-system-assigned-managed-identity-from-an-azure-vm"></a>Uitschakelen door het systeem toegewezen identiteit van een Azure VM beheerd

Als u wilt een systeem toegewezen beheerde identiteit van een bestaande VM uitschakelen, moet u een toegangstoken verkrijgen en vervolgens gebruiken we CURL om aan te roepen van het Resource Manager REST-eindpunt voor het bijwerken van het identiteitstype in `None`.

1. Een Bearer-toegangstoken, waarmee u uw virtuele machine maken met een door het systeem toegewezen beheerde identiteit gebruikt in de volgende stap in de autorisatie-header ophalen.

   ```azurecli-interactive
   az account get-access-token
   ```

2. Bijwerken van de virtuele machine met CURL om aan te roepen van het Azure Resource Manager REST-eindpunt om uit te schakelen door het systeem toegewezen beheerde identiteit.  Het volgende voorbeeld wordt het systeem toegewezen beheerde identiteit zoals vermeld in de hoofdtekst van de aanvraag door de waarde `{"identity":{"type":"None"}}` van een virtuele machine met de naam *myVM*.  Vervang `<ACCESS TOKEN>` met de waarde u hebt ontvangen in de vorige stap wanneer u een Bearer-toegangstoken aangevraagd en de `<SUBSCRIPTION ID>` waarde als geschikt is voor uw omgeving.

   > [!IMPORTANT]
   > Als u wilt zorgen dat u geen bestaande gebruiker toegewezen beheerde identiteiten die zijn toegewezen aan de virtuele machine niet verwijderen, moet u de beheerde gebruiker toegewezen identiteiten weergeven door met de volgende CURL-opdracht: `curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachines/<VM NAME>?api-version=2018-06-01' -H "Authorization: Bearer <ACCESS TOKEN>"`. Als een gebruiker toegewezen identiteiten die zijn toegewezen aan de virtuele machine, zoals vermeld in beheerde de `identity` waarde in het antwoord, gaat u naar stap 3 waarin wordt uitgelegd hoe u beheerde identiteiten gebruiker toegewezen behouden bij het uitschakelen van het systeem toegewezen beheerde identiteit op de virtuele machine.

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"None"}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

3. Als u het systeem toegewezen beheerde identiteit van een virtuele machine die beheerde identiteiten gebruiker toegewezen heeft, wilt verwijderen `SystemAssigned` uit de `{"identity":{"type:" "}}` waarde terwijl de `UserAssigned` waarde en de `userAssignedIdentities` woordenlijst als u de waarden **API-versie 2018-01-06**. Als u **API-versie 2017-12-01** of eerder, houden de `identityIds` matrix.

## <a name="user-assigned-managed-identity"></a>de gebruiker toegewezen beheerde identiteit

In deze sectie leert u hoe u toevoegen en verwijderen van beheerde identiteit op een Azure-VM met CURL aanroepen met de Azure Resource Manager REST-eindpunt door de gebruiker toegewezen.

### <a name="assign-a-user-assigned-managed-identity-during-the-creation-of-an-azure-vm"></a>Een gebruiker toegewezen beheerde identiteit toewijzen tijdens het maken van een Azure-VM

1. Een Bearer-toegangstoken, waarmee u uw virtuele machine maken met een door het systeem toegewezen beheerde identiteit gebruikt in de volgende stap in de autorisatie-header ophalen.

   ```azurecli-interactive
   az account get-access-token
   ```

2. Maak een [netwerkinterface](/cli/azure/network/nic?view=azure-cli-latest#az-network-nic-create) voor uw virtuele machine:

   ```azurecli-interactive
    az network nic create -g myResourceGroup --vnet-name myVnet --subnet mySubnet -n myNic
   ```

3.  Een Bearer-toegangstoken, waarmee u uw virtuele machine maken met een door het systeem toegewezen beheerde identiteit gebruikt in de volgende stap in de autorisatie-header ophalen.

   ```azurecli-interactive
   az account get-access-token
   ``` 

4. Maken van een gebruiker toegewezen beheerde identiteit met behulp van de instructies hier vinden: [maken van een gebruiker toegewezen beheerde identiteit](how-to-manage-ua-identity-rest.md#create-a-user-assigned-managed-identity).

5. Een virtuele machine met CURL om aan te roepen van het Azure Resource Manager REST-eindpunt maken. Het volgende voorbeeld wordt een virtuele machine met de naam *myVM* in de resourcegroep *myResourceGroup* met een gebruiker toegewezen beheerde identiteit `ID1`, zoals vermeld in de hoofdtekst van de aanvraag door de waarde `"identity":{"type":"UserAssigned"}`. Vervang `<ACCESS TOKEN>` met de waarde u hebt ontvangen in de vorige stap wanneer u een Bearer-toegangstoken aangevraagd en de `<SUBSCRIPTION ID>` waarde als geschikt is voor uw omgeving.
 
   
   **API-VERSIE 2018-06-01**
    
   ```bash   
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2018-06-01' -X PUT -d '{"location":"westus","name":"myVM",{"identity":{"type":"UserAssigned", "identityIds":["/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"]}},"properties":{"hardwareProfile":{"vmSize":"Standard_D2_v2"},"storageProfile":{"imageReference":{"sku":"2016-Datacenter","publisher":"MicrosoftWindowsServer","version":"latest","offer":"WindowsServer"},"osDisk":{"caching":"ReadWrite","managedDisk":{"storageAccountType":"Standard_LRS"},"name":"myVM3osdisk","createOption":"FromImage"},"dataDisks":[{"diskSizeGB":1023,"createOption":"Empty","lun":0},{"diskSizeGB":1023,"createOption":"Empty","lun":1}]},"osProfile":{"adminUsername":"azureuser","computerName":"myVM","adminPassword":"myPassword12"},"networkProfile":{"networkInterfaces":[{"id":"/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic","properties":{"primary":true}}]}}}' -H "Content-Type: application/json" -H "Authorization: Bearer <ACCESS TOKEN>"
   ``` 

   **API-versie 2017-12-01 en oudere versies**

   ```bash   
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2017-12-01' -X PUT -d '{"location":"westus","name":"myVM",{"identity":{"type":"UserAssigned", "identityIds":["/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"]}},"properties":{"hardwareProfile":{"vmSize":"Standard_D2_v2"},"storageProfile":{"imageReference":{"sku":"2016-Datacenter","publisher":"MicrosoftWindowsServer","version":"latest","offer":"WindowsServer"},"osDisk":{"caching":"ReadWrite","managedDisk":{"storageAccountType":"Standard_LRS"},"name":"myVM3osdisk","createOption":"FromImage"},"dataDisks":[{"diskSizeGB":1023,"createOption":"Empty","lun":0},{"diskSizeGB":1023,"createOption":"Empty","lun":1}]},"osProfile":{"adminUsername":"azureuser","computerName":"myVM","adminPassword":"myPassword12"},"networkProfile":{"networkInterfaces":[{"id":"/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic","properties":{"primary":true}}]}}}' -H "Content-Type: application/json" -H "Authorization: Bearer <ACCESS TOKEN>"
   ```

### <a name="assign-a-user-assigned-managed-identity-to-an-existing-azure-vm"></a>Een gebruiker toegewezen beheerde identiteit toewijzen aan een bestaande VM in Azure

1. Een Bearer-toegangstoken, waarmee u uw virtuele machine maken met een door het systeem toegewezen beheerde identiteit gebruikt in de volgende stap in de autorisatie-header ophalen.

   ```azurecli-interactive
   az account get-access-token
   ```

2.  Maken van een gebruiker toegewezen beheerde identiteit door de instructies hier [maken van een gebruiker toegewezen beheerde identiteit](how-to-manage-ua-identity-rest.md#create-a-user-assigned-managed-identity).

3.  Als u wilt zorgen dat u geen bestaande gebruiker of systeem toegewezen beheerde identiteiten die zijn toegewezen aan de virtuele machine wilt verwijderen, moet u de id-typen toegewezen aan de virtuele machine met behulp van de volgende CURL-opdracht weergeven. Als u identiteiten die zijn toegewezen aan de virtuele-machineschaalset beheerde, worden deze weergegeven onder de `identity` waarde.

    ```bash
    curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachines/<VM NAME>?api-version=2018-06-01' -H "Authorization: Bearer <ACCESS TOKEN>" 
    ```

    Als u een gebruiker of systeem toegewezen beheerde identiteiten die aan de virtuele machine is toegewezen, zoals vermeld in de `identity` waarde in het antwoord, gaat u verder met stap 5, waarin wordt uitgelegd hoe u beheerde identiteit passthr systeem toegewezen behouden tijdens het toevoegen van een gebruiker toegewezen beheerde identiteit op uw virtuele machine.

4. Als u geen gebruiker toegewezen beheerde identiteiten die aan uw virtuele machine is toegewezen hebt, gebruikt u de volgende CURL-opdracht uit om aan te roepen van het Azure Resource Manager REST-eindpunt voor de eerste gebruiker toegewezen beheerde identiteit toewijzen aan de virtuele machine.

   De volgende voorbeelden wordt een gebruiker toegewezen beheerde identiteit, toegewezen `ID1` aan een virtuele machine met de naam *myVM* in de resourcegroep *myResourceGroup*.  Vervang `<ACCESS TOKEN>` met de waarde u hebt ontvangen in de vorige stap wanneer u een Bearer-toegangstoken aangevraagd en de `<SUBSCRIPTION ID>` waarde als geschikt is voor uw omgeving.

   **API-VERSIE 2018-06-01**

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"UserAssigned", "userAssignedIdentities":{"/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1":{}}}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

   **API-versie 2017-12-01 en oudere versies**

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"userAssigned", "identityIds":["/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"]}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

5. Als u een bestaande gebruiker is toegewezen of door het systeem toegewezen beheerde identiteit toegewezen aan uw virtuele machine hebt:
   
   **API-VERSIE 2018-06-01**

   Toevoegen van de gebruiker toegewezen beheerde identiteit op de `userAssignedIdentities` dictionary-waarde.
    
   Bijvoorbeeld, als u het systeem toegewezen beheerde identiteit en de gebruiker toegewezen beheerde identiteit `ID1` momenteel toegewezen aan uw virtuele machine en wilt toevoegen van de gebruiker toegewezen beheerde identiteit `ID2` toe:

   ```bash
   curl  'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"SystemAssigned, UserAssigned", "userAssignedIdentities":{"/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1":{},"/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2":{}}}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

   **API-versie 2017-12-01 en oudere versies**

   Behouden van de gebruiker toegewezen beheerde identiteiten u behouden wilt de `identityIds` matrix van waarde tijdens het toevoegen van de nieuwe gebruiker toegewezen beheerde identiteit.

   Bijvoorbeeld, als u het systeem toegewezen beheerde identiteit en de gebruiker toegewezen beheerde identiteit `ID1` momenteel toegewezen aan uw virtuele machine en wilt toevoegen van de gebruiker toegewezen beheerde identiteit `ID2` toe: 

   ```bash
   curl  'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"SystemAssigned","UserAssigned", "identityIds":["/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1","/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2"]}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

### <a name="remove-a-user-assigned-managed-identity-from-an-azure-vm"></a>Verwijderen van een gebruiker toegewezen beheerde identiteit van een Azure VM

1. Een Bearer-toegangstoken, waarmee u uw virtuele machine maken met een door het systeem toegewezen beheerde identiteit gebruikt in de volgende stap in de autorisatie-header ophalen.

   ```azurecli-interactive
   az account get-access-token
   ```

2. Als u wilt zorgen dat u geen bestaande gebruiker toegewezen beheerde identiteiten die u wilt behouden toegewezen aan de virtuele machine of verwijderen van het systeem toegewezen beheerde identiteit niet verwijdert, moet u de beheerde identiteiten weergeven met behulp van de volgende CURL-opdracht: 
 
   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachines/<VM NAME>?api-version=2018-06-01' -H "Authorization: Bearer <ACCESS TOKEN>"
   ```
 
   Als u identiteiten die aan de virtuele machine is toegewezen beheerde, worden deze weergegeven in het antwoord in de `identity` waarde.

   Bijvoorbeeld, als er een gebruiker toegewezen identiteiten op beheerde `ID1` en `ID2` toegewezen aan uw virtuele machine en u alleen wilt behouden `ID1` toegewezen en de door het systeem toegewezen identiteit behouden:
   
   **API-VERSIE 2018-06-01**

   Voeg `null` beheerd naar de gebruiker toegewezen identiteit die u wilt verwijderen:

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"SystemAssigned, UserAssigned", "userAssignedIdentities":{"/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2":null}}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

   **API-versie 2017-12-01 en oudere versies**

   Alleen de gebruiker toegewezen identity(s) die u behouden wilt beheerde behouden de `identityIds` matrix:

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"SystemAssigned, UserAssigned", "identityIds":["/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"]}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

Als uw virtuele machine beide door het systeem is toegewezen heeft en gebruiker toegewezen beheerde identiteiten, u alle de gebruiker toegewezen beheerde identiteiten verwijderen kunt door over te schakelen voor het gebruik van alleen het systeem toegewezen beheerde identiteit met de volgende opdracht:

```bash
curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"SystemAssigned"}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
```
    
Als uw virtuele machine alleen door de gebruiker toegewezen identiteiten op beheerde is en u graag zou willen ze allemaal verwijderen, gebruikt u de volgende opdracht uit:

```bash
curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"None"}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
```
## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het maken, weergeven of verwijderen van beheerde identiteiten gebruiker toegewezen met behulp van REST:

- [Maken, weergeven of verwijderen van een gebruiker toegewezen beheerde identiteiten met behulp van REST-API aanroepen](how-to-manage-ua-identity-rest.md)
