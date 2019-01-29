---
title: Azure Service Bus Geo-noodherstel | Microsoft Docs
description: Het gebruik van de geografische regio's voor failover en noodherstel kunnen uitvoeren in Azure Service Bus
services: service-bus-messaging
author: axisc
manager: timlt
editor: spelluru
ms.service: service-bus-messaging
ms.topic: article
ms.date: 01/23/2019
ms.author: aschhab
ms.openlocfilehash: 9446bbd4783aaf20f1bc9079ec43f7050274bf11
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/28/2019
ms.locfileid: "55095612"
---
# <a name="azure-service-bus-geo-disaster-recovery"></a>Azure Service Bus Geo-noodherstel

Wanneer volledige Azure-regio's of datacenters (als er geen [beschikbaarheidszones](../availability-zones/az-overview.md) worden gebruikt) treedt downtime, is het essentieel is voor de verwerking van gegevens om te kunnen blijven werken in een andere regio of het datacenter. Als zodanig *Geo-noodherstel* is een belangrijk onderdeel voor dat elke onderneming. Azure Service Bus biedt ondersteuning voor geo-noodherstel op het niveau van de naamruimte.

De functie van het herstel bij Geo-gerelateerde noodgevallen is wereldwijd beschikbaar is voor de Service Bus Premium-SKU. 

>[!NOTE]
> Geo-noodherstel momenteel alleen zorgt ervoor dat de metagegevens (wachtrijen, onderwerpen, abonnementen, Filters) zijn gekopieerd uit de primaire naamruimte voor de secundaire naamruimte in combinatie.

## <a name="outages-and-disasters"></a>Uitval en noodgevallen afhandelen

Het is belangrijk te weten het onderscheid tussen "storingen" en "rampen." 

Een *onderbreking* is tijdelijk ontbreken van Azure Service Bus en kunnen invloed hebben op sommige onderdelen van de service, zoals een berichten-store, of zelfs het gehele datacenter. Echter, nadat het probleem is opgelost, Service Bus weer beschikbaar is. Een storing wordt normaal gesproken niet het verlies van berichten of andere gegevens. Een voorbeeld van een dergelijke onderbreking mogelijk een stroomstoring in het datacenter. Sommige storingen worden alleen korte verbinding verliezen vanwege problemen met de tijdelijke of netwerk. 

Een *na noodgevallen* wordt gedefinieerd als de permanente of langere verlies van een Service Bus-cluster, Azure-regio of datacenter. De regio of het datacenter mogelijk niet weer beschikbaar, kan of mogelijk niet beschikbaar voor uren of dagen kwijt bent. Voorbeelden van dergelijke rampen zijn brand, dat wordt overspoeld of aardbeving. Een ramp die permanent wordt kan leiden tot het verlies van enkele berichten, evenementen of andere gegevens. Echter, in de meeste gevallen moet er zonder verlies van gegevens en berichten kunnen worden hersteld nadat het datacenter een back-up is.

De functie voor het herstel bij Geo-gerelateerde noodgevallen van Azure Service Bus is een oplossing voor noodherstel. De concepten en werkstroom beschreven in dit artikel van toepassing op noodgevallen, en niet op tijdelijke of tijdelijke storingen. Zie voor een gedetailleerde bespreking van herstel na noodgevallen in Microsoft Azure, [in dit artikel](/azure/architecture/resiliency/disaster-recovery-azure-applications).   

## <a name="basic-concepts-and-terms"></a>Eenvoudige concepten en termen

De functie van het herstel na noodgevallen noodherstel metagegevens geïmplementeerd, en is afhankelijk van de primaire en secundaire disaster recovery-naamruimten. Houd er rekening mee dat de Geo-disaster recovery-functie is beschikbaar voor de [Premium-SKU](service-bus-premium-messaging.md) alleen. U hoeft niet te connection string wijzigen, omdat de verbinding is gemaakt via een alias.

De volgende termen worden gebruikt in dit artikel:

-  *Alias*: De naam van een configuratie die u hebt ingesteld. De alias bevat een enkele stabiel volledig FULLY Qualified Domain Name ()-verbindingsreeks. Deze verbindingsreeks alias toepassingen gebruiken voor verbinding met een naamruimte. Met behulp van een alias, zorgt u ervoor dat de verbindingsreeks niet gewijzigd is wanneer de failover wordt geactiveerd.

