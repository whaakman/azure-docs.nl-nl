---
title: Overzicht van Azure Service Bus-berichtenservice | Microsoft Docs
description: Beschrijving van Service Bus-berichten en Azure Relay
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: f99766cb-8f4b-4baf-b061-4b1e2ae570e4
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: get-started-article
ms.date: 08/30/2017
ms.author: sethm
ms.openlocfilehash: 462fa0a6e86237b8c9d9ba97aa377f8d112a8b8e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="service-bus-messaging-flexible-data-delivery-in-the-cloud"></a>Service Bus-berichtenservice: flexibele levering van gegevens in de cloud

De Microsoft Azure Service Bus is een betrouwbare service voor de levering van informatie. Het doel van deze service is om communicatie te vergemakkelijken. Wanneer twee of meer partijen informatie willen uitwisselen, hebben ze een communicatiemechanisme nodig. De Service Bus is een brokered communicatiemechanisme, of een communicatiemechanisme van derden. Het is vergelijkbaar met een postservice in de fysieke wereld. Postservices maken het gemakkelijk om verschillende soorten brieven en pakketten met tal van verschillende leveringsgaranties overal ter wereld te leveren.

Net zoals een postservice die brieven levert, is de Service Bus een flexibele leverancier van informatie voor zowel de afzender als de ontvanger. De berichtenservice zorgt ervoor dat de informatie wordt afgeleverd, zelfs als de twee partijen nooit allebei op hetzelfde moment online zijn of als ze niet op precies hetzelfde moment beschikbaar zijn. In die zin is het verzenden van berichten vergelijkbaar met het verzenden van een brief, terwijl niet-brokered communicatie vergelijkbaar is met een telefonische oproep (uit de tijd dat er nog geen wachtstand en beller-id was, want die services komen meer in de richting van Brokered Messaging).

De afzender van het bericht kan ook een aantal verschillende leveringskenmerken vereisen, zoals transacties, detectie van duplicaten, vervallen van berichten op basis van tijd en batchverzending. Ook deze patronen laten zich vergelijken met een postservice: herhaalde levering, vereisen van een handtekening, adreswijziging of intrekken van het bericht.

Service Bus ondersteunt twee verschillende berichtpatronen: *Azure Relay* en *Service Bus-berichten*.

## <a name="azure-relay"></a>Azure Relay

Het onderdeel [WCF Relay](../service-bus-relay/relay-what-is-it.md) van Azure Relay is een gecentraliseerde service (echter met maximale taakverdeling) die ondersteuning biedt voor tal van verschillende transportprotocollen en webservicestandaarden, waaronder SOAP, WS-* en zelfs REST. De [Relay-service](../service-bus-relay/service-bus-dotnet-how-to-use-relay.md) biedt tal van verschillende Relay-connectiviteitsopties en kan helpen bij het onderhandelen over directe peer-to-peer-verbindingen wanneer dit mogelijk is. Service Bus is geoptimaliseerd voor .NET-ontwikkelaars die gebruikmaken van WCF (Windows Communication Foundation), zowel op het gebied van prestaties als op het gebied van bruikbaarheid, en biedt volledige toegang tot de bijbehorende Relay-service via SOAP- en REST-interfaces. Daardoor kunnen SOAP- of REST-programmeeromgevingen worden geïntegreerd met Service Bus.

De Relay-service ondersteunt traditionele berichten in één richting, aanvraag-/antwoordberichten en peer-to-peerberichten. De service ondersteunt tevens gebeurtenisdistributie via internet voor scenario's voor publiceren/abonneren en bidirectionele socket-communicatie voor verbeterde point-to-point-efficiëntie. In het Relayed Messaging-patroon maakt een on-premises service verbinding met de Relay-service via een uitgaande poort en wordt een bidirectionele socket voor communicatie gemaakt die is gekoppeld aan een bepaald rendezvous-adres. De client kan vervolgens met de on-premises service communiceren door berichten te verzenden naar de Relay-service die gericht is op het rendezvous-adres. De Relay-service stuurt vervolgens berichten door ('relay') naar de on-premises service via de reeds aanwezige bidirectionele socket. De client heeft geen rechtstreekse verbinding met de on-premises service nodig en hoeft niet te weten waar de service zich bevindt. Voor de on-premises service is niet vereist dat poorten voor inkomend verkeer zijn geopend in de firewall.

