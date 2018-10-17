---
title: Een akoestisch model maken met de Speech Service - Azure Cognitive Services
description: Leer hoe u een akoestisch model maakt met de Speech Service in Azure Cognitive Services.
services: cognitive-services
author: PanosPeriorellis
ms.service: cognitive-services
ms.component: speech-service
ms.topic: tutorial
ms.date: 06/25/2018
ms.author: panosper
ms.openlocfilehash: 39e591f6154573bb25fccc423ff63a82f282beaf
ms.sourcegitcommit: 7bc4a872c170e3416052c87287391bc7adbf84ff
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/02/2018
ms.locfileid: "48017347"
---
# <a name="tutorial-create-a-custom-acoustic-model"></a>Zelfstudie: een aangepast akoestisch model maken

Het maken van een aangepast akoestisch model is handig als uw toepassing is bedoeld voor gebruik in een bepaalde omgeving, zoals een auto, met specifieke opnameapparaten of omstandigheden, of door een bepaalde gebruikerspopulatie. Voorbeelden zijn spraak met een accent, specifieke achtergrondgeluiden of het gebruik van een specifieke microfoon voor opname.

In dit artikel leert u het volgende:
> [!div class="checklist"]
> * De gegevens voorbereiden
> * De akoestische-gegevensset importeren
> * Het aangepaste akoestische model maken