-  *Primaire/secundaire naamruimte*: De naamruimten die met de alias overeenkomen. De primaire naamruimte ' actief ' en ontvangt berichten (dit kan een bestaande of nieuwe naamruimte zijn). De secundaire naamruimte is 'passieve' en ontvangt geen berichten. De metagegevens tussen beide is synchroon, zodat beide berichten zonder toepassing code of connection string wijzigingen aan te kunnen naadloos worden geaccepteerd. Om ervoor te zorgen dat alleen de actieve naamruimte berichten ontvangt, moet u de alias. 

-  *Metadata*: Entiteiten zoals wachtrijen, onderwerpen en abonnementen; en de bijbehorende eigenschappen van de service die gekoppeld aan de naamruimte zijn. Houd er rekening mee dat alleen de entiteiten en de bijbehorende instellingen automatisch worden gerepliceerd. Berichten worden niet gerepliceerd.

-  *Failover*: Het proces van het activeren van de secundaire naamruimte.

## <a name="setup"></a>Instellen

In de volgende sectie wordt een overzicht van setup koppelen tussen de naamruimten.

![1][]

Het installatieproces is als volgt:

1. Richt een ***primaire*** Service Bus Premium Namespace.

2. Richt een ***secundaire*** Service Bus Premium Namespace in een regio *af van waar de primaire naamruimte is ingericht*. Hiermee kunt u foutisolatie toestaan via verschillende datacenterregio's.

3. Maken van de koppeling tussen de primaire naamruimte en de secundaire naamruimte verkrijgen van de ***alias***.

4. Gebruik de ***alias*** ingeschakeld hebt verkregen in stap 3 om uw clienttoepassingen naar de Geo-DR verbinden met primaire naamruimte. In eerste instantie de alias verwijst naar de primaire naamruimte.

5. [Optioneel] Voeg enige controle om te detecteren of een failover nodig is.

## <a name="failover-flow"></a>Failover-stroom

Een failover handmatig kan worden geactiveerd door de klant (hetzij expliciet via een opdracht of eigendom van bedrijfslogica die wordt geactiveerd de opdracht-client) en nooit door Azure. Dit biedt de klant volledige eigendom en de zichtbaarheid voor de omzetting van de serviceonderbreking op van het Azure-backbone.

![4][]

Na de failover wordt geactiveerd:

1. De ***alias*** verbindingsreeks wordt bijgewerkt om te verwijzen naar de secundaire Premium-naamruimte.

2. Clients (afzenders en ontvangers) wordt automatisch verbinding met de secundaire naamruimte.

3. De bestaande koppeling tussen primaire en secundaire premium-naamruimte is verbroken.

Nadat de failover wordt gestart:

1. Als er een andere storing optreedt, die u wilt mogelijk failover opnieuw uit. Daarom een andere passieve naamruimte instellen en bijwerken van de koppeling. 

2. Berichten van de vorige primaire naamruimte op te halen wanneer deze weer beschikbaar is. Hierna die naamruimte gebruiken voor het regelmatig berichten buiten de geo-herstel-configuratie of verwijder de oude primaire naamruimte.

> [!NOTE]
> Alleen fouten doorsturen semantiek worden ondersteund. In dit scenario, failover en vervolgens opnieuw koppelen aan een nieuwe naamruimte. Failback wordt niet ondersteund. bijvoorbeeld, in een SQL-cluster. 

U kunt failover met bewaking van systemen of met op maat gemaakte bewakingsoplossingen automatiseren. Deze automatisering wordt echter extra planning en werk, die buiten het bereik van dit artikel.

![2][]

## <a name="management"></a>Beheer

Als u een fout hebt; gemaakt bijvoorbeeld, u de verkeerde regio's tijdens de eerste installatie gekoppeld, u kunt de koppeling van de twee naamruimten op elk gewenst moment verbreken. Als u gebruiken van de gekoppelde naamruimten als normale naamruimten wilt, verwijdert u de alias.

## <a name="use-existing-namespace-as-alias"></a>Gebruik van de bestaande naamruimte als alias

