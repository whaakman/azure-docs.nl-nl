---
title: Het configureren van systeem en de gebruiker toegewezen identiteiten op een Azure-VM met behulp van REST
description: Stap voor stap toegewezen instructies voor het configureren van een systeem- en identiteiten op een Azure-VM met CURL REST-API-aanroepen.
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
ms.openlocfilehash: 825f34d174c37c2ee5d4187048f7a31fbaeef226
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/23/2018
ms.locfileid: "39215926"
---
# <a name="configure-managed-identity-on-an-azure-vm-using-rest-api-calls"></a>Beheerde identiteit configureren op een Azure-VM met behulp van REST-API aanroepen

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Beheerde identiteit biedt Azure-services met een automatisch beheerde identiteit in Azure Active Directory. U kunt deze identiteit gebruiken om te verifiëren bij een service die ondersteuning biedt voor Azure AD-verificatie, zonder referenties in uw code. 

In dit artikel leert u hoe u de volgende beheerde identiteit-bewerkingen op een Azure-VM, met CURL aanroepen met de Azure Resource Manager REST-eindpunt uit te voeren:

- In- en uitschakelen van het systeem toegewezen identiteit op een Azure VM
- Toevoegen en verwijderen van een gebruiker toegewezen identiteit op een Azure VM

## <a name="prerequisites"></a>Vereisten