Als u nog geen Azure Cognitive Services-account hebt, maakt u een [gratis account](https://azure.microsoft.com/try/cognitive-services) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Zorg ervoor dat uw Cognitive Services-account is gekoppeld aan een abonnement door de pagina met [Cognitive Services-abonnementen](https://cris.ai/Subscriptions) te openen.

U kunt verbinding maken met een in de Azure-portal gemaakt Speech Service-abonnement door **Connect existing subscription** (Verbinding maken met bestaand abonnement) te selecteren.

Raadpleeg [Speech Service gratis uitproberen](get-started.md) voor informatie over het maken van een Speech Service-abonnement in de Azure-portal.

## <a name="prepare-the-data"></a>De gegevens voorbereiden

Om een ​​akoestisch model voor een bepaald domein aan te passen, is een verzameling spraakgegevens vereist. Deze verzameling kan variëren van een aantal utterances tot enkele honderden uren aan spraak. De verzameling bestaat uit een set audiobestanden met spraakgegevens en een tekstbestand met transcripties van elk audiobestand. De audiogegevens moeten representatief zijn voor het scenario waarin u de herkenning wilt gebruiken.

Bijvoorbeeld:

* Als u spraak beter wilt herkennen in een lawaaierige fabrieksomgeving, moeten de audiobestanden bestaan ​​uit mensen die in een luidruchtige fabriek praten.
* Als u bijvoorbeeld de prestaties van een bepaalde spreker wilt optimaliseren&mdash;bijvoorbeeld als u alle gesprekken van president Roosevelt bij de open haard wilt transcriberen&mdash;moeten de audiobestanden alleen uit veel voorbeelden van die spreker bestaan.

Een akoestische-gegevensset voor het aanpassen van het akoestische model bestaat uit twee delen: (1) een set audiobestanden die de spraakgegevens bevatten en (2) een bestand dat de transcripties van alle audiobestanden bevat.

### <a name="audio-data-recommendations"></a>Aanbevelingen voor audiogegevens

* Alle audiobestanden in de gegevensset moeten worden opgeslagen in de WAV (RIFF)-audio-indeling.
* De audio moet een samplefrequentie van 8 kHz of 16 kHz hebben en de sample-waarden moeten worden opgeslagen als niet-gecomprimeerde pulscodemodulatie (PCM) 16-bits gehele getallen met teken (shorts).
* Alleen audiobestanden met één kanaal (mono) worden ondersteund.
* De audiobestanden moeten tussen 100 microseconden en 1 minuut lang zijn. Elk audiobestand zou idealiter moeten beginnen en eindigen met minstens 100 microseconden stilte, en ergens tussen 500 microseconden en 1 seconde is gebruikelijk.
* Als uw gegevens achtergrondgeluid bevatten, is het aan te raden ook enkele voorbeelden met langere stiltesegmenten&mdash;bijvoorbeeld een paar seconden&mdash;in uw gegevens op te nemen, vóór en/of na de gesproken inhoud.
* Elk audiobestand moet uit één utterance bestaan&mdash;bijvoorbeeld één zin bij dicteren, één zoekopdracht of één beurt van een dialoogsysteem.
* Elk audiobestand in de gegevensset moet een unieke bestandsnaam hebben en de extensie moet 'wav' zijn.
* De set audiobestanden moet in één map zonder submappen worden geplaatst, en de volledige set audiobestanden moet als één ZIP-bestandsarchief worden verpakt.

> [!NOTE]
> Gegevensimport via de webportal is momenteel beperkt tot 2 GB, dus dit is de maximale grootte van een set met akoestische gegevens. Deze grootte komt overeen met ongeveer 17 uur op 16 kHz opgenomen audio of 34 uur op 8 kHz opgenomen audio. De belangrijkste vereisten voor de audiogegevens zijn samengevat in de volgende tabel:
>

| Eigenschap | Waarde |
|---------- |----------|
| Bestandsindeling | RIFF (WAV) |
| Samplefrequentie | 8.000 Hz of 16.000 Hz |
| Kanalen | 1 (mono) |
| Sample-indeling | PCM, 16-bits gehele getallen |
| Bestandsduur | 0,1 seconde < duur < 60 seconden |
| Stiltekraag | > 0,1 seconde |
| Archiefindeling | .zip |
| Maximale grootte archief | 2 GB |

> [!NOTE]
> In bestandsnamen mogen alleen Latijnse tekens worden gebruikt en moeten de indeling 'bestandsnaam.extensie' hebben.

## <a name="language-support"></a>Taalondersteuning

Voor een volledige lijst met talen die worden ondersteund voor aangepaste **Spraak naar tekst**-taalmodellen, ziet u deze tabel met [ondersteunde talen voor de Speech Service](language-support.md#speech-to-text).

### <a name="transcriptions-for-the-audio-dataset"></a>Transcripties voor de audiogegevensset

De transcripties voor alle WAV-bestanden moeten worden opgenomen in één bestand met tekst zonder opmaak. Elke regel van het transcriptiebestand moet de naam van een van de audiobestanden bevatten, gevolgd door de bijbehorende transcriptie. De bestandsnaam en transcriptie moeten worden gescheiden door een tab (\t).

  Bijvoorbeeld:
```
  speech01.wav  speech recognition is awesome
  speech02.wav  the quick brown fox jumped all over the place
  speech03.wav  the lazy dog was not amused
```
> [!NOTE]
> Transcriptie moet worden gecodeerd als UTF-8 BOM (Byte Order Mark).

De tekst van de transcripties wordt genormaliseerd zodat ze door het systeem kunnen worden verwerkt. Er zijn echter enkele belangrijke normalisaties die door de gebruiker moeten worden uitgevoerd _voordat_ de gegevens worden geüpload naar de Custom Speech Service. Zie [Transcriptierichtlijnen voor het gebruik van de Speech Service](prepare-transcription.md) voor de juiste taal die moet worden gebruikt bij het voorbereiden van uw transcripties.

Volg de stappen in de volgende secties met behulp van de [Speech Service-portal](https://cris.ai).

## <a name="import-the-acoustic-dataset"></a>De akoestische-gegevensset importeren

Als u de audiobestanden en transcripties hebt voorbereid, zijn deze klaar om te worden geïmporteerd in de servicewebportal.

Als u wilt gaan importeren, controleert u eerst of u bent aangemeld bij de [Speech Service-portal](https://cris.ai). Vervolgens selecteert u in de vervolgkeuzelijst **Custom Speech** op het lint de optie **Adaptation Data**. Als dit de eerste keer is dat u gegevens uploadt naar de Custom Speech Service, ziet u een lege tabel met de naam **Datasets**. 

Selecteer in de rij **Acoustic Datasets** de knop **Import**. Op de site ziet u nu een pagina voor het uploaden van een nieuwe gegevensset.

![De pagina Import Acoustic Data](media/stt/speech-acoustic-datasets-import.png)

Geef de gewenste gegevens op voor **Name** en **Description**. Gebruikersvriendelijke beschrijvingen zijn handig voor het bijhouden van de verschillende gegevenssets die u uploadt. 

Selecteer **Browse** in de vakken **Transcriptions file (.txt)** en **Audio files (.zip)**, en selecteer vervolgens het transcriptiebestand met tekst zonder opmaak en het zip-archief met WAV-bestanden. Wanneer de voorbereiding is voltooid, selecteert u **Import** om uw gegevens te uploaden. Uw gegevens worden geüpload. Bij grotere gegevenssets kan het importproces enkele minuten duren.

Wanneer het uploaden is voltooid, gaat u terug naar de tabel **Acoustic Datasets**. Hier ziet u een vermelding die overeenkomt met uw akoestische-gegevensset. U ziet dat er een unieke id (GUID) aan is toegewezen. De status is *NotStarted* als de gegevens in de wachtrij staan voor verwerking, *Running* als ze worden gevalideerd en *Complete* wanneer ze klaar zijn voor gebruik.

Gegevensvalidatie omvat een reeks controles op de audiobestanden om de bestandsindeling, lengte en samplingfrequentie te verifiëren, en op de transcriptiebestanden om de bestandsindeling te verifiëren en enige tekstnormalisatie uit te voeren.

Wanneer de status *Succeeded* is, kunt u **Details** selecteren om het verificatierapport van de akoestische gegevens te bekijken. Het aantal utterances dat wel en niet door de verificatie is gekomen, wordt weergegeven, samen met details over de mislukte utterances. In het voorbeeld in de volgende afbeelding is de verificatie van twee WAV-bestanden mislukt vanwege een onjuiste audio-indeling. In deze gegevensset heeft één bestand een onjuiste samplefrequentie en het andere bestand een onjuiste bestandsindeling.

![De pagina Adaptation Data Details](media/stt/speech-acoustic-datasets-report.png)

Als u de naam of beschrijving van de gegevensset wilt wijzigen, selecteert u de koppeling **Edit** en wijzigt u de invoer. Het is niet mogelijk om de vermeldingen voor transcripties of audiobestanden te wijzigen.

## <a name="create-a-custom-acoustic-model"></a>Een aangepast akoestisch model maken

Wanneer de status van de akoestische-gegevensset *Complete* is, kunt u de set gebruiken om een aangepast akoestisch model te maken. Selecteer hiervoor **Acoustic Models** in de vervolgkeuzelijst **Custom Speech**. In een tabel met het label **Your models** ziet u een lijst van al uw aangepaste akoestische modellen. De tabel is leeg als u de functie nog niet eerder hebt gebruikt. De titel van de tabel bevat de huidige landinstelling. Op dit moment kunt u alleen akoestische modellen maken voor Amerikaans-Engels.

Als u een nieuw model wilt maken, selecteert u **Create New** onder de titel van de tabel. Voer zoals eerder een naam en omschrijving in om u te helpen dit model te identificeren. In het veld **Description** kunt u bijvoorbeeld noteren welk startmodel en welke akoestische-gegevensset zijn gebruikt om het model te maken. 

Selecteer vervolgens in de vervolgkeuzelijst **Base Acoustic Model** een basismodel. Dit basismodel is het startpunt voor uw aanpassing. Er zijn twee akoestische basismodellen waaruit u kunt kiezen:
* Het model **Microsoft Search and Dictation AM** is geschikt voor spraak gericht op een toepassing, zoals opdrachten, zoekquery's of dicteren. 
* Het **Microsoft Conversational Model** is geschikt voor het herkennen van spraak die wordt gehanteerd in een conversatiestijl. Dit type spraak is doorgaans gericht op een andere persoon en vindt plaats in callcenters of vergaderingen. 

Latentie voor gedeeltelijke resultaten in Conversational-modellen is hoger dan in Search and Dictation-modellen.

> [!NOTE]
> We zijn momenteel ons nieuwe **universele** model aan het uitrollen, dat bedoeld is om alle scenario's aan te pakken. De bovengenoemde modellen blijven ook openbaar beschikbaar.

Selecteer vervolgens in de vervolgkeuzelijst **Acoustic Data** de akoestische gegevens die u wilt gebruiken voor het uitvoeren van de aanpassing.

![De pagina Create Acoustic Model](media/stt/speech-acoustic-models-create2.png)

Wanneer de verwerking is voltooid, kunt u er desgewenst voor kiezen een nauwkeurigheidstest van uw nieuwe model uit te voeren. Er wordt dan met behulp van het aangepaste akoestische model een evaluatie van spraak-naar-tekst gedaan op een opgegeven akoestische-gegevensset, waarna de resultaten worden gerapporteerd. Schakel het selectievakje **Accuracy Testing** in als u deze test wilt uitvoeren. Selecteer vervolgens een taalmodel in de vervolgkeuzelijst. Als u geen aangepaste taalmodellen hebt gemaakt, staan ​​alleen de basistaalmodellen in de vervolgkeuzelijst. Zie de zelfstudie [Een aangepast taalmodel maken](how-to-customize-language-model.md) voor het selecteren van het meest geschikte taalmodel.

Selecteer ten slotte de akoestische-gegevensset die u wilt gebruiken voor het evalueren van het aangepaste model. Als u een nauwkeurigheidstest uitvoert, is het voor een ​​realistisch beeld van de prestaties van het model belangrijk om een andere akoestische-gegevensset te selecteren dan de set die u hebt gebruikt voor het maken van het model. Door de nauwkeurigheid te testen op de trainingsgegevens, kunt u niet evalueren hoe het aangepaste model presteert onder echte omstandigheden. Het resultaat zal te optimistisch zijn. Ook is het zo dat nauwkeurigheidstests beperkt zijn tot 1.000 utterances. Als de akoestische-gegevensset voor het testen groter is, worden alleen de eerste 1.000 utterances geëvalueerd.

Wanneer u klaar bent om het aanpassingsproces te starten, selecteert u **Create**.

De tabel Acoustic Models bevat een nieuwe vermelding die overeenkomt met dit nieuwe model. De tabel geeft ook de status van het proces aan: *Waiting*, *Processing* of *Complete*.

![De pagina Acoustic Models](media/stt/speech-acoustic-models-creating.png)

## <a name="next-steps"></a>Volgende stappen

- [Uw proefabonnement voor Speech Service ophalen](https://azure.microsoft.com/try/cognitive-services/)
- [Spraak herkennen in C#](quickstart-csharp-dotnet-windows.md)
- [Git-voorbeeldgegevens](https://github.com/Microsoft/Cognitive-Custom-Speech-Service)
