---
title: Overzicht van AMQP 1.0 in de Azure Servicebus | Microsoft Docs
description: Meer informatie over het gebruik van de geavanceerde Message Queuing-Protocol (AMQP) 1.0 in Azure.
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 0e8d19cc-de36-478e-84ae-e089bbc2d515
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/06/2017
ms.author: sethm
ms.openlocfilehash: e2d190dddc3b84eb07eaf2ffea1af48bba0ea13a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="amqp-10-support-in-service-bus"></a>AMQP 1.0-ondersteuning in Service Bus
De Azure Service Bus cloud-service en de on-premises [Service Bus voor Windows Server (Service Bus 1.1)](https://msdn.microsoft.com/library/dn282144.aspx) ondersteuning van de geavanceerde Message Queueing Protocol (AMQP) 1.0. AMQP kunt u cross-platform, hybride toepassingen met behulp van een open standaard protocol bouwen. U kunt opstellen toepassingen met behulp van de onderdelen die zijn gemaakt met behulp van verschillende talen en frameworks en die worden uitgevoerd op verschillende besturingssystemen worden uitgevoerd. Al deze onderdelen verbinding met Service Bus en naadloos maken kunnen uitwisselen gestructureerde business berichten efficiënt en volledig betrouwbaar.

## <a name="introduction-what-is-amqp-10-and-why-is-it-important"></a>Inleiding: Wat is AMQP 1.0 en waarom is het belangrijk?
Traditioneel bericht georiënteerd middlewareproducten eigen protocollen gebruikt voor communicatie tussen clienttoepassingen en beleggingsmakelaars. Dit betekent dat wanneer u een bepaalde leverancier messaging broker hebt geselecteerd, u de bibliotheken van de leverancier gebruiken moet om verbinding maken met uw clienttoepassingen die broker. Dit resulteert in een zekere mate van afhankelijkheid van die leverancier omdat codewijzigingen in de verbonden toepassingen overbrengen van een toepassing naar een ander product vereist. 

Bovendien is het lastig berichtenbrokers verbinding te maken van verschillende leveranciers. Dit is doorgaans op toepassingsniveau bridging om berichten te verplaatsen van het ene systeem naar een andere en voor de omzetting tussen hun eigen berichtindeling. Dit is een algemene vereiste; bijvoorbeeld wanneer u moet een nieuwe, uniforme interface bieden tot oudere verschillende systemen of integreren van IT-systemen na een fusie.

De softwarebranche is een bedrijf snel bewegende; nieuwe programmeertalen en toepassingsframeworks worden soms heel breed tempo geïntroduceerd. Op dezelfde manier de vereisten van de IT-systemen evolueren na verloop van tijd en ontwikkelaars wilt profiteren van de nieuwste platformfuncties van het. Echter, soms de leverancier van de geselecteerde messaging ondersteunt geen deze platforms. Omdat messaging-protocollen eigen zijn, is het niet mogelijk voor anderen voor bibliotheken voor deze nieuwe platforms. Gebruik daarom benaderingen zoals gateways of bruggen waarmee u kunt blijven gebruiken van het product messaging bouwt.

De ontwikkeling van de geavanceerde Message Queuing-Protocol (AMQP) 1.0 is gemotiveerd door deze problemen. Deze afkomstig is van JP Morgan Chase die, zoals de meest financiële diensten ondernemingen, zware gebruikers van middleware bericht georiënteerd. Het doel is eenvoudig: voor het maken van een open standaard messaging-protocol dat voor het bouwen van toepassingen op basis van een bericht met onderdelen die zijn gebouwd met behulp van verschillende talen en frameworks besturingssystemen, alles met behulp van beste van RAS-onderdelen van een bereik van leveranciers.

## <a name="amqp-10-technical-features"></a>Technische AMQP 1.0-functies
AMQP 1.0 is een efficiënte, betrouwbare, wire-niveau messaging protocol die u gebruiken kunt voor het bouwen van robuuste, platformoverschrijdende, berichttoepassingen. Het protocol is een eenvoudige doel: voor het definiëren van het mechanisme van de veilig, betrouwbaar en efficiënt overdracht van berichten tussen twee partijen. De berichten zelf zijn gecodeerd met behulp van een draagbare gegevens weergave waarmee heterogene afzenders en ontvangers voor het uitwisselen van gestructureerde business berichten op volledige betrouwbaarheid. Hier volgt een samenvatting van de belangrijkste functies:

* **Efficiënte**: AMQP 1.0 is verbindings-geörienteerde protocol dat wordt gebruikt een binaire codering voor de instructies van het protocol en de berichten business overgedragen. Datatransportbesturing geavanceerde schema's voor het maximaliseren van het netwerk en de verbonden onderdelen opgenomen. Die gezegd, het protocol is ontworpen om u te bieden een evenwicht tussen efficiëntie, flexibiliteit en interoperabiliteit zijn.
* **Betrouwbare**: de AMQP 1.0-protocol kunnen berichten worden uitgewisseld met een bereik van betrouwbaarheid garanties, van fire-en-vergeet betrouwbare, precies-levering eenmaal bevestigd.
* **Flexibele**: AMQP 1.0 is een flexibel protocol die kan worden gebruikt ter ondersteuning van verschillende topologieën. Hetzelfde protocol kan worden gebruikt voor communicatie van client-naar-client, client-naar-broker en broker naar broker.
* **Onafhankelijk van de Broker-model**: de AMQP 1.0-specificatie maakt geen eventuele vereisten op het messaging model gebruikt door een broker. Dit betekent dat het is mogelijk eenvoudig AMQP 1.0-ondersteuning toevoegen aan bestaande berichtenbrokers.

## <a name="amqp-10-is-a-standard-with-a-capital-s"></a>AMQP 1.0 is een standaard (met een hoofdletter ')
AMQP 1.0 is een internationaal standaard goedgekeurd door de ISO en IEC als ISO/IEC 19464:2014.

AMQP 1.0 is in ontwikkeling sinds 2008 door een groep core van meer dan 20 bedrijven, zowel technologie leveranciers en eindgebruikers ondernemingen. Gedurende die tijd gebruiker bedrijven hebben bijgedragen tot de echte zakelijke vereisten en de technologieleveranciers voor het protocol om te voldoen aan deze vereisten hebben ontwikkeld. Tijdens het proces hebben leveranciers deelgenomen aan workshops waarin ze samengewerkt voor het valideren van de samenwerking tussen hun implementaties.

In oktober 2011 werd de projecten die aan een technische Comité binnen de organisatie voor de vooruitgang van gestructureerde gegevens Standards (OASIS) en de standaard OASIS AMQP 1.0 is overgegaan uitgebracht in oktober 2012. De volgende bedrijven hebben deelgenomen aan het technische Comité tijdens de ontwikkeling van de standaard:

* **Technologieleveranciers**: Axway Software, Huawei technologieën, vorm Software, INETCO systemen, Kaazing, Microsoft, Mitre Corporation, Primeton technologieën, voortgang van de Software, Red Hat, SITA, Software AG, Solace systemen, VMware, WSO2, Zenika.
* **Gebruiker ondernemingen**: Bank-Amerika, tegoed Zwitserland, Deutsche Boerse, Goldman Sachs, JPMorgan Chase.

Enkele van de vaak vermelde voordelen van open standaarden:

* Minder kans op vaste leverancier
* Interoperabiliteit
* Algemene beschikbaarheid van bibliotheken en tooling
* Bescherming tegen veroudering
* Beschikbaarheid van geïnformeerde personeel
* Lagere en beheerbare risico

## <a name="amqp-10-and-service-bus"></a>AMQP 1.0 en Service Bus
AMQP 1.0-ondersteuning in Azure Service Bus betekent dat u kunt nu gebruikmaken van het in de Service Bus-wachtrij en publiceren/abonneren brokered messaging-onderdelen uit een reeks met behulp van een efficiënte binaire protocol platforms. Bovendien kunt u toepassingen bestaan uit de onderdelen die zijn gebouwd met behulp van een combinatie van talen en frameworks besturingssystemen maken.

De volgende afbeelding ziet u een voorbeeldimplementatie waarbij Java-clients op Linux, geschreven met behulp van de standaard Java bericht Service (JMS) API en de .NET-clients waarop Windows, exchange-berichten uit via Service Bus met AMQP 1.0.

![][0]

**Afbeelding 1: Implementatie voorbeeldscenario platformonafhankelijke berichten met behulp van Service Bus en AMQP 1.0 wordt weergegeven**

Op dit moment worden de volgende clientbibliotheken bekende werken met Service Bus:

| Taal | Bibliotheek |
| --- | --- |
| Java |Apache Qpid Java bericht Service (JMS)-client<br/>VORM Software SwiftMQ Java-client |
| C |Apache Qpid Proton-C |
| PHP |Apache Qpid Proton-PHP |
| Python |Apache Qpid Proton-Python |
| C# |AMQP .net Lite |

**Afbeelding 2: Tabel met AMQP 1.0-clientbibliotheken**

## <a name="summary"></a>Samenvatting
* AMQP 1.0 is een open, betrouwbare messaging-protocol die u gebruiken kunt voor het bouwen van hybride toepassingen met meerdere platforms. AMQP 1.0 is een standaard OASIS.
* AMQP 1.0-ondersteuning is nu beschikbaar in de Azure Service Bus, evenals een Service Bus voor Windows Server (Service Bus 1.1). Prijzen is hetzelfde als voor de bestaande protocollen.

## <a name="next-steps"></a>Volgende stappen
Gereed voor meer informatie? Ga naar de volgende koppelingen:

* [Met behulp van Servicebus in .NET met AMQP]
* [Met behulp van Servicebus met Java met AMQP]
* [Apache Qpid Proton-C installeren op een Azure Linux VM]
* [AMQP in WindowsServer-Servicebus]

[0]: ./media/service-bus-amqp-overview/service-bus-amqp-1.png
[Met behulp van Servicebus in .NET met AMQP]: service-bus-amqp-dotnet.md
[Met behulp van Servicebus met Java met AMQP]: service-bus-amqp-java.md
[Apache Qpid Proton-C installeren op een Azure Linux VM]: service-bus-amqp-apache.md
[AMQP in WindowsServer-Servicebus]: https://msdn.microsoft.com/library/dn574799.aspx
