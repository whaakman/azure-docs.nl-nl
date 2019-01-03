---
title: Over het maken van Azure Resource Manager-sjabloon
description: Beschrijft het proces van het ontwerpen van een Azure Resource Manager-sjabloon.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/27/2018
ms.author: tomfitz
ms.openlocfilehash: abfc7ce78e8676e9560621be1ec9a81717d958e5
ms.sourcegitcommit: fd488a828465e7acec50e7a134e1c2cab117bee8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2019
ms.locfileid: "53994280"
---
# <a name="create-azure-resource-manager-template"></a>Azure Resource Manager-sjabloon maken

Dit artikel beschrijft het proces en de beslissingen die u bij het maken van een Azure Resource Manager-sjabloon. Het biedt een overzicht van functies die u helpen kunnen bij het ontwerpen van uw sjabloon en voorbeelden. Het artikel wordt ervan uitgegaan dat u resources implementeert naar een resourcegroep. Als u wilt resources implementeren op uw Azure-abonnement, zoals het maken van beleid voor het Azure of op rollen gebaseerd beheer toegangstoewijzingen, Zie [resourcegroepen en resources voor een Azure-abonnement maken](deploy-to-subscription.md).

## <a name="select-json-editor"></a>Selecteer JSON-editor

De Resource Manager-sjabloon is een JSON-bestand. U moet een goede bewerkingsprogramma werken in het JSON-bestand. U beschikt over veel opties, maar als u een editor die u wilt, nog niet hebt geïnstalleerd [Visual Studio Code (VS-Code)](https://code.visualstudio.com/). 

Na de installatie van VS Code, voeg de [extensie voor Azure Resource Manager-hulpprogramma's](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools). Deze extensie voegt veel functies die het ontwerpen van de sjabloon vereenvoudigen.

![Sjabloon voor Visual Studio Code](./media/how-to-create-template/template-visual-studio-code.png)

De schermafbeelding ziet u een Resource Manager-sjabloon geopend in Visual Studio Code. 

Zie voor een zelfstudie over het installeren van de Resource Manager-extensie voor hulpprogramma's en het gebruik van VS Code, [Quick Start: Azure Resource Manager-sjablonen maken met behulp van Visual Studio Code](./resource-manager-quickstart-create-templates-use-visual-studio-code.md).

## <a name="understand-the-template-structure"></a>Inzicht in de structuur van de sjabloon

Laten we bekijken de onderdelen van de sjabloon om te begrijpen hoe de sjabloon werkt. De sjabloon hebben mogelijk niet elke sectie. De secties die u wilt zich richten op zijn:

* De [parameters](resource-manager-templates-parameters.md) sectie waarin de waarden die u opgeven tijdens de implementatie van kunt het aanpassen van de infrastructuur die is geïmplementeerd. 

* De [variabelen](resource-manager-templates-variables.md) sectie waarin de waarden die worden gebruikt in de sjabloon.

* De [functies](resource-group-authoring-templates.md#functions) sectie, waaruit blijkt aangepast sjabloon-expressies die worden gebruikt in uw sjabloon.

* De [resources](resource-manager-templates-resources.md) sectie waarin de Azure-resources die zijn geïmplementeerd voor uw abonnement.

* De [levert](resource-manager-templates-outputs.md) sectie waarin de waarden die worden geretourneerd na de implementatie is voltooid.

## <a name="look-for-similar-templates"></a>Zoek naar vergelijkbare sjablonen

Vaak het geval is, vindt u een bestaande sjabloon die u een oplossing die is vergelijkbaar implementeert met wat u nodig hebt. De [Azure-Snelstartsjablonen](https://azure.microsoft.com/resources/templates/) honderden sjablonen uit de community-inzenders is.

![Snelstartgids-opslagplaats voor sjablonen](./media/how-to-create-template/template-quickstart-repo.png)

Doorzoek die opslagplaats voor een sjabloon die is vergelijkbaar met wat u nodig hebt. Het is geen probleem dat als de sjabloon niet precies wat u nodig hebt, kunt u deze kunt aanpassen.

Als er een sjabloon, selecteert u **zoeken op Github**, en kopieer vervolgens de **azuredeploy.json** -bestand van de opslagplaats. Maak een nieuw bestand met de naam in VS Code, **azuredeploy.json** en de inhoud van het sjabloonbestand die u hebt gekopieerd uit de opslagplaats voor Quickstart toevoegen.

## <a name="add-resources"></a>Resources toevoegen

Waarschijnlijk wilt u de sjabloon om ervoor te zorgen dat het geval is precies wat u wilt aanpassen. Bekijk eerst de resources die zijn geïmplementeerd. Mogelijk moet u toevoegen, verwijderen of wijzigen van resources in de sjabloon. Zie voor beschrijvingen en syntaxis van de resources [verwijzing naar de Azure Resource Manager-sjabloon](/azure/templates/).

![Sjabloonverwijzing](./media/how-to-create-template/template-reference.png)

Bekijk deze eigenschappen en maak eventuele wijzigingen. Zie voor aanbevelingen over het definiëren van resources [bronnen - aanbevolen procedures voor](template-best-practices.md#resources).

## <a name="add-or-remove-parameters"></a>Toevoegen of verwijderen van parameters

Mogelijk moet u ook de parameters voor de sjabloon aanpassen. U kunt toevoegen of verwijderen van de parameters die zijn gebaseerd op hoeveel aanpassing die u wilt inschakelen tijdens de implementatie. Zie voor aanbevelingen over het gebruik van parameters [parameters: aanbevolen procedures voor](template-best-practices.md#parameters).

## <a name="add-tags"></a>Tags toevoegen

U kunt labels toevoegen aan uw resources om een logische manier te organiseren per categorie en factureringskosten verdelen. Toevoegen van labels is eenvoudig, u ze toepast in de JSON voor de resource. Het volgende opslagaccount heeft bijvoorbeeld twee labels:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
      {
        "apiVersion": "2016-01-01",
        "type": "Microsoft.Storage/storageAccounts",
        "name": "[concat('storage', uniqueString(resourceGroup().id))]",
        "location": "[resourceGroup().location]",
        "tags": {
          "Dept": "Finance",
          "Environment": "Production"
        },
        "sku": {
          "name": "Standard_LRS"
        },
        "kind": "Storage",
        "properties": { }
      }
    ]
}
```

U kunt codes ook dynamisch toepassen van parameters. Zie voor meer informatie, [codes in de sjabloon](resource-manager-templates-resources.md#tags).

## <a name="review-template-functions"></a>Sjabloonfuncties bekijken

Merkt u wellicht de expressies in de sjabloon die wordt omgeven door vierkante haken, zoals `"[some-expression]"`. Deze expressies gebruiken sjabloonfuncties dynamisch waarden maken tijdens de implementatie.

Bijvoorbeeld, ziet u vaak een expressie, zoals:

```json
"name": "[parameters('siteName')]"
```

Deze expressie wordt de waarde van een parameter. De waarde is toegewezen aan de eigenschap name.

Of, ziet u mogelijk een complexe expressie die gebruikmaakt van verschillende functies, zoals:

```json
"[reference(resourceId(parameters('storageResourceGroup'), 'Microsoft.Storage/storageAccounts', parameters('storageAccountName')), '2018-07-01')]"
```

Deze expressie wordt een object met de eigenschappen van een storage-account.

Om te begrijpen wat de functies doet, Controleer de [functie sjabloonverwijzing](resource-group-template-functions.md) documentatie.

## <a name="create-more-than-one-instance"></a>Meer dan één exemplaar maken

Soms wilt u meer dan één exemplaar van een resource te maken. Bijvoorbeeld, moet u mogelijk meerdere opslagaccounts. In plaats daarvan dan herhalen van de resource via uw sjabloon, kunt u de `copy` syntaxis voor het opgeven van meer dan één exemplaar.

Het volgende voorbeeld maakt u drie opslagaccounts:

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

U kunt ook het aantal exemplaren van een parameter opgeven. Zie voor meer informatie, [implementeren van meer dan één instantie van een resource of eigenschap in Azure Resource Manager-sjablonen](resource-group-create-multiple.md).

## <a name="conditionally-deploy-a-resource"></a>Een resource voorwaardelijk implementeren

Soms moet u om op te geven tijdens de implementatie of een resource in de sjabloon wordt geïmplementeerd. Bijvoorbeeld, kunt u de flexibiliteit om een nieuwe resource te implementeren of een bestaande resource gebruiken. De `condition` element biedt u de mogelijkheid om in te schakelen in- of uitschakelen voor een resource-implementatie. Als de expressie in het element voorwaarde waar is, wordt de resource is geïmplementeerd. Indien onwaar, wordt de resource wordt overgeslagen tijdens de implementatie.

Het volgende voorbeeld wordt voorwaardelijk geïmplementeerd voor een storage-account:

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

Zie voor meer informatie de [voorwaarde element](resource-manager-templates-resources.md#condition).

## <a name="review-dependencies"></a>Afhankelijkheden controleren

Sommige resources in uw sjabloon moeten worden geïmplementeerd voordat u andere resources. Bijvoorbeeld, moet de SQL-server aanwezig zijn voordat de SQL-database wordt gemaakt. Resource Manager impliciet bepaalt de implementatievolgorde voor resources wanneer de [verwijzen naar de functie](resource-group-template-functions-resource.md#reference) wordt gebruikt. Echter, in sommige gevallen moet u expliciet de afhankelijkheden definiëren met behulp van de `dependsOn` element. Controleer de sjabloon om te zien als alle afhankelijkheden moeten worden toegevoegd. Wees voorzichtig onnodige afhankelijkheden niet toevoegen als ze kunnen implementatie vertragen of kringverwijzingen maken.

De volgende afbeelding toont de volgorde van de afhankelijkheid voor verschillende App Service-resources:

![Web-app-afhankelijkheden](./media/how-to-create-template/web-dependencies.png)

Het volgende voorbeeld ziet deel uit van een sjabloon die afhankelijkheden gedefinieerd.

```json
{
    "name": "[parameters('appName')]",
    "type": "Microsoft.Web/Sites",
    ...
    "resources": [
      {
          "name": "MSDeploy",
          "type": "Extensions",
          "dependsOn": [
            "[concat('Microsoft.Web/Sites/', parameters('appName'))]",
            "[concat('Microsoft.Sql/servers/', parameters('dbServerName'), '/databases/', parameters('dbName'))]",
          ],
          ...
      },
      {
          "name": "connectionstrings",
          "type": "config",
          "dependsOn": [
            "[concat('Microsoft.Web/Sites/', parameters('appName'), '/Extensions/MSDeploy')]"
          ],
          ...
      }
    ]
}
```

Zie [Define the order for deploying resources in Azure Resource Manager Templates](resource-group-define-dependencies.md) (De volgorde voor het implementeren van resources definiëren in Azure Resource Manager-sjablonen) voor meer informatie.

## <a name="review-recommended-practices"></a>Bekijk de aanbevolen procedures

Controleer voordat u de sjabloon implementeert, [aanbevolen procedures voor Azure Resource Manager-sjabloon](template-best-practices.md) om te zien of er een aanbevolen methoden die u wilt implementeren in uw sjabloon.

Als u uw sjabloon gebruiken in verschillende Azure-cloud-omgevingen wilt, Zie [ontwikkelen van Azure Resource Manager-sjablonen voor de consistentie van de cloud](templates-cloud-consistency.md).

## <a name="next-steps"></a>Volgende stappen

* Zie voor het implementeren van een sjabloon, [implementeren met Azure CLI](resource-group-template-deploy-cli.md) of [implementeren met PowerShell](resource-group-template-deploy.md).
* Zie voor een stapsgewijze snelstartgids over het maken van een sjabloon, [maken Azure Resource Manager-sjablonen met behulp van Visual Studio Code](resource-manager-quickstart-create-templates-use-visual-studio-code.md).
* Zie voor een lijst van de beschikbare functies in een sjabloon, [sjabloonfuncties](resource-group-template-functions.md).
