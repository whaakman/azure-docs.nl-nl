---
title: Spraak synthese Markup Language - spraakservices
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
ms.openlocfilehash: 77e94fd9fc08ad9f7b14e5cf226a6e616b69a37e
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57533388"
---
# <a name="speech-synthesis-markup-language"></a>Spraak synthese Markup Language

Spraak synthese Markup Language (SSML) is een op XML gebaseerde opmaaktaal die een manier voor het beheren van de uitspraak biedt en *prosody* van tekst naar spraak. Prosody verwijst naar het ritme en verkopen van spraak: de muziek, als u wordt. U kunt woorden Fonetisch opgeven, hints geven voor de interpretatie van getallen, invoegen wordt onderbroken, inspiratie besturingselement, volume, en snelheid en meer. Zie voor meer informatie, [spraak synthese Markup Language (SSML) versie 1.0](https://www.w3.org/TR/2009/REC-speech-synthesis-20090303/).

Zie voor een volledige lijst van ondersteunde talen, landinstellingen en stemmen (neurale en standaard), [taalondersteuning](language-support.md#text-to-speech).

De volgende secties bevatten voorbeelden voor algemene spraak synthese taken.

>[!IMPORTANT]
> Prosody tagging is momenteel alleen beschikbaar voor standard stemmen.

## <a name="add-a-break"></a>Einde toevoegen
```xml
<speak version='1.0' xmlns="https://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
<voice  name='Microsoft Server Speech Text to Speech Voice (en-US, Jessa24kRUS)'>
    Welcome to Microsoft Cognitive Services <break time="100ms" /> Text-to-Speech API.
</voice> </speak>
```

## <a name="change-speaking-rate"></a>Snelheid van spreken wijzigen
```xml
<speak version='1.0' xmlns="https://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
<voice  name='Microsoft Server Speech Text to Speech Voice (en-US, Guy24kRUS)'>
<prosody rate="+30.00%">
    Welcome to Microsoft Cognitive Services Text-to-Speech API.
</prosody></voice> </speak>
```

## <a name="pronunciation"></a>Uitspraak van
```xml
<speak version='1.0' xmlns="https://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
<voice  name='Microsoft Server Speech Text to Speech Voice (en-US, Jessa24kRUS)'>
    <phoneme alphabet="ipa" ph="t&#x259;mei&#x325;&#x27E;ou&#x325;"> tomato </phoneme>
</voice> </speak>
```

## <a name="change-volume"></a>Volume wijzigen
```xml
<speak version='1.0' xmlns="https://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
<voice  name='Microsoft Server Speech Text to Speech Voice (en-US, JessaRUS)'>
<prosody volume="+20.00%">
    Welcome to Microsoft Cognitive Services Text-to-Speech API.
</prosody></voice> </speak>
```

## <a name="change-pitch"></a>Breedte wijzigen
```xml
<speak version='1.0' xmlns="https://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
    <voice  name='Microsoft Server Speech Text to Speech Voice (en-US, Guy24kRUS)'>
    Welcome to <prosody pitch="high">Microsoft Cognitive Services Text-to-Speech API.</prosody>
</voice> </speak>
```

## <a name="change-pitch-contour"></a>Wijziging inspiratie contour
```xml
<speak version='1.0' xmlns="https://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
<voice  name='Microsoft Server Speech Text to Speech Voice (en-US, JessaRUS)'>
<prosody contour="(80%,+20%) (90%,+30%)" >
    Good morning.
</prosody></voice> </speak>
```

## <a name="use-multiple-voices"></a>Meerdere stemmen
```xml
<speak version='1.0' xmlns="https://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
<voice  name='Microsoft Server Speech Text to Speech Voice (en-US, JessaRUS)'>
    Good morning!
</voice>
<voice  name='Microsoft Server Speech Text to Speech Voice (en-US, Guy24kRUS)'>
    Good morning to you too Jessa!
</voice> </speak>
```

## <a name="next-steps"></a>Volgende stappen

* [Taalondersteuning: stemmen, landinstellingen, talen](language-support.md)