- Als u niet bekend met beheerde Service-identiteit bent, bekijk dan de [overzichtssectie](overview.md). **Lees de [verschil tussen een systeem toegewezen en een gebruiker toegewezen identiteit](overview.md#how-does-it-work)**.
- Als u nog geen Azure-account hebt, [registreer u dan voor een gratis account](https://azure.microsoft.com/free/) voordat u verdergaat.
- Als u wilt de beheerbewerkingen in dit artikel uitvoert, moet uw account de volgende roltoewijzingen:
    - [Inzender voor virtuele machines](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) toegewezen aan een virtuele machine maken en inschakelen en verwijderen van systeem en/of de gebruiker beheerde identiteit van een Azure-VM.
    - [Beheerde identiteit Inzender](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rol te maken van de identiteit van een gebruiker toegewezen.
    - [Beheerde identiteit Operator](/azure/role-based-access-control/built-in-roles#managed-identity-operator) rol die u wilt toewijzen en verwijderen van een gebruiker toegewezen identiteit van en naar een virtuele machine.
- Als u Windows gebruikt, installeert u de [Windows-subsysteem voor Linux](https://msdn.microsoft.com/commandline/wsl/about) of gebruik de [Azure Cloud Shell](../../cloud-shell/overview.md) in Azure portal.
- [Installeer de Azure CLI lokale console](/azure/install-azure-cli), als u de [Windows-subsysteem voor Linux](https://msdn.microsoft.com/commandline/wsl/about) of een [Linux-distributie](/cli/azure/install-azure-cli-apt?view=azure-cli-latest).
- Als u van Azure CLI lokale console gebruikmaakt, aanmelden bij Azure met `az login` met een account dat is gekoppeld aan de Azure-abonnement dat u wilt beheren systeem of gebruiker toegewezen identiteiten.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="system-assigned-identity"></a>Systeem toegewezen identiteit

In deze sectie leert u hoe u inschakelen en uitschakelen van systeem toegewezen identiteit op een Azure-VM met CURL aanroepen met de Azure Resource Manager REST-eindpunt.

### <a name="enable-system-assigned-identity-during-creation-of-an-azure-vm"></a>Systeem toegewezen identiteit tijdens het maken van een virtuele Azure-machine inschakelen

Voor het maken van een Azure-VM met systeem toegewezen identiteit is ingeschakeld, moet u een virtuele machine maken en een toegangstoken om te gebruiken we CURL om aan te roepen van het Resource Manager-eindpunt met het systeem toegewezen identiteit typewaarde ophalen.

1. Maak met [az group create](/cli/azure/group/#az_group_create) een [resourcegroep](../../azure-resource-manager/resource-group-overview.md#terminology) voor insluiting en implementatie van uw VM en de bijbehorende bronnen. U kunt deze stap overslaan als u al een resourcegroep hebt die u in plaats daarvan wilt gebruiken:

   ```azurecli-interactive 
   az group create --name myResourceGroup --location westus
   ```

2. Maak een [netwerkinterface](/cli/azure/network/nic?view=azure-cli-latest#az-network-nic-create) voor uw virtuele machine:

   ```azurecli-interactive
    az network nic create -g myResourceGroup --vnet-name myVnet --subnet mySubnet -n myNic
   ```

3.  Een Bearer-toegangstoken, waarmee u uw virtuele machine maken met een systeem toegewezen beheerde identiteit gebruikt in de volgende stap in de autorisatie-header ophalen.

   ```azurecli-interactive
   az account get-access-token
   ``` 

4. Een virtuele machine met CURL om aan te roepen van het Azure Resource Manager REST-eindpunt maken. Het volgende voorbeeld wordt een virtuele machine met de naam *myVM* met een systeem toegewezen identiteit, zoals vermeld in de hoofdtekst van de aanvraag door de waarde `"identity":{"type":"SystemAssigned"}`. Vervang `<ACCESS TOKEN>` met de waarde u hebt ontvangen in de vorige stap wanneer u een Bearer-toegangstoken aangevraagd en de `<SUBSCRIPTION ID>` waarde als geschikt is voor uw omgeving.
 
    ```bash
    curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2017-12-01' -X PUT -d '{"location":"westus","name":"myVM","identity":{"type":"SystemAssigned"},"properties":{"hardwareProfile":{"vmSize":"Standard_D2_v2"},"storageProfile":{"imageReference":{"sku":"2016-Datacenter","publisher":"MicrosoftWindowsServer","version":"latest","offer":"WindowsServer"},"osDisk":{"caching":"ReadWrite","managedDisk":{"storageAccountType":"Standard_LRS"},"name":"TestVM3osdisk","createOption":"FromImage"},"dataDisks":[{"diskSizeGB":1023,"createOption":"Empty","lun":0},{"diskSizeGB":1023,"createOption":"Empty","lun":1}]},"osProfile":{"adminUsername":"azureuser","computerName":"myVM","adminPassword":"myPassword12"},"networkProfile":{"networkInterfaces":[{"id":"/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic","properties":{"primary":true}}]}}}' -H "Content-Type: application/json" -H "Authorization: Bearer <ACCESS TOKEN>"
    ```

### <a name="enable-system-assigned-identity-on-an-existing-azure-vm"></a>Systeem toegewezen identiteit van een bestaande VM in Azure inschakelen

Om in te schakelen systeem toegewezen identiteit op een bestaande virtuele machine, moet u een toegangstoken verkrijgen en vervolgens gebruiken we CURL om aan te roepen van het Resource Manager REST-eindpunt voor het bijwerken van de id-type.

1. Een Bearer-toegangstoken, waarmee u uw virtuele machine maken met een systeem toegewezen beheerde identiteit gebruikt in de volgende stap in de autorisatie-header ophalen.

   ```azurecli-interactive
   az account get-access-token
   ```

2. Gebruik de volgende CURL-opdracht om aan te roepen van het Azure Resource Manager REST-eindpunt om in te schakelen systeem toegewezen identiteit op de virtuele machine, zoals vermeld in de hoofdtekst van de aanvraag door de waarde `{"identity":{"type":"SystemAssigned"}` voor een virtuele machine met de naam *myVM*.  Vervang `<ACCESS TOKEN>` met de waarde u hebt ontvangen in de vorige stap wanneer u een Bearer-toegangstoken aangevraagd en de `<SUBSCRIPTION ID>` waarde als geschikt is voor uw omgeving.
   
   > [!IMPORTANT]
   > Om te controleren of u een bestaande gebruiker toegewezen beheerde identiteiten die zijn toegewezen aan de virtuele machine niet verwijderen, moet u de gebruiker toegewezen identiteiten weergeven door met de volgende CURL-opdracht: `curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachines/<VM NAME>?api-version=2017-12-01' -H "Authorization: Bearer <ACCESS TOKEN>"`. Als er geen gebruiker toegewezen identiteiten die aan de virtuele machine is toegewezen, zoals vermeld in de `identity` waarde in het antwoord, gaat u naar stap 3 waarin wordt uitgelegd hoe u een gebruiker toegewezen identiteiten behouden tijdens het inschakelen van systeem toegewezen identiteit op de virtuele machine.

   ```bash
    curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"SystemAssigned"}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

3. Om in te schakelen systeem toegewezen identiteit op een virtuele machine met een bestaande gebruiker toegewezen identiteiten, moet u toevoegen `SystemAssigned` naar de `type` waarde.  
   
   Bijvoorbeeld, als uw virtuele machine heeft de gebruiker toegewezen identiteiten `ID1` en `ID2` zijn toegewezen en u graag zou willen systeem toegewezen identiteit toevoegen aan de virtuele machine, gebruikt u de volgende CURL-aanroep. Vervang `<ACCESS TOKEN>` en `<SUBSCRIPTION ID>` met waarden die geschikt is voor uw omgeving.
   
   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/TestVM?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"SystemAssigned","UserAssigned", "identityIds":["/subscriptions/<<SUBSCRIPTION ID>>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1","/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2"]}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

### <a name="disable-system-assigned-identity-from-an-azure-vm"></a>Het systeem toegewezen identiteit van een Azure VM uitgeschakeld

Als u wilt een systeem toegewezen identiteit van een bestaande VM uitschakelen, moet u een toegangstoken verkrijgen en vervolgens gebruiken we CURL om aan te roepen van het Resource Manager REST-eindpunt voor het bijwerken van het identiteitstype in `None`.

1. Een Bearer-toegangstoken, waarmee u uw virtuele machine maken met een systeem toegewezen beheerde identiteit gebruikt in de volgende stap in de autorisatie-header ophalen.

   ```azurecli-interactive
   az account get-access-token
   ```

2. Bijwerken van de virtuele machine met CURL om aan te roepen van het Azure Resource Manager REST-eindpunt om uit te schakelen systeem toegewezen identiteit.  Het volgende voorbeeld wordt het systeem toegewezen identiteit zoals vermeld in de hoofdtekst van de aanvraag door de waarde `{"identity":{"type":"None"}}` van een virtuele machine met de naam *myVM*.  Vervang `<ACCESS TOKEN>` met de waarde u hebt ontvangen in de vorige stap wanneer u een Bearer-toegangstoken aangevraagd en de `<SUBSCRIPTION ID>` waarde als geschikt is voor uw omgeving.

   > [!IMPORTANT]
   > Om te controleren of u een bestaande gebruiker toegewezen beheerde identiteiten die zijn toegewezen aan de virtuele machine niet verwijderen, moet u de gebruiker toegewezen identiteiten weergeven door met de volgende CURL-opdracht: `curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachines/<VM NAME>?api-version=2017-12-01' -H "Authorization: Bearer <ACCESS TOKEN>"`. Als er geen gebruiker toegewezen identiteiten die aan de virtuele machine is toegewezen, zoals vermeld in de `identity` waarde in het antwoord, gaat u naar stap 3 waarin wordt uitgelegd hoe u een gebruiker toegewezen identiteiten behouden bij het uitschakelen van systeem toegewezen identiteit op de virtuele machine.

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"None"}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

3. Als u systeem toegewezen identiteit van een virtuele machine waarvoor een gebruiker toegewezen identiteiten, wilt verwijderen `SystemAssigned` uit de `{"identity":{"type:" "}}` waarde terwijl de `UserAssigned` waarde en `identityIds` matrix die definieert wat een gebruiker toegewezen identiteiten zijn toegewezen aan de virtuele machine.

## <a name="user-assigned-identity"></a>Door gebruiker toegewezen identiteit

In deze sectie leert u hoe u toevoegen en verwijderen van door gebruiker toegewezen identiteit op een Azure-VM met CURL aanroepen met de Azure Resource Manager REST-eindpunt.

### <a name="assign-a-user-assigned-identity-during-the-creation-of-an-azure-vm"></a>Toewijzen van een gebruiker toegewezen identiteit tijdens het maken van een Azure-VM

1. Een Bearer-toegangstoken, waarmee u uw virtuele machine maken met een systeem toegewezen beheerde identiteit gebruikt in de volgende stap in de autorisatie-header ophalen.

   ```azurecli-interactive
   az account get-access-token
   ```

2. Maak een [netwerkinterface](/cli/azure/network/nic?view=azure-cli-latest#az-network-nic-create) voor uw virtuele machine:

   ```azurecli-interactive
    az network nic create -g myResourceGroup --vnet-name myVnet --subnet mySubnet -n myNic
   ```

3.  Een Bearer-toegangstoken, waarmee u uw virtuele machine maken met een systeem toegewezen beheerde identiteit gebruikt in de volgende stap in de autorisatie-header ophalen.

   ```azurecli-interactive
   az account get-access-token
   ``` 

4. Maken van een gebruiker toegewezen identiteit met behulp van de instructies hier vinden: [maken door een gebruiker toegewezen beheerde identiteit](how-to-manage-ua-identity-rest.md#create-a-user-assigned-managed-identity).

5. Een virtuele machine met CURL om aan te roepen van het Azure Resource Manager REST-eindpunt maken. Het volgende voorbeeld wordt een virtuele machine met de naam *myVM* in de resourcegroep *myResourceGroup* met een gebruiker toegewezen identiteit `ID1`, zoals vermeld in de hoofdtekst van de aanvraag door de waarde `"identity":{"type":"UserAssigned"}`. Vervang `<ACCESS TOKEN>` met de waarde u hebt ontvangen in de vorige stap wanneer u een Bearer-toegangstoken aangevraagd en de `<SUBSCRIPTION ID>` waarde als geschikt is voor uw omgeving.
 
   ```bash   
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2017-12-01' -X PUT -d '{"location":"westus","name":"myVM",{"identity":{"type":"UserAssigned", "identityIds":["/subscriptions/80c696ff-5efa-4909-a64d-f1b616f423ca/resourcegroups/TestRG/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"]}},"properties":{"hardwareProfile":{"vmSize":"Standard_D2_v2"},"storageProfile":{"imageReference":{"sku":"2016-Datacenter","publisher":"MicrosoftWindowsServer","version":"latest","offer":"WindowsServer"},"osDisk":{"caching":"ReadWrite","managedDisk":{"storageAccountType":"Standard_LRS"},"name":"TestVM3osdisk","createOption":"FromImage"},"dataDisks":[{"diskSizeGB":1023,"createOption":"Empty","lun":0},{"diskSizeGB":1023,"createOption":"Empty","lun":1}]},"osProfile":{"adminUsername":"azureuser","computerName":"myVM","adminPassword":"myPassword12"},"networkProfile":{"networkInterfaces":[{"id":"/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic","properties":{"primary":true}}]}}}' -H "Content-Type: application/json" -H "Authorization: Bearer <ACCESS TOKEN>"
   ```

### <a name="assign-a-user-assigned-identity-to-an-existing-azure-vm"></a>Een gebruiker toegewezen identiteit aan een bestaande VM in Azure toewijzen

1. Een Bearer-toegangstoken, waarmee u uw virtuele machine maken met een systeem toegewezen beheerde identiteit gebruikt in de volgende stap in de autorisatie-header ophalen.

   ```azurecli-interactive
   az account get-access-token
   ```

2.  Maken van een gebruiker toegewezen identiteit die door de instructies hier [maken door een gebruiker toegewezen beheerde identiteit](how-to-manage-ua-identity-rest.md#create-a-user-assigned-managed-identity).

3.  Als u wilt zorgen dat u geen bestaande gebruiker verwijderen of door het systeem toegewezen beheerde identiteiten die zijn toegewezen aan de virtuele machine, moet u de id-typen toegewezen aan de virtuele machine met behulp van de volgende CURL-opdracht weergeven. Als u identiteiten die zijn toegewezen aan de virtuele-machineschaalset beheerde, worden deze weergegeven onder de `identity` waarde.

    ```bash
    curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachines/<VM NAME>?api-version=2017-12-01' -H "Authorization: Bearer <ACCESS TOKEN>" 
    ```

    Als u een gebruiker of systeem toegewezen identiteit toegewezen aan de virtuele machine, zoals vermeld in de `identity` waarde in het antwoord, gaat u verder met stap 5, waarin wordt uitgelegd hoe u een gebruiker toegewezen identiteiten behouden tijdens het inschakelen van systeem toegewezen identiteit op de virtuele machine.

4. Als u een gebruiker toegewezen identiteiten die zijn toegewezen aan uw virtuele machine hebt, gebruikt u de volgende CURL-opdracht uit om aan te roepen van het Azure Resource Manager REST-eindpunt als u wilt toewijzen van de eerste gebruiker toegewezen identiteit voor de virtuele machine.  Als u een gebruiker toegewezen identity(s) toegewezen aan de virtuele machine hebt, gaat u naar de volgende stap waarin u meerdere gebruiker toegewezen identiteiten toevoegen aan een virtuele machine.

   Het volgende voorbeeld wordt de identiteit van een gebruiker die is toegewezen, `ID1` aan een virtuele machine met de naam *myVM* in de resourcegroep *myResourceGroup*.  Vervang `<ACCESS TOKEN>` met de waarde u hebt ontvangen in de vorige stap wanneer u een Bearer-toegangstoken aangevraagd en de `<SUBSCRIPTION ID>` waarde als geschikt is voor uw omgeving.

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"userAssigned", "identityIds":["/subscriptions/<SUBSCRIPTION ID>/resourcegroups/TestRG/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"]}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

5. Als u gebruiker of systeem toegewezen identiteit toegewezen aan uw virtuele machine hebt, moet u de nieuwe gebruiker toegewezen identiteit aan toevoegen `identityIDs` matrix, terwijl ook behouden blijft de gebruiker en het systeem toegewezen identiteit die momenteel zijn toegewezen aan de virtuele machine.

   Voor voorbeeld hebt u systeem identiteit en de gebruiker toegewezen identiteit toegewezen `ID1` momenteel toegewezen aan uw virtuele machine en wilt toevoegen van de gebruikers-id `ID2` , gebruik de volgende CURL-opdracht. Vervang `<ACCESS TOKEN>` met de waarde u hebt ontvangen in de stappen wanneer u een Bearer-toegangstoken aangevraagd en de `<SUBSCRIPTION ID>` waarde als geschikt is voor uw omgeving.

   ```bash
   curl  'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/TestVM?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"SystemAssigned","UserAssigned", "identityIds":["/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1","/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2"]}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

### <a name="remove-a-user-assigned-identity-from-an-azure-vm"></a>Een gebruiker toegewezen identiteit van een Azure VM verwijderen

1. Een Bearer-toegangstoken, waarmee u uw virtuele machine maken met een systeem toegewezen beheerde identiteit gebruikt in de volgende stap in de autorisatie-header ophalen.

   ```azurecli-interactive
   az account get-access-token
   ```

2. Om te controleren of dat u een bestaande gebruiker toegewezen beheerde identiteiten die u wilt behouden toegewezen aan de virtuele machine of verwijderen van het systeem toegewezen identiteit niet verwijdert, moet u de beheerde identiteiten weergeven met behulp van de volgende CURL-opdracht: 
 
   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachines/<VM NAME>?api-version=2017-12-01' -H "Authorization: Bearer <ACCESS TOKEN>"
   ```
 
   Als u identiteiten die aan de virtuele machine is toegewezen beheerde, worden deze weergegeven in het antwoord in de `identity` waarde.

   Bijvoorbeeld, hebt u een gebruiker toegewezen identiteiten `ID1` en `ID2` toegewezen aan uw virtuele machine, en alleen wilt houden `ID1` toegewezen en het systeem toegewezen identiteit behouden, zou u de dezelfde CURL-opdracht gebruiken als het toewijzen van een gebruiker toegewezen beheerde identiteit aan een virtuele machine alleen houden de `ID1` waarde en houd de `SystemAssigned` waarde. Hiermee verwijdert u de `ID2` gebruiker toegewezen identiteit van de virtuele machine terwijl het systeem toegewezen identiteit behouden blijft.

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"SystemAssigned","UserAssigned", "identityIds":["/subscriptions/80c696ff-5efa-4909-a64d-f1b616f423ca/resourcegroups/TestRG/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"]}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

Als uw virtuele machine zowel systeem toegewezen als de gebruiker toegewezen identiteiten, kunt u alle gebruiker toegewezen identiteiten door over te schakelen voor het gebruik van alleen systeem toegewezen met behulp van de volgende opdracht uit:

```bash
curl 'https://management.azure.com/subscriptions/80c696ff-5efa-4909-a64d-f1b616f423ca/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMachines/TestVM?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"SystemAssigned"}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
```
    
Als uw virtuele machine is de enige gebruiker toegewezen identiteiten en u graag zou willen ze allemaal verwijderen, gebruikt u de volgende opdracht uit:

```bash
curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"None"}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
```
## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het maken, weergeven of verwijderen van de gebruiker die is toegewezen met behulp van REST:

- [Maken, weergeven of verwijderen van een gebruiker toegewezen identiteit met behulp van REST-API aanroepen](how-to-manage-ua-identity-rest.md)