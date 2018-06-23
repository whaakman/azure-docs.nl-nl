---
title: Over de spraak cognitieve Services SDK | Microsoft Docs
description: Een overzicht van de SDK's zijn beschikbaar voor de service spraak.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
manager: noellelacharite
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/07/2018
ms.author: v-jerkin
ms.openlocfilehash: b9b7b8af5ce3d75788fd2c4f5e0309b5ca561a8f
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345829"
---
# <a name="about-the-cognitive-services-speech-sdk"></a>Over de spraak cognitieve Services SDK

De cognitieve Services spraak Software Development Kit (SDK) biedt uw toepassingen systeemeigen toegang tot de functies van de service spraak, waardoor het gemakkelijker om software te ontwikkelen. Op dit moment is de SDK biedt toegang tot **spraak naar tekst**, **spraak vertaling**, en **bedoeling erkenning**.

De tabel worden de besturingssystemen en programmeertalen worden momenteel ondersteund.

|Ondersteund besturingssysteem|Programmeertaal|
|-|-|
|Windows|C/C++, C#|
|Linux|C/C++|
|Apparaten|Java\*|

\* *De Java SDK maakt deel uit van de [spraak apparaten SDK](speech-devices-sdk.md).*

## <a name="get-the-windows-sdk"></a>Ophalen van de Windows-SDK

De Windows-versie van de SDK spraak omvat zowel 32-bits en 64-bits C/C++-clientbibliotheken als beheerde (.NET)-bibliotheken voor gebruik met C#. De SDK kan worden geïnstalleerd in Visual Studio met NuGet; Zoek naar `Microsoft.CognitiveServices.Speech`.

## <a name="get-the-linux-sdk"></a>Download de Linux-SDK

Zorg ervoor dat u de vereiste compiler en bibliotheken hebt met de volgende shellopdrachten:

```sh
sudo apt-get update
sudo apt-get install build-essential libssl1.0.0 libcurl3 libasound2
```

> [!NOTE]
> Deze instructies wordt ervan uitgegaan dat u Ubuntu 16.04 uitvoert op een PC (x86 of x64). Op een andere Ubuntu-versie of een ander distributiepunt voor Linux en aan te passen deze stappen voor uw omgeving.

Vervolgens [de SDK downloaden](https://aka.ms/csspeech/linuxbinary) en pak de bestanden in een map van uw keuze. Deze tabel ziet de SDK-mapstructuur.

|Pad|Beschrijving|
|-|-|
|`license.md`|Licentie|
|`third-party-notices.md`|Kennisgevingen van derden|
|`include`|Header-bestanden voor C en C++|
|`lib/x64`|Systeemeigen x64-bibliotheek voor koppelen met uw toepassing|
|`lib/x86`|Systeemeigen x86-bibliotheek voor koppelen met uw toepassing|

Een toepassing maken, kopiëren of verplaatsen van de vereiste binaire bestanden (en bibliotheken) in uw ontwikkelomgeving en deze waar nodig in uw buildproces opnemen.

## <a name="get-the-java-sdk"></a>Ophalen van de Java SDK

De Java SDK maakt deel uit van de [spraak apparaten SDK](speech-devices-sdk.md).

## <a name="next-steps"></a>Volgende stappen

* [Uw proefabonnement spraak ophalen](https://azure.microsoft.com/try/cognitive-services/)
* [Zie het herkennen van spraak in C#](quickstart-csharp-windows.md)
