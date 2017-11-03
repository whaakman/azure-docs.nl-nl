---
title: Isolatie van Azure Service Bus-toepassingen tegen storingen en noodsituaties | Microsoft Docs
description: Technieken tegen een mogelijke onderbreking van de Service Bus-toepassingen beschermt.
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: fd9fa8ab-f4c4-43f7-974f-c876df1614d4
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/06/2017
ms.author: sethm
ms.openlocfilehash: 6dd9045d7aa8d4dc8b3a1acbe6f927e232d9b505
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="best-practices-for-insulating-applications-against-service-bus-outages-and-disasters"></a>Aanbevolen procedures voor de isolatie van toepassingen tegen storingen van de Service Bus en noodsituaties

Bedrijfskritieke toepassingen hanteert continu, zelfs in aanwezigheid van niet-geplande storingen of noodsituaties. Dit onderwerp beschrijft de technieken die u gebruiken kunt voor het beveiligen van Service Bus-toepassingen op basis van een mogelijke onderbreking van deze service of na noodgevallen.

Een storing wordt gedefinieerd als Azure Service Bus, tijdelijk niet beschikbaar. De storing, kan sommige onderdelen van Service Bus, zoals een berichten-store of zelfs het gehele datacenter beïnvloeden. Nadat het probleem is verholpen, weer Service Bus beschikbaar is. Normaal gesproken veroorzaakt een storing geen verlies van berichten of andere gegevens. Een voorbeeld van een onderdeelfout is het ontbreken van een bepaalde berichten-store. Een voorbeeld van een onderbreking van de hele datacenter is een stroomstoring van het datacenter of een netwerkswitch defecte datacenter. Een storing kan de laatste van enkele minuten tot enkele dagen.

Een noodgeval wordt gedefinieerd als permanent verlies van een Service Bus-schaaleenheid of datacenter. Het datacenter kan of kan niet weer beschikbaar. Doorgaans verloren een noodgeval enkele of alle berichten of andere gegevens. Voorbeelden van noodsituaties zijn gestart, die het overspoelen of aardbeving.

## <a name="current-architecture"></a>Huidige architectuur
Service Bus maakt gebruik van meerdere berichten-stores voor het opslaan van berichten die worden verzonden naar wachtrijen en onderwerpen. Een niet-gepartitioneerde wachtrij of onderwerp is toegewezen aan één berichten-store. Als deze berichten-store niet beschikbaar is, mislukt de alle bewerkingen voor de wachtrij of onderwerp.

Alle Service Bus messaging-entiteiten (wachtrijen, onderwerpen, relays) zich bevinden in een service-naamruimte die is gekoppeld aan een datacenter. Service Bus kunnen niet automatisch geo-replicatie van gegevens, noch toestaan een naamruimte te combineren van meerdere datacenters.

## <a name="protecting-against-acs-outages"></a>Bescherming tegen storingen van ACS
Als u van referenties voor ACS gebruikmaakt en ACS niet meer beschikbaar is, kunnen clients niet langer tokens verkrijgen. Clients die een token hebben op het moment dat ACS uitvalt kunnen blijven gebruiken van Service Bus totdat de tokens verlopen. De standaardlevensduur van de token is drie uur.

Ter bescherming tegen storingen van ACS, gebruikt u de Shared Access Signature (SAS)-tokens. In dit geval verifieert de client rechtstreeks met Service Bus met een zelf minted token-ondertekening met een geheime sleutel. Aanroepen naar ACS zijn niet langer vereist. Zie voor meer informatie over SAS-tokens [Service Bus verificatie][Service Bus authentication].

## <a name="protecting-queues-and-topics-against-messaging-store-failures"></a>Beveiligen van wachtrijen en onderwerpen tegen storingen store messaging
Een niet-gepartitioneerde wachtrij of onderwerp is toegewezen aan één berichten-store. Als deze berichten-store niet beschikbaar is, mislukt de alle bewerkingen voor de wachtrij of onderwerp. Een gepartitioneerde wachtrij aan de andere kant bestaat uit meerdere fragmenten. Elke fragment wordt opgeslagen in een andere berichten-store. Wanneer een bericht wordt verzonden naar een gepartitioneerde wachtrij of onderwerp, wijst Service Bus het bericht toe aan een van de fragmenten. Als de bijbehorende berichten-store niet beschikbaar is, schrijft Service Bus het bericht indien mogelijk naar een andere fragment. Zie voor meer informatie over gepartitioneerde entiteiten [gepartitioneerde berichtentiteiten][Partitioned messaging entities].

