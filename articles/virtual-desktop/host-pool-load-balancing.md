---
title: Windows virtuele bureaublad Preview host pool taakverdelingsmethoden - Azure
description: Host groep load balancing-methoden voor een Windows Virtual Desktop Preview-omgeving.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 03/21/2019
ms.author: helohr
ms.openlocfilehash: 8b18224339654c067d8ab9b543fa49a9c7d55ddd
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/25/2019
ms.locfileid: "58400191"
---
# <a name="host-pool-load-balancing-methods"></a>Taakverdelingsmethoden voor hostpool

Windows virtuele bureaublad Preview ondersteunt twee methoden voor taakverdeling. Elke methode bepaalt welke sessiehost fungeert als host voor een gebruikerssessie wanneer ze verbinding met een resource in een pool van de host maken.

De volgende methoden voor taakverdeling zijn beschikbaar in virtuele Windows-bureaublad:

- Breedte op de eerste load balancing, kunt u gebruikerssessies gelijkmatig wordt verdeeld over de sessie-hosts in een groep host.
- Diepte van de eerste load balancing, kunt u een sessiehost verzadigen met sessies van gebruiker in een pool van de host. Zodra de eerste sessie de maximale sessie drempelwaarde bereikt, stuurt de load balancer een nieuwe gebruikersverbindingen met de host van de volgende sessie in de groep host totdat de limiet, enzovoort wordt bereikt.

Elke groep host kunt slechts één type load balancing configureren specifieke. Echter, beide methoden load balancing share hosten van de volgende gedragingen, ongeacht welke groep waarin ze zijn opgeslagen:

- Als een gebruiker al een sessie in de groep host heeft en opnieuw verbinding met die sessie maken wordt, wordt de load balancer is hen omleidt naar de host van de sessie met hun bestaande sessie. Dit gedrag is van toepassing, zelfs als die sessiehost AllowNewConnections eigenschap is ingesteld op False.
- Als een gebruiker een sessie nog niet over in de groep host, wordt niet de load balancer sessie hosts waarvan AllowNewConnections de eigenschap is ingesteld op ONWAAR tijdens taakverdeling overwegen.

## <a name="breadth-first-load-balancing-method"></a>Breedte op de eerste load balancing-methode

De breedte op de eerste load balancing-methode kunt u verbindingen van de gebruiker om te optimaliseren voor dit scenario te distribueren. Deze methode is ideaal voor organisaties die willen bieden de beste ervaring voor gebruikers verbinding maken met hun gegroepeerde virtuele bureaublad omgeving.

De breedte op de eerste methode eerst een query sessie hosts die nieuwe verbindingen toestaan. De methode vervolgens selecteert u de host van de sessie met de minste aantal sessies. Als er een tie, selecteert de methode u de host van de eerste sessie in de query.

## <a name="depth-first-load-balancing-method"></a>Diepte van de eerste load balancing-methode

De diepte van de eerste load balancing-methode kunt u één sessiehost verzadigen tegelijk te optimaliseren voor dit scenario. Deze methode is ideaal voor prijsbewuste-organisaties die willen meer nauwkeurig beheer mogelijk van het aantal virtuele machines die ze voor een groep host hebt toegewezen.

De diepte van de eerste methode eerst een query sessie hosts die nieuwe verbindingen toestaan en van boven de limiet van hun maximale sessie nog niet hebt voltooid. De methode selecteert vervolgens de host van de sessie met de hoogste aantal sessies. Als er een tie, selecteert de methode u de host van de eerste sessie in de query.