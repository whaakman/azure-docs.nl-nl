---
title: Meer informatie over de sjablonen voor virtuele machines scale set | Microsoft Docs
description: Informatie over het maken van een minimale levensvatbaar scale set-sjabloon voor virtuele-machineschaalsets
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: negat
ms.openlocfilehash: 5cd495d1332c71d7eae775f933b73d98826f10e4
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/20/2017
---
# <a name="learn-about-virtual-machine-scale-set-templates"></a>Meer informatie over de sjablonen voor virtuele machines scale set
[Azure Resource Manager-sjablonen](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#template-deployment) zijn bijzonder handig om groepen gerelateerde resources te implementeren. Deze zelfstudie reeks ziet u hoe u een minimale levensvatbaar scale set-sjabloon maken en het wijzigen van deze sjabloon aanpassen aan verschillende scenario's. Alle voorbeelden afkomstig zijn van dit [GitHub-opslagplaats](https://github.com/gatneil/mvss). 

Deze sjabloon is bedoeld om eenvoudig. Voor gedetailleerde voorbeelden van de schaal sjablonen, raadpleegt u de [Azure Quick Start-sjablonen GitHub-opslagplaats](https://github.com/Azure/azure-quickstart-templates) en zoek naar de mappen met de tekenreeks `vmss`.

Als u al bekend bent met het maken van sjablonen, kunt u naar de sectie 'Volgende stappen' voor informatie over het wijzigen van deze sjabloon overslaan.

## <a name="review-the-template"></a>Controleer de sjabloon

GitHub gebruiken om te controleren van de minimale levensvatbaar scale set-sjabloon [azuredeploy.json](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json).

In deze zelfstudie behandeld de diff (`git diff master minimum-viable-scale-set`) ingesteld voor het maken van de minimale levensvatbaar schaal sjabloon stuk door stuk.

## <a name="define-schema-and-contentversion"></a>$Schema en contentVersion definiëren
Eerst definieert `$schema` en `contentVersion` in de sjabloon. De `$schema` element definieert de versie van de sjabloontaal en wordt gebruikt voor syntaxismarkering Visual Studio en vergelijkbare validatie-functies. De `contentVersion` element wordt niet gebruikt door Azure. In plaats daarvan kunt u de sjabloonversie.

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
  "contentVersion": "1.0.0.0",
```
## <a name="define-parameters"></a>parameters definiëren
Definieer vervolgens twee parameters `adminUsername` en `adminPassword`. Parameters zijn waarden die u op het moment van implementatie opgeeft. De `adminUsername` parameter is gewoon een `string` type, maar omdat `adminPassword` is een geheim, geeft het type `securestring`. Later, worden deze parameters doorgegeven aan de configuratie van scale set.

```json
  "parameters": {
    "adminUsername": {
      "type": "string"
    },
    "adminPassword": {
      "type": "securestring"
    }
  },
```
## <a name="define-variables"></a>Variabelen definiëren
Resource Manager-sjablonen kunnen u definiëren variabelen moet later in de sjabloon worden gebruikt. In het voorbeeld gebruikt niet alle variabelen, zodat de JSON-object leeg is.

```json
  "variables": {},
```

## <a name="define-resources"></a>Resources definiëren
Naast is de bronnensectie van de sjabloon. Hier kunt definiëren u wat u daadwerkelijk wilt implementeren. In tegenstelling tot `parameters` en `variables` (die zijn JSON-objecten), `resources` is een JSON-lijst van JSON-objecten.

```json
   "resources": [
```

Alle resources vereisen `type`, `name`, `apiVersion`, en `location` eigenschappen. De eerste resource in dit voorbeeld heeft een type `Microsft.Network/virtualNetwork`, naam `myVnet`, en apiVersion `2016-03-30`. (Zie informatie over de nieuwste API-versie voor een brontype de [Azure REST API-documentatie](https://docs.microsoft.com/rest/api/).)

```json
     {
       "type": "Microsoft.Network/virtualNetworks",
       "name": "myVnet",
       "apiVersion": "2016-12-01",
```

## <a name="specify-location"></a>Geef de locatie
Geef een locatie voor het virtuele netwerk, gebruikt u een [Resource Manager sjabloonfunctie](../azure-resource-manager/resource-group-template-functions.md). Deze functie moet tussen aanhalingstekens en vierkante haken als volgt: `"[<template-function>]"`. In dit geval gebruiken de `resourceGroup` functie. Dit heeft geen argumenten en retourneert een JSON-object met metagegevens over de resourcegroep die deze implementatie wordt geïmplementeerd. De resourcegroep is ingesteld door de gebruiker op het moment van implementatie. Deze waarde wordt vervolgens geïndexeerd in dit JSON-object met `.location` de locatie van de JSON-object ophalen.

```json
       "location": "[resourceGroup().location]",
```

## <a name="specify-virtual-network-properties"></a>Eigenschappen van virtueel netwerk opgeven
Elke Resource Manager-bron heeft zijn eigen `properties` sectie voor specifieke configuraties voor de resource. In dit geval opgeven dat het virtuele netwerk één subnet met het particuliere IP-adresbereik moet hebben `10.0.0.0/16`. Een schaalset zich altijd in één subnet. Dit kan geen subnetten omvatten.

```json
       "properties": {
         "addressSpace": {
           "addressPrefixes": [
             "10.0.0.0/16"
           ]
         },
         "subnets": [
           {
             "name": "mySubnet",
             "properties": {
               "addressPrefix": "10.0.0.0/16"
             }
           }
         ]
       }
     },
```

## <a name="add-dependson-list"></a>DependsOn lijst toevoegen
Naast de vereiste `type`, `name`, `apiVersion`, en `location` eigenschappen van elke bron kunnen een optionele hebben `dependsOn` lijst met tekenreeksen. Deze lijst aangeeft welke andere bronnen van deze implementatie moeten worden voltooid voordat de implementatie van deze resource.

In dit geval is slechts één element in de lijst, het virtuele netwerk van het vorige voorbeeld. U kunt deze afhankelijkheid opgeven omdat de schaal ingesteld moet het netwerk bestaan voordat het maken van alle virtuele machines. Op deze manier de schaalaanpassingsset krijgt deze persoonlijke IP-adressen voor virtuele machines uit het IP-adresbereik eerder opgegeven in de eigenschappen van het netwerk. De indeling van elke tekenreeks in de lijst dependsOn is `<type>/<name>`. Gebruik van dezelfde `type` en `name` eerder gebruikt in de resourcedefinitie virtueel netwerk.

```json
     {
       "type": "Microsoft.Compute/virtualMachineScaleSets",
       "name": "myScaleSet",
       "apiVersion": "2016-04-30-preview",
       "location": "[resourceGroup().location]",
       "dependsOn": [
         "Microsoft.Network/virtualNetworks/myVnet"
       ],
```
## <a name="specify-scale-set-properties"></a>Scale seteigenschappen opgeven
-Schaalsets hebben veel eigenschappen voor het aanpassen van de virtuele machines in de schaalset. Zie voor een volledige lijst van deze eigenschappen de [REST API-documentatie-schaalset](https://docs.microsoft.com/rest/api/virtualmachinescalesets/create-or-update-a-set). Voor deze zelfstudie zijn slechts enkele veelgebruikte eigenschappen worden ingesteld.
### <a name="supply-vm-size-and-capacity"></a>VM-grootte en capaciteit op te geven
De schaal moeten weten welke grootte van virtuele machine te maken ('sku-naam') instellen en hoeveel dergelijke virtuele machines maken ('sku-capaciteit'). Als u wilt zien welke VM-grootten beschikbaar zijn, Zie de [VM-grootten documentatie](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-sizes).

```json
       "sku": {
         "name": "Standard_A1",
         "capacity": 2
       },
```

### <a name="choose-type-of-updates"></a>Type van de updates kiezen
De schaal ook moet weten hoe ze moeten updates op de schaalaanpassingsset verwerkt. Er zijn momenteel twee opties `Manual` en `Automatic`. Zie de documentatie voor meer informatie over de verschillen tussen de twee op [bijwerken van een schaalset](./virtual-machine-scale-sets-upgrade-scale-set.md).

```json
       "properties": {
         "upgradePolicy": {
           "mode": "Manual"
         },
```

### <a name="choose-vm-operating-system"></a>Kies VM-besturingssysteem
De schaal ingesteld moeten weten welk besturingssysteem moet worden geplaatst op de virtuele machines. Hier, maken de virtuele machines met een volledig patches Ubuntu 16.04 TNS-installatiekopie.

```json
         "virtualMachineProfile": {
           "storageProfile": {
             "imageReference": {
               "publisher": "Canonical",
               "offer": "UbuntuServer",
               "sku": "16.04-LTS",
               "version": "latest"
             }
           },
```

### <a name="specify-computernameprefix"></a>Geef de waarde voor computerNamePrefix
De schaalaanpassingsset implementeert meerdere virtuele machines. Geef in plaats van elke VM-naam geven, `computerNamePrefix`. De schaalaanpassingsset voegt een index samen met het voorvoegsel voor elke virtuele machine, zodat de VM-namen hebben de vorm `<computerNamePrefix>_<auto-generated-index>`.

Gebruik de parameters van vóór de gebruikersnaam voor de beheerder en het wachtwoord instellen voor alle VM's in de schaalset in het volgende codefragment. Dit proces maakt gebruik van de `parameters` sjabloonfunctie. Deze functie omvat een tekenreeks die Hiermee geeft u op welke parameter om te verwijzen naar en levert de waarde voor deze parameter.

```json
           "osProfile": {
             "computerNamePrefix": "vm",
             "adminUsername": "[parameters('adminUsername')]",
             "adminPassword": "[parameters('adminPassword')]"
           },
```

### <a name="specify-vm-network-configuration"></a>VM-netwerkconfiguratie opgeven
Geef ten slotte de netwerkconfiguratie voor de virtuele machines in de schaalset. In dit geval hoeft u alleen te geven van de ID van het subnet eerder hebt gemaakt. Hiermee wordt de schaal is ingesteld op de netwerkinterfaces in dit subnet plaatsen schrijfopdrachten.

Krijgt u de ID van het virtuele netwerk met het subnet met behulp van de `resourceId` sjabloonfunctie. Deze functie wordt in het type en de naam van een resource en retourneert de volledig gekwalificeerde id van de bron. Deze ID heeft de vorm:`/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/<resourceProviderNamespace>/<resourceType>/<resourceName>`

De id van het virtuele netwerk is echter niet voldoende. Geef de specifiek subnet dat in de schaalset virtuele machines moet liggen. U doet dit door samenvoegen `/subnets/mySubnet` naar de ID van het virtuele netwerk. Het resultaat is de volledig gekwalificeerde-ID van het subnet. Voer deze samenvoeging met de `concat` functie, die duurt in een reeks tekenreeksen en retourneert de samenvoeging.

```json
           "networkProfile": {
             "networkInterfaceConfigurations": [
               {
                 "name": "myNic",
                 "properties": {
                   "primary": "true",
                   "ipConfigurations": [
                     {
                       "name": "myIpConfig",
                       "properties": {
                         "subnet": {
                           "id": "[concat(resourceId('Microsoft.Network/virtualNetworks', 'myVnet'), '/subnets/mySubnet')]"
                         }
                       }
                     }
                   ]
                 }
               }
             ]
           }
         }
       }
     }
   ]
}

```

## <a name="next-steps"></a>Volgende stappen

[!INCLUDE [mvss-next-steps-include](../../includes/mvss-next-steps.md)]
