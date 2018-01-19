---
title: Azure Service Bus Veelgestelde vragen (FAQ) | Microsoft Docs
description: Antwoorden op enkele veelgestelde vragen over Azure Service Bus.
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: cc75786d-3448-4f79-9fec-eef56c0027ba
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/14/2017
ms.author: sethm
ms.openlocfilehash: ba34938883ee342936b5c7a4568dae5e02684bb2
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="service-bus-faq"></a>Veelgestelde vragen over Service Bus
Dit artikel worden enkele veelgestelde vragen over Microsoft Azure Service Bus. U kunt ook de [Azure ondersteunen Veelgestelde vragen over](http://go.microsoft.com/fwlink/?LinkID=185083) voor algemene Azure-prijzen en -ondersteuning voor informatie.

## <a name="general-questions-about-azure-service-bus"></a>Algemene vragen over Azure Service Bus
### <a name="what-is-azure-service-bus"></a>Wat is Azure Service Bus?
[Azure Service Bus](service-bus-messaging-overview.md) is een asynchrone messaging cloudplatform waarmee u kunt het verzenden van gegevens tussen ontkoppelde systemen. Microsoft biedt deze functie als een service, wat betekent dat u niet wilt hosten van uw eigen hardware om het te gebruiken.

### <a name="what-is-a-service-bus-namespace"></a>Wat is een Service Bus-naamruimte?
Een [naamruimte](service-bus-create-namespace-portal.md) biedt een scoping container voor het adresseren van Service Bus-resources in uw toepassing. Maken van een naamruimte is nodig om het gebruik van Service Bus en is een van de eerste stappen aan de slag.

### <a name="what-is-an-azure-service-bus-queue"></a>Wat is een Azure Service Bus-wachtrij?
Een [Service Bus-wachtrij](service-bus-queues-topics-subscriptions.md) is een entiteit waarin berichten worden opgeslagen. Wachtrijen zijn handig als er meerdere toepassingen of meerdere onderdelen van een gedistribueerde toepassing die met elkaar moeten communiceren. De wachtrij is vergelijkbaar met een distributie-center in dat meerdere producten (berichten) worden ontvangen en vervolgens vanuit die locatie worden verzonden.

### <a name="what-are-azure-service-bus-topics-and-subscriptions"></a>Wat zijn Azure Service Bus-onderwerpen en abonnementen?
Een onderwerp kan worden weergegeven als een wachtrij en wanneer u meerdere abonnementen, wordt een uitgebreidere messaging model; in wezen een een-op-veel-communicatie-hulpprogramma. Dit model publiceren/abonneren (of *pub subitems*) Hiermee wordt een toepassing die u een bericht naar een onderwerp met meerdere abonnementen verzendt hebben dat bericht ontvangen door meerdere toepassingen.

### <a name="what-is-a-partitioned-entity"></a>Wat is een gepartitioneerde entiteit?
Een conventionele wachtrij of onderwerp is verwerkt door een enkel bericht broker en opgeslagen in één berichten-store. Een [gepartitioneerde wachtrij of onderwerp](service-bus-partitioning.md) wordt verwerkt door meerdere bericht beleggingsmakelaars en opgeslagen in meerdere berichten-stores. Dit betekent dat de totale doorvoer van een gepartitioneerde wachtrij of onderwerp niet langer wordt beperkt door de prestaties van een enkel bericht broker of berichten-store. Bovendien weer een tijdelijke onderbreking van berichten-store niet een gepartitioneerde wachtrij of onderwerp niet beschikbaar.

Let op: ordening wordt niet gegarandeerd bij gebruik van gepartitioneerde entiteiten. In het geval dat een partitie niet beschikbaar is, kunt u nog steeds verzenden en ontvangen van berichten uit de andere partities.

## <a name="best-practices"></a>Aanbevolen procedures
### <a name="what-are-some-azure-service-bus-best-practices"></a>Wat zijn enkele aanbevolen procedures van Azure Service Bus?
Zie [Best practices voor verbeterde prestaties met Service Bus] [ Best practices for performance improvements using Service Bus] – dit artikel wordt beschreven hoe u de prestaties te optimaliseren wanneer uitwisseling van berichten.

### <a name="what-should-i-know-before-creating-entities"></a>Wat moet ik weten voordat u entiteiten
De volgende eigenschappen van een wachtrij en onderwerp zijn niet-wijzigbaar. Houd rekening met deze beperking als u uw entiteiten inricht als deze eigenschappen kunnen niet worden gewijzigd zonder dat er een nieuwe entiteit in de vervanging.

* Partitionering
* Sessies
* Detectie van duplicaten
* Snelle entiteit

## <a name="pricing"></a>Prijzen
Deze sectie worden enkele veelgestelde vragen over de Service Bus structuur prijzen.

De [Service Bus-prijzen en facturering](service-bus-pricing-billing.md) artikel wordt uitgelegd dat de facturering meters in Service Bus. Zie voor meer informatie over Service Bus prijzen opties [Service Bus prijsinformatie](https://azure.microsoft.com/pricing/details/service-bus/).

U kunt ook de [Veelgestelde vragen over Azure-ondersteuning](http://go.microsoft.com/fwlink/?LinkID=185083) voor algemene Azure prijsinformatie. 

### <a name="how-do-you-charge-for-service-bus"></a>Hoe u kosten in rekening gebracht voor Service Bus?
Zie voor meer informatie over prijzen voor Service Bus [Service Bus prijsinformatie][Pricing overview]. Naast de prijzen genoteerd, u in rekening worden gebracht voor de bijbehorende gegevensoverdracht voor uitgaande buiten het datacenter waarin de toepassing is ingericht.

### <a name="what-usage-of-service-bus-is-subject-to-data-transfer-what-is-not"></a>Welke informatie over het gebruik van Service Bus is onderworpen aan de overdracht van gegevens? Wat is er geen?
Een overdracht van gegevens binnen een bepaald Azure-regio is opgegeven zonder kosten, evenals binnenkomende gegevensoverdracht. Gegevensoverdracht buiten een regio is onderworpen aan de kosten voor uitgaande die u kunnen vinden [hier](https://azure.microsoft.com/pricing/details/bandwidth/).

### <a name="does-service-bus-charge-for-storage"></a>Service Bus kosten in rekening gebracht voor de opslag?
Nee, Service Bus biedt geen kosten in rekening gebracht voor opslag. Er is echter geen quota voor de maximale hoeveelheid gegevens die kan worden gehandhaafd per wachtrij/onderwerp beperken. Zie de volgende veelgestelde vragen.

## <a name="quotas"></a>Quota

Zie voor een lijst met Service Bus-limieten en quota's, de [overzicht van Service Bus-quota][Quotas overview].

### <a name="does-service-bus-have-any-usage-quotas"></a>Beschikt over Service Bus quota voor gebruik?
Standaard voor een cloud service Microsoft Hiermee stelt u een cumulatieve maandelijkse gebruiksgegevens van die is berekend voor alle abonnementen van de klant. Omdat we begrijpen dat u deze limieten moet mogelijk, u kunt contact opnemen met customer support op elk gewenst moment zodat we kunnen uw behoeften te begrijpen en pas deze limieten op de juiste wijze. Voor Service Bus is de cumulatieve gebruiksquotum 5 miljard berichten per maand.

Terwijl we zich het recht om uit te schakelen van een klantaccount die de quota voor gebruik in een bepaalde maand heeft overschreden behoudt, wij bieden e-mailmelding en meerdere pogingen om contact met een klant voordat u een actie maken. Klanten die meer dan deze quota zijn nog steeds zelf verantwoordelijk voor de kosten die groter is dan de quota.

Net als bij andere services op Azure Service Bus zorgt ervoor dat een set specifieke quota om ervoor te zorgen dat er evenredige gebruik van bronnen. U vindt meer informatie over deze quota in de [overzicht van Service Bus-quota][Quotas overview].

## <a name="troubleshooting"></a>Problemen oplossen
### <a name="what-are-some-of-the-exceptions-generated-by-azure-service-bus-apis-and-their-suggested-actions"></a>Wat zijn enkele van de uitzonderingen die worden gegenereerd door de Azure Service Bus-API's en hun voorgestelde acties?
Zie voor een lijst van mogelijke Service Bus-uitzonderingen [uitzonderingen overzicht][Exceptions overview].

### <a name="what-is-a-shared-access-signature-and-which-languages-support-generating-a-signature"></a>Wat is er een Shared Access Signature en welke talen ondersteuning voor een handtekening te genereren?
Handtekeningen voor gedeelde toegang zijn een verificatiemethode op basis van SHA-256 beveiligde hashes of URI's. Voor informatie over het genereren van uw eigen handtekeningen in het knooppunt, PHP, Java en C\#, Zie de [Shared Access Signatures] [ Shared Access Signatures] artikel.

## <a name="subscription-and-namespace-management"></a>Abonnement en de naamruimte management
### <a name="how-do-i-migrate-a-namespace-to-another-azure-subscription"></a>Hoe Migreer ik een naamruimte met een andere Azure-abonnement

Kunt u een naamruimte van één Azure-abonnement, met behulp van de [Azure-portal](https://portal.azure.com) of PowerShell-opdrachten. Om de bewerking niet uitvoeren, moet de naamruimte al actief zijn. De gebruiker uitvoeren van de opdrachten moet een beheerder zijn op de bron- en doel-abonnementen.

#### <a name="portal"></a>Portal

Volg de instructies voor het gebruik van de Azure-portal voor het Service Bus-naamruimten migreren naar een ander abonnement, [hier](../azure-resource-manager/resource-group-move-resources.md#use-portal). 

#### <a name="powershell"></a>PowerShell

De volgende reeks PowerShell-opdrachten wordt een naamruimte van één Azure-abonnement verplaatst naar een andere. Om deze bewerking niet uitvoeren, de naamruimte moet al actief zijn en de gebruiker die de PowerShell-opdrachten moet een beheerder zijn op de bron- en doel-abonnementen.

```powershell
# Create a new resource group in target subscription
Select-AzureRmSubscription -SubscriptionId 'ffffffff-ffff-ffff-ffff-ffffffffffff'
New-AzureRmResourceGroup -Name 'targetRG' -Location 'East US'

# Move namespace from source subscription to target subscription
Select-AzureRmSubscription -SubscriptionId 'aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa'
$res = Find-AzureRmResource -ResourceNameContains mynamespace -ResourceType 'Microsoft.ServiceBus/namespaces'
Move-AzureRmResource -DestinationResourceGroupName 'targetRG' -DestinationSubscriptionId 'ffffffff-ffff-ffff-ffff-ffffffffffff' -ResourceId $res.ResourceId
```

## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen voor meer informatie over Service Bus:

* [Introducing Azure Service Bus Premium (blogbericht)](http://azure.microsoft.com/blog/introducing-azure-service-bus-premium-messaging/)
* [Introducing Azure Service Bus Premium (Channel9)](https://channel9.msdn.com/Blogs/Subscribe/Introducing-Azure-Service-Bus-Premium-Messaging)
* [Overzicht van Service Bus](service-bus-messaging-overview.md)
* [Overzicht van Azure Service Bus-architectuur](service-bus-fundamentals-hybrid-solutions.md)
* [Aan de slag met Service Bus-wachtrijen](service-bus-dotnet-get-started-with-queues.md)

[Best practices for performance improvements using Service Bus]: service-bus-performance-improvements.md
[Best practices for insulating applications against Service Bus outages and disasters]: service-bus-outages-disasters.md
[Pricing overview]: https://azure.microsoft.com/pricing/details/service-bus/
[Quotas overview]: service-bus-quotas.md
[Exceptions overview]: service-bus-messaging-exceptions.md
[Shared Access Signatures]: service-bus-sas.md
