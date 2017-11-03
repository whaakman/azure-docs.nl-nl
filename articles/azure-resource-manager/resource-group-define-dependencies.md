---
title: Implementatievolgorde voor Azure-resources instellen | Microsoft Docs
description: "Hierin wordt beschreven hoe een resource als afhankelijk is van een andere resource tijdens de implementatie om ervoor te zorgen resources in de juiste volgorde worden geïmplementeerd."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: 
ms.assetid: 34ebaf1e-480c-4b4d-9bf6-251bd3f8f2cf
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/03/2017
ms.author: tomfitz
ms.openlocfilehash: 3d6a46116ae9d7d940bc10dfa832540f42c0af7e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="define-the-order-for-deploying-resources-in-azure-resource-manager-templates"></a>De volgorde voor het implementeren van resources in Azure Resource Manager-sjablonen definiëren
Voor een bepaalde bron, kunnen er andere bronnen die u moeten bestaan voordat de resource is geïmplementeerd. Bijvoorbeeld, moet een SQL-server bestaan voordat u probeert te implementeren van een SQL-database. U definiëren deze relatie met een resource als afhankelijk van de andere bron markeren. U definieert een afhankelijkheid tussen de **dependsOn** element, of met behulp van de **verwijzing** functie. 

Resource Manager evalueert de afhankelijkheden tussen resources en ze worden geïmplementeerd in de afhankelijke volgorde. Wanneer u resources zijn niet afhankelijk van elkaar, worden deze door Resource Manager parallel implementeert. U hoeft alleen te definiëren van afhankelijkheden voor resources die zijn geïmplementeerd in dezelfde sjabloon. 

## <a name="dependson"></a>dependsOn
Binnen uw sjabloon kunt het element dependsOn u één resource definiëren als een afhankelijk zijn van een of meer resources. De waarde kan een door komma's gescheiden lijst met resourcenamen zijn. 

Het volgende voorbeeld ziet een virtuele-machineschaalset die afhankelijk zijn van een load balancer, het virtuele netwerk en een lus die meerdere opslagaccounts worden gemaakt. Deze andere resources worden niet weergegeven in het volgende voorbeeld, maar moeten ze bestaan elders in de sjabloon.

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

In het voorgaande voorbeeld wordt een afhankelijkheid is opgenomen op de resources die zijn gemaakt via een For-lus kopiëren met de naam **storageLoop**. Zie voor een voorbeeld [maken van meerdere exemplaren van resources in Azure Resource Manager](resource-group-create-multiple.md).

Bij het definiëren van afhankelijkheden, kunt u de naamruimte resourceprovider en het resourcetype om verwarring te voorkomen kunt opnemen. Bijvoorbeeld, om te verduidelijken een load balancer en het virtuele netwerk met dezelfde naam als andere resources, gebruikt u de volgende indeling:

```json
"dependsOn": [
  "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]",
  "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
]
``` 

Terwijl u kan worden blootgesteld aan dependsOn gebruiken voor het toewijzen van relaties tussen uw resources, is het belangrijk te begrijpen waarom u dit doet. Om het document hoe resources onderling verbonden, bijvoorbeeld, is het dependsOn niet de juiste aanpak. U kunt het welke resources zijn gedefinieerd in het element dependsOn na de implementatie kan geen query uitvoeren. Met behulp van dependsOn u mogelijk gevolgen hebben voor implementatietijd omdat Resource Manager niet is geïmplementeerd in parallelle twee resources met een afhankelijkheid. In plaats daarvan gebruik om relaties tussen resources document [resources](/rest/api/resources/resourcelinks).

## <a name="child-resources"></a>Onderliggende resources
De eigenschap resources kunt u opgeven van de onderliggende resources die zijn gerelateerd aan de bron wordt gedefinieerd. Onderliggende resources kunnen alleen worden gedefinieerd vijf niveaus. Het is belangrijk te weten dat een impliciete afhankelijkheid niet tussen een bron van de onderliggende en de bovenliggende resource is gemaakt. Als u de onderliggende resource te worden geïmplementeerd nadat de bovenliggende resource nodig hebt, moet u deze afhankelijkheid met de eigenschap dependsOn expliciet vermelden. 

