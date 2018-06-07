---
title: Een virtuele Linux-machine maken met de REST-API van Azure | Microsoft Docs
description: Informatie over het maken van een virtuele Linux-machine in Azure die gebruikmaakt van schijven beheerd en SSH-verificatie met Azure REST-API.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
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
ms.author: iainfou
ms.openlocfilehash: e3f41bea26e9a5ff45b31ae9d9a2e5955317ad7a
ms.sourcegitcommit: 3017211a7d51efd6cd87e8210ee13d57585c7e3b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/06/2018
ms.locfileid: "34825201"
---
# <a name="create-a-linux-virtual-machine-that-uses-ssh-authentication-with-the-rest-api"></a>Een virtuele Linux-machine die gebruikmaakt van SSH-verificatie met de REST-API maken

Een virtuele machine (VM) in Azure wordt gedefinieerd door verschillende parameters zoals locatie, de grootte van de hardware, de installatiekopie van het besturingssysteem en aanmeldingsreferenties. In dit artikel leest u hoe de REST-API gebruiken om te maken van een virtuele Linux-machine die gebruikmaakt van SSH-verificatie.

Als u wilt maken of bijwerken van een virtuele machine, gebruikt u de volgende *plaatsen* bewerking:

``` http
PUT https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachines/{vmName}?api-version=2017-12-01
```

## <a name="create-a-request"></a>Een aanvraag maken

Maken van de *plaatsen* aanvraag, de `{subscription-id}` parameter is vereist. Als u meerdere abonnementen hebt, raadpleegt u [werken met meerdere abonnementen](/cli/azure/manage-azure-subscriptions-azure-cli?view=azure-cli-latest#working-with-multiple-subscriptions). U definieert een `{resourceGroupName}` en `{vmName}` voor uw resources samen met de `api-version` parameter. Dit artikel wordt `api-version=2017-12-01`.

De volgende headers zijn vereist:

| Aanvraag-header   | Beschrijving |
|------------------|-----------------|
| *Content-Type:*  | Vereist. Ingesteld op `application/json`. |
| *Autorisatie:* | Vereist. Ingesteld op een geldige `Bearer` [toegangstoken](https://docs.microsoft.com/rest/api/azure/#authorization-code-grant-interactive-clients). |

Zie voor meer informatie over het maken van de aanvraag [onderdelen van een REST-API-aanvragen/reacties](/rest/api/azure/#components-of-a-rest-api-requestresponse).

## <a name="create-the-request-body"></a>De aanvraagtekst maken

De volgende algemene definities worden gebruikt voor het bouwen van een aanvraagtekst:

| Naam                       | Vereist | Type                                                                                | Beschrijving  |
|----------------------------|----------|-------------------------------------------------------------------------------------|--------------|
| location                   | True     | tekenreeks                                                                              | Resourcelocatie. |
| naam                       |          | tekenreeks                                                                              | Naam voor de virtuele machine. |
| properties.hardwareProfile |          | [Het HardwareProfile](/rest/api/compute/virtualmachines/createorupdate#hardwareprofile) | Hiermee geeft u de hardware-instellingen voor de virtuele machine. |
| properties.storageProfile  |          | [StorageProfile](/rest/api/compute/virtualmachines/createorupdate#storageprofile)   | Hiermee geeft u de instellingen voor de opslag voor de virtuele machine-schijven. |
| properties.osProfile       |          | [OSProfile](/rest/api/compute/virtualmachines/createorupdate#osprofile)             | Hiermee geeft u de instellingen van het besturingssysteem voor de virtuele machine. |
| properties.networkProfile  |          | [Schaalaanpassingsset](/rest/api/compute/virtualmachines/createorupdate#networkprofile)   | Hiermee geeft u de netwerkinterfaces van de virtuele machine. |

Zie voor een volledige lijst van de beschikbare definities in de aanvraagtekst [virtuele machines maken of bijwerken van de aanvraag hoofdtekst defintions](/rest/api/compute/virtualmachines/createorupdate#definitions).

### <a name="example-request-body"></a>Voorbeeld van de aanvraagtekst

Het volgende voorbeeld-aanvraagtekst definieert een Ubuntu 18.04 TNS-installatiekopie die gebruikmaakt van beheerde Premium-schijven. SSH-verificatie voor openbare sleutel wordt gebruikt en de virtuele machine gebruikmaakt van een bestaande virtuele netwerkinterfacekaart (NIC) die u hebt [eerder gemaakte](../../virtual-network/virtual-network-network-interface.md). Geef uw openbare SSH-sleutel in de *osProfile.linuxConfiguration.ssh.publicKeys.keyData* veld. Indien nodig, kunt u [een SSH-sleutelpaar genereren](mac-create-ssh-keys.md).

```json
{
  "location": "eastus",
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
      "computerName": "myVM",
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
          "id": "/subscriptions/{subscription-id}/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/{existing-nic-name}",
          "properties": {
            "primary": true
          }
        }
      ]
    }
  },
  "name": "myVM"
}
```

## <a name="responses"></a>Antwoorden

Er zijn twee geslaagde antwoorden voor de bewerking maken of bijwerken van een virtuele machine:

| Naam        | Type                                                                              | Beschrijving |
|-------------|-----------------------------------------------------------------------------------|-------------|
| 200 OK      | [Virtuele machine](/rest/api/compute/virtualmachines/createorupdate#virtualmachine) | OK          |
| 201 gemaakt | [Virtuele machine](/rest/api/compute/virtualmachines/createorupdate#virtualmachine) | Gemaakt     |

Zie voor meer informatie over REST API antwoorden [verwerken van het antwoordbericht](/rest/api/azure/#process-the-response-message).

### <a name="example-response"></a>Voorbeeld van een antwoord

Een verkorte *201 gemaakt* reactie van het vorige voorbeeld aanvraagtekst die wordt gemaakt van een virtuele machine bevat een *vmId* is toegewezen en de *provisioningState* is*Maken*:

```json
{
    "vmId": "e0de9b84-a506-4b95-9623-00a425d05c90",
    "provisioningState": "Creating"
}
```

## <a name="next-steps"></a>Volgende stappen

Zie de volgende onderwerpen voor meer informatie over de Azure REST-API's of andere beheerprogramma's, zoals Azure PowerShell of Azure CLI 2.0:

- [Azure Compute-provider REST-API](/rest/api/compute/)
- [Aan de slag met Azure REST-API](/rest/api/azure/)
- [Azure CLI 2.0](/cli/azure/)
- [Azure PowerShell-module](/powershell/azure/overview)
