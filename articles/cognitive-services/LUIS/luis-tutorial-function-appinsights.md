---
title: Application Insights-gegevens van LUIS met behulp van Node.js
titleSuffix: Azure Cognitive Services
description: Bouw een bot die is geïntegreerd met een LUIS-toepassing en de Application Insights met behulp van Node.js.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/24/2018
ms.author: diberry
ms.openlocfilehash: 6199e4a681f7f58ea0cf57b575afb2a63d160eee
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/15/2018
ms.locfileid: "49321951"
---
# <a name="add-luis-results-to-application-insights"></a>LUIS resultaten naar Application Insights toevoegen
In deze zelfstudie voegt LUIS-aanvraag en respons informatie aan [Application Insights](https://azure.microsoft.com/services/application-insights/) telemetrie-gegevensopslag. Zodra u deze gegevens hebt, kunt u deze met de Kusto-taal of Power BI te analyseren, samenvoegen, opvragen en te rapporteren intenties en entiteiten van de utterance in realtime. Met deze analyse kunt u bepalen of u moet toevoegen of bewerken van de intenties en entiteiten van uw LUIS-app.

De bot is gebouwd met Bot Framework 3.x en de Azure-Web-app-bot.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
* Application Insights-bibliotheek toevoegen aan een web-app-bot
* Vastleggen en LUIS queryresultaten verzenden naar Application Insights
* Query uitvoeren op Application Insights voor het belangrijkste doel, score en utterance

## <a name="prerequisites"></a>Vereisten

* Uw LUIS web app-bot uit de **[vorige zelfstudie](luis-nodejs-tutorial-build-bot-framework-sample.md)** met Application Insights is ingeschakeld. 

> [!Tip]
> Als u nog geen een abonnement, kunt u zich registreren voor een [gratis account](https://azure.microsoft.com/free/).

Alle code in deze zelfstudie vindt u op de [LUIS-Samples github-opslagplaats](https://github.com/Microsoft/LUIS-Samples/tree/master/documentation-samples/tutorial-web-app-bot-application-insights/nodejs) en elke regel die is gekoppeld aan deze zelfstudie is opgenomen als met `//APPINSIGHT:`. 

## <a name="web-app-bot-with-luis"></a>Web app-bot gebruikmaken van LUIS
In deze zelfstudie wordt ervan uitgegaan dat u hebt code dat er als volgt of dat u hebt voltooid de [andere zelfstudie](luis-nodejs-tutorial-build-bot-framework-sample.md): 

   [!code-javascript[Web app bot with LUIS](~/samples-luis/documentation-samples/tutorial-web-app-bot/nodejs/app.js "Web app bot with LUIS")]

## <a name="add-application-insights-library-to-web-app-bot"></a>Application Insights-bibliotheek toevoegen aan web app-bot
Op dit moment verzamelt de Application Insights-service, die wordt gebruikt in deze web-app-bot, telemetrie van de algemene status voor de bot. Deze verzamelt geen LUIS aanvraag- en informatie die u nodig hebt om te controleren en herstellen van uw intenties en entiteiten. 

Om te kunnen leggen de LUIS-aanvraag en respons, de web-app-bot moet de **[Application Insights](https://www.npmjs.com/package/applicationinsights)** NPM-pakket geïnstalleerd en geconfigureerd in de **app.js** bestand. Vervolgens moeten de intentie dialoogvenster handlers de LUIS aanvraag en respons gegevens verzenden naar Application Insights. 

1. Selecteer in de Azure portal, in de web-app-bot-service **bouwen** onder de **Bot Management** sectie. 

    ![Zoeken naar app insights](./media/luis-tutorial-appinsights/build.png)

2. Er wordt een nieuw browsertabblad geopend met de App Service-Editor. Selecteer de naam van de app in de bovenste balk en selecteer vervolgens **Kudu-Console openen**. 

    ![Zoeken naar app insights](./media/luis-tutorial-appinsights/kudu-console.png)

3. Voer de volgende opdracht om Application Insights en het onderstrepingsteken-pakketten te installeren in de console:

    ```
    cd site\wwwroot && npm install applicationinsights && npm install underscore
    ```

    ![Zoeken naar app insights](./media/luis-tutorial-appinsights/npm-install.png)

    Wacht tot de pakketten te installeren:

    ```
    luisbot@1.0.0 D:\home\site\wwwroot
    `-- applicationinsights@1.0.1 
      +-- diagnostic-channel@0.2.0 
      +-- diagnostic-channel-publishers@0.2.1 
      `-- zone.js@0.7.6 
    
    npm WARN luisbot@1.0.0 No repository field.
    luisbot@1.0.0 D:\home\site\wwwroot
    +-- botbuilder-azure@3.0.4
    | `-- azure-storage@1.4.0
    |   `-- underscore@1.4.4 
    `-- underscore@1.8.3 
    ```

    U klaar bent met het browsertabblad voor kudu-console.

## <a name="capture-and-send-luis-query-results-to-application-insights"></a>Vastleggen en LUIS queryresultaten verzenden naar Application Insights
1. Open in het browsertabblad App Service-Editor de **app.js** bestand.

2. Voeg de volgende bibliotheken NPM onder de bestaande `requires` regels:

   [!code-javascript[Add NPM packages to app.js](~/samples-luis/documentation-samples/tutorial-web-app-bot-application-insights/nodejs/app.js?range=12-16 "Add NPM packages to app.js")]

3. De Application Insights-object maken en gebruiken van de toepassingsinstelling voor web-app-bot **BotDevInsightsKey**: 

   [!code-javascript[Create the Application Insights object](~/samples-luis/documentation-samples/tutorial-web-app-bot-application-insights/nodejs/app.js?range=68-80 "Create the Application Insights object")]

4. Voeg de **appInsightsLog** functie:

   [!code-javascript[Add the appInsightsLog function](~/samples-luis/documentation-samples/tutorial-web-app-bot-application-insights/nodejs/app.js?range=82-109 "Add the appInsightsLog function")]

    De laatste regel van de functie is waar de gegevens naar Application Insights is toegevoegd. De naam van de gebeurtenis is **LUIS-resultaten**, een unieke naam naast andere telemetriegegevens die zijn verzameld door deze web-app-bot. 

5. Gebruik de **appInsightsLog** functie. U toevoegen deze aan elke intentie dialoogvenster:

   [!code-javascript[Use the appInsightsLog function](~/samples-luis/documentation-samples/tutorial-web-app-bot-application-insights/nodejs/app.js?range=117-118 "Use the appInsightsLog function")]

6. U kunt uw web-app-bot testen met de **testen in Web Chat** functie. U kunt geen verschil moeten zien, omdat al het werk in Application Insights, niet in de antwoorden van een bot.

## <a name="view-luis-entries-in-application-insights"></a>LUIS-items weergeven in Application Insights
Open Application Insights om te zien van de LUIS-vermeldingen. 

1. Selecteer in de portal **alle resources** vervolgens filteren op de naam van de web-app-bot. Klik op de resource met het type **Application Insights**. Het pictogram voor Application Insights is een gloeilampje. 

    ![Zoeken naar app insights](./media/luis-tutorial-appinsights/search-for-app-insights.png)



2. Wanneer de resource wordt geopend, klikt u op de **zoeken** pictogram van het Vergrootglas in het deelvenster uiterst rechts. Een nieuw deelvenster met de juiste weergegeven. Afhankelijk van hoeveel telemetriegegevens wordt gevonden, het deelvenster kan duren voordat een tweede om weer te geven. Zoeken naar `LUIS-results` en hits invoeren op het toetsenbord. De lijst wordt met alleen LUIS queryresultaten toegevoegd met deze zelfstudie teruggebracht.

    ![Filteren op afhankelijkheden](./media/luis-tutorial-appinsights/app-insights-filter.png)

3. Selecteer het eerste item. Een nieuw venster geeft meer gedetailleerde gegevens, met inbegrip van de aangepaste gegevens voor de query LUIS helemaal rechts. De gegevens bevatten het belangrijkste doel en de score.

    ![Details van afhankelijkheid](./media/luis-tutorial-appinsights/app-insights-detail.png)

    Wanneer u klaar bent, selecteert u de top rechtse **X** om terug te keren naar de lijst met items van de afhankelijkheid. 


> [!Tip]
> Als u de afhankelijkheidslijst opslaan en dit bestand later wilt, klikt u op **... Meer** en klikt u op **favoriet opslaan**.

## <a name="query-application-insights-for-intent-score-and-utterance"></a>Application Insights-query voor het doel, score en utterance
Application Insights biedt u de power query uitvoeren op gegevens die met de [Kusto](https://docs.microsoft.com/azure/application-insights/app-insights-analytics#query-data-in-analytics) taal, alsmede exporteren naar [Power BI](https://powerbi.microsoft.com). 

1. Klik op **Analytics** aan de bovenkant van de afhankelijkheid worden vermeld, boven het filtervak. 

    ![Knop Analytics](./media/luis-tutorial-appinsights/analytics-button.png)

2. Er wordt een nieuw venster geopend met een query aan de bovenkant en een venster van de tabel gegevens hieronder die. Als u databases hebt gebruikt, wordt deze rangschikking bekend is. De query bevat alle objecten van de afgelopen 24 uur beginnen met de naam van de `LUIS-results`. De **CustomDimensions** kolom bevat de resultaten van de LUIS-query als naam/waarde-paren.

    ![Analytics-query-venster](./media/luis-tutorial-appinsights/analytics-query-window.png)

3. Als u wilt ophalen van de belangrijkste doel, score en utterance, voeg u de volgende vlak boven de laatste regel in het queryvenster:

    ```SQL
    | extend topIntent = tostring(customDimensions.LUIS_intent_intent)
    | extend score = todouble(customDimensions.LUIS_intent_score)
    | extend utterance = tostring(customDimensions.LUIS_text)
    ```

4. Voer de query uit. Blader naar de rechterkant in de tabel. De nieuwe kolommen topIntent, score en utterance zijn beschikbaar. Klik op de topIntent kolom te sorteren.

    ![Belangrijkste doel van Analytics](./media/luis-tutorial-appinsights/app-insights-top-intent.png)


Meer informatie over de [Kusto-querytaal](https://docs.microsoft.com/azure/log-analytics/query-language/get-started-queries) of [de gegevens exporteren naar Power BI](https://docs.microsoft.com/azure/application-insights/app-insights-export-power-bi). 

## <a name="next-steps"></a>Volgende stappen

Andere informatie die u mogelijk wilt toevoegen aan de application insights-gegevens omvatten app-ID, versie-ID, datum laatste model, laatste datum van de trein, de laatste publicatiedatum. Deze waarden kunnen ofwel worden opgehaald van de eindpunt-URL (app-ID en versie-ID) of van een [API ontwerpen](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c3d) aanroepen en vervolgens ingesteld in de bot-instellingen van de web-app en van daaruit opgehaald.  

Als u van hetzelfde abonnement endpoint voor meer dan één LUIS-app gebruikmaakt, moet u ook de abonnements-ID en een eigenschap met de mededeling dat het een gedeelde sleutel opnemen. 

> [!div class="nextstepaction"]
> [Meer informatie over de voorbeeld-uitingen](luis-how-to-add-example-utterances.md)