## <a name="protecting-against-datacenter-outages-or-disasters"></a>Bescherming tegen storingen van datacenter of rampen
Als u wilt toestaan voor een failover tussen twee datacentra, kunt u een Service Bus-Servicenaamruimte maken in elk datacenter. Bijvoorbeeld: de Service Bus-Servicenaamruimte **contosoPrimary.servicebus.windows.net** zich mogelijk in de regio Noord/centraal van de Verenigde Staten en **contosoSecondary.servicebus.windows.net**zich mogelijk in de regio VS Zuid-/ centraal. Als een Service Bus-berichtenservice entiteit blijven toegankelijk in de aanwezigheid van een storing in datacenter zijn moet, kunt u die entiteit maken in beide naamruimten.

Zie voor meer informatie de sectie '-fout van Servicebus binnen een Azure-datacenter in [asynchrone messaging-patronen en hoge beschikbaarheid][Asynchronous messaging patterns and high availability].

## <a name="protecting-relay-endpoints-against-datacenter-outages-or-disasters"></a>Beveiligen van eindpunten relay tegen storingen van datacenter of rampen
Geo-replicatie van de relay-eindpunten kan een service waarmee een relay-eindpunt om te worden bereikt met Service Bus storingen. Als u wilt bereiken geo-replicatie, moet de service relay-eindpunten maken in verschillende naamruimten. De naamruimten moet zich bevinden in verschillende datacenters en de twee eindpunten moeten verschillende namen hebben. Bijvoorbeeld: een primaire eindpunt kan worden bereikt onder **contosoPrimary.servicebus.windows.net/myPrimaryService**, terwijl het bijbehorende secundaire server kan worden bereikt onder **contosoSecondary.servicebus.windows.net /mySecondaryService**.

De service vervolgens luistert op beide eindpunten en een client de service via een eindpunt kunt aanroepen. Een clienttoepassing willekeurig een van de relays hervat als het primaire eindpunt en de aanvraag verzendt naar de actieve eindpunt. Als de bewerking is mislukt met foutcode, wordt deze fout geeft aan de relay-eindpunt is niet beschikbaar. De toepassing een kanaal geopend naar het back-eindpunt en databasebron moet worden gestuurd de aanvraag. De actieve en de back-eindpunten overschakelen op dat moment rollen: de clienttoepassing beschouwt het oude actieve eindpunt worden de nieuwe back-eindpunt en de oude back-eindpunt moet de nieuwe actieve eindpunt. Als beide mislukken verzendt, de functies van de twee entiteiten blijven ongewijzigd en wordt een fout geretourneerd.

## <a name="protecting-queues-and-topics-against-datacenter-outages-or-disasters"></a>Wachtrijen en onderwerpen tegen storingen van datacenter of noodsituaties beveiligen
Als u wilt bereiken herstelmogelijkheden tegen storingen van datacenter wanneer met behulp van brokered messaging, Service Bus ondersteunt twee methoden: *active* en *passieve* replicatie. Voor elke methode als een bepaalde wachtrij of onderwerp toegankelijk in de aanwezigheid van een storing in een datacenter blijven moet, kunt u dit maken in beide naamruimten. Beide entiteiten kunnen dezelfde naam hebben. Bijvoorbeeld: een primaire wachtrij kan worden bereikt onder **contosoPrimary.servicebus.windows.net/myQueue**, terwijl het bijbehorende secundaire server kan worden bereikt onder **contosoSecondary.servicebus.windows.net/myQueue**.

Als de toepassing geen permanente afzender naar ontvanger communicatie vereist, kan de toepassing een duurzame clientzijde wachtrij om te voorkomen dat bericht verloren gaan en af te schermen van de afzender van een tijdelijke fouten met Service Bus implementeren.

## <a name="active-replication"></a>Actieve replicatie
Actieve replicatie maakt gebruik van entiteiten in beide naamruimten voor elke bewerking. Een client die u een bericht verzendt verzendt twee kopieën van hetzelfde bericht. De eerste kopie wordt verzonden naar de primaire entiteit (bijvoorbeeld **contosoPrimary.servicebus.windows.net/sales**), en de tweede kopie van het bericht wordt verzonden naar de secundaire entiteit (bijvoorbeeld  **contosoSecondary.servicebus.windows.net/sales**).

Een client ontvangt berichten van beide wachtrijen. De ontvanger de eerste kopie van een bericht verwerkt en de tweede kopie wordt onderdrukt. Als u wilt onderdrukken dubbele berichten, moet de afzender elk bericht met een unieke identificatie labelen. Beide exemplaren van het bericht moeten worden gemarkeerd met dezelfde id. U kunt de [BrokeredMessage.MessageId] [ BrokeredMessage.MessageId] of [BrokeredMessage.Label] [ BrokeredMessage.Label] eigenschappen of een aangepaste eigenschap voor het taggen van het bericht. De ontvanger moet een lijst met berichten die al zijn ontvangen onderhouden.

