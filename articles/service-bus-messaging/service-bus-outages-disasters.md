---
title: Isolatie van Azure Service Bus-toepassingen tegen uitval en noodgevallen | Microsoft Docs
description: Technieken voor het beveiligen van toepassingen op basis van een potentieel Service Bus-uitval.
services: service-bus-messaging
author: axisc
manager: timlt
editor: spelluru
ms.service: service-bus-messaging
ms.topic: article
ms.date: 09/14/2018
ms.author: aschhab
ms.openlocfilehash: 2a51447f3d9f8e9e8bed41c47214d7784924c85a
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/28/2019
ms.locfileid: "55099829"
---
# <a name="best-practices-for-insulating-applications-against-service-bus-outages-and-disasters"></a>Aanbevolen procedures voor de isolatie van toepassingen op basis van Service Bus-uitval en noodgevallen afhandelen

Bedrijfskritische toepassingen moeten continu, zelfs met niet-geplande uitval of rampen functioneren. Dit artikel wordt beschreven technieken die kunt u Service Bus-toepassingen op basis van een potentieel service stroomstoring of ramp te beschermen.

Een storing wordt gedefinieerd als tijdelijk ontbreken van Azure Service Bus. De onderbreking kan invloed hebben op sommige onderdelen van Service Bus, zoals een berichten-store, of zelfs het gehele datacenter. Nadat het probleem is opgelost, weer Service Bus beschikbaar is. Een storing wordt normaal gesproken niet leiden tot verlies van berichten of andere gegevens. Een voorbeeld van een onderdeelfout is het ontbreken van een bepaalde berichten-store. Een voorbeeld van een heel datacenter-uitval is een stroomstoring van het datacenter of een foutieve datacenter-netwerkswitch. Een storing kan de laatste van een paar minuten tot enkele dagen.

Na een noodgeval is gedefinieerd als het permanente verlies van gegevens van een Service Bus-schaaleenheid of datacenter. Het datacenter kan of kan niet weer beschikbaar. Normaal gesproken na een noodgeval zorgt ervoor dat verlies van sommige of alle berichten of andere gegevens. Voorbeelden van rampen zijn brand, dat wordt overspoeld of aardbeving.

## <a name="protecting-against-outages-and-disasters---service-bus-premium"></a>Bescherming tegen uitval en noodgevallen - Service Bus Premium
Hoge beschikbaarheid en herstel na noodgevallen concepten zijn standaard ingebouwd in de Azure Service Bus Premium-laag, zowel binnen dezelfde regio (via Beschikbaarheidszones) en in verschillende regio's (via Geo-noodherstel).

### <a name="geo-disaster-recovery"></a>Geo-herstel na noodgeval

Service Bus Premium biedt ondersteuning voor Geo-noodherstel, op het niveau van de naamruimte. Zie voor meer informatie, [Geo-noodherstel van Azure Service Bus](service-bus-geo-dr.md). De functie disaster recovery, beschikbaar voor de [Premium-SKU](service-bus-premium-messaging.md) alleen metagegevens noodherstel implementeert, en is afhankelijk van primaire en secundaire disaster recovery-naamruimten.

### <a name="availability-zones"></a>Beschikbaarheidszones

Biedt ondersteuning voor de Service Bus Premium-SKU [Beschikbaarheidszones](../availability-zones/az-overview.md), bieden foutgeïsoleerde locaties binnen dezelfde Azure-regio.

