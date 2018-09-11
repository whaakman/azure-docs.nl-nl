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
ms.openlocfilehash: 393543bbb1891e14ed67487aff26a7bda1eebcd5
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/10/2018
ms.locfileid: "44304234"
---
# <a name="create-azure-resource-manager-templates-for-deploying-logic-apps"></a>Azure Resource Manager-sjablonen voor het implementeren van logische apps maken

Nadat een logische app is gemaakt, is het raadzaam deze als een Azure Resource Manager-sjabloon maken.
Op deze manier u kunt eenvoudig de logische app implementeren in een omgeving of de resourcegroep waar u deze mogelijk nodig.
Zie voor meer informatie over de Resource Manager-sjablonen, [Azure Resource Manager-sjablonen](../azure-resource-manager/resource-group-authoring-templates.md) en [resources implementeren met behulp van Azure Resource Manager-sjablonen](../azure-resource-manager/resource-group-template-deploy.md).

## <a name="logic-app-deployment-template"></a>Sjabloon voor logische app-implementatie

Een logische app heeft drie fundamentele onderdelen:

* **Logische app-resource**: bevat informatie over zaken zoals prijzen plan, locatie en de definitie van de werkstroom.
* **Definitie van de werkstroom**: Beschrijving van uw logische app werkstroomstappen en hoe de werkstroom moet worden uitgevoerd door de Logic Apps-engine.
U kunt deze definitie weergeven in uw logische app **codeweergave** venster.
In de logische app-resource, vindt u deze definitie in de `definition` eigenschap.
* **Verbindingen**: verwijst naar een afzonderlijke resources die veilig opslaan van metagegevens over elke connector-verbindingen, zoals een verbindingsreeks en een toegangstoken.
In de logische app-resource, uw logische app verwijst naar deze resources in de `parameters` sectie.

