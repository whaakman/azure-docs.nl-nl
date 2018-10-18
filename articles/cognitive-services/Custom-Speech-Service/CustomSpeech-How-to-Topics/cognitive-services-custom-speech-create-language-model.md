---
title: Zelfstudie voor het maken van een taalmodel met Custom Speech Service - Microsoft Cognitive Services | Microsoft Docs
description: In deze zelfstudie leert u hoe een taalmodel maakt met de Custom Speech Service in Microsoft Cognitive Services.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: tutorial
ms.date: 05/03/2017
ms.author: panosper
ms.openlocfilehash: 6af2da9ffc7678a58fcf1c647ba89c586066d2ad
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49339093"
---
# <a name="tutorial-create-a-custom-language-model"></a>Zelfstudie: Een aangepast taalmodel maken

[!INCLUDE [Deprecation note](../../../../includes/cognitive-services-custom-speech-deprecation-note.md)]

In deze zelfstudie maakt u een aangepaste taalmodel voor tekstquery's of utterances waarvan u verwacht dat gebruikers deze zeggen of typen in een toepassing. U kunt dit aangepaste taalmodel vervolgens gebruiken in combinatie met bestaande geavanceerde spraakmodellen van Microsoft om spraakinteractie toe te voegen aan uw toepassing.

In deze zelfstudie leert u het volgende:
> [!div class="checklist"]
> * De gegevens voorbereiden
> * De taalgegevensset importeren
> * Het aangepaste taalmodel maken

