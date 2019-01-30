---
title: Aan de slag met de Bing Speech-API met behulp van de C# bureaublad bibliotheek | Microsoft Docs
titlesuffix: Azure Cognitive Services
description: Ontwikkel basic Windows-toepassingen die gebruikmaken van de Bing Speech-API aan Converteer gesproken audio naar tekst.
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.subservice: bing-speech
ms.topic: article
ms.date: 09/18/2018
ms.author: zhouwang
ms.openlocfilehash: 4d03ccfddab9a4aab4a1eacde02d68652bf5103a
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55219070"
---
# <a name="quickstart-use-the-bing-speech-recognition-api-in-c35-for-net-on-windows"></a>Quickstart: Gebruik de Bing the spraakherkennings-API in C&#35; voor .NET in Windows

[!INCLUDE [Deprecation note](../../../../includes/cognitive-services-bing-speech-api-deprecation-note.md)]

Deze pagina bevat over het ontwikkelen van een eenvoudige Windows-toepassing die gebruikmaakt van de spraak-API om Converteer gesproken audio naar tekst. Met de clientbibliotheek beschikt u over realtime streaming, wat betekent dat wanneer u de clienttoepassing audio naar de service verzendt, wordt tegelijkertijd en asynchroon gedeeltelijke herkenningsresultaten terug ontvangen.

Ontwikkelaars die willen Speech Service gebruiken vanuit apps die worden uitgevoerd op elk apparaat kunnen de C# bureaublad bibliotheek gebruiken. Voor het gebruik van de bibliotheek, installeert de [NuGet-pakket Microsoft.ProjectOxford.SpeechRecognition x86](https://www.nuget.org/packages/Microsoft.ProjectOxford.SpeechRecognition-x86/) voor een 32-bits platform en de [NuGet-pakket Microsoft.ProjectOxford.SpeechRecognition x64](https://www.nuget.org/packages/Microsoft.ProjectOxford.SpeechRecognition-x64/) voor een 64-bits platform. Zie voor de API-verwijzing-clientbibliotheek [Microsoft Speech C# bureaublad bibliotheek](https://cdn.rawgit.com/Microsoft/Cognitive-Speech-STT-Windows/master/docs/SpeechSDK/index.html).

De volgende secties wordt beschreven hoe u installeert, bouwen en uitvoeren van de C#-voorbeeldtoepassing met behulp van het bureaublad C#-bibliotheek.

## <a name="prerequisites"></a>Vereisten

### <a name="platform-requirements"></a>Platformvereisten

