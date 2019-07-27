---
title: Aangepaste spraak-eerste virtuele assistenten (preview)-spraak service
titleSuffix: Azure Cognitive Services
description: Een overzicht van de functies, mogelijkheden en beperkingen voor aangepaste spraak-eerste virtuele assistenten met behulp van het directe lijn spraak kanaal op het bot-Framework en de Cognitive Services speech Software Development Kit (SDK).
services: cognitive-services
author: trrwilson
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: travisw
ms.openlocfilehash: 8a12c9e5d569092440552958554681c0b1d79590
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68552539"
---
# <a name="about-custom-voice-first-virtual-assistants-preview"></a>Over de aangepaste spraak-eerste virtuele assistenten preview

Met aangepaste virtuele assistenten die gebruikmaken van Azure speech Services kunnen ontwikkel aars natuurlijke, menselijke-achtige gespreks interfaces maken voor hun toepassingen en ervaringen. Het directe-lijn spraak kanaal van het bot-Framework verbetert deze mogelijkheden door een gecoördineerd, georganisatied ingangs punt te bieden aan een compatibele bot waarmee gesp roken communicatie mogelijk is met lage latentie en hoge betrouw baarheid. Deze bots kunnen gebruikmaken van de Language Understanding (LUIS) van micro soft voor interactie in natuurlijke taal. Direct line speech is toegankelijk via apparaten met de speech Software Development Kit (SDK).

   ![Conceptueel diagram van de service stroom van de directe lijn spraak] indeling (media/voice-first-virtual-assistants/overview.png "De stroom van het spraak kanaal")


Direct line speech en de bijbehorende functionaliteit voor aangepaste spraak-eerste virtuele assistenten zijn een ideale aanvulling op de [Virtual assistent-oplossing en de ondernemings sjabloon](https://docs.microsoft.com/azure/bot-service/bot-builder-enterprise-template-overview). Hoewel directe lijn spraak kan worden gebruikt met een compatibele bot, bieden deze resources een herbruikbare basis lijn voor gesp roken ervaringen van hoge kwaliteit, evenals algemene ondersteunende vaardig heden en modellen om snel aan de slag te gaan.


## <a name="core-features"></a>Kern functies

| Categorie | Functies |
|----------|----------|
|[Aangepast Ontwaak woord](speech-devices-sdk-create-kws.md) | U kunt gebruikers in staat stellen om gesp rekken met bots te starten met een aangepast tref woord zoals ' Hey Contoso '. Deze taak wordt uitgevoerd met een aangepaste Wake woord-engine in de Speech SDK, die kan worden geconfigureerd met een aangepast Ontwaak woord [dat u hier kunt genereren](speech-devices-sdk-create-kws.md). Het directe-lijn spraak kanaal bevat de Wake-woord verificatie aan de service zijde, waarmee de nauw keurigheid van de activering van de Wake-functie alleen op het apparaat wordt verbeterd.
|[Spraak naar tekst](speech-to-text.md) | Het directe lijn spraak kanaal bevat real-time transcriptie van audio in herkende tekst met behulp van [spraak-naar-tekst](speech-to-text.md) van Azure speech Services. Deze tekst is beschikbaar voor zowel uw bot als uw client toepassing, omdat deze is getranscribeerd.
|[Tekst-naar-spraak](text-to-speech.md) | Tekstuele reacties van uw bot worden gesynthesizerd met behulp van [tekst naar spraak](text-to-speech.md) vanuit Azure speech Services. Deze synthese wordt vervolgens beschikbaar gesteld voor uw client toepassing als een audio stroom. Micro soft biedt u de mogelijkheid om uw eigen aangepaste Neural TTS-stem van hoge kwaliteit op te bouwen die een stem op uw merk levert, voor meer informatie over [contact met ons](mailto:mstts@microsoft.com).
|[Directe lijn spraak](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-directlinespeech) | Als een kanaal binnen het bot-Framework, biedt directe lijn spraak een soepele en naadloze verbinding tussen uw client toepassing, een compatibele bot en de mogelijkheden van Azure speech Services. Zie de [pagina van de bot Framework-documentatie](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-directlinespeech)voor meer informatie over het configureren van uw bot voor het gebruik van het directe lijn spraak kanaal.

## <a name="sample-code"></a>Voorbeeldcode

Voorbeeld code voor het maken van een televoice-eerste virtuele assistent is beschikbaar op GitHub. Deze voor beelden beslaan de client toepassing voor het maken van verbinding met uw bot in verschillende populaire programmeer talen.

* [Voor beelden van spraak-eerste virtuele assistent (SDK)](https://aka.ms/csspeech/samples)
* [Quick Start: virtuele assistenten voor spraak (C#)](quickstart-virtual-assistant-csharp-uwp.md)
* [Snelstartgids: virtuele assistenten voor spraak (Java)](quickstart-virtual-assistant-java-jre.md)
* [Direct line speech-clientC#(, WPF)](https://github.com/Azure-Samples/Cognitive-Services-Direct-Line-Speech-Client)

## <a name="customization"></a>Aanpassing

Voor de spraak-eerste virtuele assistenten die zijn gemaakt met behulp van Azure speech Services, kunt u gebruikmaken van de volledige aanpassings opties die beschikbaar zijn voor [spraak naar tekst](speech-to-text.md), [tekst naar spraak](text-to-speech.md)en [aangepaste trefwoord selectie](speech-devices-sdk-create-kws.md).

> [!NOTE]
> Aanpassings opties variëren per taal/land instelling (Zie [ondersteunde talen](supported-languages.md)).

## <a name="reference-docs"></a>Referentiedocumenten

* [Speech-SDK](speech-sdk-reference.md)
* [Azure Bot Service](https://docs.microsoft.com/azure/bot-service/?view=azure-bot-service-4.0)

## <a name="next-steps"></a>Volgende stappen

* [Gratis een abonnements sleutel voor spraak Services aanschaffen](get-started.md)
* [De Speech SDK ophalen](speech-sdk.md)
* [Een basisbot maken en implementeren](https://docs.microsoft.com/azure/bot-service/bot-builder-tutorial-basic-deploy?view=azure-bot-service-4.0)
* [De Virtual assistent-oplossing en ondernemings sjabloon ophalen](https://github.com/Microsoft/AI)
