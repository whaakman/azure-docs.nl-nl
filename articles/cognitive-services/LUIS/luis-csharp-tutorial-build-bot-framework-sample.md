---
title: LUIS integreren met een bot met de SDK van de opbouwfunctie voor Bot voor C# in Azure | Microsoft Docs
description: Een geïntegreerd met een LUIS-toepassing met behulp van het Framework Bot bot bouwen.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/06/2018
ms.author: v-geberr
ms.openlocfilehash: 3db2588edefa2f07d22609d7c454367d5572d5e0
ms.sourcegitcommit: b7290b2cede85db346bb88fe3a5b3b316620808d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/05/2018
ms.locfileid: "35345974"
---
# <a name="web-app-bot-using-the-luis-template-for-c"></a>Web-App Bot met behulp van de sjabloon LUIS voor C#

Een chatbot met geïntegreerde language understanding bouwen.

## <a name="prerequisite"></a>Vereiste

* [HomeAutomation LUIS app](luis-get-started-create-app.md). De intents uit deze app LUIS toewijzing aan de bot dialoogvenster handlers. 

## <a name="luis-homeautomation-intents"></a>LUIS HomeAutomation Intents

| Doel | Voorbeeld utterance | Bot-functionaliteit |
|:----:|:----------:|---|
| HomeAutomation.TurnOn | Schakel de lichten. | Wanneer de bedoeling LUIS `HomeAutomation.TurnOn` wordt gedetecteerd, wordt de bot roept de `OnIntent` dialoogvenster handler. Dit dialoogvenster is waar roept u een IoT-service om te schakelen op een apparaat en laat de gebruiker die het apparaat is ingeschakeld. |
| HomeAutomation.TurnOff | De kamer uw lichten uitschakelen. | Wanneer de bedoeling LUIS `HomeAutomation.TurnOff` wordt gedetecteerd, wordt de bot roept de `OffIntent` dialoogvenster handler. Dit dialoogvenster is waar roept u een IoT-service een apparaat uitschakelen en de gebruiker melden dat het apparaat is uitgeschakeld. |

## <a name="create-a-language-understanding-bot-with-bot-service"></a>Language Understanding bot's maken met Bot-Service

