---
title: Implementatiesjablonen maken voor Azure Logic Apps | Microsoft Docs
description: Azure Resource Manager-sjablonen voor het implementeren van logische apps maken
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.assetid: 85928ec6-d7cb-488e-926e-2e5db89508ee
ms.date: 10/18/2016
ms.openlocfilehash: 624539557b0bf57e9d919a3a46337f1cf93a4f07
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58894231"
---
# <a name="create-azure-resource-manager-templates-for-deploying-logic-apps"></a>Azure Resource Manager-sjablonen voor het implementeren van logische apps maken

Wanneer u een logische app maakt, kunt u uitbreiden van uw logische app-definitie in een [Azure Resource Manager-sjabloon](../azure-resource-manager/resource-group-overview.md). U kunt deze sjabloon vervolgens gebruiken voor het automatiseren van implementaties met het definiëren van de resources en parameters dat u wilt gebruiken voor implementatie en het geven van de parameterwaarden via een [parameterbestand](../azure-resource-manager/resource-group-template-deploy.md#parameter-files).
Op die manier kunt u logische apps implementeren eenvoudiger en met elke omgeving of Azure-resourcegroep die u wilt. 

Azure Logic Apps biedt een [vooraf gemaakte logische apps in Azure Resource Manager-sjabloon](https://github.com/Azure/azure-quickstart-templates/blob/master/101-logic-app-create/azuredeploy.json) die u opnieuw gebruiken kunt, niet alleen voor het maken van logische apps, maar ook om de resources en parameters voor de implementatie te definiëren. U kunt deze sjabloon gebruiken voor uw eigen zakelijke scenario's of de sjabloon om te voldoen aan uw behoeften aanpassen. Meer informatie over [de structuur van de Resource Manager-sjabloon en de syntaxis](../azure-resource-manager/resource-group-authoring-templates.md). Zie voor JSON-syntaxis en eigenschappen [Microsoft.Logic resourcetypen](/azure/templates/microsoft.logic/allversions).

Zie voor meer informatie over Azure Resource Manager-sjablonen, de volgende artikelen:

* [Azure Resource Manager-sjablonen maken](../azure-resource-manager/resource-group-authoring-templates.md)
* [Azure Resource Manager-sjablonen voor de consistentie van de cloud ontwikkelen](../azure-resource-manager/templates-cloud-consistency.md)

## <a name="logic-app-structure"></a>Structuur van logische app

De definitie van uw logische app heeft deze basisgedeelten, die u bekijken kunt door het overschakelen van de 'designer-weergave' naar 'codeweergave' of door een hulpprogramma zoals [Azure Resource Explorer](http://resources.azure.com). Definities voor logische Apps gebruik van Javascript Object Notation (JSON), dus voor meer informatie over de JSON-syntaxis en eigenschappen [Microsoft.Logic resourcetypen](/azure/templates/microsoft.logic/allversions).

* **Logische app-resource**: Hierin wordt beschreven informatie zoals de locatie van uw logische app of de regio, prijzen plan en de definitie van de werkstroom.

* **Definitie van de werkstroom**: Beschrijving van uw logische app triggers en acties en hoe de service Azure Logic Apps wordt de werkstroom wordt uitgevoerd. In de weergave van code, vindt u de werkstroomdefinitie van de in de `definition` sectie.

* **Verbindingen**: Als u beheerde connectors in uw logische app gebruiken, de `$connections` sectie verwijst naar andere resources die veilig opslaan van metagegevens over deze verbindingen tussen uw logische app en andere systemen of -services, zoals verbindingsreeksen en toegangstokens. In de definitie van uw logische app verwijzingen naar deze verbindingen worden weergegeven in uw logic app-definitie `parameters` sectie.

Voor het maken van een sjabloon voor logische app die u met Azure resource group-implementaties gebruiken kunt, moet u de resources definiëren en zo nodig worden voorzien. Bijvoorbeeld, als u een ontwikkeling, testen en productie-omgeving implementeert, wilt u waarschijnlijk gebruiken verschillende verbindingsreeksen naar een SQL-database in elke omgeving.
Of u wilt implementeren in verschillende abonnementen of resourcegroepen.

## <a name="create-logic-app-deployment-templates"></a>Logic app-implementatiesjablonen maken

Voor de eenvoudigste manier om een geldige logic app-implementatiesjabloon maken, gebruikt u Visual Studio en de Azure Logic Apps-hulpprogramma's voor Visual Studio-uitbreiding. Uw logische app downloaden via de Azure-portal naar Visual Studio, krijgt u een geldige implementatie-sjabloon die u met een willekeurige Azure-abonnement en locatie gebruiken kunt. Automatisch downloaden van uw logische app parameterizes ook de definitie van de logische app die ingesloten in de sjabloon.
Zie voor meer informatie over het maken en beheren van logische apps in Visual Studio, [logische apps maken met Visual Studio](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md) en [logische apps beheren met Visual Studio](../logic-apps/manage-logic-apps-with-visual-studio.md).

Met uitzondering van Visual Studio of handmatig maken van uw sjabloon en de vereiste parameters door de instructies in dit onderwerp te volgen, u kunt ook de [PowerShell-module voor het maken van logische app-sjablonen](https://github.com/jeffhollan/LogicAppTemplateCreator). Deze open-source-module wordt eerst geëvalueerd uw logische app en alle verbindingen die gebruikmaakt van de logische app. De module genereert vervolgens sjabloonresources met de vereiste parameters voor de implementatie. Stel bijvoorbeeld dat u hebt een logische app die u ontvangt een bericht van een Azure Service Bus-wachtrij en voegt gegevens toe aan een Azure SQL database. De module hulpprogramma behoudt alle de orchestration-logica en de SQL- en Service Bus-verbindingsreeksen parameterizes zodat u deze waarden tijdens de implementatie instellen kunt.

> [!IMPORTANT]
> Verbindingen moeten zich in dezelfde Azure-resourcegroep bevinden als de logische app.
> Voor de PowerShell-module om te werken met Azure-tenant en abonnement access token, gebruik de module met de [hulpprogramma voor Azure Resource Manager-client](https://github.com/projectkudu/ARMClient). Zie voor meer informatie dit [artikel over het hulpprogramma Azure Resource Manager-client](https://blog.davidebbo.com/2015/01/azure-resource-manager-client.html) ARMClient vindt u meer details.

### <a name="install-powershell-module-for-logic-app-templates"></a>PowerShell-module voor sjablonen voor logische Apps installeren

Voor de eenvoudigste manier voor het installeren van de module op basis van de [PowerShell Gallery](https://www.powershellgallery.com/packages/LogicAppTemplate/0.1), gebruikt u deze opdracht:

`Install-Module -Name LogicAppTemplate`

U kunt ook handmatig de PowerShell-module installeren:

1. Download de meest recente [Logic App-sjabloon ontwikkelaar](https://github.com/jeffhollan/LogicAppTemplateCreator/releases).

1. Pak de map van uw PowerShell-module, die meestal `%UserProfile%\Documents\WindowsPowerShell\Modules`.

### <a name="generate-logic-app-template---powershell"></a>Genereren van de sjabloon voor logische app - PowerShell

Nadat u PowerShell hebt geïnstalleerd, kunt u een sjabloon kunt genereren met behulp van de volgende opdracht uit:

`armclient token $SubscriptionId | Get-LogicAppTemplate -LogicApp MyApp -ResourceGroup MyRG -SubscriptionId $SubscriptionId -Verbose | Out-File C:\template.json`

`$SubscriptionId` is de Azure-abonnement-ID. Deze regel voor de eerste keer een token via ARMClient, en geeft deze door naar de PowerShell-script en maakt vervolgens de sjabloon in een JSON-bestand.

## <a name="parameters-in-logic-app-templates"></a>Parameters in de sjablonen voor logische Apps

Nadat u de sjabloon voor uw logische app gemaakt, kunt u deze kunt toevoegen en bewerken van alle benodigde parameters op. De sjabloon heeft meer dan één `parameters` sectie, bijvoorbeeld: 

* Definitie van de werkstroom van uw logische app heeft een eigen [ `parameters` sectie](../logic-apps/logic-apps-workflow-definition-language.md#parameters) waar u de parameters die uw logische app wordt gebruikt voor het accepteren van invoer bij de implementatie kunt definiëren.

* Uw Resource Manager-sjabloon heeft een eigen [ `parameters` sectie](../azure-resource-manager/resource-group-authoring-templates.md#parameters), los van uw logische app `parameters` sectie. Bijvoorbeeld:

  [!INCLUDE [logic-deploy-parameters](../../includes/app-service-logic-deploy-parameters.md)]

Stel bijvoorbeeld dat de definitie van uw logische app verwijst naar een resource-ID die een Azure-functie of een geneste logische app-werkstroom vertegenwoordigt, en u wilt implementeren die resource-ID samen met uw logische app als een enkele implementatie. U kunt toevoegen die ID als een resource in uw sjabloon en parameters die ID. U kunt deze benadering ook gebruiken voor verwijzingen naar aangepaste API's of OpenAPI-eindpunten (voorheen ' Swagger') die u geïmplementeerde met elke Azure-resourcegroep wilt.

Wanneer u parameters in uw sjabloon voor de implementatie, deze richtlijnen volgt, zodat de ontwerper van logische Apps kunnen deze parameters correct worden weergegeven:

* Parameters alleen in deze triggers en acties gebruiken:

  * Azure Functions-app
  * Geneste of onderliggende werkstroom voor logische Apps
  * Aanroep van API Management
  * Runtime-URL van API-verbinding
  * Pad naar API-verbinding

* Wanneer u parameters definiëren, zorg ervoor dat u standaardwaarden met behulp van opgeven de `defaultValue` eigenschapswaarde, bijvoorbeeld:

  ```json
  "parameters": {
     "IntegrationAccount": {
        "type":"string",
        "minLength": 1,
        "defaultValue": "/subscriptions/<Azure-subscription-ID>/resourceGroups/<Azure-resource=group-name>/providers/Microsoft.Logic/integrationAccounts/<integration-account-name>"
     }
  },
  ```

* Beveilig gevoelige informatie in de sjablonen wilt beveiligen, de parameters. Meer informatie over [over het gebruik van beveiligde parameters](../logic-apps/logic-apps-securing-a-logic-app.md#secure-parameters-workflow).

Hier volgt een voorbeeld waarin wordt weergegeven hoe u de Azure Service Bus "Bericht verzenden"-actie kunt voorzien:

```json
"Send_message": {
   "type": "ApiConnection",
   "inputs": {
      "host": {
         "connection": {
            "name": "@parameters('$connections')['servicebus']['connectionId']"
         },
         // If the `host.runtimeUrl` property appears in your template, 
         // you can remove this property, which is optional, for example:
         "runtimeUrl": {}
      },
      "method": "POST",
      "path": "[concat('/@{encodeURIComponent(''', parameters('<Azure-Service-Bus-queue-name>'), ''')}/messages')]",
      "body": {
         "ContentData": "@{base64(triggerBody())}"
      },
      "queries": {
         "systemProperties": "None"
      }
   },
   "runAfter": {}
},
```

### <a name="reference-dependent-resources"></a>Naslaginformatie over afhankelijke bronnen

Als uw logische app verwijzingen naar afhankelijke resources vereist, kunt u [Azure Resource Manager-sjabloonfuncties](../azure-resource-manager/resource-group-template-functions.md) in de sjabloon voor de implementatie van uw logische app. Stel bijvoorbeeld dat u wilt dat uw logische app te verwijzen naar een Azure-functie of een integratieaccount met de definities voor partners, overeenkomsten en andere artefacten die u wilt dat samen met uw logische app is geïmplementeerd.
U kunt functies van de Resource Manager-sjablonen, zoals `parameters`, `variables`, `resourceId`, `concat`, enzovoort.

Hier volgt een voorbeeld waarin wordt weergegeven hoe u de resource-ID voor een Azure-functie kunt vervangen door deze parameters te definiëren:

``` json
"parameters": {
   "<Azure-function-name>": {
      "type": "string",
      "minLength": 1,
      "defaultValue": "<Azure-function-name>"
   }
},
```

Dit is hoe u deze parameters gebruiken wanneer u verwijst naar de Azure-functie:

```json
"MyFunction": {
   "type": "Function",
   "inputs": {
      "body": {},
      "function": {
         "id":"[resourceid('Microsoft.Web/sites/functions','<Azure-Functions-app-name>', parameters('<Azure-function-name>'))]"
      }
   },
   "runAfter": {}
},
```

## <a name="add-logic-app-to-resource-group-project"></a>Logische app toevoegen aan het resourcegroepproject

Als u een bestaand Azure-resourcegroep-project hebt, kunt u uw logische app aan dat project toevoegen met behulp van het venster JSON Outline. U kunt ook een andere logische app naast de app die u eerder hebt gemaakt toevoegen.

1. Open in Solution Explorer de `<template>.json` bestand.

2. Uit de **weergave** in het menu **andere Windows** > **JSON-overzicht**.

3. Als u wilt toevoegen een bron naar het sjabloonbestand, kies **Resource toevoegen** aan de bovenkant van het venster JSON Outline. Of met de rechtermuisknop in het venster JSON Outline **resources**, en selecteer **nieuwe Resource toevoegen**.

   ![Venster JSON-overzicht](./media/logic-apps-create-deploy-template/jsonoutline.png)

4. In de **Resource toevoegen** in het dialoogvenster, zoeken en selecteer **logische App**. De naam van uw logische app, en kies **toevoegen**.

   ![Resource toevoegen](./media/logic-apps-create-deploy-template/addresource.png)

## <a name="get-support"></a>Ondersteuning krijgen

Ga naar het [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) (Forum voor Azure Logic Apps) als u vragen hebt.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Sjablonen voor logische Apps implementeren](../logic-apps/logic-apps-create-deploy-azure-resource-manager-templates.md)
