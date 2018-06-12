---
title: Implementatiesjablonen maken voor Azure Logic Apps | Microsoft Docs
description: Azure Resource Manager-sjablonen voor het implementeren van logische apps maken
services: logic-apps
documentationcenter: .net,nodejs,java
author: ecfan
manager: jeconnoc
editor: ''
ms.assetid: 85928ec6-d7cb-488e-926e-2e5db89508ee
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.custom: H1Hack27Feb2017
ms.date: 10/18/2016
ms.author: LADocs; estfan
ms.openlocfilehash: 647ffeb05542e12d19cefa3fa0dbf55e5585109a
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35297910"
---
# <a name="create-azure-resource-manager-templates-for-deploying-logic-apps"></a>Azure Resource Manager-sjablonen voor het implementeren van logische apps maken

Nadat een logische app is gemaakt, is het raadzaam deze als een Azure Resource Manager-sjabloon te maken.
Op deze manier u kunt logische Apps eenvoudig implementeren met elke omgeving of de resourcegroep waar u deze moet mogelijk.
Zie voor meer informatie over de Resource Manager-sjablonen, [Azure Resource Manager-sjablonen ontwerpen](../azure-resource-manager/resource-group-authoring-templates.md) en [resources implementeren met behulp van Azure Resource Manager-sjablonen](../azure-resource-manager/resource-group-template-deploy.md).

## <a name="logic-app-deployment-template"></a>Sjabloon Logic app-implementatie

Een logische app bevat drie basiscomponenten:

* **Resource-Logic app**: bevat informatie over zaken als prijzen plan, locatie en de werkstroomdefinitie.
* **De workflowdefinitie**: Beschrijving van uw logische app werkstroomstappen en hoe de werkstroom moet worden uitgevoerd door de engine voor Logic Apps.
U kunt deze definitie weergeven in uw logische app **codeweergave** venster.
In de logic app-bron, vindt u deze definitie in de `definition` eigenschap.
* **Verbindingen**: verwijst naar een afzonderlijke resources die veilig opslaan metagegevens over de connector-verbindingen, zoals een verbindingsreeks en een toegangstoken.
In de bron van de app logica uw logische app verwijst naar deze bronnen in de `parameters` sectie.

