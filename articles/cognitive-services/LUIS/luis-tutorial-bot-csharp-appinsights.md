---
title: LUIS-gegevens toevoegen aan de Application Insights met behulp van C# | Microsoft Docs
titleSuffix: Azure
description: Bouw een bot die is geïntegreerd met een LUIS-toepassing en de Application Insights met behulp van C#.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/07/2018
ms.author: diberry
ms.openlocfilehash: f1efe305f5659bfab50cee13ac30d56531cc6093
ms.sourcegitcommit: 194789f8a678be2ddca5397137005c53b666e51e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39237788"
---
# <a name="add-luis-results-to-application-insights-from-a-web-app-bot"></a>LUIS resultaten naar Application Insights toevoegen uit een web-app-bot
In deze zelfstudie voegt LUIS antwoord informatie [Application Insights](https://azure.microsoft.com/services/application-insights/) telemetrie-gegevensopslag. Zodra u deze gegevens hebt, kunt u deze met de Kusto-taal of Power BI te analyseren, samenvoegen, opvragen en te rapporteren intenties en entiteiten van de utterance in realtime. Met deze analyse kunt u bepalen of u moet toevoegen of bewerken van de intenties en entiteiten van uw LUIS-app.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
* Application Insights toevoegen aan een web-app-bot
* Vastleggen en LUIS queryresultaten verzenden naar Application Insights
* Query uitvoeren op Application Insights voor het belangrijkste doel, score en utterance

## <a name="prerequisites"></a>Vereisten

* Uw LUIS web app-bot uit de **[vorige zelfstudie](luis-csharp-tutorial-build-bot-framework-sample.md)** met Application Insights is ingeschakeld. 
* [Visual Studio 2017](https://www.visualstudio.com/downloads/) lokaal op uw computer geïnstalleerd.

> [!Tip]
> Als u nog geen een abonnement, kunt u zich registreren voor een [gratis account](https://azure.microsoft.com/free/).

Alle code in deze zelfstudie vindt u op de [LUIS-Samples github-opslagplaats](https://github.com/Microsoft/LUIS-Samples/tree/master/documentation-samples/tutorial-web-app-bot-application-insights/csharp) en elke regel die is gekoppeld aan deze zelfstudie is opgenomen als met `//LUIS Tutorial:`. 

## <a name="review-luis-web-app-bot"></a>LUIS web app-bot bekijken
In deze zelfstudie wordt ervan uitgegaan dat u hebt code dat er als volgt of dat u hebt voltooid de [andere zelfstudie](luis-csharp-tutorial-build-bot-framework-sample.md): 

   [!code-csharp[Web app bot with LUIS](~/samples-luis/documentation-samples/tutorial-web-app-bot/csharp/BasicLuisDialog.cs "Web app bot with LUIS")]

## <a name="application-insights-in-web-app-bot"></a>Application Insights in web app-bot
Op dit moment verzamelt de Application Insights-service, toegevoegd als onderdeel van het maken van web-app-bot-service telemetrie van de algemene status voor de bot. Deze verzamelt geen informatie van LUIS-antwoord. Als u wilt analyseren en verbeteren van LUIS, moet u de informatie van LUIS-antwoord.  

Als u wilt vastleggen van de reactie van LUIS, de web-app-bot moet **[Application Insights](https://www.nuget.org/packages/Microsoft.ApplicationInsights/)** geïnstalleerd en geconfigureerd voor het project. 

## <a name="download-web-app-bot"></a>Web app-bot downloaden
Gebruik [Visual Studio 2017](https://www.visualstudio.com/downloads/) toevoegen en configureren van Application Insights voor de web-app-bot. Als u wilt gebruiken de web-app-bot in Visual Studio, de code in de bot voor web-app te downloaden.

1. Selecteer in de Azure-portal voor de web-app-bot, **bouwen**.

    ![Build selecteren in de portal](./media/luis-tutorial-bot-csharp-appinsights/download-build-menu.png)

2. Selecteer **Download zip-bestand** en wacht totdat het bestand wordt voorbereid.

    ![Zip-bestand downloaden](./media/luis-tutorial-bot-csharp-appinsights/download-link.png)

3. Selecteer **Download zip-bestand** in het pop-upvenster. Onthoud de locatie op uw computer, moet u deze in de volgende sectie.

    ![Zip-bestand downloaden](./media/luis-tutorial-bot-csharp-appinsights/download-popup.png)

## <a name="open-solution-in-visual-studio-2017"></a>Geopende oplossing in Visual Studio 2017

1. Pak het bestand naar een map. 

2. Open Visual Studio 2017 en open het oplossingsbestand `Microsoft.Bot.Sample.LuisBot.sln`. Als de beveiligingswaarschuwing wordt weergegeven, selecteert u 'OK'.

    ![Geopende oplossing in Visual Studio 2017](./media/luis-tutorial-bot-csharp-appinsights/vs-2017-security-warning.png)

3. Visual Studio moet afhankelijkheden toevoegen aan de oplossing. In de **Solution Explorer**, met de rechtermuisknop op **verwijzingen**, en selecteer **NuGet-pakketten beheren...** . 

    ![Manage NuGet Packages](./media/luis-tutorial-bot-csharp-appinsights/vs-2017-manage-nuget-packages.png)

4. De NuGet Package manager geeft een lijst van geïnstalleerde pakketten. Selecteer **herstellen** in de gele balk. Wacht tot het herstelproces te voltooien.

    ![NuGet-pakketten herstellen](./media/luis-tutorial-bot-csharp-appinsights/vs-2017-restore-packages.png)

## <a name="add-application-insights-to-the-project"></a>Application Insights toevoegen aan het project
Installeren en configureren van Application Insights in Visual Studio. 

1. In Visual Studio 2017, in het bovenste menu Selecteer **Project**en selecteer vervolgens **Application Insights Telemetry toevoegen...** .

2. In de **configuratie van Application Insights** venster **gratis starten**

    ![Beginnen met de configuratie van Application Insights](./media/luis-tutorial-bot-csharp-appinsights/vs-2017-configure-app-insights.png)

3. Uw app registreren bij Application Insights. U moet uw Azure-portal referenties invoeren. 

4. Visual Studio voegt Application Insights toe aan het project, status weergeven als dit gebeurt. 

    ![Status van Application Insights](./media/luis-tutorial-bot-csharp-appinsights/vs-2017-adding-application-insights-to-project.png)

    Wanneer het proces is voltooid, de **configuratie van Application Insights** de voortgangsstatus wordt weergegeven. 

    ![Voortgangsstatus van Application Insights](./media/luis-tutorial-bot-csharp-appinsights/vs-2017-configured-application-insights-to-project.png)

    De **tracering verzamelen** is rood, wat betekent dat deze niet is ingeschakeld. In deze zelfstudie gebruik geen van de functie. 

## <a name="build-and-resolve-errors"></a>Bouwen en fouten oplossen

1. Maak de oplossing hiervoor de **bouwen** in het menu Selecteer vervolgens **oplossing opnieuw opbouwen**. Wacht tot de build te voltooien. 

2. Als de build is mislukt met `CS0104` fouten, moet u deze kunt oplossen. In de `Controllers` map in de `MessagesController.cs file`, los van de niet-eenduidige gebruik van `Activity` type door het voorvoegsel van het type activiteit met de Connector-type. U doet dit door de naam wijzigen `Activity` op regel 22 en 36 van `Activity` naar `Connector.Activity`. De oplossing opnieuw opbouwen. Er mag geen fouten meer build zijn.

    De volledige bron van het bestand is:

    [!code-csharp[MessagesController.cs file](~/samples-luis/documentation-samples/tutorial-web-app-bot-application-insights/csharp/MessagesController.cs "MessagesController.cs file")]

## <a name="publish-project-to-azure"></a>Project kunt publiceren naar Azure
De **Application Insights** pakket wordt nu weergegeven in het project en correct geconfigureerd voor uw referenties in de Azure-portal. De wijzigingen voor het project moeten worden gepubliceerd naar Azure.

1. In de **Solution Explorer**, met de rechtermuisknop op de projectnaam en selecteer **publiceren**.

    ![Project kunt publiceren naar portal](./media/luis-tutorial-bot-csharp-appinsights/vs-2017-publish.png)

2. In de **publiceren** venster **nieuw profiel maken**.

    ![Project kunt publiceren naar portal](./media/luis-tutorial-bot-csharp-appinsights/vs-2017-publish-1.png)

3. Selecteer **-profiel importeren**, en selecteer **OK**.

    ![Project kunt publiceren naar portal](./media/luis-tutorial-bot-csharp-appinsights/vs-2017-publish-2.png)

4. In de **publiceren instellingen importbestand** windows, gaat u naar de projectmap, gaat u naar de `PostDeployScripts` map, selecteert u de bestanden die eindigen op `.PublishSettings`, en selecteer `Open`. U hebt nu publiceren voor dit project geconfigureerd. 

5. Uw lokale broncode publiceren naar Bot Service door het selecteren van de **publiceren** knop. De **uitvoer** venster toont de status. De rest van de zelfstudie wordt uitgevoerd in Azure portal. Sluit Visual Studio 2017. 

## <a name="open-three-browser-tabs"></a>Drie browsertabbladen openen
In de Azure-portal, de web-app-bot zoeken en te openen. De volgende stappen uit drie verschillende weergaven van de web-app-bot gebruiken. Het is mogelijk beter te hebben van drie afzonderlijke tabbladen in de browser openen: 
  
>  * Testen in chatten
>  * Build/Open online code-editor -> App Service-Editor
>  * App Service-Editor is geopend/Kudu-console -> Diagnostic-Console

## <a name="modify-basicluisdialogcs-code"></a>BasicLuisDialog.cs-code wijzigen

1. In de **App Service-Editor** browsertabblad, open de `BasicLuisDialog.cs` bestand.

2. Voeg de volgende NuGet-afhankelijkheden onder de bestaande `using` regels:

   [!code-csharp[Add using statement](~/samples-luis/documentation-samples/tutorial-web-app-bot-application-insights/csharp/BasicLuisDialog.cs?range=11-12 "Add using statement")]

3. Voeg de `LogToApplicationInsights` functie:

   [!code-csharp[Add the LogToApplicationInsights function](~/samples-luis/documentation-samples/tutorial-web-app-bot-application-insights/csharp/BasicLuisDialog.cs?range=61-92 "Add the LogToApplicationInsights function")]

    De Application Insights-instrumentatiesleutel is al in de web-app-bot toepassing instellen met de naam `BotDevInsightsKey`. 

    De laatste regel van de functie worden de gegevens naar Application Insights. De naam van de tracering is `LUIS`, een unieke naam naast andere telemetriegegevens die zijn verzameld door deze web-app-bot. Alle eigenschappen ook worden voorafgegaan door `LUIS_` zodat u welke gegevens zien kunt in deze zelfstudie wordt toegevoegd in vergelijking tot informatie die wordt bepaald door de web-app-bot.

4. Roep de `LogToApplicationInsights` functie aan de bovenkant van de `ShowLuisResult` functie:

   [!code-csharp[Use the LogToApplicationInsights function](~/samples-luis/documentation-samples/tutorial-web-app-bot-application-insights/csharp/BasicLuisDialog.cs?range=114-115 "Use the LogToApplicationInsights function")]

## <a name="build-web-app-bot"></a>Bouw web app-bot
1. Bouw de web-app-bot op twee manieren. De eerste methode is met de rechtermuisknop op `build.cmd` in de **App Service-Editor**en selecteer vervolgens **uitvoeren vanaf de Console**. De uitvoer van de console wordt weergegeven en is voltooid met `Finished successfully.`

2. Als dit niet wordt voltooid, moet u de console openen, gaat u naar het script en voeren met behulp van de volgende stappen uit. In de **App Service-Editor**, op de bovenste blauwe balk, selecteert u de naam van uw bot, vervolgens **Kudu-Console openen** in de vervolgkeuzelijst.

    ![Open Kudu-Console](./media/luis-tutorial-bot-csharp-appinsights/open-kudu-console.png)

3. Voer in het consolevenster de volgende opdracht:

    ```
    cd site\wwwroot && build.cmd
    ```

    Wacht tot de build te voltooien met `Finished successfully.`

## <a name="test-the-web-app-bot"></a>Testen van de web-app-bot

1. Als u wilt testen van uw web-app-bot, open de **testen in Web Chat** functie in de portal. 

2. Voer de woordgroep `Coffee bar on please`.  

    ![Web app-bot in chat testen](./media/luis-tutorial-bot-csharp-appinsights/test-in-web-chat.png)

3. Hier ziet u geen verschil in het antwoord chatbot. De wijziging verzendt gegevens naar Application Insights, niet in de bot antwoorden. Voer enkele meer uitingen zodat er nog iets meer gegevens in Application Insights:

```
Please deliver a pizza
Turn off all the lights
Turn on the hall light
```

## <a name="view-luis-entries-in-application-insights"></a>LUIS-items weergeven in Application Insights
Open Application Insights om te zien van de LUIS-vermeldingen. 

1. Selecteer in de portal **alle resources** vervolgens filteren op de naam van de web-app-bot. Klik op de resource met het type **Application Insights**. Het pictogram voor Application Insights is een gloeilampje. 

    ![Zoeken naar app insights](./media/luis-tutorial-bot-csharp-appinsights/portal-service-list-app-insights.png)

2. Wanneer de resource wordt geopend, klikt u op de **zoeken** pictogram van het Vergrootglas in het deelvenster uiterst rechts. Een nieuw deelvenster met de juiste weergegeven. Afhankelijk van hoeveel telemetriegegevens wordt gevonden, het deelvenster kan duren voordat een tweede om weer te geven. Zoeken naar `LUIS`. De lijst wordt met alleen LUIS queryresultaten toegevoegd met deze zelfstudie teruggebracht.

    ![Zoeken naar traceringen](./media/luis-tutorial-bot-csharp-appinsights/portal-service-list-app-insights-search-luis-trace.png)

3. Selecteer het eerste item. Een nieuw venster geeft meer gedetailleerde gegevens, met inbegrip van de aangepaste gegevens voor de query LUIS helemaal rechts. De gegevens bevatten het belangrijkste doel en de score.

    ![Lees trace artikel](./media/luis-tutorial-bot-csharp-appinsights/portal-service-list-app-insights-search-luis-trace-item.png)

    Wanneer u klaar bent, selecteert u de top rechtse **X** om terug te keren naar de lijst met items van de afhankelijkheid. 


> [!Tip]
> Als u de afhankelijkheidslijst opslaan en dit bestand later wilt, klikt u op **... Meer** en klikt u op **favoriet opslaan**.

## <a name="query-application-insights-for-intent-score-and-utterance"></a>Application Insights-query voor het doel, score en utterance
Application Insights biedt u de power query uitvoeren op gegevens die met de [Kusto](https://docs.microsoft.com/azure/application-insights/app-insights-analytics#query-data-in-analytics) taal, alsmede exporteren naar [Power BI](https://powerbi.microsoft.com). 

1. Klik op **Analytics** aan de bovenkant van de afhankelijkheid worden vermeld, boven het filtervak. 

    ![Knop Analytics](./media/luis-tutorial-bot-csharp-appinsights/portal-service-list-app-insights-search-luis-analytics-button.png)

2. Er wordt een nieuw venster geopend met een query aan de bovenkant en een venster van de tabel gegevens hieronder die. Als u databases hebt gebruikt, wordt deze rangschikking bekend is. De query bevat alle objecten van de afgelopen 24 uur beginnen met de naam van de `LUIS`. De **CustomDimensions** kolom bevat de resultaten van de LUIS-query als naam/waarde-paren.

    ![Standaard-analyserapport](./media/luis-tutorial-bot-csharp-appinsights/analytics-query-1.png)

3. Als u wilt ophalen van de belangrijkste doel, score en utterance, voeg u de volgende vlak boven de laatste regel in het queryvenster:

    ```SQL
    | extend topIntent = tostring(customDimensions.LUIS_topScoringIntent)
    | extend score = todouble(customDimensions.LUIS_topScoringIntentScore)
    | extend utterance = tostring(customDimensions.LUIS_query)
    ```

4. Voer de query uit. Blader naar de rechterkant in de tabel. De nieuwe kolommen topIntent, score en utterance zijn beschikbaar. Klik op de topIntent kolom te sorteren.

    ![Aangepaste analyserapport](./media/luis-tutorial-bot-csharp-appinsights/analytics-query-2.png)


Meer informatie over de [Kusto-querytaal](https://docs.loganalytics.io/docs/Learn/Getting-Started/Getting-started-with-queries) of [de gegevens exporteren naar Power BI](https://docs.microsoft.com/azure/application-insights/app-insights-export-power-bi). 


## <a name="learn-more-about-bot-framework"></a>Meer informatie over Bot Framework
Meer informatie over [Bot Framework](https://dev.botframework.com/).

## <a name="next-steps"></a>Volgende stappen

Andere informatie die u mogelijk wilt toevoegen aan de application insights-gegevens omvatten app-ID, versie-ID, datum laatste model, laatste datum van de trein, de laatste publicatiedatum. Deze waarden kunnen ofwel worden opgehaald van de eindpunt-URL (app-ID en versie-ID) of van een [API ontwerpen](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c3d) aanroepen en vervolgens ingesteld in de bot-instellingen van de web-app en van daaruit opgehaald.  

Als u van hetzelfde abonnement endpoint voor meer dan één LUIS-app gebruikmaakt, moet u ook de abonnements-ID en een eigenschap met de mededeling dat het een gedeelde sleutel opnemen. 

> [!div class="nextstepaction"]
> [Meer informatie over de voorbeeld-uitingen](luis-how-to-add-example-utterances.md)