De [Geo-replicatie met Service Bus Brokered berichten] [ Geo-replication with Service Bus Brokered Messages] ziet u het actieve replicatie van berichtentiteiten.

> [!NOTE]
> De actieve replicatie aanpak verdubbelt het aantal bewerkingen, dus deze benadering kan leiden tot hogere kosten.
> 
> 

## <a name="passive-replication"></a>Passieve replicatie
In het geval veroorzaakt gratis passieve replicatie maakt gebruik van slechts één van de twee berichtentiteiten. Een client verzendt het bericht naar de actieve entiteit. Als de bewerking op de actieve entiteit is mislukt met de foutcode die aangeeft van het datacenter dat als host fungeert voor de actieve entiteit is mogelijk niet beschikbaar is, verzendt de client een kopie van het bericht naar de back-entiteit. De actieve en de back-entiteiten overschakelen op dat moment rollen: de verzendende client beschouwt de oude active entiteit moet de nieuwe back-entiteit en de oude back-entiteit is de nieuwe actieve entiteit. Als beide mislukken verzendt, de functies van de twee entiteiten blijven ongewijzigd en wordt een fout geretourneerd.

Een client ontvangt berichten van beide wachtrijen. Omdat er een kans is dat de ontvanger twee kopieën van hetzelfde bericht ontvangt, moet de ontvanger dubbele berichten onderdrukt. U kunt duplicaten onderdrukken op dezelfde manier zoals beschreven voor actieve replicatie.

Passieve replicatie is in het algemeen goedkoper dan actieve replicatie omdat in de meeste gevallen slechts één bewerking wordt uitgevoerd. Latentie, doorvoer en kosten zijn identiek aan de niet-gerepliceerde scenario.

Als u passieve replicatie gebruikt, in de volgende scenario's kunnen berichten zoekraken of worden twee keer ontvangen:

* **Bericht vertraging of verlies**: wordt ervan uitgegaan dat de afzender een m1 bericht naar de primaire wachtrij verzonden en de wachtrij vervolgens niet beschikbaar wordt voordat de ontvanger m1 ontvangt. De afzender verzendt een m2 daaropvolgende bericht naar de secundaire wachtrij. Als de primaire wachtrij tijdelijk niet beschikbaar is, ontvangt de ontvanger m1 nadat de wachtrij weer beschikbaar is. Als er een ramp optreedt ontvangt de ontvanger mogelijk nooit m1.
* **Dubbele ontvangst**: wordt ervan uitgegaan dat de afzender een bericht m naar de primaire wachtrij verzendt. Service Bus m verwerkt is, maar voor het verzenden van een antwoord is mislukt. Nadat er is een time-out opgetreden voor de verzendbewerking, verzendt de afzender een identieke kopie van m naar de secundaire wachtrij. Als de ontvanger kan de eerste kopie van m ontvangen voordat de primaire wachtrij niet beschikbaar is, ontvangt de ontvanger beide kopieën van m rond dezelfde tijd. Als de ontvanger kan niet ontvangen van de eerste kopie van m voordat de primaire wachtrij niet meer beschikbaar is, wordt de ontvanger in eerste instantie ontvangt alleen het tweede exemplaar van m, maar u ontvangt vervolgens een tweede kopie van m wanneer de primaire wachtrij beschikbaar komt.

De [Geo-replicatie met Service Bus brokered berichten] [ Geo-replication with Service Bus Brokered Messages] voorbeeld demonstreert passieve replicatie van berichtentiteiten.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over herstel na noodgevallen, deze artikelen:

* [Azure SQL Database Business Continuity][Azure SQL Database Business Continuity]
* [Robuuste toepassingen voor Azure ontwerpen][Azure resiliency technical guidance]

[Service Bus Authentication]: service-bus-authentication-and-authorization.md
[Partitioned messaging entities]: service-bus-partitioning.md
[Asynchronous messaging patterns and high availability]: service-bus-async-messaging.md#failure-of-service-bus-within-an-azure-datacenter
[BrokeredMessage.MessageId]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_MessageId
[BrokeredMessage.Label]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Label
[Geo-replication with Service Bus Brokered Messages]: https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoReplication
[Azure SQL Database Business Continuity]: ../sql-database/sql-database-business-continuity.md
[Azure resiliency technical guidance]: /azure/architecture/resiliency
