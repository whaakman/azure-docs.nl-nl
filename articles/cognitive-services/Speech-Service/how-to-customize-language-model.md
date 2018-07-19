---
title: Het maken van een taalmodel met Speech Service - Microsoft Cognitive Services | Microsoft Docs
description: Leer hoe u een taalmodel kunt maken met Speech Service in Microsoft Cognitive Services.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: speech-service
ms.topic: tutorial
ms.date: 06/25/2018
ms.author: panosper
ms.openlocfilehash: 85e67be406b3d9723476821adfb09fc4db8dc1d1
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/17/2018
ms.locfileid: "39068570"
---
# <a name="tutorial-create-a-custom-language-model"></a>Zelfstudie: Een aangepast taalmodel maken

In dit document maakt u een aangepast taalmodel, dat u vervolgens kunt gebruiken in combinatie met bestaande geavanceerde spraakmodellen van Microsoft om spraakinteractie toe te voegen aan uw toepassing.

In dit document wordt het volgende beschreven:
> [!div class="checklist"]
> * De gegevens voorbereiden
> * De taalgegevensset importeren
> * Het aangepaste taalmodel maken

Als u nog geen Cognitive Services-account hebt, maakt u een [gratis account](https://azure.microsoft.com/try/cognitive-services/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Zorg ervoor dat uw Cognitive Services-account is gekoppeld aan een abonnement door de pagina met [Cognitive Services-abonnementen](https://customspeech.ai/Subscriptions) te openen.

Of u kunt verbinding maken met een in Azure Portal gemaakt Speech Service-abonnement door op de knop **Connect existing subscription** (verbinding maken met bestaand abonnement) te klikken.

Raadpleeg de [startpagina](get-started.md) voor informatie over het maken van een Speech Service-abonnement in Azure Portal.

## <a name="prepare-the-data"></a>De gegevens voorbereiden

Als u een aangepast taalmodel voor uw toepassing wilt maken, moet u een lijst met voorbeeld-utterances aan het systeem verstrekken, bijvoorbeeld:

*   "De patiënt heeft sinds de afgelopen week netelroos."
*   "De patiënt had een goed genezen herniorafielitteken."

De zinnen hoeven niet volledig of grammaticaal correct te zijn, en moeten de gesproken invoer weerspiegelen waarvan wordt verwacht dat het systeem deze in de praktijk tegenkomt. Deze voorbeelden moeten zowel de stijl als de inhoud weerspiegelen van de taak die de gebruikers met uw toepassing uitvoeren.

De gegevens van het taalmodel moeten in UTF-8 BOM zijn geschreven. Het tekstbestand moet één voorbeeld (zin, utterance of query) per regel bevatten.

Als u wilt dat sommige termen een hoger gewicht hebben (belangrijkheid), kunt u verschillende utterances die deze term bevatten aan uw gegevens toevoegen. 

De belangrijkste vereisten voor de taalgegevens worden samengevat in de volgende tabel.

| Eigenschap | Waarde |
|----------|-------|
| Tekstcodering | UTF-8 BOM|
| Aantal utterances per regel | 1 |
| Maximale bestandsgrootte | 1,5 GB |
| Opmerkingen | vermijd tekens meer dan vier keer te herhalen, bijvoorbeeld 'aaaaa'|
| Opmerkingen | geen speciale tekens als '\t' of andere UTF-8-teken boven U+00A1 in de [tabel met Unicode-tekens](http://www.utf8-chartable.de/)|
| Opmerkingen | URI's wordt ook geweigerd, omdat er geen unieke manier is om een URI uit te spreken|

Wanneer de tekst wordt geïmporteerd, wordt de tekst genormaliseerd zodat deze door het systeem kan worden verwerkt. Er zijn echter enkele belangrijke normalisaties die door de gebruiker moeten worden uitgevoerd _voordat_ de gegevens worden geüpload. Zie de [Transcriptierichtlijnen](prepare-transcription.md) om te bepalen welke taal geschikt is bij het voorbereiden van uw taalgegevens.

## <a name="language-support"></a>Taalondersteuning

De volgende talen worden ondersteund voor aangepaste **spraak naar tekst**-taalmodellen.

Klik voor de volledige lijst met [ondersteunde talen](supported-languages.md)

## <a name="import-the-language-data-set"></a>De taalgegevensset importeren

Klik op de knop 'Import' (importeren) in de rij 'Language Datasets' (taalgegevenssets). De site geeft dan een pagina weer voor het uploaden van een nieuwe gegevensset.

Wanneer u er klaar voor bent om uw taalgegevensset te importeren, meldt u zich aan bij de [Speech Service-portal](https://customspeech.ai).  Klik op de vervolgkeuzelijst 'Custom Speech' (aangepaste spraak) in het bovenste lint en selecteer 'Adaptation Data' (aanpassingsgegevens). Als dit de eerste keer is dat er gegevens naar de Custom Speech Service worden geüpload, ziet u een lege tabel met de naam 'Datasets' (gegevenssets).

Als u een nieuwe gegevensset wilt importeren, klikt u op de knop 'Import' in de rij 'Language Datasets' (taalgegevenssets). De site geeft dan een pagina weer voor het uploaden van een nieuwe gegevensset. Voer een Name (naam) en Description (omschrijving) in, zodat u de gegevensset in de toekomst kunt terugvinden, en kies de landinstelling. Gebruik vervolgens de knop 'Choose File' (bestand kiezen) om het tekstbestand met taalgegevens op te zoeken. Klik daarna op 'Import'. De gegevensset wordt geüpload. Afhankelijk van de grootte van de gegevensset kan het importeren enkele minuten duren.

![proberen](media/stt/speech-language-datasets-import.png)

Wanneer het importeren is voltooid, gaat u terug naar de tabel met taalgegevens. U ziet daar een vermelding die overeenkomt met uw taalgegevensset. U ziet dat er een unieke id (GUID) aan is toegewezen. De gegevens hebben ook een status die overeenkomt met hun huidige toestand. De status is 'Waiting' (wachtend) als de gegevens in de wachtrij staan voor verwerking, 'Processing' (wordt verwerkt) als ze de validatie doorlopen en 'Complete' (voltooid) wanneer ze klaar zijn voor gebruik. Door de gegevensvalidatie wordt een reeks controles op de tekst in het bestand en enige tekstnormalisering van de gegevens uitgevoerd.

Wanneer de status 'Complete' (voltooid) is, kunt u op 'View Report' (rapport bekijken) klikken om het verificatierapport van de taalgegevens te bekijken. Het aantal utterances dat wel en niet door de verificatie is gekomen, wordt weergegeven, samen met details over de mislukte utterances. In het onderstaande voorbeeld staan twee voorbeelden waarbij verificatie is mislukt vanwege onjuiste tekens (in deze gegevensset bevatte de eerste regel twee tabtekens en de tweede verschillende tekens die niet in de afdrukbare ASCII-tekenset staan, terwijl de derde regel leeg was).

![proberen](media/stt/speech-language-datasets-report.png)

Wanneer de status van de taalgegevensset 'Complete' (voltooid) is, kan deze worden gebruikt om een aangepast taalmodel te maken.

![proberen](media/stt/speech-language-datasets.png)

## <a name="create-a-custom-language-model"></a>Een aangepast taalmodel maken

Zodra de gegevens van uw taal gereed zijn, klikt u op 'Language Models' (taalmodellen) in de vervolgkeuzelijst 'Menu' om het maken van het aangepaste taalmodel te starten. Deze pagina bevat een tabel met de naam 'Language Models' met uw huidige aangepaste taalmodellen. Als u nog geen aangepaste taalmodellen hebt gemaakt, is de tabel leeg. De huidige landinstelling wordt weergegeven in de tabel naast de relevante gegevensinvoer.

De juiste landinstelling moet worden geselecteerd voordat er actie wordt ondernomen. De huidige landinstelling wordt aangegeven in de tabeltitel op alle gegevens-, model- en implementatiepagina's. Als u de landinstelling wilt wijzigen, klikt u op de knop ‘Change Locale’ (landinstelling wijzigen) onder de titel van de tabel, die u naar een bevestigingspagina voor de landinstelling leidt. Klik op ‘OK’ om terug te keren naar de tabel.

Voer op de pagina ‘Create Language Model’ (taalmodel maken) een 'Name' (naam) en 'Description' (omschrijving) in om relevante informatie over dit model bij te houden, zoals de gegevensset die wordt gebruikt. Selecteer vervolgens het 'Base Language Model' (basistaalmodel) in de vervolgkeuzelijst. Dit model is het startpunt voor uw aanpassing. U kunt kiezen uit twee basistaalmodellen. Het Search and Dictation-model (zoeken en dicteren) is geschikt voor spraak gericht op een toepassing, zoals opdrachten, zoekquery's of dicteren. Het Conversational model (conversatiemodel) is geschikt voor het herkennen van spraak in een conversatiestijl. Dit type spraak is doorgaans gericht op een andere persoon en vindt plaats in callcenters of vergaderingen. Er is ook een nieuw model met de naam 'Universal' (universeel) openbaar beschikbaar. Universal streeft ernaar om alle scenario's aan te pakken en uiteindelijk de modellen Search and Dictation en Conversational te vervangen.

5.  In het voorbeeld hieronder selecteert u nadat u het basistaalmodel hebt opgegeven de taalgegevensset die u wilt gebruiken voor de aanpassing met behulp van de vervolgkeuzelijst 'Language Data' (taalgegevens)

![proberen](media/stt/speech-language-models-create2.png)

Net als bij het maken van het akoestische model, kunt u het nieuwe model eventueel offline testen wanneer de verwerking is voltooid. Modelevaluaties vereisen een akoestische-gegevensset.

Als u uw taalmodel offline wilt testen, selecteert u het selectievakje naast 'Offline testen'. Selecteer vervolgens een akoestisch model in de vervolgkeuzelijst. Als u geen aangepaste akoestisch modellen hebt gemaakt, zijn de akoestische basismodellen van Microsoft de enige modellen in het menu. Als u een conversatie-LM-basismodel hebt gekozen, moet u hier een conversatie-AM gebruiken. Als u een LM-model voor zoeken en dicteren gebruikt, moet u een AM-model voor zoeken en dicteren selecteren.

Selecteer ten slotte de akoestische-gegevensset die u wilt gebruiken voor het uitvoeren van de evaluatie.

Wanneer u klaar bent om de verwerking te starten, drukt u op ‘Create’ (maken), waarna u naar de tabel met taalmodellen gaat. Er is een nieuwe vermelding in de tabel die overeenkomt met dit model. De status geeft de toestand aan waarin het model zich bevindt, en doorloopt verschillende statussen, waaronder ‘Waiting’ (wachtend), ‘Processing’ (wordt verwerkt) en ‘Complete’ (voltooid).

Wanneer het model de status 'Complete' heeft bereikt, kan het worden geïmplementeerd naar een eindpunt. Als u op ‘View Result’ (resultaat bekijken) klikt, worden de resultaten van het offline testen weergegeven, als dit is uitgevoerd.

Als u de 'Name' (naam) of 'Description' (omschrijving) van het model op een bepaald moment wilt wijzigen, kunt u hiervoor de koppeling 'Edit' (bewerken) in de juiste rij van de tabel met taalmodellen gebruiken.

## <a name="next-steps"></a>Volgende stappen

- [Uw proefabonnement voor Speech ophalen](https://azure.microsoft.com/try/cognitive-services/)
- [Spraak herkennen in C#](quickstart-csharp-dotnet-windows.md)
- [Git-voorbeeldgegevens](https://github.com/Microsoft/Cognitive-Custom-Speech-Service)
