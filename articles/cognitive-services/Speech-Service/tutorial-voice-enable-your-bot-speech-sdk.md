---
title: 'Zelfstudie: Stemmen uw bot in te scha kelen met behulp van Speech SDK'
titleSuffix: Azure Cognitive Services
description: In deze zelf studie maakt u een echo bot met behulp van micro soft bot-Framework, implementeert u deze in Azure en registreert u deze met het bot-Framework direct line speech Channel. Vervolgens configureert u een voor beeld-client-app voor Windows waarmee u naar uw bot kunt praten en kunt horen dat deze wordt weer gegeven.
services: cognitive-services
author: dargilco
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 08/06/2019
ms.author: dcohen
ms.openlocfilehash: 3c57200591f3b7de9a1f9ab4198e55ed844b4d07
ms.sourcegitcommit: 13a289ba57cfae728831e6d38b7f82dae165e59d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/09/2019
ms.locfileid: "68932059"
---
# <a name="tutorial-voice-enable-your-bot-using-the-speech-sdk"></a>Zelfstudie: Spraak: uw bot inschakelen met de Speech SDK

U kunt nu de kracht van de spraak Services gebruiken om snel een chat-bot in te scha kelen.

In deze zelf studie maakt u een echo bot met behulp van micro soft bot-Framework, implementeert u deze in Azure en registreert u deze met het bot-Framework direct line speech Channel. Vervolgens configureert u een voor beeld-client-app voor Windows waarmee u naar uw bot kunt praten en kunt horen dat deze wordt weer gegeven.

Deze zelf studie is ontworpen voor ontwikkel aars die gewoon hun reis starten met Azure, bot-Framework-bots, direct line speech of de Speech SDK, en snel een werkend systeem bouwen met beperkte code ring. Er is geen ervaring of kennis van deze services nodig.

Aan het einde van deze oefening hebt u een systeem ingesteld dat er als volgt uitziet:

1. De voor beeld-client toepassing is geconfigureerd om verbinding te maken met direct line speech Channel en de echo bot
2. Audio wordt van de standaard microfoon op de knop ingedrukt (of doorlopend vastgelegd als er een aangepast Ontwaak woord wordt geactiveerd)
3. De aangepaste detectie van de Wake-Word gebeurt optioneel, beperking audio streaming naar de Cloud
4. Met behulp van Speech SDK maakt de app verbinding met het directe lijn spraak kanaal en streams-audio
5. Eventueel een hogere nauw keurigheid van de Wake-Word-verificatie gebeurt op de service
6. De audio wordt door gegeven aan de service voor spraak herkenning en naar tekst getranscribeerd
7. De herkende tekst wordt door gegeven aan de echo-bot als een bot Framework-activiteit 
8. De antwoord tekst wordt in audio omgezet door de TTS-Service (tekst-naar-spraak) en weer gestreamd naar de client toepassing voor afspelen

![diagram-tag](media/tutorial-voice-enable-your-bot-speech-sdk/diagram.png "De stroom van het spraak kanaal")

> [!NOTE]
> Voor de stappen in deze zelf studie is geen betaalde service vereist. Als nieuwe Azure-gebruiker kunt u tegoed gebruiken van uw gratis Azure Trail-abonnement en de gratis laag van spraak Services om deze zelf studie te volt ooien.

Deze zelf studie bevat de volgende onderwerpen:
> [!div class="checklist"]
> * Nieuwe Azure-resources maken
> * Het voor beeld van de echo-bot bouwen, testen en implementeren op een Azure App Service
> * Uw bot registreren met een direct-lijn spraak kanaal
> * Bouwen en uitvoeren van de direct-line speech-client om te communiceren met uw ECHO bot
> * Aangepaste Wake Word-activering toevoegen
> * Meer informatie over het wijzigen van de taal van de herkende en gesp roken spraak

## <a name="prerequisites"></a>Vereisten

Dit is wat u moet doen om deze zelf studie te volt ooien:

