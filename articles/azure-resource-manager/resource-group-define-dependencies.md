---
title: Implementatievolgorde voor Azure-resources instellen | Microsoft Docs
description: Beschrijft hoe u het instellen van een resource als afhankelijk zijn van een andere resource tijdens de implementatie om ervoor te zorgen resources worden geïmplementeerd in de juiste volgorde.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: ''
ms.assetid: 34ebaf1e-480c-4b4d-9bf6-251bd3f8f2cf
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/05/2018
ms.author: tomfitz
ms.openlocfilehash: 5b4d8317d565528f896bf6823ddaefd010d0a845
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2018
ms.locfileid: "39528626"
---
# <a name="define-the-order-for-deploying-resources-in-azure-resource-manager-templates"></a>De volgorde voor het implementeren van resources in Azure Resource Manager-sjablonen definiëren
Voor een bepaalde resource, kunnen er andere resources die moeten bestaan voordat de resource is geïmplementeerd. Bijvoorbeeld, moet een SQL-server bestaan voordat u probeert te implementeren van een SQL-database. Deze relatie definieert u een resource als afhankelijk van de andere resource markeren. U definieert een afhankelijkheid met de **dependsOn** -element, of met behulp van de **verwijzing** functie. 

Resource Manager evalueert de afhankelijkheden tussen resources en implementeert ze in de volgorde van afhankelijkheid. Als resources niet van elkaar afhankelijk zijn, worden deze door Resource Manager parallel geïmplementeerd. U hoeft alleen te definiëren van afhankelijkheden voor resources die zijn geïmplementeerd in dezelfde sjabloon. 

## <a name="dependson"></a>dependsOn
In uw sjabloon kunt het element dependsOn u een resource als een afhankelijk van een of meer resources definiëren. De waarde kan een lijst met door komma's gescheiden namen van de resources zijn. 

Het volgende voorbeeld ziet een virtuele-machineschaalset die afhankelijk zijn van een load balancer, het virtuele netwerk en een lus die meerdere opslagaccounts worden gemaakt. Deze andere resources worden niet weergegeven in het volgende voorbeeld, maar ze moeten elders aanwezig in de sjabloon.

```json
{
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  "name": "[variables('namingInfix')]",
  "location": "[variables('location')]",
  "apiVersion": "2016-03-30",
  "tags": {
    "displayName": "VMScaleSet"
  },
  "dependsOn": [
    "[variables('loadBalancerName')]",
    "[variables('virtualNetworkName')]",
    "storageLoop",
  ],
  ...
}
```

In het voorgaande voorbeeld wordt een afhankelijkheid is opgenomen op de resources die zijn gemaakt via een For-lus kopiëren met de naam **storageLoop**. Zie voor een voorbeeld [meerdere exemplaren van resources maken in Azure Resource Manager](resource-group-create-multiple.md).

Bij het definiëren van afhankelijkheden, kunt u de naamruimte van de resource provider en het resourcetype om verwarring te voorkomen kunt opnemen. Bijvoorbeeld, voor het verduidelijken van een load balancer en een virtueel netwerk met dezelfde naam als andere resources, gebruikt u de volgende indeling:

```json
"dependsOn": [
  "[resourceId('Microsoft.Network/loadBalancers', variables('loadBalancerName'))]",
  "[resourceId('Microsoft.Network/virtualNetworks', variables('virtualNetworkName'))]"
]
``` 

Terwijl u kan worden blootgesteld aan dependsOn gebruiken voor het toewijzen van relaties tussen uw resources, is het belangrijk om te begrijpen waarom u dit doet. DependsOn om hoe resources met elkaar zijn verbonden, bijvoorbeeld niet de juiste heeft. U kunt het welke resources zijn gedefinieerd in het element dependsOn na de implementatie kan geen query uitvoeren. Met behulp van dependsOn kunt u mogelijk gevolgen hebben voor implementatietijd omdat Resource Manager niet implementeren in parallelle twee resources die afhankelijk zijn. 

