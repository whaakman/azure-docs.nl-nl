---
title: Systeem- en gebruiker toegewezen beheerde identiteiten op een Azure-VMSS met behulp van REST configureren
description: Stapsgewijze instructies voor het configureren van een systeem en de gebruiker toegewezen beheerde identiteiten op een Azure-VMSS met CURL om de REST-API-aanroepen.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: daveba
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/25/2018
ms.author: priyamo
ms.openlocfilehash: ba71e1af021188888bd9fbfe4a808c2ffcf10a4b
ms.sourcegitcommit: b4755b3262c5b7d546e598c0a034a7c0d1e261ec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54884534"
---
# <a name="configure-managed-identities-for-azure-resources-on-a-virtual-machine-scale-set-using-rest-api-calls"></a>Configureren van beheerde identiteiten voor Azure-resources op een VM-schaalset met behulp van REST-API aanroepen

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Beheerde identiteiten voor Azure-resources biedt Azure-services met een automatisch beheerde identiteit in Azure Active Directory. U kunt deze identiteit gebruiken om te verifiëren bij een service die ondersteuning biedt voor Azure AD-verificatie, zonder referenties in uw code. 

In dit artikel leert met CURL om aanroepen naar het Azure Resource Manager REST-eindpunt, u hoe u de volgende beheerde identiteiten voor bewerkingen van de Azure-resources op een virtuele-machineschaalset uitvoeren:

- In- en uitschakelen van de door het systeem toegewezen beheerde identiteit in een schaalset voor virtuele Azure-machine
- Toevoegen en verwijderen van een gebruiker toegewezen beheerde identiteit in een schaalset voor virtuele Azure-machine

## <a name="prerequisites"></a>Vereisten

