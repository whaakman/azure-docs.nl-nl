---
title: Beveiliging met beleidsregels op Windows-machines in Azure afdwingen | Microsoft Docs
description: Het toepassen van een beleid voor een Azure Resource Manager virtuele Windows-computer
services: virtual-machines-windows
documentationcenter: ''
author: singhkays
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 0b71ba54-01db-43ad-9bca-8ab358ae141b
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: kasing
ms.openlocfilehash: 7ec348bbd75bfce268d1e3d7a46b79397ee39f98
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/06/2018
---
# <a name="apply-policies-to-windows-vms-with-azure-resource-manager"></a>Beleid toepassen op Windows-VM's met Azure Resource Manager
Een organisatie kan met behulp van beleid afdwingen verschillende conventies en regels in de hele onderneming. Afdwinging van het gewenste gedrag kunt risico's te beperken tijdens bijdragen aan het succes van de organisatie. In dit artikel wordt beschreven hoe u Azure Resource Manager-beleid kunt gebruiken voor het definiëren van het gewenste gedrag voor virtuele Machines die uw organisatie.

Zie voor een inleiding tot beleidsregels, [wat is Azure beleid?](../../azure-policy/azure-policy-introduction.md).

## <a name="permitted-virtual-machines"></a>Toegestane virtuele Machines
Om ervoor te zorgen dat virtuele machines voor uw organisatie compatibel met een toepassing zijn, kunt u de toegestane besturingssystemen beperken. U kunt alleen Windows Server 2012 R2 Datacenter virtuele Machines worden gemaakt in het volgende voorbeeld voor het beleid:

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
                "MicrosoftWindowsServer"
              ]
            },
            {
              "field": "Microsoft.Compute/imageOffer",
              "in": [
                "WindowsServer"
              ]
            },
            {
              "field": "Microsoft.Compute/imageSku",
              "in": [
                "2012-R2-Datacenter"
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

Gebruik een jokerteken voor het wijzigen van het vorige beleid voor het toestaan van een installatiekopie van Windows Server Datacenter:

```json
{
  "field": "Microsoft.Compute/imageSku",
  "like": "*Datacenter"
}
```

Gebruik dragen om het voorgaande beleid aanpassen om een Windows Server 2012 R2 Datacenter of hoger installatiekopie toe te staan:

```json
{
  "anyOf": [
    {
      "field": "Microsoft.Compute/imageSku",
      "like": "2012-R2-Datacenter*"
    },
    {
      "field": "Microsoft.Compute/imageSku",
      "like": "2016-Datacenter*"
    }
  ]
}
```

Zie voor meer informatie over Beleidsvelden [beleid aliassen](../../azure-policy/policy-definition.md#aliases).

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


## <a name="azure-hybrid-use-benefit"></a>Azure Hybrid Use Benefit

Wanneer u een on-premises-licentie hebt, kunt u de licentiekosten op uw virtuele machines kunt opslaan. Wanneer u de licentie niet hebt, moet u de optie verbieden. Het volgende beleid verbiedt informatie over het gebruik van Azure hybride gebruik voordeel (AHUB):

```json
{
    "if": {
        "allOf": [
            {
                "field": "type",
                "in":[ "Microsoft.Compute/virtualMachines","Microsoft.Compute/VirtualMachineScaleSets"]
            },
            {
                "field": "Microsoft.Compute/licenseType",
                "exists": true
            }
        ]
    },
    "then": {
        "effect": "deny"
    }
}
```

## <a name="next-steps"></a>Volgende stappen
* Na het definiëren van een beleidsregel (zoals weergegeven in de voorgaande voorbeelden), moet u de beleidsdefinitie maken en toewijzen aan een bereik. Het bereik mag een abonnement, resourcegroep of resource. Om het beleid toe te wijzen, Zie [gebruik Azure-portal toewijzen en beheren van bronbeleid](../../azure-policy/assign-policy-definition.md), [Gebruik PowerShell om toe te wijzen beleid](../../azure-policy/assign-policy-definition-ps.md), of [gebruik Azure CLI voor het beleid toe te wijzen](../../azure-policy/assign-policy-definition-cli.md).
* Zie voor een inleiding tot bronbeleid, [wat is Azure beleid?](../../azure-policy/azure-policy-introduction.md).
* Voor begeleiding bij de manier waarop ondernemingen Resource Manager effectief kunnen gebruiken voor het beheer van abonnementen, gaat u naar [Azure enterprise-platform - Prescriptieve abonnementsgovernance](../../azure-resource-manager/resource-manager-subscription-governance.md).