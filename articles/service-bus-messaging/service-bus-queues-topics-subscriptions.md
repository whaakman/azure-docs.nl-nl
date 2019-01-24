---
title: Overzicht van Azure Service Bus-berichten van wachtrijen, onderwerpen en abonnementen | Microsoft Docs
description: Overzicht van Service Bus messaging-entiteiten.
services: service-bus-messaging
documentationcenter: na
author: axisc
manager: timlt
editor: spelluru
ms.service: service-bus-messaging
ms.topic: article
ms.date: 09/18/2018
ms.author: aschhab
ms.openlocfilehash: 7cacabf4f171189810e943043b5513e20113d962
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54847028"
---
# <a name="service-bus-queues-topics-and-subscriptions"></a>Service Bus-wachtrijen, -onderwerpen en -abonnementen

Microsoft Azure Service Bus ondersteunt een cloud-gebaseerde, bericht-georiënteerde middleware-technologieën, inclusief betrouwbare message Queuing- en duurzame publiceren/abonneren op berichten. Worden kunnen deze 'brokered' berichtmogelijkheden beschouwd als losgekoppeld van de functies voor berichten die ondersteuning voor publiceren / abonneren, tijdelijke ontkoppeling en scenario's met behulp van de Service Bus messaging-workload voor taakverdeling. Ontkoppelde communicatie heeft veel voordelen; clients en servers kunnen bijvoorbeeld naar behoefte verbinding maken en hun bewerkingen asynchroon uitvoeren.

De messaging-entiteiten die de kern van de mogelijkheden voor messaging in Service Bus vormen zijn wachtrijen, onderwerpen en abonnementen en regels/acties.

## <a name="queues"></a>Wachtrijen

Wachtrijen bieden *First In, First Out* (FIFO-principe)-berichtbezorging naar een of meer concurrerende consumenten. Dat wil zeggen, ontvangers meestal ontvangen en verwerken van berichten in de volgorde waarin ze zijn toegevoegd aan de wachtrij en slechts één berichtconsument ontvangt en elk bericht dat wordt verwerkt. Een belangrijk voordeel van het gebruik van wachtrijen is dat 'tijdelijke ontkoppeling' van de onderdelen van de toepassing. Met andere woorden, hoeft de producenten (afzenders) en consumenten (ontvangers) niet te worden verzonden en ontvangen van berichten op hetzelfde moment, omdat berichten blijvend worden opgeslagen in de wachtrij. Daarnaast wordt heeft de producent geen wachten op een reactie van de gebruiker om door te gaan om te verwerken en verzenden van berichten.

Een bijkomend voordeel is 'herverdeling van taken,"waardoor producenten en consumenten te verzenden en ontvangen van berichten met verschillende snelheden. In veel toepassingen varieert het laden van het systeem gedurende een periode; Er is echter de benodigde verwerkingstijd voor elke werkeenheid doorgaans constant blijft. Tussen producenten en consumenten met een wachtrij betekent dit dat de betreffende toepassing alleen moeten worden ingericht om te kunnen spelen op de gemiddelde belasting in plaats van piekbelasting. De lengte van de wachtrij neemt toe of af, al naargelang het binnenkomende verkeer. Deze mogelijkheid bespaart rechtstreeks u geld met betrekking tot de hoeveelheid infrastructuur die nodig zijn voor de belasting van de toepassing. Als de belasting toeneemt, kunnen meer werkprocessen worden toegevoegd om te lezen uit de wachtrij. Elk bericht worden door slechts één van de werkprocessen verwerkt. Bovendien kunt deze pull-gebaseerde taakverdeling optimaal gebruik van de worker-computers, zelfs als de worker-computers met betrekking tot de verwerkingskracht verschillen, zoals ze berichten op eigen maximale snelheid ophalen. Dit patroon wordt vaak aangeduid als het patroon 'concurrerende consumenten'.

Met behulp van wachtrijen naar tussenliggende tussen producenten en consumenten, biedt een inherente losse koppeling tussen de onderdelen. Omdat producenten en consumenten niet op de hoogte van elkaar worden verbonden, kan een gebruiker zonder van invloed zijn op de producent worden bijgewerkt.

### <a name="create-queues"></a>Wachtrijen maken

Maken van wachtrijen met de [Azure-portal](service-bus-quickstart-portal.md), [PowerShell](service-bus-quickstart-powershell.md), [CLI](service-bus-quickstart-cli.md), of [Resource Manager-sjablonen](service-bus-resource-manager-namespace-queue.md). U gaat vervolgens verzenden en ontvangen van berichten met behulp van een [QueueClient](/dotnet/api/microsoft.azure.servicebus.queueclient) object.