> [!NOTE]
> De ondersteuning voor Beschikbaarheidszones voor Azure Service Bus Premium is alleen beschikbaar in [Azure-regio's](../availability-zones/az-overview.md#regions-that-support-availability-zones) waar beschikbaarheidszones aanwezig zijn.

U kunt Beschikbaarheidszones inschakelen op nieuwe naamruimten, met behulp van de Azure portal. Service Bus biedt geen ondersteuning voor migratie van bestaande naamruimten. U kunt zoneredundantie niet uitschakelen nadat deze is ingeschakeld op uw naamruimte.

![1][]


## <a name="protecting-against-outages-and-disasters---service-bus-standard"></a>Bescherming tegen uitval en noodgevallen - standaard voor Service Bus
Voor het bereiken van tolerantie tegen storingen van de datacenter bij het gebruik van de berichten van de prijscategorie standard, Service Bus ondersteunt twee methoden: *active* en *passieve* replicatie. Voor elke methode als een bepaalde wachtrij of onderwerp toegankelijk met een uitval van het datacenter blijven moet, kunt u deze in beide naamruimten. Beide entiteiten kunnen dezelfde naam hebben. Bijvoorbeeld, een primaire wachtrij kan worden bereikt onder **contosoPrimary.servicebus.windows.net/myQueue**, terwijl het bijbehorende secundaire server kan worden bereikt onder **contosoSecondary.servicebus.windows.net/myQueue**.

>[!NOTE]
> De **actieve replicatie** en **passieve replicatie** setup zijn algemene oplossingen en geen specifieke functies van Service Bus. De replicatie-logica (verzenden naar 2 verschillende naamruimten) wordt toegevoegd aan de toepassingen van de afzender en de ontvanger heeft om aangepaste logica voor detectie van duplicaten.

Als de toepassing geen communicatie van permanente afzender-aan-ontvanger is vereist, kan de toepassing een duurzame client-side '-wachtrij om te voorkomen dat bericht verloren gaan en af te schermen van de afzender van tijdelijke fouten Service Bus implementeren.

### <a name="active-replication"></a>Actieve replicatie
Actieve replicatie maakt gebruik van entiteiten in beide naamruimten voor elke bewerking. Een willekeurige client die een bericht verzendt verzendt twee kopieën van hetzelfde bericht. De eerste kopie wordt verzonden naar de primaire entiteit (bijvoorbeeld **contosoPrimary.servicebus.windows.net/sales**), en de tweede kopie van het bericht wordt verzonden naar de secundaire entiteit (bijvoorbeeld  **contosoSecondary.servicebus.windows.net/sales**).

Een client ontvangt berichten van beide wachtrijen. De ontvanger het eerste exemplaar van een bericht verwerkt en de tweede kopie wordt onderdrukt. Als u wilt onderdrukken dubbele berichten, moet de afzender elk bericht een unieke id taggen. Beide exemplaren van het bericht moeten worden gelabeld met dezelfde id. U kunt de [BrokeredMessage.MessageId] [ BrokeredMessage.MessageId] of [BrokeredMessage.Label] [ BrokeredMessage.Label] eigenschappen of een aangepaste eigenschap voor het taggen van het bericht. De ontvanger moet een lijst met berichten die al is ontvangen behouden.

De [Geo-replicatie met Service Bus Standard-laag] [ Geo-replication with Service Bus Standard Tier] voorbeeld ziet u actieve replicatie van berichtentiteiten.

> [!NOTE]
> De aanpak van actieve replicatie verdubbelt het aantal bewerkingen, dus deze benadering kan leiden tot hogere kosten.
> 
> 

### <a name="passive-replication"></a>Passieve replicatie
In het geval fouttolerantie gratis passieve replicatie maakt gebruik van slechts één van de twee berichtentiteiten. Een client wordt het bericht verzonden naar de actieve entiteit. Als de bewerking op de actieve entiteit is mislukt met een foutcode die aangeeft van het datacenter dat als host fungeert voor de actieve entiteit is mogelijk niet beschikbaar is, verzendt de client een kopie van het bericht naar de back-entiteit. De actieve en de back-upentiteiten overschakelen op dat moment rollen: de verzendende client rekening gehouden met de oude active entiteit moet de nieuwe back-entiteit en de oude back-entiteit is de nieuwe actieve entiteit. Als beide mislukken verzenden, de rollen van de twee entiteiten blijven ongewijzigd en een fout geretourneerd.

Een client ontvangt berichten van beide wachtrijen. Omdat er een kans dat de ontvanger twee kopieën van hetzelfde bericht ontvangt, moet de ontvanger dubbele berichten onderdrukken. U kunt duplicaten onderdrukken op dezelfde manier zoals beschreven voor actieve replicatie.

Passieve replicatie is in het algemeen voordeliger dan de actieve replicatie omdat in de meeste gevallen kan slechts één bewerking wordt uitgevoerd. Latentie, doorvoer en monetaire kosten zijn identiek aan de niet-gerepliceerde scenario.

Als u passieve replicatie gebruikt, in de volgende scenario's kunnen berichten zoekraken of worden twee keer ontvangen:

* **Bericht vertraging of verlies**: Wordt ervan uitgegaan dat de afzender een m1 bericht naar de primaire wachtrij verzonden en vervolgens de wachtrij niet beschikbaar is voordat de ontvanger m1 ontvangt. De afzender verzendt een m2 daaropvolgende bericht naar de secundaire wachtrij. Als de primaire wachtrij tijdelijk niet beschikbaar is, ontvangt de ontvanger m1 nadat de wachtrij weer beschikbaar wordt. In geval van een ramp ontvangt de ontvanger mogelijk nooit m1.
* **Dubbele ontvangst**: Wordt ervan uitgegaan dat de afzender een bericht m naar de primaire wachtrij verzendt. Service Bus m verwerkt is, maar om een antwoord te verzenden is mislukt. Nadat er is een time-out opgetreden voor de bewerking voor het verzenden, verzendt de afzender een identieke kopie van m naar de secundaire wachtrij. Als de ontvanger kan de eerste kopie van m ontvangen voordat de primaire wachtrij niet beschikbaar is, ontvangt de ontvanger beide exemplaren van m op ongeveer hetzelfde moment. Als de ontvanger niet kan ontvangen van de eerste kopie van m voordat de primaire wachtrij niet beschikbaar is, wordt de ontvanger in eerste instantie ontvangt alleen het tweede exemplaar van m, maar u ontvangt vervolgens een tweede exemplaar van m wanneer de primaire wachtrij beschikbaar komt.

De [Geo-replicatie met Service Bus Standard-laag] [ Geo-replication with Service Bus Standard Tier] voorbeeld laat zien passieve replicatie van berichtentiteiten.

## <a name="protecting-relay-endpoints-against-datacenter-outages-or-disasters"></a>Relay-eindpunten op basis van datacenter storingen of rampen beschermen
Geo-replicatie van de relay-eindpunten kunt een service waarmee wordt aangegeven dat een relay-eindpunt om te worden bereikt met Service Bus-uitval. Als u wilt bereiken geo-replicatie, moet de service twee relay-eindpunten in verschillende naamruimten maken. De naamruimten moet zich bevinden in verschillende datacenters en de twee eindpunten moeten verschillende namen hebben. Bijvoorbeeld, een primaire eindpunt kan worden bereikt onder **contosoPrimary.servicebus.windows.net/myPrimaryService**, terwijl het bijbehorende secundaire server kan worden bereikt onder **contosoSecondary.servicebus.windows.net /mySecondaryService**.

De service vervolgens luistert op beide eindpunten en een client de service via een eindpunt kunt aanroepen. Een clienttoepassing willekeurig kiest een van de relays als het primaire eindpunt en de aanvraag verzendt naar de actieve eindpunt. Als de bewerking is mislukt met een foutcode, wordt deze fout geeft aan dat de relay-eindpunt niet beschikbaar is. De toepassing wordt een kanaal naar het eindpunt van de back-up geopend en databasebron moet worden gestuurd de aanvraag. Op dat moment de back-eindpunten en de actieve rollen overschakelen: de clienttoepassing rekening gehouden met de oude actief eindpunt moet het nieuwe eindpunt voor back-up en het oude back-eindpunt te zijn van het nieuwe eindpunt voor actieve. Als beide mislukken verzenden, de rollen van de twee entiteiten blijven ongewijzigd en een fout geretourneerd.

## <a name="next-steps"></a>Volgende stappen
Voor meer informatie over herstel na noodgevallen, Zie de volgende artikelen:

* [Azure Service Bus Geo-noodherstel](service-bus-geo-dr.md)
* [Bedrijfscontinuïteit voor Azure SQL Database][Azure SQL Database Business Continuity]
* [Flexibele toepassingen ontwerpen voor Azure][Azure resiliency technical guidance]

[Service Bus Authentication]: service-bus-authentication-and-authorization.md
[Partitioned messaging entities]: service-bus-partitioning.md
[Asynchronous messaging patterns and high availability]: service-bus-async-messaging.md#failure-of-service-bus-within-an-azure-datacenter
[BrokeredMessage.MessageId]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_MessageId
[BrokeredMessage.Label]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Label
[Geo-replication with Service Bus Standard Tier]: https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoReplication
[Azure SQL Database Business Continuity]: ../sql-database/sql-database-business-continuity.md
[Azure resiliency technical guidance]: /azure/architecture/resiliency

[1]: ./media/service-bus-outages-disasters/az.png