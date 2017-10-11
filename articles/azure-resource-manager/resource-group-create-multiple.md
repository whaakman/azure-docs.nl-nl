---
title: Meerdere exemplaren van Azure-resources implementeren | Microsoft Docs
description: Kopieerbewerking en matrices in een Azure Resource Manager-sjabloon gebruiken om te herhalen meerdere keren bij het implementeren van resources.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: 
ms.assetid: 94d95810-a87b-460f-8e82-c69d462ac3ca
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/26/2017
ms.author: tomfitz
ms.openlocfilehash: ed8e3081d2b2e07938d7cf3aa5f95f6dde81bc66
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-multiple-instances-of-a-resource-or-property-in-azure-resource-manager-templates"></a>Implementeren van meerdere exemplaren van een resource of eigenschap in Azure Resource Manager-sjablonen
Dit onderwerp leest u hoe u in uw Azure Resource Manager-sjabloon voor het maken van meerdere exemplaren van een resource of meerdere exemplaren van een eigenschap van een resource.

Als u Voeg logica toe aan de sjabloon waarmee u wilt kunt opgeven of een resource wordt geïmplementeerd, Zie [voorwaardelijk implementeren resource](#conditionally-deploy-resource).

## <a name="resource-iteration"></a>Resource herhaling
Voor het maken van meerdere exemplaren van een brontype toevoegen een `copy` element aan het brontype. In het element kopiëren, moet u het aantal iteraties en een naam op voor deze lus opgeven. De waarde van count moet een positief geheel getal zijn en mag niet meer dan 800. Resource Manager maakt de resources parallel. De volgorde waarin ze zijn gemaakt kan daarom niet worden gegarandeerd. Zie voor informatie over het maken van resources herhaald in de reeks [seriële kopiëren](#serial-copy). 

De bron voor het maken van meerdere keren wordt gebruikt in de volgende indeling:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
        {
            "apiVersion": "2016-01-01",
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[concat(copyIndex(),'storage', uniqueString(resourceGroup().id))]",
            "location": "[resourceGroup().location]",
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {},
            "copy": {
                "name": "storagecopy",
                "count": 3
            }
        }
    ],
    "outputs": {}
}
```

U ziet dat de naam van elke resource bevat de `copyIndex()` functie de huidige herhaling in de lus retourneert. `copyIndex()`is gebaseerd op nul. Hiertoe het volgende voorbeeld:

```json
"name": "[concat('storage', copyIndex())]",
```

Deze namen maakt:

* storage0
* storage1
* storage2.

Als u wilt verschuiven waarde voor de index, kunt u een waarde in de functie copyIndex() doorgeven. Het aantal iteraties om uit te voeren nog steeds is opgegeven in het element kopiëren, maar de waarde van copyIndex wordt gecompenseerd door de opgegeven waarde. Hiertoe het volgende voorbeeld:

```json
"name": "[concat('storage', copyIndex(1))]",
```

Deze namen maakt:

* storage1
* storage2
* storage3

De kopieerbewerking is handig bij het werken met matrices omdat u elk element in de matrix kunt doorlopen. Gebruik de `length` functie op de matrix te geven van het aantal iteraties, en `copyIndex` voor het ophalen van de huidige index in de matrix. Hiertoe het volgende voorbeeld:

```json
"parameters": { 
  "org": { 
     "type": "array", 
     "defaultValue": [ 
         "contoso", 
         "fabrikam", 
         "coho" 
      ] 
  }
}, 
"resources": [ 
  { 
      "name": "[concat('storage', parameters('org')[copyIndex()])]", 
      "copy": { 
         "name": "storagecopy", 
         "count": "[length(parameters('org'))]" 
      }, 
      ...
  } 
]
```

Deze namen maakt:

* storagecontoso
* storagefabrikam
* storagecoho

## <a name="serial-copy"></a>Seriële kopiëren

Wanneer u het element kopiëren gebruikt voor het maken van meerdere exemplaren van een resourcetype Resource Manager worden standaard implementeert de instanties parallel. Daarom is het raadzaam om op te geven dat de resources in volgorde worden geïmplementeerd. Bijvoorbeeld bij het bijwerken van een productieomgeving, u kunt dus spreiden van de updates alleen een bepaald aantal op elk gewenst moment worden bijgewerkt.

Resource Manager biedt de eigenschappen voor het kopiëren-element waarmee u kunt meerdere exemplaren opeenvolgend te implementeren. Stel in het element kopiëren `mode` naar **seriële** en `batchSize` aan het aantal exemplaren moeten worden geïmplementeerd op een tijdstip. Seriële modus maakt Resource Manager met een afhankelijkheid voor eerdere exemplaren in de lus, zodat dit niet één batch gestart totdat de vorige batch is voltooid.

```json
"copy": {
    "name": "iterator",
    "count": "[parameters('numberToDeploy')]",
    "mode": "serial",
    "batchSize": 2
},
```

De eigenschap mode accepteert ook **parallelle**, dit is de standaardwaarde.

Als u wilt testen seriële kopiëren zonder dat er feitelijke webbronnen, moet u de volgende sjabloon die wordt geïmplementeerd leeg geneste sjablonen gebruikt:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "numberToDeploy": {
      "type": "int",
      "minValue": 2,
      "defaultValue": 5
    }
  },
  "resources": [
    {
      "apiVersion": "2015-01-01",
      "type": "Microsoft.Resources/deployments",
      "name": "[concat('loop-', copyIndex())]",
      "copy": {
        "name": "iterator",
        "count": "[parameters('numberToDeploy')]",
        "mode": "serial",
        "batchSize": 1
      },
      "properties": {
        "mode": "Incremental",
        "template": {
          "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
          "contentVersion": "1.0.0.0",
          "parameters": {},
          "variables": {},
          "resources": [],
          "outputs": {
          }
        }
      }
    }
  ],
  "outputs": {
  }
}
```

