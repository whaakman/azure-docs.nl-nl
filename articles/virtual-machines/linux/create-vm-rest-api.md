---
title: Een virtuele Linux-machine maken met de Azure REST API | Microsoft Docs
description: Informatie over het maken van een virtuele Linux-machine in Azure die gebruikmaakt van Managed Disks en SSH-verificatie met Azure REST-API.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/05/2018
ms.author: cynthn
ms.openlocfilehash: 22a800e65c0f64dfa897433d1ea983006ed62250
ms.sourcegitcommit: 82cdc26615829df3c57ee230d99eecfa1c4ba459
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2019
ms.locfileid: "54412186"
---
# <a name="create-a-linux-virtual-machine-that-uses-ssh-authentication-with-the-rest-api"></a>Een Linux-machine die gebruikmaakt van SSH-verificatie met de REST-API maken

Een Linux virtuele machine (VM) in Azure bestaat uit verschillende bronnen, zoals schijven en netwerk-interfaces en definieert parameters, zoals locatie, grootte en operating system image en verificatie-instellingen.

U kunt een Linux-VM via de Azure portal, Azure CLI 2.0, veel Azure-SDK's, Azure Resource Manager-sjablonen en veel hulpprogramma's van derden, zoals Ansible of Terraform maken. Deze hulpprogramma's uiteindelijk de REST-API gebruiken om de Linux-VM te maken.

Dit artikel leest u hoe de REST-API gebruiken om te maken van een Linux-VM waarop Ubuntu 18.04-LTS met beheerde schijven en SSH-verificatie wordt uitgevoerd.

## <a name="before-you-start"></a>Voordat u begint

Voordat u maken en de aanvraag indienen, gaat u te werk:

* De `{subscription-id}` voor uw abonnement
  * Als u meerdere abonnementen hebt, raadpleegt u [met meerdere abonnementen werken](/cli/azure/manage-azure-subscriptions-azure-cli?view=azure-cli-latest)
* Een `{resourceGroupName}` u vooraf hebt gemaakt
* Een [virtuele netwerkinterface](../../virtual-network/virtual-network-network-interface.md) in dezelfde resourcegroep bevinden
* Een SSH-sleutelpaar (u kunt [een nieuw token genereren](mac-create-ssh-keys.md) als u geen een hebt)

## <a name="request-basics"></a>Grondbeginselen van aanvraag

Als u wilt maken of bijwerken van een virtuele machine, gebruikt u de volgende *plaatsen* bewerking:

``` http
PUT https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachines/{vmName}?api-version=2017-12-01
```

Naast de `{subscription-id}` en `{resourceGroupName}` parameters, moet u om op te geven de `{vmName}` (`api-version` is optioneel, maar in dit artikel is getest met `api-version=2017-12-01`)

De volgende headers zijn vereist:

