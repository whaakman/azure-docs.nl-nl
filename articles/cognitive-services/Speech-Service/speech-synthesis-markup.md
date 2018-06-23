---
title: Spraak synthese Markup Language | Microsoft Docs
description: Met behulp van de spraak synthese Markup language beheren uitspraak en prosody in spraak.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
manager: noellelacharite
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 04/28/2018
ms.author: v-jerkin
ms.openlocfilehash: d955e7fd7805688ba103897c0d900c44f16514f8
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345433"
---
# <a name="speech-synthesis-markup-language"></a>Spraak synthese Markup Language

Spraak synthese Markup Language (SSML) is een opmaaktaal op basis van een XML-die biedt een manier om te bepalen van de uitspraak en *prosody* van spraak. (Prosody verwijst naar het ritme en presentatie van spraak: de muziek, als u wordt). U kunt woorden Fonetisch opgeven, hints geven bij het interpreteren van getallen, invoegen wordt onderbroken, besturingselement presentatie volume, en snelheid en meer.

Zie voor meer informatie [spraak synthese Markup Language (SSML) versie 1.0](http://www.w3.org/TR/2009/REC-speech-synthesis-20090303/) op het W3C.

De volgende voorbeelden laten zien hoe SSML gebruiken voor algemene spraak synthese behoeften.

## <a name="add-a-break"></a>Een pauze toevoegen
```xml
<speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
<voice  name='Microsoft Server Speech Text to Speech Voice (en-US, Jessa24kRUS)'>
    Welcome to Microsoft Cognitive Services <break time="100ms" /> Text-to-Speech API.
</voice> </speak>
```

## <a name="change-speaking-rate"></a>De snelheid van spreken wijzigen
```xml
<speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
<voice  name='Microsoft Server Speech Text to Speech Voice (en-US, Guy24kRUS)'>
<prosody rate="+30.00%">
    Welcome to Microsoft Cognitive Services Text-to-Speech API.
</prosody></voice> </speak>
```

## <a name="pronunciation"></a>Uitspraak
```xml
<speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
<voice  name='Microsoft Server Speech Text to Speech Voice (en-US, Jessa24kRUS)'>
    <phoneme alphabet="ipa" ph="t&#x259;mei&#x325;&#x27E;ou&#x325;"> tomato </phoneme>
</voice> </speak>
```

## <a name="change-volume"></a>Volume wijzigen
```xml
<speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
<voice  name='Microsoft Server Speech Text to Speech Voice (en-US, JessaRUS)'>
<prosody volume="+20.00%">
    Welcome to Microsoft Cognitive Services Text-to-Speech API.
</prosody></voice> </speak>
```

## <a name="change-pitch"></a>Presentatie wijzigen
```xml
<speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
    <voice  name='Microsoft Server Speech Text to Speech Voice (en-US, Guy24kRUS)'>
    Welcome to <prosody pitch="high">Microsoft Cognitive Services Text-to-Speech API.</prosody>
</voice> </speak>
```

## <a name="change-pitch-contour"></a>Wijziging presentatie werklast
```xml
<speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
<voice  name='Microsoft Server Speech Text to Speech Voice (en-US, JessaRUS)'>
<prosody contour="(80%,+20%) (90%,+30%)" >
    Good morning.
</prosody></voice> </speak>
```

## <a name="next-steps"></a>Volgende stappen

* [Uw proefabonnement spraak ophalen](https://azure.microsoft.com/try/cognitive-services/)
* [Zie het herkennen van spraak in C#](quickstart-csharp-windows.md)
