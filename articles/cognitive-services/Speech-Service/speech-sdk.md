---
title: Informatie over de Speech-SDK - spraakservices
titleSuffix: Azure Cognitive Services
description: De spraak Software Development Kit (SDK) biedt uw toepassingen systeemeigen toegang tot de functies van de spraak-service, waardoor het gemakkelijker wordt om software te ontwikkelen. In dit artikel biedt aanvullende informatie over de SDK voor Windows, Linux- en Android.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 2/20/2019
ms.author: wolfma
ms.custom: seodec18
ms.openlocfilehash: 73dcddf796faa3b39dbb124fe6c528bb19eda3a9
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57841222"
---
# <a name="about-the-speech-sdk"></a>Informatie over de Speech-SDK

Spraak Software Development Kit (SDK) biedt uw toepassingen toegang tot de functies van de Services Speech, waardoor het gemakkelijker wordt om spraak ingeschakelde software te ontwikkelen. Op dit moment de SDK's bieden toegang tot **spraak naar tekst**, **Spraakomzetting**, en **bedoeling erkenning**. Een algemeen overzicht over de mogelijkheden en ondersteunde platforms kunt u vinden op de documentatie [vermelding pagina](https://aka.ms/csspeech).

[!INCLUDE [Speech SDK Platforms](../../../includes/cognitive-services-speech-service-speech-sdk-platforms.md)]

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

## <a name="get-the-sdk"></a>Download de SDK

### <a name="windows"></a>Windows

Bij Windows, wordt de volgende talen ondersteund:

* C#(UWP- en .NET), C++: U kunt verwijzen naar en de nieuwste versie van onze spraak SDK NuGet-pakket gebruiken. Het pakket bevat 32-bits en 64-bits-clientbibliotheken en beheerde (.NET)-bibliotheken. De SDK kan worden geïnstalleerd in Visual Studio met behulp van NuGet. Zoeken naar **Microsoft.CognitiveServices.Speech**.

* Java: U kunt verwijzen naar en de nieuwste versie van onze spraak SDK Maven-pakket, die ondersteuning biedt voor alleen Windows x64 gebruiken. Voeg in uw Maven-project `https://csspeechstorage.blob.core.windows.net/maven/` als een aanvullende opslagplaats en een verwijzing `com.microsoft.cognitiveservices.speech:client-sdk:1.3.1` als een afhankelijkheid.

### <a name="linux"></a>Linux

> [!NOTE]
> Op dit moment ondersteunen we alleen Ubuntu 16.04 en 18.04 op een PC (x86 of x64 voor het ontwikkelen van C++ en x64 voor .NET Core, Java en Python).

Zorg ervoor dat u hebt de vereiste compiler en -bibliotheken geïnstalleerd door het uitvoeren van de volgende shellopdrachten:

```sh
sudo apt-get update
sudo apt-get install build-essential libssl1.0.0 libasound2
```

* C#: U kunt verwijzen naar en de nieuwste versie van onze spraak SDK NuGet-pakket gebruiken. Om te verwijzen naar de SDK, het volgende pakketverwijzing toevoegen aan uw project:

  ```xml
  <PackageReference Include="Microsoft.CognitiveServices.Speech" Version="1.3.1" />
  ```

* Java: U kunt verwijzen naar en de nieuwste versie van onze spraak SDK Maven-pakket gebruiken. Voeg in uw Maven-project `https://csspeechstorage.blob.core.windows.net/maven/` als een aanvullende opslagplaats en een verwijzing `com.microsoft.cognitiveservices.speech:client-sdk:1.3.1` als een afhankelijkheid.

* C++: Download de SDK als een [tar-clientinstallatiepakket](https://aka.ms/csspeech/linuxbinary) en pak de bestanden in een map van uw keuze. De volgende tabel bevat de SDK-mapstructuur:

  |Pad|Description|
  |-|-|
  |`license.md`|Licentie|
  |`ThirdPartyNotices.md`|Mededelingen van derden|
  |`include`|Header-bestanden voor C en C++|
  |`lib/x64`|Systeemeigen x64 bibliotheek voor het koppelen met uw toepassing|
  |`lib/x86`|Systeemeigen x86 bibliotheek voor het koppelen met uw toepassing|

  Voor het maken van een toepassing, kopiëren of verplaatsen van de vereiste binaire bestanden (en -bibliotheken) in uw ontwikkelomgeving. Deze waar nodig in uw build-proces opnemen.

### <a name="android"></a>Android

De Java-SDK voor Android wordt geleverd als een [AAR (Android-bibliotheek)](https://developer.android.com/studio/projects/android-library), waaronder de vereiste bibliotheken en Android-machtigingen vereist. Deze wordt gehost in een Maven-opslagplaats op `https://csspeechstorage.blob.core.windows.net/maven/` als pakket `com.microsoft.cognitiveservices.speech:client-sdk:1.3.1`.

Het pakket van uw Android Studio-project gebruiken, moet u de volgende wijzigingen aanbrengen:

* In het niveau van het project build.gradle-bestand, het volgende toevoegen aan de `repository` sectie:

  ```gradle
  maven { url 'https://csspeechstorage.blob.core.windows.net/maven/' }
  ```

* In de module op serverniveau build.gradle-bestand, het volgende toevoegen aan de `dependencies` sectie:

  ```gradle
  implementation 'com.microsoft.cognitiveservices.speech:client-sdk:1.3.1'
  ```

De Java-SDK is ook deel uit van de [spraak Devices SDK](speech-devices-sdk.md).

[!INCLUDE [Get the samples](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]

## <a name="next-steps"></a>Volgende stappen

* [Uw proefabonnement voor Speech ophalen](https://azure.microsoft.com/try/cognitive-services/)
* [Zie voor het herkennen van gesproken tekst in C#](quickstart-csharp-dotnet-windows.md)
