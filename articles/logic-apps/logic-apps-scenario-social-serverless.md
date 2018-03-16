---
title: 'Zonder server scenario: een klant insights dashboard maken met Azure | Microsoft Docs'
description: Meer informatie over hoe u feedback van klanten, sociale mediagegevens en meer door het bouwen van een klant-dashboard met Azure Logic Apps en Azure Functions kunt beheren
keywords: 
services: logic-apps
author: jeffhollan
manager: SyntaxC4
editor: 
documentationcenter: 
ms.assetid: d565873c-6b1b-4057-9250-cf81a96180ae
ms.service: logic-apps
ms.workload: logic-apps
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/15/2018
ms.author: jehollan; LADocs
ms.openlocfilehash: 0a31a71305a4729575c5266b3a6138004d2dbdc6
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/16/2018
---
# <a name="create-a-streaming-customer-insights-dashboard-with-azure-logic-apps-and-azure-functions"></a>Een streaming klant insights dashboard maken met Azure Logic Apps en Azure Functions

Azure biedt zonder Server hulpmiddelen waarmee u snel apps bouwen en host in de cloud, zonder dat bij het nadenken over de infrastructuur. In deze zelfstudie maakt kunt u een dashboard dat activeert op feedback van klanten, analyseert feedback met machine learning en inzichten publiceert naar een bron, zoals Power BI of Azure Data Lake maken.

