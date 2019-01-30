---
title: Apps ontwikkelen met de spraak-SDK - spraakservices
titleSuffix: Azure Cognitive Services
description: Informatie over het maken van apps met behulp van de spraak-SDK.
services: cognitive-services
author: wolfma61
manager: cgronlun
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/18/2018
ms.author: wolfma
ms.custom: seodec18
ms.openlocfilehash: b37ba55e0d9e1a93994f90630f92075deb4af7e5
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55206439"
---
# <a name="ship-an-application"></a>Verzenden van een toepassing

Bekijk de [spraak SDK licentie](https://aka.ms/csspeech/license201809), evenals de [software van derden kennisgevingen](https://csspeechstorage.blob.core.windows.net/drop/1.0.0/ThirdPartyNotices.html) wanneer u de Azure Cognitive Services Speech SDK distribueert. Bekijk ook de [privacyverklaring van Microsoft](https://aka.ms/csspeech/privacy).

Afhankelijk van het platform, er verschillende afhankelijkheden bestaan voor het uitvoeren van uw toepassing.

## <a name="windows"></a>Windows

De Cognitive Services Speech SDK is getest op Windows 10 en Windows Server 2016.

De Cognitive Services Speech SDK vereist de [Microsoft Visual C++ Redistributable voor Visual Studio 2017](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads) op het systeem. U kunt downloaden installatieprogramma's voor de nieuwste versie van de `Microsoft Visual C++ Redistributable for Visual Studio 2017` hier:

- [Win32](https://aka.ms/vs/15/release/vc_redist.x86.exe)
- [x64](https://aka.ms/vs/15/release/vc_redist.x64.exe)

Als uw toepassing gebruikmaakt van beheerde code, de `.NET Framework 4.6.1` of hoger is vereist op de doelcomputer.

De Media Foundation-bibliotheken moeten worden geïnstalleerd voor de invoer van de microfoon. Deze bibliotheken maken deel uit van Windows 10 en Windows Server 2016. Het is mogelijk het gebruik van de spraak-SDK zonder deze bibliotheken, zolang een microfoon niet wordt gebruikt als de audio-invoerapparaat.

De vereiste spraak SDK-bestanden kunnen worden geïmplementeerd in dezelfde map als uw toepassing. Op deze manier uw toepassing hebben rechtstreeks toegang tot de bibliotheken. Zorg ervoor dat u selecteert de juiste versie (Win32/x64) die overeenkomt met uw toepassing.

| Name | Function
|:-----|:----|
| `Microsoft.CognitiveServices.Speech.core.dll` | Core-SDK, vereist voor de systeemeigen en beheerde implementatie
| `Microsoft.CognitiveServices.Speech.csharp.bindings.dll` | vereist voor de beheerde implementatie
| `Microsoft.CognitiveServices.Speech.csharp.dll` | vereist voor de beheerde implementatie

## <a name="linux"></a>Linux

Voor een systeemeigen toepassing, moet u voor het verzenden van de spraak-SDK-bibliotheek, `libMicrosoft.CognitiveServices.Speech.core.so`.
Zorg ervoor dat u selecteert u de versie (x86, x64) die overeenkomt met uw toepassing. Afhankelijk van de Linux-versie moet u ook mogelijk om op te nemen van de volgende afhankelijkheden:

* De gedeelde bibliotheken van de GNU C-bibliotheek (met inbegrip van de bibliotheek POSIX Threads Programming `libpthreads`)
* De OpenSSL-bibliotheek (`libssl.so.1.0.0`)
* De bibliotheek cURL (`libcurl.so.4`)
* De gedeelde bibliotheek voor ALSA toepassingen (`libasound.so.2`)

Op Ubuntu 16.04 of 18.04, bijvoorbeeld moeten de GNU C-bibliotheken al worden standaard geïnstalleerd. De laatste drie kan worden geïnstalleerd met behulp van deze opdrachten:

```sh
sudo apt-get update
sudo apt-get install libssl1.0.0 libcurl3 libasound2 wget
```

## <a name="next-steps"></a>Volgende stappen

* [Uw proefabonnement voor Speech ophalen](https://azure.microsoft.com/try/cognitive-services/)
* [Zie voor het herkennen van gesproken tekst in C#](quickstart-csharp-dotnet-windows.md)