Als u wilt snel informatie over het maken van een wachtrij, en vervolgens verzenden en ontvangen van berichten naar en uit de wachtrij, Zie de [snelstartgidsen](service-bus-quickstart-portal.md) voor elke methode. Zie voor een meer diepgaande zelfstudie over het gebruik van wachtrijen, [aan de slag met Service Bus-wachtrijen](service-bus-dotnet-get-started-with-queues.md).

Zie voor een voorbeeld werkt de [BasicSendReceiveUsingQueueClient voorbeeld](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/GettingStarted/Microsoft.Azure.ServiceBus/BasicSendReceiveUsingQueueClient) op GitHub.

### <a name="receive-modes"></a>Modi ontvangen

U kunt twee verschillende modi waarin Service Bus berichten ontvangt opgeven: *ReceiveAndDelete* of *PeekLock*. In de [ReceiveAndDelete](/dotnet/api/microsoft.azure.servicebus.receivemode) modus, de receive-bewerking is één; dat wil zeggen, Service Bus de aanvraag ontvangt, wordt het bericht als verbruikt gemarkeerd en geeft dit terug aan de toepassing. **ReceiveAndDelete** modus is het eenvoudigste model en werkt het beste voor scenario's waarin de toepassing niet verwerken van een bericht tolereren kan als er een fout optreedt. Voor meer informatie over dit scenario, u hebt een scenario waarin de consument problemen met de aanvraag ontvangen en vervolgens vastloopt voordat deze wordt verwerkt. Omdat Service Bus het bericht als verbruikt, wanneer de toepassing opnieuw wordt opgestart en het verbruik van berichten opnieuw begint gemarkeerd, ontbreekt het bericht dat voor het vastlopen is verbruikt.

In [PeekLock](/dotnet/api/microsoft.azure.servicebus.receivemode) modus wordt de ontvangstbewerking twee fasen, wordt, waardoor het mogelijk toepassingen kunnen worden ondersteund die geen ontbrekende berichten kunnen tolereren. Wanneer de aanvraag wordt ontvangen door Service Bus, deze vindt het volgende bericht om te worden verbruikt, wordt vergrendeld om te voorkomen dat andere consumenten het ontvangen en vervolgens terugkeert naar de toepassing. Nadat de toepassing klaar is met het verwerken van bericht (of deze op betrouwbare wijze voor toekomstige verwerking slaat), is de tweede fase van het ontvangstproces voltooid door het aanroepen van [CompleteAsync](/dotnet/api/microsoft.azure.servicebus.queueclient.completeasync) voor het ontvangen bericht. Wanneer Service Bus ziet de **CompleteAsync** aanroep, deze wordt het bericht als verbruikt gemarkeerd.

Als de toepassing niet kan het bericht te verwerken voor een of andere reden, roept deze de [AbandonAsync](/dotnet/api/microsoft.azure.servicebus.queueclient.abandonasync) methode voor het ontvangen bericht (in plaats van [CompleteAsync](/dotnet/api/microsoft.azure.servicebus.queueclient.completeasync)). Deze methode kunt Service Bus het bericht ontgrendelt en het toegankelijk maken voor het opnieuw worden ontvangen door de dezelfde consument of door een andere concurrerende consumenten. Ten tweede, er is een time-out die is gekoppeld aan de vergrendeling en als de toepassing niet kan verwerken van het bericht voordat de time-out van de vergrendeling verloopt (bijvoorbeeld, als de toepassing vastloopt), en vervolgens de Service Bus het bericht ontgrendelt en wordt het beschikbaar is voor ontvangen opnieuw) in wezen uitvoeren van een [AbandonAsync](/dotnet/api/microsoft.azure.servicebus.queueclient.abandonasync) bewerking standaard).

In het geval dat de toepassing is vastgelopen na het verwerken van het bericht, maar voordat de **CompleteAsync** aanvraag is uitgegeven, wordt het bericht is opnieuw naar de toepassing bezorgd wanneer deze opnieuw wordt opgestart. Dit proces heet vaak *tenminste eenmaal* verwerking; dat wil zeggen, elk bericht ten minste één keer wordt verwerkt. Echter, in bepaalde situaties hetzelfde bericht opnieuw kan worden bezorgd. Als het scenario niet kan dubbele verwerking, tolereren en vervolgens aanvullende logica is vereist in de toepassing voor het detecteren van duplicaten, die kan worden bereikt op basis van de [MessageId](/dotnet/api/microsoft.azure.servicebus.message.messageid) eigenschap van het bericht dat gelijk blijft bij in bezorgingspogingen. Deze functie wordt ook wel *exact één keer* verwerken.

## <a name="topics-and-subscriptions"></a>Onderwerpen en abonnementen

