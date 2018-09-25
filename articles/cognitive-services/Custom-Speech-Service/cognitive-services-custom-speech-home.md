---
title: Aangepaste Speech Service-overzicht in Azure | Microsoft Docs
description: De Custom Speech Service is een cloud-gebaseerde service waarmee gebruikers om aan te passen spraakmodellen voor transcriptie van spraak-naar-tekst.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: article
ms.date: 02/07/2017
ms.author: panosper
ROBOTS: NOINDEX
ms.openlocfilehash: da88989753069f7ba8ca2c2e2806a648f3df4e3c
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46948363"
---
# <a name="what-is-custom-speech-service"></a>Wat is Custom Speech Service?

Met Custom Speech Service is een cloud-gebaseerde service waarmee gebruikers de mogelijkheid om aan te passen spraakmodellen voor transcriptie van spraak-naar-tekst.
Voor het gebruik van de Custom Speech Service, raadpleegt u de [Custom Speech Service Portal](https://cris.ai).

De Custom Speech Service kunt u aangepaste taalmodellen en akoestische modellen die zijn afgestemd op uw toepassing en uw gebruikers maken. Door uw specifieke spraak en/of gegevens uploaden naar de Custom Speech Service, kunt u aangepaste modellen die kunnen worden gebruikt in combinatie met bestaande-status-of-the-art spraakmodellen van Microsoft.

Bijvoorbeeld, als u spraak interactie aan een mobiele telefoon, tablet of PC app toevoegt, kunt u een aangepast taalmodel die kan worden gecombineerd met akoestisch model van Microsoft om een eindpunt spraak-naar-tekst is speciaal ontworpen voor uw app te maken. Als uw toepassing is ontworpen voor gebruik in een specifieke omgeving of door een bepaalde gebruikerspopulatie, kunt u ook maken en implementeren van een aangepast akoestisch model met deze service.


## <a name="how-do-speech-recognition-systems-work"></a>Hoe werken spraakherkenning systemen?
Spraakherkenning systemen bestaan uit verschillende onderdelen die samenwerken. Twee van de belangrijkste onderdelen zijn het akoestisch model en het taalmodel.

Het akoestisch model is een classificatie waarbij in één van een aantal Fonemen, of klankeenheden in een bepaalde taal korte fragmenten van audio van labels. Het woord 'spraak' bijvoorbeeld bestaat Fonemen 's p iy AA k'. Deze classificaties worden gemaakt in de orde van grootte van 100 keer per seconde.

Het taalmodel is een waarschijnlijkheidsverdeling over reeksen woorden. Het taalmodel helpt het systeem een beslissing te nemen bij reeksen woorden die (bijna) hetzelfde klinken, op basis van de waarschijnlijkheid van de woordreeksen zelf. Zo klinken 'hij drinkt water' en 'hij dringt water' bijna hetzelfde, maar omdat de eerste vorm veel vaker zal voorkomen dan de tweede, krijgt deze een hogere score in het taalmodel.

Zowel de akoestische en modellen zijn statistische modellen geleerd van trainingsgegevens. Als gevolg hiervan uitvoeren ze beste wanneer de gesproken tekst optreden bij gebruik in toepassingen vergelijkbaar met de gegevens die tijdens de training in acht genomen is. De akoestische en modellen in de engine voor Microsoft Speech To Text zijn getraind op een enorme verzameling van spraak en tekst en status-of-the-art-prestaties voor de meest voorkomende gebruiksscenario's, zoals de interactie met Cortana op uw smartcard leveren telefoon, tablet of PC, zoeken op het web met uw stem of SMS-berichten naar een vriend dicteren.

## <a name="why-use-the-custom-speech-service"></a>Waarom de Custom Speech Service gebruiken?
De engine voor Microsoft Speech To Text wereldklasse is, is het gericht op de scenario's die hierboven worden beschreven. Als u verwacht dat gesproken query's voor uw toepassing bepaalde items, zoals productnamen of vaktermen die zelden in normale spraak, bevat is het echter waarschijnlijk dat u betere prestaties verkrijgen kunt door het taalmodel aan te passen.

Als u bijvoorbeeld een app aan het bouwen bent om met gesproken opdrachten te zoeken in MSDN, komen termen als 'objectgeoriënteerd', 'naamruimte' of 'dot net' waarschijnlijk vaker voor dan in doorsnee spraaktoepassingen. Door het taalmodel aan te passen zorgt u ervoor dat het systeem dergelijke termen leert kennen.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het gebruik van de Custom Speech Service, de [Custom Speech Service Portal] (https://cris.ai).

* [Aan de slag](cognitive-services-custom-speech-get-started.md)
* [Veelgestelde vragen](cognitive-services-custom-speech-faq.md)
* [Woordenlijst](cognitive-services-custom-speech-glossary.md)
