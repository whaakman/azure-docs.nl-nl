---
title: Veelgestelde vragen over directe regel spraak
titleSuffix: Azure Cognitive Services
description: Vind antwoorden op de meest populaire vragen over virtuele voice-first-assistenten met behulp van het kanaal directe regel spraak.
services: cognitive-services
author: trrwilson
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: travisw
ms.openlocfilehash: 16e4bcbb1514cfd5bbddc22b663d636292095231
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/02/2019
ms.locfileid: "65025894"
---
# <a name="voice-first-virtual-assistants-preview-frequently-asked-questions"></a>Stem op de eerste virtuele assistenten Preview-versie: Veelgestelde vragen

Als u antwoorden op uw vragen niet in dit document vinden, bekijk dan [andere ondersteuningsopties](support.md).

## <a name="general"></a>Algemeen

**V: Wat is een directe regel spraak?**

**A:** De `SpeechBotConnector` van de spraak-SDK biedt in twee richtingen, asynchrone communicatie met bots die zijn verbonden met het kanaal directe regel spraak van Bot Framework. Dit kanaal biedt gecoördineerde toegang tot de spraak-naar-tekst en spraak vanuit Azure Speech Services waarmee bots om te worden volledig spraak in, stem uit eigen ervaring. Met de ondersteuning voor wake-woorden en Wake Word verificatie maakt deze oplossing het mogelijk om te hoge mate aanpasbare stem op de eerste virtuele assistenten voor uw merk of product bouwen.

**V: Hoe ga ik aan de slag?**

**A:** De beste manier begint met het maken van een stem op de eerste virtuele assistent is te beginnen met [het maken van een eenvoudige Bot Framework-bot](https://docs.microsoft.com/azure/bot-service/bot-builder-tutorial-basic-deploy?view=azure-bot-service-4.0). Koppel vervolgens uw bot de [channel voor directe regel spraak](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-directlinespeech.md).

## <a name="debugging"></a>Foutopsporing

**V: Ik ontvang een 401-fout bij het verbinden en niets werkt. Ik weet dat mijn abonnement spraak sleutel geldig is. Wat gebeurt er?**

**A:** Preview-versie is directe regel spraak zeer specifieke beperkingen op het abonnement dat is gebruikt. Zorg ervoor dat u de **spraak** resource (Microsoft.CognitiveServicesSpeechServices, 'Spraak') en *niet* de **Cognitive Services** resource ( Microsoft.CognitiveServicesAllInOne, 'Alle Cognitive Services'). Bovendien, houd er rekening mee dat alleen de **westus2** regio wordt momenteel ondersteund.

![abonnement voor rechtstreekse regel spraak corrigeren](media/voice-first-virtual-assistants/faq-supported-subscription.png "voorbeeld van een compatibel spraak-abonnement")

**V: Ik erkenning tekst teruggaan van rechtstreekse regel spraak, maar ik een foutbericht "1011" en niets in mijn bot zien. Waarom is dat?**

**A:** Deze fout wijst op een communicatieprobleem met de tussen de bot en directe regel spraak. Zorg ervoor dat u hebt [verbonden het kanaal directe regel spraak](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-directlinespeech.md), [toegevoegde ondersteuning voor Streaming-protocol](https://aka.ms/botframework/addstreamingprotocolsupport) aan uw bot (met de gerelateerde Web Socket-ondersteuning) en vervolgens controleren of uw bot reageert op binnenkomende aanvragen van het kanaal.

**V: Dit ook niet werkt en/of krijg ik een andere fout bij het gebruik van een SpeechBotConnector en het niet duidelijk is wat ik moet doen. Wat *moet* moet ik doen?**

**A:** Logboekregistratie op basis van bestanden vindt u aanzienlijk meer details over en kan helpen versnellen ondersteuningsaanvragen. U kunt deze inschakelen, Zie [over het gebruik van logboekregistratie](how-to-use-logging.md).

## <a name="next-steps"></a>Volgende stappen

* [Problemen oplossen](troubleshooting.md)
* [Releaseopmerkingen](releasenotes.md)