| Aanvraagheader   | Description |
|------------------|-----------------|
| *Content-Type:*  | Vereist. Ingesteld op `application/json`. |
| *Autorisatie:* | Vereist. Ingesteld op een geldige `Bearer` [toegangstoken](https://docs.microsoft.com/rest/api/azure/#authorization-code-grant-interactive-clients). |

Raadpleeg voor algemene informatie over het werken met REST API-aanvragen [onderdelen van een REST-API-aanvraag/antwoord](/rest/api/azure/#components-of-a-rest-api-requestresponse).

## <a name="create-the-request-body"></a>Hoofdtekst van de aanvraag maken

De volgende algemene definities worden gebruikt voor het bouwen van een aanvraagtekst:

| Name                       | Vereist | Type                                                                                | Description  |
|----------------------------|----------|-------------------------------------------------------------------------------------|--------------|
| location                   | True     | string                                                                              | Resourcelocatie. |
| naam                       |          | string                                                                              | Naam voor de virtuele machine. |
| properties.hardwareProfile |          | [HardwareProfile](/rest/api/compute/virtualmachines/createorupdate#hardwareprofile) | Hiermee geeft u de hardware-instellingen voor de virtuele machine. |
| properties.storageProfile  |          | [StorageProfile](/rest/api/compute/virtualmachines/createorupdate#storageprofile)   | Hiermee geeft u de opslaginstellingen voor de virtuele-machineschijven. |
| properties.osProfile       |          | [OSProfile](/rest/api/compute/virtualmachines/createorupdate#osprofile)             | Hiermee geeft u de instellingen van het besturingssysteem voor de virtuele machine. |
| properties.networkProfile  |          | [NetworkProfile](/rest/api/compute/virtualmachines/createorupdate#networkprofile)   | Hiermee geeft u de netwerkinterfaces van de virtuele machine. |

Een voorbeeld van de aanvraagtekst is lager dan. Zorg ervoor dat u de naam van de virtuele machine in de `{computerName}` en `{name}` parameters, de naam van de netwerkinterface die u hebt gemaakt onder `networkInterfaces`, uw gebruikersnaam in `adminUsername` en `path`, en de *openbare*gedeelte van uw SSH-sleutelpaar (zich in, bijvoorbeeld `~/.ssh/id_rsa.pub`) in `keyData`. Andere parameters die u wilt wijzigen bevatten `location` en `vmSize`.  

```json
{
  "location": "eastus",
  "name": "{vmName}",
  "properties": {
    "hardwareProfile": {
      "vmSize": "Standard_DS1_v2"
    },
    "storageProfile": {
      "imageReference": {
        "sku": "18.04-LTS",
        "publisher": "Canonical",
        "version": "latest",
        "offer": "UbuntuServer"
      },
      "osDisk": {
        "caching": "ReadWrite",
        "managedDisk": {
          "storageAccountType": "Premium_LRS"
        },
        "name": "myVMosdisk",
        "createOption": "FromImage"
      }
    },
    "osProfile": {
      "adminUsername": "{your-username}",
      "computerName": "{vmName}",
      "linuxConfiguration": {
        "ssh": {
          "publicKeys": [
            {
              "path": "/home/{your-username}/.ssh/authorized_keys",
              "keyData": "ssh-rsa AAAAB3NzaC1{snip}mf69/J1"
            }
          ]
        },
        "disablePasswordAuthentication": true
      }
    },
    "networkProfile": {
      "networkInterfaces": [
        {
          "id": "/subscriptions/{subscription-id}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkInterfaces/{existing-nic-name}",
          "properties": {
            "primary": true
          }
        }
      ]
    }
  }
}
```

Zie voor een volledige lijst van de beschikbare definities in de aanvraagtekst [virtuele machines maken of bijwerken van de aanvraag hoofdtekst defintions](/rest/api/compute/virtualmachines/createorupdate#definitions).

## <a name="sending-the-request"></a>De aanvraag wordt verzonden

U kunt de client van uw voorkeur voor het verzenden van deze HTTP-aanvraag. U kunt ook een [in de browser hulpprogramma](https://docs.microsoft.com/rest/api/compute/virtualmachines/createorupdate) door te klikken op de **uitproberen** knop.

### <a name="responses"></a>Antwoorden

Er zijn twee gelukt-antwoorden voor de bewerking voor het maken of bijwerken van een virtuele machine:

| Name        | Type                                                                              | Description |
|-------------|-----------------------------------------------------------------------------------|-------------|
| 200 OK      | [VirtualMachine](/rest/api/compute/virtualmachines/createorupdate#virtualmachine) | OK          |
| 201-gemaakt | [VirtualMachine](/rest/api/compute/virtualmachines/createorupdate#virtualmachine) | Gemaakt     |

Een verkorte *201-gemaakt* antwoord van het vorige voorbeeld van de hoofdtekst van de aanvraag die wordt gemaakt van een virtuele machine blijkt een *vmId* is toegewezen en de *provisioningState* is*Maken*:

```json
{
    "vmId": "e0de9b84-a506-4b95-9623-00a425d05c90",
    "provisioningState": "Creating"
}
```

Zie voor meer informatie over de REST-API-reacties [verwerken van het antwoordbericht](/rest/api/azure/#process-the-response-message).

## <a name="next-steps"></a>Volgende stappen

Zie de volgende onderwerpen voor meer informatie over de Azure REST API's of andere beheerprogramma's zoals Azure CLI of Azure PowerShell:

- [Azure Compute-provider REST-API](/rest/api/compute/)
- [Aan de slag met REST API van Azure](/rest/api/azure/)
- [Azure-CLI](/cli/azure/)
- [Azure PowerShell-module](/powershell/azure/overview)
