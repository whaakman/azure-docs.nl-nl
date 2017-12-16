---
title: Azure Event Hubs geo-disaster recovery | Microsoft Docs
description: Het gebruik van de geografische regio's voor failover en herstel na noodgevallen uitvoeren in Azure Event Hubs
services: event-hubs
documentationcenter: 
author: sethmanheim
manager: timlt
editor: 
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2017
ms.author: sethm
ms.openlocfilehash: 237b0639be75e12cff56f40ac76426aba7a8a701
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/16/2017
---
# <a name="azure-event-hubs-geo-disaster-recovery"></a>Azure Event Hubs Geo-noodherstel

Wanneer de volledige Azure-regio's of datacenters (als er geen [beschikbaarheid zones](../availability-zones/az-overview.md) worden gebruikt) krijgen uitvaltijd, het is essentieel voor gegevensverwerking blijven gebruiken in een andere regio of datacenter. Als zodanig *Geo-noodherstel* en *Geo-replicatie* zijn de belangrijkste functies die voor elke onderneming. Azure Event Hubs ondersteunt zowel geo-noodherstel als geo-replicatie op het niveau van de naamruimte. 

De Geo-disaster recovery-functie is algemeen beschikbaar is voor de standaard SKU van Event Hubs.

## <a name="outages-and-disasters"></a>Storingen en rampen

Het is belangrijk te weten het onderscheid tussen 'uitval' en "noodsituaties." Een *onderbreking* Azure Event Hubs, tijdelijk niet beschikbaar is en een aantal onderdelen van de service, zoals een berichten-store of zelfs het gehele datacenter kunnen beïnvloeden. Echter, nadat het probleem is opgelost, Event Hubs weer beschikbaar. Een storing leidt doorgaans niet tot het verlies van berichten of andere gegevens. Een voorbeeld van een dergelijke onderbreking mogelijk een stroomstoring in het datacenter. Sommige storingen zijn alleen korte verbinding verliezen vanwege problemen met de tijdelijke of netwerk. 

Een *na noodgevallen* is gedefinieerd als de permanente of langere verlies van een cluster Event Hubs, Azure-regio of datacenter. De regio of het datacenter mogelijk niet mogelijk beschikbaar opnieuw of voor uren of dagen is mogelijk niet actief. Voorbeelden van dergelijke rampen zijn gestart, die het overspoelen of aardbeving. Een ramp permanente wordt mogelijk het verlies van sommige berichten, gebeurtenissen of andere gegevens. Echter, in de meeste gevallen moet er geen verlies van gegevens en berichten kunnen worden hersteld zodra het datacenter een back-up is.

De Geo-disaster recovery functie van Azure Event Hubs is een noodherstel. De concepten en werkstroom beschreven in dit artikel van toepassing op noodherstel scenario's en niet op tijdelijke of tijdelijke storingen. Zie voor een gedetailleerde bespreking van herstel na noodgevallen in Microsoft Azure [in dit artikel](/azure/architecture/resiliency/disaster-recovery-azure-applications).

## <a name="basic-concepts-and-terms"></a>Basic-concepten en termen