## <a name="child-resources"></a>Onderliggende resources
De eigenschap resources kunt u om op te geven van de onderliggende resources die zijn gerelateerd aan de resource wordt gedefinieerd. Onderliggende resources mag alleen bestaan uit gedefinieerde vijf niveaus. Het is belangrijk te weten dat een impliciete afhankelijkheid tussen een onderliggende resource en de bovenliggende resource is niet gemaakt. Als u de onderliggende bron om te worden geïmplementeerd nadat de bovenliggende resource nodig hebt, moet u expliciet worden vermeld die afhankelijkheid met de eigenschap DEPENDSON te maken. 

Elke bovenliggende resource accepteert alleen bepaalde resourcetypen als onderliggende resources. De toegestane resourcetypen zijn opgegeven in de [sjabloonschema](https://github.com/Azure/azure-resource-manager-schemas) van de bovenliggende resource. De naam van het type van onderliggende resource bevat de naam van het type van de bovenliggende resource, zoals **Microsoft.Web/sites/config** en **Microsoft.Web/sites/extensions** zijn beide onderliggende resources van de **Microsoft.Web/sites**.

Het volgende voorbeeld ziet een SQL server en SQL-database. U ziet dat er een expliciete afhankelijkheid is gedefinieerd tussen de SQL-database en SQL server, zelfs als de database een onderliggend element van de server is.

```json
"resources": [
  {
    "name": "[variables('sqlserverName')]",
    "type": "Microsoft.Sql/servers",
    "location": "[resourceGroup().location]",
    "tags": {
      "displayName": "SqlServer"
    },
    "apiVersion": "2014-04-01-preview",
    "properties": {
      "administratorLogin": "[parameters('administratorLogin')]",
      "administratorLoginPassword": "[parameters('administratorLoginPassword')]"
    },
    "resources": [
      {
        "name": "[parameters('databaseName')]",
        "type": "databases",
        "location": "[resourceGroup().location]",
        "tags": {
          "displayName": "Database"
        },
        "apiVersion": "2014-04-01-preview",
        "dependsOn": [
          "[variables('sqlserverName')]"
        ],
        "properties": {
          "edition": "[parameters('edition')]",
          "collation": "[parameters('collation')]",
          "maxSizeBytes": "[parameters('maxSizeBytes')]",
          "requestedServiceObjectiveName": "[parameters('requestedServiceObjectiveName')]"
        }
      }
    ]
  }
]
```

## <a name="reference-and-list-functions"></a>referentie- en -functies
De [verwijzen naar de functie](resource-group-template-functions-resource.md#reference) wordt een expressie voor de waarde ervan afgeleid van andere JSON naam / waarde-paren of runtime-bronnen. De [lijst * functies](resource-group-template-functions-resource.md#list) waarden voor een resource van de bewerking voor een lijst met retourneren.  Referentie- en lijst met expressies declareren impliciet dat een resource afhankelijk van een andere, is wanneer de resource waarnaar wordt verwezen, is geïmplementeerd in dezelfde sjabloon en waarnaar wordt verwezen door de naam (geen resource-ID). Als u de resource-ID in de verwijzing of lijst met functies doorgeeft, is een impliciete verwijzing is niet gemaakt.

De algemene indeling van de functie verwijzing is:

```json
reference('resourceName').propertyPath
```

De algemene indeling van de functie listKeys is:

```json
listKeys('resourceName', 'yyyy-mm-dd')
```

In het volgende voorbeeld wordt een CDN-eindpunt expliciet is afhankelijk van het CDN-profiel en impliciet afhankelijk is van een web-app.

```json
{
    "name": "[variables('endpointName')]",
    "type": "endpoints",
    "location": "[resourceGroup().location]",
    "apiVersion": "2016-04-02",
    "dependsOn": [
            "[variables('profileName')]"
    ],
    "properties": {
        "originHostHeader": "[reference(variables('webAppName')).hostNames[0]]",
        ...
    }
```

U kunt dit element of het element dependsOn gebruiken om op te geven van afhankelijkheden, maar u niet wilt gebruiken voor dezelfde afhankelijke resource. Gebruik indien mogelijk een impliciete verwijzing om te voorkomen dat een onnodige afhankelijkheid toe te voegen.

Zie voor meer informatie, [verwijzen naar de functie](resource-group-template-functions-resource.md#reference).

## <a name="recommendations-for-setting-dependencies"></a>Aanbevelingen voor het instellen van afhankelijkheden

Wanneer u bepaalt welke afhankelijkheden om in te stellen, gebruikt u de volgende richtlijnen:

* Afhankelijkheden van zo weinig mogelijk ingesteld.
* Een onderliggende resource instellen als afhankelijk van de bovenliggende resource.
* Gebruik de **verwijzing** functioneren en geef de resourcenaam impliciete afhankelijkheden tussen resources die nodig hebt voor het delen van een eigenschap instellen. Geen een expliciete afhankelijkheid toevoegen (**dependsOn**) wanneer u een impliciete afhankelijkheid al hebt gedefinieerd. Deze aanpak vermindert het risico dat onnodige afhankelijkheden. 
* Een afhankelijkheid ingesteld wanneer een resource kan niet worden **gemaakt** zonder de functionaliteit van een andere resource. Een afhankelijkheid niet worden ingesteld als de resources alleen interactie kunnen aangaan na de implementatie.
* Laat afhankelijkheden cascade zonder expliciet instelt. Bijvoorbeeld: uw virtuele machine is afhankelijk van een virtuele netwerkinterface en de virtuele netwerkinterface is afhankelijk van een virtueel netwerk en openbare IP-adressen. Daarom wordt de virtuele machine is geïmplementeerd nadat alle drie resources, maar de virtuele machine niet expliciet worden ingesteld als afhankelijk is van alle drie resources. Deze aanpak wordt uitleg gegeven over de volgorde van de afhankelijkheid en kunt u gemakkelijker de sjabloon later wijzigen.
* Als een waarde kan worden bepaald vóór de implementatie, kunt u de implementatie van de resource zijn zonder een afhankelijkheid. Bijvoorbeeld, als een configuratiewaarde de naam van een andere bron moet, moet u mogelijk niet een afhankelijkheid. In deze richtlijnen werken niet altijd, omdat sommige resources controleren of er sprake van de andere resource. Als u een foutbericht ontvangt, moet u een afhankelijkheid toevoegen. 

Resource Manager identificeert circulaire afhankelijkheden tijdens de sjabloonvalidatie. Als u een foutmelding waarin staat dat er een circulaire afhankelijkheid bestaat, evalueren de sjabloon om te controleren of alle afhankelijkheden zijn niet nodig en kunnen worden verwijderd. Als het verwijderen van afhankelijkheden niet werkt, kunt u circulaire afhankelijkheden voorkomen door bepaalde implementatiebewerkingen verplaatsen naar onderliggende resources die zijn geïmplementeerd nadat de resources die u een circulaire afhankelijkheid hebt. Stel bijvoorbeeld dat u twee virtuele machines implementeert, maar moet u de eigenschappen instellen op elk die verwijzen naar de andere. U kunt deze implementeren in de volgende volgorde:

1. vm1
2. vm2
3. Extensie op vm1, is afhankelijk van vm1 en vm2. De extensie stelt waarden in voor vm1 die van vm2 krijgt.
4. Extensie op vm2, is afhankelijk van vm1 en vm2. De extensie stelt waarden in voor vm2 die van vm1 krijgt.

Zie voor meer informatie over de implementatievolgorde vast te stellen en het oplossen van afhankelijkheidsfouten [veelvoorkomende problemen oplossen Azure-implementatie met Azure Resource Manager](resource-manager-common-deployment-errors.md).

## <a name="next-steps"></a>Volgende stappen
* Zie voor meer informatie over het oplossen van afhankelijkheden tijdens de implementatie, [veelvoorkomende problemen oplossen Azure-implementatie met Azure Resource Manager](resource-manager-common-deployment-errors.md).
* Zie voor meer informatie over het maken van Azure Resource Manager-sjablonen, [-sjablonen maken](resource-group-authoring-templates.md). 
* Zie voor een lijst van de beschikbare functies in een sjabloon, [sjabloonfuncties](resource-group-template-functions.md).

