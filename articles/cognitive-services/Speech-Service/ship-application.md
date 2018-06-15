---
title: Azure cognitieve Services cognitieve Services spraak SDK-API-documentatie - zelfstudies, API-referentiemateriaal | Microsoft Docs
description: Meer informatie over het maken en ontwikkelen van apps met de cognitieve Services Speech SDK
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: wolfma61
manager: onano
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 06/07/2018
ms.author: wolfma
ms.openlocfilehash: d410dda09fdd30181b633c454b1d44610b10c472
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/07/2018
ms.locfileid: "35349985"
---
# <a name="shipping-an-application"></a>Verzending van een toepassing

Zien de [spraak SDK licentie](license.md), evenals de [mededelingen van de software van derden](third-party-notices.md) tijdens het distribueren van de SDK cognitieve Services spraak. Bekijk ook de [privacyverklaring van Microsoft](https://aka.ms/csspeech/privacy).

Afhankelijk van het platform bestaan verschillende afhankelijkheden voor het uitvoeren van uw toepassing.

## <a name="windows"></a>Windows

De cognitieve Services spraak SDK is getest op Windows 10 en Windows Server 2016.

De cognitieve Services spraak SDK vereist de [Microsoft Visual C++ Redistributable voor Visual Studio 2017](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads) op het systeem. U kunt downloaden installatieprogramma's voor de meest recente versie van de `Microsoft Visual C++ Redistributable for Visual Studio 2017` hier:

- [Win32](https://aka.ms/vs/15/release/vc_redist.x86.exe)
- [x64](https://aka.ms/vs/15/release/vc_redist.x64.exe)

Als uw toepassing van beheerde code gebruikmaakt, de `.Net Framework 4.6.1` of hoger is vereist op de doelcomputer.

Voor invoer van de microfoon moeten de Media Foundation-bibliotheken worden geïnstalleerd. Deze bibliotheken maken deel uit van Windows 10 en Windows Server 2016. Het kan de SDK spraak gebruiken zonder deze bibliotheken, zolang microfoon niet wordt gebruikt als de audio-invoerapparaat.

De vereiste bestanden voor spraak-SDK kunnen worden geïmplementeerd in dezelfde map als uw toepassing. Op deze manier die uw toepassing rechtstreeks toegang de tapewisselaars tot. Zorg ervoor dat u de juiste versie (Win32/x64) die overeenkomt met uw toepassing selecteren.

| Naam | Functie
|:-----|:----|
| `Microsoft.CognitiveServices.Speech.core.dll` | Core-SDK vereist voor systeemeigen en beheerde implementatie
| `Microsoft.CognitiveServices.Speech.csharp.bindings.dll` | vereist is voor beheerde implementatie
| `Microsoft.CognitiveServices.Speech.csharp.dll` | vereist is voor beheerde implementatie

## <a name="linux"></a>Linux

Voor een native toepassing die u moet de spraak SDK-bibliotheek verzenden `libMicrosoft.CognitiveServices.Speech.core.so`.
Zorg ervoor dat u de versie (x86, x64) selecteert die overeenkomt met uw toepassing. Mogelijk moet u ook het opnemen van de volgende afhankelijkheden, afhankelijk van de Linux-versie:

* De gedeelde bibliotheken van de GNU C-bibliotheek (met inbegrip van de bibliotheek POSIX Threads Programming `libpthreads`)
* De OpenSSL-bibliotheek (`libssl.so.1.0.0`)
* De bibliotheek cURL (`libcurl.so.4`)
* De gedeelde bibliotheek voor ALSA toepassingen (`libasound.so.2`)

Op Ubuntu 16.04, bijvoorbeeld moeten de bibliotheken GNU C al worden standaard geïnstalleerd. De laatste drie kan worden geïnstalleerd met de volgende opdrachten:

```sh
sudo apt-get update
sudo apt-get install libssl1.0.0 libcurl3 libasound2 wget
```

## <a name="next-steps"></a>Volgende stappen

* [Uw proefabonnement spraak ophalen](https://azure.microsoft.com/try/cognitive-services/)
* [Zie het herkennen van spraak in C#](quickstart-csharp-windows.md)