Elke bovenliggende resource accepteert alleen bepaalde resourcetypen als onderliggende resources. De geaccepteerde brontypen zijn opgegeven in de [sjabloonschema](https://github.com/Azure/azure-resource-manager-schemas) van de bovenliggende resource. Bevat de naam van het type van de bovenliggende resource, zoals de naam van de onderliggende brontype **Microsoft.Web/sites/config** en **Microsoft.Web/sites/extensions** zijn beide onderliggende resources van de **Microsoft.Web/sites**.

Het volgende voorbeeld ziet een SQL server en SQL-database. U ziet dat er een expliciete afhankelijkheid is gedefinieerd tussen de SQL-database en SQL server, ondanks dat de database een onderliggend element van de server is.

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

## <a name="reference-function"></a>verwijzing naar functie
De [verwijst naar functie](resource-group-template-functions-resource.md#reference) kunt u een expressie die moet worden afgeleid van de waarde van andere JSON naam / waarde-paren of runtime-bronnen. Verwijzingsexpressies declareren impliciet dat één resource afhankelijk van een andere is. De algemene indeling is:

```json
reference('resourceName').propertyPath
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

U kunt dit element of het element dependsOn gebruiken om op te geven, afhankelijkheden, maar u hoeft niet te gebruiken voor dezelfde afhankelijke resource. Gebruik indien mogelijk een impliciete verwijzing om te voorkomen dat een onnodige afhankelijkheid toevoegen.

Zie voor meer informatie, [verwijst naar functie](resource-group-template-functions-resource.md#reference).

## <a name="recommendations-for-setting-dependencies"></a>Aanbevelingen voor het instellen van de afhankelijkheden

Wanneer u beslist welke afhankelijkheden in te stellen, gebruikt u de volgende richtlijnen:

* Afhankelijkheden van zo weinig mogelijk ingesteld.
* Stel een onderliggende resource als afhankelijk van de bovenliggende resource.
* Gebruik de **verwijzing** functie impliciete afhankelijkheden tussen resources die nodig zijn voor het delen van een eigenschap instellen. Voeg een expliciete afhankelijkheid niet (**dependsOn**) wanneer u een impliciete afhankelijkheid al hebt gedefinieerd. Deze aanpak vermindert het risico van conflicterende onnodige afhankelijkheden. 
* Een afhankelijkheid ingesteld wanneer een resource kan niet worden **gemaakt** zonder de functionaliteit van een andere resource. Stel een afhankelijkheid niet als de bronnen alleen na de implementatie werken.
* Laat afhankelijkheden cascade zonder expliciet instelt. Bijvoorbeeld, de virtuele machine is afhankelijk van de virtuele netwerkinterface en de virtuele netwerkinterface is afhankelijk van een virtueel netwerk en openbare IP-adressen. Daarom wordt de virtuele machine is geïmplementeerd nadat alle drie resources, maar de virtuele machine niet expliciet als afhankelijk is van alle drie resources instelt. Deze aanpak wordt uitleg gegeven over de volgorde van afhankelijkheid en kunt u gemakkelijker de sjabloon later wijzigen.
* Als een waarde kan worden bepaald vóór de implementatie, probeert u de implementatie van de bron zonder een afhankelijkheid. Bijvoorbeeld, als een configuratiewaarde de naam van een andere resource moet, moet u mogelijk niet een afhankelijkheid. In deze richtlijnen werkt niet altijd omdat sommige resources verifieert of de andere resource. Als u een foutbericht ontvangt, voegt u een afhankelijkheid. 

Resource Manager identificeert circulaire afhankelijkheden tijdens de validatie van de sjabloon. Als u er een foutmelding weergegeven dat er een circulaire afhankelijkheid bestaat, evalueren de sjabloon of alle afhankelijkheden zijn niet nodig en kunnen worden verwijderd. Als het verwijderen van afhankelijkheden niet werkt, kunt u circulaire afhankelijkheden voorkomen door het verplaatsen van bepaalde implementatiebewerkingen naar onderliggende resources die zijn geïmplementeerd nadat de resources met circulaire afhankelijkheid een. Stel bijvoorbeeld dat u twee virtuele machines implementeert, maar u moet eigenschappen instellen voor elk criterium die verwijzen naar de andere. U kunt deze implementeren in de volgende volgorde:

1. vm1
2. vm2
3. Extensie op vm1, is afhankelijk van vm1 en vm2. De extensie waarden ingesteld op vm1 die van vm2 krijgt.
4. Extensie op vm2, is afhankelijk van vm1 en vm2. De extensie waarden ingesteld op vm2 van van vm1 krijgt.

Zie voor meer informatie over de beoordeling van de implementatievolgorde en het oplossen van afhankelijkheidsfouten [oplossen van veelvoorkomende fouten voor Azure-implementatie met Azure Resource Manager](resource-manager-common-deployment-errors.md).

## <a name="next-steps"></a>Volgende stappen
* Zie voor meer informatie over het oplossen van afhankelijkheden tijdens de implementatie, [oplossen van veelvoorkomende fouten voor Azure-implementatie met Azure Resource Manager](resource-manager-common-deployment-errors.md).
* Zie voor meer informatie over het maken van Azure Resource Manager-sjablonen, [sjablonen](resource-group-authoring-templates.md). 
* Zie voor een lijst van de beschikbare functies in een sjabloon, [sjabloonfuncties](resource-group-template-functions.md).

