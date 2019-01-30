---
title: Aangepaste Speech Service-overzicht in Azure | Microsoft Docs
description: Custom Speech Service is een cloudgebaseerde service waarmee gebruikers spraakmodellen kunnen aanpassen voor spraak-naar-teksttranscriptie.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.subservice: custom-speech
ms.topic: article
ms.date: 02/07/2017
ms.author: panosper
ms.openlocfilehash: 00c9a89bea9deafe4096566032cc9685df8c111c
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55227995"
---
# <a name="what-is-custom-speech-service"></a>Wat is Custom Speech Service?

[!INCLUDE [Deprecation note](../../../includes/cognitive-services-custom-speech-deprecation-note.md)]

Custom Speech Service is een cloudgebaseerde service waarmee gebruikers spraakmodellen kunnen aanpassen voor spraak-naar-teksttranscriptie.
Als u met Custom Speech Service aan de slag wilt, raadpleegt u de [Custom Speech Service-portal](https://cris.ai).

Met Custom Speech Service kunt u aangepaste taalmodellen en akoestische modellen maken op basis van uw toepassing en uw gebruikers. Door uw specifieke spraak- en/of tekstgegevens te uploaden naar Custom Speech Service, kunt u aangepaste modellen maken die u vervolgens in combinatie met de bestaande moderne spraakmodellen van Microsoft kunt gebruiken.

Als u bijvoorbeeld spraakinteractie toevoegt aan een toepassing op een mobiele telefoon, tablet of pc, kunt u een aangepast taalmodel maken dat u vervolgens combineert met het akoestische model van Microsoft. Zo ontstaat er een spraak-naar-teksteindpunt dat speciaal dient voor uw app. Als uw toepassing is bedoeld voor gebruik in een speciale omgeving of voor een speciale groep gebruikers, kunt u ook een aangepast akoestisch model maken en dit implementeren in de service.


## <a name="how-do-speech-recognition-systems-work"></a>Hoe werken spraakherkenningssystemen?
Spraakherkenningssystemen bestaan uit verschillende onderdelen die samenwerken. Twee van de belangrijkste onderdelen zijn het akoestische model en het taalmodel.

Het akoestische model is een classificatie waarbij in een bepaalde taal korte audiofragmenten worden gekwalificeerd als een van verschillende fonemen, of klankeenheden. Het woord 'spraak' bestaat bijvoorbeeld uit vijf fonemen: 's p r aa k'. Deze classificaties worden gemaakt in de orde van grootte van 100 keer per seconde.

Het taalmodel is een waarschijnlijkheidsverdeling over reeksen woorden. Het taalmodel helpt het systeem een beslissing te nemen bij reeksen woorden die (bijna) hetzelfde klinken, op basis van de waarschijnlijkheid van de woordreeksen zelf. Zo klinken 'hij drinkt water' en 'hij dringt water' bijna hetzelfde, maar omdat de eerste vorm veel vaker zal voorkomen dan de tweede, krijgt deze een hogere score in het taalmodel.

Zowel het akoestische model als het taalmodel zijn statische modellen gemaakt op basis van trainingsgegevens. Ze werken dan ook het beste als de taal die wordt gebruikt in de toepassingen overeenkomt met de gegevens die tijdens de training zijn gebruikt. De akoestische modellen en taalmodellen in de Microsoft Speech-To-Text-engine zijn getraind op basis van een enorme verzameling spraak en tekst en bieden uitstekende prestaties in de meest voorkomende gebruiksscenario's - bijvoorbeeld wanneer u met Cortana wilt communiceren via uw smartphone, tablet of pc, wanneer u met spraak op internet wilt zoeken of wanneer u sms-berichten voor een vriend wilt inspreken.

## <a name="why-use-the-custom-speech-service"></a>Waarom de Custom Speech Service gebruiken?
Hoewel de Speech-To-Text-engine van Microsoft van wereldklasse is, is deze vooral bedoeld voor de hierboven beschreven scenario's. Als u echter verwacht dat gesproken query's voor uw toepassing bepaalde items bevatten zoals productnamen of vaktermen die in gewone gesproken taal nauwelijks voorkomen, kunt u de prestaties in veel gevallen verbeteren door het taalmodel aan te passen.

Als u bijvoorbeeld een app aan het bouwen bent om met gesproken opdrachten te zoeken in MSDN, komen termen als 'objectgeoriënteerd', 'naamruimte' of 'dot net' waarschijnlijk vaker voor dan in doorsnee spraaktoepassingen. Door het taalmodel aan te passen zorgt u ervoor dat het systeem dergelijke termen leert kennen.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het gebruik van de Custom Speech Service, de [Custom Speech Service Portal](https://cris.ai).

* [Aan de slag](cognitive-services-custom-speech-get-started.md)
* [Veelgestelde vragen](cognitive-services-custom-speech-faq.md)
* [Woordenlijst](cognitive-services-custom-speech-glossary.md)
