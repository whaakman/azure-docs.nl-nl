---
title: LUIS gegevens toevoegen aan de Application Insights met C# | Microsoft Docs
titleSuffix: Azure
description: Een geïntegreerd met een LUIS toepassing en de Application Insights met C#-bot bouwen.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/07/2018
ms.author: v-geberr
ms.openlocfilehash: 52b6ae224b0e8da12eb4903f5100a6e5cc39704d
ms.sourcegitcommit: b7290b2cede85db346bb88fe3a5b3b316620808d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/05/2018
ms.locfileid: "35345972"
---
# <a name="add-luis-results-to-application-insights-from-a-web-app-bot"></a>LUIS resultaten van een web-app-bot naar Application Insights toevoegen
Deze zelfstudie voegt LUIS antwoord informatie [Application Insights](https://azure.microsoft.com/services/application-insights/) telemetrie gegevensopslag. Zodra u dat de gegevens hebt, kunt u deze query met de Kusto taal of Power BI analyseren, samenvoegen, en in het rapport op intents en entiteiten van de utterance in realtime. Dit analyse kunt u bepalen of u moet toevoegen of intents en entiteiten van uw app LUIS bewerken.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
* Application Insights toevoegen aan een web-app-bot
* Vastleggen en LUIS queryresultaten verzenden naar Application Insights
* Application Insights voor bovenste bedoelingen, score en utterance query

## <a name="prerequisites"></a>Vereisten

* Uw bot LUIS web-app uit de **[vorige zelfstudie](luis-csharp-tutorial-build-bot-framework-sample.md)** met Application Insights is ingeschakeld. 
* [Visual Studio 2017](https://www.visualstudio.com/downloads/) lokaal op uw computer geïnstalleerd.

> [!Tip]
> Als u nog geen een abonnement, kunt u registreren voor een [gratis account](https://azure.microsoft.com/free/).

Alle code in deze zelfstudie is beschikbaar op de [LUIS-Samples github-opslagplaats](https://github.com/Microsoft/LUIS-Samples/tree/master/documentation-samples/tutorial-web-app-bot-application-insights/csharp) en elke regel die is gekoppeld aan deze zelfstudie is toegelicht met `//LUIS Tutorial:`. 

## <a name="review-luis-web-app-bot"></a>Bekijk LUIS web app bot
Deze zelfstudie wordt ervan uitgegaan dat u hebt code dat vergelijkbaar is met de volgende of dat u hebt voltooid de [andere zelfstudie](luis-csharp-tutorial-build-bot-framework-sample.md): 

   [!code-csharp[Web app bot with LUIS](~/samples-luis/documentation-samples/tutorial-web-app-bot/csharp/BasicLuisDialog.cs "Web app bot with LUIS")]

## <a name="application-insights-in-web-app-bot"></a>Application Insights in web-app bot
De Application Insights-service toegevoegd als onderdeel van het maken van web-app bot service verzamelt momenteel telemetrie van de algemene status voor de bot. De functie verzamelt geen LUIS antwoord informatie. Als u wilt analyseren en verbeteren LUIS, moet u de informatie van de reactie LUIS.  

Als u wilt vastleggen in het antwoord LUIS, de web-app-bot moet **[Application Insights](https://www.nuget.org/packages/Microsoft.ApplicationInsights/)** geïnstalleerd en geconfigureerd voor het project. 

## <a name="download-web-app-bot"></a>Web-app bot downloaden
Gebruik [Visual Studio 2017](https://www.visualstudio.com/downloads/) toevoegen en configureren van Application Insights voor de web-app-bot. Om te gebruiken de WebBot-app in Visual Studio, downloaden de web-app-bot-code.

1. Selecteer in de Azure-portal voor de web-app-bot **bouwen**.

    ![Build in de portal selecteren](./media/luis-tutorial-bot-csharp-appinsights/download-build-menu.png)

2. Selecteer **Download zip-bestand** en wacht totdat het bestand is voorbereid.

    ![Zip-bestand downloaden](./media/luis-tutorial-bot-csharp-appinsights/download-link.png)

3. Selecteer **Download zip-bestand** in het pop-upvenster. Onthoud de locatie op uw computer, moet u deze in de volgende sectie.

    ![Het zip-bestand downloaden](./media/luis-tutorial-bot-csharp-appinsights/download-popup.png)

## <a name="open-solution-in-visual-studio-2017"></a>Geopende oplossing in Visual Studio 2017

1. Pak het bestand naar een map. 

2. Open Visual Studio 2017 en open het oplossingsbestand `Microsoft.Bot.Sample.LuisBot.sln`. Als de beveiligingswaarschuwing weergegeven, selecteert u 'OK'.

    ![Geopende oplossing in Visual Studio 2017](./media/luis-tutorial-bot-csharp-appinsights/vs-2017-security-warning.png)

3. Visual Studio moet afhankelijkheden toevoegen aan de oplossing. In de **Solution Explorer**, met de rechtermuisknop op **verwijzingen**, en selecteer **NuGet-pakketten beheren...** . 

    ![Manage NuGet Packages](./media/luis-tutorial-bot-csharp-appinsights/vs-2017-manage-nuget-packages.png)

4. De NuGet Package manager bevat een lijst met geïnstalleerde pakketten. Selecteer **herstellen** in de gele balk. Wacht totdat het herstelproces te voltooien.

    ![NuGet-pakketten herstellen](./media/luis-tutorial-bot-csharp-appinsights/vs-2017-restore-packages.png)

## <a name="add-application-insights-to-the-project"></a>Application Insights toevoegen aan het project
Installeren en configureren van Application Insights in Visual Studio. 

1. In Visual Studio-2017 in het bovenste menu selecteren **Project**, selecteer daarna **Application Insights Telemetry toevoegen...** .

2. In de **Application Insights-configuratie** Selecteer **gratis starten**

    ![Beginnen met het configureren van Application Insights](./media/luis-tutorial-bot-csharp-appinsights/vs-2017-configure-app-insights.png)

3. Uw app registreren met Application Insights. U moet uw Azure-portal referenties invoeren. 

4. Visual Studio wordt Application Insights toegevoegd aan het project weergeven status als dit wordt uitgevoerd. 

    ![Application Insights status](./media/luis-tutorial-bot-csharp-appinsights/vs-2017-adding-application-insights-to-project.png)

    Wanneer het proces is voltooid, de **Application Insights-configuratie** ziet u de voortgang weergegeven. 

    ![Voortgangsstatus van Application Insights](./media/luis-tutorial-bot-csharp-appinsights/vs-2017-configured-application-insights-to-project.png)

    De **trace verzamelen** is rood, wat betekent dat het niet is ingeschakeld. Deze zelfstudie gebruikt de functie niet. 

## <a name="build-and-resolve-errors"></a>Opbouwen en fouten oplossen

1. De oplossing bouwen door het selecteren van de **bouwen** menu selecteert **oplossing opnieuw opbouwen**. Wacht totdat de build te voltooien. 

2. Als de build met mislukt `CS0104` fouten, moet u deze kunt oplossen. In de `Controllers` map in de `MessagesController.cs file`, los van het niet-eenduidige gebruik van `Activity` type door het voorvoegsel van het type activiteit met het type Connector. U doet dit door de naam wijzigen `Activity` op regels 22 en 36 van `Activity` naar `Connector.Activity`. De oplossing opnieuw opbouwen. Er mag geen fouten meer build zijn.

    De volledige bron van het bestand is:

    [!code-csharp[MessagesController.cs file](~/samples-luis/documentation-samples/tutorial-web-app-bot-application-insights/csharp/MessagesController.cs "MessagesController.cs file")]

## <a name="publish-project-to-azure"></a>Project publiceren naar Azure
De **Application Insights** pakket bevindt zich nu in het project en correct geconfigureerd voor uw referenties in de Azure portal. De wijzigingen voor het project moeten worden gepubliceerd naar Azure.

1. In de **Solution Explorer**, met de rechtermuisknop op de projectnaam en selecteer vervolgens **publiceren**.

    ![Project naar portal publiceren](./media/luis-tutorial-bot-csharp-appinsights/vs-2017-publish.png)

2. In de **publiceren** Selecteer **nieuw profiel maken**.

    ![Project naar portal publiceren](./media/luis-tutorial-bot-csharp-appinsights/vs-2017-publish-1.png)

3. Selecteer **profiel importeren**, en selecteer **OK**.

    ![Project naar portal publiceren](./media/luis-tutorial-bot-csharp-appinsights/vs-2017-publish-2.png)

4. In de **publiceren instellingen importbestand** windows, navigeer naar de projectmap van uw, gaat u naar de `PostDeployScripts` map, selecteert u de bestanden die eindigen op `.PublishSettings`, en selecteer `Open`. U hebt nu publiceren voor dit project geconfigureerd. 

5. De broncode van uw lokale publiceren naar Bot-Service door het selecteren van de **publiceren** knop. De **uitvoer** venster status bevat. De rest van de zelfstudie is in de Azure portal voltooid. Visual Studio 2017 te sluiten. 

## <a name="open-three-browser-tabs"></a>Drie browsertabbladen openen
In de Azure portal, de web-app-bot zoeken en te openen. De volgende stappen uit drie verschillende weergaven van de web-app-bot gebruiken. Het is mogelijk beter te hebben van drie afzonderlijke tabbladen in de browser openen: 
  
>  * Testen in chatten
>  * Build/Open online code-editor -> App Service-Editor
>  * App Service Editor/Open Kudu-console -> Diagnostic-Console

## <a name="modify-basicluisdialogcs-code"></a>BasicLuisDialog.cs code wijzigen

1. In de **App Service-Editor** browsertabblad, open de `BasicLuisDialog.cs` bestand.

2. Voeg de volgende NuGet-afhankelijkheid onder de bestaande `using` regels:

   [!code-csharp[Add using statement](~/samples-luis/documentation-samples/tutorial-web-app-bot-application-insights/csharp/BasicLuisDialog.cs?range=11-12 "Add using statement")]

3. Voeg de `LogToApplicationInsights` functie:

   [!code-csharp[Add the LogToApplicationInsights function](~/samples-luis/documentation-samples/tutorial-web-app-bot-application-insights/csharp/BasicLuisDialog.cs?range=61-92 "Add the LogToApplicationInsights function")]

    De Application Insights-instrumentatiesleutel is al in de web-app-bot toepassing instelling met de naam `BotDevInsightsKey`. 

    De laatste regel van de functie worden de gegevens naar Application Insights toegevoegd. De naam van de tracering is `LUIS`, een unieke naam naast andere telemetriegegevens die door deze web-app-bot worden verzameld. Alle eigenschappen ook worden voorafgegaan door `LUIS_` zodat u welke gegevens zien kunt in deze zelfstudie wordt toegevoegd in vergelijking tot informatie die wordt bepaald door de web-app-bot.

4. Roep de `LogToApplicationInsights` functie aan de bovenkant van de `ShowLuisResult` functie:

   [!code-csharp[Use the LogToApplicationInsights function](~/samples-luis/documentation-samples/tutorial-web-app-bot-application-insights/csharp/BasicLuisDialog.cs?range=114-115 "Use the LogToApplicationInsights function")]

## <a name="build-web-app-bot"></a>Web-app bot bouwen
1. De web-app-bot op twee manieren maken. De eerste methode is met de rechtermuisknop op `build.cmd` in de **App Service-Editor**, selecteer daarna **uitvoeren vanaf de Console**. De uitvoer van de console weergegeven en wordt automatisch ingevuld `Finished successfully.`

2. Als dit niet wordt voltooid, moet u open de console, gaat u naar het script en voer deze met de volgende stappen uit. In de **App Service-Editor**, op de bovenste balk van blauw, selecteert u de naam van uw bot en selecteer vervolgens **Kudu-Console openen** in de vervolgkeuzelijst.

    ![Open Kudu-Console](./media/luis-tutorial-bot-csharp-appinsights/open-kudu-console.png)

3. Voer in het consolevenster de volgende opdracht:

    ```
    cd site\wwwroot && build.cmd
    ```

    Wacht totdat de build om te worden voltooid `Finished successfully.`

## <a name="test-the-web-app-bot"></a>De web-app-bot testen

1. Als u wilt testen van uw web-app-bot, opent u de **testen in Web chatten** functie in de portal. 

2. Voer de woordgroep `Coffee bar on please`.  

    ![Web-app bot in chat testen](./media/luis-tutorial-bot-csharp-appinsights/test-in-web-chat.png)

3. U moet zien geen verschil in het antwoord chatbot. De wijziging is verzenden van gegevens naar Application Insights niet in de bot antwoorden. Voer enkele meer utterances zodat er iets meer gegevens in Application Insights:

```
Please deliver a pizza
Turn off all the lights
Turn on the hall light
```

## <a name="view-luis-entries-in-application-insights"></a>LUIS items weergeven in Application Insights
Application Insights om te zien van de vermeldingen LUIS openen. 

1. Selecteer in de portal **alle resources** vervolgens filteren op de bot-naam van de web-app. Klik op de resource met het type **Application Insights**. Het pictogram voor Application Insights is een gloeilampje. 

    ![Zoeken naar de app insights](./media/luis-tutorial-bot-csharp-appinsights/portal-service-list-app-insights.png)

2. Wanneer de bron wordt geopend, klikt u op de **Search** pictogram van het Vergrootglas in het deelvenster uiterst rechts. Een nieuw deelvenster naar de juiste wordt weergegeven. Afhankelijk van hoeveel telemetriegegevens wordt gevonden, het paneel kan duren voordat een tweede om weer te geven. Zoeken naar `LUIS`. De lijst wordt met NET LUIS queryresultaten toegevoegd met deze zelfstudie verkleind.

    ![Zoeken naar traceringen](./media/luis-tutorial-bot-csharp-appinsights/portal-service-list-app-insights-search-luis-trace.png)

3. Selecteer de bovenste waarde. Meer gedetailleerde gegevens, inclusief de aangepaste gegevens voor de query LUIS helemaal rechts in een nieuw venster weergegeven. De gegevens bevatten het belangrijkste doel en de score.

    ![Trace-item controleren](./media/luis-tutorial-bot-csharp-appinsights/portal-service-list-app-insights-search-luis-trace-item.png)

    Wanneer u klaar bent, selecteert u de meest rechtse top **X** om terug te keren naar de lijst met items van afhankelijkheid. 


> [!Tip]
> Als u de lijst afhankelijkheid opslaan en terugkeren naar deze later wilt, klikt u op **... Meer** en klik op **favoriet opslaan**.

## <a name="query-application-insights-for-intent-score-and-utterance"></a>Application Insights-query voor het doel en score utterance
Application Insights biedt u de power query uitvoeren op de gegevens met de [Kusto](https://docs.microsoft.com/azure/application-insights/app-insights-analytics#query-data-in-analytics) taal, alsmede exporteren naar [PowerBI](https://powerbi.microsoft.com). 

1. Klik op **Analytics** aan de bovenkant van de afhankelijkheid aanbieding boven het filtervak. 

    ![Knop Analytics](./media/luis-tutorial-bot-csharp-appinsights/portal-service-list-app-insights-search-luis-analytics-button.png)

2. Een nieuw venster geopend met een venster van de tabel gegevens hieronder die en een queryvenster aan de bovenkant. Als u eerder databases hebt gebruikt, wordt deze rangschikking bekend is. De query bevat alle items uit de afgelopen 24 uur dat begint met de naam `LUIS`. De **CustomDimensions** kolom heeft de queryresultaten LUIS als naam/waarde-paren.

    ![Standaard analytics-rapport](./media/luis-tutorial-bot-csharp-appinsights/analytics-query-1.png)

3. Toevoegen om op te halen uit de bovenste bedoelingen, score en utterance, de volgende net boven de laatste regel in het queryvenster:

    ```SQL
    | extend topIntent = tostring(customDimensions.LUIS_topScoringIntent)
    | extend score = todouble(customDimensions.LUIS_topScoringIntentScore)
    | extend utterance = tostring(customDimensions.LUIS_query)
    ```

4. Voer de query uit. Schuif naar rechts in de tabel. De nieuwe kolommen van topIntent, score en utterance zijn beschikbaar. Klik op de topIntent kolom te sorteren.

    ![Aangepaste analytics-rapport](./media/luis-tutorial-bot-csharp-appinsights/analytics-query-2.png)


Meer informatie over de [Kusto querytaal](https://docs.loganalytics.io/docs/Learn/Getting-Started/Getting-started-with-queries) of [exporteren van gegevens naar PowerBi](https://docs.microsoft.com/azure/application-insights/app-insights-export-power-bi). 


## <a name="learn-more-about-bot-framework"></a>Meer informatie over Bot-Framework
Meer informatie over [Bot Framework](https://dev.botframework.com/).

## <a name="next-steps"></a>Volgende stappen

Andere informatie die u mogelijk wilt toevoegen aan de application insights-gegevens omvat app-ID, versie-ID, datum van laatste model, laatste trainen datum, datum laatste te publiceren. Deze waarden kunnen ofwel worden opgehaald van de eindpunt-URL (app-ID en versie-ID), of van een [API authoring](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c3d) Roep vervolgens ingesteld in de web-appinstellingen bot en van daaruit opgehaald.  

Als u hetzelfde abonnement endpoint voor meer dan één LUIS app gebruikt, moet u ook de abonnements-ID en een eigenschap met de mededeling dat deze een gedeelde sleutel opnemen. 

> [!div class="nextstepaction"]
> [Meer informatie over voorbeeld utterances](luis-how-to-add-example-utterances.md)
