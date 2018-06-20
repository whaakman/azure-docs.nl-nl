---
title: Overzicht van Azure Service Bus-basisbeginselen | Microsoft Docs
description: Een inleiding in het gebruik van Service Bus om Azure-toepassingen te verbinden met andere software.
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
ms.service: service-bus-messaging
ms.topic: get-started-article
ms.date: 05/23/2018
ms.author: sethm
ms.openlocfilehash: 994510b415e21288fd38a116f7e77a59ba79af59
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34641319"
---
# <a name="azure-service-bus"></a>Azure Service Bus

Een toepassing of service moet vaak communiceren met andere toepassingen of services, ongeacht of de toepassing of service in de cloud of on-premises wordt uitgevoerd. Microsoft Azure biedt Service Bus om dit op een veelzijdige, handige manier te doen. In dit artikel wordt een overzicht gegeven van deze technologie en wordt beschreven wat de service inhoudt en waarom u deze zou kunnen overwegen.

## <a name="service-bus-fundamentals"></a>Grondbeginselen van Service Bus

Verschillende situaties vragen om verschillende communicatiemethoden. Soms is het de beste oplossing als toepassingen berichten verzenden en ontvangen via een eenvoudige wachtrij. In andere gevallen is een gewone wachtrij niet voldoende en voldoet een wachtrij met een mechanisme voor publiceren en abonneren beter. In sommige gevallen is alleen een verbinding tussen toepassingen nodig en is er geen behoefte aan een wachtrij. Azure Service Bus biedt alle drie de opties, zodat uw toepassingen op verschillende manieren met elkaar kunnen communiceren.

Service Bus is een multitenant-cloudservice, hetgeen betekent dat de service wordt gedeeld door meerdere gebruikers. Elke gebruiker, zoals een toepassingsontwikkelaar, maakt een *naamruimte* en definieert vervolgens de communicatiemechanismen die binnen deze naamruimte nodig zijn. In afbeelding 1 ziet u deze architectuur:

![][1]

**Afbeelding 1: Service Bus voorziet in een multitenant-service voor verbinding van toepassingen via de cloud.**

Binnen een naamruimte kunt u een of meer exemplaren van drie verschillende communicatiemechanismen gebruiken, die toepassingen alle drie op een andere manier verbinden. De opties zijn:

* *Wachtrijen*, waarmee communicatie in één richting mogelijk is. Elke wachtrij fungeert als intermediaire service (ook wel *broker* genoemd) die de verzonden berichten opslaat totdat ze worden ontvangen. Elk bericht wordt door één ontvanger ontvangen.
* *Onderwerpen*, die met behulp van *abonnementen* communicatie in één richting bieden. Eén onderwerp kan meerdere abonnementen hebben. Net zoals een wachtrij fungeert een onderwerp als broker, maar elk abonnement kan optioneel een filter gebruiken om alleen berichten te ontvangen die aan bepaalde criteria voldoen.
* *Relays*, die bidirectionele communicatie bieden. In tegenstelling tot wachtrijen en onderwerpen, worden via een relay berichten die onderweg zijn niet opgeslagen. Een relay is geen broker. De berichten worden slechts doorgegeven aan de doeltoepassing.

Wanneer u een wachtrij, onderwerp of relay maakt, geeft u deze een naam. In combinatie met de naam van uw naamruimte wordt aan de hand van deze naam een unieke id voor het object gemaakt. Toepassingen kunnen deze naam verstrekken aan Service Bus en vervolgens de wachtrij, het onderwerp of de relay gebruiken om met elkaar te communiceren. 