1. In de [Azure-portal](https://portal.azure.com), selecteer **Maak nieuwe resource** in het bovenste menu links.

    ![Nieuwe resource maken](./media/luis-tutorial-cscharp-web-bot/bot-service-creation.png)

2. Zoek in het zoekvak op **Web App Bot**. 

    ![Nieuwe resource maken](./media/luis-tutorial-cscharp-web-bot/bot-service-selection.png)

3. Klik in het venster Web App Bot **maken**.

4. In **Bot Service**, geef de vereiste gegevens en klik op **maken**. Hiermee maakt en implementeert de bot-service en LUIS app in Azure. Als u wilt gebruiken [spraak voorbereiden](https://docs.microsoft.com/bot-framework/bot-service-manage-speech-priming), Bekijk [regio vereisten](luis-resources-faq.md#what-luis-regions-support-bot-framework-speech-priming) voordat u uw bot maakt. 
    * Stel **appnaam** aan uw bot-naam. De naam wordt gebruikt als het subdomein wanneer uw bot wordt geïmplementeerd naar de cloud (bijvoorbeeld mynotesbot.azurewebsites.net). <!-- This name is also used as the name of the LUIS app associated with your bot. Copy it to use later, to find the LUIS app associated with the bot. -->
    * Selecteer het abonnement [resourcegroep](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview), App service-abonnement en [locatie](https://azure.microsoft.com/regions/).
    * Selecteer de **Language understanding (C#)** sjabloon voor de **Bot sjabloon** veld.
    * Selecteer de **LUIS App locatie**. Dit is het ontwerp [regio] [ LUIS] in de app wordt gemaakt.
    * Schakel het selectievakje bevestiging voor de juridische mededeling. De voorwaarden van de juridische kennisgeving zijn lager dan het selectievakje in.

    ![Bot Service](./media/luis-tutorial-cscharp-web-bot/bot-service-setting-callout-template.png)


5. Bevestig dat de bot-service is geïmplementeerd.
    * Klik op meldingen (het belpictogram bevindt zich aan de bovenkant van de Azure portal). De melding wordt gewijzigd van **implementatie gestart** naar **implementatie is voltooid**.
    * Nadat de melding wijzigingen in **implementatie is voltooid**, klikt u op **gaat u naar de resource** op deze melding.

> [!Note]
> Deze web-app bot maakproces ook een nieuwe LUIS-app voor u gemaakt. Het is getraind en gepubliceerd voor u. 

## <a name="try-the-default-bot"></a>Probeer de standaard-bot

Bevestig dat de bot is geïmplementeerd door het selecteren van de **meldingen** selectievakje. De meldingen wordt gewijzigd van **implementatie wordt uitgevoerd...**  naar **implementatie is voltooid**. Klik op **gaat u naar de resource** knop waarmee de bot resources wordt geopend.

Zodra de bot is geïmplementeerd, klikt u op **testen in de Web chatten** om het deelvenster Web chatten te openen. Typ in het Web chatten "hello".

  ![De bot in Web chatten testen](./media/luis-tutorial-cscharp-web-bot/bot-service-web-chat.png)

De bot reageert door in te spreken 'u kunt begroeting hebt bereikt. U gezegd: Hallo ".  Dit antwoord wordt bevestigd dat de bot heeft ontvangen van uw bericht en doorgegeven aan op een standaardwaarde LUIS app dat is gemaakt. Deze standaardinstelling LUIS app een bedoeling begroeting gedetecteerd. In de volgende stap maakt u verbinding met de bot de LUIS-app die u eerder hebt gemaakt in plaats van de standaard LUIS app.

## <a name="connect-your-luis-app-to-the-bot"></a>Uw app LUIS verbinden met de bot

Open **toepassingsinstellingen** en bewerkt u de **LuisAppId** veld de toepassings-ID van uw app LUIS bevatten. Als u uw app HomeAutomation LUIS in een andere regio dan VS-West gemaakt, moet u wijzigt de **LuisAPIHostName** ook. De **LuisAPIKey** is momenteel ingesteld op de sleutel van uw ontwerp. U wijzigen dit in de abonnementssleutel van uw wanneer het verkeer het quotum voor gratis laag overschrijdt. 

  ![Bijwerken van de ID van de app LUIS in Azure](./media/luis-tutorial-cscharp-web-bot/bot-service-app-settings.png)

> [!Note]
> Als u geen de LUIS app-ID van de [start Automation app](luis-get-started-create-app.md), meld u aan bij de [LUIS](luis-reference-regions.md) website met dezelfde account die u gebruikt voor aanmelding bij Azure. 
> 1. Klik op **mijn apps**. 
> 2. De LUIS-app die u eerder hebt gemaakt met de intents en entiteiten van het domein HomeAutomation te zoeken.
> 3. In de **instellingen** pagina voor de app LUIS, vinden en kopiëren van de app-ID. Zorg ervoor dat dit [getraind](Train-Test.md) en [gepubliceerd](PublishApp.md). 

    > [!WARNING]
    > If you delete your app ID or LUIS key, the bot will stop working.

## <a name="modify-the-bot-code"></a>Wijzig de bot-code

1. Klik op **bouwen** en klik vervolgens op **online code-editor openen**.

   ![Open online code-editor](./media/luis-tutorial-cscharp-web-bot/bot-service-build.png)

2. Klik met de rechtermuisknop `build.cmd` en kies **uitvoeren vanaf de Console** om de app te bouwen. Er zijn verschillende build stappen die de service is automatisch voltooid voor u. De build is voltooid zodra deze is voltooid met 'Is voltooid'.

3. Open in de code-editor `/Dialogs/BasicLuisDialog.cs`. Bevat de volgende code:

   [!code-csharp[Default BasicLuisDialog.cs](~/samples-luis/documentation-samples/tutorial-web-app-bot/csharp/Default_BasicLuisDialog.cs "Default BasicLuisDialog.cs")]

## <a name="change-code-to-homeautomation-intents"></a>Code HomeAutomation intents wijzigen


1. Verwijder de drie opzet kenmerken en methoden voor het **begroeting**, **annuleren**, en **Help**. Deze intents worden niet gebruikt in het vooraf gedefinieerde HomeAutomation-domein. Zorg ervoor dat de **geen** opzet kenmerk en de methode. 

2. Afhankelijkheden toevoegen aan de bovenkant van het bestand, met de andere afhankelijkheden:

   [!code-csharp[Dependencies](~/samples-luis/documentation-samples/tutorial-web-app-bot/csharp/BasicLuisDialog.cs?range=4-5&dedent=8 "dependencies")]

3. Toevoegen van constanten voor het beheren van tekenreeksen aan de bovenkant van de `BasicLuisDialog ` klasse:

   [!code-csharp[Add Intent and Entity Constants](~/samples-luis/documentation-samples/tutorial-web-app-bot/csharp/BasicLuisDialog.cs?range=23-32&dedent=8 "Add Intent and Entity Constants")]

4. Voeg de code voor de nieuwe intentie van `HomeAutomation.TurnOn` en `HomeAutomation.TurnOff` binnen de `BasicLuisDialog ` klasse:

   [!code-csharp[Add Intents](~/samples-luis/documentation-samples/tutorial-web-app-bot/csharp/BasicLuisDialog.cs?range=61-71&dedent=8 "Add Intents")]

5. Voeg de code voor het ophalen van de entiteiten die zijn gevonden door LUIS binnen de `BasicLuisDialog ` klasse:

   [!code-csharp[Collect entities](~/samples-luis/documentation-samples/tutorial-web-app-bot/csharp/BasicLuisDialog.cs?range=34-53&dedent=8 "Collect entities")]

6. Wijziging **ShowLuisResult** methode in de `BasicLuisDialog ` klasse voor het afronden van de score, de entiteiten verzamelen en weergeven van het antwoordbericht in de chatbot:

   [!code-csharp[Display message in chatbot](~/samples-luis/documentation-samples/tutorial-web-app-bot/csharp/BasicLuisDialog.cs?range=73-83&dedent=8 "Display message in chatbot")]

## <a name="build-the-bot"></a>De bot bouwen
In de code-editor met de rechtermuisknop op `build.cmd` en selecteer **uitvoeren vanaf de Console**.

![Web-bot bouwen ](./media/luis-tutorial-cscharp-web-bot/bot-service-build-run-from-console.png)

De codeweergave wordt vervangen door een terminalvenster waarin de voortgang en resultaten van de build.

![Web-bot opbouwen is voltooid](./media/luis-tutorial-cscharp-web-bot/bot-service-build-success.png)

> [!TIP]
> Een alternatieve methode voor het bouwen van de bot is de naam van de bot selecteren in de bovenste balk blauw en selecteer **Kudu-Console openen**. De console geopend voor **D:\home**. 
> 
> Wijzig de map in **D:\home\site\wwwroot** door te typen: `cd site\wwwroot`
>
> Voer het opbouwscript door te typen: `build.cmd`

## <a name="test-the-bot"></a>De bot testen

Klik in de Azure-portal op **testen in Web chatten** voor het testen van de bot. Typ berichten like "inschakelen het licht' en 'Mijn verwarmingssysteem uitschakelen' om aan te roepen de intents die u hebt toegevoegd aan.

   ![HomeAutomation bot in Web chatten testen](./media/luis-tutorial-cscharp-web-bot/bot-service-chat-results.png)

> [!TIP]
> U kunt uw app LUIS zonder dat aanpassingen uw bot code opnieuw trainen. Zie [voorbeeld utterances toevoegen](https://docs.microsoft.com/azure/cognitive-services/LUIS/add-example-utterances) en [trainen en te testen van uw app LUIS](https://docs.microsoft.com/azure/cognitive-services/LUIS/train-test). 

## <a name="download-the-bot-to-debug"></a>Download de bot voor foutopsporing
Als uw bot niet werkt, downloadt u het project naar uw lokale computer en doorgaan [foutopsporing](https://docs.microsoft.com/bot-framework/bot-service-debug-bot#debug-an-azure-app-service-web-app-c-bot). 

## <a name="learn-more-about-bot-framework"></a>Meer informatie over Bot-Framework
Meer informatie over [Bot Framework](https://dev.botframework.com/) en de [3.x](https://github.com/Microsoft/BotBuilder) en [4.x](https://github.com/Microsoft/botbuilder-dotnet) SDK's.

## <a name="next-steps"></a>Volgende stappen

Voeg de LUIS intents en Bot service dialoogvensters voor het verwerken van **Help**, **annuleren**, en **begroeting** intents. Vergeet niet te trainen, publiceren en [bouwen](#build-the-bot) de WebBot-app. LUIS zowel de bot moet het dezelfde intents hebben.

> [!div class="nextstepaction"]
> [Toevoegen van intents](./luis-how-to-add-intents.md)
> [spraak voorbereiden](https://docs.microsoft.com/bot-framework/bot-service-manage-speech-priming)
<!-- Links -->
[Github-BotFramework-Emulator-Download]: https://aka.ms/bot-framework-emulator
[Github-LUIS-Samples]: https://github.com/Microsoft/LUIS-Samples
[Github-LUIS-Samples-cs-hotel-bot]: https://github.com/Microsoft/LUIS-Samples/tree/master/bot-integration-samples/hotel-finder/csharp
[Github-LUIS-Samples-cs-hotel-bot-readme]: https://github.com/Microsoft/LUIS-Samples/blob/master/bot-integration-samples/hotel-finder/csharp/README.md
[BFPortal]: https://dev.botframework.com/
[RegisterInstructions]: https://docs.microsoft.com/bot-framework/portal-register-bot
[BotFramework]: https://docs.microsoft.com/bot-framework/
[AssignedEndpointDoc]:https://docs.microsoft.com/azure/cognitive-services/LUIS/manage-keys
[VisualStudio]: https://www.visualstudio.com/
[LUIS]:luis-reference-regions.md
<!-- tested on Win10 -->
