---
title: Azure Cognitive Services, Cognitive Services Speech SDK-API-documentatie - zelfstudies, API-verwijzing
description: Meer informatie over het maken en ontwikkelen van apps met de Cognitive Services spraak-SDK
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: wolfma61
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 06/07/2018
ms.author: wolfma
ms.openlocfilehash: b920b7bcb86d94b2948ed0a9bf783cee1282b33f
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47041072"
---
# <a name="ship-an-application"></a>Verzenden van een toepassing

Bekijk de [spraak SDK licentie](license.md), evenals de [software van derden kennisgevingen](third-party-notices.md) wanneer u de Azure Cognitive Services Speech SDK distribueert. Bekijk ook de [privacyverklaring van Microsoft](https://aka.ms/csspeech/privacy).

Afhankelijk van het platform, er verschillende afhankelijkheden bestaan voor het uitvoeren van uw toepassing.

## <a name="windows"></a>Windows

De Cognitive Services Speech SDK is getest op Windows 10 en Windows Server 2016.

De Cognitive Services Speech SDK vereist de [Microsoft Visual C++ Redistributable voor Visual Studio 2017](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads) op het systeem. U kunt downloaden installatieprogramma's voor de nieuwste versie van de `Microsoft Visual C++ Redistributable for Visual Studio 2017` hier:

- [Win32](https://aka.ms/vs/15/release/vc_redist.x86.exe)
- [x64](https://aka.ms/vs/15/release/vc_redist.x64.exe)

Als uw toepassing gebruikmaakt van beheerde code, de `.NET Framework 4.6.1` of hoger is vereist op de doelcomputer.

De Media Foundation-bibliotheken moeten worden geïnstalleerd voor de invoer van de microfoon. Deze bibliotheken maken deel uit van Windows 10 en Windows Server 2016. Het is mogelijk het gebruik van de spraak-SDK zonder deze bibliotheken, zolang een microfoon niet wordt gebruikt als de audio-invoerapparaat.

De vereiste spraak SDK-bestanden kunnen worden geïmplementeerd in dezelfde map als uw toepassing. Op deze manier uw toepassing hebben rechtstreeks toegang tot de bibliotheken. Zorg ervoor dat u selecteert de juiste versie (Win32/x64) die overeenkomt met uw toepassing.

| Naam | Functie
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

Op Ubuntu 16.04, bijvoorbeeld moeten de GNU C-bibliotheken al worden standaard geïnstalleerd. De laatste drie kan worden geïnstalleerd met behulp van deze opdrachten:

```sh
sudo apt-get update
sudo apt-get install libssl1.0.0 libcurl3 libasound2 wget
```

## <a name="next-steps"></a>Volgende stappen

* [Uw proefabonnement voor Speech ophalen](https://azure.microsoft.com/try/cognitive-services/)
* [Zie voor het herkennen van gesproken tekst in C#](quickstart-csharp-dotnet-windows.md)
