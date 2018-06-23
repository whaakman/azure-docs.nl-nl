---
title: Aan de slag met de Microsoft Speech Recognition API met behulp van de C# bureaublad bibliotheek | Microsoft Docs
description: Basic Windows toepassingen ontwikkelen die gebruikmaken van de API van Microsoft spraak erkenning gesproken audio converteren naar tekst.
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.component: bing-speech
ms.topic: article
ms.date: 09/27/2017
ms.author: zhouwang
ms.openlocfilehash: e59b0e25401fb5182edd52f82985ffed9052286d
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344695"
---
# <a name="get-started-with-the-speech-recognition-api-in-c35-for-net-on-windows"></a>Aan de slag met de erkenning Speech-API in C&#35; voor .NET in Windows

Deze pagina bevat het ontwikkelen van een eenvoudige Windows-toepassing die gebruikmaakt van de erkenning Speech-API aan gesproken audio naar tekst wilt converteren. Met behulp van de clientbibliotheek kunt u realtime streaming, wat betekent dat wanneer u de clienttoepassing audio naar de service verzendt, het tegelijkertijd en asynchroon gedeeltelijke herkenningsresultaten terug ontvangt.

Ontwikkelaars die Service wilt gebruiken spraak van apps die op elk apparaat uitvoeren, kunnen de C# bureaublad bibliotheek gebruiken. Voor het gebruik van de bibliotheek installeert de [NuGet-pakket Microsoft.ProjectOxford.SpeechRecognition x86](https://www.nuget.org/packages/Microsoft.ProjectOxford.SpeechRecognition-x86/) voor een 32-bits platform en de [NuGet-pakket Microsoft.ProjectOxford.SpeechRecognition x64](https://www.nuget.org/packages/Microsoft.ProjectOxford.SpeechRecognition-x64/) voor een 64-bits platform. Zie voor de API-referentiemateriaal-clientbibliotheek [Microsoft spraak C# bureaublad bibliotheek](https://cdn.rawgit.com/Microsoft/Cognitive-Speech-STT-Windows/master/docs/SpeechSDK/index.html).

De volgende secties wordt beschreven hoe installeren, bouwen en uitvoeren van de C#-voorbeeldtoepassing met behulp van de C# bureaublad bibliotheek.

## <a name="prerequisites"></a>Vereisten

### <a name="platform-requirements"></a>Platformvereisten

