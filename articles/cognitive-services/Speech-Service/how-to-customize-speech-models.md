---
title: Spraakherkenning aanpassen op tekst modellen | Microsoft Docs
description: Spraakherkenning door aan te passen Speech Text modellen te verbeteren.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
manager: noellelacharite
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/07/2018
ms.author: v-jerkin
ms.openlocfilehash: 380c585f57737c0aa4ec99303c52d4567500b5f4
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345331"
---
# <a name="customize-speech-to-text-models-using-speech-service"></a>Met behulp van spraak service 'Spraak-tekst'-modellen aanpassen

Om te kunnen behalen betere spraak herkenningsresultaten, kunt u drie modellen die worden gebruikt door aanpassen met de service spraak de **spraak naar tekst** API. De modellen worden automatisch getraind met voorbeeldgegevens die u opgeeft.

| Model | Voorbeeldgegevens | Doel |
|-------|---------------|---------|
| Akoestisch model      | Spraak, tekst | De geluiden (Fonemen) die zijn gekoppeld aan bepaalde woorden aanpassen. Training van een nieuwe accent of een ander dialect, spreken omgeving, enzovoort. |
| Taalmodel      | Tekst | De woorden die bekend zijn bij de service en hoe ze worden gebruikt in utterances aanpassen. Toevoegen van technische termen, lokale namen, enzovoort. |
| Uitspraak | Tekst | Herkenning van lastige woorden, verbindingen en afkortingen verbeteren. Bijvoorbeeld: toewijzen 'Zie threepio' om te worden herkend als 'C3PO' |

Na het maken van nieuwe modellen, moet u een aangepaste eindpunt dat gebruikmaakt van uw model voor een of meer van de bovenstaande doeleinden maken. U kunt ook dat een basismodel geleverd door de service spraak als u gebruiken wilt, bijvoorbeeld een aangepaste akoestisch model en niet een taalmodel van de standaard. Vervolgens gebruikt u het aangepaste eindpunt in plaats van het standaardeindpunt voor REST-aanvragen. Elk eindpunt heeft een bijbehorende *implementatie-ID* zodat deze kan worden gebruikt met de SDK spraak.

