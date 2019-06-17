---
title: Bekrachtigingen - Personalizer
titleSuffix: Azure Cognitive Services
description: 'High-performance en drukbezochte websites en toepassingen hebt u twee hoofdfactoren te houden met Personalizer voor schaalbaarheid en prestaties: latentie en doorvoer van de training.'
services: cognitive-services
author: edjez
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: overview
ms.date: 06/07/2019
ms.author: edjez
ms.openlocfilehash: 242cc1b96c08cd79dc3e2ef5efbbe96a934b8ad3
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67058439"
---
# <a name="scalability-and-performance"></a>Schaalbaarheid en prestaties

High-performance en drukbezochte websites en toepassingen hebt twee hoofdfactoren te houden met Personalizer voor schaalbaarheid en prestaties:

* Lage latentie te houden bij het maken van positie-API-aanroepen
* Zorg dat training doorvoer kan voldoen aan de invoer van de gebeurtenis

Persoonlijke instellingen kan zeer snel een positie retourneren met de meeste van de duur van toegewezen aan communicatie via de REST-API. Azure wordt automatisch geschaald met de mogelijkheid snel reageren op aanvragen.

##  <a name="low-latency-scenarios"></a>Scenario's met lage latentie

Sommige toepassingen vereisen lage latenties bij het retourneren van een positie. Dit is nodig:

* Beoordeeld om te voorkomen dat de gebruiker nog een merkbare hoeveelheid tijd voordat het weergeven van inhoud.
* Om een server waarop extreme verkeer voordoet zich te vermijden schaarse compute tijd en de netwerkverbindingen gebruikt.

<!--

If your web site is scaled on your infrastructure, you can avoid making HTTP calls by hosting the Personalizer API in your own servers running a Docker container.

This change would be transparent to your application, other than using an endpoint URL referring to the running docker instances as opposed to an online service in the cloud.



### Extreme Low Latency Scenarios

If you require latencies under a millisecond, and have already tested using Personalizer via containers, please contact our support team so we can assess your scenario and provide guidance suited to your needs.

-->

## <a name="scalability-and-training-throughput"></a>Schaalbaarheid en training doorvoer

Personalizer werkt door het bijwerken van een model dat retrained is gebaseerd op berichten die asynchroon door Personalizer zijn verzonden nadat de positie en API's van derden. Deze berichten worden verzonden met behulp van een Azure Event hub voor de toepassing.

 Is het onwaarschijnlijk dat de meeste toepassingen zal contact met u de maximale toevoegen en de doorvoer van Personalizer training. Hoewel dit maximum is bereikt niet de toepassing vertraagt, zou dit impliceren Event Hub wachtrijen ophalen opgevuld intern sneller dan ze kunnen worden opgeschoond.

## <a name="how-to-estimate-your-throughput-requirements"></a>Hoe maak een schatting van uw vereisten voor doorvoer

* Maak een schatting van het gemiddelde aantal bytes per rangorde gebeurtenis toe te voegen de lengte van de context en de actie JSON-documenten.
* 20MB per seconde door deze geschatte gemiddelde aantal bytes dat delen.

Bijvoorbeeld, als uw gemiddelde nettolading 500 functies heeft en elk een geschatte 20 tekens is, klikt u vervolgens elke gebeurtenis is ongeveer 10kb. Met deze schatting 20.000.000 / 10.000 = van 2000 gebeurtenissen per seconde, die ongeveer 173 miljoen gebeurtenissen per dag is. 

Als u deze limieten zijn bereikt, neem contact met ons ondersteuningsteam voor de architectuur van advies.

## <a name="next-steps"></a>Volgende stappen

[Maken en configureren van Personalizer](how-to-settings.md).