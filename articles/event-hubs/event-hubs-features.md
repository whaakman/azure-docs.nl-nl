---
title: Overzicht van Azure Event Hubs-functies | Microsoft Docs
description: Overzicht en meer informatie over de functies van Azure Event Hubs
services: event-hubs
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/17/2017
ms.author: sethm
ms.openlocfilehash: aa9fc3b03e24d0b4d1a7ecd9a945b67d8d182492
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/18/2017
---
# <a name="event-hubs-features-overview"></a>Overzicht van Event Hubs-functies

Azure Event Hubs is een schaalbare verwerkingsservice die opgenomen en grote hoeveelheden gebeurtenissen en de gegevens, verwerkt met een lage latentie en hoge betrouwbaarheid. Zie [wat is er Event Hubs?](event-hubs-what-is-event-hubs.md) voor een overzicht van de service.

In dit artikel is gebaseerd op de informatie in de [overzichtsartikel](event-hubs-what-is-event-hubs.md), en biedt technische en implementatie-informatie over Event Hubs-onderdelen en functies.

## <a name="event-publishers"></a>Gebeurtenisuitgevers

Een entiteit waarmee gegevens worden verzonden naar een event hub is een producent gebeurtenis of *gebeurtenisuitgever*. Gebeurtenisuitgevers kunnen gebeurtenissen publiceren met HTTPS of AMQP 1.0. Gebeurtenisuitgevers gebruiken een Shared Access Signature-token (SAS) om zichzelf te identificeren bij een Event Hub en kunnen een unieke identiteit hebben of een algemene SAS-token gebruiken.

### <a name="publishing-an-event"></a>Een gebeurtenis publiceren