De functie van het herstel na noodgevallen metagegevens noodherstel geïmplementeerd, en is afhankelijk van de primaire en secundaire disaster recovery naamruimten. Houd er rekening mee dat de Geo-disaster recovery functie beschikbaar is voor de [standaard SKU](https://azure.microsoft.com/pricing/details/event-hubs/) alleen. U hoeft niet te Breng eventuele wijzigingen van de tekenreeks verbinding als de verbinding is gemaakt met een alias.

De volgende termen worden gebruikt in dit artikel:

-  *Alias*: de naam voor een noodherstelconfiguratie die u hebt ingesteld. De alias bevat een stabiele verbindingstekenreeks van de Fully Qualified Domain Name (FQDN). Toepassingen gebruiken deze verbindingsreeks alias verbinding maken met een naamruimte. 

-  *Primaire en secundaire naamruimte*: de naamruimten die met de alias overeenkomen. De primaire naamruimte ' actief ' en ontvangt berichten (dit kan een bestaande of nieuwe naamruimte zijn). De secundaire naamruimte is 'passieve' en er wordt geen berichten. De metagegevens tussen beide is synchroon, zodat beide berichten zonder toepassing code of connection string wijzigingen naadloos kunnen accepteren. Om ervoor te zorgen dat alleen de actieve naamruimte berichten ontvangt, moet u de alias gebruiken. 

-  *Metagegevens*: entiteiten zoals event hubs en consumergroepen; en hun eigenschappen die gekoppeld aan de naamruimte zijn van de service. Houd er rekening mee dat alleen entiteiten en hun instellingen worden automatisch gerepliceerd. Berichten en gebeurtenissen worden niet gerepliceerd. 

-  *Failover*: het proces van het activeren van de secundaire naamruimte.

## <a name="setup-and-failover-flow"></a>Het installatieprogramma en failover-stroom

De volgende sectie wordt een overzicht van de failoverproces en wordt uitgelegd hoe u voor het instellen van de eerste failover. 

![1][]

### <a name="setup"></a>Instellen

U maakt eerst of gebruik een bestaande primaire naamruimte en een nieuwe secundaire naamruimte, en koppel de twee. Deze koppeling geeft u een alias die u kunt verbinding maken. Omdat u een alias gebruiken, kunt u beschikt niet over verbindingsreeksen wijzigen. Alleen nieuwe naamruimten kunnen worden toegevoegd aan uw failover-koppeling. Tot slot moet u toevoegen sommige bewaken om te bepalen of er een failover nodig is. In de meeste gevallen maakt deel uit van een grote ecosysteem van de service, waardoor automatische failovers zelden mogelijk als heel vaak failovers synchroon met de resterende subsysteem of infrastructuur moet worden uitgevoerd.

### <a name="example"></a>Voorbeeld

In een voorbeeld van dit scenario kunt u een punt van verkooppunten (POS)-oplossing die u berichten of gebeurtenissen verzendt. Event Hubs geeft de gebeurtenissen die aan bepaalde oplossing koppelen of opnieuw formatteren, waarbij stuurt toegewezen gegevens naar een ander systeem voor verdere verwerking. Al deze systemen mogelijk op dat moment worden gehost in dezelfde Azure-regio. De beslissing over wanneer en welk onderdeel Failover is afhankelijk van de stroom van gegevens in uw infrastructuur. 

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

## <a name="samples"></a>Voorbeelden

De [op GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/GeoDRClient) laat zien hoe u instelt en initieer een failover. Dit voorbeeld ziet u de volgende concepten:

- Instellingen voor het gebruik van Azure Resource Manager met Event Hubs in Azure Active Directory vereist. 
- Stappen voor het uitvoeren van de voorbeeldcode. 
- Verzenden en ontvangen van de huidige primaire naamruimte. 

## <a name="considerations"></a>Overwegingen

Houd rekening met de volgende overwegingen rekening houden met deze versie:

1. Overweeg ook de tijd factor in de planning van failover. Als u connectiviteit langer dan 15-20 minuten verliest, wilt u mogelijk start de failover. 
 
2. Het feit dat er geen gegevens worden gerepliceerd betekent dat momenteel actieve sessies worden niet gerepliceerd. Bovendien detectie van duplicaten en geplande berichten werkt mogelijk niet. Nieuwe sessies, geplande berichten en nieuwe duplicaten werkt. 

3. Mislukte via een complexe gedistribueerde infrastructuur moet [uitgetest](/azure/architecture/resiliency/disaster-recovery-azure-applications#disaster-simulation) ten minste één keer. 

4. Synchroniseren van entiteiten kan enige tijd duren, ongeveer 50-100 entiteiten per minuut.

## <a name="next-steps"></a>Volgende stappen

* De [op GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/GeoDRClient) wordt begeleid bij een eenvoudige werkstroom die u maakt een geo-koppeling en start een failover voor een noodherstelscenario.
* De [naslaginformatie over REST API](/rest/api/eventhub/disasterrecoveryconfigs) API's voor het uitvoeren van de Geo noodherstelconfiguratie beschrijft.

Voor meer informatie over Event Hubs gaat u naar de volgende koppelingen:

* Aan de slag met een [Event Hubs-zelfstudie](event-hubs-dotnet-standard-getstarted-send.md)
* [Veelgestelde vragen over Event Hubs](event-hubs-faq.md)
* [Voorbeeldtoepassingen die gebruikmaken van Event Hubs](https://github.com/Azure/azure-event-hubs/tree/master/samples)

[1]: ./media/event-hubs-geo-dr/geo1.png
[2]: ./media/event-hubs-geo-dr/geo2.png