---
title: Beveiliging met beleidsregels op Linux-machines in Azure afdwingen | Microsoft Docs
description: Het toepassen van een beleid voor een Azure Resource Manager virtuele Linux-Machine
services: virtual-machines-linux
documentationcenter: 
author: singhkays
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 06778ab4-f8ff-4eed-ae10-26a276fc3faa
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: singhkay
ms.openlocfilehash: 58eaab4fa03afc1e6a5e38bef691cce62a921ea9
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="apply-policies-to-linux-vms-with-azure-resource-manager"></a>Beleid toepassen op Linux VM's met Azure Resource Manager
Een organisatie kan met behulp van beleid afdwingen verschillende conventies en regels in de hele onderneming. Afdwinging van het gewenste gedrag kunt risico's te beperken tijdens bijdragen aan het succes van de organisatie. In dit artikel wordt beschreven hoe u Azure Resource Manager-beleid kunt gebruiken voor het definiëren van het gewenste gedrag voor virtuele Machines die uw organisatie.

Zie voor een inleiding tot beleidsregels, [beleid gebruiken voor het beheren van resources en toegangsbeheer](../../azure-resource-manager/resource-manager-policy.md).

## <a name="permitted-virtual-machines"></a>Toegestane virtuele Machines
Om ervoor te zorgen dat virtuele machines voor uw organisatie compatibel met een toepassing zijn, kunt u de toegestane besturingssystemen beperken. In het volgende voorbeeld voor het beleid kunt u alleen Ubuntu 14.04.2-LTS virtuele Machines worden gemaakt.

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

Een jokerteken gebruiken voor het wijzigen van het vorige beleid zodat geen Ubuntu LTS afbeelding: 

```json
{
  "field": "Microsoft.Compute/virtualMachines/imageSku",
  "like": "*LTS"
}
```

Zie voor meer informatie over Beleidsvelden [beleid aliassen](../../azure-resource-manager/resource-manager-policy.md#aliases).

## <a name="managed-disks"></a>Managed Disks

Als u wilt het gebruik van beheerde schijven, gebruikt u het volgende beleid:

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

Uit veiligheidsoverwegingen kunt u vereisen dat alleen goedgekeurde aangepaste installatiekopieën in uw omgeving zijn geïmplementeerd. Kunt u ofwel de resourcegroep die de goedgekeurde installatiekopieën bevat, of de specifieke goedgekeurd installatiekopieën.

Het volgende voorbeeld is vereist voor installatiekopieën van een goedgekeurde resourcegroep:

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

Het volgende voorbeeld wordt de goedgekeurde installatiekopie-id's:

```json
{
    "field": "Microsoft.Compute/imageId",
    "in": ["{imageId1}","{imageId2}"]
}
```

## <a name="virtual-machine-extensions"></a>Uitbreidingen van de virtuele Machine

U kunt informatie over het gebruik van bepaalde soorten extensies verbieden. Bijvoorbeeld, een uitbreiding mogelijk niet compatibel met bepaalde afbeeldingen aangepaste virtuele machine. Het volgende voorbeeld laat zien hoe een bepaalde extensie blokkeren. Uitgever en type worden gebruikt om te bepalen welke extensie te blokkeren.

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
* Na het definiëren van een beleidsregel (zoals weergegeven in de voorgaande voorbeelden), moet u de beleidsdefinitie maken en toewijzen aan een bereik. Het bereik mag een abonnement, resourcegroep of resource. Als u wilt toewijzen beleid via de portal, Zie [gebruik Azure-portal toewijzen en beheren van bronbeleid](../../azure-resource-manager/resource-manager-policy-portal.md). Als u wilt toewijzen beleid via REST API, PowerShell of Azure CLI, Zie [toewijzen en beheren van beleid via script](../../azure-resource-manager/resource-manager-policy-create-assign.md).
* Zie voor een inleiding tot bronbeleid, [Resource overzicht](../../azure-resource-manager/resource-manager-policy.md).
* Voor begeleiding bij de manier waarop ondernemingen Resource Manager effectief kunnen gebruiken voor het beheer van abonnementen, gaat u naar [Azure enterprise-platform - Prescriptieve abonnementsgovernance](../../azure-resource-manager/resource-manager-subscription-governance.md).
