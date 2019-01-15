---
title: Meer informatie over de virtuele machine schaalsetsjablonen | Microsoft Docs
description: Meer informatie over het maken van een sjabloon voor minimaal levensvatbare schaalsets voor virtuele-machineschaalsets
services: virtual-machine-scale-sets
documentationcenter: ''
author: mayanknayar
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: manayar
ms.openlocfilehash: 601c23bbcddeaeff89e1d1b1a4be8cb6b0256e51
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/14/2019
ms.locfileid: "54258778"
---
# <a name="learn-about-virtual-machine-scale-set-templates"></a>Meer informatie over schaalsetsjablonen virtuele machine
[Azure Resource Manager-sjablonen](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#template-deployment) zijn bijzonder handig om groepen gerelateerde resources te implementeren. In deze reeks zelfstudies ziet over het maken van een sjabloon voor minimaal levensvatbare schaalsets en hoe u deze sjabloon aan de behoeften van verschillende scenario's te wijzigen. Alle voorbeelden afkomstig zijn van dit [GitHub-opslagplaats](https://github.com/gatneil/mvss). 

Deze sjabloon is bedoeld om eenvoudig zijn. Voor meer voorbeelden van de schaal sjablonen, raadpleegt u de [Azure Quickstart-sjablonen GitHub-opslagplaats](https://github.com/Azure/azure-quickstart-templates) en zoek naar de mappen met de tekenreeks `vmss`.

Als u al bekend bent met het maken van sjablonen, kunt u overslaan naar de sectie 'Volgende stappen' voor informatie over het wijzigen van deze sjabloon.

## <a name="review-the-template"></a>Controleer de sjabloon

GitHub gebruiken om te controleren van de minimale levensvatbare schaalsetsjabloon, [azuredeploy.json](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json).

In deze zelfstudie gaan we bekijken de diff (`git diff master minimum-viable-scale-set`) instellen om te maken van de schaal voor minimaal levensvatbare sjabloon stuk per stuk.

## <a name="define-schema-and-contentversion"></a>$Schema en sjablooneigenschap definiëren
Eerst definieert u `$schema` en `contentVersion` in de sjabloon. De `$schema` -element de versie van de taal van de sjabloon definieert en wordt gebruikt voor Visual Studio syntaxismarkering en soortgelijke validatie-functies. De `contentVersion` element wordt niet gebruikt door Azure. In plaats daarvan kunt u de versie van het bijhouden van.

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
  "contentVersion": "1.0.0.0",
```
## <a name="define-parameters"></a>De parameters definiëren
Vervolgens definieert u twee parameters `adminUsername` en `adminPassword`. Parameters zijn waarden die u op het moment van implementatie opgeeft. De `adminUsername` parameter is gewoon een `string` type, maar omdat `adminPassword` is een geheim, geef het type `securestring`. Later, worden deze parameters doorgegeven in de configuratie van de schaalset.

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
Resource Manager-sjablonen kunnen u bij het definiëren van variabelen later in de sjabloon moet worden gebruikt. Het voorbeeld niet alle variabelen gebruikt, zodat het JSON-object leeg is.

```json
  "variables": {},
```

## <a name="define-resources"></a>Resources definiëren
Vervolgens wordt de sectie met resources van de sjabloon. Hier kunt definiëren u wat u daadwerkelijk wilt implementeren. In tegenstelling tot `parameters` en `variables` (die zijn JSON-objecten), `resources` is een JSON-lijst van JSON-objecten.

```json
   "resources": [
```

Alle resources vereisen `type`, `name`, `apiVersion`, en `location` eigenschappen. De eerste resource van dit voorbeeld heeft een type [Microsoft.Network/virtualNetwork](/azure/templates/microsoft.network/virtualnetworks)en de naam `myVnet`, en apiVersion `2016-03-30`. (De meest recente versie van de API voor een resourcetype, Zie de [verwijzing naar de Azure Resource Manager-sjabloon](/azure/templates/).)

```json
     {
       "type": "Microsoft.Network/virtualNetworks",
       "name": "myVnet",
       "apiVersion": "2016-12-01",
```

## <a name="specify-location"></a>Locatie opgeven
Als u de locatie voor het virtuele netwerk, gebruikt u een [Resource Manager-sjabloonfunctie](../azure-resource-manager/resource-group-template-functions.md). Deze functie moet worden tussen aanhalingstekens en vierkante haken als volgt: `"[<template-function>]"`. In dit geval gebruiken de `resourceGroup` functie. Het neemt geen argumenten en retourneert een JSON-object met metagegevens over de resourcegroep die deze implementatie wordt geïmplementeerd. De resourcegroep is ingesteld door de gebruiker op het moment van implementatie. Deze waarde wordt vervolgens geïndexeerd in dit JSON-object met `.location` om op te halen van de locatie van het JSON-object.

```json
       "location": "[resourceGroup().location]",
```

## <a name="specify-virtual-network-properties"></a>Eigenschappen van virtueel netwerk opgeven
Elke Resource Manager-resource heeft een eigen `properties` sectie voor specifieke configuraties voor de resource. In dit geval, geef het virtuele netwerk moet één subnet met behulp van het particuliere IP-adresbereik `10.0.0.0/16`. Een schaalset is altijd zich in een subnet. Er kan geen subnetten omspannen.

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
Naast de vereiste `type`, `name`, `apiVersion`, en `location` eigenschappen voor elke resource kunnen hebben een optioneel `dependsOn` lijst met tekenreeksen. Deze lijst geeft aan welke andere resources in deze implementatie moeten worden voltooid voordat het implementeren van deze resource.

Er is in dit geval slechts één element in de lijst, het virtuele netwerk van het vorige voorbeeld. U kunt deze afhankelijkheid opgeven omdat de Virtual Machine scale sets moet het netwerk bestaan voordat het maken van virtuele machines. Op deze manier de schaalset kan deze persoonlijke IP-adressen van virtuele machines geven van het IP-adresbereik dat eerder is opgegeven in de eigenschappen van het netwerk. De indeling van elke tekenreeks in de lijst met dependsOn is `<type>/<name>`. Gebruik dezelfde `type` en `name` eerder hebt gebruikt in de resource-uitbreiding van het virtuele netwerk.

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
Schaalsets hebben veel eigenschappen voor het aanpassen van de virtuele machines in de schaalset. Zie voor een volledige lijst van deze eigenschappen, de [sjabloonverwijzing](/azure/templates/microsoft.compute/virtualmachinescalesets). Voor deze zelfstudie zijn slechts enkele veelgebruikte eigenschappen worden ingesteld.
### <a name="supply-vm-size-and-capacity"></a>VM-grootte en capaciteit op te geven
De schaalset moet weten welke grootte van virtuele machine te maken ('sku-naam') en hoeveel dergelijke virtuele machines te maken ('sku-capaciteit'). Als u wilt zien welke VM-grootten beschikbaar zijn, Zie de [VM-grootten documentatie](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-sizes).

```json
       "sku": {
         "name": "Standard_A1",
         "capacity": 2
       },
```

### <a name="choose-type-of-updates"></a>Kies het type updates
De schaalset ook moet weten over het verwerken van updates op de schaalset. Er zijn momenteel twee opties `Manual` en `Automatic`. Zie de documentatie voor meer informatie over de verschillen tussen de twee op [upgrade uitvoeren van een schaalset](./virtual-machine-scale-sets-upgrade-scale-set.md).

```json
       "properties": {
         "upgradePolicy": {
           "mode": "Manual"
         },
```

### <a name="choose-vm-operating-system"></a>VM-besturingssysteem kiezen
De schaalset moeten weten welk besturingssysteem op de virtuele machines wilt plaatsen. Hier kunt u de VM's maken met een volledig patches Ubuntu 16.04 LTS-installatiekopie.

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
De schaalset implementeert u meerdere virtuele machines. Geef in plaats van op te geven de naam van elke virtuele machine, `computerNamePrefix`. De schaalset wordt toegevoegd een index aan het voorvoegsel voor elke virtuele machine, zodat de namen van de virtuele machine de notatie moeten `<computerNamePrefix>_<auto-generated-index>`.

Gebruik de parameters van vóór de gebruikersnaam voor de beheerder en het wachtwoord instellen voor alle virtuele machines in de schaalset in het volgende codefragment. Dit proces maakt gebruik van de `parameters` sjabloonfunctie. Deze functie omvat een tekenreeks is die Hiermee geeft u op welke parameter om te verwijzen naar en de waarde voor die parameter weergeeft.

```json
           "osProfile": {
             "computerNamePrefix": "vm",
             "adminUsername": "[parameters('adminUsername')]",
             "adminPassword": "[parameters('adminPassword')]"
           },
```

### <a name="specify-vm-network-configuration"></a>VM-netwerkconfiguratie opgeven
Ten slotte geeft u de netwerkconfiguratie voor de virtuele machines in de schaalset. In dit geval hoeft u alleen te geven van de ID van het subnet eerder hebt gemaakt. Hiermee wordt aangegeven voor de schaalset om de netwerkinterfaces in dit subnet.

Krijgt u de ID van het virtuele netwerk met het subnet met behulp van de `resourceId` sjabloonfunctie. Deze functie gebruikt in het type en de naam van een resource en retourneert de volledig gekwalificeerde id van die resource. Deze ID heeft de notatie: `/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/<resourceProviderNamespace>/<resourceType>/<resourceName>`

De id van het virtuele netwerk is echter niet voldoende. Geef de specifieke subnet dat de schaalset-VM's moet zich in. U doet dit door samenvoegen `/subnets/mySubnet` aan de ID van het virtuele netwerk. Het resultaat is de volledig gekwalificeerde ID van het subnet. Voer deze samenvoeging met de `concat` functie, die neemt in een reeks tekenreeksen en retourneert de samenvoeging.

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
