---
title: Het configureren van systeem en de gebruiker toegewezen identiteiten op een Azure-VMSS met behulp van REST
description: Stap voor stap toegewezen instructies voor het configureren van een systeem- en identiteiten op een Azure-VMSS met CURL REST-API-aanroepen.
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
ms.openlocfilehash: 704342db2d1ff56a66eb70b33d3da10874844f2d
ms.sourcegitcommit: d2f2356d8fe7845860b6cf6b6545f2a5036a3dd6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/16/2018
ms.locfileid: "42058112"
---
# <a name="configure-managed-identity-on-a-virtual-machine-scale-set-using-rest-api-calls"></a>Beheerde identiteit configureren voor een virtuele-Machineschaalset met behulp van REST-API-aanroepen

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Beheerde identiteit biedt Azure-services met een automatisch beheerde identiteit in Azure Active Directory. U kunt deze identiteit gebruiken om te verifiëren bij een service die ondersteuning biedt voor Azure AD-verificatie, zonder referenties in uw code. 

In dit artikel leert u hoe u de volgende beheerde identiteit bewerkingen uitvoeren op een virtuele-machineschaalset met CURL aanroepen met de Azure Resource Manager REST-eindpunt:

- In- en uitschakelen van het systeem toegewezen identiteit in een schaalset voor virtuele Azure-machine
- Toevoegen en verwijderen van een gebruiker toegewezen identiteit op een schaalset voor virtuele Azure-machine

## <a name="prerequisites"></a>Vereisten

