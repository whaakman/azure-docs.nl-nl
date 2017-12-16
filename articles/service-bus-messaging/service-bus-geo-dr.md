---
title: Azure Service Bus Geo-noodherstel | Microsoft Docs
description: Het gebruik van de geografische regio's voor failover en herstel na noodgevallen in Azure Service Bus uitvoeren
services: service-bus-messaging
documentationcenter: 
author: christianwolf42
manager: timlt
editor: 
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2017
ms.author: sethm
ms.openlocfilehash: fdeb9ba55fc8eade95f6fca88f47dd12aa18a480
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/16/2017
---
# <a name="azure-service-bus-geo-disaster-recovery"></a>Azure Service Bus Geo-noodherstel

Wanneer de volledige Azure-regio's of datacenters (als er geen [beschikbaarheid zones](../availability-zones/az-overview.md) worden gebruikt) krijgen uitvaltijd, het is essentieel voor gegevensverwerking blijven gebruiken in een andere regio of datacenter. Als zodanig *Geo-noodherstel* en *Geo-replicatie* zijn de belangrijkste functies die voor elke onderneming. Azure Service Bus ondersteunt zowel geo-noodherstel als geo-replicatie op het niveau van de naamruimte. 

De Geo-disaster recovery-functie is algemeen beschikbaar is voor de Service Bus Premium-SKU. 

## <a name="outages-and-disasters"></a>Storingen en rampen

Het is belangrijk te weten het onderscheid tussen 'uitval' en "noodsituaties." Een *onderbreking* Azure Service Bus, tijdelijk niet beschikbaar is en een aantal onderdelen van de service, zoals een berichten-store of zelfs het gehele datacenter kunnen beïnvloeden. Echter, nadat het probleem is opgelost, Service Bus weer beschikbaar. Een storing leidt doorgaans niet tot het verlies van berichten of andere gegevens. Een voorbeeld van een dergelijke onderbreking mogelijk een stroomstoring in het datacenter. Sommige storingen zijn alleen korte verbinding verliezen vanwege problemen met de tijdelijke of netwerk. 

Een *na noodgevallen* is gedefinieerd als het permanente of langere verlies van een Service Bus-cluster, Azure-regio of datacenter. De regio of het datacenter mogelijk niet mogelijk beschikbaar opnieuw of voor uren of dagen is mogelijk niet actief. Voorbeelden van dergelijke rampen zijn gestart, die het overspoelen of aardbeving. Een ramp permanente wordt mogelijk het verlies van sommige berichten, gebeurtenissen of andere gegevens. Echter, in de meeste gevallen moet er geen verlies van gegevens en berichten kunnen worden hersteld zodra het datacenter een back-up is.

De Geo-disaster recovery functie van Azure Service Bus is een noodherstel. De concepten en werkstroom beschreven in dit artikel van toepassing op noodherstel scenario's en niet op tijdelijke of tijdelijke storingen. Zie voor een gedetailleerde bespreking van herstel na noodgevallen in Microsoft Azure [in dit artikel](/azure/architecture/resiliency/disaster-recovery-azure-applications).   

## <a name="basic-concepts-and-terms"></a>Basic-concepten en termen

De functie van het herstel na noodgevallen metagegevens noodherstel geïmplementeerd, en is afhankelijk van de primaire en secundaire disaster recovery naamruimten. Houd er rekening mee dat de Geo-disaster recovery functie beschikbaar is voor de [Premium-SKU](service-bus-premium-messaging.md) alleen. U hoeft niet te Breng eventuele wijzigingen van de tekenreeks verbinding als de verbinding is gemaakt met een alias.

De volgende termen worden gebruikt in dit artikel:

-  *Alias*: de naam voor een noodherstelconfiguratie die u hebt ingesteld. De alias bevat een stabiele verbindingstekenreeks van de Fully Qualified Domain Name (FQDN). Toepassingen gebruiken deze verbindingsreeks alias verbinding maken met een naamruimte. 

-  *Primaire en secundaire naamruimte*: de naamruimten die met de alias overeenkomen. De primaire naamruimte ' actief ' en ontvangt berichten (dit kan een bestaande of nieuwe naamruimte zijn). De secundaire naamruimte is 'passieve' en er wordt geen berichten. De metagegevens tussen beide is synchroon, zodat beide berichten zonder toepassing code of connection string wijzigingen naadloos kunnen accepteren. Om ervoor te zorgen dat alleen de actieve naamruimte berichten ontvangt, moet u de alias gebruiken. 

-  *Metagegevens*: entiteiten zoals wachtrijen, onderwerpen en abonnementen; en hun eigenschappen die gekoppeld aan de naamruimte zijn van de service. Houd er rekening mee dat alleen entiteiten en hun instellingen worden automatisch gerepliceerd. Berichten worden niet gerepliceerd. 

-  *Failover*: het proces van het activeren van de secundaire naamruimte.

## <a name="setup-and-failover-flow"></a>Het installatieprogramma en failover-stroom

De volgende sectie wordt een overzicht van de failoverproces en wordt uitgelegd hoe u voor het instellen van de eerste failover. 

![1][]

### <a name="setup"></a>Instellen

