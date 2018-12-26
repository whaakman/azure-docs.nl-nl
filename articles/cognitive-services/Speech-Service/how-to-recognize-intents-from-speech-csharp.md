---
title: 'Zelfstudie: Intenties van gesproken inhoud herkennen met behulp van de Speech SDK voor C#'
titleSuffix: Azure Cognitive Services
description: In deze zelfstudie leert u hoe u met de Speech SDK voor C# intenties van gesproken inhoud herkent.
services: cognitive-services
author: wolfma61
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: tutorial
ms.date: 09/24/2018
ms.author: wolfma
ms.openlocfilehash: 64fa194225c7e9fa4c272ca8a9e95b44282ec1df
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/19/2018
ms.locfileid: "49466474"
---
# <a name="tutorial-recognize-intents-from-speech-using-the-speech-sdk-for-c"></a>Zelfstudie: Intenties van gesproken inhoud herkennen met behulp van de Speech SDK voor C#

[!INCLUDE [Article selector](../../../includes/cognitive-services-speech-service-how-to-recognize-intents-from-speech-selector.md)]

De [Speech SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) van Cognitive Services kan voor **intentieherkenning** worden geïntegreerd met de [Language Understanding-service (LUIS)](https://www.luis.ai/home). Een intentie is iets dat de gebruiker wil doen: een vlucht reserveren, de weersverwachting controleren of iemand bellen. De gebruiker kan elke term gebruiken die natuurlijk aanvoelt. Met behulp van machine learning koppelt LUIS aanvragen van gebruikers aan de intenties die u hebt gedefinieerd.

> [!NOTE]
> Een LUIS-toepassing definieert de intenties en entiteiten die u wilt herkennen. Deze toepassing staat los van de C#-toepassing die gebruikmaakt van de Speech-service. In dit artikel verwijst 'app' naar de LUIS-app en 'toepassing' naar de C#-code.

In deze zelfstudie gebruikt u de Speech SDK voor het ontwikkelen van een C#-consoletoepassing die intenties afleidt van tekst die de gebruiker spreekt in de microfoon van uw apparaat. U leert het volgende:

> [!div class="checklist"]
> * Een Visual Studio-project maken dat verwijst naar het NuGet-pakket van de Speech SDK
> * Een spraakconfiguratie en een mechanisme voor intentieherkenning maken
> * Het model voor uw LUIS-app ophalen en de benodigde intenties toevoegen
> * De taal voor spraakherkenning opgeven
> * Spraak herkennen uit een bestand
> * Asynchrone, gebeurtenisgestuurde continue herkenning gebruiken

## <a name="prerequisites"></a>Vereisten

Zorg dat u over het volgende beschikt voordat u met deze zelfstudie begint.

* Een LUIS-account. U kunt een gratis account aanvragen via het [LUIS portal](https://www.luis.ai/home).
* Visual Studio 2017 (willekeurige editie).

## <a name="luis-and-speech"></a>LUIS en spraakherkenning

LUIS kan worden geïntegreerd met de Speech-service voor het herkennen van intenties van spraak. U hebt geen abonnement op de Speech-service nodig, alleen op LUIS.

LUIS maakt gebruik van twee soorten sleutels: 

|Type sleutel|Doel|
|--------|-------|
|authoring|Hiermee kunt u via programmacode LUIS-apps maken en wijzigen|
|endpoint |Hiermee kunt u toegang verlenen tot een bepaalde LUIS-app|

De endpoint-sleutel is de LUIS-sleutel die nodig is voor deze zelfstudie. In deze zelfstudie wordt gebruikgemaakt van de voorbeeld-app Home Automation van LUIS. Deze app kunt u maken aan de hand van de instructies in [Snelstart: Een vooraf gemaakte app voor huisautomatisering gebruiken](https://docs.microsoft.com/azure/cognitive-services/luis/luis-get-started-create-app). Als u de beschikking hebt over een andere LUIS-app, kunt u die ook gebruiken.

Wanneer u een LUIS-app maakt, wordt er automatisch een starter-sleutel gegenereerd, zodat u de app kunt testen met tekstquery's. Deze sleutel zorgt er overigens niet voor dat de integratie van de Speech-service wordt ingeschakeld en werkt ook niet met deze zelfstudie. U moet een LUIS-resource maken in het Azure-dashboard maken en deze toewijzen aan de LUIS-app. U kunt de gratis versie van het abonnement gebruiken voor deze zelfstudie. 

Na het maken van de LUIS-resource in het Azure-dashboard, meldt u zich aan bij de [LUIS-portal](https://www.luis.ai/home), kiest u uw toepassing op de pagina My Apps en gaat u naar de pagina Manage van de app. Klik hier op **Keys and Endpoints** in de zijbalk.

![Instellingen voor sleutels en eindpunten in de LUIS-portal](media/sdk/luis-keys-endpoints-page.png)

Ga als volgt te werk op de pagina Keys and Endpoints:

1. Scrol omlaag naar het gedeelte Resources and Keys en klik op **Assign resource**.
1. Geef het volgende op in het dialoogvenster **Assign a key to your app**:

    * Kies Microsoft als de tenant.
    * Kies onder Subscription Name het Azure-abonnement met de LUIS-resource die u wilt gebruiken.
    * Kies onder Key de LUIS-resource die u wilt gebruiken met de app.

Na korte tijd wordt het nieuwe abonnement weergegeven in de tabel onderaan de pagina. Klik op het pictogram naast een sleutel om deze naar het klembord te kopiëren. (U kunt beide sleutels gebruiken.)

![Abonnementssleutels voor LUIS-app](media/sdk/luis-keys-assigned.png)

## <a name="create-a-speech-project-in-visual-studio"></a>Een spraakproject maken in Visual Studio

[!INCLUDE [Create project ](../../../includes/cognitive-services-speech-service-create-speech-project-vs-csharp.md)]

## <a name="add-the-code"></a>Code toevoegen

Open het bestand `Program.cs` in het Visual Studio-project en vervang het blok `using`-instructies aan het begin van het bestand door de volgende declaraties.

[!code-csharp[Top-level declarations](~/samples-cognitive-services-speech-sdk/samples/csharp/sharedcontent/console/intent_recognition_samples.cs#toplevel)]

Voeg binnen de opgegeven `Main()`-methode de volgende code toe.

```csharp
RecognizeIntentAsync().Wait();
Console.WriteLine("Please press Enter to continue.");
Console.ReadLine();
```

Maak een lege asynchrone methode `RecognizeIntentAsync()`, zoals hier wordt weergegeven.

```csharp
static async Task RecognizeIntentAsync()
{
}
```

Voeg in de hoofdtekst van deze nieuwe methode deze code toe.

[!code-csharp[Intent recognition by using a microphone](~/samples-cognitive-services-speech-sdk/samples/csharp/sharedcontent/console/intent_recognition_samples.cs#intentRecognitionWithMicrophone)]

Vervang de tijdelijke aanduidingen in deze methode als volgt door de sleutel, regio en app-id van uw LUIS-abonnement.

|Tijdelijke aanduiding|Vervangen door|
|-----------|------------|
|`YourLanguageUnderstandingSubscriptionKey`|Uw LUIS-endpoint-sleutel Zoals eerder vermeld, moet dit een sleutel zijn die u hebt verkregen via uw Azure-dashboard, niet een 'starter-key'. U kunt de sleutel vinden op de pagina Keys and Endpoints van uw app (onder Manage) in de [LUIS-portal](https://www.luis.ai/home).|
|`YourLanguageUnderstandingServiceRegion`|De korte id voor de regio van uw LUIS-abonnement, zoals `westus` voor US - west. Zie [Regio's](regions.md).|
|`YourLanguageUnderstandingAppId`|De id van de LUIS-app. U kunt deze vinden op de pagina Settings van de [LUIS-portal](https://www.luis.ai/home).|

Als u klaar bent met deze wijzigingen, kunt u de toepassing voor de zelfstudie compileren (Ctrl-Shift-B) en uitvoeren (F5). Als dat wordt gevraagd, zegt u "Turn off the lights" in de microfoon van uw pc. Het resultaat wordt weergegeven in het consolevenster.

In de volgende secties wordt dieper ingegaan op de code.


## <a name="create-an-intent-recognizer"></a>Een mechanisme voor intentieherkenning maken

De eerste stap bij het herkennen intenties in spraak is het maken van een spraakconfiguratie op basis van de eindpuntsleutel en regio van LUIS. Spraakconfiguraties kunnen worden gebruikt voor het maken van mechanismen voor intentieherkenning voor de verschillende mogelijkheden van de Speech SDK. Er zijn verschillende manieren om in de spraakconfiguratie het abonnement op te geven dat u wilt gebruiken. In dit voorbeeld gebruiken we `FromSubscription`, waarbij de abonnementssleutel en de regio worden opgehaald.

> [!NOTE]
> Gebruik de sleutel en de regio van uw LUIS-abonnement en niet van een Speech Service-abonnement.

Maak vervolgens een mechanisme voor intentieherkenning met behulp van `new IntentRecognizer(config)`. Aangezien de configuratie al weet welk abonnement moet worden gebruikt, is het niet nodig om de abonnementssleutel en het eindpunt opnieuw op te geven bij het maken van het mechanisme.

## <a name="import-a-luis-model-and-add-intents"></a>Een LUIS-model importeren en intenties toevoegen

Importeer het model nu uit de LUIS-app met `LanguageUnderstandingModel.FromAppId()` en voeg de intenties van LUIS toe die u wilt herkennen via de methode `AddIntent()` van het mechanisme. Deze twee stappen verbeteren de nauwkeurigheid van spraakherkenning door woorden aan te geven die de gebruiker waarschijnlijk zal gebruiken in aanvragen. Het is niet nodig om alle intenties van de app toe te voegen als u ze niet allemaal hoeft te herkennen in uw toepassing.

Er zijn drie argumenten vereist voor het toevoegen van intenties: het LUIS-model (dat net is gemaakt en de naam `model` heeft), de naam van de intentie en een intentie-id. Het verschil tussen de id en de naam is als volgt.

|Argument voor `AddIntent()`|Doel|
|--------|-------|
|intentName |De naam van de intentie zoals gedefinieerd in de LUIS-app. Moet exact overeenkomen met de naam van de LUIS-intentie.|
|intentID    |Een id die door de Speech SDK wordt toegewezen aan een herkende intentie. Deze id kan elke gewenste waarde hebben en hoeft niet overeen te komen met de naam van de intentie zoals deze is gedefinieerd in de LUIS-app. Als in dezelfde code bijvoorbeeld meerdere intenties worden afgehandeld, kun u dezelfde id voor de intenties gebruiken.|

De LUIS-app Home Automation heeft twee intenties: een voor het inschakelen van een apparaat en een het uitschakelen van een apparaat. Met de onderstaande regels worden deze intenties toegevoegd aan het mechanisme voor intentieherkenning. Vervang de drie regels `AddIntent` in de methode `RecognizeIntentAsync()` door deze code.

```csharp
recognizer.AddIntent(model, "HomeAutomation.TurnOff", "off");
recognizer.AddIntent(model, "HomeAutomation.TurnOn", "on");
```

## <a name="start-recognition"></a>Herkenning starten

Het mechanisme voor intentieherkenning is gemaakt en de intenties zijn toegevoegd. We kunnen dus het proces van herkenning starten. De Speech SDK biedt ondersteuning voor herkenning van zowel afzonderlijke opnamen als continue herkenning.

|Herkennings-modus|Methoden om aan te roepen|Resultaat|
|----------------|-----------------|---------|
|Eén opname|`RecognizeOnceAsync()`|Retourneert de herkende intentie, indien aanwezig, na één utterance.|
|Continu|`StartContinuousRecognitionAsync()`<br>`StopContinuousRecognitionAsync()`|Herkent meerdere utterances. Verzendt gebeurtenissen (bijvoorbeeld `IntermediateResultReceived`) als er resultaten beschikbaar zijn.|

In de zelfstudie toepassing wordt de continue modus gebruikt en dus wordt `RecognizeOnceAsync()` aangeroepen om te starten met herkenning. Het resultaat is een `IntentRecognitionResult`-object met informatie over de herkende intentie. Het JSON-antwoord van LUIS kan worden geëxtraheerd met de volgende expressie:

```csharp
result.Properties.GetProperty(PropertyId.LanguageUnderstandingServiceResponse_JsonResult)
```

Het JSON-resultaat wordt niet geparseerd door de toepassing in de zelfstudie, maar wordt alleen weergegeven in het consolevenster.

![Resultaten van herkenning door LUIS](media/sdk/luis-results.png)

## <a name="specify-recognition-language"></a>Herkenningstaal opgeven

Standaard herkent LUIS intenties in het Amerikaans-Engels (`en-us`). U kunt een landinstellingscode toewijzen aan de eigenschap `SpeechRecognitionLanguage` van de spraakconfiguratie om intenties in andere talen te herkennen. Als u bijvoorbeeld `config.SpeechRecognitionLanguage = "de-de";` toevoegt aan onze toepassing voordat u het mechanisme voor intentieherkenning maakt, kunt u intenties in het Duits herkennen. Zie [Ondersteunde talen](language-support.md#speech-to-text) voor meer informatie.

## <a name="continuous-recognition-from-a-file"></a>Continue herkenning uit een bestand

De volgende code illustreert twee aanvullende mogelijkheden van intentieherkenning met behulp van de Speech SDK. De eerste mogelijkheid, die eerder is genoemd, is continue spraakherkenning. Hierbij verzendt het mechanisme voor intentieherkenning gebeurtenissen wanneer er resultaten beschikbaar zijn. Deze gebeurtenissen kunnen vervolgens worden verwerkt door gebeurtenis-handlers die u opgeeft. Bij continue spraakherkenning roept u de methode `StartContinuousRecognitionAsync()` van het mechanisme aan in plaats van `RecognizeOnceAsync()` om de herkenning te starten.

De andere mogelijkheid is het lezen van de audio met de spraak die moet worden verwerkt uit een WAV-bestand. Hiervoor is het nodig om een audioconfiguratie te maken die kan worden gebruikt bij het maken van het mechanisme voor intentieherkenning. Het bestand moet éénkanaals (mono) zijn met een samplefrequentie van 16 kHz.

Als u deze functies wilt uitproberen, vervangt u de hoofdtekst van de methode `RecognizeIntentAsync()` door de volgende code. 

[!code-csharp[Intent recognition by using events from a file](~/samples-cognitive-services-speech-sdk/samples/csharp/sharedcontent/console/intent_recognition_samples.cs#intentContinuousRecognitionWithFile)]

Wijzig de code met uw LUIS-eindpuntsleutel, regio en app-id en voeg de intenties van Home Automation toe, zoals u eerder hebt gedaan. Wijzig `whatstheweatherlike.wav` in de naam van uw audiobestand. Compileer de code vervolgens en voer deze uit.

[!INCLUDE [Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
De code voor dit artikel kunt u vinden in de map samples/csharp/sharedcontent/console.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Spraak herkennen](how-to-recognize-speech-csharp.md)