- Als u niet bekend met beheerde Service-identiteit bent, bekijk dan de [overzichtssectie](overview.md). **Lees de [verschil tussen een systeem toegewezen en een gebruiker toegewezen identiteit](overview.md#how-does-it-work)**.
- Als u nog geen Azure-account hebt, [registreer u dan voor een gratis account](https://azure.microsoft.com/free/) voordat u verdergaat.
- Als u wilt de beheerbewerkingen in dit artikel uitvoert, moet uw account de volgende roltoewijzingen:
    - [Inzender voor virtuele machines](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) instellen voor een virtuele-machineschaalset maken en inschakelen en remove-systeem en/of gebruiker toegewezen beheerde identiteit van een virtuele-machineschaalset.
    - [Beheerde identiteit Inzender](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rol te maken van de identiteit van een gebruiker toegewezen.
    - [Beheerde identiteit Operator](/azure/role-based-access-control/built-in-roles#managed-identity-operator) rol die u wilt toewijzen en verwijderen van een gebruiker toegewezen identiteit van en naar een virtuele-machineschaalset.
- Als u Windows gebruikt, installeert u de [Windows-subsysteem voor Linux](https://msdn.microsoft.com/commandline/wsl/about) of gebruik de [Azure Cloud Shell](../../cloud-shell/overview.md) in Azure portal.
- [Installeer de Azure CLI lokale console](/azure/install-azure-cli), als u de [Windows-subsysteem voor Linux](https://msdn.microsoft.com/commandline/wsl/about) of een [Linux-distributie](/cli/azure/install-azure-cli-apt?view=azure-cli-latest).
- Als u van Azure CLI lokale console gebruikmaakt, aanmelden bij Azure met `az login` met een account dat is gekoppeld aan de Azure-abonnement dat u wilt beheren systeem of gebruiker toegewezen identiteiten.


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="system-assigned-identity"></a>Systeem toegewezen identiteit

In deze sectie leert u hoe u inschakelen en uitschakelen van systeem toegewezen identiteit in een schaalset van de virtuele machine met CURL aanroepen met de Azure Resource Manager REST-eindpunt.

### <a name="enable-system-assigned-identity-during-creation-of-a-virtual-machine-scale-set"></a>Systeem toegewezen identiteit inschakelen tijdens het maken van een virtuele-machineschaalset

Voor het maken van een virtuele-machineschaalset met systeem toegewezen identiteit is ingeschakeld, moet u een virtuele-machineschaalset maken en een toegangstoken om te gebruiken we CURL om aan te roepen van het Resource Manager-eindpunt met het systeem toegewezen identiteit typewaarde ophalen.

1. Maak een [resourcegroep](../../azure-resource-manager/resource-group-overview.md#terminology) voor insluiting en implementatie van uw virtuele-machineschaalset en de bijbehorende resources, met behulp van [az-groep maken](/cli/azure/group/#az-group-create). U kunt deze stap overslaan als u al een resourcegroep hebt die u in plaats daarvan wilt gebruiken:

   ```azurecli-interactive 
   az group create --name myResourceGroup --location westus
   ```

2. Maak een [netwerkinterface](/cli/azure/network/nic?view=azure-cli-latest#az-network-nic-create) instellen voor uw virtuele-machineschaalsets:

   ```azurecli-interactive
    az network nic create -g myResourceGroup --vnet-name myVnet --subnet mySubnet -n myNic
   ```

3.  Een Bearer-toegangstoken, die u in de volgende stap in de autorisatie-header wordt gebruiken om uw virtuele-machineschaalset met een beheerde identiteit toegewezen systeem ophalen.

   ```azurecli-interactive
   az account get-access-token
   ``` 

4. Maak een VM-schaalset met CURL om aan te roepen van het Azure Resource Manager REST-eindpunt. Het volgende voorbeeld wordt een virtuele-machineschaalset met de naam *myVMSS* in de *myResourceGroup* met een systeem toegewezen identiteit, zoals vermeld in de hoofdtekst van de aanvraag door de waarde `"identity":{"type":"SystemAssigned"}`. Vervang `<ACCESS TOKEN>` met de waarde u hebt ontvangen in de vorige stap wanneer u een Bearer-toegangstoken aangevraagd en de `<SUBSCRIPTION ID>` waarde als geschikt is voor uw omgeving.
 
    ```bash   
    curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01' -X PUT -d '{"sku":{"tier":"Standard","capacity":3,"name":"Standard_D1_v2"},"location":"eastus","identity":{"type":"SystemAssigned"},"properties":{"overprovision":true,"virtualMachineProfile":{"storageProfile":{"imageReference":{"sku":"2016-Datacenter","publisher":"MicrosoftWindowsServer","version":"latest","offer":"WindowsServer"},"osDisk":{"caching":"ReadWrite","managedDisk":{"storageAccountType":"Standard_LRS"},"createOption":"FromImage"}},"osProfile":{"computerNamePrefix":"myVMSS","adminUsername":"azureuser","adminPassword":"myPassword12"},"networkProfile":{"networkInterfaceConfigurations":[{"name":"myVMSS","properties":{"primary":true,"enableIPForwarding":true,"ipConfigurations":[{"name":"myVMSS","properties":{"subnet":{"id":"/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet"}}}]}}]}},"upgradePolicy":{"mode":"Manual"}}}' -H "Content-Type: application/json" -H "Authorization: Bearer <ACCESS TOKEN>"
    ```

### <a name="enable-system-assigned-identity-on-an-existing-azure-virtual-machine-scale-set"></a>Systeem toegewezen identiteit op een bestaande schaalset voor virtuele Azure-machine inschakelen

Om in te schakelen systeem toegewezen identiteit op een bestaande virtuele-machineschaalset, moet u een toegangstoken verkrijgen en vervolgens gebruiken we CURL om aan te roepen van het Resource Manager REST-eindpunt voor het bijwerken van de id-type.

1. Een Bearer-toegangstoken, die u in de volgende stap in de autorisatie-header wordt gebruiken om uw virtuele-machineschaalset met een beheerde identiteit toegewezen systeem ophalen.

   ```azurecli-interactive
   az account get-access-token
   ```

2. Gebruik de volgende CURL-opdracht uit om aan te roepen van het Azure Resource Manager REST-eindpunt om in te schakelen systeem toegewezen identiteit op de schaal van uw virtuele machine ingesteld zoals vermeld in de hoofdtekst van de aanvraag door de waarde `{"identity":{"type":"SystemAssigned"}` voor een virtuele-machineschaalset met de naam  *myVMSS*.  Vervang `<ACCESS TOKEN>` met de waarde u hebt ontvangen in de vorige stap wanneer u een Bearer-toegangstoken aangevraagd en de `<SUBSCRIPTION ID>` waarde als geschikt is voor uw omgeving.
   
   > [!IMPORTANT]
   > Om te controleren of u een bestaande gebruiker toegewezen beheerde identiteiten die zijn toegewezen aan de virtuele-machineschaalset niet verwijderen, moet u de gebruiker toegewezen identiteiten weergeven door met de volgende CURL-opdracht: `curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachineScaleSets/<VMSS NAME>?api-version=2018-06-01' -H "Authorization: Bearer <ACCESS TOKEN>"`. Als er geen gebruiker toegewezen identiteiten toegewezen aan de virtuele-machineschaalset zoals vermeld in de `identity` waarde in het antwoord, gaat u naar stap 3 waarin wordt uitgelegd hoe u een gebruiker toegewezen identiteiten behouden tijdens het inschakelen van systeem toegewezen identiteit voor uw virtuele VM scale set.

   ```bash
    curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"SystemAssigned"}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

3. Om in te schakelen systeem toegewezen identiteit op een virtuele-machineschaalset met bestaande gebruiker toegewezen identiteiten, moet u toevoegen `SystemAssigned` naar de `type` waarde.  
   
   Bijvoorbeeld, als uw virtuele-machineschaalset heeft de gebruiker toegewezen identiteiten `ID1` en `ID2` zijn toegewezen en u graag zou willen systeem toegewezen identiteit toevoegen aan de virtuele-machineschaalset, gebruikt u de volgende CURL-aanroep. Vervang `<ACCESS TOKEN>` en `<SUBSCRIPTION ID>` met waarden die geschikt is voor uw omgeving.

   API-versie `2018-06-01` slaat een gebruiker toegewezen identiteiten in de `userAssignedIdentities` waarde in een woordenlijst indeling plaats de `identityIds` waarde in de indeling van een matrix die wordt gebruikt in API-versie `2017-12-01` en eerdere versies.
   
   **API-VERSIE 2018-06-01**

   ```bash
   curl -v 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"SystemAssigned,UserAssigned", "userAssignedIdentities":{"/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1":{},"/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2":{}}}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```
   
   **API-versie 2017-12-01 en oudere versies**

   ```bash
   curl -v 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"SystemAssigned","UserAssigned", "identityIds":["/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1","/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2"]}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

### <a name="disable-system-assigned-identity-from-an-azure-virtual-machine-scale-set"></a>Systeem toegewezen identiteit van een schaalset voor virtuele Azure-machine uitschakelen

Als u wilt een systeem toegewezen identiteit op een bestaande virtuele-machineschaalset uitschakelen, moet u een toegangstoken verkrijgen en vervolgens gebruiken we CURL om aan te roepen van het Resource Manager REST-eindpunt voor het bijwerken van het identiteitstype in `None`.

1. Een Bearer-toegangstoken, die u in de volgende stap in de autorisatie-header wordt gebruiken om uw virtuele-machineschaalset met een beheerde identiteit toegewezen systeem ophalen.

   ```azurecli-interactive
   az account get-access-token
   ```

2. Bijwerken van de virtuele-machineschaalset met CURL om aan te roepen van het Azure Resource Manager REST-eindpunt om uit te schakelen systeem toegewezen identiteit.  Het volgende voorbeeld wordt het systeem toegewezen identiteit zoals vermeld in de hoofdtekst van de aanvraag door de waarde `{"identity":{"type":"None"}}` van een virtuele-machineschaalset met de naam *myVMSS*.  Vervang `<ACCESS TOKEN>` met de waarde u hebt ontvangen in de vorige stap wanneer u een Bearer-toegangstoken aangevraagd en de `<SUBSCRIPTION ID>` waarde als geschikt is voor uw omgeving.

   > [!IMPORTANT]
   > Om te controleren of u een bestaande gebruiker toegewezen beheerde identiteiten die zijn toegewezen aan de virtuele-machineschaalset niet verwijderen, moet u de gebruiker toegewezen identiteiten weergeven door met de volgende CURL-opdracht: `curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachineScaleSets/<VMSS NAME>?api-version=2018-06-01' -H "Authorization: Bearer <ACCESS TOKEN>"`. Als u een gebruiker toegewezen identiteit toegewezen aan de virtuele-machineschaalset hebt, gaat u naar stap 3 die laat u hoe de gebruiker toegewezen identiteiten zien tijdens het verwijderen van het systeem toegewezen identiteit van uw virtuele-machineschaalset behouden.

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"None"}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

3. Als u systeem toegewezen identiteit van een virtuele-machineschaalset waarvoor een gebruiker toegewezen identiteiten, wilt verwijderen `SystemAssigned` uit de `{"identity":{"type:" "}}` waarde terwijl de `UserAssigned` waarde en de `userAssignedIdentities` woordenlijst waarden als u **API-versie 2018-01-06**. Als u **API-versie 2017-12-01** of eerder, houden de `identityIds` matrix.

## <a name="user-assigned-identity"></a>Door gebruiker toegewezen identiteit

In deze sectie leert u hoe u toevoegen en verwijderen van door gebruiker toegewezen identiteit in een schaalset van de virtuele machine met CURL aanroepen met de Azure Resource Manager REST-eindpunt.

### <a name="assign-a-user-assigned-identity-during-the-creation-of-a-virtual-machine-scale-set"></a>Een gebruiker toegewezen identiteit toewijzen tijdens het maken van een virtuele-machineschaalset

1. Een Bearer-toegangstoken, die u in de volgende stap in de autorisatie-header wordt gebruiken om uw virtuele-machineschaalset met een beheerde identiteit toegewezen systeem ophalen.

   ```azurecli-interactive
   az account get-access-token
   ```

2. Maak een [netwerkinterface](/cli/azure/network/nic?view=azure-cli-latest#az-network-nic-create) instellen voor uw virtuele-machineschaalsets:

   ```azurecli-interactive
    az network nic create -g myResourceGroup --vnet-name myVnet --subnet mySubnet -n myNic
   ```

3.  Een Bearer-toegangstoken, die u in de volgende stap in de autorisatie-header wordt gebruiken om uw virtuele-machineschaalset met een beheerde identiteit toegewezen systeem ophalen.

   ```azurecli-interactive
   az account get-access-token
   ``` 

4. Maken van een gebruiker toegewezen identiteit met behulp van de instructies hier vinden: [maken door een gebruiker toegewezen beheerde identiteit](how-to-manage-ua-identity-rest.md#create-a-user-assigned-managed-identity).

5. Maak een VM-schaalset met CURL om aan te roepen van het Azure Resource Manager REST-eindpunt. Het volgende voorbeeld wordt een virtuele-machineschaalset met de naam *myVMSS* in de resourcegroep *myResourceGroup* met een gebruiker toegewezen identiteit `ID1`, zoals vermeld in de hoofdtekst van de aanvraag door de waarde `"identity":{"type":"UserAssigned"}`. Vervang `<ACCESS TOKEN>` met de waarde u hebt ontvangen in de vorige stap wanneer u een Bearer-toegangstoken aangevraagd en de `<SUBSCRIPTION ID>` waarde als geschikt is voor uw omgeving.
 
   **API-VERSIE 2018-06-01**

   ```bash   
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01' -X PUT -d '{"sku":{"tier":"Standard","capacity":3,"name":"Standard_D1_v2"},"location":"eastus",{"identity":{"type":"UserAssigned", "userAssignedIdentities":{"/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1":{}}}},"properties":{"overprovision":true,"virtualMachineProfile":{"storageProfile":{"imageReference":{"sku":"2016-Datacenter","publisher":"MicrosoftWindowsServer","version":"latest","offer":"WindowsServer"},"osDisk":{"caching":"ReadWrite","managedDisk":{"storageAccountType":"Standard_LRS"},"createOption":"FromImage"}},"osProfile":{"computerNamePrefix":"myVMSS","adminUsername":"azureuser","adminPassword":"myPassword12"},"networkProfile":{"networkInterfaceConfigurations":[{"name":"myVMSS","properties":{"primary":true,"enableIPForwarding":true,"ipConfigurations":[{"name":"myVMSS","properties":{"subnet":{"id":"/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet"}}}]}}]}},"upgradePolicy":{"mode":"Manual"}}}' -H "Content-Type: application/json" -H "Authorization: Bearer <ACCESS TOKEN>"
   ```

   **API-versie 2017-12-01 en oudere versies**

   ```bash   
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2017-12-01' -X PUT -d '{"sku":{"tier":"Standard","capacity":3,"name":"Standard_D1_v2"},"location":"eastus",{"identity":{"type":"UserAssigned", "identityIds":["/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"]}},"properties":{"overprovision":true,"virtualMachineProfile":{"storageProfile":{"imageReference":{"sku":"2016-Datacenter","publisher":"MicrosoftWindowsServer","version":"latest","offer":"WindowsServer"},"osDisk":{"caching":"ReadWrite","managedDisk":{"storageAccountType":"Standard_LRS"},"createOption":"FromImage"}},"osProfile":{"computerNamePrefix":"myVMSS","adminUsername":"azureuser","adminPassword":"myPassword12"},"networkProfile":{"networkInterfaceConfigurations":[{"name":"myVMSS","properties":{"primary":true,"enableIPForwarding":true,"ipConfigurations":[{"name":"myVMSS","properties":{"subnet":{"id":"/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet"}}}]}}]}},"upgradePolicy":{"mode":"Manual"}}}' -H "Content-Type: application/json" -H "Authorization: Bearer <ACCESS TOKEN>"
   ```

### <a name="assign-a-user-assigned-identity-to-an-existing-azure-virtual-machine-scale-set"></a>Een gebruiker toegewezen identiteit toewijzen aan een bestaande schaalset voor virtuele Azure-machine

1. Een Bearer-toegangstoken, die u in de volgende stap in de autorisatie-header wordt gebruiken om uw virtuele-machineschaalset met een beheerde identiteit toegewezen systeem ophalen.

   ```azurecli-interactive
   az account get-access-token
   ```

2.  Maken van een gebruiker toegewezen identiteit die door de instructies hier [maken door een gebruiker toegewezen beheerde identiteit](how-to-manage-ua-identity-rest.md#create-a-user-assigned-managed-identity).

3. Toegewezen beheerde identiteiten die zijn toegewezen aan de virtuele-machineschaalset om te controleren of dat u geen bestaande gebruiker of systeem wilt verwijderen, moet u om de identiteit van de typen die zijn toegewezen aan de virtuele-machineschaalset instellen met behulp van de volgende CURL-opdracht weer te geven. Als u identiteiten die zijn toegewezen aan de virtuele-machineschaalset beheerde, ze worden weergegeven in de `identity` waarde.
 
   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachineScaleSets/<VMSS NAME>?api-version=2018-06-01' -H "Authorization: Bearer <ACCESS TOKEN>"
   ```

4. Als u niet een gebruiker of systeem toegewezen identiteiten die zijn toegewezen aan uw virtuele-machineschaalset hebt, gebruikt u de volgende CURL-opdracht uit om aan te roepen van het Azure Resource Manager REST-eindpunt als u wilt toewijzen van de eerste gebruiker toegewezen identiteit voor de virtuele-machineschaalset.  Als u een gebruiker of systeem identity(s) toegewezen aan de virtuele-machineschaalset toegewezen, gaat u naar stap 5, waarin u meerdere gebruiker toegewezen identiteiten toevoegen aan een virtuele-machineschaalset terwijl het systeem toegewezen identiteit wordt ook behouden.

   Het volgende voorbeeld wordt de identiteit van een gebruiker die is toegewezen, `ID1` aan een virtuele-machineschaalset met de naam *myVMSS* in de resourcegroep *myResourceGroup*.  Vervang `<ACCESS TOKEN>` met de waarde u hebt ontvangen in de vorige stap wanneer u een Bearer-toegangstoken aangevraagd en de `<SUBSCRIPTION ID>` waarde als geschikt is voor uw omgeving.

   **API-VERSIE 2018-06-01**

    ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-12-01' -X PATCH -d '{"identity":{"type":"userAssigned", "userAssignedIdentities":{"/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1":{}}}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```   
    
   **API-versie 2017-12-01 en oudere versies**

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"userAssigned", "identityIds":["/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"]}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

5. Als u een bestaande gebruiker of systeem toegewezen identiteit toegewezen aan uw virtuele-machineschaalset:
   
   **API-VERSIE 2018-06-01**

   Toevoegen van de identiteit van de gebruiker toegewezen aan de `userAssignedIdentities` dictionary-waarde.

   Bijvoorbeeld, hebt u het systeem toegewezen identiteit en de identiteit van de gebruiker toegewezen `ID1` momenteel toegewezen aan uw virtuele-machineschaalset en wilt toevoegen van de gebruiker toegewezen identiteit `ID2` toe:

   ```bash
   curl  'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"SystemAssigned, UserAssigned", "userAssignedIdentities":{"/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1":{},"/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2":{}}}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

   **API-versie 2017-12-01 en oudere versies**

   Behouden van de gebruiker toegewezen identiteiten die u behouden wilt de `identityIds` matrix van waarde tijdens het toevoegen van de nieuwe gebruiker toegewezen identiteit.

   Bijvoorbeeld, hebt u het systeem toegewezen identiteit en de identiteit van de gebruiker toegewezen `ID1` momenteel toegewezen aan uw virtuele-machineschaalset en wilt toevoegen van de gebruikers-id `ID2` toe: 

   ```bash
   curl  'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"SystemAssigned, UserAssigned", "identityIds":["/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1","/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2"]}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

### <a name="remove-a-user-assigned-identity-from-a-virtual-machine-scale-set"></a>Een gebruiker toegewezen identiteit van een virtuele-machineschaalset verwijderen

1. Een Bearer-toegangstoken, die u in de volgende stap in de autorisatie-header wordt gebruiken om uw virtuele-machineschaalset met een beheerde identiteit toegewezen systeem ophalen.

   ```azurecli-interactive
   az account get-access-token
   ```

2. Om te controleren of dat u een bestaande gebruiker toegewezen beheerde identiteiten die u wilt behouden toegewezen aan de virtuele-machineschaalset of verwijderen van het systeem toegewezen identiteit niet verwijdert, moet u de beheerde identiteiten weergeven met behulp van de volgende CURL-opdracht: 
   
   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachineScaleSets/<VMSS NAME>?api-version=2018-06-01' -H "Authorization: Bearer <ACCESS TOKEN>" 
   ```
   
   Als u identiteiten die aan de virtuele machine is toegewezen beheerde, worden deze weergegeven in het antwoord in de `identity` waarde. 
    
   Bijvoorbeeld, hebt u een gebruiker toegewezen identiteiten `ID1` en `ID2` toegewezen aan uw virtuele-machineschaalset en u alleen wilt behouden `ID1` toegewezen en het systeem toegewezen identiteit behouden:

   **API-VERSIE 2018-06-01**

   Voeg `null` aan de gebruiker toegewezen identiteit die u wilt verwijderen:

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"SystemAssigned, UserAssigned", "userAssignedIdentities":{"/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1":{}}}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

   **API-versie 2017-12-01 en oudere versies**

   Bewaren van alleen de gebruiker identity(s) die u wilt behouden toegewezen de `identityIds` matrix:   

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"SystemAssigned","UserAssigned", "identityIds":["/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"]}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

Als uw virtuele-machineschaalset heeft zowel de systeem toegewezen als de gebruiker toegewezen identiteiten, kunt u alle gebruiker toegewezen identiteiten door over te schakelen voor het gebruik van alleen systeem toegewezen met behulp van de volgende opdracht uit:

```bash
curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"SystemAssigned"}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
```
    
Als uw virtuele-machineschaalset is de enige gebruiker toegewezen identiteiten en u graag zou willen ze allemaal verwijderen, gebruikt u de volgende opdracht uit:

```bash
curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"None"}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
```

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het maken, weergeven of verwijderen van de gebruiker die is toegewezen met behulp van REST:

- [Maken, weergeven of verwijderen van een gebruiker toegewezen identiteit met behulp van REST-API aanroepen](how-to-manage-ua-identity-rest.md)