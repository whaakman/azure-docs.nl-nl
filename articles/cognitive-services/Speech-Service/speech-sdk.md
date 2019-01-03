---
title: Over de Spraakservice-SDK - spraakservices
titleSuffix: Azure Cognitive Services
description: De spraak-Service Software Development Kit (SDK) biedt uw toepassingen systeemeigen toegang tot de functies van de spraak-service, waardoor het gemakkelijker wordt om software te ontwikkelen. In dit artikel biedt aanvullende informatie over de SDK voor Windows, Linux- en Android.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: conceptual
ms.date: 12/18/2018
ms.author: wolfma
ms.custom: seodec18
ms.openlocfilehash: 1c29ed47e499ee23fab9f3b34e3974f479e3dbb7
ms.sourcegitcommit: 4eeeb520acf8b2419bcc73d8fcc81a075b81663a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2018
ms.locfileid: "53604268"
---
# <a name="about-the-speech-service-sdk"></a>Over de Spraakservice-SDK

De spraak-Service Software Development Kit (SDK) biedt uw toepassingen systeemeigen toegang tot de functies van de spraak-service, waardoor het gemakkelijker wordt om software te ontwikkelen. De SDK biedt op dit moment toegang tot **spraak naar tekst**, **Spraakomzetting**, en **bedoeling erkenning**.

[!INCLUDE [Speech SDK Platforms](../../../includes/cognitive-services-speech-service-speech-sdk-platforms.md)]

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

## <a name="get-the-sdk"></a>Download de SDK

### <a name="windows"></a>Windows

Bij Windows, wordt de volgende talen ondersteund:

* C#(UWP- en .NET), C++: U kunt verwijzen naar en de nieuwste versie van onze spraak SDK NuGet-pakket gebruiken. Het pakket bevat 32-bits en 64-bits-clientbibliotheken en beheerde (.NET)-bibliotheken. De SDK kan worden geïnstalleerd in Visual Studio met behulp van NuGet. Zoeken naar **Microsoft.CognitiveServices.Speech**.

* Java: U kunt verwijzen naar en de nieuwste versie van onze spraak SDK Maven-pakket, die ondersteuning biedt voor alleen Windows x64 gebruiken. Voeg in uw Maven-project `https://csspeechstorage.blob.core.windows.net/maven/` als een aanvullende opslagplaats en een verwijzing `com.microsoft.cognitiveservices.speech:client-sdk:1.2.0` als een afhankelijkheid.

### <a name="linux"></a>Linux

> [!NOTE]
> Op dit moment ondersteunen we alleen Ubuntu 16.04 en 18.04 op een PC (x86 of x64 voor het ontwikkelen van C++ en x64 voor .NET Core en Java).

Zorg ervoor dat u hebt de vereiste compiler en -bibliotheken geïnstalleerd door het uitvoeren van de volgende shellopdrachten:

```sh
sudo apt-get update
sudo apt-get install build-essential libssl1.0.0 libcurl3 libasound2
```

* C#: U kunt verwijzen naar en de nieuwste versie van onze spraak SDK NuGet-pakket gebruiken. Om te verwijzen naar de SDK, het volgende pakketverwijzing toevoegen aan uw project:

  ```xml
  <PackageReference Include="Microsoft.CognitiveServices.Speech" Version="1.2.0" />
  ```

* Java: U kunt verwijzen naar en de nieuwste versie van onze spraak SDK Maven-pakket gebruiken. Voeg in uw Maven-project `https://csspeechstorage.blob.core.windows.net/maven/` als een aanvullende opslagplaats en een verwijzing `com.microsoft.cognitiveservices.speech:client-sdk:1.2.0` als een afhankelijkheid.

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

De Java-SDK voor Android wordt geleverd als een [AAR (Android-bibliotheek)](https://developer.android.com/studio/projects/android-library), waaronder de vereiste bibliotheken en Android-machtigingen vereist. Deze wordt gehost in een Maven-opslagplaats op `https://csspeechstorage.blob.core.windows.net/maven/` als pakket `com.microsoft.cognitiveservices.speech:client-sdk:1.2.0`.

Het pakket van uw Android Studio-project gebruiken, moet u de volgende wijzigingen aanbrengen:

* In het niveau van het project build.gradle-bestand, het volgende toevoegen aan de `repository` sectie:

  ```text
  maven { url 'https://csspeechstorage.blob.core.windows.net/maven/' }
  ```

* In de module op serverniveau build.gradle-bestand, het volgende toevoegen aan de `dependencies` sectie:

  ```text
  implementation 'com.microsoft.cognitiveservices.speech:client-sdk:1.2.0'
  ```

De Java-SDK is ook deel uit van de [spraak Devices SDK](speech-devices-sdk.md).

[!INCLUDE [Get the samples](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]

## <a name="next-steps"></a>Volgende stappen

* [Uw proefabonnement voor Speech ophalen](https://azure.microsoft.com/try/cognitive-services/)
* [Zie voor het herkennen van gesproken tekst in C#](quickstart-csharp-dotnet-windows.md)