U start de verbinding tussen uw on-premises service en de Relay-service met een reeks WCF 'Relay'-bindingen. Achter de schermen worden de Relay-bindingen toegewezen aan nieuwe transportbindingselementen die zijn ontworpen om WCF-kanaalonderdelen te maken die kunnen worden geïntegreerd met Service Bus in de cloud.

WFC Relay biedt veel voordelen. Hiervoor is echter wel vereist dat de server en client allebei op hetzelfde moment online zijn om berichten te kunnen verzenden en ontvangen. Dit is niet optimaal voor HTTP-communicatie, waarbij de aanvragen doorgaans mogelijk geen lange levensduur hebben, noch voor clients die maar zo nu en dan verbinding maken, zoals browsers, mobiele toepassingen enzovoort. Brokered Messaging ondersteunt ontkoppelde communicatie en heeft voordelen op zich. Clients en servers kunnen wanneer nodig verbinding maken en hun bewerkingen asynchroon uitvoeren.

## <a name="brokered-messaging"></a>Brokered Messaging

In tegenstelling tot het relay-schema kunnen Service Bus-berichten met [wachtrijen, onderwerpen en abonnementen](service-bus-queues-topics-subscriptions.md) worden beschouwd als asynchroon of 'tijdelijk losgekoppeld'. Producenten (afzenders) en consumenten (ontvangers) hoeven niet gelijktijdig online te zijn. De berichteninfrastructuur slaat berichten veilig op in een 'broker' (zoals een wachtrij) tot de ontvangende partij gereed is om ze te ontvangen. Hierdoor kunnen de onderdelen van de gedistribueerde toepassing worden losgekoppeld - hetzij vrijwillig, bijvoorbeeld voor onderhoud, hetzij vanwege het vastlopen van een onderdeel - zonder dat dit van invloed is op het hele systeem. Bovendien hoeft de ontvangende toepassing slechts op bepaalde tijdstippen gedurende de dag online te zijn. Denk bijvoorbeeld aan een systeem voor voorraadbeheer, dat alleen aan het einde van de dag hoeft te worden uitgevoerd.

De belangrijkste onderdelen van de infrastructuur voor Service Bus-berichten zijn wachtrijen, onderwerpen en abonnementen. Het belangrijkste verschil is dat onderwerpen mogelijkheden voor publiceren/abonneren ondersteunen die kunnen worden gebruikt voor geavanceerde, op inhoud gebaseerde routering en logica voor aflevering, met inbegrip van verzending naar meerdere ontvangers. Deze onderdelen faciliteren nieuwe scenario’s voor asynchrone berichtverzending, zoals tijdelijke ontkoppeling, publiceren/abonneren en taakverdeling. Zie [Service Bus-wachtrijen, -onderwerpen en -abonnementen](service-bus-queues-topics-subscriptions.md) voor meer informatie over deze berichtentiteiten.

Net als bij de WCF Relay-infrastructuur wordt de Brokered Messaging-functie aangeboden voor programmeurs van WCF en .NET Framework en via REST.

## <a name="next-steps"></a>Volgende stappen

Zie de volgende onderwerpen voor meer informatie over de Service Bus-berichtenservice

* [Grondbeginselen van Service Bus](service-bus-fundamentals-hybrid-solutions.md)
* [Service Bus-wachtrijen, -onderwerpen en -abonnementen](service-bus-queues-topics-subscriptions.md)
* [Aan de slag met Service Bus-wachtrijen](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus-onderwerpen en -abonnementen gebruiken](service-bus-dotnet-how-to-use-topics-subscriptions.md)