U maakt eerst of gebruik een bestaande primaire naamruimte en een nieuwe secundaire naamruimte, en koppel de twee. Deze koppeling geeft u een alias die u kunt verbinding maken. Omdat u een alias gebruiken, kunt u beschikt niet over verbindingsreeksen wijzigen. Alleen nieuwe naamruimten kunnen worden toegevoegd aan uw failover-koppeling. Tot slot moet u toevoegen sommige bewaken om te bepalen of er een failover nodig is. In de meeste gevallen maakt deel uit van een grote ecosysteem van de service, waardoor automatische failovers zelden mogelijk als heel vaak failovers synchroon met de resterende subsysteem of infrastructuur moet worden uitgevoerd.

### <a name="example"></a>Voorbeeld

In een voorbeeld van dit scenario kunt u een punt van verkooppunten (POS)-oplossing die u berichten of gebeurtenissen verzendt. Service Bus geeft de gebeurtenissen die aan bepaalde toewijzings- of herformatteer-oplossing die toegewezen gegevens naar een ander systeem stuurt voor verdere verwerking. Al deze systemen mogelijk op dat moment worden gehost in dezelfde Azure-regio. De beslissing over wanneer en welk onderdeel Failover is afhankelijk van de stroom van gegevens in uw infrastructuur. 

U kunt de failover met het bewaken van systemen of met op maat gemaakte bewakingsoplossingen automatiseren. Dergelijke automation heeft echter extra planning en werk, die buiten het bereik van dit artikel.

### <a name="failover-flow"></a>Failoverstroom

Als u de failover start, zijn twee stappen vereist:

1. Als er een andere storing optreedt, die u wilt worden opnieuw failover mogelijk is. Daarom instellen van een andere passieve naamruimte en werk de koppeling. 

2. Berichten van de oude primaire naamruimte ophalen zodra deze weer beschikbaar is. Hierna is waarmee de naamruimte voor reguliere messaging buiten uw installatie van de geo-recovery, of verwijder de oude primaire naamruimte.

> [!NOTE]
> Alleen fouten doorsturen semantiek worden ondersteund. In dit scenario, failover en opnieuw koppelen aan een nieuwe naamruimte. Mislukte terug wordt niet ondersteund; bijvoorbeeld in een SQL-cluster. 

![2][]

## <a name="management"></a>Beheer

Als u onrechte ten; bijvoorbeeld, u de verkeerde regio's gekoppeld tijdens de eerste installatie, verbreekt u de koppeling van de twee naamruimten op elk gewenst moment. Als u gebruiken van de naamruimten die zijn gekoppeld als reguliere naamruimten wilt, verwijdert u de alias.

## <a name="use-existing-namespace-as-alias"></a>Bestaande naamruimte als alias gebruiken

Als u een scenario waarin u de verbindingen van producenten en consumenten niet wijzigen hebt, kunt u de naamruimtenaam van uw hergebruiken als aliasnaam. Zie de [voorbeeldcode op GitHub hier](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoDR/SBGeoDR2/SBGeoDR_existing_namespace_name).

## <a name="samples"></a>Voorbeelden

De [voorbeelden op GitHub](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoDR/SBGeoDR2/SBGeoDR2) laten zien hoe u instelt en start een failover. Deze voorbeelden laten zien dat de volgende concepten:

- Instellingen voor het gebruik van Azure Resource Manager met Service Bus in Azure Active Directory vereist. 
- Stappen voor het uitvoeren van de voorbeeldcode. 
- Verzenden en ontvangen van de huidige primaire naamruimte. 
- Het gebruik van een bestaande naamruimte als alias.

## <a name="considerations"></a>Overwegingen

Houd rekening met de volgende overwegingen rekening houden met deze versie:

1. Overweeg ook de tijd factor in de planning van failover. Als u connectiviteit langer dan 15-20 minuten verliest, wilt u mogelijk start de failover. 
 
2. Het feit dat er geen gegevens worden gerepliceerd betekent dat momenteel actieve sessies worden niet gerepliceerd. Bovendien detectie van duplicaten en geplande berichten werkt mogelijk niet. Nieuwe sessies, geplande berichten en nieuwe duplicaten werkt. 

3. Mislukte via een complexe gedistribueerde infrastructuur moet [uitgetest](/azure/architecture/resiliency/disaster-recovery-azure-applications#disaster-simulation) ten minste één keer. 

4. Synchroniseren van entiteiten kan enige tijd duren, ongeveer 50-100 entiteiten per minuut. Abonnementen en regels tellen ook als entiteiten. 

## <a name="next-steps"></a>Volgende stappen

- Zie de Geo-noodherstel [hier naslaginformatie over REST API](/rest/api/servicebus/disasterrecoveryconfigs).
- Voer de Geo-noodherstel [op GitHub](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoDR/SBGeoDR2/SBGeoDR2).
- Zie de Geo-noodherstel [voorbeeldtoepassing die u berichten naar een alias verzendt](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoDR/TestGeoDR/ConsoleApp1).

Zie voor meer informatie over Service Bus-berichtenservice, de volgende artikelen:

* [Grondbeginselen van Service Bus](service-bus-fundamentals-hybrid-solutions.md)
* [Service Bus-wachtrijen, -onderwerpen en -abonnementen](service-bus-queues-topics-subscriptions.md)
* [Aan de slag met Service Bus-wachtrijen](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus-onderwerpen en -abonnementen gebruiken](service-bus-dotnet-how-to-use-topics-subscriptions.md)
* [Rest-API](/rest/api/servicebus/) 

[1]: ./media/service-bus-geo-dr/geo1.png
[2]: ./media/service-bus-geo-dr/geo2.png