Het volgende voorbeeld is ontwikkeld voor Windows 8 + en .NET Framework 4.5 + met behulp van [Visual Studio 2015, Community Edition](https://www.visualstudio.com/products/visual-studio-community-vs).

### <a name="get-the-sample-application"></a>Ophalen van de voorbeeldtoepassing

Klonen van de steekproef van de [spraak C# bureaublad bibliotheek voorbeeld](https://github.com/microsoft/cognitive-speech-stt-windows) opslagplaats.

### <a name="subscribe-to-the-speech-recognition-api-and-get-a-free-trial-subscription-key"></a>Abonneer u op de erkenning Speech-API en de sleutel voor een gratis proefabonnement ophalen

De Speech-API maakt deel uit van cognitieve Services (eerder Project Oxford). Krijgt u gratis proefabonnement sleutels van de [abonnement cognitieve Services](https://azure.microsoft.com/try/cognitive-services/) pagina. Nadat u de Speech-API selecteert, selecteert u **API-sleutel ophalen** ophalen van de sleutel. Het resultaat een primaire en secundaire sleutel. Beide sleutels zijn gekoppeld aan hetzelfde contingent, zodat u van sleutel kunt gebruiken.

> [!IMPORTANT]
> * De abonnementssleutel voor een ophalen. Voordat u de clientbibliotheken spraak gebruikt, hebt u een [abonnementssleutel](https://azure.microsoft.com/try/cognitive-services/).
>
> * De abonnementssleutel van uw worden gebruikt. Met de opgegeven C# bureaublad voorbeeldtoepassing, plakt u de abonnementssleutel van uw in het tekstvak wanneer u het voorbeeld uitvoert. Zie voor meer informatie [uitvoeren van de voorbeeldtoepassing](#step-3-run-the-sample-application).

## <a name="step-1-install-the-sample-application"></a>Stap 1: De voorbeeldtoepassing installeren

1. Start Visual Studio 2015 en selecteer **bestand** > **Open** > **Project/oplossing**.

2. Blader naar de map waar u de gedownloade herkenning van API-bestanden opgeslagen. Selecteer **spraak** > **Windows**, en selecteer vervolgens de voorbeeld-WP-map.

3. Dubbelklik op om de Visual Studio 2015-oplossing (.sln)-bestand met de naam SpeechToText-WPF-Samples.sln te openen. De oplossing in Visual Studio is geopend.

## <a name="step-2-build-the-sample-application"></a>Stap 2: De voorbeeldtoepassing

1. Als u wilt gebruiken *herkenning met opzet*, moet u eerst aanmelden voor de [Language Understanding Intelligent Service (LUIS)](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/). De eindpunt-URL van uw app LUIS vervolgens gebruiken om in te stellen van de waarde van de sleutel `LuisEndpointUrl` in het bestand app.config in de map samples/SpeechRecognitionServiceExample. Zie voor meer informatie over de eindpunt-URL van de app LUIS [publiceren van uw app](../../luis/luis-get-started-create-app.md#publish-your-app).

   > [!TIP]
   > Vervang de `&` teken in de LUIS eindpunt-URL met `&amp;` om ervoor te zorgen dat de URL juist is geïnterpreteerd door de XML-parser.

2. Druk op Ctrl + Shift + B, of selecteer **bouwen** in het lintmenu. Selecteer vervolgens **Build Solution**.

## <a name="step-3-run-the-sample-application"></a>Stap 3: Voer de voorbeeldtoepassing

1. Nadat de build is voltooid, druk op F5 of selecteer **Start** in het lintmenu om het voorbeeld uitvoert.

2. Ga naar de **Project Oxford Speech Text steekproef** venster. Plak uw abonnementssleutel in de **plak hier uw abonnementssleutel voor het starten van** tekstvak zoals wordt weergegeven. Selecteer om te blijven behouden de abonnementssleutel van uw op uw PC of laptop, **sleutel opslaan**. Selecteer het om abonnementssleutel te verwijderen uit het systeem, **sleutel verwijderen** van uw PC of laptop verwijderen.

   ![Spraak erkenning plakken in de sleutel](../Images/SpeechRecog_paste_key.PNG)

3. Onder **Speech Recognition bron**, kies een van de zes spraak-bronnen, die kunnen worden onderverdeeld in twee hoofdcategorieën invoer:

   * Microfoon van uw computer of een aangesloten microfoon gebruiken om vast te leggen spraak.
   * Een audiobestand afspelen.

   Elke categorie heeft drie erkenning modi:

    * **ShortPhrase modus**: een utterance maximaal 15 seconden lang. Wanneer gegevens worden verzonden naar de server, ontvangt de client meerdere gedeeltelijke resultaten en één eindresultaat meerdere n beste keuze.
    * **LongDictation modus**: een utterance tot twee minuten lang. Wanneer gegevens worden verzonden naar de server, ontvangt de client meerdere gedeeltelijke resultaten en meerdere laatste resultaten op basis van waar de server geeft aan zin wordt onderbroken.
    * **Detectie van opzet**: de server meer gestructureerde informatie over de spraakinvoer retourneert. Voor het gebruik van detectie van opzet, moet u eerst een model te trainen via [LUIS](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/).

Audio-voorbeeldbestanden gebruiken met deze voorbeeldtoepassing. De bestanden niet vinden in de opslagplaats die u hebt gedownload met dit voorbeeld onder de map samples/SpeechRecognitionServiceExample. Deze audio voorbeeldbestanden automatisch wordt uitgevoerd als er geen andere bestanden worden gekozen wanneer u selecteert **wav-bestanden gebruiken voor de modus Shortphrase** of **wav-bestanden gebruiken voor de modus Longdictation** als uw spraakinvoer. Op dit moment wordt alleen WAV audio-indeling ondersteund.

![Spraak voor tekst-interface](../Images/HelloJones.PNG)

## <a name="samples-explained"></a>Voorbeelden uitgelegd

### <a name="recognition-events"></a>Opname-gebeurtenissen

* **Gedeeltelijke resultaten gebeurtenissen**: deze gebeurtenis wordt aangeroepen telkens spraak Service wat u mogelijk worden gezegd voorspelt, voordat u klaar bent met het spreken (als u `MicrophoneRecognitionClient`) of eindigen met het verzenden van gegevens (als u `DataRecognitionClient`).
* **Foutgebeurtenissen**: wordt aangeroepen wanneer de service een fout wordt gedetecteerd.
* **Opzet gebeurtenissen**: aangeroepen op clients 'WithIntent' (alleen in de modus ShortPhrase) na de laatste opname resultaat in een gestructureerde JSON wordt intentie wordt geparseerd.
* **Gebeurtenissen leiden**:
  * In `ShortPhrase` modus, deze gebeurtenis wordt aangeroepen en retourneert de resultaten n beste nadat u klaar bent met het spreken.
  * In `LongDictation` modus wordt de gebeurtenis-handler is meermaals aangeroepen, op basis van waar de service zin onderbroken identificeert.
  * **Voor elk van de keuzes n beste**, een waarde vertrouwen en een aantal verschillende vormen van de herkende tekst worden geretourneerd. Zie voor meer informatie [uitvoerindeling](../Concepts.md#output-format).

Gebeurtenis-handlers zijn al uiteengezet in de code in de vorm van codeopmerkingen.

## <a name="related-topics"></a>Verwante onderwerpen

* [Verwijzing naar het bureaublad-bibliotheek van Microsoft Speech](https://cdn.rawgit.com/Microsoft/Cognitive-Speech-STT-Windows/master/docs/SpeechSDK/index.html)
* [Aan de slag met de Microsoft Speech Recognition API in Java op Android](GetStartedJavaAndroid.md)
* [Aan de slag met de Microsoft Speech Recognition API in Objective-C op iOS](Get-Started-ObjectiveC-iOS.md)
* [Aan de slag met de Microsoft Speech Recognition API in JavaScript](GetStartedJSWebsockets.md)
* [Aan de slag met de Microsoft Speech Recognition API via REST](GetStartedREST.md)
