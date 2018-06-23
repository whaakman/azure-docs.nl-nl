---
title: LUIS gegevens toevoegen aan de Application Insights met behulp van Node.js | Microsoft Docs
titleSuffix: Azure
description: Bouw een bot geïntegreerd met een LUIS toepassing en de Application Insights met behulp van Node.js.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 01/18/2018
ms.author: v-geberr
ms.openlocfilehash: 929b6e1cc980d7215f91a616820e257aed26bab7
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345769"
---
# <a name="add-luis-results-to-application-insights-from-a-web-app-bot"></a>LUIS resultaten van een web-app-bot naar Application Insights toevoegen
Deze zelfstudie voegt LUIS aanvraag en antwoord-informatie naar [Application Insights](https://azure.microsoft.com/services/application-insights/) telemetrie gegevensopslag. Zodra u dat de gegevens hebt, kunt u deze query met de Kusto taal of Power BI analyseren, samenvoegen, en in het rapport op intents en entiteiten van de utterance in realtime. Dit analyse kunt u bepalen of u moet toevoegen of intents en entiteiten van uw app LUIS bewerken.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
* Application Insights-bibliotheek toevoegen aan een web-app-bot
* Vastleggen en LUIS queryresultaten verzenden naar Application Insights
* Application Insights voor bovenste bedoelingen, score en utterance query

## <a name="prerequisites"></a>Vereisten

* Uw bot LUIS web-app uit de **[vorige zelfstudie](luis-nodejs-tutorial-build-bot-framework-sample.md)** met Application Insights is ingeschakeld. 

> [!Tip]
> Als u nog geen een abonnement, kunt u registreren voor een [gratis account](https://azure.microsoft.com/free/).

Alle code in deze zelfstudie is beschikbaar op de [LUIS-Samples github-opslagplaats](https://github.com/Microsoft/LUIS-Samples/tree/master/documentation-samples/tutorial-web-app-bot-application-insights/nodejs) en elke regel die is gekoppeld aan deze zelfstudie is toegelicht met `//APPINSIGHT:`. 

## <a name="web-app-bot-with-luis"></a>Web-app bot met LUIS
Deze zelfstudie wordt ervan uitgegaan dat u hebt code dat vergelijkbaar is met de volgende of dat u hebt voltooid de [andere zelfstudie](luis-nodejs-tutorial-build-bot-framework-sample.md): 

   [!code-javascript[Web app bot with LUIS](~/samples-luis/documentation-samples/tutorial-web-app-bot/nodejs/app.js "Web app bot with LUIS")]

## <a name="add-application-insights-library-to-web-app-bot"></a>Application Insights-bibliotheek toevoegen aan web-app bot
Op dit moment in de Application Insights-service gebruikt in deze web-app-bot telemetrie van de algemene status voor de bot worden verzameld. Deze verzamelt geen LUIS aanvraag en -antwoord informatie die u nodig hebt om te controleren en herstellen van uw intents en entiteiten. 

Als u wilt vastleggen die de LUIS aanvraag en -antwoord zijn, de WebBot-app moet de **[Application Insights](https://www.npmjs.com/package/applicationinsights)** NPM-pakket geïnstalleerd en geconfigureerd de **app.js** bestand. De handlers opzet dialoogvenster moeten de LUIS aanvraag en -antwoord om informatie te verzenden naar Application Insights. 

1. Selecteer in de Azure-portal in de web-app bot service **bouwen** onder de **Bot Management** sectie. 

    ![Zoeken naar de app insights](./media/luis-tutorial-appinsights/build.png)

2. Een nieuw browsertabblad geopend met de App Service-Editor. Selecteer de naam van de app in de bovenste balk en vervolgens **Kudu-Console openen**. 

    ![Zoeken naar de app insights](./media/luis-tutorial-appinsights/kudu-console.png)

3. Voer de volgende opdracht om Application Insights en het onderstrepingsteken pakketten te installeren in de console:

    ```
    cd site\wwwroot && npm install applicationinsights && npm install underscore
    ```

    ![Zoeken naar de app insights](./media/luis-tutorial-appinsights/npm-install.png)

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

    U klaar bent met het browsertabblad kudu-console.

## <a name="capture-and-send-luis-query-results-to-application-insights"></a>Vastleggen en LUIS queryresultaten verzenden naar Application Insights
1. Open in het tabblad App Service-Editor browser de **app.js** bestand.

2. Voeg de volgende bibliotheken NPM onder de bestaande `requires` regels:

   [!code-javascript[Add NPM packages to app.js](~/samples-luis/documentation-samples/tutorial-web-app-bot-application-insights/nodejs/app.js?range=12-16 "Add NPM packages to app.js")]

3. De Application Insights-object maken en gebruiken van de toepassingsinstelling voor web-app bot **BotDevInsightsKey**: 

   [!code-javascript[Create the Application Insights object](~/samples-luis/documentation-samples/tutorial-web-app-bot-application-insights/nodejs/app.js?range=68-80 "Create the Application Insights object")]

4. Voeg de **appInsightsLog** functie:

   [!code-javascript[Add the appInsightsLog function](~/samples-luis/documentation-samples/tutorial-web-app-bot-application-insights/nodejs/app.js?range=82-109 "Add the appInsightsLog function")]

    De laatste regel van de functie is waar de gegevens naar Application Insights is toegevoegd. De naam van de gebeurtenis is **LUIS resultaten**, een unieke naam naast andere telemetriegegevens die door deze web-app-bot worden verzameld. 

5. Gebruik de **appInsightsLog** functie. U toevoegen deze aan elke opzet dialoogvenster:

   [!code-javascript[Use the appInsightsLog function](~/samples-luis/documentation-samples/tutorial-web-app-bot-application-insights/nodejs/app.js?range=117-118 "Use the appInsightsLog function")]

6. U kunt uw web-app-bot testen met de **testen in Web chatten** functie. U moet zien geen verschil omdat al het werk in Application Insights, niet in de bot-antwoorden.

## <a name="view-luis-entries-in-application-insights"></a>LUIS items weergeven in Application Insights
Application Insights om te zien van de vermeldingen LUIS openen. 

1. Selecteer in de portal **alle resources** vervolgens filteren op de bot-naam van de web-app. Klik op de resource met het type **Application Insights**. Het pictogram voor Application Insights is een gloeilampje. 

    ![Zoeken naar de app insights](./media/luis-tutorial-appinsights/search-for-app-insights.png)



2. Wanneer de bron wordt geopend, klikt u op de **Search** pictogram van het Vergrootglas in het deelvenster uiterst rechts. Een nieuw deelvenster naar de juiste wordt weergegeven. Afhankelijk van hoeveel telemetriegegevens wordt gevonden, het paneel kan duren voordat een tweede om weer te geven. Zoeken naar `LUIS-results` en drukt op het toetsenbord invoeren. De lijst wordt met NET LUIS queryresultaten toegevoegd met deze zelfstudie verkleind.

    ![Filteren op afhankelijkheden](./media/luis-tutorial-appinsights/app-insights-filter.png)

3. Selecteer de bovenste waarde. Meer gedetailleerde gegevens, inclusief de aangepaste gegevens voor de query LUIS helemaal rechts in een nieuw venster weergegeven. De gegevens bevatten het belangrijkste doel en de score.

    ![Details van afhankelijkheid](./media/luis-tutorial-appinsights/app-insights-detail.png)

    Wanneer u klaar bent, selecteert u de meest rechtse top **X** om terug te keren naar de lijst met items van afhankelijkheid. 


> [!Tip]
> Als u de lijst afhankelijkheid opslaan en terugkeren naar deze later wilt, klikt u op **... Meer** en klik op **favoriet opslaan**.

## <a name="query-application-insights-for-intent-score-and-utterance"></a>Application Insights-query voor het doel en score utterance
Application Insights biedt u de power query uitvoeren op de gegevens met de [Kusto](https://docs.microsoft.com/azure/application-insights/app-insights-analytics#query-data-in-analytics) taal, alsmede exporteren naar [PowerBI](https://powerbi.microsoft.com). 

1. Klik op **Analytics** aan de bovenkant van de afhankelijkheid aanbieding boven het filtervak. 

    ![Knop Analytics](./media/luis-tutorial-appinsights/analytics-button.png)

2. Een nieuw venster geopend met een venster van de tabel gegevens hieronder die en een queryvenster aan de bovenkant. Als u eerder databases hebt gebruikt, wordt deze rangschikking bekend is. De query bevat alle items uit de afgelopen 24 uur dat begint met de naam `LUIS-results`. De **CustomDimensions** kolom heeft de queryresultaten LUIS als naam/waarde-paren.

    ![Analytics query-venster](./media/luis-tutorial-appinsights/analytics-query-window.png)

3. Toevoegen om op te halen uit de bovenste bedoelingen, score en utterance, de volgende net boven de laatste regel in het queryvenster:

    ```SQL
    | extend topIntent = tostring(customDimensions.LUIS_intent_intent)
    | extend score = todouble(customDimensions.LUIS_intent_score)
    | extend utterance = tostring(customDimensions.LUIS_text)
    ```

4. Voer de query uit. Schuif naar rechts in de tabel. De nieuwe kolommen van topIntent, score en utterance zijn beschikbaar. Klik op de topIntent kolom te sorteren.

    ![De bovenste bedoeling Analytics](./media/luis-tutorial-appinsights/app-insights-top-intent.png)


Meer informatie over de [Kusto querytaal](https://docs.loganalytics.io/docs/Learn/Getting-Started/Getting-started-with-queries) of [exporteren van gegevens naar PowerBi](https://docs.microsoft.com/azure/application-insights/app-insights-export-power-bi). 

## <a name="next-steps"></a>Volgende stappen

Andere informatie die u mogelijk wilt toevoegen aan de application insights-gegevens omvat app-ID, versie-ID, datum van laatste model, laatste trainen datum, datum laatste te publiceren. Deze waarden kunnen ofwel worden opgehaald van de eindpunt-URL (app-ID en versie-ID), of van een [API authoring](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c3d) Roep vervolgens ingesteld in de web-appinstellingen bot en van daaruit opgehaald.  

Als u hetzelfde abonnement endpoint voor meer dan één LUIS app gebruikt, moet u ook de abonnements-ID en een eigenschap met de mededeling dat deze een gedeelde sleutel opnemen. 

> [!div class="nextstepaction"]
> [Meer informatie over voorbeeld utterances](luis-how-to-add-example-utterances.md)
