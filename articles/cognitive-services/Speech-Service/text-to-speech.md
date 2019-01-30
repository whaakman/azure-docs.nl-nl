---
title: Over Text to Speech-Service voor spraak
titleSuffix: Azure Cognitive Services
description: De Text to Speech-API biedt meer dan 75 stemmen in meer dan 45 talen en landen. Voor het gebruik van standard spraakstijlen, hoeft u alleen de stem-naam met een aantal andere parameters opgeven bij het aanroepen van de Speech-Service.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/13/2018
ms.author: erhopf
ms.custom: seodec18
ms.openlocfilehash: b413bd65582dceadd9aab912694c3b560070c4f3
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55251551"
---
# <a name="about-the-text-to-speech-api"></a>Over de Text to Speech-API

De **tekst naar spraak** (Text-to-Speech) API invoertekst converteert naar natuurlijk klinkende spraak (ook wel genoemd *spraaksynthese*).

Voor het genereren van spraak, verzendt uw toepassing HTTP POST-aanvragen naar de Text to Speech-API. Daar tekst is spraaksynthese human klinkende en geretourneerd als een geluidsbestand. Een aantal stemmen en talen worden ondersteund.

In welke spraak synthese wordt toegepast scenario's omvatten:

* *Verbetering van de toegankelijkheid:* **tekst naar spraak** technologie kunnen eigenaren van inhoud en uitgevers om te reageren op de verschillende manieren waarop mensen werken met hun eigen inhoud. Mensen met gezichtsvermogen of problemen bij het lezen waarderen kunnen inhoud aurally gebruiken. Stem ook uitvoer maakt het gemakkelijker voor gebruikers om te profiteren van tekstuele inhoud, zoals newspapers of blogs, op mobiele apparaten bij het nakomen van of als.

* *Reageert in scenario's multitasking:* **tekst naar spraak** kunnen mensen kunnen worden opgevangen belangrijke informatie snel en probleemloos tijdens het verkeer of anders buiten een handige lezen van de omgeving. Navigatie is een algemene toepassing op dit gebied.

* *Verbetering van leren met meerdere modi:* Verschillende mensen Leer best op verschillende manieren. Online learning experts heeft aangetoond dat de spraak- en samen bieden gegevens gemakkelijker helpen kan te leren en te behouden.

* *Intuïtieve bots of assistenten leveren:* De mogelijkheid om te communiceren, kan een integraal onderdeel van een intelligente chatbot of een virtuele assistent zijn. Steeds meer bedrijven zijn chat bots om aantrekkelijke klant een service ervaringen voor hun klanten te ontwikkelen. Stem wordt een andere dimensie toegevoegd doordat de antwoorden van de bot wilt aurally ontvangen (bijvoorbeeld via de telefoon).

## <a name="voice-support"></a>Voice-ondersteuning

De Microsoft **tekst naar spraak** service biedt meer dan 75 stemmen in meer dan 45 talen en landen. Voor het gebruik van deze standaard 'spraakstijlen', moet u alleen de naam van de gesproken met een aantal andere parameters opgeven wanneer u een van de service REST-API aanroept. Zie voor meer informatie over ondersteunde talen, landinstellingen en stemmen [ondersteunde talen](language-support.md#text-to-speech).

> [!IMPORTANT]
> Kosten variëren voor standaard, aangepaste en neurale stemmen. Zie voor meer informatie, [prijzen](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/).

### <a name="neural-voices"></a>Neurale stemmen

Neurale tekst naar spraak kan worden gebruikt om de interacties met chatbots en virtuele assistenten natuurlijker en aantrekkelijke digitale teksten, zoals e-books converteren naar audiobooks en verbeteren van de in de auto-navigatiesystemen. Met de menselijke natuurlijke prosody en duidelijke afbakening van woorden, is Neural TTS aanzienlijk verminderd, luistert intensief gebruik wanneer u met AI-systemen communiceren. Zie voor meer informatie over neurale stemmen [ondersteunde talen](language-support.md#text-to-speech).

### <a name="custom-voices"></a>Aangepaste stemmen

Spraak-naar-spraak aanpassingen kunt u een herkenbare, één van een unieke stem voor uw merk maken: een *spraakstijl.* Voor het maken van uw spraakstijl, moet u een studio-opname maken en uploaden van de bijbehorende scripts als de trainingsgegevens. De service maakt vervolgens een unieke stem-model dat is afgestemd op de opname. U kunt zijn spraakstijl gebruiken om na te bootsen spraak. Zie voor meer informatie, [aangepaste spraakstijlen](how-to-customize-voice-font.md).

## <a name="api-capabilities"></a>API-functies

Veel van de mogelijkheden van de **tekst naar spraak** API, met name wat betreft de aanpassing kunnen zijn beschikbaar via REST. De volgende tabel geeft een overzicht van de mogelijkheden van elke methode van de toegang tot de API. Zie voor een volledige lijst van de mogelijkheden en API-details, [Swagger verwijzing](https://westus.cris.ai/swagger/ui/index).

| Use-case | REST | SDK's |
|-----|-----|-----|----|
| Gegevenssets voor de toon aanpassing uploaden | Ja | Nee |
| Maken en beheren van modellen voor spraak-lettertype | Ja | Nee |
| Maken en beheren van implementaties van spraak lettertype | Ja | Nee |
| Maken en beheren van spraak lettertype tests| Ja | Nee |
| Abonnementen beheren | Ja | Nee |

> [!NOTE]
> De API implementeert die de API-aanvragen voor 25 per vijf seconden beperkingslimieten. Berichtkoppen wordt op de hoogte van de limieten.

## <a name="next-steps"></a>Volgende stappen

* [Neem een abonnement op spraak-gratis Services](https://azure.microsoft.com/try/cognitive-services/)
* [Snelstart: Converteren van tekst naar spraak, Python](quickstart-python-text-to-speech.md)
* [Snelstart: Converteren van tekst naar spraak, .NET Core](quickstart-dotnet-text-to-speech.md)
* [Naslaginformatie over REST API](rest-apis.md)