Als u nog geen Cognitive Services-account hebt, maakt u een [gratis account](https://cris.ai) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Zorg ervoor dat uw Cognitive Services-account is gekoppeld aan een abonnement door de pagina met [Cognitive Services-abonnementen](https://cris.ai/Subscriptions) te openen.

Als er geen abonnementen worden vermeld, kunt u Cognitive Services een account voor u laten maken door te klikken op de knop **Gratis abonnement ophalen**. Of u kunt verbinding maken met een Custom Search Service-abonnement dat is gemaakt in Azure Portal door te klikken op de knop **Verbinding maken met bestaand abonnement**.

Zie [Een Cognitive Services-API's-account maken in de Azure portal](../../cognitive-services-apis-create-account.md) voor meer informatie over het maken van een Custom Search Service-abonnement in Azure Portal.

## <a name="prepare-the-data"></a>De gegevens voorbereiden

Als u een aangepast taalmodel voor uw toepassing wilt maken, moet u een lijst met voorbeeld-utterances aan het systeem verstrekken, bijvoorbeeld:

*   "Hij heeft sinds de afgelopen week netelroos."
*   "Het hernialitteken van de patiënt is goed genezen."

De zinnen hoeven niet volledig of grammaticaal correct te zijn en moeten de gesproken invoer weerspiegelen waarvan u verwacht dat het systeem deze in de praktijk tegenkomt. Deze voorbeelden moeten zowel de stijl als de inhoud weerspiegelen van de taak die de gebruikers met uw toepassing uitvoeren.

De taalmodelgegevens moeten worden geschreven in een bestand met tekst zonder opmaak met behulp van US-ASCII- of UTF-8, afhankelijk van de landinstelling. Voor en-US worden beide coderingen ondersteund. Voor zh-CN wordt alleen UTF-8 ondersteund (BOM is optioneel). Het tekstbestand moet één voorbeeld (zin, utterance of query) per regel bevatten.

Als u wilt dat enkele zinnen meer gewicht (belang) hebben, kunt u deze meerdere keren aan uw gegevens toevoegen. Een goed aantal herhalingen is tussen 10-100. Als u deze op 100 normaliseert, kunt u zinnen eenvoudig ten opzichte hiervan gewicht geven.

De belangrijkste vereisten voor de taalgegevens worden samengevat in de volgende tabel.

| Eigenschap | Waarde |
|----------|-------|
| Tekstcodering | en-US: VS-ACSII- of UTF-8- of zh-CN: UTF-8|
| Aantal utterances per regel | 1 |
| Maximale bestandsgrootte | 200 MB |
| Opmerkingen | vermijd tekens meer dan 4 keer te herhalen, bijvoorbeeld 'aaaaa'|
| Opmerkingen | geen speciale tekens als '\t' of andere UTF-8-teken boven U+00A1 in de [tabel met Unicode-tekens](http://www.utf8-chartable.de/)|
| Opmerkingen | URI's wordt ook geweigerd, omdat er geen unieke manier is om een URI uit te spreken|

Wanneer de tekst wordt geïmporteerd, wordt de tekst genormaliseerd zodat deze door het systeem kan worden verwerkt. Er zijn echter enkele belangrijke normalisaties die door de gebruiker moeten worden uitgevoerd _voordat_ de gegevens worden geüpload. Zie de [Transcriptierichtlijnen](cognitive-services-custom-speech-transcription-guidelines.md) om te bepalen welke taal geschikt is bij het voorbereiden van uw taalgegevens.

## <a name="import-the-language-data-set"></a>De taalgegevensset importeren

Klik op de knop 'Import' in de rij 'Acoustic Datasets'. De site geeft dan een pagina weer voor het uploaden van een nieuwe gegevensset.

Wanneer u er klaar voor bent om uw taalgegevensset te importeren, meld u zich aan bij het [Custom Speech Service-portal](https://cris.ai).  Klik op de vervolgkeuzelijst 'Custom Speech' in het bovenste lint en selecteer 'Adaptation Data'. Als dit de eerste keer is dat u gegevens naar de Custom Speech Service uploadt, ziet u een lege tabel met de naam 'Datasets'.

Als u een nieuwe gegevensset wilt importeren, klikt u op de knop 'Import' in de rij 'Language Datasets'. De site geeft dan een pagina weer voor het uploaden van een nieuwe gegevensset. Voer een naam en beschrijving in, zodat u de gegevensset in de toekomst kunt terugvinden. Gebruik vervolgens de knop 'Choose File' om het tekstbestand met taalgegevens op te zoeken. Klik daarna op 'Import'. De gegevensset wordt geüpload. Afhankelijk van de grootte van de gegevensset kan dit enkele minuten duren.

![proberen](../../../media/cognitive-services/custom-speech-service/custom-speech-language-datasets-import.png)

Wanneer het importeren is voltooid, gaat u terug naar de tabel met taalgegevens. U ziet daar een vermelding die overeenkomt met uw taalgegevensset. U ziet dat er een unieke id (GUID) aan is toegewezen. De gegevens hebben ook een status die overeenkomt met hun huidige toestand. De status is 'Waiting' (wachtend) als de gegevens in de wachtrij staan voor verwerking, 'Processing' (wordt verwerkt) als ze de validatie doorlopen en 'Complete' (voltooid) wanneer ze klaar zijn voor gebruik. Door de gegevensvalidatie wordt een reeks controles op de tekst in het bestand en enige tekstnormalisering van de gegevens uitgevoerd.

Wanneer de status 'Complete' (voltooid) is, kunt u op 'View Report' (rapport bekijken) klikken om het verificatierapport van de taalgegevens te bekijken. Het aantal utterances dat wel en niet door de verificatie zijn gekomen, wordt weergegeven, samen met details over de mislukte utterances. In het onderstaande voorbeeld staan twee voorbeelden waarbij verificatie is mislukt vanwege onjuiste tekens (in deze gegevensset bevat de eerste twee emoticons en de tweede verschillende tekens die niet in de afdrukbare ASCII-tekenset staan).

![proberen](../../../media/cognitive-services/custom-speech-service/custom-speech-language-datasets-report.png)

Wanneer de status van de taalgegevensset 'Complete' (voltooid) is, kan deze worden gebruikt om een aangepast taalmodel te maken.

![proberen](../../../media/cognitive-services/custom-speech-service/custom-speech-language-datasets.png)

## <a name="create-a-custom-language-model"></a>Een aangepast taalmodel maken

Zodra de gegevens van uw taal gereed zijn, klikt u op 'Language Models' (taalmodellen) in de vervolgkeuzelijst 'Menu' om het maken van het aangepaste taalmodel te starten. Deze pagina bevat een tabel met de naam 'Language Models' met uw huidige aangepaste taalmodellen. Als u nog geen aangepaste taalmodellen hebt gemaakt, is de tabel leeg. De huidige landinstelling wordt weergegeven in de titel van de tabel. Als u een taalmodel voor een andere taal wilt maken, klikt u op 'Landinstelling wijzigen'. Meer informatie over ondersteunde talen vindt u in de sectie [Landinstelling wijzigen](cognitive-services-custom-speech-change-locale.md). Klik op de koppeling 'Nieuw' onder de titel van de tabel om een nieuw model te maken.

Voer op de pagina 'Taalmodel maken' een 'Naam' en 'Beschrijving' in om relevante informatie over dit model bij te houden, zoals de gegevensset die wordt gebruikt. Selecteer vervolgens het 'Base Language Model' (basistaalmodel) in de vervolgkeuzelijst. Dit model is het startpunt voor uw aanpassing. U kunt kiezen uit twee standaardtaalmodellen. De _Microsoft LM voor zoeken en dicteren_ is geschikt is voor spraak gericht op een toepassing, zoals opdrachten, zoekquery's of dicteren. De _Microsoft LM voor conversaties_ is geschikt voor het herkennen van spraak die wordt gesproken in een eigen stijl. Dit type spraak is doorgaans gericht op een andere persoon en vindt plaats in callcenters of vergaderingen.

Nadat u het basistaalmodel hebt opgegeven, selecteert u de taalgegevensset die u wilt gebruiken voor de aanpassing met behulp van de vervolgkeuzelijst 'Taalgegevens'

![proberen](../../../media/cognitive-services/custom-speech-service/custom-speech-language-models-create2.png)

Net als bij het maken van het akoestische model, kunt u het nieuwe model eventueel offline testen wanneer de verwerking is voltooid. Omdat dit een evaluatie van de prestaties van spraak-naar-tekst is, is voor offline testen een akoestische gegevensset vereist.

Als u uw taalmodel offline wilt testen, selecteert u het selectievakje naast 'Offline testen'. Selecteer vervolgens een akoestisch model in de vervolgkeuzelijst. Als u geen aangepaste akoestisch modellen hebt gemaakt, zijn de akoestische basismodellen van Microsoft de enige modellen in het menu. Als u een conversatie-LM-basismodel hebt gekozen, moet u hier een conversatie-AM gebruiken. Als u een LM-model voor zoeken en dicteren gebruikt, moet u een AM-model voor zoeken en dicteren selecteren.

Selecteer ten slotte de akoestische gegevensset die u wilt gebruiken voor het uitvoeren van de evaluatie.

Druk op 'Maken' als u klaar bent om met de verwerking te beginnen. U gaat nu terug naar de tabel met taalmodellen. Er is een nieuwe vermelding in de tabel die overeenkomt met dit model. De status geeft de toestand aan waarin het model zich bevindt, en doorloopt verschillende statussen, waaronder ‘Waiting’ (wachtend), ‘Processing’ (wordt verwerkt) en ‘Complete’ (voltooid).

Wanneer het model de status 'Complete' heeft bereikt, kan het worden geïmplementeerd naar een eindpunt. Als u op ‘View Result’ (resultaat bekijken) klikt, worden de resultaten van het offline testen weergegeven, als dit is uitgevoerd.

Als u de 'Name' (naam) of 'Description' (omschrijving) van het model op een bepaald moment wilt wijzigen, kunt u hiervoor de koppeling 'Edit' (bewerken) in de juiste rij van de tabel met taalmodellen gebruiken.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u een aangepast taalmodel voor gebruik met tekst ontwikkeld. Als u een aangepast akoestisch model voor gebruik met audiobestanden en transcripties wilt maken, gaat u verder met de zelfstudie over het maken van een akoestisch model.

> [!div class="nextstepaction"]
> [Een aangepast akoestisch model maken](cognitive-services-custom-speech-create-acoustic-model.md)
