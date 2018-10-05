---
title: 'Zelfstudie: een akoestisch model maken - Custom Speech Service'
titlesuffix: Azure Cognitive Services
description: In deze zelfstudie leert u hoe u een akoestisch model maakt met de Custom Speech Service.
services: cognitive-services
author: PanosPeriorellis
manager: cgronlun
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: tutorial
ms.date: 05/03/2017
ms.author: panosper
ROBOTS: NOINDEX
ms.openlocfilehash: 72c5a0dfb8f33f273ba850378c1fefeef82b4d7a
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/26/2018
ms.locfileid: "47220212"
---
# <a name="tutorial-create-a-custom-acoustic-model"></a>Zelfstudie: een aangepast akoestisch model maken

In deze zelfstudie maakt u een aangepast akoestisch model voor spraakgegevens dat u uw toepassing verwacht te herkennen. Het maken van een aangepast akoestisch model is handig als uw toepassing is bedoeld voor gebruik in een bepaalde omgeving, zoals een lawaaierige fabriek, of door een bepaalde gebruikerspopulatie.

In deze zelfstudie leert u het volgende:
> [!div class="checklist"]
> * De gegevens voorbereiden
> * De akoestische-gegevensset importeren
> * Het aangepaste akoestische model maken

Als u nog geen Cognitive Services-account hebt, maakt u een [gratis account](https://cris.ai) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Zorg ervoor dat uw Cognitive Services-account is gekoppeld aan een abonnement door de pagina met [Cognitive Services-abonnementen](https://cris.ai/Subscriptions) te openen.

Als er geen abonnementen worden vermeld, kunt u Cognitive Services een account voor u laten maken door te klikken op de knop **Gratis abonnement ophalen**. Of u kunt verbinding maken met een Custom Search Service-abonnement dat is gemaakt in de Azure-portal door te klikken op de knop **Verbinding maken met bestaand abonnement**.

Zie [Een Cognitive Services-API's-account maken in de Azure-portal](../../cognitive-services-apis-create-account.md) voor meer informatie over het maken van een Custom Search Service-abonnement in de Azure-portal.

## <a name="prepare-the-data"></a>De gegevens voorbereiden

Om het ​​akoestische model aan een bepaald domein aan te passen, is een verzameling spraakgegevens vereist. Deze verzameling bestaat uit een reeks audiobestanden met spraakgegevens en een tekstbestand met transcripties van elk audiobestand. De audiogegevens moeten representatief zijn voor het scenario waarin u de herkenning wilt gebruiken.

Bijvoorbeeld:

*   Als u spraak beter wilt herkennen in een lawaaierige fabrieksomgeving, moeten de audiobestanden bestaan ​​uit mensen die in een luidruchtige fabriek spreken.
<a name="Preparing data to customize the acoustic model"></a>
*   Als u bijvoorbeeld de prestaties van een enkele luidspreker wilt optimaliseren, bijvoorbeeld als u alle Fireside-chats van president Roosevelt wilt transcriberen, moeten de audiobestanden alleen uit veel voorbeelden van die spreker bestaan.

Een akoestische-gegevensset voor het aanpassen van het akoestische model bestaat uit twee delen: (1) een set audiobestanden die de spraakgegevens bevatten en (2) een bestand dat de transcripties van alle audiobestanden bevat.

### <a name="audio-data-recommendations"></a>Aanbevelingen voor audiogegevens

*   Alle audiobestanden in de gegevensset moeten worden opgeslagen in de WAV (RIFF)-audio-indeling.
*   De audio moet een samplefrequentie van 8 kHz of 16 kHz hebben en de sample-waarden moeten worden opgeslagen als niet-gecomprimeerde PCM 16-bits gehele getallen met teken (shorts).
*   Alleen audiobestanden met één kanaal (mono) worden ondersteund.
*   De audiobestanden moeten tussen de 100 ms en 1 minuut lang zijn. Elk audiobestand moet idealiter beginnen en eindigen met een stilte van ten minste 100 ms (500 ms tot 1 seconde is gebruikelijk).
*   Als uw gegevens achtergrondgeluid bevatten, is het aan te raden ook enkele voorbeelden met langere stiltesegmenten, bijvoorbeeld een paar seconden, in uw gegevens op te nemen, vóór en of na de spraakinhoud.
*   Elk audiobestand moet uit één utterance bestaan, bijvoorbeeld één zin bij dicteren, één zoekopdracht, of één beurt van een dialoogsysteem.
*   Elk audiobestand in de gegevensset moet een unieke bestandsnaam en de extensie ‘wav’ hebben.
*   De set audiobestanden moet in één map zonder submappen worden geplaatst, en de volledige set audiobestanden moet als één ZIP-bestandsarchief worden verpakt.

> [!NOTE]
> Gegevensimport via het webportaal is momenteel beperkt tot 2 GB, dus dit is de maximale grootte van een set met akoestische gegevens. Dit komt overeen met ongeveer 17 uur op 16 kHz opgenomen audio of 34 uur op 8 kHz opgenomen audio. De belangrijkste vereisten voor de audiogegevens worden samengevat in de volgende tabel.
>

| Eigenschap | Waarde |
|---------- |----------|
| Bestandsindeling | RIFF (WAV) |
| Samplefrequentie | 8.000 Hz of 16.000 Hz |
| Kanalen | 1 (mono) |
| Sample-indeling | PCM, 16-bits gehele getallen |
| Bestandsduur | 0,1 seconde < duur < 60 seconden |
| Stiltekraag | > 0,1 seconde |
| Archiefindeling | Zip |
| Maximale grootte archief | 2 GB |

### <a name="transcriptions"></a>Transcripties

De transcripties voor alle WAV-bestanden moeten worden opgenomen in één bestand met tekst zonder opmaak. Elke regel van het transcriptiebestand moet de naam van een van de audiobestanden bevatten, gevolgd door de bijbehorende transcriptie. De bestandsnaam en transcriptie moeten worden gescheiden door een tab (\t).

  Bijvoorbeeld:
```
  speech01.wav  speech recognition is awesome

  speech02.wav  the quick brown fox jumped all over the place

  speech03.wav  the lazy dog was not amused
```

De tekst van de transcripties wordt genormaliseerd zodat ze door het systeem kunnen worden verwerkt. Er zijn echter enkele belangrijke normalisaties die door de gebruiker moeten worden uitgevoerd _voordat_ de gegevens worden geüpload naar de Custom Speech Service. Raadpleeg de sectie over [transcriptierichtlijnen](cognitive-services-custom-speech-transcription-guidelines.md) voor de juiste taal bij het voorbereiden van uw transcripties.

De volgende stappen worden uitgevoerd met behulp van de [Custom Speech Service-portal](https://cris.ai). 

## <a name="import-the-acoustic-data-set"></a>De akoestische-gegevensset importeren

Nadat de audiobestanden en transcripties zijn voorbereid, zijn ze klaar om te worden geïmporteerd in de servicewebportal.

Zorg er eerst voor dat u bent aangemeld bij de [Custom Speech Service-portal](https://cris.ai). Klik op de vervolgkeuzelijst 'Custom Speech' (aangepaste spraak) in het bovenste lint en selecteer 'Adaptation Data' (aanpassingsgegevens). Als dit de eerste keer is dat u gegevens naar de Custom Speech Service uploadt, ziet u een lege tabel met de naam 'Datasets' (gegevenssets). 

Klik op de knop 'Import' in de rij 'Acoustic Datasets' (akoestische-gegevenssets). De site geeft dan een pagina weer voor het uploaden van een nieuwe gegevensset.

![proberen](../../../media/cognitive-services/custom-speech-service/custom-speech-acoustic-datasets-import.png)

Voer een naam en beschrijving in de tekstvakken _Name_ en _Description_ in. Deze zijn handig voor het bijhouden van verschillende gegevenssets die u uploadt. Klik vervolgens op ‘Choose File’ (bestand kiezen) bij ‘Transcription File’ (transcriptiebestand) en ‘WAV files’ (WAV-bestanden) en selecteer respectievelijk het tekstbestand met transcripties en het ZIP-archief met WAV-bestanden. Wanneer dit is voltooid, klikt u op ‘Import’ (importeren) om uw gegevens te uploaden. Uw gegevens worden geüpload. Bij grotere gegevenssets kan dit enkele minuten duren.

Wanneer het uploaden is voltooid, gaat u terug naar de tabel ‘Acoustic Datasets’. U ziet daar een vermelding die overeenkomt met uw akoestische-gegevensset. U ziet dat er een unieke id (GUID) aan is toegewezen. De gegevens hebben ook een status die overeenkomt met hun huidige toestand. De status is 'Waiting' (wachtend) als de gegevens in de wachtrij staan voor verwerking, 'Processing' (wordt verwerkt) als ze de validatie doorlopen en 'Complete' (voltooid) wanneer ze klaar zijn voor gebruik.

Gegevensvalidatie omvat een reeks controles op de audiobestanden om de bestandsindeling, lengte en samplingfrequentie te verifiëren, en op de transcriptiebestanden om de bestandsindeling te verifiëren en enige tekstnormalisatie uit te voeren.

Wanneer de status ‘Complete’ (voltooid) is, kunt u op ‘Details’ klikken om het verificatierapport van de akoestische gegevens te bekijken. Het aantal utterances dat wel en niet door de verificatie is gekomen, wordt weergegeven, samen met details over de mislukte utterances. In het onderstaande voorbeeld zijn twee WAV-bestanden niet geverifieerd vanwege een onjuiste audio-indeling (in deze dataset had het ene bestand een onjuiste samplingfrequentie en het andere een onjuiste bestandsindeling).

![proberen](../../../media/cognitive-services/custom-speech-service/custom-speech-acoustic-datasets-report.png)

Als u op een gegeven moment de naam of beschrijving van de gegevensset wilt wijzigen, klikt u op de koppeling ‘Bewerken’. U kunt de audiobestanden of transcripties niet wijzigen.

## <a name="create-a-custom-acoustic-model"></a>Een aangepast akoestisch model maken

Wanneer de status van de akoestische-gegevensset taal 'Complete' (voltooid) is, kan deze worden gebruikt om een aangepast akoestisch model te maken. Klik hiervoor op ‘Acoustic Models’ (akoestische modellen) in de vervolgkeuzelijst 'Custom Speech' (aangepaste spraak). U ziet een tabel met de naam ‘Your models’ (uw modellen) met al uw aangepaste akoestische modellen. Deze tabel is leeg als dit uw eerste gebruik is. De huidige landinstelling wordt weergegeven in de titel van de tabel. Op dit moment kunnen akoestische modellen alleen worden gemaakt voor Amerikaans Engels.

Als u een nieuw model wilt maken, klikt u op 'Nieuw maken' onder de titel van de tabel. Voer zoals eerder een naam en omschrijving in om u te helpen dit model te identificeren. In het veld ‘Description’ (beschrijving) kan bijvoorbeeld worden vastgelegd welk startmodel en welke akoestische-gegevensset zijn gebruikt om het model te maken. Selecteer vervolgens een akoestisch basismodel in de vervolgkeuzelijst. Het basismodel is het model dat het startpunt is voor uw aanpassing. Er zijn twee akoestische basismodellen waaruit u kunt kiezen. Het _Microsoft Search and Dictation AM_ (Microsoft AM voor zoeken en dicteren) is geschikt voor spraak gericht op een toepassing, zoals opdrachten, zoekquery's of dicteren. Het _Microsoft Conversational model_ (conversatiemodel van Microsoft) is geschikt voor het herkennen van spraak in een conversatiestijl. Dit type spraak is doorgaans gericht op een andere persoon en vindt plaats in callcenters of vergaderingen. Latentie voor gedeeltelijke resultaten in Conversational-modellen is hoger dan in Search and Dictation-modellen.

Selecteer vervolgens de akoestische gegevens die u wilt gebruiken om de aanpassing uit te voeren met behulp van het vervolgkeuzemenu.

![proberen](../../../media/cognitive-services/custom-speech-service/custom-speech-acoustic-models-create2.png)

Wanneer de verwerking is voltooid, kunt u het nieuwe model eventueel offline testen. Dit zal een spraak-naar-tekst-evaluatie op een opgegeven akoestische-gegevensset uitvoeren met behulp van het aangepaste akoestische model en de resultaten rapporteren. Selecteer het selectievakje ‘Accuracy Testing’ (nauwkeurigheidstests) als u deze test wilt uitvoeren. Selecteer vervolgens een taalmodel in de vervolgkeuzelijst. Als u geen aangepaste taalmodellen hebt gemaakt, staan ​​alleen de basistaalmodellen in de vervolgkeuzelijst. Zie de [beschrijving](cognitive-services-custom-speech-create-language-model.md) van de basistaalmodellen in de gids en selecteer het model dat het meest geschikt is.

Selecteer ten slotte de akoestische-gegevensset die u wilt gebruiken voor het evalueren van het aangepaste model. Als u nauwkeurigheidstests uitvoert, is het belangrijk om akoestische gegevens te selecteren die verschillen van die waarmee het model is gemaakt, om een ​​realistisch beeld te krijgen van de prestaties van het model. Merk ook op dat de offlinetest beperkt is tot 1000 utterances. Als de akoestische-gegevensset voor het testen groter dan dat is, worden alleen de eerste 1000 utterances geëvalueerd.

Wanneer u klaar bent om het aanpassingsproces te starten, drukt u op ‘Create’ (maken).

U ziet nu een nieuw item in de tabel met akoestische modellen dat overeenkomt met dit nieuwe model. De status van het proces wordt weergegeven in de tabel. De status kan ‘Waiting’ (wachtend), ‘Processing’ (wordt verwerkt) of ‘Complete’ (voltooid) zijn.

![proberen](../../../media/cognitive-services/custom-speech-service/custom-speech-acoustic-models-creating.png)

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u een aangepast akoestisch model ontwikkeld voor gebruik met audiobestanden en transcripties. Als u een aangepast taalbestand wilt maken voor gebruik met tekstbestanden, gaat u verder met de zelfstudie voor het maken van een aangepast taalmodel.

> [!div class="nextstepaction"]
> [Een aangepast akoestisch model maken](cognitive-services-custom-speech-create-language-model.md)
