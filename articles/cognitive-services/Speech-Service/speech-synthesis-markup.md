---
title: Spraak synthese Markup Language (SSML) - spraakservices
titleSuffix: Azure Cognitive Services
description: Met behulp van de spraakherkenning synthese Markup Language voor het beheren van uitspraak en prosody in tekst naar spraak.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/15/2019
ms.author: erhopf
ms.custom: seodec18
ms.openlocfilehash: 488cbf1874e023cf3a665bc9f2087900b30b3d3d
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67204913"
---
# <a name="speech-synthesis-markup-language-ssml"></a>Speech Synthesis Markup Language (SSML)

Spraak synthese Markup Language (SSML) is een op XML gebaseerde opmaakcodetaal waarmee ontwikkelaars opgeven hoe invoertekst wordt omgezet in kunstmatige spraak met behulp van de Text to Speech-service. Vergeleken met de tekst zonder opmaak, kan SSML ontwikkelaars voor het afstemmen van de presentatie, uitspraak, spreken snelheid, volume en meer van de Text to Speech-uitvoer. Normale interpunctie, zoals na een periode onderbreken of met behulp van de juiste intonation wanneer een zin met een vraagteken eindigt worden automatisch verwerkt.

De implementatie van de spraakservices van SSML is gebaseerd op World Wide Web-Consortium [spraak synthese Markup Language versie 1.0](https://www.w3.org/TR/speech-synthesis).

> [!IMPORTANT]
> Chinese, Japans en Koreaanse tekens tellen als twee tekens voor facturering. Zie voor meer informatie, [prijzen](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/).

## <a name="standard-neural-and-custom-voices"></a>Standard, neural en aangepaste stemmen

Kiezen uit de standard- en neurale stemmen of maak uw eigen aangepaste gesproken uniek is voor uw product of een merk. 75 + standard stemmen zijn beschikbaar in meer dan 45 talen en landinstellingen en 5 neurale stemmen zijn beschikbaar in 4 talen of landinstellingen. Zie voor een volledige lijst van ondersteunde talen, landinstellingen en stemmen (neurale en standaard), [taalondersteuning](language-support.md).

Zie voor meer informatie over neurale, standaard en aangepaste stemmen, [tekst naar spraak overzicht](text-to-speech.md).

## <a name="supported-ssml-elements"></a>Ondersteunde SSML elementen

Elk document SSML wordt gemaakt met SSML elementen (of tags). Deze elementen worden gebruikt om aan te passen verkopen, prosody en volume. De volgende secties bevatten informatie over hoe elk element worden gebruikt en wanneer een element is vereist of optioneel zijn.  

> [!IMPORTANT]
> Vergeet niet te Gebruik dubbele aanhalingstekens rond de kenmerkwaarden. Standaarden voor opgemaakte, geldige XML vereist kenmerkwaarden moet tussen dubbele aanhalingstekens. Bijvoorbeeld, `<prosody volume="90">` is een goed ingedeelde, geldige-element, maar `<prosody volume=90>` niet. SSML mogelijk kenmerkwaarden die zich niet in de aanhalingstekens niet herkend.

## <a name="create-an-ssml-document"></a>Een document SSML maken

`speak` het hoofdelement is en is **vereist** voor alle SSML documenten. De `speak` element bevat belangrijke informatie, zoals versie, taal en de definitie van de markup-woordenlijst.

**Syntaxis**

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="string"></speak>
```

**Kenmerken**

| Kenmerk | Description | Vereiste / optioneel |
|-----------|-------------|---------------------|
| version | Geeft de versie van de SSML-specificatie gebruikt voor het interpreteren van de document-opmaak. De huidige versie is 1.0. | Vereist |
| xml:lang | Hiermee geeft u de taal van het root-document. De waarde kan een taalcode in kleine letters en twee letters bevatten (bijvoorbeeld **en**), of de taal en hoofdletters land/regio (bijvoorbeeld **en-US**). | Vereist |
| xmlns | Hiermee geeft u de URI moet het document dat het vocabulaire markup (elementtypen en kenmerknamen) van het document SSML definieert. De URI van de huidige https://www.w3.org/2001/10/synthesis. | Vereist |

## <a name="choose-a-voice-for-text-to-speech"></a>Kies een stem voor tekst naar spraak

De `voice` element is vereist. Het wordt gebruikt om op te geven van de stem die wordt gebruikt voor tekst naar spraak.

**Syntaxis**

```xml
<voice name="string">
    This text will get converted into synthesized speech.
</voice>
```

**Kenmerken**

| Kenmerk | Description | Vereiste / optioneel |
|-----------|-------------|---------------------|
| name | Hiermee geeft u de stem voor Text to Speech uitvoer gebruikt. Zie voor een volledige lijst van ondersteunde stemmen, [taalondersteuning](language-support.md#text-to-speech). | Vereist |

**Voorbeeld**

> [!NOTE]
> In dit voorbeeld wordt de `en-US-Jessa24kRUS` stem. Zie voor een volledige lijst van ondersteunde stemmen, [taalondersteuning](language-support.md#text-to-speech).

```XML
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice  name="en-US-Jessa24kRUS">
        This is the text that is spoken.
    </voice>
</speak>
```

## <a name="use-multiple-voices"></a>Meerdere stemmen

Binnen de `speak` -element, kunt u meerdere stemmen voor Text to Speech-uitvoer. Deze stemmen kunnen zich in verschillende talen. Voor elke stem de tekst die moet worden verpakt een `voice` element.

**Kenmerken**

| Kenmerk | Description | Vereiste / optioneel |
|-----------|-------------|---------------------|
| name | Hiermee geeft u de stem voor Text to Speech uitvoer gebruikt. Zie voor een volledige lijst van ondersteunde stemmen, [taalondersteuning](language-support.md#text-to-speech). | Vereist |

**Voorbeeld**

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice  name="en-US-Jessa24kRUS">
        Good morning!
    </voice>
    <voice  name="en-US-Guy24kRUS">
        Good morning to you too Jessa!
    </voice>
</speak>
```

## <a name="adjust-speaking-styles"></a>Spreekstijl aanpassen

> [!IMPORTANT]
> Deze functie werkt alleen met neurale stemmen.

De Text to Speech-service synthetiseert standaard, tekst met behulp van een neutrale spreekstijl voor zowel standaard als neurale stemmen. Met neurale stemmen, kunt u de spreekstijl snelle cheerfulness, empathie of sentiment met de `<mstts:express-as>` element. Dit is een optioneel element uniek is voor Azure Speech Services.

Op dit moment worden spreken stijl aanpassingen ondersteund voor deze neurale stemmen:
* `en-US-JessaNeural`
* `zh-CN-XiaoxiaoNeural`

Wijzigingen worden toegepast op het niveau van de zin en stijl zijn afhankelijk van spraak. Als een stijl wordt niet ondersteund, wordt de service geretourneerd spraak in de standaard neutrale spraakstijl.

**Syntaxis**

```xml
<mstts:express-as type="string"></mstts:express-as>
```

**Kenmerken**

| Kenmerk | Description | Vereiste / optioneel |
|-----------|-------------|---------------------|
| type | Hiermee geeft u de spreekstijl. Op dit moment zijn spreekstijl specifieke stem. | Vereist als het aanpassen van de spreekstijl van een neural stem. Als u `mstts:express-as`, en vervolgens het type moet worden opgegeven. Als een ongeldige waarde is opgegeven, is dit element wordt genegeerd. |

Gebruik deze tabel om te bepalen welke spreekstijl worden ondersteund voor elke neurale spraak.

| Spraak | Type | Description |
|-------|------|-------------|
| `en-US-JessaNeural` | type=`cheerful` | Geeft aan een emoties positief en tevreden |
| | type=`empathy` | Geeft aan een idee van caring en begrijpen |
| `zh-CN-XiaoxiaoNeural` | type=`newscast` | Geeft aan een formele toon, vergelijkbaar met de nieuws-broadcasts |
| | type=`sentiment` | Geeft een raakt bericht of een verhaal |

**Voorbeeld**

Dit fragment SSML ziet u hoe de `<mstts:express-as>` element wordt gebruikt voor het wijzigen van de spreekstijl naar `cheerful`.

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xmlns:mstts="https://www.w3.org/2001/mstts" xml:lang="en-US">
    <voice name="en-US-JessaNeural">
        <mstts:express-as type="cheerful">
            That'd be just amazing!
        </mstts:express-as>
    </voice>
</speak>
```

## <a name="add-or-remove-a-breakpause"></a>Toevoegen of verwijderen van een einde/onderbroken

Gebruik de `break` element voor het invoegen van pauzes (of onderbrekingen) tussen woorden of te voorkomen dat automatisch is toegevoegd door de Text to Speech-service wordt onderbroken.

> [!NOTE]
> Gebruik dit element op het standaardgedrag van spraak voor een woord of woordgroep negeren als de kunstmatige spraak voor dat woord of woordgroep onnatuurlijke geluiden. Stel `strength` naar `none` om te voorkomen dat een prosodic einde, die automatisch door de tex-naar-spraak-service wordt ingevoegd.

**Syntaxis**

```xml
<break strength="string" />
<break time="string" />
```

**Kenmerken**

| Kenmerk | Description | Vereiste / optioneel |
|-----------|-------------|---------------------|
| sterkte | Hiermee geeft u de relatieve duur van een pauze van met een van de volgende waarden:<ul><li>Geen</li><li>x-weak</li><li>zwakke</li><li>Normaal (standaard)</li><li>Sterke</li><li>x-strong</li></ul> | Optioneel |
| time | Hiermee geeft u de absolute duur van een onderbreking in enkele seconden of milliseconden. Voorbeelden van geldige waarden zijn 2s en 500 | Optioneel |

| Sterkte | Description |
|----------|-------------|
| Geen, of als er geen waarde opgegeven | 0 ms |
| x-weak | 250 ms |
| zwakke | 500 ms |
| Gemiddeld | 750 ms |
| Sterke | 1000 ms |
| x-strong | 1250 ms |


**Voorbeeld**

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice  name="en-US-Jessa24kRUS">
        Welcome to Microsoft Cognitive Services <break time="100ms" /> Text-to-Speech API.
    </voice>
</speak>
```

## <a name="specify-paragraphs-and-sentences"></a>Geef de alinea's en zinnen

`p` en `s` -elementen worden gebruikt om aan te duiden alinea's en zinnen, respectievelijk. In de afwezigheid van deze elementen bepaalt de Text to Speech-service automatisch de structuur van het document SSML.

De `p` element bevat mogelijk tekst en de volgende elementen: `audio`, `break`, `phoneme`, `prosody`, `say-as`, `sub`, `mstts:express-as`, en `s`.

De `s` element bevat mogelijk tekst en de volgende elementen: `audio`, `break`, `phoneme`, `prosody`, `say-as`, `mstts:express-as`, en `sub`.

**Syntaxis**

```XML
<p></p>
<s></s>
```

**Voorbeeld**

```XML
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice  name="en-US-Jessa24kRUS">
        <p>
            <s>Introducing the sentence element.</s>
            <s>Used to mark individual sentences.</s>
        </p>
        <p>
            Another simple paragraph.
            Sentence structure in this paragraph is not explicitly marked.
        </p>
    </voice>
</speak>
```

## <a name="use-phonemes-to-improve-pronunciation"></a>Fonemen gebruiken voor het verbeteren van de uitspraak

De `ph` element wordt gebruikt voor voor de fonetische uitspraak in SSML documenten. De `ph` element mag alleen tekst, geen andere elementen. Altijd opgeven voor mensen leesbare spraak als terugval.

Phonetic letters bestaan uit telefoons, die bestaan uit letters, cijfers of tekens, soms in combinatie. Elke telefoon beschrijft een unieke geluid van spraak. Dit is in tegenstelling tot het Latijnse alfabet, waarbij elke letter meerdere gesproken geluiden kan staan. Houd rekening met de verschillende uitspraak van de letter "c" in de woorden 'snoep"en 'niet langer' of de verschillende uitspraak van de combinatie van de letter 'e' in de woorden 'ding' en 'die'.

**Syntaxis**

```XML
<phoneme alphabet="string" ph="string"></phoneme>
```

**Kenmerken**

| Kenmerk | Description | Vereiste / optioneel |
|-----------|-------------|---------------------|
| alfabet | Hiermee geeft u het alfabet phonetic moet worden gebruikt wanneer de stof voor de uitspraak van de tekenreeks in de synthese de `ph` kenmerk. De tekenreeks opgeven van het alfabet moet worden opgegeven in kleine letters. Hier volgen de mogelijke letters die u kunt opgeven.<ul><li>ipa &ndash; International Phonetic Alphabet</li><li>SAPI &ndash; Speech API telefoon instellen</li><li>UPS &ndash; Universal telefoon instellen</li></ul>Het alfabet geldt alleen voor de foneem in het element. Zie voor meer informatie, [Phonetic alfabet verwijzing](https://msdn.microsoft.com/library/hh362879(v=office.14).aspx). | Optioneel |
| pH | Een tekenreeks met telefoons dat opgeeft de uitspraak van het woord in de `phoneme` element. Als de opgegeven tekenreeks niet-herkende telefoons bevat, wordt de Text to Speech (Text-to-Speech)-service het hele SSML document geweigerd en produceert geen van de spraakherkenning uitvoerwaarde zijn opgegeven in het document. | Vereist als Fonemen. |

**Voorbeelden**

```XML
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice  name="en-US-Jessa24kRUS">
        <s>His name is Mike <phoneme alphabet="ups" ph="JH AU"> Zhou </phoneme></s>
    </voice>
</speak>
```

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice  name="en-US-Jessa24kRUS">
        <phoneme alphabet="ipa" ph="t&#x259;mei&#x325;&#x27E;ou&#x325;"> tomato </phoneme>
    </voice>
</speak>
```

## <a name="adjust-prosody"></a>Prosody aanpassen

De `prosody` element wordt gebruikt om wijzigingen te verkopen, countour, bereik, frequentie, duur en volume voor de Text to Speech uitvoer geven. De `prosody` element bevat mogelijk tekst en de volgende elementen: `audio`, `break`, `p`, `phoneme`, `prosody`, `say-as`, `sub`, en `s`.

Omdat prosodic kenmerkwaarden weer over een breed bereik variëren kunnen, wordt de toegewezen waarden in de spraakherkenning geïnterpreteerd als een suggestie van wat de werkelijke prosodic waarden van de geselecteerde stem moeten zijn. De Text to Speech-service beperkt of vervangen door waarden die niet worden ondersteund. Voorbeelden van niet-ondersteunde waarden zijn een presentatie van 1 MHz of een volume van 120.

**Syntaxis**

```XML
<prosody pitch="value" contour="value" range="value" rate="value" duration="value" volume="value"></prosody>
```

**Kenmerken**

| Kenmerk | Description | Vereiste / optioneel |
|-----------|-------------|---------------------|
| tekenbreedte | Geeft de breedte van de basislijn voor de tekst. U kunt de presentatie als express:<ul><li>Een absolute waarde, uitgedrukt als een getal gevolgd door 'Hz' (Hz). Bijvoorbeeld: 600Hz.</li><li>Een relatieve waarde, uitgedrukt als een getal voorafgegaan door '+' of '-' en gevolgd door 'Hz' of 'e', waarmee een bedrag voor het wijzigen van de presentatie. Bijvoorbeeld: + 80 Hz of -2st. De 'e' geeft aan de eenheid wijzigen halve, dit de helft van een toon (een halve stap) op de standaard diatonic-schaal is.</li><li>Een constante waarde:<ul><li>x-laag</li><li>Laag</li><li>Gemiddeld</li><li>Hoog</li><li>x-high</li><li>standaardinstelling</li></ul></li></ul>. | Optioneel |
| contour | Contour wordt niet ondersteund voor neurale stemmen. Wijzigingen in de presentatie van de gesproken inhoud vertegenwoordigt contour als een matrix met doelen op een opgegeven periode plaats in de spraakuitvoer van. Elk doel wordt gedefinieerd door de sets met de parameter-paren. Bijvoorbeeld: <br/><br/>`<prosody contour="(0%,+20Hz) (10%,-2st) (40%,+10Hz)">`<br/><br/>De eerste waarde in elke set parameters geeft de locatie van de wijziging verkopen als een percentage van de duur van de tekst. De tweede waarde geeft het bedrag om te verhogen of verlagen van de presentatie, met behulp van een relatieve waarde of een opsommingswaarde voor presentatie (Zie `pitch`). | Optioneel |
| Bereik  | Een waarde die aangeeft van het bereik van de breedte van de tekst. U kunt snelle `range` met behulp van de dezelfde absolute waarden, de relatieve waarden of de opsommingswaarden gebruikt om te beschrijven `pitch`. | Optioneel |
| Snelheid  | Geeft de spreken snelheid van de tekst. U kunt snelle `rate` als:<ul><li>Een relatieve waarde, uitgedrukt als een getal dat als een vermenigvuldiger van de standaard fungeert. Bijvoorbeeld: een waarde van *1* resulteert in geen wijziging in het tarief. Een waarde van *.5* resulteert in een halverende van het tarief. Een waarde van *3* resulteert in een tripling van het tarief.</li><li>Een constante waarde:<ul><li>x-slow</li><li>browserpagina's met trage</li><li>Gemiddeld</li><li>snelle</li><li>x-fast</li><li>standaardinstelling</li></ul></li></ul> | Optioneel |
| Duur  | De periode die tijdens de gesproken tekst verstrijken moet synthese (Text-to-Speech) service leest de tekst, in seconden of milliseconden. Bijvoorbeeld, *2s* of *1800ms*. | Optioneel |
| volume  | Geeft het volumeniveau van de spreken stem. U kunt het volume indien mogelijk express:<ul><li>Een absolute waarde, uitgedrukt als een getal in het bereik van 0,0 aan 100,0 van *quietest* naar *hardste*. Bijvoorbeeld: 75. De standaardwaarde is 100,0 zijn.</li><li>Een relatieve waarde, uitgedrukt als een getal voorafgegaan door '+' of '-' die aangeeft dat een bedrag voor het wijzigen van het volume. Bijvoorbeeld + 10 of-5.5.</li><li>Een constante waarde:<ul><li>silent</li><li>x-soft</li><li>voorlopig</li><li>Gemiddeld</li><li>loud</li><li>x-loud</li><li>standaardinstelling</li></ul></li></ul> | Optioneel |

### <a name="change-speaking-rate"></a>Snelheid van spreken wijzigen

Tarief spreken kan worden toegepast op standard stemmen in het woord of de zin op serverniveau. Terwijl u spreekt tarief kan alleen worden toegepast op neurale stemmen op het niveau van de zin.

**Voorbeeld**

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice  name="en-US-Guy24kRUS">
        <prosody rate="+30.00%">
            Welcome to Microsoft Cognitive Services Text-to-Speech API.
        </prosody>
    </voice>
</speak>
```

### <a name="change-volume"></a>Volume wijzigen

Volumewijzigingen kunnen worden toegepast op standard stemmen in het woord of de zin op serverniveau. Terwijl het volume worden gewijzigd, kunnen alleen worden toegepast op neurale stemmen op het niveau van de zin.

**Voorbeeld**

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice  name="en-US-Jessa24kRUS">
        <prosody volume="+20.00%">
            Welcome to Microsoft Cognitive Services Text-to-Speech API.
        </prosody>
    </voice>
</speak>
```

### <a name="change-pitch"></a>Breedte wijzigen

Presentatie wijzigingen kunnen worden toegepast op standard stemmen in het woord of de zin op serverniveau. Terwijl de tekenbreedte wijzigingen kunnen alleen worden toegepast op neurale stemmen op het niveau van de zin.

**Voorbeeld**

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice  name="en-US-Guy24kRUS">
        Welcome to <prosody pitch="high">Microsoft Cognitive Services Text-to-Speech API.</prosody>
    </voice>
</speak>
```

### <a name="change-pitch-contour"></a>Wijziging inspiratie contour

> [!IMPORTANT]
> Tekenbreedte contour wijzigingen worden niet ondersteund met neurale stemmen.

**Voorbeeld**

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice  name="en-US-Jessa24kRUS">
        <prosody contour="(80%,+20%) (90%,+30%)" >
            Good morning.
        </prosody>
    </voice>
</speak>
```

## <a name="next-steps"></a>Volgende stappen

* [Taalondersteuning: stemmen, landinstellingen, talen](language-support.md)
