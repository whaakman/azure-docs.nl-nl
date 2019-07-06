---
title: Text to Speech uitvoer - Speech Services aanpassen
titleSuffix: Azure Cognitive Services
description: Schakel logboekregistratie in de spraak-SDK.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: erhopf
ms.openlocfilehash: 94b58279b1a9fd4d9acdb4183f59b0a8579c17fd
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67606451"
---
# <a name="fine-tune-text-to-speech-output"></a>Uitvoer tekst-naar-spraak afstellen

Azure Speech Services kunt u aan te passen van de snelheid, uitspraak van volume, inspiratie en omtrek van tekst naar spraak uitvoer met behulp van [spraak synthese Markup Language (SSML)](speech-synthesis-markup.md). SSML is een op XML gebaseerde opmaakcodetaal die gebruikmaakt van labels aan de service te informeren over welke functie afstemmen vereist. Het bericht SSML wordt verzonden in de hoofdtekst van elke aanvraag naar de Text to Speech-service. Ter vereenvoudiging van het aanpassingsproces voor, bieden de Speech Services nu een [stem afstemmen](https://aka.ms/voicetuning) hulpprogramma waarmee u visueel inspecteren en afstemmen van de Text to Speech uitvoer in realtime.

Het hulpprogramma voor spraak afstemmen ondersteunt Microsofts [standard](language-support.md#standard-voices), [neurale](language-support.md#text-to-speech), en [aangepaste stemmen](how-to-customize-voice-font.md).

## <a name="get-started-with-the-voice-tuning-tool"></a>Aan de slag met het hulpprogramma voor spraak afstemmen

Voordat u de Text to Speech uitvoer met het hulpprogramma voor het afstemmen van stem aan te passen begint, moet u deze stappen uitvoeren:

1. Maak een [gratis Microsoft-account](https://account.microsoft.com/account) als u er nog geen hebt.
2. Maak een [gratis Azure-account](https://azure.microsoft.com/free/) als u er nog geen hebt. Klik op **gratis starten**, en maak een nieuwe Azure-account met behulp van uw Microsoft-account.

3. Een abonnement Speech Services maken in Azure portal. Stapsgewijze instructies voor het [over het maken van een resource spraak](https://docs.microsoft.com/azure/cognitive-services/speech-service/get-started#create-a-speech-resource-in-azure) beschikbaar zijn.
   >[!NOTE]
   >Wanneer u een spraak-resource in Azure portal maakt, moet de Azure-locatie-informatie komt overeen met de TTS voice-regio. Neurale TTS-stem biedt ondersteuning voor een set sub van Azure-locaties. Zie voor een volledige lijst met ondersteuning, [regio's](regions.md#text-to-speech).

   >[!NOTE]
   >U moet een F0 of een S0-sleutel in de Azure-portal hebt gemaakt voordat u de service kunt gebruiken. Stem afstemmen **niet** ondersteuning voor de [30 dagen gratis proefversie sleutel](https://docs.microsoft.com/azure/cognitive-services/speech-service/get-started?branch=release-build-cogserv-speech-services#free-trial).

4. Aanmelden bij de [stem afstemmen](https://aka.ms/voicetuning) portal, en verbinding maken met uw abonnement Speech Services. Kies één spraakservices abonnement en maak vervolgens een project.
5. Selecteer **nieuwe afstemmen**. Volg deze stappen:

   * Zoek en selecteer **alle abonnementen**.  
   * Selecteer **verbinding maken met bestaande abonnement**.  
     ![Verbinding maken met een bestaand abonnement](./media/custom-voice/custom-voice-connect-subscription.png).
   * Geef uw abonnementssleutel Azure Speech Services, en selecteer vervolgens **toevoegen**. Uw abonnementssleutels zijn beschikbaar in de portal van de spraakherkenning aanpassen van de [pagina abonnement](https://go.microsoft.com/fwlink/?linkid=2090458). U kunt de sleutels ook krijgen in het deelvenster Beheer van Resource in de [Azure-portal](https://portal.azure.com/).
   * Hebt u meer dan één Speech Services-abonnement dat u wilt gebruiken, Herhaal deze stappen voor elk abonnement.

## <a name="customize-the-text-to-speech-output"></a>De Text to Speech uitvoer aanpassen

Nu dat u hebt gemaakt van accounts en uw abonnement is gekoppeld, kunt u beginnen met het afstemmen van de Text to Speech uitvoer.

1. Kies een stem.
2. Voer de tekst die u wilt bewerken.
3. Voordat u bewerkingen uitvoeren, moet u de audio om een idee voor de uitvoer afspelen.
4. Selecteer de word/zin dat u wilt verfijnen en ga experimenteren met verschillende SSML op basis van functies.

>[!TIP]
> Zie voor gedetailleerde informatie over SSML aanpassen en het afstemmen van de stem-uitvoer, [spraak synthese Markup Language (SSML)](speech-synthesis-markup.md).

## <a name="limitations"></a>Beperkingen

Neurale stem afstemmen verschilt enigszins afstemming voor Standard- en aangepaste stemmen.

* Intonation wordt niet ondersteund voor Neurale stemmen.
* Presentatie en volume functies werken alleen met volledige zinnen. Deze functies zijn niet beschikbaar op de word-niveau.
* Voor de frequentie, kunnen sommige Neurale stemmen worden afgestemd op basis van woorden, terwijl andere vereisen dat u volledige zinnen selecteren.

> [!TIP]
> Het hulpprogramma stem afstemmen biedt contextuele informatie over functies en afstemmen.

## <a name="next-steps"></a>Volgende stappen
* [Een spraak-resource maken in Azure](https://docs.microsoft.com/azure/cognitive-services/speech-service/get-started#create-a-speech-resource-in-azure)
* [Start stem afstemmen](https://speech.microsoft.com/app.html#/VoiceTuning)
* [Spraak synthese Markup Language (SSML)](speech-synthesis-markup.md)
