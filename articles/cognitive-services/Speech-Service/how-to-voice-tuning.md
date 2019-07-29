---
title: Tekst-naar-spraak-uitvoer voor spraak service nauw keurig afstemmen
titleSuffix: Azure Cognitive Services
description: Schakel logboek registratie in de Speech SDK in.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: erhopf
ms.openlocfilehash: 4cf2338d76ce31f44eaf3fb235e5f8796602d819
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68562750"
---
# <a name="fine-tune-text-to-speech-output"></a>Uitvoer tekst-naar-spraak afstellen

Met Azure speech Services kunt u de snelheid, de uitspraak, het volume, de Toon en de contour van tekst-naar-spraak-uitvoer aanpassen met behulp van [SSML (Speech Synthesis Markup Language)](speech-synthesis-markup.md). SSML is een op XML gebaseerde Markup-taal die gebruikmaakt van tags om de service te informeren over welke functie moet worden afgestemd. Het SSML-bericht wordt vervolgens in de hoofd tekst van elke aanvraag verzonden naar de service tekst naar spraak. Om het aanpassings proces te vereenvoudigen, bieden de spraak Services nu een hulp programma voor [stem afstemming](https://aka.ms/voicetuning) waarmee u tekst-naar-spraak-uitvoer in realtime visueel kunt controleren en verfijnen.

Het hulp programma stem tuning ondersteunt de [standaard](language-support.md#standard-voices)-, [Neural](language-support.md#text-to-speech)-en [aangepaste stemmen](how-to-customize-voice-font.md)van micro soft.

## <a name="get-started-with-the-voice-tuning-tool"></a>Aan de slag met het hulp programma stem tuning

Voordat u kunt beginnen met het verfijnen van de tekst-naar-spraak-uitvoer met het hulp programma stem afstemmen, moet u deze stappen uitvoeren:

1. Maak een [gratis Microsoft-account](https://account.microsoft.com/account) als u er nog geen hebt.
2. Maak een [gratis Azure-account](https://azure.microsoft.com/free/) als u er nog geen hebt. Klik op **gratis starten**en maak een nieuw Azure-account met behulp van uw Microsoft-account.

3. Maak een abonnement voor spraak Services in de Azure Portal. Stapsgewijze instructies voor [het maken van een spraak bron](https://docs.microsoft.com/azure/cognitive-services/speech-service/get-started#create-a-speech-resource-in-azure) zijn beschikbaar.
   >[!NOTE]
   >Wanneer u een spraak bron maakt in de Azure Portal, moet de locatie-informatie van Azure overeenkomen met de TTS-spraak regio. Neural TTS Voice ondersteunt een verzameling Azure-locaties. Zie [regio's](regions.md#text-to-speech)voor een volledige lijst met ondersteuning.

   >[!NOTE]
   >U moet een F0 of een S0-sleutel maken in de Azure Portal voordat u de service kunt gebruiken. Stem afstemmen biedt **geen** ondersteuning voor de [gratis proef versie van 30 dagen](https://docs.microsoft.com/azure/cognitive-services/speech-service/get-started?branch=release-build-cogserv-speech-services#free-trial).

4. Meld u aan bij de portal voor [stem afstemming](https://aka.ms/voicetuning) en verbind uw speech Services-abonnement. Kies een enkel spraak Services-abonnement en maak vervolgens een project.
5. Selecteer **nieuwe afstemming**. Voer vervolgens de volgende stappen uit:

   * Zoek en selecteer **alle abonnementen**.  
   * Selecteer **verbinding maken met bestaande abonnement**.  
     ![Verbinding maken met een](./media/custom-voice/custom-voice-connect-subscription.png)bestaand abonnement.
   * Voer uw abonnements sleutel voor uw Azure speech Services in en selecteer vervolgens **toevoegen**. Uw abonnements sleutels zijn beschikbaar in de portal voor het aanpassen van spraak op de [pagina abonnement](https://go.microsoft.com/fwlink/?linkid=2090458). U kunt de sleutels ook ophalen via het deel venster Resource beheer in de [Azure Portal](https://portal.azure.com/).
   * Als u meer dan één spraak Services-abonnement hebt dat u wilt gebruiken, herhaalt u deze stappen voor elk abonnement.

## <a name="customize-the-text-to-speech-output"></a>De tekst-naar-spraak-uitvoer aanpassen

Nu u accounts hebt gemaakt en uw abonnement hebt gekoppeld, kunt u beginnen met het afstemmen van de tekst-naar-spraak-uitvoer.

1. Kies een stem.
2. Voer de tekst in die u wilt bewerken.
3. Voordat u begint met bewerken, moet u de audio afspelen om de uitvoer te laten zien.
4. Selecteer het woord/de zin dat u wilt verfijnen en begin met experimenteren met verschillende SSML-functies.

>[!TIP]
> Zie voor gedetailleerde informatie over het aanpassen van SSML en het afstemmen van de spraak uitvoer de [SSML (Speech synthese Markup Language)](speech-synthesis-markup.md).

## <a name="limitations"></a>Beperkingen

Neural stem tuning wijkt enigszins af van de afstemming voor standaard-en aangepaste stemmen.

* Intonation wordt niet ondersteund voor Neural stemmen.
* De functies Pitch en volume werken alleen met volledige zinnen. Deze functies zijn niet beschikbaar op woord niveau.
* Sommige Neural stemmen kunnen worden afgestemd op basis van woorden, terwijl anderen u verplichten om hele zinnen te selecteren.

> [!TIP]
> Het hulp programma voor stem afstemming biedt contextuele informatie over functies en het afstemmen.

## <a name="next-steps"></a>Volgende stappen
* [Een spraak bron maken in azure](https://docs.microsoft.com/azure/cognitive-services/speech-service/get-started#create-a-speech-resource-in-azure)
* [Stem fijnafstelling starten](https://speech.microsoft.com/app.html#/VoiceTuning)
* [Markup Language voor spraak synthese (SSML)](speech-synthesis-markup.md)
