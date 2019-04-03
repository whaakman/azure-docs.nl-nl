---
title: Bot - C# - v3
titleSuffix: Language Understanding - Azure Cognitive Services
description: Bouw met behulp van C# een chatbot met ingebouwd taalbegrip (LUIS). Deze chatbot maakt gebruik van het vooraf gedefinieerde HomeAutomation domein een bot-oplossing snel implementeren.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 01/09/2019
ms.author: diberry
ms.openlocfilehash: f23cf78bfca48b3a78e234520d645abdb354038f
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58878465"
---
# <a name="luis-bot-in-c-with-the-bot-framework-3x-and-the-azure-web-app-bot"></a>LUIS-bot in C# met Bot Framework 3.x en de Azure-Web-app-bot

Bouw met behulp van C# een chatbot met ingebouwd taalbegrip (LUIS). Deze chatbot maakt gebruik van het vooraf gedefinieerde HomeAutomation domein een bot-oplossing snel implementeren. De bot is gebouwd met Bot Framework 3.x en de Azure-Web-app-bot.

## <a name="prerequisite"></a>Vereiste

* [LUIS-app HomeAutomation](luis-get-started-create-app.md). De intenties van deze kaart LUIS-app aan van de bot dialoogvenster handlers. 

## <a name="luis-homeautomation-intents"></a>LUIS HomeAutomation Intents

| Intentie | Voorbeeld van een utterance | Bot-functionaliteit |
|:----:|:----------:|---|
| HomeAutomation.TurnOn | Schakel in het licht. | Wanneer de bedoeling LUIS `HomeAutomation.TurnOn` wordt gedetecteerd, wordt de bot roept de `OnIntent` dialoogvenster handler. Dit dialoogvenster is waar u een IoT-service om te schakelen op een apparaat en de gebruiker die het apparaat is ingeschakeld zien wilt aanroepen. |
| HomeAutomation.TurnOff | De verlichting slaapkamers uitschakelen. | Wanneer de bedoeling LUIS `HomeAutomation.TurnOff` wordt gedetecteerd, wordt de bot roept de `OffIntent` dialoogvenster handler. Dit dialoogvenster is waar roept u een IoT-service waarmee een apparaat uitschakelen en de gebruiker melden dat het apparaat is uitgeschakeld. |

## <a name="create-a-language-understanding-bot-with-bot-service"></a>Maken van een bot Language Understanding met Bot Service