U ziet dat de geneste implementaties in volgorde worden verwerkt in de geschiedenis van de implementatie.

![seriële implementatie](./media/resource-group-create-multiple/serial-copy.png)

Voor een meer realistische scenario is implementeert het volgende voorbeeld twee instanties op een tijdstip van een Linux-VM uit een geneste sjabloon:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "adminUsername": {
            "type": "string",
            "metadata": {
                "description": "User name for the Virtual Machine."
            }
        },
        "adminPassword": {
            "type": "securestring",
            "metadata": {
                "description": "Password for the Virtual Machine."
            }
        },
        "dnsLabelPrefix": {
            "type": "string",
            "metadata": {
                "description": "Unique DNS Name for the Public IP used to access the Virtual Machine."
            }
        },
        "ubuntuOSVersion": {
            "type": "string",
            "defaultValue": "16.04.0-LTS",
            "allowedValues": [
                "12.04.5-LTS",
                "14.04.5-LTS",
                "15.10",
                "16.04.0-LTS"
            ],
            "metadata": {
                "description": "The Ubuntu version for the VM. This will pick a fully patched image of this given Ubuntu version."
            }
        }
    },
    "variables": {
        "templatelink": "https://raw.githubusercontent.com/rjmax/Build2017/master/Act1.TemplateEnhancements/Chapter03.LinuxVM.json"
    },
    "resources": [
        {
            "apiVersion": "2015-01-01",
            "name": "[concat('nestedDeployment',copyIndex())]",
            "type": "Microsoft.Resources/deployments",
            "copy": {
                "name": "myCopySet",
                "count": 4,
                "mode": "serial",
                "batchSize": 2
            },
            "properties": {
                "mode": "Incremental",
                "templateLink": {
                    "uri": "[variables('templatelink')]",
                    "contentVersion": "1.0.0.0"
                },
                "parameters": {
                    "adminUsername": {
                        "value": "[parameters('adminUsername')]"
                    },
                    "adminPassword": {
                        "value": "[parameters('adminPassword')]"
                    },
                    "dnsLabelPrefix": {
                        "value": "[parameters('dnsLabelPrefix')]"
                    },
                    "ubuntuOSVersion": {
                        "value": "[parameters('ubuntuOSVersion')]"
                    },
                    "index":{
                        "value": "[copyIndex()]"
                    }
                }
            }
        }
    ]
}
```

## <a name="property-iteration"></a>Eigenschap herhaling

Voor het maken van meerdere waarden voor een eigenschap van een resource, Voeg een `copy` matrix in het element eigenschappen. Deze matrix bevat objecten en elk object heeft de volgende eigenschappen:

* naam - de naam van de eigenschap voor het maken van meerdere waarden voor
* aantal - het aantal waarden maken
* invoer - object dat de waarden toewijzen aan de eigenschap bevat  

Het volgende voorbeeld laat zien hoe om toe te passen `copy` voor de eigenschap dataDisks op een virtuele machine:

```json
{
  "name": "examplevm",
  "type": "Microsoft.Compute/virtualMachines",
  "apiVersion": "2017-03-30",
  "properties": {
    "storageProfile": {
      "copy": [{
          "name": "dataDisks",
          "count": 3,
          "input": {
              "lun": "[copyIndex('dataDisks')]",
              "createOption": "Empty",
              "diskSizeGB": "1023"
          }
      }],
      ...
```

Merk op dat wanneer u `copyIndex` binnen de herhaling van een eigenschap, moet u de naam van de herhaling opgeven. U hebt niet de naam gebruikt in combinatie met herhaling van de resource op te geven.

Resource Manager breidt de `copy` matrix tijdens de implementatie. De naam van de matrix, wordt de naam van de eigenschap. De invoerwaarden, worden de objecteigenschappen. De geïmplementeerde sjabloon als volgt uit:

```json
{
  "name": "examplevm",
  "type": "Microsoft.Compute/virtualMachines",
  "apiVersion": "2017-03-30",
  "properties": {
    "storageProfile": {
      "dataDisks": [
          {
              "lun": 0,
              "createOption": "Empty",
              "diskSizeGB": "1023"
          },
          {
              "lun": 1,
              "createOption": "Empty",
              "diskSizeGB": "1023"
          },
          {
              "lun": 2,
              "createOption": "Empty",
              "diskSizeGB": "1023"
          }
      }],
      ...
```

U kunt resource en de eigenschap iteratie samen gebruiken. Verwijzing naar de herhaling van de eigenschap met de naam.

```json
{
    "type": "Microsoft.Network/virtualNetworks",
    "name": "[concat(parameters('vnetname'), copyIndex())]",
    "apiVersion": "2016-06-01",
    "copy":{
        "count": 2,
        "name": "vnetloop"
    },
    "location": "[resourceGroup().location]",
    "properties": {
        "addressSpace": {
            "addressPrefixes": [
                "[parameters('addressPrefix')]"
            ]
        },
        "copy": [
            {
                "name": "subnets",
                "count": 2,
                "input": {
                    "name": "[concat('subnet-', copyIndex('subnets'))]",
                    "properties": {
                        "addressPrefix": "[variables('subnetAddressPrefix')[copyIndex('subnets')]]"
                    }
                }
            }
        ]
    }
}
```

U kunt alleen één exemplaar element opnemen in de eigenschappen voor elke resource. Als u een lus herhaling voor meer dan één eigenschap, meerdere objecten definiëren die in de matrix kopiëren. Elk object wordt afzonderlijk herhaald. Om bijvoorbeeld te maken van meerdere exemplaren van zowel de `frontendIPConfigurations` eigenschap en de `loadBalancingRules` eigenschap op een load balancer beide objecten definiëren die in een element met één exemplaar: 

```json
{
    "name": "[variables('loadBalancerName')]",
    "type": "Microsoft.Network/loadBalancers",
    "properties": {
        "copy": [
          {
              "name": "frontendIPConfigurations",
              "count": 2,
              "input": {
                  "name": "[concat('loadBalancerFrontEnd', copyIndex('frontendIPConfigurations', 1))]",
                  "properties": {
                      "publicIPAddress": {
                          "id": "[variables(concat('publicIPAddressID', copyIndex('frontendIPConfigurations', 1)))]"
                      }
                  }
              }
          },
          {
              "name": "loadBalancingRules",
              "count": 2,
              "input": {
                  "name": "[concat('LBRuleForVIP', copyIndex('loadBalancingRules', 1))]",
                  "properties": {
                      "frontendIPConfiguration": {
                          "id": "[variables(concat('frontEndIPConfigID', copyIndex('loadBalancingRules', 1)))]"
                      },
                      "backendAddressPool": {
                          "id": "[variables('lbBackendPoolID')]"
                      },
                      "protocol": "tcp",
                      "frontendPort": "[variables(concat('frontEndPort' copyIndex('loadBalancingRules', 1))]",
                      "backendPort": "[variables(concat('backEndPort' copyIndex('loadBalancingRules', 1))]",
                      "probe": {
                          "id": "[variables('lbProbeID')]"
                      }
                  }
              }
          }
        ],
        ...
    }
}
```

## <a name="depend-on-resources-in-a-loop"></a>Afhankelijk zijn van bronnen in een lus
U opgeven dat een resource na een andere resource wordt geïmplementeerd met behulp van de `dependsOn` element. Geef de naam van de lus kopie in het element dependsOn voor het implementeren van een resource die afhankelijk zijn van de verzameling van resources in een lus. Het volgende voorbeeld laat zien hoe drie storage-accounts te implementeren voordat u de virtuele Machine implementeert. De definitie van de volledige virtuele Machine niet wordt weergegeven. U ziet dat het element kopiëren naam ingesteld op `storagecopy` en het element dependsOn voor de virtuele Machines ook is ingesteld op `storagecopy`.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "resources": [
        {
            "apiVersion": "2016-01-01",
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[concat(copyIndex(),'storage', uniqueString(resourceGroup().id))]",
            "location": "[resourceGroup().location]",
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {},
            "copy": {
                "name": "storagecopy",
                "count": 3
            }
        },
        {
            "apiVersion": "2015-06-15", 
            "type": "Microsoft.Compute/virtualMachines", 
            "name": "[concat('VM', uniqueString(resourceGroup().id))]",  
            "dependsOn": ["storagecopy"],
            ...
        }
    ],
    "outputs": {}
}
```

## <a name="create-multiple-instances-of-a-child-resource"></a>Meerdere exemplaren van een onderliggende resource maken
U kunt een lus kopie niet gebruiken voor een onderliggende resource. Voor het maken van meerdere exemplaren van een resource die u normaal gesproken als genest in een andere resource definiëren, moet u in plaats daarvan die resource maken als een resource op het hoogste niveau. U definieert de relatie met de bovenliggende resource via de eigenschappen van het type en de naam.

Stel bijvoorbeeld dat u doorgaans een gegevensset definiëren als een onderliggende bron binnen een gegevensfactory.

```json
"resources": [
{
    "type": "Microsoft.DataFactory/datafactories",
    "name": "exampleDataFactory",
    ...
    "resources": [
    {
        "type": "datasets",
        "name": "exampleDataSet",
        "dependsOn": [
            "exampleDataFactory"
        ],
        ...
    }
}]
```

Voor het maken van meerdere exemplaren van gegevenssets, verplaatst u het buiten de gegevensfactory. De gegevensset moet zich op hetzelfde niveau als de gegevensfactory, maar nog steeds een onderliggende resource van de gegevensfactory. U behouden de relatie tussen de gegevensset en data factory via de eigenschappen van het type en de naam. Aangezien het type kan niet meer worden afgeleid van de positie in de sjabloon, moet u de volledig gekwalificeerde type in de indeling opgeven: `{resource-provider-namespace}/{parent-resource-type}/{child-resource-type}`.

Geef een naam voor de gegevensset met de naam van de bovenliggende resource voor het opzetten van een bovenliggende/onderliggende relatie met een exemplaar van de gegevensfactory. Gebruik de indeling: `{parent-resource-name}/{child-resource-name}`.  

Het volgende voorbeeld ziet u de implementatie:

```json
"resources": [
{
    "type": "Microsoft.DataFactory/datafactories",
    "name": "exampleDataFactory",
    ...
},
{
    "type": "Microsoft.DataFactory/datafactories/datasets",
    "name": "[concat('exampleDataFactory', '/', 'exampleDataSet', copyIndex())]",
    "dependsOn": [
        "exampleDataFactory"
    ],
    "copy": { 
        "name": "datasetcopy", 
        "count": "3" 
    } 
    ...
}]
```

## <a name="conditionally-deploy-resource"></a>Voorwaardelijk resource implementeren

Als u wilt opgeven of een resource wordt geïmplementeerd, gebruiken de `condition` element. De waarde voor dit element wordt omgezet in true of false. Wanneer de waarde true is, wordt de bron wordt geïmplementeerd. Wanneer de waarde false is, wordt de resource wordt niet geïmplementeerd. Bijvoorbeeld: als u wilt opgeven of een nieuw opslagaccount wordt geïmplementeerd of een bestaand opslagaccount wordt gebruikt, gebruiken:

```json
{
    "condition": "[equals(parameters('newOrExisting'),'new')]",
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[variables('storageAccountName')]",
    "apiVersion": "2017-06-01",
    "location": "[resourceGroup().location]",
    "sku": {
        "name": "[variables('storageAccountType')]"
    },
    "kind": "Storage",
    "properties": {}
}
```

Zie voor een voorbeeld van het gebruik van een nieuwe of bestaande resourcegroep [nieuwe of bestaande voorwaarde sjabloon](https://github.com/rjmax/Build2017/blob/master/Act1.TemplateEnhancements/Chapter05.ConditionalResources.NewOrExisting.json).

Zie voor een voorbeeld van het gebruik van een wachtwoord of SSH-sleutel voor het implementeren van virtuele machine [gebruikersnaam of SSH voorwaarde sjabloon](https://github.com/rjmax/Build2017/blob/master/Act1.TemplateEnhancements/Chapter05.ConditionalResourcesUsernameOrSsh.json).

## <a name="next-steps"></a>Volgende stappen
* Als u wilt voor meer informatie over de secties van een sjabloon, Zie [Azure Resource Manager-sjablonen ontwerpen](resource-group-authoring-templates.md).
* Zie voor meer informatie over het implementeren van uw sjabloon, [Implementeer een toepassing met Azure Resource Manager-sjabloon](resource-group-template-deploy.md).

