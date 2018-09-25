---
title: Azure voordeur Service - architectuur van Routering | Microsoft Docs
description: In dit artikel krijgt u inzicht in het algemeen overzicht aspect van de architectuur van de voordeur.
services: front-door
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/10/2018
ms.author: sharadag
ms.openlocfilehash: 42a9c4f9983a68459351fc98fb8b757759e585bf
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46964876"
---
# <a name="routing-architecture-overview"></a>Overzicht van de architectuur van Routering

De voordeur Azure Service wanneer deze de client ontvangt om vraagt vervolgens een van beide antwoorden deze (indien de cache is ingeschakeld) of doorstuurt ze op de desbetreffende toepassing back-end (als een omgekeerde proxy).

</br>Er zijn mogelijkheden voor het optimaliseren van het verkeer wanneer de routering naar Azure voordeur evenals bij routering naar back-ends.

## <a name = "anycast"></a>De voordeur-omgeving voor het verkeer (Anycast) selecteren

Routering aan de voordeur van Azure omgevingen maakt gebruik van [Anycast](https://en.wikipedia.org/wiki/Anycast) voor zowel DNS (Domain Name System) en HTTP (Hypertext Transfer Protocol)-verkeer, zodat gebruikersverkeer wordt omgeleid naar de dichtstbijzijnde omgeving in termen van de netwerktopologie (minste hops). Deze architectuur biedt doorgaans beter traject keer voor eindgebruikers (maximaliseren van de voordelen van de gesplitste TCP). Voordeur organiseert de omgevingen in de primaire en alternatieve "ringen'.  De buitenste ring heeft omgevingen die zich dichter bij gebruikers, kortere wachttijden bieden.  De binnenste ring heeft omgevingen waarin de failover voor de buitenste ring-omgeving kunnen worden verwerkt als een probleem treedt op. De buitenste ring is het voorkeursdoel voor al het verkeer, maar de binnenste ring is nodig om te handelen verkeer overloop van de buitenste ring. In termen van VIP's (virtuele IP-adressen), elke front-host of -domein geleverd door de voordeur, is toegewezen een primaire VIP, in omgevingen in de binnenste en buitenste ring is aangekondigd, evenals een alternatieve VIP die alleen door omgevingen wordt aangekondigd in de binnenste ring. 

</br>Deze strategie voor algehele zorgt ervoor dat aanvragen van uw gebruikers altijd de dichtstbijzijnde voordeur omgeving bereiken en dat, zelfs als de gewenste voordeur-omgeving niet in orde is wordt verkeer automatisch naar de volgende dichtstbijzijnde omgeving verplaatst.

## <a name = "splittcp"></a>Verbinding maken met de voordeur omgeving (splitsen TCP)

[Gesplitste TCP](https://en.wikipedia.org/wiki/Performance-enhancing_proxy) is een techniek voor latentie en TCP-problemen verminderen door het verbreken van een verbinding die een hoge retourtijd rekening worden gebracht in kleinere delen.  Een TCP-verbinding met een grote retourtijd (RTT) naar back-end van toepassing is door de voordeur omgevingen dichter bij te stellen aan eindgebruikers plaatsen en het beëindigen van TCP-verbindingen maakt binnen de omgeving voordeur, opgesplitst in twee TCP-verbindingen. De korte verbinding tussen de eindgebruiker en de omgeving voordeur betekent dat de verbinding tot stand wordt meer dan drie korte retouren in plaats van drie lang retouren, latentie opslaat.  De long verbinding tussen de voordeur-omgeving en de back-end kan worden vooraf tot stand gebracht en hergebruikt voor meerdere aanroepen van de eindgebruiker, het opslaan van de TCP-verbindingstijd.  Het effect wordt vermenigvuldigd bij het maken van een verbinding SSL/TLS (Transport Layer Security) als er meer retouren voor het beveiligen van de verbinding.

## <a name="processing-request-to-match-a-routing-rule"></a>Verwerken van de aanvraag overeenkomt met een regel voor doorsturen
Nadat u hebt een verbinding tot stand brengen en uitvoeren van een SSL is-handshake wordt als een aanvraag terechtkomt op een omgeving met voordeur die overeenkomt met een regel voor doorsturen de eerste stap. Deze overeenkomst in feite is het vaststellen van uit alle configuraties voor de deur, zodat deze overeenkomen met de aanvraag voor die bepaalde routering regel. Meer informatie over hoe gaat de voordeur [route die overeenkomt met](front-door-route-matching.md) voor meer informatie.

## <a name="identifying-available-backends-in-the-backend-pool-for-the-routing-rule"></a>Identificeren van beschikbare back-ends via de back-endpool voor de regel voor doorsturen
Nadat de voordeur heeft een overeenkomende tekenreeks is voor een regel voor doorsturen op basis van de binnenkomende aanvraag en als er geen caching, klikt u vervolgens is de volgende stap om op te halen van de integriteitsstatus test voor de back-endpool die zijn gekoppeld aan de overeenkomende route. Meer informatie over hoe voordeur back-end met behulp van de gezondheid van bewaakt [statuscontroles](front-door-health-probes.md) voor meer informatie.

## <a name="forwarding-the-request-to-your-application-backend"></a>De aanvraag naar de back-end van uw toepassing
Ten slotte, ervan uitgaande dat er is geen caching geconfigureerd, de aanvraag van de gebruiker wordt doorgestuurd naar het 'beste' back-end op basis van uw [voordeur routeringsmethode](front-door-routing-methods.md) configuratie.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [maken van een voordeur](quickstart-create-front-door.md).
