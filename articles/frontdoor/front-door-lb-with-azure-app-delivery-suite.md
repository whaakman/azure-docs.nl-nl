---
title: Voordeur-Service, Azure - Load Balancing met van Azure application delivery suite | Microsoft Docs
description: Dit artikel helpt u meer informatie over hoe Azure raadt aan om de taakverdeling met de application delivery suite
services: frontdoor
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/10/2018
ms.author: sharadag
ms.openlocfilehash: a89043f814bc97aeb081789e92d9e4488712a465
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57439023"
---
# <a name="load-balancing-with-azures-application-delivery-suite"></a>Taakverdeling realiseren met de Azure-suite voor toepassingslevering

## <a name="introduction"></a>Inleiding
Microsoft Azure biedt verschillende globale en regionale services voor het beheren van hoe uw netwerkverkeer wordt verdeeld en gelijkmatig verdeeld: Traffic Manager, voordeur Service, Application Gateway en Load Balancer.  Samen met veel van de Azure-regio's en zonegebonden, architectuur, samen met behulp van deze services kunt u om robuuste, schaalbare krachtige toepassingen te bouwen.

![Application Delivery-Suite ][1]
 
Deze services worden onderverdeeld in twee categorieën:
1. **Globale taakverdeling services** zoals Traffic Manager en de voordeur distribueren van verkeer van uw eindgebruikers in uw regionale back-ends, voor clouds of zelfs uw hybride on-premises services. Globale taakverdeling uw verkeer routeert naar de dichtstbijzijnde back-end service en reageert op wijzigingen in de betrouwbaarheid van de service of de prestaties te onderhouden altijd beschikbaar en maximale prestaties voor uw gebruikers. 
2. **Regionale load balancing-services** zoals standaardversie van Load Balancer of Application Gateway bieden de mogelijkheid om te verdelen van verkeer in virtuele netwerken (VNETs) over uw virtuele machines (VM's) of de zonegebonden service-eindpunten in een regio.

Globale en regionale services in uw toepassing combineren biedt een end-to-end betrouwbare, goed presterende en veilige manier om verkeer te routeren naar en van uw gebruikers aan uw IaaS, PaaS, of on-premises services. In de volgende sectie wordt beschreven elk van deze services.

## <a name="global-load-balancing"></a>Globale taakverdeling
**Traffic Manager** biedt globale DNS wordt de taakverdeling. Deze kijkt naar binnenkomende aanvragen voor DNS- en reageert met een goede back-end, in overeenstemming met het beleid voor het routeren dat de klant is geselecteerd. Opties voor methoden zijn:
- Prestaties van routering voor het verzenden van de aanvrager naar de dichtstbijzijnde back-end in termen van latentie.
- Prioriteit routering om al het verkeer naar een back-end, met andere back-ends als back up.
- Gewogen round robin routering, die verdeelt het verkeer op basis van het gewicht dat is toegewezen aan elke back-end.
- De geografische routering om ervoor te zorgen dat aanvragers in specifieke geografische regio's worden omgeleid naar de back-ends toegewezen aan deze regio's (bijvoorbeeld alle aanvragen van Spanje moeten worden omgeleid naar de Frankrijk centraal Azure-regio)
- Back-ends subnet routering waarmee u IP-adres toewijzen bereiken zodat aanvragen die afkomstig zijn van de referenties die worden verzonden naar de opgegeven back-end (bijvoorbeeld alle gebruikers die verbinding maakt vanaf uw hoofdkantoor zakelijke IP-adresbereik moeten krijgen verschillende webinhoud dan de algemene gebruikers)

De client maakt rechtstreeks verbinding met deze back-end. Met Azure Traffic Manager detecteert wanneer een back-end niet in orde is en vervolgens de clients omgeleid naar een ander exemplaar van de in orde. Raadpleeg [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md) documentatie voor meer informatie over de service.

**Azure voordeur Service** dynamische website acceleration (DSA) met inbegrip van algemene HTTP-taakverdeling biedt.  Hier wordt gekeken binnenkomende HTTP-aanvragen routes naar de dichtstbijzijnde service back-end / de regio voor de opgegeven hostnaam, de URL-pad en de geconfigureerde regels.  
Voordeur HTTP-aanvragen aan de rand van het netwerk van Microsoft wordt beëindigd en actief voor het detecteren van de App of infrastructuur status of latentie wijzigingen tests.  Voordeur vervolgens routeert altijd verkeer naar de snelste en beschikbaar is (in orde) back-end. Raadpleeg de voordeur [routering architectuur](front-door-routing-architecture.md) details en [verkeersrouteringsmethoden](front-door-routing-methods.md) voor meer informatie over de service.

## <a name="regional-load-balancing"></a>Regionale taakverdeling
Application Gateway biedt application delivery controller (ADC) als een service biedt verschillende Layer 7 load balancing mogelijkheden voor uw toepassing. Hiermee kunnen klanten de productiviteit van webfarms optimaliseren door het offloaden van CPU-intensieve SSL-beëindiging aan de toepassingsgateway. Andere Routeringsmogelijkheden voor laag 7 bevatten round-robin-distributie van inkomend verkeer, cookies gebaseerde sessieaffiniteit, URL-pad gebaseerde routering en de mogelijkheid voor het hosten van meerdere websites achter één application gateway. Application Gateway kan worden geconfigureerd als een Internet gerichte gateway, een gateway alleen interne of een combinatie van beide. Application Gateway wordt volledig door Azure beheerde, schaalbare en maximaal beschikbaar. Het biedt een uitgebreide verzameling diagnostische gegevens en functies voor logboekregistratie voor betere beheersbaarheid.
Load Balancer is een integraal onderdeel van de Azure SDN-stack, leveren van hoge prestaties en lage latentie Layer 4 load balancing services voor alle UDP en TCP-protocollen. Het beheert binnenkomende en uitgaande verbindingen. U kunt openbare en interne taakverdeling eindpunten configureren en definieer regels voor binnenkomende verbindingen naar back-end-pool bestemmingen toewijzen met behulp van TCP- en HTTP-statustesten opties voor het beheren van de beschikbaarheid van de service.


## <a name="choosing-a-global-load-balancer"></a>Een globale load balancer kiezen
Bij het kiezen van een globale load balancer tussen Traffic Manager en Azure voordeur voor globale routering, moet u rekening houden met wat is vergelijkbaar en wat is er anders over de twee services.   Beide services bieden
- **Multi-geografische redundantie:** Als een regio uitvalt, routeert verkeer naadloos naar de dichtstbijzijnde regio zonder tussenkomst van de eigenaar van de toepassing.
- **Dichtstbijzijnde regio routering:** Verkeer wordt automatisch omgeleid naar de dichtstbijzijnde regio

</br>De volgende tabel beschrijft de verschillen tussen Traffic Manager en Azure voordeur Service:</br>

| Traffic Manager | Azure Front Door Service |
| --------------- | ------------------------ |
|**Elk protocol voor:** Omdat het Traffic Manager werkt op de DNS-laag, kunt u elk type netwerkverkeer; doorsturen HTTP-, TCP, UDP, enzovoort. | **HTTP-versnelling:** Met de voordeur wordt verkeer via proxy op de rand van het netwerk van Microsoft.  Als gevolg hiervan Zie HTTP (S) aanvragen latentie en doorvoer verbeteringen korte wachttijden voor SSL-onderhandeling en het gebruik van ' hot ' verbindingen van AFD naar uw toepassing.|
|**On-premises routering:** Met routering op het niveau van een DNS-verkeer altijd wordt gerouteerd van punt naar punt.  Routering van het filiaal naar uw on-premises datacenter kan duren voordat een rechtstreeks pad. zelfs op uw eigen netwerk met Traffic Manager. | **Onafhankelijke schaalbaarheid:** Omdat de voordeur werkt met de HTTP-aanvraag, aanvragen aan verschillende URL-paden kunnen worden gerouteerd naar verschillende back-end / regionale groepen (microservices) op basis van regels en de status van elke toepassing microservice-service.|
|**Facturering-indeling:** Facturering op basis van DNS kan worden geschaald met uw gebruikers en services met meer gebruikers, om te verminderen van kosten op een hoger gebruik plateaus. |**Inline-beveiliging:** Voordeur kunnen regels, zoals gelden enkele beperkingen en IP-ACL-ing beveiligen van uw back-ends voordat verkeer bereikt uw toepassing. 

</br>Vanwege de prestaties ervan en de voordelen van beveiliging op http-workloads met voordeur, wordt het aangeraden klanten voordeur gebruiken voor de HTTP-workloads.    Traffic Manager en voordeur kunnen parallel worden gebruikt voor het bieden van al het verkeer voor uw toepassing. 

## <a name="building-with-azures-application-delivery-suite"></a>Bouwen met Azure toepassing levering suite 
We raden aan dat alle websites,-API's,-services geografisch redundant worden en verkeer voor de gebruikers van de dichtstbijzijnde (laagste latentie) leveren locatie aan waar mogelijk.  Services van Traffic Manager, voordeur Service Application-Gateway en Load Balancer combineren, kunt u bouwen zonally en geografisch redundante betrouwbaarheid, schaalbaarheid en prestaties te maximaliseren.

In het volgende diagram wordt een voorbeeld van de service die een combinatie van al deze services gebruikt voor het maken van een globale webservice beschreven.   In dit geval de architect Traffic Manager is gebruikt om te routeren algemene back-ends voor de levering van bestands- en object tijdens het gebruik van voordeur wereldwijd routeren URL-paden die overeenkomen met het patroon/store / * naar de service hebben ze gemigreerd naar App Service bij alle andere routering aanvragen voor regionale Toepassingsgateways.

In de regio, voor hun service IaaS ontwikkelaar van de toepassing heeft besloten dat URL's die overeenkomen met het patroon/afbeeldingen / * worden aangeleverd vanuit een specifieke pool van virtuele machines die verschillen van de rest van de webfarm.

Daarnaast moet de standaardgroep van de virtuele machine de dynamische inhoud om te communiceren met een back-end-database die wordt gehost op een cluster met hoge beschikbaarheid. De volledige implementatie is ingesteld via Azure Resource Manager.

Het volgende diagram toont de architectuur van dit scenario:

![Gedetailleerde architectuur van Application Delivery-Suite][2] 

> [!NOTE]
> In dit voorbeeld is slechts één van de vele mogelijke configuraties van de load balancing-services die Azure biedt. Traffic Manager, voordeur, Application Gateway en Load Balancer kunnen worden gecombineerd en afgestemd op zelf de behoeften van uw load balancing. Bijvoorbeeld, als SSL-offload of Layer 7-verwerking niet nodig is, kan Load Balancer kan worden gebruikt in plaats van Application Gateway.


## <a name="next-steps"></a>Volgende stappen

- Lees hoe u [een Front Door maakt](quickstart-create-front-door.md).
- Lees [hoe Front Door werkt](front-door-routing-architecture.md).

<!--Image references-->
[1]: ./media/front-door-lb-with-azure-app-delivery-suite/application-delivery-figure1.png
[2]: ./media/front-door-lb-with-azure-app-delivery-suite/application-delivery-figure2.png
