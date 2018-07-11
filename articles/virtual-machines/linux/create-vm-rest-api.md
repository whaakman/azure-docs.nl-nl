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
ms.openlocfilehash: 0f77b46be0207b0ce96e6dc2562fb5298afbe36b
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37928179"
---
# <a name="create-a-linux-virtual-machine-that-uses-ssh-authentication-with-the-rest-api"></a>Een Linux-machine die gebruikmaakt van SSH-verificatie met de REST-API maken

Een virtuele machine (VM) in Azure wordt gedefinieerd door de verschillende parameters, zoals locatie, de grootte van de hardware, de installatiekopie van besturingssysteem en de aanmeldingsreferenties. Dit artikel leest u hoe de REST-API gebruiken om te maken van een virtuele Linux-machine die gebruikmaakt van SSH-verificatie.

Als u wilt maken of bijwerken van een virtuele machine, gebruikt u de volgende *plaatsen* bewerking:

``` http
PUT https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachines/{vmName}?api-version=2017-12-01
```

## <a name="create-a-request"></a>Een aanvraag maken

Maakt de *plaatsen* aanvraag, de `{subscription-id}` parameter is vereist. Als u meerdere abonnementen hebt, raadpleegt u [werken met meerdere abonnementen](/cli/azure/manage-azure-subscriptions-azure-cli?view=azure-cli-latest#working-with-multiple-subscriptions). U definieert een `{resourceGroupName}` en `{vmName}` voor uw resources samen met de `api-version` parameter. In dit artikel wordt gebruikgemaakt van `api-version=2017-12-01`.

De volgende headers zijn vereist:

| Aanvraagheader   | Beschrijving |
|------------------|-----------------|
| *Content-Type:*  | Vereist. Ingesteld op `application/json`. |
| *Autorisatie:* | Vereist. Ingesteld op een geldige `Bearer` [toegangstoken](https://docs.microsoft.com/rest/api/azure/#authorization-code-grant-interactive-clients). |

Zie voor meer informatie over het maken van de aanvraag [onderdelen van een REST-API-aanvraag/antwoord](/rest/api/azure/#components-of-a-rest-api-requestresponse).

## <a name="create-the-request-body"></a>Hoofdtekst van de aanvraag maken

De volgende algemene definities worden gebruikt voor het bouwen van een aanvraagtekst:

| Naam                       | Vereist | Type                                                                                | Beschrijving  |
|----------------------------|----------|-------------------------------------------------------------------------------------|--------------|
| location                   | True     | tekenreeks                                                                              | Resourcelocatie. |
| naam                       |          | tekenreeks                                                                              | Naam voor de virtuele machine. |
| properties.hardwareProfile |          | [Het HardwareProfile](/rest/api/compute/virtualmachines/createorupdate#hardwareprofile) | Hiermee geeft u de hardware-instellingen voor de virtuele machine. |
| properties.storageProfile  |          | [StorageProfile](/rest/api/compute/virtualmachines/createorupdate#storageprofile)   | Hiermee geeft u de opslaginstellingen voor de virtuele-machineschijven. |
| properties.osProfile       |          | [OSProfile](/rest/api/compute/virtualmachines/createorupdate#osprofile)             | Hiermee geeft u de instellingen van het besturingssysteem voor de virtuele machine. |
| properties.networkProfile  |          | [NetworkProfile](/rest/api/compute/virtualmachines/createorupdate#networkprofile)   | Hiermee geeft u de netwerkinterfaces van de virtuele machine. |

Zie voor een volledige lijst van de beschikbare definities in de aanvraagtekst [virtuele machines maken of bijwerken van de aanvraag hoofdtekst defintions](/rest/api/compute/virtualmachines/createorupdate#definitions).

### <a name="example-request-body"></a>Voorbeeld van de aanvraag hoofdtekst

De aanvraagtekst van de volgende voorbeeld definieert een Ubuntu 18.04 LTS-installatiekopie die gebruikmaakt van Premium-beheerde schijven. SSH-verificatie voor openbare sleutel wordt gebruikt en de virtuele machine maakt gebruik van een bestaande virtuele netwerkinterfacekaart (NIC) die u hebt [eerder hebt gemaakt](../../virtual-network/virtual-network-network-interface.md). Geef uw openbare SSH-sleutel in de *osProfile.linuxConfiguration.ssh.publicKeys.keyData* veld. Indien nodig, kunt u [een SSH-sleutelpaar genereren](mac-create-ssh-keys.md).

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

Er zijn twee gelukt-antwoorden voor de bewerking voor het maken of bijwerken van een virtuele machine:

| Naam        | Type                                                                              | Beschrijving |
|-------------|-----------------------------------------------------------------------------------|-------------|
| 200 OK      | [VirtualMachine](/rest/api/compute/virtualmachines/createorupdate#virtualmachine) | OK          |
| 201-gemaakt | [VirtualMachine](/rest/api/compute/virtualmachines/createorupdate#virtualmachine) | Gemaakt     |

Zie voor meer informatie over de REST-API-reacties [verwerken van het antwoordbericht](/rest/api/azure/#process-the-response-message).

### <a name="example-response"></a>Voorbeeld van een antwoord

Een verkorte *201-gemaakt* antwoord van het vorige voorbeeld van de hoofdtekst van de aanvraag die wordt gemaakt van een virtuele machine blijkt een *vmId* is toegewezen en de *provisioningState* is*Maken*:

```json
{
    "vmId": "e0de9b84-a506-4b95-9623-00a425d05c90",
    "provisioningState": "Creating"
}
```

## <a name="next-steps"></a>Volgende stappen

Zie de volgende onderwerpen voor meer informatie over de Azure REST API's of andere beheerprogramma's zoals Azure CLI 2.0 of Azure PowerShell:

- [Azure Compute-provider REST-API](/rest/api/compute/)
- [Aan de slag met REST API van Azure](/rest/api/azure/)
- [Azure CLI 2.0](/cli/azure/)
- [Azure PowerShell-module](/powershell/azure/overview)
