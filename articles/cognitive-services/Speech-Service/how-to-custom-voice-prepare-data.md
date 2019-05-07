---
title: Over het voorbereiden van gegevens voor aangepaste spraak - spraakservices
titlesuffix: Azure Cognitive Services
description: Maak een aangepaste stem voor uw merk met Azure Speech Services. U studio opnamen en de bijbehorende scripts opgeven, de service genereert een unieke stem-model dat is afgestemd op de geregistreerde stem. Deze stem gebruiken om na te bootsen gesproken tekst in uw producten, -hulpprogramma's en toepassingen.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: erhopf
ms.openlocfilehash: 18e1bb486c47baf7648a74e31451e2db73f72250
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65156857"
---
# <a name="prepare-data-to-create-a-custom-voice"></a>Voorbereiden van gegevens voor het maken van een aangepaste spraak

Wanneer u klaar om te maken van een aangepaste spraak stem voor uw toepassing bent, is de eerste stap voor het verzamelen van audio-opnamen en de bijbehorende scripts om te beginnen het stem-model te trainen. De service gebruikt deze gegevens om een unieke stem afgestemd op de toon in de opnamen maken. Nadat u de stem hebt getraind, kunt u beginnen stof voor de synthese gesproken tekst in uw toepassingen.

U kunt beginnen met een kleine hoeveelheid gegevens voor het maken van een bewijs van concept. Echter, hoe meer gegevens die u opgeeft, de natuurlijker uw aangepaste gesproken geluid wordt. Voordat u uw eigen stem-model te trainen kunt, moet u audio-opnamen en de bijbehorende tekst transcripties. Op deze pagina wordt gecontroleerd gegevenstypen, hoe ze worden gebruikt en over het beheren van elk.

## <a name="data-types"></a>Gegevenstypen

Een stem training-gegevensset bevat audio-opnamen, en een tekstbestand met de bijbehorende transcripties. Elk geluidsbestand moet een enkel utterance (één zin of een enkel inschakelen voor een dialoogvenster systeem) bevatten en minder dan 15 seconden lang zijn.

In sommige gevallen moet u mogelijk niet de juiste gegevensset klaar en wilt testen van de aangepaste Spraaktraining met beschikbaar audiobestanden, korte of lange, met of zonder transcripties. We bieden extra (bèta) zodat u kunt uw audio segmenteren in uitingen en voorbereiden van Transcripten met behulp van de [Batch transcriptie API](batch-transcription.md).

Deze tabel bevat de gegevenstypen en hoe deze worden gebruikt om een aangepaste tekst-naar-spraak-model te maken.

| Gegevenstype | Description | Wanneer gebruikt u dit? | Extra service die vereist is | Aantal voor het trainen van een model | Locale(s) |
| --------- | ----------- | ----------- | --------------------------- | ----------------------------- | --------- |
| **Afzonderlijke uitingen + overeenkomende transcript** | Een verzameling (.zip) van de audio-bestanden (.wav) als afzonderlijke uitingen. Elke audiobestand moet op 15 seconden of minder tekens, gekoppeld aan een opgemaakte transcript (.txt). | Professionele opnamen met de bijbehorende transcripties | Gereed voor training. | Er is geen vaste vereiste voor en-US en zh-CN. Meer dan 2000 + afzonderlijke uitingen voor andere landinstellingen. | Alle aangepaste spraak-landinstellingen |
| **Lange audio + transcript (bèta)** | Lange, niet-gesegmenteerde audio-bestanden (meer dan 20 seconden), een verzameling (.zip) gekoppeld aan een transcript (.txt) waarin alle gesproken woorden. | Hebt u audio-bestanden en overeenkomende transcripties, maar ze niet zijn gesegmenteerd in uitingen. | Segmentering (met behulp van transcriptie batch).<br>Audio-indeling transformatie indien nodig. | Er is geen vaste vereiste voor en-US en zh-CN. | `en-US` en `zh-CN` |
| **Alleen audio (bèta)** | Een verzameling (.zip) van de audio-bestanden zonder een transcript. | U hoeft alleen audio-bestanden beschikbaar zijn, zonder transcripties. | Segmentering + transcript te genereren (met behulp van transcriptie batch).<br>Audio-indeling transformatie indien nodig.| Er is geen vaste vereiste voor `en-US` en `zh-CN`. | `en-US` en `zh-CN` |

