---
title: 'Zelfstudie: Een taalmodel maken met Speech Service'
titlesuffix: Azure Cognitive Services
description: Leer hoe u een taalmodel maakt met Speech Service.
services: cognitive-services
author: PanosPeriorellis
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: tutorial
ms.date: 06/25/2018
ms.author: panosper
ms.openlocfilehash: b8d58450ccc5081e3be3131761d1321a32567df3
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/19/2018
ms.locfileid: "49468990"
---
# <a name="tutorial-create-a-custom-language-model"></a>Zelfstudie: Een aangepast taalmodel maken

In dit document maakt u een aangepast taalmodel. U kunt dit aangepaste taalmodel vervolgens gebruiken in combinatie met bestaande geavanceerde spraakmodellen van Microsoft om spraakinteractie toe te voegen aan uw toepassing.

In dit document wordt het volgende beschreven:
> [!div class="checklist"]
> * De gegevens voorbereiden
> * De taalgegevensset importeren
> * Het aangepaste taalmodel maken

Als u nog geen Cognitive Services-account hebt, maakt u een [gratis account](https://azure.microsoft.com/try/cognitive-services/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Controleer of uw Cognitive Services-account aan een abonnement gekoppeld is door de pagina met [Cognitive Services-abonnementen](https://customspeech.ai/Subscriptions) te openen.

U kunt verbinding maken met een in de Azure-portal gemaakt Speech Service-abonnement door de knop **Connect existing subscription** (Verbinding maken met bestaand abonnement) te selecteren.

Raadpleeg de [startpagina](get-started.md) voor informatie over het maken van een Speech Service-abonnement in de Azure-portal.

## <a name="prepare-the-data"></a>De gegevens voorbereiden

Als u een aangepast taalmodel voor uw toepassing wilt maken, moet u een lijst met voorbeeld-utterances aan het systeem verstrekken, bijvoorbeeld:

*   "De patiënt heeft sinds de afgelopen week netelroos."
*   "De patiënt had een goed genezen herniorafielitteken."

De zinnen hoeven niet volledig of grammaticaal correct te zijn, maar moeten wel de gesproken invoer weerspiegelen waarvan wordt verwacht dat het systeem deze in de praktijk tegenkomt. Deze voorbeelden moeten zowel de stijl als de inhoud weerspiegelen van de taak die de gebruikers met uw toepassing uitvoeren.

De gegevens van het taalmodel moeten in UTF-8 BOM zijn geschreven. Het tekstbestand moet één voorbeeld (zin, utterance of query) per regel bevatten.

Als u wilt dat sommige termen een hoger gewicht hebben (belangrijker zijn), kunt u aan uw gegevens verschillende utterances toevoegen die deze termen bevatten. 

De belangrijkste vereisten voor de taalgegevens worden samengevat in de volgende tabel.

| Eigenschap | Waarde |
|----------|-------|
| Tekstcodering | UTF-8 BOM|
| Aantal utterances per regel | 1 |
| Maximale bestandsgrootte | 1,5 GB |
| Opmerkingen | Vermijd tekens meer dan vier keer te herhalen, bijvoorbeeld 'aaaaa'|
| Opmerkingen | Geen speciale tekens als '\t' of andere UTF-8-teken boven U+00A1 in de [tabel met Unicode-tekens](http://www.utf8-chartable.de/)|
| Opmerkingen | URI's wordt ook geweigerd, omdat er geen unieke manier is om een URI uit te spreken|

Wanneer de tekst wordt geïmporteerd, wordt de tekst genormaliseerd zodat deze door het systeem kan worden verwerkt. Er zijn echter enkele belangrijke normalisaties die door de gebruiker moeten worden uitgevoerd _voordat_ de gegevens worden geüpload. Zie de [transcriptierichtlijnen](prepare-transcription.md) om te bepalen welke taal geschikt is om uw taalgegevens voor te bereiden.

## <a name="language-support"></a>Taalondersteuning

Zie de volledige lijst met [ondersteunde talen](language-support.md#text-to-speech) voor aangepaste **spraak-naar-tekst**-taalmodellen.



## <a name="import-the-language-data-set"></a>De taalgegevensset importeren

Selecteer de knop **Import** (importeren) in de rij **Language Datasets** (taalgegevenssets). De site geeft dan een pagina weer voor het uploaden van een nieuwe gegevensset.

Wanneer u er klaar voor bent om uw taalgegevensset te importeren, meldt u zich aan bij de [Speech Service-portal](https://customspeech.ai). Klik op de vervolgkeuzelijst **Custom Speech** (Aangepaste spraak) in het bovenste lint. Selecteer vervolgens**Adaptation Data** (Aanpassingsgegevens). Als dit de eerste keer is dat u gegevens naar Speech Services probeert te uploaden, ziet u een lege tabel met de naam **Datasets** (Gegevenssets).

Voor het importeren van een nieuwe gegevensset selecteert u de knop **Import** (Importeren) in de rij **Language Datasets** (Taalgegevenssets). Vervolgens wordt er een pagina weergegeven waar u een nieuwe gegevensset kunt uploaden. Voer een **naam** en **beschrijving** in zodat u de gegevensset later gemakkelijk kunt herkennen en kies de gewenste taalinstelling. 

Gebruik vervolgens de knop **Choose File** (Bestand kiezen) om het tekstbestand met taalgegevens op te zoeken. Selecteer **Import** (Importeren). De gegevensset wordt geüpload. Afhankelijk van de grootte van de gegevensset kan het importeren enkele minuten duren.

![Proberen](media/stt/speech-language-datasets-import.png)

Nadat het importeren is voltooid, ziet u dat de taalgegevens een vermelding hebben die overeenkomt met uw taalgegevensset. U ziet dat er een unieke id (GUID) aan is toegewezen. De gegevens hebben ook een status die overeenkomt met hun huidige toestand. De status is **Waiting** (Wachten) als de gegevens in de wachtrij staan voor verwerking, **Processing** (Wordt verwerkt) als ze de validatie doorlopen en **Complete** (Voltooid) wanneer ze klaar zijn voor gebruik. Door de gegevensvalidatie wordt een reeks controles op de tekst in het bestand uitgevoerd. Er vindt ook enige tekstnormalisering van de gegevens plaats.

Wanneer de status **Complete** (Voltooid) is, kunt u op **View Report** (Rapport bekijken) klikken om het verificatierapport van de taalgegevens te bekijken. Het aantal utterances dat wel en niet door de verificatie is gekomen, wordt weergegeven, samen met details over de mislukte utterances. In het volgende voorbeeld mislukt de verificatie van twee voorbeelden vanwege het gebruik van onjuiste tekens. (In deze gegevensset had de eerste regel twee tabtekens, had de tweede verschillende tekens die geen deel uitmaken van de ASCII-tekenset en was de derde regel leeg).

![Proberen](media/stt/speech-language-datasets-report.png)

Wanneer de status van de taalgegevensset **Complete** (Voltooid) is, kan deze worden gebruikt om een aangepast taalmodel te maken.

![Proberen](media/stt/speech-language-datasets.png)

## <a name="create-a-custom-language-model"></a>Een aangepast taalmodel maken

Zodra de gegevens van uw taal gereed zijn, selecteert u **Language Models** (Taalmodellen) in de vervolgkeuzelijst **Menu** om het maken van het aangepaste taalmodel te starten. Deze pagina bevat een tabel met de naam **Language Models** met uw huidige aangepaste taalmodellen. Als u nog geen aangepaste taalmodellen hebt gemaakt, is de tabel leeg. De huidige landinstelling wordt weergegeven in de tabel naast de relevante gegevensinvoer.

De juiste landinstelling moet worden geselecteerd voordat er actie wordt ondernomen. De huidige landinstelling wordt aangegeven in de tabeltitel op alle gegevens-, model- en implementatiepagina's. Als u de landinstelling wilt wijzigen, selecteert u de knop **Change Locale** (Landinstelling wijzigen) onder de tabeltitel.  Hiermee wordt een bevestigingspagina voor de landinstelling geopend. Selecteer **OK** om terug te keren naar de tabel.

Voer op de pagina Create Language Model (Taalmodel maken) een **naam** en **beschrijving** in om relevante informatie over dit model bij te houden, zoals de gegevensset die wordt gebruikt. Selecteer vervolgens het **Base Language Model** (Basistaalmodel) in de vervolgkeuzelijst. Dit model is het startpunt voor uw aanpassing. 

U kunt kiezen uit twee basistaalmodellen. Het Search and Dictation-model (Zoeken en dicteren) is geschikt voor spraak gericht op een toepassing, zoals opdrachten, zoekquery's of dicteren. Het Conversational model (Conversatiemodel) is geschikt voor het herkennen van spraak in een conversatiestijl. Dit type spraak is doorgaans gericht op een andere persoon en vindt plaats in callcenters of vergaderingen. 

Het Search and Dictation-model (Zoeken en dicteren) is geschikt voor spraak gericht op een toepassing, zoals opdrachten, zoekquery's of dicteren. Het Conversational model (Conversatiemodel) is geschikt voor het herkennen van spraak in een conversatiestijl. Dit type spraak is doorgaans gericht op een andere persoon en vindt plaats in callcenters of vergaderingen. Er is ook een nieuw model met de naam 'Universal' (universeel) openbaar beschikbaar. Universal streeft ernaar om alle scenario's aan te pakken en uiteindelijk de modellen Search and Dictation en Conversational te vervangen.

Zoals het voorbeeld hieronder aangeeft, gebruikt u nadat u het basistaalmodel hebt opgegeven, de vervolgkeuzelijst **Language Data** (Taalgegevens) om de taalgegevensset te selecteren die u voor de aanpassing wilt gebruiken.

![Proberen](media/stt/speech-language-models-create2.png)

Net als bij het maken van het akoestische model, kunt u het nieuwe model eventueel offline testen wanneer de verwerking is voltooid. Modelevaluaties vereisen een akoestische-gegevensset.

Als u uw taalmodel offline wilt testen, schakelt u het selectievakje naast **Offline Testing** (Offline testen) in. Selecteer vervolgens een akoestisch model in de vervolgkeuzelijst. Als u geen aangepaste akoestisch modellen hebt gemaakt, zijn de akoestische basismodellen van Microsoft de enige modellen in het menu. Als u een conversatie-LM-basismodel hebt gekozen, moet u hier een conversatie-AM gebruiken. Als u een LM-model voor zoeken en dicteren gebruikt, moet u een AM-model voor zoeken en dicteren selecteren.

Selecteer ten slotte de akoestische-gegevensset die u wilt gebruiken voor het uitvoeren van de evaluatie.

Selecteer **Create** (Maken) als u klaar bent om met de verwerking te beginnen. Vervolgens wordt de tabel met taalmodellen weergegeven. Er is een nieuwe vermelding in de tabel die overeenkomt met dit model. De status geeft de toestand aan waarin het model zich bevindt, en doorloopt verschillende statussen, waaronder **Waiting** (Wachten), **Processing** (Wordt verwerkt) en **Complete** (Voltooid).

Wanneer het model de status **Complete** heeft bereikt, kan het worden geïmplementeerd naar een eindpunt. Als u **View Result** (Resultaat bekijken) selecteert, worden de resultaten van het offline testen weergegeven, als u dat hebt uitgevoerd.

Als u de **naam** of **beschrijving** van het model op een bepaald moment wilt wijzigen, kunt u hiervoor de koppeling **Edit** (Bewerken) in de juiste rij van de tabel met taalmodellen gebruiken.

## <a name="next-steps"></a>Volgende stappen

- [Uw proefabonnement voor Speech Services ophalen](https://azure.microsoft.com/try/cognitive-services/)
- [Spraak herkennen in C#](quickstart-csharp-dotnet-windows.md)
- [Git-voorbeeldgegevens](https://github.com/Microsoft/Cognitive-Custom-Speech-Service)
