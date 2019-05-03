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
ms.date: 12/13/2018
ms.author: erhopf
ms.custom: seodec18
ms.openlocfilehash: 9871e0106ee6caf11c5a1e24459fbd2044f5f3d7
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/02/2019
ms.locfileid: "65021439"
---
# <a name="speech-synthesis-markup-language-ssml"></a>Speech Synthesis Markup Language (SSML)

Spraak synthese Markup Language (SSML) is een op XML gebaseerde opmaaktaal die een manier voor het beheren van de uitspraak biedt en *prosody* van tekst naar spraak. Prosody verwijst naar het ritme en verkopen van spraak: de muziek, als u wordt. U kunt woorden Fonetisch opgeven, hints geven voor de interpretatie van getallen, invoegen wordt onderbroken, inspiratie besturingselement, volume, en snelheid en meer. Zie voor meer informatie, [spraak synthese Markup Language (SSML) versie 1.0](https://www.w3.org/TR/2009/REC-speech-synthesis-20090303/). 

Zie voor een volledige lijst van ondersteunde talen, landinstellingen en stemmen (neurale en standaard), [taalondersteuning](language-support.md#text-to-speech).

De volgende secties bevatten voorbeelden voor algemene spraak synthese taken.

## <a name="adjust-speaking-style-for-neural-voices"></a>Spreekstijl voor neurale stemmen aanpassen

U kunt SSML gebruiken om aan te passen de spreekstijl bij het gebruik van een van de neurale stemmen.

De Text to Speech-service synthetiseert standaard, tekst in een neutrale stijl. De neurale stemmen uitbreiden SSML met een `<mstts:express-as>` element dat tekst wordt geconverteerd naar kunstmatige spraak in verschillende spreken stijlen. De stijl van labels worden op dit moment alleen ondersteund met deze stemmen:

* `en-US-JessaNeural` 
* `zh-CN-XiaoxiaoNeural`.

Wijzigingen in stijl spreken kan worden toegepast op het niveau van de zin. De stijlen zijn afhankelijk van spraak. Als een stijl-type wordt niet ondersteund, wordt de service de kunstmatige spraak geretourneerd als de standaard neutrale.

| Spraak | Style | Description | 
|-----------|-----------------|----------|
| `en-US-JessaNeural` | type=`cheerful` | Geeft aan een emoties positief en tevreden |
| | type=`empathy` | Geeft aan een idee van caring en begrijpen |
| `zh-CN-XiaoxiaoNeural` | type=`newscast` | Geeft aan een formele toon, vergelijkbaar met de nieuws-broadcasts |
| | type=`sentiment ` | Geeft een raakt bericht of een verhaal |

```xml
<speak version='1.0' xmlns="https://www.w3.org/2001/10/synthesis" xmlns:mstts="https://www.w3.org/2001/mstts" xml:lang="en-US">
<voice name='en-US-JessaNeural'>
<mstts:express-as type="cheerful"> 
    That'd be just amazing! 
</mstts:express-as></voice></speak>
```

## <a name="add-a-break"></a>Einde toevoegen
```xml
<speak version='1.0' xmlns="https://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
<voice  name='en-US-Jessa24kRUS'>
    Welcome to Microsoft Cognitive Services <break time="100ms" /> Text-to-Speech API.
</voice> </speak>
```

## <a name="change-speaking-rate"></a>Snelheid van spreken wijzigen

Tarief spreken kan worden toegepast op standard stemmen in het woord of de zin op serverniveau. Terwijl u spreekt tarief kan alleen worden toegepast op neurale stemmen op het niveau van de zin.

```xml
<speak version='1.0' xmlns="https://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
<voice  name='en-US-Guy24kRUS'>
<prosody rate="+30.00%">
    Welcome to Microsoft Cognitive Services Text-to-Speech API.
</prosody></voice> </speak>
```

## <a name="pronunciation"></a>Uitspraak van
```xml
<speak version='1.0' xmlns="https://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
<voice  name='en-US-Jessa24kRUS'>
    <phoneme alphabet="ipa" ph="t&#x259;mei&#x325;&#x27E;ou&#x325;"> tomato </phoneme>
</voice> </speak>
```

## <a name="change-volume"></a>Volume wijzigen

Volumewijzigingen kunnen worden toegepast op standard stemmen in het woord of de zin op serverniveau. Terwijl het volume worden gewijzigd, kunnen alleen worden toegepast op neurale stemmen op het niveau van de zin.

```xml
<speak version='1.0' xmlns="https://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
<voice  name='en-US-Jessa24kRUS'>
<prosody volume="+20.00%">
    Welcome to Microsoft Cognitive Services Text-to-Speech API.
</prosody></voice> </speak>
```

## <a name="change-pitch"></a>Breedte wijzigen

Presentatie wijzigingen kunnen worden toegepast op standard stemmen in het woord of de zin op serverniveau. Terwijl de tekenbreedte wijzigingen kunnen alleen worden toegepast op neurale stemmen op het niveau van de zin.

```xml
<speak version='1.0' xmlns="https://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
    <voice  name='en-US-Guy24kRUS'>
    Welcome to <prosody pitch="high">Microsoft Cognitive Services Text-to-Speech API.</prosody>
</voice> </speak>
```

## <a name="change-pitch-contour"></a>Wijziging inspiratie contour

> [!IMPORTANT]
> Tekenbreedte contour wijzigingen worden niet ondersteund met neurale stemmen.

```xml
<speak version='1.0' xmlns="https://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
<voice  name='en-US-Jessa24kRUS'>
<prosody contour="(80%,+20%) (90%,+30%)" >
    Good morning.
</prosody></voice> </speak>
```

## <a name="use-multiple-voices"></a>Meerdere stemmen
```xml
<speak version='1.0' xmlns="https://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
<voice  name='en-US-Jessa24kRUS'>
    Good morning!
</voice>
<voice  name='en-US-Guy24kRUS'>
    Good morning to you too Jessa!
</voice> </speak>
```

## <a name="next-steps"></a>Volgende stappen

* [Taalondersteuning: stemmen, landinstellingen, talen](language-support.md)
