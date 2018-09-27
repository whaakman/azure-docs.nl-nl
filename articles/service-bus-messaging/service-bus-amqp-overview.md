---
title: Overzicht van AMQP 1.0 in Azure Servicebus | Microsoft Docs
description: Meer informatie over het gebruik van het Advanced Message Queuing Protocol (AMQP 1.0) in Azure.
services: service-bus-messaging
documentationcenter: .net
author: spelluru
manager: timlt
editor: ''
ms.assetid: 0e8d19cc-de36-478e-84ae-e089bbc2d515
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 09/26/2018
ms.author: spelluru
ms.openlocfilehash: b43cdfa0b5f9e5bf6a94f4f59034e07f59ddb163
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47393329"
---
# <a name="amqp-10-support-in-service-bus"></a>Ondersteuning van AMQP 1.0 in Service Bus
De cloudservice van Azure Service Bus- en on-premises [Service Bus voor Windows Server (Service Bus 1.1)](https://msdn.microsoft.com/library/dn282144.aspx) ondersteuning voor het Advanced Message Queueing Protocol (AMQP 1.0). AMQP kunt u platformonafhankelijke, hybride toepassingen met een open standaard protocol bouwen. Toepassingen die gebruikmaken van onderdelen die zijn gebouwd met behulp van verschillende talen en frameworks en die op verschillende besturingssystemen worden uitgevoerd, kunt u maken. Al deze onderdelen verbinding met Service Bus en naadloos maken kunnen uitwisselen gestructureerde business-berichten efficiënt en vol vertrouwen.

## <a name="introduction-what-is-amqp-10-and-why-is-it-important"></a>Inleiding: Wat is AMQP 1.0- en waarom is het belangrijk?
Bericht-georiënteerde middlewareproducten hebben de eigen protocollen van traditioneel gebruikt voor communicatie tussen client-toepassingen en brokers. Dit betekent dat wanneer u een bepaalde leverancier berichtbroker hebt geselecteerd, u de bibliotheken van de leverancier gebruiken moet om uw clienttoepassingen die broker verbinding te maken. Dit resulteert in een bepaalde mate van afhankelijkheid van de leverancier, omdat wijzigingen in de code in de verbonden toepassingen overzetten van een toepassing naar een ander product vereist. 

Bovendien is het lastig berichtenbrokers verbinding te maken van verschillende leveranciers. Hiervoor wordt meestal op toepassingsniveau bridging verplaatsen van berichten van het ene systeem naar een andere en voor de omzetting tussen hun eigen berichtindelingen. Dit is een algemene vereiste; bijvoorbeeld wanneer u moet een nieuwe, geïntegreerde interface naar oudere ongelijksoortige systemen bieden of na een fusie IT-systemen integreren.

De software-industrie is een bedrijf ontdekt; nieuwe moderne programmeertalen en toepassingsframeworks zijn geïntroduceerd in een tempo soms heel breed. Op dezelfde manier de vereisten van de IT-systemen loop der tijd veranderen en ontwikkelaars meer willen profiteren van de nieuwste functies van het platform. Maar ondersteunt soms de leverancier van de geselecteerde messaging geen deze platformen. Omdat messaging-protocollen eigen zijn, is het niet mogelijk is voor anderen voor bibliotheken voor deze nieuwe platformen. Daarom moet u methoden, zoals het bouwen van gateways of bruggen waarmee u kunt echter ook doorgaan met het product messaging.

De ontwikkeling van het Advanced Message Queuing Protocol (AMQP 1.0) is gemotiveerd door deze problemen. Deze afkomstig is van JP Morgan Chase, die, zoals de meeste financiële dienstverleners, zware gebruikers van de bericht-georiënteerde middleware. Het doel is eenvoudig: het maken van een open standaard-berichtenprotocol dat maakte het mogelijk om met behulp van de onderdelen die zijn gebouwd met behulp van verschillende talen, frameworks en -besturingssystemen op basis van een bericht-toepassingen te bouwen alles met behulp van beste van RAS-onderdelen uit een scala aan leveranciers.

## <a name="amqp-10-technical-features"></a>Technische AMQP 1.0-functies
AMQP 1.0 is een efficiënte, betrouwbare, wire-level berichtenprotocol dat u gebruiken kunt voor het bouwen van robuuste, platformonafhankelijke toepassingen berichten. Het protocol is een eenvoudige doel: voor het definiëren van het mechanisme van de beveiligde, betrouwbare en efficiënte overdracht van berichten tussen twee partijen. De berichten zelf zijn gecodeerd met behulp van de weergave van een draagbare gegevens waarmee heterogene afzenders en ontvangers voor het uitwisselen van gestructureerde business-berichten op vol vertrouwen. Hier volgt een overzicht van de belangrijkste functies:

* **Efficiënte**: AMQP 1.0 is een verbinding-georiënteerd die maakt gebruik van een binaire codering voor de instructies van het protocol en de business-berichten overgebracht via het protocol. Deze bevat geavanceerde datatransportbesturing schema's voor het gebruik van het netwerk en de verbonden onderdelen te maximaliseren. Dat gezegd, het protocol is ontworpen een evenwicht tot stand tussen de efficiëntie, flexibiliteit en interoperabiliteit zijn.
* **Betrouwbare**: het AMQP 1.0-protocol kan berichten worden uitgewisseld met een scala aan garanties voor betrouwbaarheid, van fire-and-forgetstromen voor betrouwbare, precies-eenmaal bevestigd levering.
* **Flexibele**: AMQP 1.0 is een flexibel protocol die kan worden gebruikt voor de ondersteuning van verschillende topologieën. Hetzelfde protocol kan worden gebruikt voor communicatie van client-naar-client, client-naar-broker en broker-naar-broker.
* **Onafhankelijk van de Broker-model**: de AMQP 1.0-specificatie maakt geen geen vereisten voor het model van de berichten die worden gebruikt door een broker. Dit betekent dat het is mogelijk eenvoudig AMQP 1.0-ondersteuning toevoegen aan bestaande berichtenbrokers.

## <a name="amqp-10-is-a-standard-with-a-capital-s"></a>AMQP 1.0 is een standaard (met een hoofdletter van ')
AMQP 1.0 is een internationale standaard, goedgekeurd door de ISO en IEC als ISO/IEC 19464:2014.

AMQP 1.0 is in ontwikkeling sinds 2008 door een groep core van meer dan 20 bedrijven, zowel leveranciers van technologie en eindgebruikers ondernemingen. Gedurende deze periode kan gebruiker ondernemingen hebben bijgedragen hun echte zakelijke vereisten en de technologieleveranciers het protocol om te voldoen aan deze vereisten hebben ontwikkeld. Gedurende het proces hebben leveranciers deelgenomen aan workshops waarin ze heeft voor het valideren van de interoperabiliteit tussen hun implementaties.

In oktober 2011, is de projecten overgezet naar een technisch comité binnen de organisatie voor de Advancement of Structured Information Standards (OASIS) en de Standard OASIS AMQP 1.0 in oktober 2012 uitgebracht. De volgende ondernemingen hebben deelgenomen aan het technische Comité tijdens de ontwikkeling van de standaard:

* **Technologieleveranciers**: Axway Software, Huawei Technologies, vorm Software, INETCO systemen, Kaazing, Microsoft, Mitre Corporation, Primeton technologieën, voortgang van Software, Red Hat, SITA, Software AG, Solace systemen, VMware, WSO2, Zenika.
* **Gebruiker ondernemingen**: Bank-Amerika, tegoed Zwitserland, Deutsche Boerse, Goldman Sachs, JPMorgan Chase.

De vaak gehoord voordelen van open standaarden zijn onder andere:

* Minder kans van de leverancier van vergrendeling
* Interoperabiliteit
* Grote beschikbaarheid van bibliotheken en hulpprogramma 's
* Bescherming tegen veroudering
* Beschikbaarheid van deskundig personeel
* Lagere en beheerbare risico

## <a name="amqp-10-and-service-bus"></a>AMQP 1.0- en Service Bus
Ondersteuning van AMQP 1.0 in Azure Service Bus betekent dat u kunt nu gebruikmaken van het in de Service Bus-wachtrij en publiceren/abonneren brokered messaging-onderdelen uit een scala aan platformen met behulp van een doeltreffende binaire-protocol. Bovendien kunt u toepassingen bestaat uit onderdelen die zijn gebouwd met behulp van een combinatie van programmeertalen, frameworks en besturingssystemen bouwen.

De volgende afbeelding ziet u een voorbeeld van een implementatie waarbij Java-clients die worden uitgevoerd op Linux, die zijn geschreven met behulp van de standaard Java Message Service (JMS) API en .NET-clients die worden uitgevoerd op Windows, berichten uitgewisseld via Service Bus AMQP 1.0 via exchange.

![][0]

**Afbeelding 1: Implementatie voorbeeldscenario met platformonafhankelijke berichtenverzending met behulp van Service Bus en AMQP 1.0**

Op dit moment worden de volgende clientbibliotheken bekend bij het werken met Service Bus:

| Taal | Bibliotheek |
| --- | --- |
| Java |Apache Qpid Java Message Service JMS ()-client<br/>VORM Software SwiftMQ Java-clients |
| C |Apache Qpid Proton-C |
| PHP |Apache Qpid Proton-PHP |
| Python |Apache Qpid Proton-Python |
| C# |AMQP .Net Lite |

**Afbeelding 2: Tabel van AMQP 1.0-clientbibliotheken**

## <a name="summary"></a>Samenvatting
* AMQP 1.0 is een open, betrouwbare berichtenprotocol dat kunt u platformonafhankelijke, hybride toepassingen bouwen. AMQP 1.0 is een OASIS-standaard.
* Ondersteuning van AMQP 1.0 is nu beschikbaar in Azure Service Bus, evenals Service Bus voor Windows Server (Service Bus 1.1). Prijzen zijn dezelfde als die voor de bestaande protocollen.

## <a name="next-steps"></a>Volgende stappen
Bent u klaar om meer te weten? Ga naar de volgende koppelingen:

* [Met behulp van Servicebus van .NET met AMQP]
* [Met behulp van Servicebus in Java met AMQP]
* [Apache Qpid Proton-C installeren op een Azure Linux-VM]
* [AMQP in WindowsServer-Servicebus]

[0]: ./media/service-bus-amqp-overview/service-bus-amqp-1.png
[Met behulp van Servicebus van .NET met AMQP]: service-bus-amqp-dotnet.md
[Met behulp van Servicebus in Java met AMQP]: service-bus-amqp-java.md
[Apache Qpid Proton-C installeren op een Azure Linux-VM]: service-bus-amqp-apache.md
[AMQP in WindowsServer-Servicebus]: https://msdn.microsoft.com/library/dn574799.aspx
