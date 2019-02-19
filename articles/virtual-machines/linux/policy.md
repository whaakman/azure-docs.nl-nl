---
title: Afdwingen van beveiliging met beleidsregels op virtuele Linux-machines in Azure | Microsoft Docs
description: Hoe u een beleid toepassen op een Azure Resource Manager virtuele Linux-Machine
services: virtual-machines-linux
documentationcenter: ''
author: singhkays
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 06778ab4-f8ff-4eed-ae10-26a276fc3faa
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: singhkay
ms.openlocfilehash: 8ad1bf371c5d5dbcbf3657ad69eace2003a8dda9
ms.sourcegitcommit: fcb674cc4e43ac5e4583e0098d06af7b398bd9a9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/18/2019
ms.locfileid: "56342006"
---
# <a name="apply-policies-to-linux-vms-with-azure-resource-manager"></a>Beleid toepassen op virtuele Linux-machines met Azure Resource Manager
Met behulp van beleid, kan het afdwingen van een organisatie verschillende conventies en regels in de hele onderneming. Afdwinging van het gewenste gedrag kunt risico's te beperken tijdens een bijdrage levert aan het succes van de organisatie. In dit artikel wordt beschreven hoe u Azure Resource Manager-beleid kunt gebruiken voor het definiëren van het gewenste gedrag voor virtuele Machines die uw organisatie.

Zie voor een inleiding tot beleid, [wat is Azure Policy?](../../governance/policy/overview.md).

## <a name="permitted-virtual-machines"></a>Toegestane virtuele Machines
Om ervoor te zorgen dat virtuele machines voor uw organisatie compatibel met een toepassing zijn, kunt u de toegestane besturingssystemen beperken. In het volgende voorbeeld van beleid kunt u alleen Ubuntu 14.04.2-LTS virtuele Machines moet worden gemaakt.

```json
{
  "if": {
    "allOf": [
      {
        "field": "type",
        "in": [
          "Microsoft.Compute/disks",
          "Microsoft.Compute/virtualMachines",
          "Microsoft.Compute/VirtualMachineScaleSets"
        ]
      },
      {
        "not": {
          "allOf": [
            {
              "field": "Microsoft.Compute/imagePublisher",
              "in": [
                "Canonical"
              ]
            },
            {
              "field": "Microsoft.Compute/imageOffer",
              "in": [
                "UbuntuServer"
              ]
            },
            {
              "field": "Microsoft.Compute/imageSku",
              "in": [
                "14.04.2-LTS"
              ]
            },
            {
              "field": "Microsoft.Compute/imageVersion",
              "in": [
                "latest"
              ]
            }
          ]
        }
      }
    ]
  },
  "then": {
    "effect": "deny"
  }
}
```

Een jokerteken gebruiken om te wijzigen van het vorige beleid om toe te staan een Ubuntu LTS-installatiekopie: 

```json
{
  "field": "Microsoft.Compute/virtualMachines/imageSku",
  "like": "*LTS"
}
```

Zie voor meer informatie over de velden voor het beleid [beleid aliassen](../../governance/policy/concepts/definition-structure.md#aliases).

## <a name="managed-disks"></a>Managed Disks

Om te vereisen dat het gebruik van beheerde schijven, gebruikt u het volgende beleid:

```json
{
  "if": {
    "anyOf": [
      {
        "allOf": [
          {
            "field": "type",
            "equals": "Microsoft.Compute/virtualMachines"
          },
          {
            "field": "Microsoft.Compute/virtualMachines/osDisk.uri",
            "exists": true
          }
        ]
      },
      {
        "allOf": [
          {
            "field": "type",
            "equals": "Microsoft.Compute/VirtualMachineScaleSets"
          },
          {
            "anyOf": [
              {
                "field": "Microsoft.Compute/VirtualMachineScaleSets/osDisk.vhdContainers",
                "exists": true
              },
              {
                "field": "Microsoft.Compute/VirtualMachineScaleSets/osdisk.imageUrl",
                "exists": true
              }
            ]
          }
        ]
      }
    ]
  },
  "then": {
    "effect": "deny"
  }
}
```

## <a name="images-for-virtual-machines"></a>Installatiekopieën voor virtuele Machines

Uit veiligheidsoverwegingen kunt u vereisen dat alleen goedgekeurde aangepaste installatiekopieën in uw omgeving zijn geïmplementeerd. Kunt u ofwel de resourcegroep die de goedgekeurde installatiekopieën bevat, of de specifieke goedgekeurde installatiekopieën.

Het volgende voorbeeld is afbeeldingen uit een goedgekeurde resourcegroep vereist:

```json
{
    "if": {
        "allOf": [
            {
                "field": "type",
                "in": [
                    "Microsoft.Compute/virtualMachines",
                    "Microsoft.Compute/VirtualMachineScaleSets"
                ]
            },
            {
                "not": {
                    "field": "Microsoft.Compute/imageId",
                    "contains": "resourceGroups/CustomImage"
                }
            }
        ]
    },
    "then": {
        "effect": "deny"
    }
} 
```

Het volgende voorbeeld geeft de goedgekeurde installatiekopie-id's:

```json
{
    "field": "Microsoft.Compute/imageId",
    "in": ["{imageId1}","{imageId2}"]
}
```

## <a name="virtual-machine-extensions"></a>Extensies voor virtuele machines

U kunt het gebruik van bepaalde typen extensies verbieden. Bijvoorbeeld, een extensie mogelijk niet compatibel is met bepaalde installatiekopieën voor aangepaste virtuele machine. Het volgende voorbeeld ziet hoe u een bepaalde extensie blokkeren. Uitgever en type worden gebruikt om te bepalen welke extensie om te blokkeren.

```json
{
    "if": {
        "allOf": [
            {
                "field": "type",
                "equals": "Microsoft.Compute/virtualMachines/extensions"
            },
            {
                "field": "Microsoft.Compute/virtualMachines/extensions/publisher",
                "equals": "Microsoft.Compute"
            },
            {
                "field": "Microsoft.Compute/virtualMachines/extensions/type",
                "equals": "{extension-type}"

      }
        ]
    },
    "then": {
        "effect": "deny"
    }
}
```


## <a name="next-steps"></a>Volgende stappen
* Na het definiëren van een beleidsregel (zoals weergegeven in de voorgaande voorbeelden), moet u de beleidsdefinitie van de maken en deze toewijzen aan een bereik. Het bereik mag een abonnement, resourcegroep of resource. Als u wilt toewijzen van beleid, Zie [gebruik Azure portal voor het toewijzen en beheren van resourcebeleid](../../governance/policy/assign-policy-portal.md), [Gebruik PowerShell om toe te wijzen beleid](../../governance/policy/assign-policy-powershell.md), of [gebruik Azure CLI voor het toewijzen van beleidsregels](../../governance/policy/assign-policy-azurecli.md).
* Zie voor een inleiding tot resourcebeleid, [wat is Azure Policy?](../../governance/policy/overview.md).
* Voor begeleiding bij de manier waarop ondernemingen Resource Manager effectief kunnen gebruiken voor het beheer van abonnementen, gaat u naar [Azure enterprise-platform - Prescriptieve abonnementsgovernance](/azure/architecture/cloud-adoption-guide/subscription-governance).