U kunt een gebeurtenis publiceren met AMQP 1.0 of HTTPS. Event Hubs biedt [clientbibliotheken en klassen](event-hubs-dotnet-framework-api-overview.md) voor het publiceren van gebeurtenissen naar een event hub vanaf .NET-clients. Voor andere runtimes en platforms kunt u een AMQP 1.0-client gebruiken, zoals [Apache Qpid](http://qpid.apache.org/). U kunt gebeurtenissen afzonderlijk of batchgewijs publiceren. Eén publicatie (exemplaar met gebeurtenisgegevens) heeft een limiet van 256 kB, ongeacht of het om één gebeurtenis of om een batch gaat. Het publiceren van gebeurtenissen die groter is dan deze drempelwaarde resultaten in een fout. Het is voor uitgevers een best practice om niets te weten over de partities binnen Event Hub en alleen een *partitiesleutel* (zie volgende sectie) of hun identiteit via de SAS-token op te geven.

De keuze om AMQP of HTTPS te gebruiken, geldt specifiek voor het gebruiksscenario. AMQP vereist de inrichting van een permanente bidirectionele socket naast Transport Layer Security (TLS) of SSL/TLS. AMQP gaat gepaard met hogere netwerkkosten tijdens de initialisatie van de sessie. Voor HTTPS is echter extra SSL-overhead vereist voor elke aanvraag. AMQP biedt betere prestaties voor regelmatige uitgevers.

![Event Hubs](./media/event-hubs-features/partition_keys.png)

Event Hubs zorgt ervoor dat alle gebeurtenissen met een partitiesleutelwaarde op volgorde en aan dezelfde partitie worden geleverd. De identiteit van de uitgever en de waarde van de partitiesleutel moeten overeenkomen als er partitiesleutels met uitgeversbeleid worden gebruikt. Als deze niet overeenkomen, treedt er een fout op.

### <a name="publisher-policy"></a>Uitgeversbeleid

In Event Hubs kunt u gebeurtenisuitgevers nauwkeurig beheren met behulp van *uitgeversbeleid*. Uitgeversbeleid bestaat uit runtimefuncties die zijn ontworpen om grote aantallen onafhankelijke gebeurtenisuitgevers mogelijk te maken. Als u uitgeversbeleid implementeert, gebruikt elke uitgever zijn eigen unieke id bij het publiceren van gebeurtenissen naar een Event Hub. Hierbij wordt het volgende mechanisme gebruikt:

```
//[my namespace].servicebus.windows.net/[event hub name]/publishers/[my publisher name]
```

Het is niet nodig om van tevoren uitgeversnamen te maken. De namen moeten echter wel overeenkomen met het SAS-token dat wordt gebruikt wanneer een gebeurtenis wordt gepubliceerd. Hiermee wordt voor onafhankelijke uitgeversidentiteiten gezorgd. Als u uitgeversbeleid gebruikt, is de waarde **Partitiesleutel** ingesteld op de naam van de uitgever. Voor een goede werking moeten deze waarden overeenkomen.

## <a name="capture"></a>Capture

[Event Hubs vastleggen](event-hubs-capture-overview.md) kunt u automatisch de streaming-gegevens in Event Hubs vastleggen en opslaan op uw keuze van een Blob storage-account of een Azure Data Lake-Service-account. U kunt vastleggen inschakelen via de Azure-portal en geef een minimale grootte en tijdvenster om uit te voeren van het vastleggen. Met Event Hubs vastleggen opgeven u uw eigen Azure Blob Storage-account en een container of een Azure Data Lake-Service-account wordt gebruikt voor het opslaan van de vastgelegde gegevens. Gegevensopname is geschreven in de Apache Avro-indeling.

## <a name="partitions"></a>Partities

Event Hubs daarentegen biedt streaming van berichten via een model op basis van gepartitioneerd gebruik, waarbij elke consumer slechts een specifieke subset of partitie van de berichtenstroom leest. Dit patroon maakt een horizontale schaal voor de verwerking van gebeurtenissen mogelijk en biedt andere stroomgerichte functies die niet beschikbaar zijn in wachtrijen en onderwerpen.

Een partitie is een geordende reeks gebeurtenissen die in een Event Hub wordt bewaard. Als er nieuwere gebeurtenissen plaatsvinden, worden deze toegevoegd aan het einde van deze reeks. Een partitie kan worden beschouwd als een 'doorvoerlogboek'.

![Event Hubs](./media/event-hubs-features/partition.png)

Event Hubs behoudt gegevens voor een geconfigureerde bewaartijd die geldt voor alle partities in de event hub. Gebeurtenissen verlopen op basis van tijd. U kunt ze niet expliciet verwijderen. Omdat partities onafhankelijk zijn en hun eigen reeks gegevens bevatten, groeien ze vaak met verschillende snelheden.

![Event Hubs](./media/event-hubs-features/multiple_partitions.png)

Het aantal partities wordt opgegeven bij het maken en moet tussen 2 en 32 liggen. Het aantal partities kan niet worden gewijzigd. Houd bij het instellen van het aantal partities dus uw doelen op de lange termijn in gedachten. Partities zijn een mechanisme voor gegevensordening. Ze hebben betrekking op de mate van downstreamparallelheid die is vereist bij het gebruik van toepassingen. Het aantal partities in een Event Hub houdt rechtstreeks verband met het aantal verwachte gelijktijdige lezers. Neem contact op met het team van Event Hubs als u meer dan 32 partities wilt maken.

Terwijl partities te herkennen en rechtstreeks naar kunnen worden verzonden, wordt het verzenden van rechtstreeks naar een partitie niet aanbevolen. In plaats daarvan kunt u constructies op een hoger niveau gebruiken. Deze vindt u in de secties [Gebeurtenisuitgever](#event-publishers) en [Capaciteit](#capacity). 

Partities bestaan uit een reeks gebeurtenisgegevens. Deze bevatten de hoofdtekst van de gebeurtenis, een door de gebruiker gedefinieerde eigenschappenverzameling en metagegevens, zoals de offset in de partitie en het nummer in de stroomreeks.

Zie de artikelen [Programmeergids voor Event Hubs](event-hubs-programming-guide.md#partition-key) en [Beschikbaarheid en consistentie in Event Hubs](event-hubs-availability-and-consistency.md) voor meer informatie over partities en de verhouding tussen de beschikbaarheid en betrouwbaarheid.

### <a name="partition-key"></a>Partitiesleutel

U kunt een [partitiesleutel](event-hubs-programming-guide.md#partition-key) gebruiken om inkomende gebeurtenisgegevens toe te wijzen aan specifieke partities, zodat de gegevens kunnen worden geordend. De partitiesleutel is een door de afzender opgegeven waarde die aan een Event Hub wordt doorgegeven. De partitiesleutel wordt verwerkt door een statische hash-functie, die zorgt voor de partitietoewijzing. Als u bij het publiceren van een gebeurtenis geen partitiesleutel opgeeft, wordt er gebruikgemaakt van round robin-toewijzing.

De gebeurtenisuitgever is alleen op de hoogte van de partitiesleutel en niet van de partitie waarop de gebeurtenissen worden gepubliceerd. Deze ontkoppeling van sleutel en partitie schermt de afzender af, zodat deze niet te veel te weten hoeft te komen over de downstreamverwerking. Goede partitiesleutels zijn bijvoorbeeld een apparaatspecifieke of een gebruikersspecifieke identiteit, maar voor het groeperen van gerelateerde gebeurtenissen in dezelfde partitie kunnen ook andere kenmerken, zoals geografie, worden gebruikt.

## <a name="sas-tokens"></a>SAS-tokens

Event Hubs gebruikt *Shared Access Signatures* die beschikbaar zijn op het niveau van de naamruimte en Event Hub. Een SAS-token wordt gegenereerd uit een SAS-sleutel en is een SHA-hash of URL. gecodeerd in een specifieke indeling. Event Hubs kan de hash opnieuw genereren met de naam van de sleutel (het beleid) en de token, en op deze manier de afzender verifiëren. Normaal gesproken worden SAS-tokens voor gebeurtenisuitgevers alleen gemaakt met bevoegdheden voor **verzenden** voor een specifieke Event Hub. Dit URL-mechanisme met SAS-token vormt de basis voor de uitgeversidentificatie die in het uitgeversbeleid wordt geïntroduceerd. Zie [Shared Access Signature-verificatie met Service Bus](../service-bus-messaging/service-bus-sas.md) voor meer informatie over werken met SAS.

## <a name="event-consumers"></a>Gebeurtenisconsumers

Elke entiteit die gebeurtenisgegevens van een Event Hub leest, is een *gebeurtenisconsumer*. Alle Event Hubs-consumers maken verbinding via de AMQP 1.0-sessie, waarin gebeurtenissen worden geleverd zodra deze beschikbaar komen. De client hoeft de beschikbaarheid van de gegevens niet te peilen.

### <a name="consumer-groups"></a>Consumergroepen

Het Event Hubs-mechanisme voor publiceren/abonneren wordt geactiveerd via *consumergroepen*. Een consumergroep is een weergave (status, positie of offset) van een volledige Event Hub. Consumergroepen maken het mogelijk dat meerdere consumers beschikken over een afzonderlijke weergave van de gebeurtenisstroom. De toepassingen kunnen de stroom onafhankelijk, in hun eigen tempo en met hun eigen offsets lezen.

In een architectuur waarin de stroom wordt verwerkt, is elke downstream-toepassing gelijk aan een consumergroep. Als u gebeurtenisgegevens naar de langetermijnopslag wilt schrijven, is de schrijftoepassing die hiervoor wordt gebruikt, een consumergroep. De complexe verwerking van gebeurtenissen kan vervolgens worden uitgevoerd door een andere, afzonderlijke consumergroep. U hebt alleen toegang tot partities via een consumergroep. Er mag maximaal 5 gelijktijdige lezers op een andere partitie per consumergroep; echter **het wordt aanbevolen dat op een andere partitie per consumergroep slechts één actieve ontvanger is**. Een Event Hub bevat altijd een standaardconsumergroep. Voor een Event Hub van het type Standaard kunt u maximaal 20 consumergroepen maken.

Dit zijn voorbeelden van de URI-conventie voor consumergroepen:

```http
//[my namespace].servicebus.windows.net/[event hub name]/[Consumer Group #1]
//[my namespace].servicebus.windows.net/[event hub name]/[Consumer Group #2]
```

In de volgende afbeelding ziet u de architectuur voor de verwerking van stromen van Event Hubs:

![Event Hubs](./media/event-hubs-features/event_hubs_architecture.png)

### <a name="stream-offsets"></a>Stroom-offsets

Een *offset* is de positie van een gebeurtenis binnen een partitie. U kunt een offset beschouwen als een clientcursor. De offset is een bytenummering van de gebeurtenis. Met deze offset kan een gebeurtenisconsumer (lezer) een punt in de gebeurtenisstroom opgeven vanwaaruit begonnen moet worden met het lezen van gebeurtenissen. U kunt de offset opgeven als een tijdstempel of als een offsetwaarde. Consumers zijn zelf verantwoordelijk voor het opslaan van hun eigen offsetwaarden buiten de Event Hubs-service. Binnen een partitie bevat elke gebeurtenis een offset.

![Event Hubs](./media/event-hubs-features/partition_offset.png)

### <a name="checkpointing"></a>Controlepunten plaatsen

*Het plaatsen van controlepunten* is een proces waarbij lezers hun positie binnen de gebeurtenisvolgorde van een partitie markeren of vastleggen. Het plaatsen van controlepunten is de verantwoordelijkheid van de consumer en vindt plaats per partitie binnen een consumergroep. Deze verantwoordelijkheid houdt in dat elke partitielezer voor elke consumergroep de huidige positie in de gebeurtenisstroom moet bijhouden en de service kan informeren wanneer de gegevensstroom is voltooid.

Als een lezer van een partitie is losgekoppeld en er vervolgens weer verbinding wordt gemaakt, begint het lezen bij het controlepunt dat eerder is verzonden door de laatste lezer van de betreffende partitie in de consumergroep. Wanneer de lezer verbinding maakt, wordt deze offset aan de Event Hub doorgegeven om de locatie op te geven waar moet worden begonnen met lezen. Op deze manier kunt u het plaatsen van controlepunten gebruiken om gebeurtenissen te markeren als 'voltooid' door downstream-toepassingen. Bovendien beschikt u met controlepunten over tolerantie bij een failover tussen lezers die op verschillende apparaten worden uitgevoerd. Het is mogelijk om terug te keren naar de oudere gegevens door een lagere offset van dit controlepuntproces op te geven. Via dit mechanisme zorgt het plaatsen van controlepunten voor failover-tolerantie en voor herhaling van gebeurtenisstromen.

### <a name="common-consumer-tasks"></a>Algemene taken voor consumers

Alle Event Hubs-consumers maken verbinding via een AMQP 1.0-sessie, een statusbewust bidirectioneel communicatiekanaal. Elke partitie heeft een AMQP 1.0-sessie die het mogelijk maakt partitiespecifieke gebeurtenissen te transporteren.

#### <a name="connect-to-a-partition"></a>Verbinding maken met een partitie

Het is gebruikelijk om een leasemechanisme te gebruiken wanneer u verbinding maakt met partities, zodat u het verbinden van lezers aan specifieke partities kunt coördineren. Op deze manier maakt u het mogelijk dat elke partitie in een consumergroep slechts één actieve lezer heeft. Het plaatsen van controlepunten voor en het leasen en beheren van lezers wordt vereenvoudigd door toepassing van de [EventProcessorHost](/dotnet/api/microsoft.servicebus.messaging.eventprocessorhost)-klasse voor .NET-clients. De Event Processor Host is een intelligente consumeragent.

#### <a name="read-events"></a>Gebeurtenissen lezen

Nadat er voor een specifieke partitie een AMQP 1.0-sessie en -koppeling zijn geopend, worden de gebeurtenissen door de Event Hubs-service aan de AMQP 1.0-client geleverd. Dit leveringsmechanisme maakt hogere doorvoer en lagere latentie mogelijk dan pull-mechanismen zoals HTTP GET. Tijdens het verzenden van gebeurtenissen naar de client wordt elk gebeurtenisgegeven voorzien van belangrijke metagegevens, zoals de offset en het volgnummer. Deze worden gebruikt om het plaatsen van controlepunten in de gebeurtenisvolgorde mogelijk te maken.

Gebeurtenisgegevens:
* Offset
* Volgnummer
* Hoofdtekst
* Gebruikerseigenschappen
* Systeemeigenschappen

U bent verantwoordelijk voor het beheer van de offset.

## <a name="capacity"></a>Capaciteit

Event Hubs heeft een uiterst schaalbare parallelle architectuur. Er zijn diverse belangrijke factoren waarmee u rekening moet houden wanneer u schaalt.

### <a name="throughput-units"></a>Doorvoereenheden

De doorvoercapaciteit van Event Hubs wordt bepaald door het aantal beschikbare *doorvoereenheden*. Doorvoereenheden zijn vooraf aangeschafte capaciteitseenheden. Eén doorvoereenheid bevat de volgende capaciteit:

* Binnenkomende gegevens: maximaal 1 MB per seconde of 1000 gebeurtenissen per seconde (afhankelijk van wat het eerst wordt bereikt)
* Uitgaande gegevens: maximaal 2 MB per seconde

Wanneer de capaciteit van de aangekochte doorvoereenheden wordt overschreven, wordt de invoer vertraagd en een [ServerBusyException](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception) afgegeven. De uitvoer geeft geen vertragingsuitzonderingen af, maar is nog steeds beperkt tot de capaciteit van de aangekochte doorvoereenheden. Als zich uitzonderingen met betrekking tot de publicatiesnelheid voordoen of als u meer uitgaande gegevens verwacht, controleert u hoeveel doorvoereenheden u hebt aangeschaft voor de naamruimte. U kunt doorvoereenheden beheren op de **Scale** blade van de naamruimten in de [Azure-portal](https://portal.azure.com). U kunt ook beheren doorvoereenheden programmatisch met behulp van de [Event Hubs-API's](event-hubs-api-overview.md).

Doorvoereenheden worden per uur in rekening gebracht en zijn vooraf aangeschaft. Nadat u doorvoereenheden hebt aangeschaft, worden deze voor minimaal één uur in rekening gebracht. Er kunnen maximaal 20 doorvoereenheden worden aangeschaft voor een naamruimte in Event Hubs. Deze worden gedeeld door alle Event Hubs in de naamruimte.

U kunt meer doorvoereenheden aanschaffen in blokken van 20, met een maximum van 100 doorvoereenheden, door contact op te nemen met de ondersteuning van Azure. Daarna kunt u ook blokken van 100 doorvoereenheden aanschaffen.

Het is raadzaam dat u doorvoereenheden en partities om te zorgen voor optimale schaal worden verdeeld. Per partitie is maximaal één doorvoereenheid mogelijk. Het aantal doorvoereenheden moet daarom kleiner zijn dan of gelijk zijn aan het aantal partities in een Event Hub.

Zie [Prijzen van Event Hubs](https://azure.microsoft.com/pricing/details/event-hubs/) voor gedetailleerde informatie over prijzen van Event Hubs.

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over Event Hubs gaat u naar de volgende koppelingen:

* Aan de slag met een [Event Hubs-zelfstudie][Event Hubs tutorial]
* [Programmeerhandleiding voor Event Hubs](event-hubs-programming-guide.md)
* [Beschikbaarheid en consistentie in Event Hubs](event-hubs-availability-and-consistency.md)
* [Veelgestelde vragen over Event Hubs](event-hubs-faq.md)
* [Voorbeelden van Event Hubs][]

[Event Hubs tutorial]: event-hubs-dotnet-standard-getstarted-send.md
[Voorbeelden van Event Hubs]: https://github.com/Azure/azure-event-hubs/tree/master/samples
