---
title: Aangepaste spraak op de eerste virtuele-assistenten (Preview) - spraakservices
titleSuffix: Azure Cognitive Services
description: Een overzicht van de functies, mogelijkheden en beperkingen voor aangepaste gesproken de eerste virtuele assistenten met behulp van het kanaal directe regel spraak voor Bot Framework en de Cognitive Services spraak Software Development Kit (SDK).
services: cognitive-services
author: trrwilson
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: travisw
ms.custom: ''
ms.openlocfilehash: f40e53c67f099fea739e45f6bdc6963ee1e235cb
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/02/2019
ms.locfileid: "65026102"
---
# <a name="about-custom-voice-first-virtual-assistants-preview"></a>Over aangepaste spraak op de eerste virtuele assistenten Preview-versie

Aangepaste virtuele assistenten met behulp van Azure Speech Services meer mogelijkheden bieden ontwikkelaars voor het maken van de natuurlijke, menselijke conversatie-interfaces voor hun toepassingen en ervaringen. De Bot Framework directe regel spraak kanaal verbetert deze mogelijkheden door te geven van een gecoördineerde, gecoördineerd toegangspunt een compatibel bot waarmee spraak in spraak van interactie met lage latentie en hoge betrouwbaarheid. Deze bots kunt van Microsoft Language Understanding (LUIS) gebruiken voor interactie met de natuurlijke taal. Directe regel spraak is toegankelijk voor apparaten met behulp van de Speech Services Software Development Kit (SDK).

   ![Conceptueel diagram van de service-stroom van rechtstreekse regel spraak orchestration](media/voice-first-virtual-assistants/overview.png "stroom van de spraak-kanaal")

Directe regel spraak- en de bijbehorende functionaliteit voor aangepaste gesproken de eerste virtuele assistenten zijn een ideale aanvulling op de [Virtual Assistant Solution](https://docs.microsoft.com/azure/bot-service/bot-builder-virtual-assistant-introduction) en [sjabloon van de onderneming](https://docs.microsoft.com/azure/bot-service/bot-builder-enterprise-template-overview). Hoewel directe regel spraak met een compatibele bot werken kunt, bieden deze resources een herbruikbare basislijn voor conversatie ervaringen van hoge kwaliteit, evenals algemene ondersteunende vaardigheden en -modellen voor snel aan de slag.

## <a name="core-features"></a>Belangrijkste functies

| Category | Functies |
|----------|----------|
|[Aangepaste wake word](speech-devices-sdk-create-kws.md) | U kunt gebruikers om te beginnen met gesprekken met bots met behulp van een aangepaste trefwoord, zoals "Hallo Contoso." Deze taak wordt uitgevoerd met een aangepaste wake word-engine in de SDK-spraak, die kan worden geconfigureerd met een aangepaste wake-woord [die u hier kunt genereren](speech-devices-sdk-create-kws.md). Het kanaal directe regel spraak bevat servicezijde wake word verificatie waarmee de nauwkeurigheid van de activering wake word ten opzichte van het apparaat alleen worden verbeterd.
|[Spraak naar tekst](speech-to-text.md) | Het kanaal directe regel spraak bevat realtime transcriptie van audio in de herkende tekst met [spraak-naar-tekst](speech-to-text.md) van Azure Speech Services. Deze tekst is beschikbaar voor zowel uw bot als uw clienttoepassing zoals deze wordt omgezet.
|[Tekst naar spraak](text-to-speech.md) | Tekstuele reacties van uw bot worden gemaakt met behulp van [tekst naar spraak](text-to-speech.md) van Azure Speech Services. Deze synthese wordt vervolgens naar uw clienttoepassing als een audiostream beschikbaar worden gemaakt. Microsoft biedt de mogelijkheid voor het bouwen van uw eigen [aangepaste, hoogwaardige Neurale TTS-stem](https://aka.ms/customneuraltts) die een stem geeft tot uw merk.
|[Rechtstreekse regel spraak](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-directlinespeech.md) | Als een kanaal binnen de Bot Framework kunt directe regel spraak een probleemloze verbinding tussen de clienttoepassing, een compatibel bot en de mogelijkheden van Azure Speech Services. Zie voor meer informatie over het configureren van uw bot voor het gebruik van het kanaal directe regel spraak [een pagina in de documentatie van Bot Framework](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-directlinespeech.md).

## <a name="sample-code"></a>Voorbeeldcode

Voorbeeldcode voor het maken van een stem op de eerste virtuele assistent is beschikbaar op GitHub. Deze voorbeelden hebben betrekking op de clienttoepassing voor het verbinden met uw bot in verschillende populaire programmeertalen.

* [Stem op de eerste virtuele assistent voorbeelden (SDK)](https://aka.ms/csspeech/samples)
* [Snelstartgids: voice-first virtuele assistenten (C#)](quickstart-virtual-assistant-csharp-uwp.md)
* [Snelstartgids: stem op de eerste virtuele assistenten (Java)](quickstart-virtual-assistant-java-jre.md)

## <a name="customization"></a>Aanpassing

Stem op de eerste virtuele assistenten gebouwd met behulp van Azure Speech Services kunnen gebruiken het volledige bereik van aanpassingsopties die beschikbaar zijn voor [spraak-naar-tekst](speech-to-text.md), [tekst naar spraak](text-to-speech.md), en [aangepaste trefwoord selectie](speech-devices-sdk-create-kws.md).

> [!NOTE]
> Opties voor het aanpassen variëren per taal/landinstelling (Zie [ondersteunde talen](supported-languages.md)).

## <a name="reference-docs"></a>Referentiedocumenten

* [Speech-SDK](speech-sdk-reference.md)
* [Azure Bot Service](https://docs.microsoft.com/azure/bot-service/?view=azure-bot-service-4.0)

## <a name="next-steps"></a>Volgende stappen

* [Ontvangt u een abonnementssleutel Speech Services gratis](get-started.md)
* [De spraak-SDK ophalen](speech-sdk.md)
* [Maken en implementeren van een basic-bot](https://docs.microsoft.com/azure/bot-service/bot-builder-tutorial-basic-deploy?view=azure-bot-service-4.0)
* [Ophalen van de virtuele-assistent oplossing en de Enterprise-sjabloon](https://github.com/Microsoft/AI)
