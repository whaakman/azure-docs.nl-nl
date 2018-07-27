---
title: Informatie over de Cognitive Services Speech-SDK
description: Een overzicht van de SDK's beschikbaar voor de Speech-service.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 07/17/2018
ms.author: v-jerkin
ms.openlocfilehash: 20fe0c3501e562584cd7762555479457a34a9297
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/27/2018
ms.locfileid: "39281320"
---
# <a name="about-the-cognitive-services-speech-sdk"></a>Informatie over de Cognitive Services Speech-SDK

De Cognitive Services spraak Software Development Kit (SDK) biedt uw toepassingen systeemeigen toegang tot de functies van de spraak-service, waardoor het gemakkelijker wordt om software te ontwikkelen. De SDK biedt op dit moment toegang tot **spraak naar tekst**, **Spraakomzetting**, en **bedoeling erkenning**.

[!include[Speech SDK Platforms](../../../includes/cognitive-services-speech-service-speech-sdk-platforms.md)]

[!include[License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

## <a name="get-the-windows-sdk"></a>Ophalen van de Windows SDK

De Windows-versie van de spraak-SDK bevat clientbibliotheken voor 32-bits en 64-bits-C/C++, evenals beheerde (.NET)-bibliotheken voor gebruik met C#. De SDK kan worden geïnstalleerd in Visual Studio met behulp van NuGet; Zoek `Microsoft.CognitiveServices.Speech`.

## <a name="get-the-linux-sdk"></a>De Linux-SDK ophalen

Zorg ervoor dat u de vereiste compiler en bibliotheken hebt door het uitvoeren van de volgende shellopdrachten:

```sh
sudo apt-get update
sudo apt-get install build-essential libssl1.0.0 libcurl3 libasound2
```

> [!NOTE]
> Deze instructies wordt ervan uitgegaan dat u Ubuntu 16.04 uitvoert op een PC (x86 of x64). Op een andere versie van Ubuntu, of een andere Linux-distributie en pas deze stappen om uw omgeving.

Vervolgens [de SDK downloaden](https://aka.ms/csspeech/linuxbinary) en pak de bestanden op in een map van uw keuze. Deze tabel bevat de SDK-mapstructuur.

|Pad|Beschrijving|
|-|-|
|`license.md`|Licentie|
|`third-party-notices.md`|Kennisgevingen van derden|
|`include`|Header-bestanden voor C en C++|
|`lib/x64`|Systeemeigen x64 bibliotheek voor het koppelen met uw toepassing|
|`lib/x86`|Systeemeigen x86 bibliotheek voor het koppelen met uw toepassing|

Voor het maken van een toepassing, kopiëren of verplaatsen van de vereiste binaire bestanden (en -bibliotheken) naar uw ontwikkelomgeving en nemen ze zo nodig in uw buildproces.

## <a name="get-the-java-sdk"></a>Ophalen van de Java SDK

De Java-SDK voor Android wordt geleverd als een [AAR (Android-bibliotheek)](https://developer.android.com/studio/projects/android-library), waaronder de vereiste bibliotheken, evenals de vereiste Android-machtigingen voor het gebruik ervan.
Deze wordt gehost in een Maven-opslagplaats op `https://csspeechstorage.blob.core.windows.net/maven/` als pakket `com.microsoft.cognitiveservices.speech:client-sdk:0.5.0`.
De verbruiken het pakket van uw Android Studio-project de volgende wijzigingen aanbrengen:

* In de project-niveau `build.gradle` bestand, voeg de volgende in de `repository` sectie:

  ```text
  maven { url 'https://csspeechstorage.blob.core.windows.net/maven/' }
  ```

* In de module-niveau `build.gradle` bestand, voeg de volgende in de `dependencies` sectie:

  ```text
  implementation 'com.microsoft.cognitiveservices.speech:client-sdk:0.5.0'
  ```

De Java-SDK is ook deel uit van de [spraak Devices SDK](speech-devices-sdk.md).

## <a name="next-steps"></a>Volgende stappen

* [Uw proefabonnement voor Speech ophalen](https://azure.microsoft.com/try/cognitive-services/)
* [Zie voor het herkennen van gesproken tekst in C#](quickstart-csharp-dotnet-windows.md)