Bestanden moeten worden gegroepeerd op type naar een gegevensset en geüpload als een zip-bestand. Elke gegevensset kan slechts één gegevenstype bevatten.

> [!NOTE]
> Het maximum aantal gegevenssets kunnen worden geïmporteerd per abonnement is 10 .zip bestanden gratis abonnementsgebruikers (F0) en 500 voor abonnementsgebruikers van de standard-(S0).

## <a name="individual-utterances--matching-transcript"></a>Afzonderlijke uitingen + overeenkomende transcript

U kunt opnamen van afzonderlijke uitingen en het overeenkomende transcript op twee manieren voorbereiden. Ofwel een script schrijven en deze gelezen door een stem talent of openbaar beschikbare audio gebruiken en deze moet transcriberen naar tekst. Als u de laatste, bewerk disfluencies uit de audio-bestanden, zoals 'um' en andere geluiden opvullende, haperen, mumbled woorden of mispronunciations.

Produceren van een goede spraakstijl opnamen in een rustige ruimte te maken met een hoge kwaliteit microfoon. Consistente volume, prijs, spreekstijl inspiratie en expressieve gebaren van spraak spreken zijn essentieel.

> [!TIP]
> Voor het maken van een stem voor gebruik in productieomgevingen, is het raadzaam dat een professionele opname studio en toon talent te gebruiken. Zie voor meer informatie, [vastleggen voice-voorbeelden voor een aangepaste spraak](https://review.docs.microsoft.com/azure/cognitive-services/speech-service/record-custom-voice-samples).

### <a name="audio-files"></a>Audio-bestanden

Elke audiobestand moet een enkel utterance (één zin of één schakelen van een dialoogvenster systeem), bevatten minder dan 15 seconden lang. Alle bestanden moeten zich in dezelfde gesproken taal. Meertalige aangepaste spraak stemmen worden niet ondersteund, met uitzondering van de Chinees-Engels-bi-talen. Elke audiobestand moet een unieke numerieke bestandsnaam met de extensie filename .wav hebben.

Volg deze richtlijnen bij het voorbereiden van audio.

| Eigenschap | Value |
| -------- | ----- |
| Bestandsindeling | RIFF (.wav) gegroepeerd in een ZIP-bestand |
| Samplefrequentie | ten minste 16.000 Hz |
| Voorbeeldindeling | PCM, 16-bits |
| Bestandsnaam | Numerieke waarde, met de extensie. Er zijn geen dubbele namen toegestaan. |
| Lengte van de audio | Korter zijn dan 15 seconden |
| Indeling archiveren | .zip |
| Grootte van maximaal archiveren | 200 MB |

> [!NOTE]
> wav-bestanden met een lager is dan 16.000 Hz samplefrequentie worden geweigerd. Als een ZIP-bestand wav-bestanden met verschillende voorbeeld-tarieven bevat, worden alleen die gelijk is aan of hoger is dan 16.000 Hz wordt geïmporteerd. De portal momenteel invoer ZIP-archieven 200 MB. Meerdere archieven kunnen echter worden geüpload.

### <a name="transcripts"></a>Transcripten

Het bestand transcriptie is een bestand met tekst zonder opmaak. Gebruik deze richtlijnen voor het voorbereiden van uw transcripties.

| Eigenschap | Value |
| -------- | ----- |
| Bestandsindeling | Tekst zonder opmaak (*.txt) |
| De coderingsindeling | ANSI/ASCII, UTF-8, UTF-8-BOM, UTF-16-LE of UTF-16-BE. Voor zh-CN, ANSI/ASCII- en UTF-8 coderingen niet ondersteund. |
| Aantal utterances per regel | **Een** -elke regel van het bestand transcriptie moet de naam van een van de audio-bestanden, gevolgd door de bijbehorende transcriptie bevatten. De bestandsnaam en transcriptie moeten worden gescheiden door een tab (\t). |
| Maximale bestandsgrootte | 50 MB |

Hieronder volgt een voorbeeld van hoe de Transcripten utterance door utterance zijn ingedeeld in een txt-bestand:

```
0000000001[tab] This is the waistline, and it's falling.
0000000002[tab] We have trouble scoring.
0000000003[tab] It was Janet Maslin.
```
Het is belangrijk dat de Transcripten nauwkeurige transcripties 100% van de bijbehorende audio zijn. Fouten in de Transcripten vindt kwaliteitsverlies tijdens de training.

> [!TIP]
> Bij het bouwen van productie-naar-spraak stemmen, selecteer uitingen (of scripts schrijven), die in rekening met zowel fonetische dekking en efficiëntie. Problemen met het ophalen van de resultaten wilt u dat? [Neem contact op met de aangepaste spraak](mailto:speechsupport@microsoft.com) team te vinden out meer over ons een raadplegen.

## <a name="long-audio--transcript-beta"></a>Lange audio + transcript (bèta)

In sommige gevallen wordt u mogelijk niet zijn gesegmenteerd audio beschikbaar. We bieden een service (bèta) via de portal voor aangepaste gesproken kunt u segmenteren lang audiobestanden en transcripties te maken. Houd er rekening mee, deze service wordt in rekening gebracht voor het abonnementsgebruik van uw voor spraak-naar-tekst.

> [!NOTE]
> De service voor lange-audio segmentering gebruik van de functie van de batch transcriptie van spraak naar tekst, die alleen ondersteuning biedt voor abonnementsgebruikers van de standard-(S0). Tijdens de verwerking van de segmentatie, uw audio-bestanden en de Transcripten wordt ook worden verzonden naar de Custom Speech service om de opname-model te verfijnen, zodat de nauwkeurigheid van uw gegevens kan worden verbeterd. Er zijn geen gegevens blijft behouden tijdens dit proces. Nadat de segmentatie is voltooid, wordt alleen de uitingen gesegmenteerd en hun transcripties toewijzing worden opgeslagen voor het downloaden en training.

### <a name="audio-files"></a>Audio-bestanden

Volg deze richtlijnen bij het voorbereiden van audio van segmentering.

| Eigenschap | Value |
| -------- | ----- |
| Bestandsindeling | RIFF (.wav) met een samplefrequentie van ten minste 16 khz-16-bits in PCM of .mp3 met een bitsnelheid van ten minste 256 KBps, gegroepeerd in een ZIP-bestand |
| Bestandsnaam | Alleen ASCII-tekens. Unicode-tekens in de naam van de mislukt (bijvoorbeeld de Chinese tekens en symbolen zoals ":"). Er zijn geen dubbele namen toegestaan. |
| Lengte van de audio | Meer dan 20 seconden |
| Indeling archiveren | .zip |
| Grootte van maximaal archiveren | 200 MB |

Alle audio-bestanden moeten worden gegroepeerd in een zip-bestand. Het is OK wav-bestanden en MP3-bestanden in één audio zip plaatsen, maar er is geen submap is toegestaan in het zip-bestand. U kunt bijvoorbeeld een zip-bestand met een audio-bestand met de naam 'kingstory.wav', 45 seconden jaren en een andere één met de naam 'queenstory.mp3", 200-seconde-lang, zonder eventuele submappen uploaden. Alle MP3-bestanden omgezet in de indeling .wav na de verwerking.

### <a name="transcripts"></a>Transcripten

Transcripten moeten worden voorbereid voor de specificaties die worden vermeld in deze tabel. Elke audiobestand moet overeenkomen met een transcript.

| Eigenschap | Value |
| -------- | ----- |
| Bestandsindeling | Tekst zonder opmaak (*.txt) gegroepeerd in een ZIP |
| Bestandsnaam | Gebruik dezelfde naam als de overeenkomende audiobestand |
| De coderingsindeling | UTF-8-stuklijst |
| Aantal utterances per regel | Geen limiet |
| Maximale bestandsgrootte | 50 MILJOEN |

Alle Transcripten bestanden in dit gegevenstype moeten worden gegroepeerd in een zip-bestand. Er is geen submap is toegestaan in het zip-bestand. Bijvoorbeeld, u hebt een zip-bestand met een audio-bestand met de naam 'kingstory.wav' 45 seconden lang geüpload en een andere één met de naam 'queenstory.mp3', 200 seconden lang. U moet een andere zip-bestand met twee transcripties, één met de naam 'kingstory.txt', de andere één 'queenstory.txt' uploaden. Binnen elk bestand met tekst zonder opmaak krijgt u de volledige juiste transcriptie voor de overeenkomende audio.

Nadat uw gegevensset is geüpload, zullen we helpen u de audiobestand segmenteren in uitingen op basis van het transcript opgegeven. U kunt de gesegmenteerde uitingen en de overeenkomende transcripties controleren door te downloaden van de gegevensset. De unieke id's wordt automatisch toegewezen aan de gesegmenteerde uitingen. Het is belangrijk dat u ervoor zorgen de Transcripten die u opgeeft zijn 100% betrouwbaar. Fouten in de Transcripten kunnen de nauwkeurigheid verminderen tijdens de audio Segmentatie en verder kwaliteitsverlies introduceren in de fase van de training die wordt geleverd later.

## <a name="audio-only-beta"></a>Alleen audio (bèta)

Als u geen transcripties voor uw audio-opnamen, gebruikt u de **alleen Audio** optie om uw gegevens te uploaden. Ons systeem kunt u segmenteren en audiobestanden transcriberen. Houd er rekening mee, deze service wordt tellen mee voor het abonnementsgebruik van uw voor spraak-naar-tekst.

Volg deze richtlijnen bij het voorbereiden van audio.

> [!NOTE]
> De service voor lange-audio segmentering gebruik van de functie van de batch transcriptie van spraak naar tekst, die alleen ondersteuning biedt voor abonnementsgebruikers van de standard-(S0).

| Eigenschap | Value |
| -------- | ----- |
| Bestandsindeling | RIFF (.wav) met een samplefrequentie van ten minste 16 khz-16-bits in PCM of .mp3 met een bitsnelheid van ten minste 256 KBps, gegroepeerd in een ZIP-bestand |
| Bestandsnaam | Alleen ASCII-tekens. Unicode-tekens in de naam van de mislukt (bijvoorbeeld de Chinese tekens en symbolen zoals ":"). Er zijn geen dubbele naam toegestaan. |
| Lengte van de audio | Meer dan 20 seconden |
| Indeling archiveren | .zip |
| Grootte van maximaal archiveren | 200 MB |

Alle audio-bestanden moeten worden gegroepeerd in een zip-bestand. Er is geen submap is toegestaan in het zip-bestand. Als uw gegevensset is geüpload, zullen we helpen u de audiobestand segmenteren in uitingen op basis van onze service transcriptie van spraak batch. De unieke id's wordt automatisch toegewezen aan de gesegmenteerde uitingen. Die overeenkomen met de uitgeschreven worden via spraakherkenning gegenereerd. Alle MP3-bestanden omgezet in de indeling .wav na de verwerking. U kunt de gesegmenteerde uitingen en de overeenkomende transcripties controleren door te downloaden van de gegevensset.

## <a name="next-steps"></a>Volgende stappen

- [Maken van een aangepaste spraak](how-to-custom-voice-create-voice.md)
- [Handleiding voor het: Registreren van uw stem-voorbeelden](record-custom-voice-samples.md)