U kunt al deze onderdelen van logische apps weergeven met behulp van een hulpprogramma zoals [Azure Resource Explorer](http://resources.azure.com).

Als u een sjabloon voor een logische app voor gebruik met brongroepimplementaties, moet u de resources definiëren en parameteriseren indien nodig.
Bijvoorbeeld, als u een ontwikkeling, testen en productie-omgeving implementeert, wilt u waarschijnlijk gebruiken verschillende verbindingsreeksen naar een SQL-database in elke omgeving.
Of u wilt implementeren in verschillende abonnementen of resourcegroepen.  

## <a name="create-a-logic-app-deployment-template"></a>Maken van een sjabloon voor logische app-implementatie

De eenvoudigste manier om een geldige logic app-implementatiesjabloon is met de [Visual Studio-hulpprogramma's voor logische Apps](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md#prerequisites).
De Visual Studio-hulpprogramma's genereren een geldige implementatiesjabloon die kan worden gebruikt in een abonnement of andere locatie.

Enkele andere hulpprogramma's kunnen u helpen bij het maken van een sjabloon voor logische app-implementatie.
U kunt maken met de hand, dat wil zeggen, met behulp van de resources die al hier besproken voor het maken van parameters naar behoefte.
Een andere optie is om te gebruiken een [logic app-sjabloon ontwikkelaar](https://github.com/jeffhollan/LogicAppTemplateCreator) PowerShell-module. Deze open-source-module wordt eerst geëvalueerd de logische app en alle verbindingen dat deze wordt gebruikt, en vervolgens sjabloonresources met de vereiste parameters voor de implementatie genereert.
Bijvoorbeeld, hebt u een logische app die u ontvangt een bericht van een Azure Service Bus-wachtrij en voegt gegevens toe aan een Azure SQL database, het hulpprogramma behoudt alle de orchestration-logica en parameterizes de tekenreeksen implementeren zodat ze kunnen worden ingesteld op SQL- en Service Bus-verbinding tekstuitlijning.

> [!NOTE]
> Verbindingen moeten zich binnen dezelfde resourcegroep bevinden als de logische app.
>
>

### <a name="install-the-logic-app-template-powershell-module"></a>De logische app sjabloon PowerShell-module installeren
De eenvoudigste manier om de module te installeren is via de [PowerShell Gallery](https://www.powershellgallery.com/packages/LogicAppTemplate/0.1), met behulp van de opdracht `Install-Module -Name LogicAppTemplate`.  

Ook kunt u de PowerShell-module handmatig:

1. Download de nieuwste release van de [logic app-sjabloon ontwikkelaar](https://github.com/jeffhollan/LogicAppTemplateCreator/releases).  
2. Pak de map van uw PowerShell-module (meestal `%UserProfile%\Documents\WindowsPowerShell\Modules`).

Voor de module om te werken met elke tenant en hetzelfde abonnement access token, raden wij aan dat u met de [ARMClient](https://github.com/projectkudu/ARMClient) opdrachtregel-hulpprogramma.  Dit [blogbericht](http://blog.davidebbo.com/2015/01/azure-resource-manager-client.html) ARMClient vindt u meer details.

### <a name="generate-a-logic-app-template-by-using-powershell"></a>Een sjabloon voor logische app genereren met behulp van PowerShell
Nadat u PowerShell hebt geïnstalleerd, kunt u een sjabloon kunt genereren met behulp van de volgende opdracht uit:

`armclient token $SubscriptionId | Get-LogicAppTemplate -LogicApp MyApp -ResourceGroup MyRG -SubscriptionId $SubscriptionId -Verbose | Out-File C:\template.json`

`$SubscriptionId` is de Azure-abonnement-ID. Deze regel voor de eerste keer een token via ARMClient, en geeft deze door naar de PowerShell-script en maakt vervolgens de sjabloon in een JSON-bestand.

## <a name="add-parameters-to-a-logic-app-template"></a>Voeg parameters toe aan een sjabloon voor logische app
Nadat u de sjabloon voor uw logische app gemaakt, kunt u blijven toevoegen of wijzigen van parameters die u mogelijk nodig hebt. Bijvoorbeeld, als uw definitie een resource-ID voor een Azure-functie of geneste werkstroom die u wilt implementeren in een enkele implementatie bevat, kunt u meer resources toevoegen aan uw sjabloon en id's parameteriseren indien nodig. Hetzelfde geldt voor alle verwijzingen naar aangepaste API's of Swagger eindpunten die u verwacht te implementeren met elke resourcegroep.

### <a name="add-references-for-dependent-resources-to-visual-studio-deployment-templates"></a>Naslaginformatie voor afhankelijke resources toevoegen aan Visual Studio-implementatiesjablonen

Als u wilt dat uw logische app om te verwijzen naar afhankelijke resources, kunt u [Azure Resource Manager-sjabloonfuncties](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-functions) in uw sjabloon voor logische app-implementatie. Bijvoorbeeld, kunt u uw logische app om te verwijzen naar een Azure-functie of integratie-account dat u naast uw logische app wilt implementeren. Volg deze richtlijnen over het gebruik van parameters in uw sjabloon voor de implementatie, zodat Logic App Designer correct wordt weergegeven. 

U kunt parameters van de logische app in dit soort triggers en acties gebruiken:

*   Onderliggende werkstroom
*   Functie-app
*   APIM-aanroep
*   Runtime-URL van API-verbinding
*   Pad naar API-verbinding

En u kunt functies van sjablonen, zoals parameters, variabelen, resourceId, samenvoegen, enzovoort. Dit is hoe u de resource-ID van het Azure-functie kunt vervangen:

```
"parameters":{
    "functionName": {
        "type":"string",
        "minLength":1,
        "defaultValue":"<FunctionName>"
    }
},
```

En wanneer u parameters wilt gebruiken:

```
"MyFunction": {
    "type": "Function",
    "inputs": {
        "body":{},
        "function":{
            "id":"[resourceid('Microsoft.Web/sites/functions','functionApp',parameters('functionName'))]"
        }
    },
    "runAfter":{}
}
```
Als een ander voorbeeld kunt u de bewerking voor het Service Bus verzenden bericht voorzien:

```
"Send_message": {
    "type": "ApiConnection",
        "inputs": {
            "host": {
                "connection": {
                    "name": "@parameters('$connections')['servicebus']['connectionId']"
                }
            },
            "method": "post",
            "path": "[concat('/@{encodeURIComponent(''', parameters('queueuname'), ''')}/messages')]",
            "body": {
                "ContentData": "@{base64(triggerBody())}"
            },
            "queries": {
                "systemProperties": "None"
            }
        },
        "runAfter": {}
    }
```
> [!NOTE] 
> host.runtimeUrl is optioneel en kan worden verwijderd uit de sjabloon, indien aanwezig.
> 


> [!NOTE] 
> Voor de Logic App Designer om te werken wanneer u parameters, moet u standaardwaarden, bijvoorbeeld opgeven:
> 
> ```
> "parameters": {
>     "IntegrationAccount": {
>     "type":"string",
>     "minLength":1,
>     "defaultValue":"/subscriptions/<subscriptionID>/resourceGroups/<resourceGroupName>/providers/Microsoft.Logic/integrationAccounts/<integrationAccountName>"
>     }
> },
> ```

## <a name="add-your-logic-app-to-an-existing-resource-group-project"></a>Uw logische app toevoegen aan een bestaande resourcegroep-project

Als u een bestaande resourcegroep-project hebt, kunt u uw logische app toevoegen aan het project in het venster JSON Outline. U kunt ook een andere logische app naast de app die u eerder hebt gemaakt toevoegen.

1. Open het `<template>.json`-bestand.

2. Als u wilt openen in het venster JSON Outline, gaat u naar **weergave** > **andere Windows** > **JSON-overzicht**.

3. Als u wilt een bron naar het sjabloonbestand toevoegen, klikt u op **Resource toevoegen** aan de bovenkant van het venster JSON Outline. Of met de rechtermuisknop in het venster JSON Outline **resources**, en selecteer **nieuwe Resource toevoegen**.

    ![Venster JSON-overzicht](./media/logic-apps-create-deploy-template/jsonoutline.png)
    
4. In de **Resource toevoegen** in het dialoogvenster, zoeken en selecteer **logische App**. De naam van uw logische app, en kies **toevoegen**.

    ![Resource toevoegen](./media/logic-apps-create-deploy-template/addresource.png)


## <a name="deploy-a-logic-app-template"></a>Een sjabloon voor logische app implementeren

U kunt uw sjabloon implementeren met behulp van een programma's zoals PowerShell, REST-API, [Release Management voor Azure DevOps](#team-services), en de sjabloonimplementatie via Azure portal.
Ook voor het opslaan van de waarden voor parameters, wordt aangeraden dat u maken een [parameterbestand](../azure-resource-manager/resource-group-template-deploy.md#parameter-files).
Meer informatie over het [resources implementeren met Azure Resource Manager-sjablonen en PowerShell](../azure-resource-manager/resource-group-template-deploy.md) of [resources implementeren met Azure Resource Manager-sjablonen en Azure portal](../azure-resource-manager/resource-group-template-deploy-portal.md).

### <a name="authorize-oauth-connections"></a>OAuth-verbindingen toestaan

Na de implementatie werkt de logische app end-to-end met geldige parameters op.
U moet echter nog steeds OAuth-verbindingen voor het genereren van een geldige toegangstoken autoriseren.
Als u wilt machtigen OAuth-verbindingen, opent u de logische app in de ontwerper van logische Apps en deze verbindingen toestaan. Of voor automatische implementatie, kunt u een script gebruiken om in te stemmen op elke OAuth-verbinding.
Er is een voorbeeldscript op GitHub onder de [LogicAppConnectionAuth](https://github.com/logicappsio/LogicAppConnectionAuth) project.

<a name="team-services"></a>
## <a name="azure-devops-release-management"></a>Azure DevOps-Release Management

Een veelvoorkomend scenario voor het implementeren en beheren van een omgeving is het gebruik van een hulpprogramma zoals Release Management in Azure DevOps, met een sjabloon voor logische app-implementatie. Azure DevOps bevat een [Azure-resourcegroep implementeren](https://github.com/Microsoft/vsts-tasks/tree/master/Tasks/DeployAzureResourceGroup) taak die u kunt aan een build toevoegen of release-pijplijn. U moet beschikken over een [service-principal](https://blogs.msdn.microsoft.com/visualstudioalm/2015/10/04/automating-azure-resource-group-deployment-using-a-service-principal-in-visual-studio-online-buildrelease-management/) voor autorisatie om te implementeren, en u vervolgens de release-pijplijn kunt genereren.

1. Selecteer in de Release Management, **leeg** zodat u een lege pijplijn maken.

    ![Lege pijplijn maken][1]

2. Kies alle resources die u nodig hebt voor dit waarschijnlijk met inbegrip van de sjabloon voor logische app die wordt gegenereerd handmatig of als onderdeel van het bouwproces.
3. Voeg een **Azure Resource Group Deployment** taak.
4. Configureren met een [service-principal](https://blogs.msdn.microsoft.com/visualstudioalm/2015/10/04/automating-azure-resource-group-deployment-using-a-service-principal-in-visual-studio-online-buildrelease-management/), en verwijzen naar de sjabloon en Parameters van de sjabloon-bestanden.
5. Doorgaan met het bouwen van de stappen in het uitgifteproces voor omgeving, geautomatiseerde test of goedkeurders indien nodig.

<!-- Image References -->
[1]: ./media/logic-apps-create-deploy-template/emptyreleasedefinition.png