- Een Windows 10-PC met een werkende microfoon en luid sprekers (of hoofd telefoon)
- [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) of hoger
- [.Net core SDK](https://dotnet.microsoft.com/download) versie 2,1 of hoger
- Een Azure-account. [Meld u gratis aan](https://azure.microsoft.com/free/ai/).
- Een [github](https://github.com/) -account
- [Git voor Windows](https://git-scm.com/download/win)

## <a name="create-a-resource-group"></a>Een resourcegroep maken

De client-app die u in deze zelf studie maakt, maakt gebruik van een aantal Azure-Services. Als u de retour tijd voor reacties van uw bot wilt beperken, moet u ervoor zorgen dat deze services zich in dezelfde Azure-regio bevinden. In deze sectie maakt u een resource groep in de regio **VS-West** . Deze resource groep wordt gebruikt bij het maken van afzonderlijke resources voor het bot-Framework, het direct lijn spraak kanaal en spraak Services.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer **resource groepen**in de linkernavigatiebalk. Klik vervolgens op **toevoegen** om een nieuwe resource groep toe te voegen.
3. U wordt gevraagd om de volgende informatie op te geven:
   * Stel **abonnement** in op **gratis proef versie** (u kunt ook een bestaand abonnement gebruiken).
   * Voer een naam in voor de **resource groep**. We raden **SpeechEchoBotTutorial-ResourceGroup**aan.
   * Selecteer in de vervolg keuzelijst **regio** **VS West**.
4. Klik op **controleren en maken**. U ziet een banner waarvoor Lees **validatie is geslaagd**.
5. Klik op **Create**. Het kan enkele minuten duren om de resource groep te maken.
6. Net als bij de resources die u later in deze zelf studie maakt, is het een goed idee om deze resource groep vast te maken aan uw dash board voor eenvoudige toegang. Als u deze resource groep wilt vastmaken, klikt u op het speld pictogram in de rechter bovenhoek van het dash board.

### <a name="choosing-an-azure-region"></a>Een Azure-regio kiezen

Als u voor deze zelf studie een andere regio wilt gebruiken, kunnen de volgende factoren uw keuzes beperken:

* Het directe lijn spraak kanaal is een preview-service. Dit kan worden beperkt tot specifieke Azure-regio's. Zie voor meer informatie over beschik bare regio's [spraak-eerste virtuele assistenten](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#voice-first-virtual-assistants).
* Het directe lijn spraak kanaal maakt gebruik van de tekst-naar-spraak-service, die standaard en Neural stemmen heeft. Neural stemmen zijn [beperkt tot specifieke Azure-regio's](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#standard-and-neural-voices).
* Sleutels voor een gratis proef versie kunnen worden beperkt tot een specifieke regio.

Zie [Azure-locaties](https://azure.microsoft.com/global-infrastructure/locations/)voor meer informatie over regio's.

## <a name="create-resources"></a>Resources maken

Nu u een resource groep hebt in de regio **VS-West** , is de volgende stap het maken van afzonderlijke resources voor elke service die u in deze zelf studie gaat gebruiken.

### <a name="create-a-speech-services-resource"></a>Een speech Services-resource maken

Volg deze instructies om een spraak bron te maken:

1. Selecteer **een resource maken** in het linkernavigatievenster.
2. Typ **spraak**in de zoek balk.
3. Selecteer **spraak**en klik vervolgens op **maken**.
4. U wordt gevraagd om de volgende informatie op te geven:
   * Geef uw resource een **naam**. U kunt **het beste SpeechEchoBotTutorial-speech**
   * Zorg ervoor dat een **gratis proef versie** is geselecteerd voor het **abonnement**.
   * Selecteer voor **locatie**de optie **VS-West**.
   * Selecteer voor **prijs categorie**de optie **F0**. Dit is de gratis laag.
   * Voor **resource groep**selecteert u **SpeechEchoBotTutorial-ResourceGroup**.
5. Nadat u alle vereiste gegevens hebt ingevoerd, klikt u op **maken**. Het kan een paar minuten duren voordat de resource is gemaakt.
6. Verderop in deze zelf studie hebt u abonnements sleutels voor deze service nodig. U kunt deze sleutels op elk gewenst moment openen vanuit het **overzicht** van uw resource (sleutels beheren) of **sleutels**.

Controleer op dit punt of de resource groep (**SpeechEchoBotTutorial-ResourceGroup**) een spraak bron heeft:

| NAAM | TYPE  | LOCATIE |
|------|-------|----------|
| SpeechEchoBotTutorial-speech | Cognitive Services | US - west |

### <a name="create-an-azure-app-service-plan"></a>Een Azure App Service-plan maken

De volgende stap is het maken van een App Service-abonnement. Een App Service-plan definieert een set rekenresources waarmee een web-app kan worden uitgevoerd.

1. Selecteer **een resource maken** in het linkernavigatievenster.
2. Typ **app service plan**op de zoek balk. Zoek en selecteer vervolgens de kaart **app service plan** in de zoek resultaten.
3. Klik op **Create**.
4. U wordt gevraagd om de volgende informatie op te geven:
   * Stel **abonnement** in op **gratis proef versie** (u kunt ook een bestaand abonnement gebruiken).
   * Voor **resource groep**selecteert u **SpeechEchoBotTutorial-ResourceGroup**.
   * Geef uw resource een **naam**. We raden **SpeechEchoBotTutorial-AppServicePlan** aan
   * Selecteer voor **besturings systeem** **Windows**.
   * Selecteer voor **regio** **VS-West**.
   * Zorg ervoor dat **standaard S1** is geselecteerd voor de **prijs categorie**. Dit moet de standaard waarde zijn.
5. Klik op **controleren en maken**. U ziet een banner waarvoor Lees **validatie is geslaagd**.
6. Klik op **Create**. Het kan enkele minuten duren om de resource groep te maken.

Controleer op dit punt of de resource groep (**SpeechEchoBotTutorial-ResourceGroup**) twee resources heeft:

| NAAM | TYPE  | LOCATIE |
|------|-------|----------|
| SpeechEchoBotTutorial-AppServicePlan | App Service-abonnement | US - west |
| SpeechEchoBotTutorial-speech | Cognitive Services | US - west |

## <a name="build-an-echo-bot"></a>Een echo bot bouwen

Nu u enkele resources hebt gemaakt, gaan we een bot bouwen. We beginnen met het voor beeld van de echo bot, zoals de naam al aangeeft, ECHO de tekst die u hebt ingevoerd als antwoord. U hoeft zich geen zorgen te maken dat de voorbeeld code klaar is om zonder wijzigingen te worden gebruikt. Het is geconfigureerd om te werken met het directe-lijn spraak kanaal dat we gaan verbinden nadat we de bot hebben geïmplementeerd in Azure.

> [!NOTE]
> De volgende instructies, evenals aanvullende informatie over de echo-bot, zijn beschikbaar in het Leesmij-bestand van het [voor beeld op github](https://github.com/microsoft/BotBuilder-Samples/blob/master/experimental/directline-speech/csharp_dotnetcore/02.echo-bot/README.md).

### <a name="download-and-run-the-sample"></a>Het voorbeeld downloaden en uitvoeren

1. De opslag plaats voor beelden klonen:
   ```bash
   git clone https://github.com/Microsoft/botbuilder-samples.git
   ```
2. Start Visual Studio.
3. Selecteer in de werk balk **File** > **Open** > **Project/Solution**en open het project bestand van de echo bot die is geconfigureerd voor gebruik met een direct-lijn spraak kanaal:
   ```
   experimental\directline-speech\csharp_dotnetcore\02.echo-bot\EchoBot.csproj
   ```
4. Wanneer het project is geladen, drukt `F5` u op om het project uit te voeren.

### <a name="test-with-the-bot-framework-emulator"></a>Testen met de bot Framework emulator

De [bot Framework-emulator](https://github.com/microsoft/botframework-emulator) is een bureau blad-app waarmee bot-ontwikkel aars hun bots lokaal of op afstand kunnen testen en door lopen via een tunnel. Volg deze stappen om de bot Framework emulator te gebruiken om uw bot te testen.

1. De [bot Framework-emulator](https://github.com/Microsoft/BotFramework-Emulator/releases/latest) versie 4.3.0 of hoger installeren
2. Start de bot Framework emulator en open uw bot:
   *  -> **Geopende bot**bestand.
3. Voer de URL in voor uw bot. Bijvoorbeeld:
   ```
   http://localhost:3978/api/messages
   ```
4. Gebruik de gebruikers interface om met uw bot te communiceren met getypte tekst. Bevestig dat u een reactie krijgt.


## <a name="deploy-your-bot-to-an-azure-app-service"></a>Uw bot implementeren op een Azure App Service

De volgende stap is het implementeren van de echo-bot in Azure. Er zijn enkele manieren om een bot te implementeren, maar in deze zelf studie wordt u geadviseerd om rechtstreeks vanuit Visual Studio te publiceren.

> [!NOTE]
> U kunt ook een bot implementeren met behulp van de [Azure cli](https://docs.microsoft.com/azure/bot-service/bot-builder-deploy-az-cli) -en [implementatie sjablonen](https://github.com/microsoft/BotBuilder-Samples/tree/master/experimental/directline-speech/csharp_dotnetcore/02.echo-bot/DeploymentTemplates).

1. Open in Visual Studio de echo-bot die is geconfigureerd voor gebruik met een direct-lijn spraak kanaal:
   ```
   experimental\directline-speech\csharp_dotnetcore\02.echo-bot\EchoBot.csproj
   ```
2. Klik in de **Solution Explorer**met de rechter muisknop op de oplossing **EchoBot** en selecteer **publiceren...**
3. Er wordt een nieuw venster met **de titel Kies een publicatie doel** geopend.
3. Selecteer **app service** in de linkernavigatiebalk, selecteer **nieuwe maken**en klik vervolgens op **publiceren**.
5. Wanneer het venster **app service maken** wordt weer gegeven:
   * Klik op **een account toevoegen**en meld u aan met de referenties van uw Azure-account. Als u al bent aangemeld, selecteert u het gewenste account in de vervolg keuzelijst.
   * Voor de **naam**van de app moet u een wereld wijd unieke naam invoeren voor uw bot. Deze naam wordt gebruikt om een unieke bot-URL te maken. Er wordt een standaard waarde ingevuld, inclusief de datum en tijd (bijvoorbeeld: "EchoBot20190805125647"). U kunt de standaard naam voor deze zelf studie gebruiken.
   * Voor het **abonnement**stelt u deze in op **gratis proef versie**
   * Voor **resource groep**selecteert u **SpeechEchoBotTutorial-ResourceGroup**
   * Voor **hosting plan**selecteert u **SpeechEchoBotTutorial-AppServicePlan**
6. Klik op **Maken**
7. Er wordt een bericht weer gegeven in Visual Studio dat er als volgt uitziet:
   ```
   Publish Succeeded.
   Web App was published successfully https://EchoBot20190805125647.azurewebsites.net/
   ```
8. De standaard browser moet een pagina openen en weer geven met de volgende strekking: ' Uw bot is klaar! '.
9. Controleer op dit punt de resource groep **SpeechEchoBotTutorial-ResourceGroup** in de Azure Portal en bevestig dat er drie resources zijn:

| NAAM | TYPE  | LOCATIE |
|------|-------|----------|
| EchoBot20190805125647 | App Service | US - west |
| SpeechEchoBotTutorial-AppServicePlan | App Service-plan | US - west |
| SpeechEchoBotTutorial-speech | Cognitive Services | US - west |

## <a name="enable-web-sockets"></a>Websockets inschakelen

U moet een kleine configuratie wijziging aanbrengen zodat uw bot met behulp van websockets met het directe lijn spraak kanaal kan communiceren. Volg deze stappen om Web sockets in te scha kelen:

1. Ga naar het [Azure Portal](https://portal.azure.com)en zoek uw app service. De resource moet een naam hebben die vergelijkbaar is met **EchoBot20190805125647** (uw unieke app-naam).
2. Selecteer in het navigatie venster links **instellingen**en klik vervolgens op **configuratie**.
3. Selecteer het tabblad **algemene instellingen** .
4. Zoek de wissel knop voor **Web sockets** en stel deze **in op aan**.
5. Klik op **Opslaan**.

> [!TIP]
> U kunt de besturings elementen aan de bovenkant van de Azure App Service-pagina gebruiken om de service te stoppen of opnieuw te starten. Dit kan handig zijn bij het oplossen van problemen.

## <a name="create-a-channel-registration"></a>Een kanaal registratie maken

Nu u een Azure App Service hebt gemaakt om uw bot te hosten, is de volgende stap het maken van een **bot-kanaal registratie**. Het maken van een kanaal registratie is een vereiste voor het registreren van uw bot met bot-Framework kanalen, waaronder direct line speech Channel.

> [!NOTE]
> Zie [een bot verbinden met kanalen](https://docs.microsoft.com/azure/bot-service/bot-service-manage-channels?view=azure-bot-service-4.0)als u meer wilt weten over het gebruik van bots kanalen.

1. De eerste stap is het maken van een nieuwe resource voor de registratie. Klik in de [Azure Portal](https://portal.azure.com)op **een resource maken**.
2. Selecteer in de **bot**van het type zoek balk, nadat de resultaten worden weer gegeven, de **registratie bot-kanalen**.
3. Klik op **Create**.
4. U wordt gevraagd om de volgende informatie op te geven:
   * Voer voor **bot naam** **SpeechEchoBotTutorial-BotRegistration**in.
   * Selecteer voor **abonnement**een **gratis proef versie**.
   * Voor **resource groep**selecteert u **SpeechEchoBotTutorial-ResourceGroup**.
   * Selecteer voor **locatie**de optie **VS-West**.
     * Selecteer voor **prijs categorie**de optie **F0**.
     * Voor **berichten eind punt**voert u de URL voor uw web-app `/api/messages` in waarbij het pad aan het einde is toegevoegd. Als uw wereld wijde unieke app-naam bijvoorbeeld **EchoBot20190805125647**is, zou uw e-mail `https://EchoBot20190805125647.azurewebsites.net/api/messages/`eindpunt:.
     * Voor **Application Insights**kunt u dit instellen op **uit**. Zie [bot Analytics](https://docs.microsoft.com/azure/bot-service/bot-service-manage-analytics?view=azure-bot-service-4.0)voor meer informatie.
     * **Automatisch maken van app-id en wacht woord**negeren.
5. Ga terug naar de **registratie bot-kanalen** en klik op **maken**.

Controleer op dit punt de resource groep **SpeechEchoBotTutorial-ResourceGroup** in de Azure Portal. Er moeten nu vier resources worden weer gegeven:

| NAAM | TYPE  | LOCATIE |
|------|-------|----------|
| EchoBot20190805125647 | App Service | US - west |
| SpeechEchoBotTutorial-AppServicePlan | App Service-plan | US - west |
| SpeechEchoBotTutorial-BotRegistration | Registratie van bot-kanalen | Wereldwijd |
| SpeechEchoBotTutorial-speech | Cognitive Services | US - west |

> [!IMPORTANT]
> In de bron van de bot-kanalen wordt de wereld wijde regio weer gegeven, zelfs als u vs West hebt geselecteerd. Dit is normaal gedrag.

## <a name="register-the-direct-line-speech-channel"></a>Het directe-lijn spraak kanaal registreren

Nu is het tijd om uw bot te registreren met het directe-lijn spraak kanaal. Dit kanaal is wat wordt gebruikt om een verbinding tot stand te brengen tussen uw ECHO bot en een client-app die is gecompileerd met de spraak-SDK.

1. Zoek en open uw **SpeechEchoBotTutorial-BotRegistration-** resource in de [Azure Portal](https://portal.azure.com).
2. Selecteer in het navigatie venster aan de linkerkant de optie **kanalen**.
   * Zoek naar **meer kanalen**, zoek en klik op **direct line speech**.
   * Bekijk de tekst op de pagina met de titel **directe lijn spraak configureren (preview)** en klik vervolgens op **Opslaan**.
   * Als onderdeel van maken zijn er twee **geheime sleutels** gegenereerd. Deze sleutels zijn uniek voor uw bot. Wanneer u een client-app schrijft met behulp van de [Speech SDK](https://docs.microsoft.com/azure/cognitive-services/speech-service/), geeft u een van deze sleutels op om een verbinding tot stand te brengen tussen de client-app, het directe lijn spraak kanaal en uw bot-service. In deze zelf studie gebruikt u de direct line speech-client (WPF C#).
   * Klik op **weer geven** en kopieer een van de sleutels op een plek waar u deze eenvoudig kunt openen. Maakt u zich geen zorgen, u kunt altijd toegang krijgen tot de sleutels van de Azure Portal.
3. Klik op **instellingen**in het navigatie venster aan de linkerkant.
   * Schakel het selectie vakje **streaming-eind punt inschakelen**in het vak in. Dit is nodig voor het inschakelen van een communicatie protocol dat is gebaseerd op Web-sockets tussen uw bot en het directe lijn spraak kanaal.
   * Klik op **Opslaan**.

> [!TIP]
> Zie [een bot verbinden met direct line Speech (preview)](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-directlinespeech?view=azure-bot-service-4.0)als u meer wilt weten. Deze pagina bevat aanvullende informatie en bekende problemen.

## <a name="build-the-direct-line-speech-client"></a>De direct-line speech-client bouwen

In deze stap gaat u de direct-line speech-client bouwen. De client is een Windows Presentation Foundation-app (WPF) C# in die de [Speech SDK](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk) gebruikt voor het beheren van de communicatie met uw bot met behulp van het directe lijn spraak kanaal. Gebruik het om te communiceren met en test uw bot voordat u een aangepaste client-app schrijft.

De direct line speech-client beschikt over een eenvoudige gebruikers interface waarmee u de verbinding met uw bot kunt configureren, het tekst gesprek kunt bekijken, de activiteiten van bot-Framework weer geven in JSON-indeling en adaptieve kaarten weergeeft. Het biedt ook ondersteuning voor het gebruik van aangepaste Ontwaak woorden. U gebruikt deze client om met uw bot te praten en een antwoord op de telefoon te ontvangen.

Voordat we aan de slag gaan, moet u ervoor zorgen dat uw microfoon en luid sprekers zijn ingeschakeld en werken.

1. Navigeer naar de GitHub-opslag plaats voor de [direct line speech-client](https://github.com/Azure-Samples/Cognitive-Services-Direct-Line-Speech-Client/blob/master/README.md).
2. Volg de instructies voor het klonen van de opslag plaats, het bouwen van het project, het configureren van de client en het starten van de client.
3. Klik op **opnieuw verbinding maken** en zorg ervoor dat het bericht wordt weer gegeven **op de microfoon knop of typ om te praten met uw bot**.
4. Laten we dit testen. Klik op de knop microfoon en spreek enkele woorden in het Engels in. De herkende tekst wordt weer gegeven tijdens het spreken. Wanneer u klaar bent met spreken, reageert de bot op een eigen stem, met de tekst ' echo ' gevolgd door de herkende woorden.
5. U kunt ook tekst gebruiken om te communiceren met de bot. U hoeft alleen de tekst in de onderste balk te typen. 

### <a name="troubleshooting-errors-in-direct-line-speech-client"></a>Probleemoplossings fouten in client voor directe spraak

Als er een fout bericht wordt weer gegeven in het hoofd venster van de app, gebruikt u deze tabel om de fout te identificeren en op te lossen:

| Fout | Wat moet u doen? |
|-------|----------------------|
|App-fout (zie logboek voor details): Micro soft. CognitiveServices. speech. csharp: De waarde mag niet null zijn. Parameter naam: speechConfig | Dit is een client-app-fout. Zorg ervoor dat u een niet-lege waarde hebt voor het *bot-geheim* in het hoofd venster van de app (Zie [de sectie uw bot registreren met een direct lijn spraak kanaal](#register-the-direct-line-speech-channel)) |
|Fout AuthenticationFailure: De WebSocket-upgrade is mislukt met een verificatie fout (401). Controleren op juiste abonnements sleutel (of autorisatie token) en regio naam| Controleer op de pagina instellingen van de app of u de sleutel voor spraak abonnementen en de regio juist hebt ingevoerd.<br>Controleer of het bot-geheim juist is ingevoerd. |
|Fout ConnectionFailure: De verbinding is door de externe host gesloten. Foutcode: 1011. Foutdetails: Er kan geen verbinding worden gemaakt met de bot voordat een bericht wordt verzonden | Zorg ervoor dat u het selectie vakje [streaming-eind punt inschakelen](#register-the-direct-line-speech-channel) en/of websockets op op hebt ingeschakeld. [ ](#enable-web-sockets)<br>Zorg ervoor dat uw Azure App Service actief is. Als dat het geval is, start u de App Service opnieuw op.|
|Fout ConnectionFailure: De verbinding is door de externe host gesloten. Foutcode: 1011. Foutdetails: De antwoord status code duidt niet op geslaagde pogingen: 500 (InternalServerError)| Uw bot heeft een Neural-stem opgegeven in het [](https://github.com/microsoft/botframework-sdk/blob/master/specs/botframework-activity/botframework-activity.md#speak) veld uitspreken van uitvoer activiteit, maar de Azure-regio die aan uw spraak abonnements sleutel is gekoppeld, biedt geen ondersteuning voor Neural stemmen. Zie de [standaard-en Neural stemmen](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#standard-and-neural-voices).|
|Fout ConnectionFailure: De verbinding is door de externe host gesloten. Foutcode: 1000. Foutdetails: De maximum duur van de verbinding met een Internet socket is overschreden (> 300000 MS)| Dit is een verwachte fout wanneer er meer dan vijf minuten een verbinding met het kanaal is geopend en inactief is. |

Als uw probleem niet wordt opgelost in de tabel, [raadpleegt u de preview-versie van de virtuele assistenten voor Voice-First: Veelgestelde vragen](https://docs.microsoft.com/azure/cognitive-services/speech-service/faq-voice-first-virtual-assistants).

### <a name="view-bot-activities"></a>Bot-activiteiten weer geven

Elke bot verzendt en ontvangt **activiteiten** berichten. In het venster **activiteiten logboek** van de direct line speech-client ziet u getimede logboeken met elke activiteit die de client heeft ontvangen van de bot. U kunt ook de activiteiten zien die de client naar de bot heeft verzonden met [`DialogServiceConnector.SendActivityAsync`](https://docs.microsoft.com/en-us/dotnet/api/microsoft.cognitiveservices.speech.dialog.dialogserviceconnector.sendactivityasync) behulp van de-methode. Wanneer u een logboek item selecteert, worden de details van de gekoppelde activiteit als JSON weer gegeven.

Hier volgt een voor beeld van een JSON van een activiteit die de client heeft ontvangen:
```json
{
    "attachments":[],
    "channelData":{
        "conversationalAiData":{
             "requestInfo":{
                 "interactionId":"8d5cb416-73c3-476b-95fd-9358cbfaebfa",
                 "version":"0.2"
             }
         }
    },
    "channelId":"directlinespeech",
    "conversation":{
        "id":"129ebffe-772b-47f0-9812-7c5bfd4aca79",
        "isGroup":false
    },
    "entities":[],
    "from":{
        "id":"SpeechEchoBotTutorial-BotRegistration"
    },
    "id":"89841b4d-46ce-42de-9960-4fe4070c70cc",
    "inputHint":"acceptingInput",
    "recipient":{
        "id":"129ebffe-772b-47f0-9812-7c5bfd4aca79|0000"
    },
    "replyToId":"67c823b4-4c7a-4828-9d6e-0b84fd052869",
    "serviceUrl":"urn:botframework:websocket:directlinespeech",
    "speak":"<speak version='1.0' xmlns='https://www.w3.org/2001/10/synthesis' xml:lang='en-US'><voice name='Microsoft Server Speech Text to Speech Voice (en-US, JessaRUS)'>Echo: Hello and welcome.</voice></speak>",
    "text":"Echo: Hello and welcome.",
    "timestamp":"2019-07-19T20:03:51.1939097Z",
    "type":"message"
}
```

Zie [velden in de activiteit](https://github.com/microsoft/botframework-sdk/blob/master/specs/botframework-activity/botframework-activity.md)voor meer informatie over wat er wordt geretourneerd in de JSON-uitvoer. Voor het doel van deze zelf studie kunt u zich richten op de velden [tekst](https://github.com/microsoft/botframework-sdk/blob/master/specs/botframework-activity/botframework-activity.md#text) en [spreken](https://github.com/microsoft/botframework-sdk/blob/master/specs/botframework-activity/botframework-activity.md#speak) .

### <a name="view-client-source-code-for-calls-to-the-speech-sdk"></a>Client bron code weer geven voor aanroepen naar de Speech SDK

De direct line speech-client maakt gebruik van het NuGet-pakket [micro soft. CognitiveServices. speech](https://www.nuget.org/packages/Microsoft.CognitiveServices.Speech/), dat de Speech SDK bevat. Een goede plaats om te beginnen met het controleren van de voorbeeld code is de methode InitSpeechConnector [`DLSpeechClient\MainWindow.xaml.cs`](https://github.com/Azure-Samples/Cognitive-Services-Direct-Line-Speech-Client/blob/master/DLSpeechClient/MainWindow.xaml.cs)() in het bestand, waarmee deze twee Speech SDK-objecten worden gemaakt:
- [`DialogServiceConfig`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.dialog.dialogserviceconfig)-Voor configuratie-instellingen (sleutel van spraak abonnement, sleutel regio, bot geheim)
- [`DialogServiceConnector`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.dialog.dialogserviceconnector.-ctor)-De kanaal verbinding en gebeurtenissen van client abonnementen beheren voor het verwerken van herkende spraak-en bot-reacties.

## <a name="add-custom-wake-word-activation"></a>Aangepaste Wake Word-activering toevoegen

De Speech-SDK ondersteunt aangepaste activering van de functie Wake Word. Net als bij "Hey Cortana" voor de assistent van micro soft kunt u een app schrijven die voortdurend wordt geluisterd naar een Ontwaak woord van uw keuze. Houd er rekening mee dat een Ontwaak woord één woord of een woord groep met meerdere woorden kan zijn.

> [!NOTE]
> De term *Wake work* wordt vaak door elkaar gebruikt in combi natie met het *sleutel woord*en mogelijk worden beide gebruikt in micro soft-documentatie.

De detectie van het Wake-woord wordt uitgevoerd op de client-app. Als u een Ontwaak woord gebruikt, wordt audio alleen gestreamd naar het spraak kanaal direct lijn als het Ontwaak woord wordt gedetecteerd. Het directe lijn spraak kanaal bevat een onderdeel met de naam verificatie met een *sleutel woord (KWV)* , dat complexere verwerking in de Cloud biedt om te controleren of het Ontwaak woord dat u hebt gekozen, aan het begin van de audio stroom is. Als de verificatie van de sleutel woorden is geslaagd, communiceert het kanaal met de bot.

Volg deze stappen voor het maken van een Ontwaak woord model, Configureer de direct-speech-client voor het gebruik van dit model en test deze vervolgens met uw bot.

1. Volg deze instructies voor [het maken van een aangepast Ontwaak woord met behulp van de speech-service](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-devices-sdk-create-kws).
2. Pak het model bestand uit dat u in de vorige stap hebt gedownload. Dit moet een naam voor het Ontwaak woord zijn. U zoekt naar een bestand met de `kws.table`naam.
3. Ga in de client direct speech naar het menu **instellingen** (Zoek het tandwiel pictogram in de rechter bovenhoek). Zoek het pad naar het **model bestand** en voer de volledige padnaam `kws.table` op voor het bestand uit stap 2.
4. Zorg ervoor dat het selectie vakje is **ingeschakeld**. Dit bericht wordt weer gegeven naast het selectie vakje: "Luistert naar het Ontwaak woord bij de volgende verbinding". Als u het verkeerde bestand of een ongeldig pad hebt, wordt een fout bericht weer gegeven.
5. Voer de sleutel van uw spraak **abonnement**, de regio van de **abonnements sleutel**en klik vervolgens op **OK** om het menu **instellingen** te sluiten.
6. Selecteer een **bot-geheim**en klik vervolgens op **opnieuw verbinding maken**. Er wordt een bericht weer gegeven met de volgende strekking: "Nieuwe conversatie gestart-type, druk op de knop microfoon of zeg het Ontwaak woord". De app luistert nu voortdurend.
7. Spreek een wille keurige woord groep in die begint met het Ontwaak woord. Bijvoorbeeld: " **{uw Ontwaak woord}** , wat is de tijd?". U hoeft niet te onderbreken nadat het Ontwaak woord is uttering. Wanneer u klaar bent, gebeuren er twee dingen:
   * U ziet een transcriptie van uw Spaak
   * Kort daarna kunt u het antwoord van de bot horen
8. Ga door met het experimenteren met de drie invoer typen die uw bot ondersteunt:
   * Getypte tekst in de onderste balk
   * Het microfoon pictogram drukken en spreken
   * Een woord groep zeggen die begint met het Ontwaak woord

### <a name="view-the-source-code-that-enables-wake-word"></a>De bron code weer geven waarmee het activeren van een woord wordt ingeschakeld

Bekijk in de bron code van de directe regel client de volgende bestanden om de code te controleren die wordt gebruikt om Wake-woord detectie in te scha kelen:

1. [`DLSpeechClient\Models.cs`](https://github.com/Azure-Samples/Cognitive-Services-Direct-Line-Speech-Client/blob/master/DLSpeechClient/Models.cs)bevat een aanroep van de methode [`KeywordRecognitionModel.fromFile()`](https://docs.microsoft.com/en-us/javascript/api/microsoft-cognitiveservices-speech-sdk/keywordrecognitionmodel?view=azure-node-latest#fromfile-string-)Speech SDK, die wordt gebruikt om het model te instantiëren vanuit een lokaal bestand op schijf.
1. [`DLSpeechClient\MainWindow.xaml.cs`](https://github.com/Azure-Samples/Cognitive-Services-Direct-Line-Speech-Client/blob/master/DLSpeechClient/MainWindow.xaml.cs)bevat een aanroep naar de Speech SDK [`DialogServiceConnector.StartKeywordRecognitionAsync()`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.dialog.dialogserviceconnector.startkeywordrecognitionasync)-methode, waarmee doorlopende detectie van het detecteren van woorden wordt geactiveerd.

## <a name="optional-change-the-language-and-redeploy-your-bot"></a>Beschrijving De taal wijzigen en uw bot opnieuw implementeren

De bot die u hebt gemaakt, luistert naar en reageert in het Engels. U bent echter niet beperkt tot het gebruik van Engels. In deze sectie leert u hoe u de taal kunt wijzigen die door uw bot wordt geluisterd en hierop wordt gereageerd en de bot opnieuw te implementeren.

### <a name="change-the-language"></a>De taal wijzigen

1. Laten we beginnen met het `\experimental\directline-speech\csharp_dotnetcore\02.echo-bot\Bots\echo-bot.cs`openen van.
2. Zoek vervolgens de SSML op. Het is eenvoudig te vinden, omdat het is inge sloten in `<speak></speak>` Tags.
3. Zoek in de teken reeks SSML het `<voice name>` label, vervang `<voice name='de-DE-Stefan-Apollo'>`dit door en sla het op. Deze opgemaakte teken reeks vertelt de service tekst naar spraak om een gesynthesizerd spraak antwoord te retour `de-DE-Stefan-Apollo`neren met behulp van de spraak, dat is geoptimaliseerd voor Duits.

>[!NOTE]
> U bent niet beperkt tot Duits en u kunt kiezen uit de lijst met beschik bare stemmen van de [Speech-Service](language-support.md#text-to-speech).

### <a name="redeploy-your-bot"></a>Uw bot opnieuw implementeren

Nu u de nood zakelijke wijziging hebt aangebracht in de bot, is de volgende stap het opnieuw publiceren naar uw Azure App Service en probeer het uit:

1. Bouw uw oplossing in Visual Studio en los eventuele build-fouten op.
2. Klik in het venster Solution Explorer met de rechter muisknop op het project **EchoBot** en selecteer **publiceren**.
3. De vorige implementatie configuratie is al geladen als standaard. Klik op **publiceren** naast **EchoBot20190805125647-Web Deploy**.
4. Het bericht **publiceren is voltooid** wordt weer gegeven in het venster Visual Studio-uitvoer en er wordt een webpagina geopend met het bericht ' uw bot is klaar! '.
5. Open de client-app direct line speech en klik op de knop instellingen (bovenste tandwiel pictogram) en voer `de-de` in het veld taal in. Hiermee wordt de gesp roken taal herkend en wordt de standaard waarde `en-us`genegeerd.
6. Volg de instructies in [Build the direct line speech client](#build-the-direct-line-speech-client) om opnieuw verbinding te maken met de zojuist geïmplementeerde bot, praat in de nieuwe taal en luistert u in die taal met de nieuwe stem.

## <a name="clean-up-resources"></a>Resources opschonen

Als u de in deze zelf studie geïmplementeerde ECHO-bot niet wilt blijven gebruiken, kunt u deze verwijderen en alle bijbehorende Azure-resources door simpelweg de Azure-resource groep **SpeechEchoBotTutorial-ResourceGroup**te verwijderen.

1. Klik vanuit de [Azure Portal](https://portal.azure.com)op **resource groepen** in het linkernavigatievenster.
2. Zoek de resource groep met de naam: **SpeechEchoBotTutorial-ResourceGroup**. Klik op de drie puntjes (...).
3. Selecteer **Resourcegroep verwijderen**.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Uw eigen client-app bouwen met de Speech SDK](quickstart-virtual-assistant-csharp-uwp.md)

## <a name="see-also"></a>Zie ook

* Implementatie in een [Azure-regio](https://azure.microsoft.com/global-infrastructure/locations/) in de buurt om de reactie tijd van bot te bekijken
* Implementeren in een [Azure-regio die hoogwaardige NEURAL TTS-stemmen ondersteunt](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#standard-and-neural-voices)
* Prijzen gekoppeld aan direct-lijn spraak kanaal:
  * [Prijzen van bot-service](https://azure.microsoft.com/pricing/details/bot-service/)
  * [Spraakservices](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/)
* Bouwen en implementeren van uw eigen spraak ingeschakelde bot:
    * Bouw een [bot-raam](https://dev.botframework.com/)werk op. Registreer deze met [direct line speech Channel](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-directlinespeech?view=azure-bot-service-4.0) en [Pas uw bot aan voor spraak](https://docs.microsoft.com/azure/bot-service/directline-speech-bot?view=azure-bot-service-4.0)
    * Bestaande [bot-Framework oplossingen](https://github.com/microsoft/botframework-solutions)verkennen: Maak een [aangepaste spraak-eerste virtuele assistent](https://docs.microsoft.com/azure/cognitive-services/speech-service/voice-first-virtual-assistants) en [stem deze in](https://github.com/microsoft/botframework-solutions/blob/master/docs/howto/assistant/csharp/speechenablement.md)
