---
title: Spraak apparaten SDK-documentatie
titleSuffix: Azure Cognitive Services
description: Opmerkingen bij de release - wat is gewijzigd in de meest recente versies
services: cognitive-services
author: wsturman
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 08/07/2018
ms.author: gracez
ms.openlocfilehash: 4d802b9f71edee1eec4b2c92881e2a8796db2865
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58005509"
---
# <a name="release-notes-of-cognitive-services-speech-devices-sdk"></a>Opmerkingen bij de release van Cognitive Services spraak Devices SDK

De volgende secties lijst met wijzigingen in de meest recente versies.

## <a name="cognitive-services-speech-devices-sdk-131-2019-mar-release"></a>Cognitive Services spraak apparaten SDK 1.3.1: 2019 maart-release 

* Bijgewerkt de [spraak SDK](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) onderdeel naar versie 1.3.1. Zie voor meer informatie de [opmerkingen bij de release](https://aka.ms/csspeech/whatsnew). 
*   Bijgewerkte wake woord verwerken, belangrijke wijzigingen zien.
*   Voorbeeld van een toepassing wordt toegevoegd keuze van taal spraakherkenning en voor omzetting.

**Belangrijke wijzigingen** 

*   [Installeren van een woord wake](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-devices-sdk-qsg#run-a-sample-application) is vereenvoudigd, het is nu onderdeel van de app en hiervoor hoeft geen afzonderlijke installatie op het apparaat.
*   De opname van wake word is gewijzigd en twee gebeurtenissen worden ondersteund.
    - RecognizingKeyword, geeft aan dat het resultaat van spraak (niet-geverifieerde) sleutelwoord tekst bevat.
    - RecognizedKeyword, geeft aan dat sleutelwoord de opname voltooid herkennen van het opgegeven trefwoord.


## <a name="cognitive-services-speech-devices-sdk-110-2018-nov-release"></a>Cognitive Services spraak apparaten SDK 1.1.0: November 2018-release 

* Bijgewerkt de [spraak SDK](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) onderdeel naar versie 1.1.0. Zie voor meer informatie de [opmerkingen bij de release](https://aka.ms/csspeech/whatsnew). 
* Uiterst veld spraakherkenning is, verbeterd met onze verbeterde verwerking van audio-algoritme.
* Voorbeeld van een toepassing Chinese spraakherkenning ondersteuning toegevoegd.

## <a name="cognitive-services-speech-devices-sdk-101-2018-oct-release"></a>Cognitive Services spraak apparaten SDK 1.0.1: Release van oktober 2018 

* Bijgewerkt de [spraak SDK](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) onderdeel naar versie 1.0.1. Zie voor meer informatie de [opmerkingen bij de release](https://aka.ms/csspeech/whatsnew). 
* Nauwkeurigheid van de spraakherkenning zal worden verbeterd met onze verbeterde verwerking van audio-algoritme  
* Een continue erkenning audio-sessie-fout is verholpen.

**Belangrijke wijzigingen** 

* Met deze release zijn een aantal belangrijke wijzigingen worden geïntroduceerd. Controleer of [deze pagina](https://aka.ms/csspeech/breakingchanges_1_0_0) voor meer informatie met betrekking tot de API's. 
* Het model KWS bestanden zijn niet compatibel met spraak Devices SDK 1.0.1. De bestaande Wake Word-bestanden worden verwijderd nadat de nieuwe Wake Word-bestanden worden geschreven naar het apparaat. 

## <a name="cognitive-services-speech-devices-sdk-050-2018-aug-release"></a>Cognitive Services spraak apparaten SDK 0.5.0: 2018-Aug release

* De nauwkeurigheid van de spraakherkenning verbeterd door een bug in de van audio-verwerkingscode op te lossen.
* Bijgewerkt de [spraak SDK](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) onderdeel naar versie 0.5.0. Zie voor meer informatie de [opmerkingen bij de release](releasenotes.md#cognitive-services-speech-sdk-050-2018-july-release).

## <a name="cognitive-services-speech-devices-sdk-0212733-2018-may-release"></a>Cognitive Services Speech Devices SDK 0.2.12733: 2018-mei release

De eerste openbare preview-versie van de Cognitive Services spraak apparaten SDK.