In tegenstelling tot wachtrijen, waarin elk bericht wordt verwerkt door een enkele gebruiker *onderwerpen* en *abonnementen* voorzien in de vorm van een een-op-veel communicatie, in een *publiceren/abonneren* patroon. Kan handig zijn voor het schalen van grote aantallen ontvangers, elk gepubliceerde bericht beschikbaar wordt gesteld aan elk abonnement dat is geregistreerd bij het onderwerp. Berichten worden verzonden naar een onderwerp en die worden geleverd aan een of meer gekoppelde abonnementen, afhankelijk van de filterregels die kunnen worden ingesteld op basis van per abonnement. De abonnementen kunnen extra filters gebruiken om te beperken van de berichten die ze willen ontvangen. Berichten worden verzonden naar een onderwerp op dezelfde manier als ze worden verzonden naar een wachtrij, maar berichten zijn niet ontvangen van het onderwerp rechtstreeks. In plaats daarvan worden ze heeft ontvangen van abonnementen. Een onderwerpabonnement lijkt op een virtuele wachtrij die kopieën van de berichten die worden verzonden naar het onderwerp ontvangt. Berichten worden ontvangen van een abonnement identiek aan de manier waarop die ze worden ontvangen van een wachtrij.

Ter vergelijking, de functionaliteit bericht verzenden van een wachtrij wordt toegewezen rechtstreeks naar een onderwerp en de functionaliteit van het bericht ontvangen die zijn toegewezen aan een abonnement. Deze functie betekent onder andere abonnementen ondersteuning van de dezelfde patronen die eerder zijn beschreven in deze sectie met betrekking tot wachtrijen: concurrerende consumenten, tijdelijke ontkoppeling, load leveling en taakverdeling.

### <a name="create-topics-and-subscriptions"></a>Maken van onderwerpen en abonnementen

Het maken van een onderwerp is vergelijkbaar met het maken van een wachtrij, zoals beschreven in de vorige sectie. Vervolgens verzenden van berichten met de [TopicClient](/dotnet/api/microsoft.azure.servicebus.topicclient) klasse. Om berichten te ontvangen, maakt u een of meer abonnementen naar het onderwerp. Net als bij wachtrijen, berichten worden ontvangen van een abonnement met een [SubscriptionClient](/dotnet/api/microsoft.azure.servicebus.subscriptionclient) object in plaats van een [QueueClient](/dotnet/api/microsoft.azure.servicebus.queueclient) object. Maak de abonnementsclient, waarbij de naam van het onderwerp, de naam van het abonnement en (optioneel) de modus ontvangen als parameters doorgegeven.

Voor een volledige werken voorbeeld, Zie de [BasicSendReceiveUsingTopicSubscriptionClient voorbeeld](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/GettingStarted/Microsoft.Azure.ServiceBus/BasicSendReceiveUsingTopicSubscriptionClient) op GitHub.

### <a name="rules-and-actions"></a>Regels en acties

In veel scenario's, moeten berichten met specifieke kenmerken op verschillende manieren worden verwerkt. U kunt abonnementen op berichten zoeken die u hebt de gewenste eigenschappen en klik vervolgens op bepaalde wijzigingen in de eigenschappen configureren zodat deze verwerking. Terwijl de Service Bus-abonnementen weergeven alle berichten die naar het onderwerp, kunt u alleen een subset van deze berichten kopiëren naar de abonnementenwachtrij voor virtuele. Dit filter wordt gerealiseerd met behulp van abonnementfilters. Dergelijke wijzigingen worden genoemd *filteracties*. Als u een abonnement maakt, kunt u een filterexpressie die is van invloed op de eigenschappen van het bericht, zowel de Systeemeigenschappen opgeven (bijvoorbeeld **Label**) en aangepaste toepassingseigenschappen (bijvoorbeeld  **StoreName**.) De SQL-filterexpressie is optioneel in dit geval; zonder een SQL-filter-expressie, wordt geen filteractie gedefinieerd voor een abonnement worden uitgevoerd op alle berichten voor dat abonnement.

Voor een volledige werken voorbeeld, Zie de [TopicSubscriptionWithRuleOperationsSample voorbeeld](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/GettingStarted/Microsoft.Azure.ServiceBus/TopicSubscriptionWithRuleOperationsSample) op GitHub.

Voor meer informatie over mogelijke filterwaarden, Zie de documentatie voor de [SqlFilter](/dotnet/api/microsoft.azure.servicebus.sqlfilter) en [SqlRuleAction](/dotnet/api/microsoft.azure.servicebus.sqlruleaction) klassen.

## <a name="next-steps"></a>Volgende stappen

Zie de volgende geavanceerde onderwerpen voor meer informatie en voorbeelden van het gebruik van Service Bus-berichten:

* [Overzicht van Service Bus-berichten](service-bus-messaging-overview.md)
* [Snelstart: Verzenden en berichten ontvangen met behulp van de Azure portal en .NET](service-bus-quickstart-portal.md)
* [Zelfstudie: Bijwerken van de voorraad met behulp van Azure portal en onderwerpen/abonnementen](service-bus-tutorial-topics-subscriptions-portal.md)


