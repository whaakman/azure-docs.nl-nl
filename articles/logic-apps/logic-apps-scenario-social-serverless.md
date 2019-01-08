---
title: Serverloze scenario - customer insights dashboard maken met de Azure-services | Microsoft Docs
description: Beheren van feedback van klanten, sociale media-gegevens en meer met het bouwen van een klant-dashboard met Azure Logic Apps en Azure Functions
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: jeffhollan
ms.author: jehollan
ms.reviewer: estfan, LADocs
ms.assetid: d565873c-6b1b-4057-9250-cf81a96180ae
ms.topic: article
ms.date: 03/15/2018
ms.openlocfilehash: 638b29dd2a15d0467c41e20ecfed9f333b34c04d
ms.sourcegitcommit: 3ab534773c4decd755c1e433b89a15f7634e088a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/07/2019
ms.locfileid: "54061054"
---
# <a name="create-streaming-customer-insights-dashboard-with-azure-logic-apps-and-azure-functions"></a>Streaming customer insights-dashboard maken met Azure Logic Apps en Azure Functions

Azure biedt [serverloze](https://azure.microsoft.com/solutions/serverless/) hulpprogramma's waarmee u snel apps bouwen en hosten in de cloud, zonder dat om na te denken over de infrastructuur. In deze zelfstudie, kunt u een dashboard dat wordt geactiveerd op feedback van klanten, analyseert feedback met machine learning en inzichten publiceert naar een bron, zoals Power BI of Azure Data Lake.

Voor deze oplossing kunt u deze belangrijke Azure-onderdelen voor serverloze apps gebruiken: [Azure Functions](https://azure.microsoft.com/services/functions/) en [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/).
Met Azure Logic Apps biedt een engine voor serverloze werkstromen in de cloud, zodat u kunt maken van indelingen voor serverloze onderdelen en verbinding met meer dan 200 services en API's maken. Azure Functions biedt serverloze computing in de cloud. Deze oplossing maakt gebruik van Azure Functions voor het markeren van de klant tweets op basis van vooraf gedefinieerde trefwoorden.

In dit scenario maakt u een logische app die wordt geactiveerd op het vinden van feedback van klanten. Sommige connectors dat help u reageren op feedback van klanten zijn onder andere Outlook.com, Office 365, enquête Monkey, Twitter, en een [HTTP-aanvraag van een webformulier](https://blogs.msdn.microsoft.com/logicapps/2017/01/30/calling-a-logic-app-from-an-html-form/). De werkstroom die u maakt bewaakt een hashtag op Twitter.

U kunt [bouwen van de gehele oplossing in Visual Studio](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md) en [implementeren van de oplossing met Azure Resource Manager-sjabloon](../logic-apps/logic-apps-create-deploy-template.md). Voor een video-overzicht waarin wordt getoond hoe u deze oplossing maakt [Bekijk deze video voor Channel 9](https://aka.ms/logicappsdemo). 

## <a name="trigger-on-customer-data"></a>Activeren op gegevens van de klant

1. In de Azure portal of Visual Studio, een lege logische app te maken. 

   Als u geen ervaring met logische apps, raadpleegt u de [Quick Start voor de Azure-portal](../logic-apps/quickstart-create-first-logic-app-workflow.md) of de [Quick Start voor Visual Studio](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md).

2. Zoek in Logic App Designer en de Twitter-trigger met deze actie toevoegen: **Wanneer er een nieuwe tweet wordt geplaatst**

3. De trigger instellen om te luisteren naar tweets op basis van een sleutelwoord of hashtag.

   Op triggers op basis van een polling, zoals de Twitter-trigger, bepaalt de eigenschap recurrence hoe vaak de logische app nieuwe items controleert.

   ![Voorbeeld van de Twitter-trigger][1]

Deze logische app wordt nu geactiveerd op alle nieuwe tweets. Vervolgens kunt u ondernemen en analyseren van de tweet-gegevens, zodat u beter in de sentimenten uitgedrukt inzicht. 

## <a name="analyze-tweet-text"></a>Tekst van de tweet analyseren

U kunt gebruiken voor het detecteren van het sentiment achter tekst [Azure Cognitive Services](https://azure.microsoft.com/services/cognitive-services/).

1. In Logic App Designer, onder de trigger kiezen **nieuwe stap**.

2. Zoek de **Tekstanalyse** connector.

3. Selecteer de **gevoel detecteren** actie.

4. Als u hierom wordt gevraagd, geeft u een geldige Cognitive Services-sleutel voor de Text Analytics-service.

5. Onder **aanvraagtekst**, selecteer de **Tweettekst** veld, waarmee de tekst van de tweet als invoer voor analyse.

Nadat u de gegevens van de tweet en inzichten over de tweet ontvangen, kunt u nu verschillende andere relevante connectors en de bijbehorende acties gebruiken:

* **Power BI - rijen toevoegen aan de Streaminggegevensset**: Binnenkomende tweets in een Power BI-dashboard weergeven.
* **Azure Data Lake - toevoegen aan het bestand**: Gegevens van de klant toevoegen aan een Azure Data Lake-gegevensset om op te nemen in analytics-taken.
* **SQL - rijen toevoegen**: Gegevens in een database voor het ophalen van hoger Store.
* **Slack - bericht verzenden**: Een Slack-kanaal over negatieve feedback die wellicht actie is vereist op de hoogte stellen.

U kunt ook maken en een Azure-functie zodat u kunt aangepaste verwerking uitvoeren op uw gegevens. 

## <a name="process-data-with-azure-functions"></a>Gegevens verwerken met Azure Functions

Voordat u een functie maakt, moet u een functie-app maken in uw Azure-abonnement. Ook voor uw logische app voor het rechtstreeks aanroepen van een functie, de functie moet beschikken over een HTTP-binding, bijvoorbeeld activeren, gebruikt u de **HttpTrigger** sjabloon. Informatie over [over het maken van uw eerste functie-app en de functie in Azure portal](../azure-functions/functions-create-first-azure-function-azure-portal.md).

Voor dit scenario gebruikt u de tekst van de tweet als hoofdtekst van de aanvraag voor uw Azure-functie. In uw functiecode aan te geven, definieert u de logica waarmee wordt bepaald of de tekst van de tweet een sleutelwoord of woordgroep bevat. Houd de functie als eenvoudig of complex zijn als nodig is voor het scenario.
Aan het einde van de functie, een antwoord aan de logische app met gegevens, bijvoorbeeld, een eenvoudige Booleaanse waarde, zoals `containsKeyword` of een complex object.

> [!TIP]
> Voor toegang tot een complexe antwoord van een functie in een logische app, gebruikt de **JSON parseren** actie.

Wanneer u klaar bent, sla de functie en voeg vervolgens de functie als een actie in de logische app die u bouwt.

## <a name="add-azure-function-to-logic-app"></a>Azure-functie toevoegen aan de logische app

1. In Logic App Designer, onder de **gevoel detecteren** actie, kiest u **nieuwe stap**.

2. Zoek de **Azure Functions** connector en selecteer vervolgens de functie die u hebt gemaakt.

3. Onder **aanvraagtekst**, selecteer **Tweettekst**.

![Geconfigureerde Azure-functie stap][2]

## <a name="run-and-monitor-your-logic-app"></a>Uitvoeren en controleren van uw logische app

Als u wilt controleren elke huidige of de vorige uitvoeringen voor uw logische app, kunt u de uitgebreide foutopsporing en bewakingsmogelijkheden met Azure Logic Apps in Azure portal, Visual Studio, of via de Azure REST API's en SDK's.

Als u wilt testen eenvoudig uw logische app, in Logic App Designer, kies **Trigger uitvoeren**. De trigger peilt tweets op basis van de opgegeven planning totdat er een tweet die voldoet aan uw criteria wordt gevonden. Tijdens de voortgang van de run toont de ontwerpfunctie voor een live weergave voor die run.

Naar de vorige weergave uitvoeringsgeschiedenis in Visual Studio of de Azure-portal: 

* Open Visual Studio Cloud Explorer. Uw logische app zoeken, opent u het snelmenu van de app. Selecteer **Open uitvoeringsgeschiedenis**.

* In de Azure-portal zoeken uw logische app. Kies in het menu van uw logische app **overzicht**. 

## <a name="create-automated-deployment-templates"></a>Sjablonen voor automatische implementatie maken

Nadat u de oplossing van een logische app maakt, u kunt vastleggen en implementeren van uw app als een [Azure Resource Manager-sjabloon](../azure-resource-manager/resource-group-overview.md#template-deployment) aan andere Azure-regio's in de hele wereld. U kunt deze mogelijkheid om te wijzigen van parameters voor het maken van verschillende versies van uw app en voor het integreren van uw oplossing in Azure-pijplijnen. U kunt ook Azure Functions opnemen in uw sjabloon voor de implementatie, zodat u de gehele oplossing met alle afhankelijkheden als één enkele sjabloon kunt beheren. Informatie over [over het maken van logische app implementatiesjablonen](../logic-apps/logic-apps-create-deploy-template.md).

Voor een voorbeeld-implementatiesjabloon met een Azure-functie, Controleer de [opslagplaats voor Azure quickstart-sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/101-function-app-create-dynamic).

## <a name="next-steps"></a>Volgende stappen

* [Andere voorbeelden en scenario's voor Azure Logic Apps vinden](logic-apps-examples-and-scenarios.md)

<!-- Image References -->
[1]: ./media/logic-apps-scenario-social-serverless/twitter.png
[2]: ./media/logic-apps-scenario-social-serverless/function.png