- Als u niet bekend met beheerde identiteiten voor Azure-resources bent, lees de [overzichtssectie](overview.md). **Lees de [verschil tussen een beheerde identiteit door het systeem is toegewezen en de gebruiker toegewezen](overview.md#how-does-it-work)**.
- Als u nog geen Azure-account hebt, [registreer u dan voor een gratis account](https://azure.microsoft.com/free/) voordat u verdergaat.
- Als u wilt de beheerbewerkingen in dit artikel uitvoert, moet uw account de volgende Azure op basis van rollen access control-toewijzingen:

    > [!NOTE]
    > Er zijn geen extra Azure AD directory-roltoewijzingen is vereist.

    - [Inzender voor virtuele machines](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) aan een virtuele-machineschaalset maken en inschakelen en verwijderen van systeem en/of de gebruiker toegewezen beheerde identiteit van een virtuele-machineschaalset.
    - [Beheerde identiteit Inzender](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rol te maken van een gebruiker toegewezen beheerde identiteit.
    - [Beheerde identiteit Operator](/azure/role-based-access-control/built-in-roles#managed-identity-operator) rol die u wilt toewijzen en verwijderen van een gebruiker toegewezen identiteit van en naar een virtuele-machineschaalset.
- Als u Windows gebruikt, installeert u de [Windows-subsysteem voor Linux](https://msdn.microsoft.com/commandline/wsl/about) of gebruik de [Azure Cloud Shell](../../cloud-shell/overview.md) in Azure portal.
- [Installeer de Azure CLI lokale console](/cli/azure/install-azure-cli), als u de [Windows-subsysteem voor Linux](https://msdn.microsoft.com/commandline/wsl/about) of een [Linux-distributie](/cli/azure/install-azure-cli-apt?view=azure-cli-latest).
- Als u van Azure CLI lokale console gebruikmaakt, aanmelden bij Azure met `az login` met een account dat is gekoppeld aan het Azure-abonnement u wilt beheren, systeem- of beheerde identiteiten gebruiker toegewezen.


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="system-assigned-managed-identity"></a>Het systeem toegewezen beheerde identiteit

In deze sectie leert u hoe u inschakelen en uitschakelen van het systeem toegewezen beheerde identiteit in een schaalset van de virtuele machine met CURL aanroepen met de Azure Resource Manager REST-eindpunt.

### <a name="enable-system-assigned-managed-identity-during-creation-of-a-virtual-machine-scale-set"></a>Beheerde identiteit systeem toegewezen tijdens het maken van een virtuele-machineschaalset inschakelen

Voor het maken van een virtuele-machineschaalset met het systeem toegewezen beheerde identiteit ingeschakeld, moet u een virtuele-machineschaalset maken en ophalen van een toegangstoken om te gebruiken we CURL om aan te roepen van het Resource Manager-eindpunt met de waarde van het type beheerde identiteit door het systeem is toegewezen.

1. Maak een [resourcegroep](../../azure-resource-manager/resource-group-overview.md#terminology) voor insluiting en implementatie van uw virtuele-machineschaalset en de bijbehorende resources, met behulp van [az-groep maken](/cli/azure/group/#az-group-create). U kunt deze stap overslaan als u al een resourcegroep hebt die u in plaats daarvan wilt gebruiken:

   ```azurecli-interactive 
   az group create --name myResourceGroup --location westus
   ```

2. Maak een [netwerkinterface](/cli/azure/network/nic?view=azure-cli-latest#az-network-nic-create) instellen voor uw virtuele-machineschaalsets:

   ```azurecli-interactive
    az network nic create -g myResourceGroup --vnet-name myVnet --subnet mySubnet -n myNic
   ```

3.  Een Bearer-toegangstoken, die u in de volgende stap in de autorisatie-header wordt gebruiken om uw virtuele-machineschaalset met een beheerde identiteit voor het systeem toegewezen ophalen.

   ```azurecli-interactive
   az account get-access-token
   ``` 

4. Maak een VM-schaalset met CURL om aan te roepen van het Azure Resource Manager REST-eindpunt. Het volgende voorbeeld wordt een virtuele-machineschaalset met de naam *myVMSS* in de *myResourceGroup* beheerd met een door het systeem toegewezen identiteit, zoals vermeld in de hoofdtekst van de aanvraag door de waarde `"identity":{"type":"SystemAssigned"}`. Vervang `<ACCESS TOKEN>` met de waarde u hebt ontvangen in de vorige stap wanneer u een Bearer-toegangstoken aangevraagd en de `<SUBSCRIPTION ID>` waarde als geschikt is voor uw omgeving.

   ```bash   
  curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01' -X PUT -d '{"sku":{"tier":"Standard","capacity":3,"name":"Standard_D1_v2"},"location":"eastus","identity":{"type":"SystemAssigned"},"properties":{"overprovision":true,"virtualMachineProfile":{"storageProfile":{"imageReference":{"sku":"2016-Datacenter","publisher":"MicrosoftWindowsServer","version":"latest","offer":"WindowsServer"},"osDisk":{"caching":"ReadWrite","managedDisk":{"storageAccountType":"Standard_LRS"},"createOption":"FromImage"}},"osProfile":{"computerNamePrefix":"myVMSS","adminUsername":"azureuser","adminPassword":"myPassword12"},"networkProfile":{"networkInterfaceConfigurations":[{"name":"myVMSS","properties":{"primary":true,"enableIPForwarding":true,"ipConfigurations":[{"name":"myVMSS","properties":{"subnet":{"id":"/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet"}}}]}}]}},"upgradePolicy":{"mode":"Manual"}}}' -H "Content-Type: application/json" -H "Authorization: Bearer <ACCESS TOKEN>"
  ```

   ```HTTP
   PUT https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01 HTTP/1.1
   ```

  **Aanvraagheaders**

  |Aanvraagheader  |Description  |
  |---------|---------|
  |*Content-Type*     | Vereist. Ingesteld op `application/json`.        |
  |*Autorisatie*     | Vereist. Ingesteld op een geldige `Bearer` toegangstoken. | 

  **Aanvraagtekst**

  ```JSON
    {
       "sku":{
          "tier":"Standard",
          "capacity":3,
          "name":"Standard_D1_v2"
       },
       "location":"eastus",
       "identity":{
          "type":"SystemAssigned"
       },
       "properties":{
          "overprovision":true,
          "virtualMachineProfile":{
             "storageProfile":{
                "imageReference":{
                   "sku":"2016-Datacenter",
                   "publisher":"MicrosoftWindowsServer",
                   "version":"latest",
                   "offer":"WindowsServer"
                },
                "osDisk":{
                   "caching":"ReadWrite",
                   "managedDisk":{
                      "storageAccountType":"Standard_LRS"
                   },
                   "createOption":"FromImage"
                }
             },
             "osProfile":{
                "computerNamePrefix":"myVMSS",
                "adminUsername":"azureuser",
                "adminPassword":"myPassword12"
             },
             "networkProfile":{
                "networkInterfaceConfigurations":[
                   {
                      "name":"myVMSS",
                      "properties":{
                         "primary":true,
                         "enableIPForwarding":true,
                         "ipConfigurations":[
                            {
                               "name":"myVMSS",
                               "properties":{
                                  "subnet":{
                                     "id":"/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet"
                                  }
                               }
                            }
                         ]
                      }
                   }
                ]
             }
          },
          "upgradePolicy":{
             "mode":"Manual"
          }
       }
    }  
  ```  

### <a name="enable-system-assigned-managed-identity-on-a-existing-virtual-machine-scale-set"></a>De beheerde inschakelen door het systeem toegewezen identiteit op een bestaande virtuele-machineschaalset

Om in te schakelen door het systeem toegewezen beheerde identiteit op een bestaande virtuele-machineschaalset, moet u een toegangstoken verkrijgen en vervolgens gebruiken we CURL om aan te roepen van het Resource Manager REST-eindpunt voor het bijwerken van de id-type.

1. Een Bearer-toegangstoken, die u in de volgende stap in de autorisatie-header wordt gebruiken om uw virtuele-machineschaalset met een beheerde identiteit voor het systeem toegewezen ophalen.

   ```azurecli-interactive
   az account get-access-token
   ```

2. Gebruik de volgende CURL-opdracht uit om aan te roepen van het Azure Resource Manager REST-eindpunt om in te schakelen door het systeem toegewezen beheerde identiteit op de schaal van uw virtuele machine ingesteld zoals vermeld in de hoofdtekst van de aanvraag door de waarde `{"identity":{"type":"SystemAssigned"}` voor een virtuele-machineschaalset met de naam *myVMSS*.  Vervang `<ACCESS TOKEN>` met de waarde u hebt ontvangen in de vorige stap wanneer u een Bearer-toegangstoken aangevraagd en de `<SUBSCRIPTION ID>` waarde als geschikt is voor uw omgeving.
   
   > [!IMPORTANT]
   > Als u wilt zorgen dat u geen bestaande gebruiker toegewezen beheerde identiteiten die zijn toegewezen aan de virtuele-machineschaalset niet verwijderen, moet u de beheerde gebruiker toegewezen identiteiten weergeven door met de volgende CURL-opdracht: `curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachineScaleSets/<VMSS NAME>?api-version=2018-06-01' -H "Authorization: Bearer <ACCESS TOKEN>"`. Als een gebruiker toegewezen identiteiten die zijn toegewezen aan de virtuele-machineschaalset zoals vermeld in beheerde de `identity` waarde in het antwoord, gaat u naar stap 3 waarin wordt uitgelegd hoe u beheerde identiteiten gebruiker toegewezen behouden tijdens het inschakelen van het systeem toegewezen beheerde identiteit op de virtuele-machineschaalset.

   ```bash
    curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"SystemAssigned"}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01 HTTP/1.1
   ```

   **Aanvraagheaders**

   |Aanvraagheader  |Description  |
   |---------|---------|
   |*Content-Type*     | Vereist. Ingesteld op `application/json`.        |
   |*Autorisatie*     | Vereist. Ingesteld op een geldige `Bearer` toegangstoken. | 

   **Aanvraagtekst**

   ```JSON
    {
       "identity":{
          "type":"SystemAssigned"
       }
    }
   ```

3. Om in te schakelen door het systeem toegewezen beheerde identiteit op een virtuele-machineschaalset met bestaande gebruiker toegewezen beheerde identiteiten, moet u toevoegen `SystemAssigned` naar de `type` waarde.  
   
   Bijvoorbeeld, als uw virtuele-machineschaalset heeft de gebruiker toegewezen beheerde identiteiten `ID1` en `ID2` zijn toegewezen en u wilt toevoegen van beheerde identiteit systeem toegewezen aan de virtuele-machineschaalset, gebruikt u de volgende CURL-aanroep. Vervang `<ACCESS TOKEN>` en `<SUBSCRIPTION ID>` met waarden die geschikt is voor uw omgeving.

   API-versie `2018-06-01` worden opgeslagen door de gebruiker toegewezen beheerde identiteiten in de `userAssignedIdentities` waarde in een woordenlijst indeling plaats de `identityIds` waarde in de indeling van een matrix die wordt gebruikt in API-versie `2017-12-01`.
   
   **API-VERSIE 2018-06-01**

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"SystemAssigned,UserAssigned", "userAssignedIdentities":{"/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1":{},"/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2":{}}}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01 HTTP/1.1
   ```

   **Aanvraagheaders**

   |Aanvraagheader  |Description  |
   |---------|---------|
   |*Content-Type*     | Vereist. Ingesteld op `application/json`.        |
   |*Autorisatie*     | Vereist. Ingesteld op een geldige `Bearer` toegangstoken. |
 
   **Aanvraagtekst**

   ```JSON
    {
       "identity":{
          "type":"SystemAssigned,UserAssigned",
          "userAssignedIdentities":{
             "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1":{
             },
             "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2":{
    
             }
          }
       }
    }
   ```
   
   **API-VERSIE 2017-12-01**

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"SystemAssigned,UserAssigned", "identityIds":["/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1","/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2"]}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2017-12-01 HTTP/1.1
   ```

   **Aanvraagheaders**

   |Aanvraagheader  |Description  |
   |---------|---------|
   |*Content-Type*     | Vereist. Ingesteld op `application/json`.        |
   |*Autorisatie*     | Vereist. Ingesteld op een geldige `Bearer` toegangstoken. | 

   **Aanvraagtekst**

   ```JSON
    {
       "identity":{
          "type":"SystemAssigned,UserAssigned",
          "identityIds":[
             "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1",
             "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2"
          ]
       }
    }
   ```

### <a name="disable-system-assigned-managed-identity-from-a-virtual-machine-scale-set"></a>Het systeem toegewezen beheerde identiteit van een virtuele-machineschaalset uitschakelen

Als u wilt een systeem toegewezen identiteit op een bestaande virtuele-machineschaalset uitschakelen, moet u een toegangstoken verkrijgen en vervolgens gebruiken we CURL om aan te roepen van het Resource Manager REST-eindpunt voor het bijwerken van het identiteitstype in `None`.

1. Een Bearer-toegangstoken, die u in de volgende stap in de autorisatie-header wordt gebruiken om uw virtuele-machineschaalset met een beheerde identiteit voor het systeem toegewezen ophalen.

   ```azurecli-interactive
   az account get-access-token
   ```

2. Bijwerken van de virtuele-machineschaalset met CURL om aan te roepen van het Azure Resource Manager REST-eindpunt om uit te schakelen door het systeem toegewezen beheerde identiteit.  Het volgende voorbeeld wordt het systeem toegewezen beheerde identiteit zoals vermeld in de hoofdtekst van de aanvraag door de waarde `{"identity":{"type":"None"}}` van een virtuele-machineschaalset met de naam *myVMSS*.  Vervang `<ACCESS TOKEN>` met de waarde u hebt ontvangen in de vorige stap wanneer u een Bearer-toegangstoken aangevraagd en de `<SUBSCRIPTION ID>` waarde als geschikt is voor uw omgeving.

   > [!IMPORTANT]
   > Als u wilt zorgen dat u geen bestaande gebruiker toegewezen beheerde identiteiten die zijn toegewezen aan de virtuele-machineschaalset niet verwijderen, moet u de beheerde gebruiker toegewezen identiteiten weergeven door met de volgende CURL-opdracht: `curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachineScaleSets/<VMSS NAME>?api-version=2018-06-01' -H "Authorization: Bearer <ACCESS TOKEN>"`. Als u een gebruiker toegewezen beheerde identiteit toegewezen aan de virtuele-machineschaalset hebt, gaat u naar stap 3 die laat u hoe de beheerde gebruiker toegewezen identiteiten behouden zien tijdens het verwijderen van het systeem toegewezen beheerde identiteit van uw virtuele-machineschaalset.

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"None"}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01 HTTP/1.1
   ```

   **Aanvraagheaders**

   |Aanvraagheader  |Description  |
   |---------|---------|
   |*Content-Type*     | Vereist. Ingesteld op `application/json`.        |
   |*Autorisatie*     | Vereist. Ingesteld op een geldige `Bearer` toegangstoken. | 

   **Aanvraagtekst**

   ```JSON
    {
       "identity":{
          "type":"None"
       }
    }
   ```

   Als u het systeem toegewezen beheerde identiteit van een virtuele-machineschaalset waarvoor de gebruiker toegewezen beheerde identiteiten, wilt verwijderen `SystemAssigned` uit de `{"identity":{"type:" "}}` waarde terwijl de `UserAssigned` waarde en de `userAssignedIdentities` woordenlijst waarden als u met behulp van zijn **API-versie 2018-01-06**. Als u **API-versie 2017-12-01** of eerder, houden de `identityIds` matrix.

## <a name="user-assigned-managed-identity"></a>de gebruiker toegewezen beheerde identiteit

In deze sectie leert u hoe u toevoegen en verwijderen van beheerde identiteit op een virtuele-machineschaalset met CURL aanroepen met de Azure Resource Manager REST-eindpunt instellen door de gebruiker toegewezen.

### <a name="assign-a-user-assigned-managed-identity-during-the-creation-of-a-virtual-machine-scale-set"></a>Een gebruiker toegewezen beheerde identiteit toewijzen tijdens het maken van een virtuele-machineschaalset

1. Een Bearer-toegangstoken, die u in de volgende stap in de autorisatie-header wordt gebruiken om uw virtuele-machineschaalset met een beheerde identiteit voor het systeem toegewezen ophalen.

   ```azurecli-interactive
   az account get-access-token
   ```

2. Maak een [netwerkinterface](/cli/azure/network/nic?view=azure-cli-latest#az-network-nic-create) instellen voor uw virtuele-machineschaalsets:

   ```azurecli-interactive
    az network nic create -g myResourceGroup --vnet-name myVnet --subnet mySubnet -n myNic
   ```

3.  Een Bearer-toegangstoken, die u in de volgende stap in de autorisatie-header wordt gebruiken om uw virtuele-machineschaalset met een beheerde identiteit voor het systeem toegewezen ophalen.

   ```azurecli-interactive
   az account get-access-token
   ``` 

4. Maak een gebruiker toegewezen beheerde identiteit met behulp van de instructies hier vinden: [Maken van een gebruiker toegewezen beheerde identiteit](how-to-manage-ua-identity-rest.md#create-a-user-assigned-managed-identity).

5. Maak een VM-schaalset met CURL om aan te roepen van het Azure Resource Manager REST-eindpunt. Het volgende voorbeeld wordt een virtuele-machineschaalset met de naam *myVMSS* in de resourcegroep *myResourceGroup* met een gebruiker toegewezen beheerde identiteit `ID1`, zoals vermeld in de aanvraagtekst door de waarde `"identity":{"type":"UserAssigned"}`. Vervang `<ACCESS TOKEN>` met de waarde u hebt ontvangen in de vorige stap wanneer u een Bearer-toegangstoken aangevraagd en de `<SUBSCRIPTION ID>` waarde als geschikt is voor uw omgeving.
 
   **API-VERSIE 2018-06-01**

   ```bash   
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01' -X PUT -d '{"sku":{"tier":"Standard","capacity":3,"name":"Standard_D1_v2"},"location":"eastus","identity":{"type":"UserAssigned","userAssignedIdentities":{"/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1":{}}},"properties":{"overprovision":true,"virtualMachineProfile":{"storageProfile":{"imageReference":{"sku":"2016-Datacenter","publisher":"MicrosoftWindowsServer","version":"latest","offer":"WindowsServer"},"osDisk":{"caching":"ReadWrite","managedDisk":{"storageAccountType":"Standard_LRS"},"createOption":"FromImage"}},"osProfile":{"computerNamePrefix":"myVMSS","adminUsername":"azureuser","adminPassword":"myPassword12"},"networkProfile":{"networkInterfaceConfigurations":[{"name":"myVMSS","properties":{"primary":true,"enableIPForwarding":true,"ipConfigurations":[{"name":"myVMSS","properties":{"subnet":{"id":"/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet"}}}]}}]}},"upgradePolicy":{"mode":"Manual"}}}' -H "Content-Type: application/json" -H "Authorization: Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PUT https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01 HTTP/1.1
   ```

   **Aanvraagheaders**

   |Aanvraagheader  |Description  |
   |---------|---------|
   |*Content-Type*     | Vereist. Ingesteld op `application/json`.        |
   |*Autorisatie*     | Vereist. Ingesteld op een geldige `Bearer` toegangstoken. | 

   **Aanvraagtekst**

   ```JSON
    {
       "sku":{
          "tier":"Standard",
          "capacity":3,
          "name":"Standard_D1_v2"
       },
       "location":"eastus",
       "identity":{
          "type":"UserAssigned",
          "userAssignedIdentities":{
             "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1":{
    
             }
          }
       },
       "properties":{
          "overprovision":true,
          "virtualMachineProfile":{
             "storageProfile":{
                "imageReference":{
                   "sku":"2016-Datacenter",
                   "publisher":"MicrosoftWindowsServer",
                   "version":"latest",
                   "offer":"WindowsServer"
                },
                "osDisk":{
                   "caching":"ReadWrite",
                   "managedDisk":{
                      "storageAccountType":"Standard_LRS"
                   },
                   "createOption":"FromImage"
                }
             },
             "osProfile":{
                "computerNamePrefix":"myVMSS",
                "adminUsername":"azureuser",
                "adminPassword":"myPassword12"
             },
             "networkProfile":{
                "networkInterfaceConfigurations":[
                   {
                      "name":"myVMSS",
                      "properties":{
                         "primary":true,
                         "enableIPForwarding":true,
                         "ipConfigurations":[
                            {
                               "name":"myVMSS",
                               "properties":{
                                  "subnet":{
                                     "id":"/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet"
                                  }
                               }
                            }
                         ]
                      }
                   }
                ]
             }
          },
          "upgradePolicy":{
             "mode":"Manual"
          }
       }
    }
   ```   

   **API-VERSIE 2017-12-01**

   ```bash   
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2017-12-01' -X PUT -d '{"sku":{"tier":"Standard","capacity":3,"name":"Standard_D1_v2"},"location":"eastus","identity":{"type":"UserAssigned","identityIds":["/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"]},"properties":{"overprovision":true,"virtualMachineProfile":{"storageProfile":{"imageReference":{"sku":"2016-Datacenter","publisher":"MicrosoftWindowsServer","version":"latest","offer":"WindowsServer"},"osDisk":{"caching":"ReadWrite","managedDisk":{"storageAccountType":"Standard_LRS"},"createOption":"FromImage"}},"osProfile":{"computerNamePrefix":"myVMSS","adminUsername":"azureuser","adminPassword":"myPassword12"},"networkProfile":{"networkInterfaceConfigurations":[{"name":"myVMSS","properties":{"primary":true,"enableIPForwarding":true,"ipConfigurations":[{"name":"myVMSS","properties":{"subnet":{"id":"/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet"}}}]}}]}},"upgradePolicy":{"mode":"Manual"}}}' -H "Content-Type: application/json" -H "Authorization: Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PUT https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2017-12-01 HTTP/1.1
   ```

   **Aanvraagheaders**

   |Aanvraagheader  |Description  |
   |---------|---------|
   |*Content-Type*     | Vereist. Ingesteld op `application/json`.        |
   |*Autorisatie*     | Vereist. Ingesteld op een geldige `Bearer` toegangstoken. |
 
   **Aanvraagtekst**

   ```JSON
    {
       "sku":{
          "tier":"Standard",
          "capacity":3,
          "name":"Standard_D1_v2"
       },
       "location":"eastus",
       "identity":{
          "type":"UserAssigned",
          "identityIds":[
             "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"
          ]
       },
       "properties":{
          "overprovision":true,
          "virtualMachineProfile":{
             "storageProfile":{
                "imageReference":{
                   "sku":"2016-Datacenter",
                   "publisher":"MicrosoftWindowsServer",
                   "version":"latest",
                   "offer":"WindowsServer"
                },
                "osDisk":{
                   "caching":"ReadWrite",
                   "managedDisk":{
                      "storageAccountType":"Standard_LRS"
                   },
                   "createOption":"FromImage"
                }
             },
             "osProfile":{
                "computerNamePrefix":"myVMSS",
                "adminUsername":"azureuser",
                "adminPassword":"myPassword12"
             },
             "networkProfile":{
                "networkInterfaceConfigurations":[
                   {
                      "name":"myVMSS",
                      "properties":{
                         "primary":true,
                         "enableIPForwarding":true,
                         "ipConfigurations":[
                            {
                               "name":"myVMSS",
                               "properties":{
                                  "subnet":{
                                     "id":"/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet"
                                  }
                               }
                            }
                         ]
                      }
                   }
                ]
             }
          },
          "upgradePolicy":{
             "mode":"Manual"
          }
       }
    }
   ```

### <a name="assign-a-user-assigned-managed-identity-to-an-existing-azure-virtual-machine-scale-set"></a>Een gebruiker toegewezen beheerde identiteit toewijzen aan een bestaande schaalset voor virtuele Azure-machine

1. Een Bearer-toegangstoken, die u in de volgende stap in de autorisatie-header wordt gebruiken om uw virtuele-machineschaalset met een beheerde identiteit voor het systeem toegewezen ophalen.

   ```azurecli-interactive
   az account get-access-token
   ```

2.  Maken van een gebruiker toegewezen beheerde identiteit door de instructies hier [maken van een gebruiker toegewezen beheerde identiteit](how-to-manage-ua-identity-rest.md#create-a-user-assigned-managed-identity).

3. Om te controleren of dat u bestaande gebruiker of systeem toegewezen beheerde identiteiten die zijn toegewezen aan de virtuele-machineschaalset niet verwijderen, moet u de identiteit van de typen die zijn toegewezen aan de virtuele-machineschaalset instellen met behulp van de volgende CURL-opdracht weergeven. Als u identiteiten die zijn toegewezen aan de virtuele-machineschaalset beheerde, ze worden weergegeven in de `identity` waarde.

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachineScaleSets/<VMSS NAME>?api-version=2018-06-01' -H "Authorization: Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   GET https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachineScaleSets/<VMSS NAME>?api-version=2018-06-01 HTTP/1.1
   ```

   **Aanvraagheaders**

   |Aanvraagheader  |Description  |
   |---------|---------|
   |*Autorisatie*     | Vereist. Ingesteld op een geldige `Bearer` toegangstoken. |   
 

4. Als u niet beschikt over een gebruiker of beheerd door het systeem toegewezen identiteiten die zijn toegewezen aan uw virtuele-machineschaalset instellen, gebruikt u de volgende CURL-opdracht om aan te roepen van het Azure Resource Manager REST-eindpunt voor de eerste gebruiker toegewezen beheerde identiteit toewijzen aan de virtuele machine schaalset.  Als u een gebruiker of systeem toegewezen beheerde identity(s) toegewezen aan de virtuele-machineschaalset hebt, gaat u naar stap 5 waarin u meerdere gebruiker toegewezen beheerde identiteiten toevoegen aan een virtuele-machineschaalset terwijl ook het systeem toegewezen beheerd de identiteit.

   Het volgende voorbeeld wordt een gebruiker toegewezen beheerde identiteit, `ID1` aan een virtuele-machineschaalset met de naam *myVMSS* in de resourcegroep *myResourceGroup*.  Vervang `<ACCESS TOKEN>` met de waarde u hebt ontvangen in de vorige stap wanneer u een Bearer-toegangstoken aangevraagd en de `<SUBSCRIPTION ID>` waarde als geschikt is voor uw omgeving.

   **API-VERSIE 2018-06-01**

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-12-01' -X PATCH -d '{"identity":{"type":"userAssigned", "userAssignedIdentities":{"/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1":{}}}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-12-01 HTTP/1.1
   ```

   **Aanvraagheaders**

   |Aanvraagheader  |Description  |
   |---------|---------|
   |*Content-Type*     | Vereist. Ingesteld op `application/json`.        |
   |*Autorisatie*     | Vereist. Ingesteld op een geldige `Bearer` toegangstoken. | 

   **Aanvraagtekst**

   ```JSON
    {
       "identity":{
          "type":"userAssigned",
          "userAssignedIdentities":{
             "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1":{
    
             }
          }
       }
    }
   ``` 
    
   **API-VERSIE 2017-12-01**

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"userAssigned", "identityIds":["/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"]}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2017-12-01 HTTP/1.1
   ```

   **Aanvraagheaders**

   |Aanvraagheader  |Description  |
   |---------|---------|
   |*Content-Type*     | Vereist. Ingesteld op `application/json`.        |
   |*Autorisatie*     | Vereist. Ingesteld op een geldige `Bearer` toegangstoken. | 

   **Aanvraagtekst**

   ```JSON
    {
       "identity":{
          "type":"userAssigned",
          "identityIds":[
             "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"
          ]
       }
    }
   ```  

5. Als u een bestaande gebruiker toegewezen of door het systeem toegewezen identiteit toegewezen aan uw virtuele-machineschaalset beheerd:
   
   **API-VERSIE 2018-06-01**

   Toevoegen van de gebruiker toegewezen beheerde identiteit op de `userAssignedIdentities` dictionary-waarde.

   Bijvoorbeeld, als u het systeem toegewezen beheerde identiteit en de gebruiker toegewezen beheerde identiteit `ID1` momenteel toegewezen aan uw virtuele-machineschaalset en wilt toevoegen van de gebruiker toegewezen beheerde identiteit `ID2` toe:

   ```bash
   curl  'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"SystemAssigned, UserAssigned", "userAssignedIdentities":{"/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1":{},"/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2":{}}}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01 HTTP/1.1
   ```

   **Aanvraagheaders**

   |Aanvraagheader  |Description  |
   |---------|---------|
   |*Content-Type*     | Vereist. Ingesteld op `application/json`.        |
   |*Autorisatie*     | Vereist. Ingesteld op een geldige `Bearer` toegangstoken. | 

   **Aanvraagtekst**

   ```JSON
    {
       "identity":{
          "type":"SystemAssigned, UserAssigned",
          "userAssignedIdentities":{
             "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1":{
    
             },
             "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2":{
    
             }
          }
       }
    }
   ```

   **API-VERSIE 2017-12-01**

   Behouden van de gebruiker toegewezen beheerde identiteiten u behouden wilt de `identityIds` matrix van waarde tijdens het toevoegen van de nieuwe gebruiker toegewezen beheerde identiteit.

   Bijvoorbeeld, als u het systeem toegewezen identiteit en de gebruiker toegewezen beheerde identiteit `ID1` momenteel toegewezen aan uw virtuele-machineschaalset en wilt toevoegen van de gebruiker toegewezen beheerde identiteit `ID2` toe:

    ```bash
   curl  'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"SystemAssigned, UserAssigned", "identityIds":["/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1","/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2"]}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2017-12-01 HTTP/1.1
   ```

    **Aanvraagheaders**

   |Aanvraagheader  |Description  |
   |---------|---------|
   |*Content-Type*     | Vereist. Ingesteld op `application/json`.        |
   |*Autorisatie*     | Vereist. Ingesteld op een geldige `Bearer` toegangstoken. | 

   **Aanvraagtekst**

   ```JSON
    {
       "identity":{
          "type":"SystemAssigned, UserAssigned",
          "identityIds":[
             "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1",
             "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2"
          ]
       }
    }
   ```

### <a name="remove-a-user-assigned-managed-identity-from-a-virtual-machine-scale-set"></a>Een gebruiker toegewezen beheerde identiteit van een virtuele-machineschaalset verwijderen

1. Een Bearer-toegangstoken, die u in de volgende stap in de autorisatie-header wordt gebruiken om uw virtuele-machineschaalset met een beheerde identiteit voor het systeem toegewezen ophalen.

   ```azurecli-interactive
   az account get-access-token
   ```

2. Als u wilt zorgen dat u geen bestaande gebruiker toegewezen beheerde identiteiten die u wilt behouden toegewezen aan de virtuele-machineschaalset of verwijderen van het systeem toegewezen beheerde identiteit niet verwijdert, moet u de beheerde identiteiten weergeven met behulp van de volgende CURL-opdracht :

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachineScaleSets/<VMSS NAME>?api-version=2018-06-01' -H "Authorization: Bearer <ACCESS TOKEN>" 
   ```

   ```HTTP
   GET https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachineScaleSets/<VMSS NAME>?api-version=2018-06-01 HTTP/1.1
   ```

   **Aanvraagheaders**

   |Aanvraagheader  |Description  |
   |---------|---------|
   |*Autorisatie*     | Vereist. Ingesteld op een geldige `Bearer` toegangstoken. |
   
   Als u identiteiten die aan de virtuele machine is toegewezen beheerde, worden deze weergegeven in het antwoord in de `identity` waarde. 
    
   Bijvoorbeeld, als er een gebruiker toegewezen identiteiten op beheerde `ID1` en `ID2` toegewezen aan uw virtuele-machineschaalset en u alleen wilt behouden `ID1` toegewezen en de door het systeem toegewezen beheerde identiteit behouden:

   **API-VERSIE 2018-06-01**

   Voeg `null` beheerd naar de gebruiker toegewezen identiteit die u wilt verwijderen:

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"SystemAssigned, UserAssigned", "userAssignedIdentities":{"/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2":null}}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01 HTTP/1.1
   ```

   **Aanvraagheaders**

   |Aanvraagheader  |Description  |
   |---------|---------|
   |*Content-Type*     | Vereist. Ingesteld op `application/json`.        |
   |*Autorisatie*     | Vereist. Ingesteld op een geldige `Bearer` toegangstoken. | 

   **Aanvraagtekst**

   ```JSON
    {
       "identity":{
          "type":"SystemAssigned, UserAssigned",
          "userAssignedIdentities":{
             "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2":null
          }
       }
    }
   ```

   **API-VERSIE 2017-12-01**

   Alleen de gebruiker toegewezen identity(s) die u behouden wilt beheerde behouden de `identityIds` matrix:

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"SystemAssigned,UserAssigned", "identityIds":["/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"]}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```   

   ```HTTP
   PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2017-12-01 HTTP/1.1
   ```

   **Aanvraagheaders**

   |Aanvraagheader  |Description  |
   |---------|---------|
   |*Content-Type*     | Vereist. Ingesteld op `application/json`.        |
   |*Autorisatie*     | Vereist. Ingesteld op een geldige `Bearer` toegangstoken. | 

   **Aanvraagtekst**

   ```JSON
    {
       "identity":{
          "type":"SystemAssigned,UserAssigned",
          "identityIds":[
             "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"
          ]
       }
    }
   ```

Als uw virtuele-machineschaalset beide door het systeem is toegewezen en gebruiker toegewezen beheerde identiteiten, kunt u alle van de gebruiker toegewezen beheerde identiteiten door over te schakelen voor het gebruik van alleen systeem toegewezen met behulp van de volgende opdracht uit:

```bash
curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"SystemAssigned"}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
```

```HTTP
PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01 HTTP/1.1
```

**Aanvraagheaders**

|Aanvraagheader  |Description  |
|---------|---------|
|*Content-Type*     | Vereist. Ingesteld op `application/json`.        |
|*Autorisatie*     | Vereist. Ingesteld op een geldige `Bearer` toegangstoken. | 

**Aanvraagtekst**

```JSON
{
   "identity":{
      "type":"SystemAssigned"
   }
}
```
    
Als uw virtuele-machineschaalset beheerde identiteiten alleen door de gebruiker toegewezen heeft en u graag zou willen ze allemaal verwijderen, gebruikt u de volgende opdracht uit:

```bash
curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"None"}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
```

```HTTP
PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01 HTTP/1.1
```

**Aanvraagheaders**

|Aanvraagheader  |Description  |
|---------|---------|
|*Content-Type*     | Vereist. Ingesteld op `application/json`.        |
|*Autorisatie*     | Vereist. Ingesteld op een geldige `Bearer` toegangstoken. | 

**Aanvraagtekst**

```JSON
{
   "identity":{
      "type":"None"
   }
}
```

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het maken, weergeven of verwijderen van beheerde identiteiten gebruiker toegewezen met behulp van REST:

- [Maken, weergeven of verwijderen van een gebruiker toegewezen beheerde identiteit met behulp van REST-API aanroepen](how-to-manage-ua-identity-rest.md)