Aanpassing van alle modellen wordt gedaan door de [aangepaste spraak portal](https://www.cris.ai/).

## <a name="language-support"></a>Taalondersteuning

De volgende talen worden ondersteund voor aangepaste **spraak naar tekst** taal modellen.

| Code | Taal |
|-|-|
| nl-NL | Engels (Verenigde Staten) 
| zh-CN | Chinees 
| SP SP | Spaans (Spanje) 
| fr-FR | Frans (Frankrijk) 
| IT-IT | Italiaans 
| de-DE | Duits
| pt-BR | Portugees (Brazilië)
| ru-RU | Russisch
| JP JP | Japans
| ar bijvoorbeeld | Arabisch (Egypte)

Aangepaste akoestisch modellen ondersteunen alleen Amerikaans Engels (en-US). Chinees akoestisch gegevenssets kunnen echter worden geïmporteerd voor het testen van modellen Chinees.

Aangepaste uitspraak ondersteunt alleen Amerikaans Engels (en-US) en Duits (nl-nl) op dit moment.

## <a name="prepare-data-sets"></a>Gegevenssets voorbereiden

Elk type model vereist enigszins andere gegevens en opmaak, zoals hier wordt beschreven.

| Model | Wat u levert      |
|-------|-----------------------|
| Akoestisch | Een ZIP-bestand met audio-bestanden van de volledige utterances en een tekstbestand met transcriptie van deze bestanden. Elke regel van het bestand moet bestaan uit de naam van het bestand, een tabblad (ASCII-9) en de tekst.|
| Taal | Een tekstbestand met één utterance per regel. |
| Uitspraak | Een tekstbestand met een hint uitspraak op elke regel. Elke hint wordt een formulier weergeven (een woord of afkorting), gevolgd door een tabblad (ASCII-9) en het gesproken formulier (de gewenste uitspraak).  |

Tekstbestanden moeten volgen de [tekst schrijffouten richtlijnen](prepare-transcription.md) voor de taal van het model.

## <a name="prepare-audio-files"></a>Geluidsbestanden voorbereiden

Geluidsbestanden voor akoestisch modellen moeten worden opgenomen in een representatieve locatie door diverse representatieve gebruikers (tenzij het doel is om te optimaliseren herkenning van één spreker), met behulp van een microfoon vergelijkbaar met wat uw gebruikers hebben. De vereiste indeling van alle audio voorbeelden wordt in deze tabel beschreven.

| Eigenschap | Vereiste waarde |
|----------|------|
Bestandsindeling | RIFF (WAV)
Samplefrequentie | 8000 Hz of 16.000 Hz
Kanalen | 1 (mono)
Voorbeeld-indeling | PCM, 16-bits geheel getal
De bestandsduur van het | Tussen 0,1 en 60 seconden
Stilte halsband | 0,1 seconde
Archiefindeling | Postcode
De archiefgrootte van de maximale | 2 GB

Als u bij het trainen van een model om te werken in veel ruis veroorzaken achtergrond, zoals een fabriek of een auto, opgenomen in een paar seconden van typische achtergrondruis aan het begin of einde van een aantal voorbeelden. Neem geen voorbeelden ruis alleen-lezen.

## <a name="upload-data-sets"></a>Gegevenssets uploaden

Voor het maken van een aangepaste model eerst uw gegevens uploaden en vervolgens beginnen met de training.

1.  Meld u aan bij de [aangepaste spraak portal](https://www.cris.ai/).

1.  Kies het type van de gegevensset die u wilt maken op basis van de **aangepaste spraak** menu - aanpassing gegevens voor akoestisch modellen, taal-gegevens voor de taal modellen of uitspraak. Er verschijnt een lijst met bestaande gegevenssets van dat type (indien aanwezig).

1. De taal kiezen door te klikken op **wijziging landinstelling**.

1.  Klik op **importeren nieuwe** en geef een naam en beschrijving voor de nieuwe gegevensset.

1. Kies de gegevensbestanden die u hebt voorbereid.

1. Klik op **importeren** voor het uploaden van de gegevens en beginnen met validatie. Validatie zorgt ervoor dat alle bestanden in de juiste indeling. Validatie kan even duren.

## <a name="create-a-model"></a>Een model maken

 Nadat uw gegevensset is gevalideerd, kunt u als volgt het model trainen.

> [!NOTE]
> Uitspraak gegevens hoeft niet te worden getraind.

1. Kies het type van model maken uit de **aangepaste spraak** menu, klikt u vervolgens op **nieuwe maken.**

1. Kies de landinstelling voor het model.

1. Kies een basismodel voor uw nieuwe model. Uw keuze van basismodel bepaalt de opname-modi waarvoor uw model kan worden gebruikt, evenals fungeren als een opvang voor alle gegevens die niet in uw gegevensset.

1.  Kies de gegevensset waarvan het model wordt gemaakt. Een set gegevens kan worden gebruikt door een willekeurig aantal modellen.

1. Klik op **maken** om te beginnen met het nieuwe model te trainen.

## <a name="test-a-model"></a>Een model te testen

Als onderdeel van het proces voor het model maken, kunt u uw model op basis van een gegevensset akoestisch testen. Het nieuwe model wordt gebruikt voor het herkennen van de spraak in de gegevensset audio-bestanden en de resultaten die de bijbehorende tekst getest. Gebruik een andere akoestisch gegevensset dan de versie die u gebruikt voor het maken van het model voor de beste resultaten.

## <a name="custom-endpoint"></a>Aangepast eindpunt

Nadat u aangepaste akoestisch modellen of modellen taal hebt gemaakt, kunt u ze implementeren naar een aangepaste **spraak naar tekst** eindpunt. Alleen het account dat een eindpunt gemaakt is aanroepen naar deze toegestaan.

Kies voor het maken van een eindpunt **implementaties** van de **aangepaste spraak** menu aan de bovenkant van de pagina. De **implementaties** pagina bevat een tabel met de huidige aangepaste eindpunten, als u een hebt gemaakt. Klik op **nieuw** om een nieuwe eindpunt te maken.

Kies de modellen die u gebruiken wilt de **akoestisch Model** en **taalmodel** bevat. De beschikbare opties bevatten altijd de base Microsoft-modellen. U kunt geen eigen modellen combineren met zoeken en dicteren modellen, dus een akoestisch model kiezen beperkt de modellen beschikbare taal en vice versa. U kunt de dezelfde modellen in een willekeurig aantal eindpunten.

Klik op **maken** na de modellen kiezen. Uw nieuwe eindpunt kan maximaal 30 minuten duren om in te richten.

Wanneer uw eindpunt klaar is, selecteert u deze in de **implementaties** tabel om te zien van de URI en implementatie-ID. U kunt aangepaste eindpunten met de [Rest-API](rest-apis.md#speech-to-text) en de [spraak SDK](speech-sdk.md). De [codevoorbeelden](samples.md) bevatten een voorbeeld van het gebruik van een aangepaste Speech Text-eindpunt.

## <a name="next-steps"></a>Volgende stappen

- [Uw proefabonnement spraak ophalen](https://azure.microsoft.com/try/cognitive-services/)
- [Het herkennen van spraak in C#](quickstart-csharp-windows.md)
