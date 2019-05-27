---
title: Testgegevens voorbereiden voor de aangepaste spraak - spraakservices
titlesuffix: Azure Cognitive Services
description: Of u test om te zien hoe nauwkeurig Microsoft-spraakherkenning is of uw eigen modellen trainen, moet u gegevens (in de vorm van audio en/of tekst). Op deze pagina behandelen we de typen gegevens, hoe ze worden gebruikt en hoe deze te beheren.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: erhopf
ms.openlocfilehash: de2f1009c574d9768330d4e6a38a219ba1f81daa
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2019
ms.locfileid: "66237958"
---
# <a name="prepare-data-for-custom-speech"></a>Gegevens voorbereiden voor de aangepaste spraak

Of u test om te zien hoe nauwkeurig Microsoft-spraakherkenning is of uw eigen modellen trainen, moet u gegevens in de vorm van audio en tekst. Op deze pagina behandelen we de typen gegevens, hoe ze worden gebruikt en hoe deze te beheren.

## <a name="data-types"></a>Gegevenstypen

Deze tabel bevat de geaccepteerde gegevenstypen, wanneer elk gegevenstype moet worden gebruikt en de aanbevolen hoeveelheid. Niet alle gegevens van het type is vereist om een model te maken. Vereisten voor gegevens varieert afhankelijk van of u een test maakt of een model te trainen.