Als u een scenario waarin u de verbindingen van producenten en consumenten niet wijzigen hebt, kunt u de naamruimtenaam van uw opnieuw gebruiken als naam van de alias. Zie de [voorbeeldcode op GitHub hier](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoDR/SBGeoDR2/SBGeoDR_existing_namespace_name).

## <a name="samples"></a>Voorbeelden

De [voorbeelden op GitHub](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoDR/SBGeoDR2/) laten zien hoe u instelt en een failover worden gestart. Deze voorbeelden laten zien van de volgende concepten:

- Een voorbeeld van .NET en instellingen die nodig zijn in Azure Active Directory voor het gebruik van Azure Resource Manager met Service Bus, instellen en inschakelen van Geo-noodherstel.
- Stappen die nodig zijn voor het uitvoeren van de voorbeeldcode.
- Het gebruik van een bestaande naamruimte als een alias.
- Stappen voor het Geo-noodherstel via PowerShell of CLI ook worden ingeschakeld.
- [Verzenden en ontvangen](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoDR/TestGeoDR/ConsoleApp1) uit de huidige primaire of secundaire naamruimte met behulp van de alias.

## <a name="considerations"></a>Overwegingen

Houd rekening met de volgende punten moet rekening houden met deze release:

1. In de planning van failover moet u ook rekening houden met de tijd van meerdere factoren. Als u langer dan 15-20 minuten-connectiviteit verliest, wilt u mogelijk de failover te initiëren.

2. Het feit dat er geen gegevens worden gerepliceerd, betekent dat momenteel actieve sessies worden niet gerepliceerd. Detectie van duplicaten en geplande berichten mag bovendien niet werken. Nieuwe sessies, nieuwe geplande berichten en nieuwe duplicaten werkt. 

3. Failover wordt uitgevoerd een complexe gedistribueerde infrastructuur moet [uitgetest](/azure/architecture/resiliency/disaster-recovery-azure-applications#disaster-simulation) ten minste één keer.

4. Synchroniseren van entiteiten kan enige tijd duren, ongeveer 50-100 entiteiten per minuut. Abonnementen en regels ook meegeteld als entiteiten.

## <a name="availability-zones"></a>Beschikbaarheidszones

De Service Bus Premium-SKU biedt ook ondersteuning voor [Beschikbaarheidszones](../availability-zones/az-overview.md), bieden foutgeïsoleerde locaties binnen een Azure-regio.

> [!NOTE]
> De ondersteuning voor Beschikbaarheidszones voor Azure Service Bus Premium is alleen beschikbaar in [Azure-regio's](../availability-zones/az-overview.md#regions-that-support-availability-zones) waar beschikbaarheidszones aanwezig zijn.

U kunt Beschikbaarheidszones inschakelen op nieuwe naamruimten, met behulp van de Azure portal. Service Bus biedt geen ondersteuning voor migratie van bestaande naamruimten. U kunt zoneredundantie niet uitschakelen nadat deze is ingeschakeld op uw naamruimte.

![3][]

## <a name="next-steps"></a>Volgende stappen

- Zie de Geo-noodherstel [hier naslaginformatie over REST API](/rest/api/servicebus/disasterrecoveryconfigs).
- De Geo-noodherstel uitvoeren [op GitHub](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoDR/SBGeoDR2/SBGeoDR2).
- Zie de Geo-noodherstel [voorbeeld waarmee berichten worden verzonden naar een alias](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoDR/TestGeoDR/ConsoleApp1).

Zie voor meer informatie over Service Bus-berichten, de volgende artikelen:

* [Service Bus-wachtrijen, -onderwerpen en -abonnementen](service-bus-queues-topics-subscriptions.md)
* [Aan de slag met Service Bus-wachtrijen](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus-onderwerpen en -abonnementen gebruiken](service-bus-dotnet-how-to-use-topics-subscriptions.md)
* [Rest-API](/rest/api/servicebus/) 

[1]: ./media/service-bus-geo-dr/geodr_setup_pairing.png
[2]: ./media/service-bus-geo-dr/geo2.png
[3]: ./media/service-bus-geo-dr/az.png
[4]: ./media/service-bus-geo-dr/geodr_failover_alias_update.png