---
title: Azure Service Bus Veelgestelde vragen (FAQ) | Microsoft Docs
description: Antwoorden op enkele veelgestelde vragen over Azure Service Bus.
services: service-bus-messaging
author: spelluru
manager: timlt
ms.service: service-bus-messaging
ms.topic: article
ms.date: 09/05/2018
ms.author: spelluru
ms.openlocfilehash: 87a6ccf61e10a6a8c7feeccad3690cdcb9b63641
ms.sourcegitcommit: 67abaa44871ab98770b22b29d899ff2f396bdae3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/08/2018
ms.locfileid: "48857853"
---
# <a name="service-bus-faq"></a>Veelgestelde vragen over Service Bus

Dit artikel worden enkele veelgestelde vragen over Microsoft Azure Service Bus. U kunt ook de [ondersteuning voor veelgestelde vragen over Azure](https://azure.microsoft.com/en-us/support/faq/) voor algemene Azure-prijzen en ondersteunende informatie.

## <a name="general-questions-about-azure-service-bus"></a>Algemene vragen over Azure Service Bus
### <a name="what-is-azure-service-bus"></a>Wat is Azure Service Bus?
[Azure Service Bus](service-bus-messaging-overview.md) is een asynchrone messaging cloudplatform waarmee u voor het verzenden van gegevens tussen systemen met elkaar zijn losgekoppeld. Microsoft biedt deze functie als een service, wat betekent dat u niet hoeft voor het hosten van uw eigen hardware om het te gebruiken.

### <a name="what-is-a-service-bus-namespace"></a>Wat is een Service Bus-naamruimte?
Een [naamruimte](service-bus-create-namespace-portal.md) biedt een scoping container voor het verwerken van Service Bus-resources in uw toepassing. Het maken van een naamruimte is nodig om het gebruik van Service Bus en is een van de eerste stappen aan de slag.

### <a name="what-is-an-azure-service-bus-queue"></a>Wat is een Azure Service Bus-wachtrij?
Een [Service Bus-wachtrij](service-bus-queues-topics-subscriptions.md) is een entiteit waarin berichten worden opgeslagen. Wachtrijen zijn handig als u meerdere toepassingen of meerdere onderdelen van een gedistribueerde toepassing die nodig hebt om te communiceren met elkaar hebt. De wachtrij is vergelijkbaar met een distributie-center in dat meerdere producten (berichten) worden ontvangen en vanaf die locatie verzonden.

### <a name="what-are-azure-service-bus-topics-and-subscriptions"></a>Wat zijn Azure Service Bus-onderwerpen en abonnementen?
Een onderwerp als een wachtrij kan worden gevisualiseerd en wanneer u meerdere abonnementen gebruikt, wordt deze een uitgebreidere messaging-model. in feite een een-op-veel communicatie-hulpprogramma. Dit model publiceren/abonneren (of *pub/sub*) kan een toepassing die u een bericht naar een onderwerp met meerdere abonnementen verzendt hebben dat bericht ontvangen door meerdere toepassingen.

### <a name="what-is-a-partitioned-entity"></a>Wat is een gepartitioneerde eenheid?
Een conventionele wachtrij of onderwerp is afgehandeld door een enkel bericht-broker en die zijn opgeslagen in één berichten-store. Alleen ondersteund in de Basic en Standard messaging-lagen, een [gepartitioneerde wachtrij of onderwerp](service-bus-partitioning.md) wordt verwerkt door meerdere berichtenbrokers en opgeslagen in meerdere berichten-stores. Deze functie betekent dat de algehele doorvoer van een gepartitioneerde wachtrij of onderwerp niet meer op basis van de prestaties van één bericht-broker of berichtenarchief wordt beperkt. Bovendien weer een tijdelijke onderbreking van een berichten-store niet een gepartitioneerde wachtrij of onderwerp niet beschikbaar.

Volgorde wordt niet gegarandeerd wanneer met behulp van entiteiten gepartitioneerde. In het geval dat een partitie niet beschikbaar is, kunt u nog steeds verzenden en ontvangen van berichten van de andere partities.

 Gepartitioneerde entiteiten worden niet meer ondersteund de [Premium-SKU](service-bus-premium-messaging.md). 

## <a name="best-practices"></a>Aanbevolen procedures
### <a name="what-are-some-azure-service-bus-best-practices"></a>Wat zijn enkele aanbevolen procedures voor Azure Service Bus?
Zie [aanbevolen procedures voor prestatieverbeteringen met Service Bus] [ Best practices for performance improvements using Service Bus] : in dit artikel wordt beschreven hoe u het optimaliseren van tijdens het uitwisselen van berichten.

### <a name="what-should-i-know-before-creating-entities"></a>Wat moet ik weten voordat u het maken van entiteiten
De volgende eigenschappen van een wachtrij en onderwerp zijn onveranderd. Rekening houden met deze beperking bij het inrichten van uw entiteiten, zoals deze eigenschappen kunnen niet worden gewijzigd zonder dat er een nieuwe entiteit in de vervanging wordt gemaakt.

* Partitionering
* Sessies
* Detectie van duplicaten
* Snelle entiteit

## <a name="pricing"></a>Prijzen
In deze sectie vindt u antwoorden op enkele veelgestelde vragen over Service Bus prijsstructuur.

De [Service Bus prijzen en facturering](service-bus-pricing-billing.md) artikel wordt uitgelegd voor de factureringsmeters in Service Bus. Zie voor meer informatie over Service Bus prijsopties [Service Bus prijsinformatie](https://azure.microsoft.com/pricing/details/service-bus/).

U kunt ook de [Veelgestelde vragen over Azure-ondersteuning](https://azure.microsoft.com/en-us/support/faq/) voor algemene Azure informatie over prijzen. 

### <a name="how-do-you-charge-for-service-bus"></a>Hoe u kosten in rekening gebracht voor Service Bus?
Zie voor meer informatie over de prijzen van Service Bus [Service Bus prijsinformatie][Pricing overview]. Naast de vermelde prijzen in rekening gebracht voor gegevensoverdracht van de bijbehorende gegevens voor uitgaande gegevens buiten het datacenter waarin uw toepassing is ingericht.

### <a name="what-usage-of-service-bus-is-subject-to-data-transfer-what-is-not"></a>Wat het gebruik van Service Bus is onderworpen aan de overdracht van gegevens? Wat is er niet?
Alle gegevensoverdracht binnen een bepaald Azure-regio is opgegeven zonder kosten, en alle inkomende gegevensoverdracht. Overdracht van gegevens buiten een regio is onderworpen aan kosten voor uitgaand verkeer, die kunnen worden gevonden [hier](https://azure.microsoft.com/pricing/details/bandwidth/).

### <a name="does-service-bus-charge-for-storage"></a>Service Bus in rekening gebracht voor opslag?
Nee, Service Bus niet in rekening voor opslag. Er is echter een beperking van de maximale hoeveelheid gegevens die kunnen worden vastgehouden per wachtrij/onderwerp van het quotum. Zie de volgende veelgestelde vragen.

## <a name="quotas"></a>Quota

Zie voor een lijst van Service Bus-limieten en quota's, de [overzicht van Service Bus-quota][Quotas overview].

### <a name="does-service-bus-have-any-usage-quotas"></a>Beschikt over Service Bus quota's die gebruik?
Standaard voor elke cloud service Microsoft Hiermee stelt u een cumulatieve quotum voor het maken van maandelijkse gebruik dat wordt berekend voor alle abonnementen van de klant. Als u meer dan deze limieten nodig hebt, kunt u contact opnemen met customer support op elk gewenst moment te begrijpen van uw behoeften deze limieten op de juiste wijze aanpassen. Voor Service Bus is het quotum voor het totale gebruik 5 miljard berichten per maand.

Microsoft behoudt zich het recht om uit te schakelen van een klantaccount die de quota voor gebruik in een bepaalde maand heeft overschreden, e-mailmeldingen worden verzonden en meerdere pogingen zijn aangebracht aan het contact maken met een klant voordat een actie te ondernemen. Klanten van meer dan deze quota zijn nog steeds verantwoordelijk is voor de kosten die de quota overschrijdt.

Net als bij andere services op Azure, wordt een reeks specifieke quota om ervoor te zorgen dat er geoorloofd gebruik van bronnen afgedwongen door Service Bus. U vindt meer informatie over deze quota's in de [overzicht van Service Bus-quota][Quotas overview].

## <a name="troubleshooting"></a>Problemen oplossen
### <a name="what-are-some-of-the-exceptions-generated-by-azure-service-bus-apis-and-their-suggested-actions"></a>Wat zijn enkele van de uitzonderingen die worden gegenereerd door Azure Service Bus-API's en hun voorgestelde acties?
Zie voor een lijst van mogelijke Service Bus-uitzonderingen, [uitzonderingen overzicht][Exceptions overview].

### <a name="what-is-a-shared-access-signature-and-which-languages-support-generating-a-signature"></a>Wat is er een handtekening voor gedeelde toegang en welke talen ondersteuning voor het genereren van een handtekening?
Shared Access Signatures zijn een verificatiemechanisme voor op basis van SHA-256 beveiligde hashes of URI's. Voor informatie over het genereren van uw eigen handtekeningen in Node, PHP, Java en C\#, Zie de [Shared Access Signatures] [ Shared Access Signatures] artikel.

## <a name="subscription-and-namespace-management"></a>Beheer van abonnementen en -naamruimte
### <a name="how-do-i-migrate-a-namespace-to-another-azure-subscription"></a>Hoe Migreer ik een naamruimte met een andere Azure-abonnement?

U kunt een naamruimte uit een Azure-abonnement naar de andere verplaatsen met behulp van de [Azure-portal](https://portal.azure.com) of PowerShell-opdrachten. Als u wilt de bewerking niet uitvoeren, moet de naamruimte al actief zijn. De gebruiker de opdrachten uitvoert, moet een beheerder zijn op zowel de bron- en -abonnementen.

#### <a name="portal"></a>Portal

Volg de aanwijzingen voor het gebruik van de Azure-portal voor het migreren van Service Bus-naamruimten naar een ander abonnement, [hier](../azure-resource-manager/resource-group-move-resources.md#use-portal). 

#### <a name="powershell"></a>PowerShell

De volgende volgorde van de PowerShell-opdrachten wordt een naamruimte uit een Azure-abonnement verplaatst naar een andere. Voor het uitvoeren van deze bewerking, de naamruimte moet al actief zijn en de gebruiker die de PowerShell-opdrachten moet een beheerder zijn op zowel de bron- en -abonnementen.

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
* [Introducing Azure Service Bus Premium (Channel 9)](https://channel9.msdn.com/Blogs/Subscribe/Introducing-Azure-Service-Bus-Premium-Messaging)
* [Overzicht van Service Bus](service-bus-messaging-overview.md)
* [Aan de slag met Service Bus-wachtrijen](service-bus-dotnet-get-started-with-queues.md)

[Best practices for performance improvements using Service Bus]: service-bus-performance-improvements.md
[Best practices for insulating applications against Service Bus outages and disasters]: service-bus-outages-disasters.md
[Pricing overview]: https://azure.microsoft.com/pricing/details/service-bus/
[Quotas overview]: service-bus-quotas.md
[Exceptions overview]: service-bus-messaging-exceptions.md
[Shared Access Signatures]: service-bus-sas.md