| Gegevenstype | Gebruikt van testen | Hoeveelheid | Gebruikt voor training | Hoeveelheid |
|-----------|-----------------|----------|-------------------|----------|
| [Audio](#audio-data-for-testing) | Ja<br>Gebruikt voor visueel inspecteren | 5 + audiobestanden | Nee | N/a |
| [Audio + Human etiket transcripties](#audio--human-labeled-transcript-data-for-testingtraining) | Ja<br>Gebruikt voor het evalueren van nauwkeurigheid | 0,5 - 5 uur audio | Ja | 1 - 1000 uur aan audio |
| [Tekst](##related-text-data-for-training) | Nee | N/a | Ja | 1-200 MB van tekst |

Bestanden moeten worden gegroepeerd op type naar een gegevensset en geüpload als een zip-bestand. Elke gegevensset kan slechts één gegevenstype bevatten.

## <a name="upload-data"></a>Gegevens uploaden

Wanneer u klaar om uw gegevens te uploaden bent, klikt u op **gegevens uploaden** om te starten van de wizard en maken van uw eerste gegevensset. U wordt gevraagd een spraak-gegevenstype voor uw gegevensset selecteren voordat u uw gegevens te uploaden.

![Selecteer audio van de spraak-Portal](./media/custom-speech/custom-speech-select-audio.png)

Elke gegevensset die u uploadt, moet voldoen aan de vereisten voor het gegevenstype dat u kiest. Het is belangrijk om correct uw gegevens voordat deze wordt geüpload. Dit zorgt ervoor dat de gegevens nauwkeurig wordt verwerkt door de Custom Speech service. Vereisten worden vermeld in de volgende secties.

Nadat uw gegevensset is geüpload, hebt u enkele opties:

* U kunt navigeren naar de **testen** tabblad en visueel inspecteren alleen audio- of audio + human etiket transcriptie gegevens.
* U kunt navigeren naar de **Training** tabblad en ons audio + menselijke transcriptie gegevens of gegevens van de tekst met het trainen van een aangepast model.

## <a name="audio-data-for-testing"></a>Audiogegevens voor het testen

Audiogegevens is geoptimaliseerd voor het testen van de nauwkeurigheid van de Microsoft baseline spraak-naar-tekst model of een aangepast model. Houd er rekening mee, audiogegevens is gebruikt om de nauwkeurigheid van de spraakherkenning met betrekking tot een specifiek model prestaties controleren. Als u de nauwkeurigheid van een model kwantificeren wilt, gebruikt u [audio + human etiket transcriptie gegevens](#audio--human-labeled-transcript-data-for-testingtraining).

Deze tabel gebruiken om ervoor te zorgen dat uw audio-bestanden zijn ingedeeld voor gebruik met aangepaste spraak:

| Eigenschap | Value |
|----------|-------|
| Bestandsindeling | RIFF (WAV) |
| Steekproeffrequentie | 8000 Hz of 16.000 Hz |
| Kanalen | 1 (mono) |
| Maximale lengte per audio | 2 uur |
| Voorbeeldindeling | PCM, 16-bits |
| Indeling archiveren | .zip |
| Grootte van maximaal archiveren | 2 GB |

Als uw audio niet voldoet aan deze eigenschappen of als u controleren wilt als dit het geval is, het is raadzaam downloaden [sox](http://sox.sourceforge.net) om te controleren of de audio converteren. Hieronder volgen enkele voorbeelden van hoe elk van deze activiteiten kan worden uitgevoerd via de opdrachtregel:

| Activiteit | Description | SOx opdracht |
|----------|-------------|-------------|
| Controleer de audio-indeling | Deze opdracht gebruiken om te controleren of de audio-indeling. | `sox --i <filename>` |
| Audio-indeling converteren | Met deze opdracht kunt u het audio bestand converteren naar één kanaal, 16-bits, 16 KHz. | `sox <input> -b 16 -e signed-integer -c 1 -r 16k -t wav <output>.wav` |

## <a name="audio--human-labeled-transcript-data-for-testingtraining"></a>Audio + human etiket transcript gegevens voor testen/training

Als u wilt meten de nauwkeurigheid van de nauwkeurigheid van de spraak-naar-tekst van Microsoft bij het verwerken van uw audio-bestanden, moet u human etiket transcripties (-per-woord) voor vergelijking opgeven. Hoewel transcriptie human etiket vaak tijdrovend is, is het nodig zijn om te evalueren nauwkeurigheid en met het trainen van het model voor uw use-cases. Houd er rekening mee, is de verbeteringen in de opname alleen nuttig als de gegevens die zijn opgegeven. Om die reden is het belangrijk dat alleen hoogwaardige Transcripten worden geüpload.  

| Eigenschap | Value |
|----------|-------|
| Bestandsindeling | RIFF (WAV) |
| Steekproeffrequentie | 8000 Hz of 16.000 Hz |
| Kanalen | 1 (mono) |
| Maximale lengte per audio | 60 s |
| Voorbeeldindeling | PCM, 16-bits |
| Indeling archiveren | .zip |
| Maximale zip-grootte | 2 GB |

Om problemen te verhelpen, zoals word verwijderen of vervangen, een aanzienlijke hoeveelheid gegevens is vereist om te verbeteren. Over het algemeen is het raadzaam voor per woord transcripties van ongeveer 10-1000 uur aan audio. De transcripties voor alle WAV-bestanden moeten worden opgenomen in één bestand met tekst zonder opmaak. Elke regel van het transcriptiebestand moet de naam van een van de audiobestanden bevatten, gevolgd door de bijbehorende transcriptie. De bestandsnaam en transcriptie moeten worden gescheiden door een tab (\t).

  Bijvoorbeeld:
```
  speech01.wav  speech recognition is awesome
  speech02.wav  the quick brown fox jumped all over the place
  speech03.wav  the lazy dog was not amused
```
> [!NOTE]
> Transcriptie moet worden gecodeerd als UTF-8 BOM (Byte Order Mark).

De tekst van de transcripties wordt genormaliseerd zodat ze door het systeem kunnen worden verwerkt. Er zijn echter enkele belangrijke normalisaties die door de gebruiker moeten worden uitgevoerd _voordat_ de gegevens worden geüpload naar de Custom Speech Service. Zie voor de juiste taal moet worden gebruikt bij het voorbereiden van uw transcripties [over het maken van een transcriptie human etiket](how-to-custom-speech-human-labeled-transcriptions.md)

Nadat u uw audio-bestanden en de bijbehorende transcripties hebt verzameld, moeten ze voordat u uploadt naar de portal voor aangepaste spraak als één ZIP-bestand worden verpakt. Dit is een voorbeeld van de gegevensset met drie audio-bestanden en een bestand transcriptie human gelabeld:

![Selecteer audio van de spraak-Portal](./media/custom-speech/custom-speech-audio-transcript-pairs.png)

## <a name="related-text-data-for-training"></a>Gerelateerde tekstgegevens voor training

Als u beschikt over productnamen of functies die uniek zijn en u wilt controleren of dat ze goed worden herkend, is het belangrijk om gerelateerde gegevens voor de training. Twee typen verwante tekstgegevens kunnen worden opgegeven voor het verbeteren van opname:

| Gegevenstype | Hoe deze gegevens verbetert de opname |
|-----------|------------------------------------|
| Uitingen en/of zinnen | Deze kunnen nauwkeurigheid verbeteren bij het herkennen van productnamen of branchespecifieke vocabulaire binnen de context van een zin. |
| Uitspraak | Deze kunnen uitspraak van ongebruikelijke termen, acroniemen of andere woorden met niet-gedefinieerde uitspraak verbeteren. |

Uitingen kunnen worden opgegeven als een of meerdere tekstbestanden. Hoe dichter de tekst van de gegevens zijn voor wat wordt gesproken kan worden, des te groter de kans dat de nauwkeurigheid is verbeterd. Uitspraak moeten worden opgegeven als een enkele tekst-bestand. Alles kan worden geleverd als een enkel zip-bestand en geüpload naar de portal voor aangepaste spraak.

### <a name="guidelines-to-create-an-utterances-file"></a>Richtlijnen voor het maken van een bestand uitingen

Voor het maken van een aangepast model met behulp van tekst, moet u een lijst van voorbeeld-uitingen opgeven. Deze uitingen hoeft te worden van volledige zinnen of grammaticaal corrigeren, maar ze moeten de gesproken invoer die u in productie verwacht nauwkeurig weergeven. Als u wilt dat bepaalde voorwaarden meer gewicht, kunt u meerdere zinnen kunt toevoegen aan het bestand gerelateerde gegevens die deze specifieke termen bevatten.

Deze tabel gebruiken om ervoor te zorgen dat uw gerelateerde gegevensbestand voor uitingen correct is ingedeeld:

| Eigenschap | Value |
|----------|-------|
| Tekstcodering | UTF-8 BOM |
| Aantal utterances per regel | 1 |
| Maximale bestandsgrootte | 200 MB |

Bovendien moet u rekening voor de volgende beperkingen:

* Vermijd tekens meer dan vier keer herhalen. Bijvoorbeeld: "aaaa" of 'uuuu'.
* Gebruik geen speciale tekens of UTF-8 tekens boven U + 00A1.
* URI's worden geweigerd.

### <a name="guidelines-to-create-a-pronunciation-file"></a>Richtlijnen voor het maken van een uitspraak van bestand

Als er ongewone voorwaarden zonder standard uitspraak die uw gebruikers ondervinden of gebruiken, kunt u een aangepaste uitspraak van bestand ter verbetering van spraakherkenning kunt opgeven.

> [!IMPORTANT]
> Het wordt niet aanbevolen deze functie wilt gebruiken om te wijzigen van de uitspraak van veelvoorkomende woorden.

Dit bevat voorbeelden van een gesproken utterance en een aangepaste uitspraak van voor elke:

| Gesproken formulier | Formulier herkend/weergegeven |
|--------------|--------------------------|
| drie c-p-o | 3CPO |  
| c n t k | CNTK |
| ik drievoudige e | IEEE |

Het formulier gesproken is de fonetische reeks voluit geschreven. Het kan bestaan uit letters, woorden, lettergrepen of een combinatie van alle drie.

Aangepaste uitspraak is beschikbaar in het Engels (en-US) en Duits (nl-nl). Deze tabel ziet u ondersteunde tekens per taal:

| Taal | Landinstelling | Tekens |
|----------|--------|------------|
| Nederlands | en-US | a, b, c, d, e, f, g, h, i, j, k, l, m, n, o, p, q, r, s, t, u, v, w, x, y, z |
| Duits | de-DE | ä, ö, ü, a, b, c, d, e, f, g, h, i, "j", k, l, m, n, o, p, q, r, s, t, u, v, w, x, y, z |

Deze tabel gebruiken om ervoor te zorgen dat uw gerelateerde gegevensbestand voor uitspraak correct is ingedeeld. Uitspraak van bestanden klein zijn en mag niet meer dan een paar kB's.

| Eigenschap | Value |
|----------|-------|
| Tekstcodering | Stuklijst UTF-8 (ANSI wordt ook ondersteund voor het Engels) |
| Totaal aantal uitspraken per regel | 1 |
| Maximale bestandsgrootte | 1 MB (1 KB voor de laag gratis) |

## <a name="next-steps"></a>Volgende stappen

* [Controleer uw gegevens](how-to-custom-speech-inspect-data.md)
* [Evalueren van uw gegevens](how-to-custom-speech-evaluate-data.md)
* [Uw model te trainen](how-to-custom-speech-train-model.md)
* [Uw model implementeren](how-to-custom-speech-deploy-model.md)