Windows-toepassingen kunnen WCF (Windows Communication Foundation) gebruiken om een van deze objecten in het relayscenario te gebruiken. Deze service wordt ook wel [WCF-relay](../service-bus-relay/relay-what-is-it.md). Voor wachtrijen en onderwerpen kunnen Windows-toepassingen door Service Bus gedefinieerde berichtenservice-API's gebruiken. Microsoft biedt SDK's voor Java, Node.js en andere talen om deze objecten gemakkelijker te kunnen gebruiken vanuit niet-Windows-toepassingen. U hebt ook toegang tot wachtrijen en onderwerpen met behulp van [REST-API's](/rest/api/servicebus/) via HTTP(S). 

Het is belangrijk te begrijpen dat toepassingen die Service Bus gebruiken overal kunnen worden uitgevoerd, hoewel Service Bus zelf wordt uitgevoerd in de cloud (dat wil zeggen, in datacenters van Microsoft Azure). U kunt Service Bus bijvoorbeeld gebruiken om verbinding te maken met toepassingen die worden uitgevoerd in Azure, of toepassingen die worden uitgevoerd binnen uw eigen datacenter. U kunt Service Bus ook gebruiken om verbinding te maken met een toepassing die wordt uitgevoerd in Azure of een ander cloudplatform met een on-premises toepassing, of met een tablet of telefoon. Service Bus is een communicatiemechanisme in de cloud dat vanaf vrijwel elke locatie toegankelijk is. De manier waarop u Service Bus gebruikt, hangt af van de taak van uw toepassingen.

## <a name="queues"></a>Wachtrijen

Stel dat u met twee toepassingen verbinding wilt maken via een Service Bus-wachtrij. In afbeelding 2 ziet u deze situatie:

![][2]

**Afbeelding 2: Service Bus-wachtrijen bieden asynchrone wachtrijservices in één richting.**

Een afzender verzendt een bericht naar een Service Bus-wachtrij en een ontvanger verwerkt dat bericht op een later tijdstip in ontvangst. Een wachtrij kan slechts één ontvanger, hebben, zoals in afbeelding 2 wordt weergegeven. Of meerdere toepassingen kunnen vanuit de dezelfde wachtrij lezen. In het laatste geval wordt elk bericht door slechts één ontvanger gelezen. Voor een multicast-service is het echter beter om een onderwerp te gebruiken.

Elk bericht bestaat uit twee delen: een reeks eigenschappen, elk een sleutel-waardepaar, en een berichtnettolading. De nettolading kan binair, tekst of zelfs XML zijn. Hoe deze worden gebruikt, hangt af van wat een toepassing probeert te doen. Een toepassing die een bericht verzendt over een recente verkoop, kan bijvoorbeeld de eigenschappen **Verkoper = Ava** en **Bedrag = 10000** bevatten. De berichttekst kan een gescande afbeelding bevatten van de ondertekende verkoopovereenkomst of, als die niet beschikbaar is, leeg blijven.

Een ontvanger kan op twee verschillende manieren een bericht in een Service Bus-wachtrij lezen. Met de eerste optie, *[ReceiveAndDelete](/dotnet/api/microsoft.azure.servicebus.receivemode)*, wordt een bericht uit de wachtrij ontvangen en onmiddellijk gewist. Dit is een eenvoudig aanpak, maar als de computer van de ontvanger vastloopt voordat het bericht is verwerkt, gaat het bericht verloren. Andere ontvangers hebben geen toegang tot het bericht, omdat het uit de wachtrij is verwijderd. 

De tweede optie, *[PeekLock](/dotnet/api/microsoft.azure.servicebus.receivemode)*, is bedoeld om dit probleem te ondervangen. Net zoals bij **ReceiveAndDelete** wordt het bericht met **PeekLock** verwijderd uit de wachtrij. Het bericht wordt echter niet gewist. In plaats daarvan wordt het vergrendeld, waardoor het onzichtbaar wordt voor andere ontvangers. Vervolgens wordt gewacht op een van drie gebeurtenissen:

* Als de ontvanger het bericht met succes heeft verwerkt, ontvangt de wachtrij de melding [Complete()](/dotnet/api/microsoft.azure.servicebus.queueclient.completeasync) en wordt het bericht uit de wachtrij verwijderd. 
* Als de ontvanger het bericht niet naar behoren kan verwerken, ontvangt de wachtrij de melding [Abandon()](/dotnet/api/microsoft.azure.servicebus.queueclient.abandonasync). De vergrendeling van het bericht in de wachtrij wordt ongedaan gemaakt en het bericht is beschikbaar voor andere ontvangers.
* Als de ontvanger binnen de geconfigureerde tijd (standaard 60 seconden) geen van beide methoden aanroept, wordt ervan uitgegaan dat het ontvangen is mislukt. In dit geval gedraagt de wachtrij zich alsof de ontvanger de melding **Afbreken** heeft afgegeven en wordt het bericht beschikbaar gesteld aan andere ontvangers.

Ziet u wat er hier kan gebeuren? Hetzelfde bericht kan dus tweemaal worden bezorgd, mogelijk bij twee verschillende ontvangers. Toepassingen die gebruikmaken van Service Bus-wachtrijen, moeten hiermee rekening houden. Om detectie van duplicaten gemakkelijker te maken, heeft elk bericht een unieke [MessageID](/dotnet/api/microsoft.azure.servicebus.message.messageid#Microsoft_Azure_ServiceBus_Message_MessageId). Deze eigenschap blijft standaard hetzelfde, ongeacht hoe vaak het bericht in een wachtrij is gelezen. 

Wachtrijen zijn handig in tal van situaties. Met behulp van wachtrijen kunnen toepassingen zelfs communiceren wanneer ze niet tegelijkertijd worden uitgevoerd. Dit is met name handig bij batchtoepassingen en mobiele toepassingen. Een wachtrij met meerdere ontvangers biedt ook automatische taakverdeling, aangezien verzonden berichten worden verdeeld over deze ontvangers.

## <a name="topics"></a>Onderwerpen

Hoewel ze handig zijn, zijn wachtrijen niet altijd de juiste oplossing. Soms zijn onderwerpen beter. Afbeelding 3 laat dit zien:

![][3]

**Afbeelding 3: Op basis van het filter dat door een geabonneerde toepassing wordt opgegeven, kan de toepassing enkele of alle berichten ontvangen die naar een Service Bus-onderwerp zijn verzonden.**

Een *onderwerp* lijkt in veel opzichten op een wachtrij. Afzenders verzenden berichten op dezelfde manier naar een onderwerp als naar een wachtrij en de berichten zien er hetzelfde uit als bij een wachtrij. Het verschil is dat het bij onderwerpen mogelijk is dat elke ontvangende toepassing zijn eigen *abonnement* en eventueel een *filter* definieert. Een abonnee ontvangt een kopie van elk bericht in het onderwerp, maar met behulp van een filter kunnen vervolgens alleen de berichten worden ontvangen die overeenkomen met het filter. In afbeelding 3 ziet u bijvoorbeeld een afzender en een onderwerp met drie abonnees, elk met een eigen filter:

* Abonnee 1 ontvangt alleen berichten met de eigenschap **Verkoper =Ava**.
* Abonnee 2 ontvangt berichten met de eigenschap **Verkoper = Ruby** en/of een eigenschap **Bedrag** met een waarde van meer dan 100.000. Wellicht is Ruby de verkoopmanager en wil ze zowel haar eigen verkopen als alle omvangrijke verkopen zien, ongeacht wie ze tot stand brengt.
* Abonnee 3 heeft het filter ingesteld op **Waar**, hetgeen betekent dat alle berichten worden ontvangen. Misschien is deze toepassing bijvoorbeeld verantwoordelijk voor het onderhouden van een audittrail en moet deze alle berichten zien.

Net zoals het geval is voor wachtrijen, kunnen abonnees van een onderwerp berichten lezen met [ReceiveAndDelete of PeekLock](/dotnet/api/microsoft.azure.servicebus.receivemode). In tegenstelling tot wachtrijen kan echter een enkel bericht dat naar een onderwerp is verzonden door meerdere abonnementen worden ontvangen. Deze benadering, doorgaans aangeduid met *publiceren en abonneren* (of *pub/sub*), is nuttig wanneer meerdere toepassingen in dezelfde berichten zijn geïnteresseerd. Door het juiste filter te definiëren, kunnen abonnees alleen het gedeelte van de berichtenstroom ontvangen dat ze nodig hebben.

## <a name="relays"></a>Relays

Wachtrijen en onderwerpen bieden allebei asynchrone communicatie in één richting via een broker. Het verkeer stroomt in één richting en er is geen directe verbinding tussen afzenders en ontvangers. Maar wat als u dit niet wilt? Stel dat uw toepassingen berichten moeten verzenden en berichten moeten ontvangen, of stel dat u een rechtstreekse koppeling tussen uw toepassingen wilt en geen broker nodig hebt voor het opslaan van berichten. Voor dit soort scenario's biedt Service Bus *relays*, zoals afbeelding 4 laat zien:

![][4]

**Afbeelding 4: Service Bus Relay biedt synchrone communicatie in twee richtingen tussen toepassingen.**

De voor de hand liggende vraag over relays is: waarom zou u een relay gebruiken? Zelfs als u geen wachtrijen nodig hebt, waarom zouden toepassingen dan moeten communiceren via een cloudservice in plaats van dat ze rechtstreeks communiceren? Het antwoord is dat rechtstreeks communiceren lastiger kan zijn dan u denkt.

Stel dat u twee on-premises toepassingen wilt verbinden, die allebei worden uitgevoerd binnen bedrijfsdatacenters. Elk van deze toepassingen bevindt zich achter een firewall en elk datacenter maakt waarschijnlijk gebruik van NAT (netwerkadresomzetting). De firewall blokkeert binnenkomende gegevens op bijna alle poorten en NAT impliceert dat de computer waarop de toepassingen worden uitgevoerd, geen vast IP-adres heeft dat u rechtstreeks kunt bereiken buiten het datacenter. Zonder extra hulp is het lastig om deze toepassingen via het openbare internet met elkaar te verbinden.

Een Service Bus-relay biedt uitkomst. Voor communicatie in twee richtingen via een Relay, brengt elke toepassing een uitgaande TCP-verbinding tot stand met Service Bus. Deze verbinding blijft geopend. Alle communicatie tussen de twee toepassingen verloopt via deze verbindingen. Omdat elke verbinding vanuit het datacenter tot stand is gebracht, staat de firewall inkomend verkeer naar elke toepassing toe zonder nieuwe poorten te openen. Via deze benadering wordt ook het NAT-probleem opgelost, omdat elke toepassing tijdens de communicatie een consistent eindpunt in de cloud heeft. Door gegevens uit te wisselen via de Relay kunnen de toepassingen problemen voorkomen die de communicatie anders zouden bemoeilijken. 

Voor het gebruik van Service Bus Relays zijn toepassingen afhankelijk van WCF (Windows Communication Foundation). Service Bus biedt WCF-bindingen waarmee Windows-toepassingen eenvoudig kunnen communiceren via Relays. Toepassingen die al gebruikmaken van WCF hoeven doorgaans slechts een van deze bindingen op te geven en kunnen vervolgens met elkaar communiceren via een Relay. Relays kunnen worden gebruikt vanuit niet-Windows-toepassingen, maar in tegenstelling tot wachtrijen en onderwerpen vereist dit enige programmering. Er worden geen standaardbibliotheken geleverd.

In tegenstelling tot wachtrijen en onderwerpen, maken toepassingen Relays niet expliciet. Wanneer een toepassing die berichten wil ontvangen, een TCP-verbinding tot stand brengt met Service Bus, wordt automatisch een Relay gemaakt. Wanneer de verbinding wordt verbroken, wordt de Relay verwijderd. Service Bus voorziet in een register aan de hand waarvan toepassingen een specifieke Relay op basis van de naam kunnen vinden. Zo kan een toepassing de Relay vinden die door een specifieke listener is gemaakt.

Relays zijn de juiste oplossing wanneer rechtstreekse communicatie tussen toepassingen nodig is. Denk bijvoorbeeld aan een reserveringssysteem voor een luchtvaartmaatschappij dat wordt uitgevoerd in een on-premises datacenter en dat toegankelijk moet zijn via incheckbalies, mobiele apparaten en andere computers. Toepassingen die op al deze systemen worden uitgevoerd, zouden op Service Bus Relays in de cloud kunnen vertrouwen voor communicatie, waar ze ook worden uitgevoerd.

## <a name="summary"></a>Samenvatting

Het verbinden van toepassingen is altijd onderdeel geweest van het ontwikkelen van complete oplossingen. De verscheidenheid van scenario's waarin toepassingen en services met elkaar moeten communiceren, zal toenemen naarmate meer toepassingen en apparaten zijn verbonden met internet. Service Bus is erop gericht om deze essentiële functie gemakkelijker te implementeren en op grotere schaal beschikbaar te maken door cloudtechnologieën te leveren die deze communicatie mogelijk maken via wachtrijen, onderwerpen en relays.

## <a name="next-steps"></a>Volgende stappen

U hebt kennisgemaakt met de grondbeginselen van Azure Service Bus. Klik op de volgende koppelingen voor meer informatie.

* [Service Bus-wachtrijen](service-bus-dotnet-get-started-with-queues.md) gebruiken
* [Service Bus-onderwerpen](service-bus-dotnet-how-to-use-topics-subscriptions.md) gebruiken
* [Service Bus Relay](../service-bus-relay/service-bus-dotnet-how-to-use-relay.md) gebruiken
* [Voorbeelden van Service Bus](service-bus-samples.md)

[1]: ./media/service-bus-fundamentals-hybrid-solutions/SvcBus_01_architecture.png
[2]: ./media/service-bus-fundamentals-hybrid-solutions/SvcBus_02_queues.png
[3]: ./media/service-bus-fundamentals-hybrid-solutions/SvcBus_03_topicsandsubscriptions.png
[4]: ./media/service-bus-fundamentals-hybrid-solutions/SvcBus_04_relay.png
