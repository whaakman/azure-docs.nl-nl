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
ms.date: 12/15/2017
ms.author: tomfitz
ms.openlocfilehash: e19833cb58f37f5f8b83d5558d74255583137684
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/16/2017
---
# <a name="deploy-multiple-instances-of-a-resource-or-property-in-azure-resource-manager-templates"></a>Implementeren van meerdere exemplaren van een resource of eigenschap in Azure Resource Manager-sjablonen
In dit artikel leest u het implementeren van voorwaardelijk een resource, en hoe u in uw Azure Resource Manager-sjabloon maken van meerdere exemplaren van een resource.

## <a name="conditionally-deploy-resource"></a>Voorwaardelijk resource implementeren

Wanneer u bepalen tijdens de implementatie van moet één exemplaar of er zijn geen exemplaren van een resource te maken, gebruiken de `condition` element. De waarde voor dit element wordt omgezet in true of false. Wanneer de waarde true is, wordt de bron wordt geïmplementeerd. Wanneer de waarde false is, wordt de resource wordt niet geïmplementeerd. Bijvoorbeeld: als u wilt opgeven of een nieuw opslagaccount wordt geïmplementeerd of een bestaand opslagaccount wordt gebruikt, gebruiken:

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

## <a name="resource-iteration"></a>Resource herhaling
Wanneer u bepalen tijdens de implementatie van moet een of meer exemplaren van een resource maken, voegt een `copy` element aan het brontype. In het element kopiëren, moet u het aantal iteraties en een naam op voor deze lus opgeven. De waarde van count moet een positief geheel getal zijn en mag niet meer dan 800. 

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

Resource Manager maakt standaard de resources parallel. De volgorde waarin ze zijn gemaakt kan daarom niet worden gegarandeerd. Daarom is het raadzaam om op te geven dat de resources in volgorde worden geïmplementeerd. Bijvoorbeeld bij het bijwerken van een productieomgeving, u kunt dus spreiden van de updates alleen een bepaald aantal op elk gewenst moment worden bijgewerkt.

Als u wilt implementeren opeenvolgend meerdere exemplaren van een resource, instellen `mode` naar **seriële** en `batchSize` aan het aantal exemplaren moeten worden geïmplementeerd op een tijdstip. Seriële modus maakt Resource Manager met een afhankelijkheid voor eerdere exemplaren in de lus, zodat dit niet één batch gestart totdat de vorige batch is voltooid.

Bijvoorbeeld: als u wilt implementeren opeenvolgend opslagaccounts twee tegelijk, gebruiken:

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
                "count": 4,
                "mode": "serial",
                "batchSize": 2
            }
        }
    ],
    "outputs": {}
}
``` 

De eigenschap mode accepteert ook **parallelle**, dit is de standaardwaarde.

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

## <a name="variable-iteration"></a>Variabele herhaling

Gebruik voor het maken van meerdere exemplaren van een variabele de `copy` element in het gedeelte variabelen. U kunt meerdere exemplaren van objecten met de bijbehorende waarden maken en vervolgens deze waarden toewijzen aan exemplaren van de resource. U kunt kopiëren van een object te maken met een matrixeigenschap van een of een matrix. Beide benaderingen worden weergegeven in het volgende voorbeeld:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {},
  "variables": {
    "disk-array-on-object": {
      "copy": [
        {
          "name": "disks",
          "count": 5,
          "input": {
            "name": "[concat('myDataDisk', copyIndex('disks', 1))]",
            "diskSizeGB": "1",
            "diskIndex": "[copyIndex('disks')]"
          }
        }
      ]
    },
    "copy": [
      {
        "name": "disks-top-level-array",
        "count": 5,
        "input": {
          "name": "[concat('myDataDisk', copyIndex('disks-top-level-array', 1))]",
          "diskSizeGB": "1",
          "diskIndex": "[copyIndex('disks-top-level-array')]"
        }
      }
    ]
  },
  "resources": [],
  "outputs": {
    "exampleObject": {
      "value": "[variables('disk-array-on-object')]",
      "type": "object"
    },
    "exampleArrayOnObject": {
      "value": "[variables('disk-array-on-object').disks]",
      "type" : "array"
    },
    "exampleArray": {
      "value": "[variables('disks-top-level-array')]",
      "type" : "array"
    }
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

## <a name="iteration-for-a-child-resource"></a>Herhaling voor een onderliggende resource
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

## <a name="example-templates"></a>Voorbeeld-sjablonen

De volgende voorbeelden tonen algemene scenario's voor het maken van meerdere resources of eigenschappen.

|Template  |Beschrijving  |
|---------|---------|
|[Opslag kopiëren](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/copystorage.json) |Meerdere opslagaccounts met indexnummer in de naam implementeert. |
|[Opslagruimte voor de seriële](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/serialcopystorage.json) |Meerdere opslagaccounts één implementeert gelijktijdig. De naam bevat het indexnummer. |
|[Opslag met een matrix kopiëren](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/copystoragewitharray.json) |Meerdere opslagaccounts implementeert. De naam bevat een waarde van een matrix. |
|[Virtuele machine met een nieuwe of bestaande virtuele netwerk, opslag- en openbare IP-adres](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-new-or-existing-conditions) |Voorwaardelijk implementeert nieuwe of bestaande resources met een virtuele machine. |
|[Implementaties van virtuele machines met een variabele aantal gegevensschijven](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-windows-copy-datadisks) |Meerdere gegevensschijven met een virtuele machine implementeert. |
|[Variabelen kopiëren](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/copyvariables.json) |Toont de verschillende manieren van het doorlopen van variabelen. |
|[Meerdere regels](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/multiplesecurityrules.json) |Meerdere beveiligingsregels implementeert in een netwerkbeveiligingsgroep. Dit vormt de beveiligingsregels voor verbindingen van een parameter. |

## <a name="next-steps"></a>Volgende stappen
* Als u wilt voor meer informatie over de secties van een sjabloon, Zie [Azure Resource Manager-sjablonen ontwerpen](resource-group-authoring-templates.md).
* Zie voor meer informatie over het implementeren van uw sjabloon, [Implementeer een toepassing met Azure Resource Manager-sjabloon](resource-group-template-deploy.md).

