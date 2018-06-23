---
title: Overzicht van de aangepaste spraak Service in Azure | Microsoft Docs
description: De aangepaste spraak-Service is een cloudservice waarmee gebruikers spraak modellen voor spraak-naar-tekst schrijffouten aanpassen.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: article
ms.date: 02/07/2017
ms.author: panosper
ms.openlocfilehash: a6139283a555f8f97371c02f9d1f3d53dc6f15d3
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344914"
---
# <a name="what-is-custom-speech-service"></a>Wat is aangepast spraak Service?

Aangepaste spraak-Service is een cloudservice waarmee gebruikers de mogelijkheid om aan te passen voor spraak-naar-tekst schrijffouten spraak-modellen.
Voor het gebruik van de aangepaste spraak Service verwijzen naar de [aangepaste spraak serviceportal](https://cris.ai).

De aangepaste spraak Service kunt u aangepaste taal modellen en akoestisch modellen die zijn afgestemd op uw toepassing en uw gebruikers maken. Door specifieke spraak en/of gegevens naar de aangepaste spraak-Service geüpload, kunt u aangepaste modellen die kunnen worden gebruikt in combinatie met de bestaande de nieuwste spraak modellen van Microsoft.

Als u voice interactie aan een mobiele telefoon, tablet of PC-app toevoegt, kunt u bijvoorbeeld een aangepaste taalmodel dat kan worden gecombineerd met akoestisch model van Microsoft voor het maken van een spraak-naar-tekst-eindpunt dat speciaal is ontworpen voor uw app maken. Als uw toepassing is ontworpen voor gebruik in een bepaalde omgeving of door een bepaalde gebruikerspopulatie, kunt u ook maken en implementeren van een aangepaste akoestisch model met deze service.


## <a name="how-do-speech-recognition-systems-work"></a>Hoe werken speech recognition systemen?
Spraak erkenning systemen bestaan uit verschillende onderdelen die samenwerken. Twee van de belangrijkste onderdelen zijn het akoestisch model en het taalmodel.

Het akoestisch model is een classificatie korte fragmenten van audio labels in een van een aantal Fonemen of geluid eenheden, in een bepaalde taal. Bijvoorbeeld 'spraak' bestaat uit vier Fonemen 's p iy ch'. Deze classificaties worden gemaakt in de orde van grootte van 100 keer per seconde.

Het taalmodel is een waarschijnlijkheidsverdeling over reeksen woorden. Het taalmodel helpt het systeem een beslissing te nemen bij reeksen woorden die (bijna) hetzelfde klinken, op basis van de waarschijnlijkheid van de woordreeksen zelf. Zo klinken 'hij drinkt water' en 'hij dringt water' bijna hetzelfde, maar omdat de eerste vorm veel vaker zal voorkomen dan de tweede, krijgt deze een hogere score in het taalmodel.

De acoustic- en taalinstellingen modellen zijn statistische modellen van trainingsgegevens geleerd. Hierdoor kunnen werken ze het beste als de spraak optreden bij gebruik in toepassingen vergelijkbaar met de gegevens die tijdens de training waargenomen is. Acoustic- en taalinstellingen modellen in de engine voor het Microsoft spraak-naar-tekst op een enorme hoeveelheid tekst zijn getraind en biedt de nieuwste prestaties voor de meest voorkomende gebruiksscenario's, zoals de interactie met Cortana op de smartcard telefoon, tablet of PC, zoeken op het web door stem of SMS-berichten naar een vriend dicteren.

## <a name="why-use-the-custom-speech-service"></a>Waarom de aangepaste spraak-Service gebruiken?
De engine voor het Microsoft spraak-naar-tekst is hoogwaardige, is het gericht op de scenario's die hierboven worden beschreven. Als u verwacht voice-query's naar uw toepassing op bepaalde woordenlijst items bevatten, zoals productnamen of jargon die zelden in normale spraak, voorkomen dat is het echter waarschijnlijk dat kunt u betere prestaties door aan te passen van het taalmodel.

Als u bijvoorbeeld een app aan het bouwen bent om met gesproken opdrachten te zoeken in MSDN, komen termen als 'objectgeoriënteerd', 'naamruimte' of 'dot net' waarschijnlijk vaker voor dan in doorsnee spraaktoepassingen. Door het taalmodel aan te passen zorgt u ervoor dat het systeem dergelijke termen leert kennen.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het gebruik van de aangepaste spraak Service [aangepaste spraak Service Portal] (https://cris.ai).

* [Aan de slag](cognitive-services-custom-speech-get-started.md)
* [Veelgestelde vragen](cognitive-services-custom-speech-faq.md)
* [Woordenlijst](cognitive-services-custom-speech-glossary.md)