1. In de [Azure-portal](https://portal.azure.com), selecteer **maken van nieuwe resource** in het bovenste menu links.

    ![Nieuwe resource maken in Azure portal](./media/luis-tutorial-cscharp-web-bot/bot-service-creation.png)

2. Zoek in het zoekvak **Web App-Bot**. 

    ![Web app-bot als resourcetype selecteren](./media/luis-tutorial-cscharp-web-bot/bot-service-selection.png)

3. Klik in het venster Web App-Bot **maken**.

4. In **Bot Service**, geef de vereiste gegevens op en klik op **maken**. Hiermee maakt en implementeert de botservice en LUIS-app in Azure. Als u wilt gebruiken [spraak voorbereiden](https://docs.microsoft.com/bot-framework/bot-service-manage-speech-priming), Bekijk [regio vereisten](troubleshooting.md#what-luis-regions-support-bot-framework-speech-priming) voordat het maken van uw bot. 
   * Stel **appnaam** op de naam van uw bot. De naam wordt gebruikt als het subdomein wanneer uw bot wordt geïmplementeerd naar de cloud (bijvoorbeeld mynotesbot.azurewebsites.net). <!-- This name is also used as the name of the LUIS app associated with your bot. Copy it to use later, to find the LUIS app associated with the bot. -->
   * Selecteer het abonnement [resourcegroep](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview), App service-plan en [locatie](https://azure.microsoft.com/regions/).
   * Voor **Bot sjabloon**selecteert:
       * **SDK v3**
       * **C#**
       * **Taal begrijpen**
   * Selecteer de **LUIS-App locatie**. Dit is het ontwerpen van [regio](luis-reference-regions.md) in de app is gemaakt.
   * Selecteer het selectievakje bevestiging voor de juridische kennisgeving. De voorwaarden van de juridische kennisgeving staan hieronder het selectievakje in.

     ![Bot-service](./media/luis-tutorial-cscharp-web-bot/bot-service-setting-callout-template.png)


5. Bevestig dat de botservice is geïmplementeerd.
    * Klik op meldingen (het belpictogram die zich aan de bovenkant van de Azure-portal). De melding wijzigt van **implementatie is gestart** naar **implementatie is voltooid**.
    * Nadat de melding wordt gewijzigd in **implementatie is voltooid**, klikt u op **naar de resource gaan** op waarmee de melding.

> [!Note]
> Dit proces voor het maken van web-app-bot wordt ook een nieuwe LUIS-app voor u gemaakt. Het is getraind en gepubliceerd voor u. 

## <a name="try-the-default-bot"></a>Probeer de standaard-bot

Bevestig dat de bot is geïmplementeerd door het selecteren van de **meldingen** selectievakje. De meldingen wordt gewijzigd van **implementatie wordt uitgevoerd...**  naar **implementatie is voltooid**. Klik op **naar de resource gaan** knop voor het openen van de bot-resources.

Nadat de bot wordt geïmplementeerd, klikt u op **Test in Web Chat** om de Web Chat-deelvenster te openen. Typ "Hallo" in Web Chat.

  ![De bot in Web Chat testen](./media/luis-tutorial-cscharp-web-bot/bot-service-web-chat.png)

De bot reageert door te spreken 'u hebt begroeting bereikt. U zegt: hello '.  Dit antwoord wordt bevestigd dat de bot heeft ontvangen van uw bericht en doorgegeven aan een LUIS-app dat is gemaakt van de standaardinstelling. Deze standaardinstelling LUIS-app kunt u lezen wat een begroeting gedetecteerd. In de volgende stap maakt u verbinding met de bot de LUIS-app die u eerder hebt gemaakt in plaats van de standaard LUIS-app.

## <a name="connect-your-luis-app-to-the-bot"></a>Uw LUIS-app verbinden met de bot

Open **toepassingsinstellingen** en bewerk de **LuisAppId** veld bevat de toepassings-ID van uw LUIS-app. Als u uw HomeAutomation LUIS-app in een andere regio dan VS-West gemaakt, moet u wijzigen de **LuisAPIHostName** ook. De **LuisAPIKey** is momenteel ingesteld op de sleutel van uw ontwerpen. U wijzigen dit naar de eindpuntsleutel van uw wanneer uw verkeer het quotum voor de gratis laag overschrijdt. 

  ![Werk de LUIS-app-ID in Azure](./media/luis-tutorial-cscharp-web-bot/bot-service-app-settings.png)

> [!Note]
> Als u geen de LUIS-app-ID van de [start Automation app](luis-get-started-create-app.md), meld u aan bij de [LUIS](luis-reference-regions.md) website met hetzelfde account als u zich aanmeldt bij Azure. 
> 1. Klik op **mijn apps**. 
> 2. De LUIS-app die u eerder hebt gemaakt, waarin de intenties en entiteiten uit het domein HomeAutomation vinden.
> 3. In de **instellingen** pagina voor de app LUIS, zoeken en kopiëren van de app-ID. Zorg ervoor dat dit [getrainde](luis-interactive-test.md) en [gepubliceerd](luis-how-to-publish-app.md). 
> 
> [!WARNING]
> Als u uw app-ID of een LUIS-sleutel verwijdert, wordt de bot werken niet meer.

## <a name="modify-the-bot-code"></a>De bot-code wijzigen

1. Klik op **bouwen** en klik vervolgens op **online code-editor openen**.

   ![Online code-editor openen](./media/luis-tutorial-cscharp-web-bot/bot-service-build.png)

2. Klik met de rechtermuisknop `build.cmd` en kies **uitvoeren vanaf de Console** om de app te bouwen. Er zijn meerdere build-stappen die de service automatisch voor u uitgevoerd. De bewerking is voltooid wanneer deze is voltooid met 'Is voltooid'.

3. Open in de code-editor `/Dialogs/BasicLuisDialog.cs`. Het bevat de volgende code:

   [!code-csharp[Default BasicLuisDialog.cs](~/samples-luis/documentation-samples/tutorial-web-app-bot/csharp/Default_BasicLuisDialog.cs "Default BasicLuisDialog.cs")]

## <a name="change-code-to-homeautomation-intents"></a>Code HomeAutomation intents wijzigen


1. Verwijderen van de drie intentie kenmerken en -methoden voor **begroeting**, **annuleren**, en **Help**. Deze intents worden niet gebruikt in de vooraf gedefinieerde HomeAutomation-domein. Zorg ervoor dat de **geen** intentie kenmerk en de methode. 

2. Afhankelijkheden toevoegen aan het begin van het bestand, met de andere afhankelijkheden:

   [!code-csharp[Dependencies](~/samples-luis/documentation-samples/tutorial-web-app-bot/csharp/BasicLuisDialog.cs?range=4-5&dedent=8 "dependencies")]

3. Voeg constanten toe voor het beheren van tekenreeksen aan de bovenkant van de `BasicLuisDialog` klasse:

   [!code-csharp[Add Intent and Entity Constants](~/samples-luis/documentation-samples/tutorial-web-app-bot/csharp/BasicLuisDialog.cs?range=23-32&dedent=8 "Add Intent and Entity Constants")]

4. De code voor de nieuwe intenties van `HomeAutomation.TurnOn` en `HomeAutomation.TurnOff` binnen de `BasicLuisDialog` klasse:

   [!code-csharp[Add Intents](~/samples-luis/documentation-samples/tutorial-web-app-bot/csharp/BasicLuisDialog.cs?range=61-71&dedent=8 "Add Intents")]

5. Voeg de code om op te halen van alle entiteiten vinden door LUIS binnen de `BasicLuisDialog` klasse:

   [!code-csharp[Collect entities](~/samples-luis/documentation-samples/tutorial-web-app-bot/csharp/BasicLuisDialog.cs?range=34-53&dedent=8 "Collect entities")]

6. Wijziging **ShowLuisResult** methode in de `BasicLuisDialog` klasse die u wilt de score afgerond, verzamelen van de entiteiten en het antwoordbericht dat wordt weergegeven in de chatbot:

   [!code-csharp[Display message in chatbot](~/samples-luis/documentation-samples/tutorial-web-app-bot/csharp/BasicLuisDialog.cs?range=73-83&dedent=8 "Display message in chatbot")]

## <a name="build-the-bot"></a>De bot bouwen
In de code-editor met de rechtermuisknop op `build.cmd` en selecteer **uitvoeren vanaf de Console**.

![Bouw Web-bot](./media/luis-tutorial-cscharp-web-bot/bot-service-build-run-from-console.png)

De weergave van de code wordt vervangen door een terminalvenster waarin de voortgang en resultaten van de build.

![Bouw Web-bot geslaagd](./media/luis-tutorial-cscharp-web-bot/bot-service-build-success.png)

> [!TIP]
> Een alternatieve methode voor het bouwen van de bot is de botnaam van de selecteren in de bovenste blauwe balk en selecteer **Kudu-Console openen**. De console wordt geopend op **D:\home**. 
> 
> Wijzig de map in **D:\home\site\wwwroot** door te typen: `cd site\wwwroot`
>
> Het build-script uitvoeren door te typen: `build.cmd`

## <a name="test-the-bot"></a>De bot testen

Klik in de Azure-portal op **testen in Web Chat** voor het testen van de bot. Typ berichten like "inschakelen het licht aan" en 'Mijn brandstof uitschakelen' om aan te roepen van de intenties die u hebt toegevoegd aan.

   ![HomeAutomation bot in Web Chat testen](./media/luis-tutorial-cscharp-web-bot/bot-service-chat-results.png)

> [!TIP]
> U kunt uw LUIS-app zonder wijzigingen in de code van uw bot opnieuw trainen. Zie [voorbeeld utterances toevoegen](https://docs.microsoft.com/azure/cognitive-services/LUIS/add-example-utterances) en [trainen en testen van uw LUIS-app](https://docs.microsoft.com/azure/cognitive-services/LUIS/luis-interactive-test). 

## <a name="download-the-bot-to-debug"></a>De bot voor foutopsporing downloaden
Als uw bot niet werkt, het project hebt gedownload naar uw lokale computer en verder [foutopsporing](https://docs.microsoft.com/bot-framework/bot-service-debug-bot). 

## <a name="learn-more-about-bot-framework"></a>Meer informatie over Bot Framework
Meer informatie over [Bot Framework](https://dev.botframework.com/) en de [3.x](https://github.com/Microsoft/BotBuilder) en [4.x](https://github.com/Microsoft/botbuilder-dotnet) SDK's.

## <a name="next-steps"></a>Volgende stappen

Voeg de LUIS intents en Bot service dialoogvensters voor het verwerken van **Help**, **annuleren**, en **begroeting** intents. Houd er rekening mee te trainen, publiceren en [bouwen](#build-the-bot) de web-app-bot. LUIS zowel de bot moet hebben de dezelfde intents.

Bekijk meer [voorbeelden](https://github.com/Microsoft/AI) met gespreksbots. 

> [!div class="nextstepaction"]
> [Intents toevoegen](./luis-how-to-add-intents.md)
> [spraak voorbereiden](https://docs.microsoft.com/bot-framework/bot-service-manage-speech-priming)

<!-- tested on Win10 -->