Voor deze oplossing u deze sleutel Azure onderdelen voor apps zonder server gebruiken: [Azure Functions](https://azure.microsoft.com/services/functions/) en [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/).
Logische Apps van Azure biedt een zonder server werkstroomengine in de cloud zodat u kunt integraties voor zonder Server-onderdelen maken en verbinding met 200 + services en API's maken. Azure Functions bevat zonder server computergebruik in de cloud. Deze oplossing gebruikt Azure Functions voor klant tweets op basis van vooraf gedefinieerde trefwoorden markeert.

In dit scenario maakt u een logische app, die wordt geactiveerd voor het zoeken van feedback van klanten. Sommige verbindingslijnen dat u reageren op feedback van klanten help Outlook.com, Office 365, enquête Monkey, Twitter, bevatten en een [HTTP-aanvraag van een webformulier](https://blogs.msdn.microsoft.com/logicapps/2017/01/30/calling-a-logic-app-from-an-html-form/). De werkstroom die u maakt bewaakt een hashtag op Twitter.

U kunt [de hele oplossing in Visual Studio](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md) en [implementeren van de oplossing met Azure Resource Manager-sjabloon](../logic-apps/logic-apps-create-deploy-template.md). Voor een video-overzicht waarin wordt uitgelegd hoe u deze oplossing maken [Bekijk deze video van Channel 9](http://aka.ms/logicappsdemo). 

## <a name="trigger-on-customer-data"></a>Activeren van klantgegevens

1. In de Azure-portal of Visual Studio maakt een lege logische app. 

   Als u geen ervaring met logische apps, raadpleegt u de [Quick Start voor de Azure portal](../logic-apps/quickstart-create-first-logic-app-workflow.md) of de [Quick Start voor Visual Studio](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md).

2. In Logic App-ontwerper, zoeken en toevoegen van de Twitter-trigger met deze actie: **wanneer een nieuwe tweet wordt gepost**

3. De trigger instellen om te luisteren naar tweets op basis van een sleutelwoord of hashtag.

   Op een polling-triggers, zoals de trigger Twitter bepaalt de eigenschap terugkeerpatroon hoe vaak de logische app controleert op nieuwe items.

   ![Voorbeeld van een trigger Twitter][1]

Deze logische app is nu wordt geactiveerd op alle nieuwe tweets. U kunt nemen en de tweet gegevens analyseren zodat u beter in de patronen die zijn uitgedrukt inzicht. 

## <a name="analyze-tweet-text"></a>Analyseren tweet tekst

U kunt gebruiken om te detecteren de gevoel achter tekst, [cognitieve Azure-Services](https://azure.microsoft.com/services/cognitive-services/).

1. In Logic App Designer onder de trigger kiezen **nieuwe stap**.

2. Zoek de **Tekstanalyse** connector.

3. Selecteer de **detecteren gevoel** in te grijpen.

4. Als u wordt gevraagd, Geef een geldige sleutel cognitieve Services voor de service Tekstanalyse.

5. Onder **aanvraagtekst**, selecteer de **Tweet tekst** veld, wat zorgt voor de tekst tweet als invoer voor analyse.

Nadat u de tweet gegevens en inzicht in de tweet ontvangen, kunt u nu verschillende relevante connectors en hun bewerkingen gebruiken:

* **Power BI - rijen toevoegen aan de gegevensset Streaming**: binnenkomende tweets op een Power BI-dashboard weergeven.
* **Azure Data Lake - toevoegen van**: klantgegevens toevoegen aan een Azure Data Lake-gegevensset in analytics-taken op te nemen.
* **SQL - rijen toevoegen**: opslaan van gegevens in een database voor later gebruik.
* **Vertraging - bericht verzenden**: melding van een ongebruikt kanaal over negatieve feedback die wellicht actie is vereist.

U kunt ook maken en een Azure functioneren zodat u kunt aangepaste verwerking uitvoeren op uw gegevens. 

## <a name="process-data-with-azure-functions"></a>Procesgegevens met Azure Functions

Voordat u een functie maakt, moet u een functie-app maken in uw Azure-abonnement. Ook voor uw logische app rechtstreeks een functie aanroept, de functie moet beschikken over een HTTP-binding, bijvoorbeeld activeren, gebruikt u de **HttpTrigger** sjabloon. Meer informatie over [het maken van uw eerste functie-app en de functie in de Azure portal](../azure-functions/functions-create-first-azure-function-azure-portal.md).

Voor dit scenario gebruikt u de tekst tweet als aanvraagtekst voor uw Azure-functie. Definieer de logica die bepaalt of de tekst tweet een sleutelwoord of woordgroep bevat in uw functiecode. Houd de functie als eenvoudige of complexe die nodig zijn voor het scenario.
Aan het einde van de functie, bijvoorbeeld een antwoord naar de logische app met enkele gegevens retourneren, een eenvoudige Booleaanse waarde, zoals `containsKeyword` of een complex object.

> [!TIP]
> Voor toegang tot een complexe antwoord van een functie in een logische app, gebruikt de **parseren JSON** in te grijpen.

Wanneer u bent klaar, de functie opslaan en vervolgens de functie toevoegen als een actie in de logische app die u maakt.

## <a name="add-azure-function-to-logic-app"></a>Azure-functie toevoegen aan logische app

1. In Logic App Designer onder de **detecteren gevoel** actie, kies **nieuwe stap**.

2. Zoek de **Azure Functions** connector en selecteer vervolgens de functie die u hebt gemaakt.

3. Onder **aanvraagtekst**, selecteer **Tweet tekst**.

![Geconfigureerde Azure-functie stap][2]

## <a name="run-and-monitor-your-logic-app"></a>Uitvoeren en controleren van uw logische app

Als u wilt weergeven van alle huidige of de vorige wordt uitgevoerd voor uw logische app, kunt u de uitgebreide foutopsporing en bewaking van de mogelijkheden die Azure Logic Apps kunt u in de Azure portal, Visual Studio, of via de Azure REST API's en SDK's.

Test uw logische app eenvoudig in Logic App-ontwerper, kies **uitvoeren Trigger**. De trigger worden opgevraagd op tweets op basis van de opgegeven planning tot een tweet die voldoet aan uw criteria is gevonden. Tijdens de voortgang van de uitvoering toont de designer een actuele weergave voor deze uitvoeren.

Voer de geschiedenis in Visual Studio of de Azure-portal naar de vorige weergave: 

* Open Visual Studio Cloud Explorer. Uw logische app vinden, opent u het snelmenu van de app. Selecteer **Open uitvoeringsgeschiedenis**.

* Zoek uw logische app in de Azure-portal. Kies uw logische app menu **overzicht**. 

## <a name="create-automated-deployment-templates"></a>Sjablonen voor automatische implementatie maken

Nadat u een oplossing voor logic app hebt gemaakt, kunt u vastleggen en implementeren van uw app als een [Azure Resource Manager-sjabloon](../azure-resource-manager/resource-group-overview.md#template-deployment) alle Azure-regio in de hele wereld. U kunt deze mogelijkheid zowel parameters voor het maken van verschillende versies van uw app en voor het integreren van uw oplossing in een build wijzigen en de vrijgave van de pijplijn. U kunt ook Azure Functions opnemen in uw implementatiesjabloon, zodat u de volledige oplossing met alle afhankelijkheden als één sjabloon kunt beheren. Meer informatie over [logic app-implementatiesjablonen maken](../logic-apps/logic-apps-create-deploy-template.md).

Controleer voor een voorbeeld-implementatiesjabloon met een Azure-functie, de [Azure quickstart sjabloon opslagplaats](https://github.com/Azure/azure-quickstart-templates/tree/master/101-function-app-create-dynamic).

## <a name="next-steps"></a>Volgende stappen

* [Andere voorbeelden en scenario's voor Azure Logic Apps zoeken](logic-apps-examples-and-scenarios.md)

<!-- Image References -->
[1]: ./media/logic-apps-scenario-social-serverless/twitter.png
[2]: ./media/logic-apps-scenario-social-serverless/function.png