Het volgende voorbeeld is ontwikkeld voor Windows 8 + en .NET Framework 4.5 + met behulp van [Visual Studio 2015, Community-editie](https://www.visualstudio.com/products/visual-studio-community-vs).

### <a name="get-the-sample-application"></a>De voorbeeldtoepassing ophalen

Klonen van het voorbeeld van de [spraak C# bureaublad bibliotheek voorbeeld](https://github.com/microsoft/cognitive-speech-stt-windows) opslagplaats.

### <a name="subscribe-to-the-speech-recognition-api-and-get-a-free-trial-subscription-key"></a>Abonneer u op de spraak-API en een gratis proefabonnement-sleutel ophalen

De spraak-API maakt deel uit van Cognitive Services (eerder Project Oxford). U krijgt een gratis proefabonnement sleutels uit de [Cognitive Services-abonnement](https://azure.microsoft.com/try/cognitive-services/) pagina. Nadat u de spraak-API selecteert, selecteert u **API-sleutel ophalen** om op te halen van de sleutel. Het resultaat een primaire en secundaire sleutel. Beide sleutels zijn gekoppeld aan dezelfde quota, zodat u beide sleutels kunt gebruiken.

> [!IMPORTANT]
> * De abonnementssleutel van een ophalen. Voordat u de spraak-clientbibliotheken gebruikt, moet u beschikken over een [abonnementssleutel](https://azure.microsoft.com/try/cognitive-services/).
>
> * Gebruik uw abonnement. Met de opgegeven C# bureaublad-voorbeeldtoepassing, plakt u de abonnementssleutel van uw in het tekstvak wanneer u het voorbeeld uitvoert. Zie voor meer informatie, [de voorbeeldtoepassing uitvoeren](#step-3-run-the-sample-application).

## <a name="step-1-install-the-sample-application"></a>Stap 1: De voorbeeld-App installeren

1. Start Visual Studio 2015 en selecteer **bestand** > **Open** > **Project/oplossing**.

2. Blader naar de map waar u de gedownloade spraak Recognition-API-bestanden hebt opgeslagen. Selecteer **spraak** > **Windows**, en selecteer vervolgens de voorbeeld-WP-map.

3. Dubbelklik op de Visual Studio 2015-oplossing (.sln)-bestand met de naam SpeechToText-WPF-Samples.sln. De oplossing wordt geopend in Visual Studio.

## <a name="step-2-build-the-sample-application"></a>Stap 2: De voorbeeldtoepassing

1. Als u wilt gebruiken *erkenning met opzet*, moet u eerst zich aanmelden voor de [Language Understanding Intelligent Service (LUIS)](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/). De eindpunt-URL van uw LUIS-app vervolgens gebruiken om in te stellen de waarde van de sleutel `LuisEndpointUrl` in het bestand app.config in de map samples/SpeechRecognitionServiceExample. Zie voor meer informatie over de eindpunt-URL van de LUIS-app, [Publiceer uw app](../../luis/luis-get-started-create-app.md#publish-your-app).

   > [!TIP]
   > Vervang de `&` teken in de eindpunt-URL van LUIS met `&amp;` om ervoor te zorgen dat de URL juist wordt geïnterpreteerd door de XML-parser.

2. Druk op Ctrl + Shift + B, of selecteer **bouwen** in het lintmenu. Selecteer vervolgens **Build Solution**.

## <a name="step-3-run-the-sample-application"></a>Stap 3: De voorbeeldtoepassing uitvoeren

1. Nadat de build is voltooid, druk op F5 of selecteer **Start** in het lintmenu om het voorbeeld uitvoert.

2. Ga naar de **Project Oxford spraak naar tekstvoorbeeld** venster. Plak de abonnementssleutel van uw in de **plak hier uw abonnementssleutel voor het starten van** tekstvak, zoals wordt weergegeven. Selecteer om te blijven behouden uw abonnementssleutel op uw PC of laptop, **sleutel opslaan**. Selecteer wilt verwijderen van de abonnementssleutel van het systeem **sleutel verwijderen** te verwijderen van uw PC of laptop.

   ![Spraakherkenning plakken in sleutel](../Images/SpeechRecog_paste_key.PNG)

3. Onder **spraakherkenning bron**, kies een van de zes spraak-bronnen, die kunnen worden onderverdeeld in twee hoofdcategorieën invoer:

   * Microfoon van uw computer of een gekoppelde microfoon gebruiken om vast te leggen van spraak.
   * Een geluidsbestand afgespeeld.

   Elke categorie heeft drie erkenning modi:

    * **ShortPhrase modus**: Een utterance tot maximaal 15 seconden lang. Gegevens worden verzonden naar de server, ontvangt de client meerdere gedeeltelijke resultaten en één eindresultaat met meerdere n-beste keuzen.
    * **LongDictation modus**: Een utterance tot twee minuten lang. Gegevens worden verzonden naar de server, ontvangt de client meerdere gedeeltelijke resultaten en meerdere eindresultaten, op basis van waar de server pauzes in zinnen aangeeft.
    * **Detectie van intentie**: De server retourneert meer gestructureerde informatie over de spraak. Voor het gebruik van detectie van intentie, moet u eerst een model met behulp van de trein [LUIS](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/).

Voorbeeld-bestanden met deze voorbeeldtoepassing gebruiken. De bestanden in de opslagplaats die u hebt gedownload met dit voorbeeld onder de map samples/SpeechRecognitionServiceExample vinden. Deze voorbeeld-audiobestanden automatisch wordt uitgevoerd als er geen andere bestanden worden gekozen wanneer u selecteert **wav-bestanden gebruiken voor de modus Shortphrase** of **wav-bestanden gebruiken voor de modus Longdictation** als uw spraakinvoer. Op dit moment wordt alleen WAV audio-indeling ondersteund.

![Spraak naar tekst-interface](../Images/HelloJones.PNG)

## <a name="samples-explained"></a>Voorbeelden uitgelegd

### <a name="recognition-events"></a>Herkenning van gebeurtenissen

* **Gedeeltelijke resultaten gebeurtenissen**: Deze gebeurtenis wordt aangeroepen wanneer Spraakservice wat u mogelijk worden zeggen voorspelt, voordat u klaar bent met het spreken (als u `MicrophoneRecognitionClient`) of u klaar bent met het verzenden van gegevens (als u `DataRecognitionClient`).
* **Gebeurtenissen op foutniveau**: Wordt aangeroepen wanneer de service een fout wordt gedetecteerd.
* **Intentie gebeurtenissen**: Met de naam op de clients 'WithIntent' (alleen in de modus ShortPhrase) na de laatste opname resultaat in een gestructureerde JSON-doel wordt geparseerd.
* **Gebeurtenissen resulteren**:
  * In `ShortPhrase` modus, deze gebeurtenis wordt genoemd en retourneert de resultaten van de n-beste nadat u klaar bent met het spreken.
  * In `LongDictation` modus wordt de gebeurtenis-handler heet meerdere keren op basis van waar de service pauzes in zinnen aangeeft.
  * **Voor elk van de n-beste keuzen**, een waarde vertrouwen en enkele andere vormen van de herkende tekst worden geretourneerd. Zie voor meer informatie, [uitvoerindeling](../Concepts.md#output-format).

Gebeurtenis-handlers zijn al uiteengezet in de code in de vorm van codeopmerkingen.

## <a name="related-topics"></a>Verwante onderwerpen

* [Microsoft Speech bureaublad bibliotheekverwijzing](https://cdn.rawgit.com/Microsoft/Cognitive-Speech-STT-Windows/master/docs/SpeechSDK/index.html)
* [Aan de slag met de Microsoft Speech-API in Java op Android](GetStartedJavaAndroid.md)
* [Aan de slag met de Microsoft Speech-API in Objective-C in iOS](Get-Started-ObjectiveC-iOS.md)
* [Aan de slag met de Microsoft Speech-API in JavaScript](GetStartedJSWebsockets.md)
* [Aan de slag met de Microsoft Speech-API via REST](GetStartedREST.md)