U kunt deze informatie van bestaande logic apps weergeven met behulp van een hulpprogramma zoals [Azure Resource Explorer](http://resources.azure.com).

Als u een sjabloon voor een logische app gebruiken met resourcegroepimplementaties, moet u de resources bepalen en naar behoefte te voorzien.
Bijvoorbeeld, als u een ontwikkeling, testen en productie-omgeving implementeert, wilt u waarschijnlijk gebruiken verschillende verbindingsreeksen naar een SQL-database in elke omgeving.
Of u mogelijk wilt implementeren in verschillende abonnementen of resourcegroepen.  

## <a name="create-a-logic-app-deployment-template"></a>Maken van een sjabloon logic app-implementatie

De eenvoudigste manier om een geldige logic app-implementatiesjabloon is met de [Visual Studio Tools voor Logic Apps](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md#prerequisites).
De Visual Studio tools genereren een geldige implementatie-sjabloon die via een abonnement of een locatie kan worden gebruikt.

Enkele andere hulpprogramma's kunnen u helpen bij het maken van een sjabloon logic app-implementatie.
U kunt schrijven handmatig, dat wil zeggen, met behulp van de resources die al die hier worden besproken voor het maken van parameters, indien nodig.
Een andere optie is met een [logic app sjabloon Maker](https://github.com/jeffhollan/LogicAppTemplateCreator) PowerShell-module. Deze open source-module evalueert eerst de logische app en dat het wordt gebruikt en vervolgens sjabloonresources met de benodigde parameters op voor de implementatie van genereert verbindingen.
Bijvoorbeeld als er een logische app die een bericht ontvangt van een Azure Service Bus-wachtrij en gegevens toegevoegd aan een Azure SQL database, het hulpprogramma behoudt alle de orchestration-logica en de SQL- en Service Bus-verbindingsreeksen parameterizes zodat ze kunnen worden ingesteld op implementatie.

> [!NOTE]
> Verbindingen moeten in dezelfde resourcegroep bevinden als de logische app.
>
>

### <a name="install-the-logic-app-template-powershell-module"></a>De logische app sjabloon PowerShell-module installeren
De eenvoudigste manier voor het installeren van de module is via de [PowerShell Gallery](https://www.powershellgallery.com/packages/LogicAppTemplate/0.1), met de opdracht `Install-Module -Name LogicAppTemplate`.  

Ook kunt u de PowerShell-module handmatig installeren:

1. Download de nieuwste versie van de [logic app sjabloon Maker](https://github.com/jeffhollan/LogicAppTemplateCreator/releases).  
2. Pak de map van uw PowerShell-module (meestal `%UserProfile%\Documents\WindowsPowerShell\Modules`).

Voor de module voor gebruik met elke tenant en -abonnement toegang token, het is raadzaam dat u deze met gebruikt de [ARMClient](https://github.com/projectkudu/ARMClient) opdrachtregelprogramma.  Dit [blogbericht](http://blog.davidebbo.com/2015/01/azure-resource-manager-client.html) ARMClient in meer detail besproken.

### <a name="generate-a-logic-app-template-by-using-powershell"></a>Genereren van een sjabloon voor logische Apps met behulp van PowerShell
Nadat u PowerShell is geïnstalleerd, kunt u een sjabloon genereren met behulp van de volgende opdracht:

`armclient token $SubscriptionId | Get-LogicAppTemplate -LogicApp MyApp -ResourceGroup MyRG -SubscriptionId $SubscriptionId -Verbose | Out-File C:\template.json`

`$SubscriptionId` is de Azure-abonnement-ID. Deze regel voor de eerste keer een access token via ARMClient, en vervolgens deze via doorgesluisd naar de PowerShell-script en maakt vervolgens de sjabloon in een JSON-bestand.

## <a name="add-parameters-to-a-logic-app-template"></a>Voeg parameters toe aan een sjabloon voor logic Apps
Nadat u uw logische app-sjabloon maakt, kunt u toevoegen of wijzigen van de parameters die u moet doorgaan. Bijvoorbeeld, als de definitie van de bevat een resource-ID moet een Azure functie of geneste werkstroom die u wilt implementeren in een implementatie met één, kunt u meer resources toevoegen aan uw sjabloon en voorzien van id's, indien nodig. Hetzelfde geldt voor alle verwijzingen naar aangepaste API's of Swagger eindpunten die u verwacht te implementeren met elke resourcegroep.

### <a name="add-references-for-dependent-resources-to-visual-studio-deployment-templates"></a>Verwijzingen voor afhankelijke resources toevoegen aan Visual Studio-implementatiesjablonen

Als u wilt dat uw logische app om te verwijzen naar afhankelijke resources, kunt u [Azure Resource Manager-sjabloonfuncties](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-functions) in uw sjabloon logic app-implementatie. Bijvoorbeeld, kunt u uw logische app om te verwijzen naar een Azure-functie of integratiepakket account die u samen met uw logische app wilt implementeren. Volg deze richtlijnen over het gebruik van parameters in de implementatiesjabloon voor, zodat de Logic App Designer correct wordt gerenderd. 

U kunt logische app parameters in deze soorten triggers en acties:

*   Onderliggende werkstroom
*   Function App
*   APIM aanroep
*   De URL van de runtime verbinding API
*   Verbindingspad API

En u kunt een sjabloonfuncties, zoals parameters, variabelen, resourceId, concat, enzovoort. Dit is hoe u de resource-ID van het Azure-functie kunt vervangen:

```
"parameters":{
    "functionName": {
        "type":"string",
        "minLength":1,
        "defaultValue":"<FunctionName>"
    }
},
```

En waar u parameters wilt gebruiken:

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
Als een ander voorbeeld kunt u de Service Bus-verzendbewerking bericht voorzien:

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
> Voor Logic App Designer werken wanneer u parameters gebruiken, moet u standaardwaarden, bijvoorbeeld opgeven:
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

Als u een bestaand project voor de resourcegroep hebt, kunt u uw logische app toevoegen aan het project in het venster JSON-overzicht. U kunt ook een andere logische app naast de app die u eerder hebt gemaakt toevoegen.

1. Open het `<template>.json`-bestand.

2. De JSON-overzicht om venster te openen, gaat u naar **weergave** > **overige vensters** > **JSON-overzicht**.

3. Als u wilt een resource toevoegen aan het sjabloonbestand, klikt u op **Resource toevoegen** aan de bovenkant van het venster JSON-overzicht. Of in het venster JSON-overzicht met de rechtermuisknop op **resources**, en selecteer **nieuwe Resource toevoegen**.

    ![Venster JSON-overzicht](./media/logic-apps-create-deploy-template/jsonoutline.png)
    
4. In de **Resource toevoegen** in het dialoogvenster, zoeken en selecteert u **logische App**. Naam van uw logische app en kies **toevoegen**.

    ![Resource toevoegen](./media/logic-apps-create-deploy-template/addresource.png)


## <a name="deploy-a-logic-app-template"></a>Een logische app-sjabloon implementeren

U kunt uw sjabloon implementeren met behulp van hulpprogramma's zoals PowerShell, REST-API [Visual Studio Team Services Release Management](#team-services), en sjabloonimplementatie via Azure portal.
Ook voor het opslaan van de waarden voor parameters, wordt aangeraden dat u maakt een [parameterbestand](../azure-resource-manager/resource-group-template-deploy.md#parameter-files).
Meer informatie over hoe [implementeren van resources met Azure Resource Manager-sjablonen en PowerShell](../azure-resource-manager/resource-group-template-deploy.md) of [implementeren van resources met Azure Resource Manager-sjablonen en de Azure portal](../azure-resource-manager/resource-group-template-deploy-portal.md).

### <a name="authorize-oauth-connections"></a>OAuth-verbindingen toestaan

Na de implementatie werkt de logische app end-to-end met geldige parameters op.
U moet echter nog steeds OAuth-verbindingen voor het genereren van een geldige toegangstoken autoriseren.
Voor het autoriseren van verbindingen OAuth, opent u de logische app in de ontwerpfunctie voor Logic Apps en autoriseren van deze verbindingen. Of voor automatische implementatie, kunt u een script toe te staan elke OAuth-verbinding.
Er is een voorbeeldscript op GitHub onder de [LogicAppConnectionAuth](https://github.com/logicappsio/LogicAppConnectionAuth) project.

<a name="team-services"></a>
## <a name="visual-studio-team-services-release-management"></a>Visual Studio Team Services Release Management

Een veelvoorkomend scenario voor het implementeren en beheren van een omgeving is het gebruik van een hulpprogramma zoals Release Management in Visual Studio Team Services, met een sjabloon logic app-implementatie. Visual Studio Team Services bevat een [implementeren Azure Resource Group](https://github.com/Microsoft/vsts-tasks/tree/master/Tasks/DeployAzureResourceGroup) taak toevoegen aan een build of vrijgeven van de pijplijn. Moet u beschikken over een [service-principal](https://blogs.msdn.microsoft.com/visualstudioalm/2015/10/04/automating-azure-resource-group-deployment-using-a-service-principal-in-visual-studio-online-buildrelease-management/) voor autorisatie te implementeren, waarna u de definitie van de release kan genereren.

1. Selecteer in de Release Management **leeg** zodat u de definitie van een leeg maken.

    ![Lege definitie maken][1]

2. Kies alle resources die u nodig voor deze hebt, inclusief waarschijnlijk de logic app-sjabloon die handmatig of als onderdeel van de buildproces wordt gegenereerd.
3. Voeg een **Azure Resource Group Deployment** taak.
4. Configureren met een [service-principal](https://blogs.msdn.microsoft.com/visualstudioalm/2015/10/04/automating-azure-resource-group-deployment-using-a-service-principal-in-visual-studio-online-buildrelease-management/), en verwijzen naar de sjabloon en sjabloonparameters-bestanden.
5. Doorgaan met het bouwen van de stappen in de release-proces voor de omgeving, geautomatiseerde test of goedkeurders zo nodig.

<!-- Image References -->
[1]: ./media/logic-apps-create-deploy-template/emptyreleasedefinition.png
