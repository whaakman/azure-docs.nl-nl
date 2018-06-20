---
title: Overzicht van Azure Service Bus-wachtrijen, onderwerpen en abonnementen messaging | Microsoft Docs
description: Overzicht van Service Bus-berichtentiteiten.
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
ms.service: service-bus-messaging
ms.topic: article
ms.date: 06/18/2018
ms.author: sethm
ms.openlocfilehash: 424004a2a39bd0d05bce515dc17685e60f7a0c9b
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36231573"
---
# <a name="service-bus-queues-topics-and-subscriptions"></a>Service Bus-wachtrijen, -onderwerpen en -abonnementen

Microsoft Azure Service Bus ondersteunt een set van cloud-gebaseerde, bericht georiënteerd middleware-technologieën waaronder betrouwbare message Queuing- en duurzame publiceren/abonneren messaging. Deze 'brokered' messaging-mogelijkheden worden van beschouwd als losgekoppeld van de functies voor berichten die ondersteuning voor publiceren / abonneren, tijdelijke ontkoppeling en scenario's met behulp van de werkbelasting van Service Bus-berichten voor taakverdeling. Ontkoppelde communicatie heeft veel voordelen; clients en servers kunnen bijvoorbeeld naar behoefte verbinding maken en hun bewerkingen asynchroon uitvoeren.

De messaging-entiteiten die de kern van de mogelijkheden voor messaging in Service Bus vormen zijn wachtrijen, onderwerpen en abonnementen en regels/acties.

## <a name="queues"></a>Wachtrijen

Wachtrijen bieden *First In, First Out* (FIFO-principe)-berichtbezorging naar een of meer concurrerende consumenten. Dat wil zeggen, ontvangers meestal ontvangen en verwerken van berichten in de volgorde waarin ze zijn toegevoegd aan de wachtrij en slechts één berichtengebruiker ontvangen en verwerken van elk bericht. Er is een belangrijk voordeel van het gebruik van wachtrijen 'tijdelijke ontkoppeling' bereiken van toepassingsonderdelen. Met andere woorden, hoeft de producenten (afzenders) en consumenten (ontvangers) niet te verzenden en ontvangen van berichten op hetzelfde moment omdat berichten blijvend worden opgeslagen in de wachtrij. De producent heeft bovendien geen moet worden gewacht op een reactie van de gebruiker om door te gaan om te verwerken en verzenden van berichten.

Een bijkomend voordeel is 'herverdeling van taken,' waardoor producenten en consumenten berichten met verschillende snelheden verzenden en ontvangen. In veel toepassingen varieert de de systeembelasting gedurende een bepaalde periode; Er is echter de benodigde verwerkingstijd voor elke werkeenheid doorgaans constant blijft. Tussen bericht producenten en consumenten met een wachtrij betekent dat de betreffende toepassing alleen moeten worden ingericht om te kunnen afhandelen gemiddelde belasting in plaats van piekbelasting. De lengte van de wachtrij neemt toe of af, al naargelang het binnenkomende verkeer. Deze mogelijkheid bespaart rechtstreeks geld met betrekking tot de hoeveelheid infrastructuur die nodig zijn voor het laden van toepassingen. Als de belasting toeneemt, kunnen meer werkprocessen worden toegevoegd om te lezen uit de wachtrij. Elk bericht worden door slechts één van de werkprocessen verwerkt. Bovendien kunt deze pull-gebaseerde taakverdeling u optimaal gebruik van de werknemer computers zelfs als de computers van de werknemer met betrekking tot verwerkingskracht verschillen, zoals ze berichten op eigen maximale snelheid ophalen. Dit patroon wordt vaak aangeduid als het patroon 'concurrerende consumenten'.

Met behulp van wachtrijen naar de tussenliggende tussen bericht producenten en consumenten biedt een inherente losse koppeling tussen de onderdelen. Omdat producenten en consumenten niet op de hoogte van elkaar zijn, kan een gebruiker zonder gevolgen voor de producent worden bijgewerkt.

### <a name="create-queues"></a>Wachtrijen maken

Maken van wachtrijen met de [Azure-portal](service-bus-quickstart-portal.md), [PowerShell](service-bus-quickstart-powershell.md), [CLI](service-bus-quickstart-cli.md), of [Resource Manager-sjablonen](service-bus-resource-manager-namespace-queue.md). U vervolgens verzenden en ontvangen berichten via een [QueueClient](/dotnet/api/microsoft.azure.servicebus.queueclient) object. 

Zie snel informatie over het maken van een wachtrij en vervolgens verzenden en ontvangen van berichten naar en uit de wachtrij, het [snelstartgidsen](service-bus-quickstart-portal.md) voor elke methode. Zie voor een uitgebreidere zelfstudie over het gebruik van wachtrijen, [aan de slag met Service Bus-wachtrijen](service-bus-dotnet-get-started-with-queues.md). 

Zie voor een voorbeeld werken de [BasicSendReceiveUsingQueueClient voorbeeld](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/GettingStarted/Microsoft.Azure.ServiceBus/BasicSendReceiveUsingQueueClient) op GitHub.

### <a name="receive-modes"></a>Modi ontvangen

U kunt twee verschillende modi waarin de Service Bus berichten ontvangt opgeven: *ReceiveAndDelete* of *PeekLock*. In de [ReceiveAndDelete](/dotnet/api/microsoft.azure.servicebus.receivemode) modus kan de bewerking receive is één; dat wil zeggen als Service Bus de aanvraag ontvangt, wordt het bericht als verbruikt gemarkeerd en naar de toepassing wordt geretourneerd. **ReceiveAndDelete** modus is het eenvoudigste model en werkt het beste voor scenario's waarin de toepassing niet verwerken een bericht tolereren kan als er een fout optreedt. Om dit scenario te begrijpen, stelt u zich een scenario waarin de consument de receive-aanvraag indient en vervolgens vastloopt voordat het wordt verwerkt. Omdat Service Bus het bericht als verbruikt, wanneer de toepassing opnieuw wordt opgestart en verbruik van berichten opnieuw begint gemarkeerd, ontbreekt het bericht dat voor het vastlopen is verbruikt.

In [PeekLock](/dotnet/api/microsoft.azure.servicebus.receivemode) modus, de ontvangstbewerking verandert in twee fasen, waardoor het mogelijk voor de ondersteuning voor toepassingen die geen ontbrekende berichten kunnen tolereren. Als Service Bus de aanvraag ontvangt, het zoeken naar het volgende bericht wordt verbruikt, wordt vergrendeld om te voorkomen dat andere consumenten het ontvangen en vervolgens terugkeert naar de toepassing. Nadat de toepassing klaar is met verwerking van het bericht (of veilig heeft opgeslagen voor toekomstige verwerking), is de tweede fase van het ontvangstproces voltooid door het aanroepen van [CompleteAsync](/dotnet/api/microsoft.azure.servicebus.queueclient.completeasync) voor het ontvangen bericht. Wanneer Service Bus de aanroep de **CompleteAsync** aanroep, markeert deze het bericht als verbruikt.

Als de toepassing de kan bericht niet verwerken voor een bepaalde reden wordt, kan worden aangeroepen de [AbandonAsync](/dotnet/api/microsoft.azure.servicebus.queueclient.abandonasync) methode voor het ontvangen bericht (in plaats van [CompleteAsync](/dotnet/api/microsoft.azure.servicebus.queueclient.completeasync)). Deze methode kunt Service Bus het bericht ontgrendelt en moet opnieuw worden ontvangen door de dezelfde gebruiker of door een andere concurrerend consumenten beschikbaar maken. Ten tweede, er is een time-out gekoppeld aan de vergrendeling en als de toepassing niet kan verwerken van het bericht voordat de time-out van de vergrendeling verloopt (bijvoorbeeld als de toepassing vastloopt), en vervolgens de Service Bus het bericht ontgrendelt en wordt het beschikbaar zijn voor ontvangen opnieuw) in wezen uitvoeren van een [AbandonAsync](/dotnet/api/microsoft.azure.servicebus.queueclient.abandonasync) bewerking standaard).

In het geval dat de toepassing is vastgelopen na het verwerken van het bericht, maar voordat de **CompleteAsync** verzoek is uitgegeven, wordt het bericht is opnieuw bij de toepassing bezorgd wanneer opnieuw wordt gestart. Dit proces wordt vaak genoemd *tenminste eenmaal* verwerking; dat wil zeggen, elk bericht ten minste één keer wordt verwerkt. In bepaalde situaties kan hetzelfde bericht worden geleverd. Als het scenario kan geen dubbele verwerken tolereren en vervolgens extra logica is vereist in de toepassing voor het detecteren van duplicaten, die kan worden bereikt op basis van de [MessageId](/dotnet/api/microsoft.azure.servicebus.message.messageid) eigenschap van het bericht dat via constant is gebleven bezorgingspogingen. Deze functie wordt ook wel *exact één keer* verwerken.

## <a name="topics-and-subscriptions"></a>Onderwerpen en abonnementen

In tegenstelling tot wachtrijen, waarin elk bericht wordt verwerkt door een enkele gebruiker *onderwerpen* en *abonnementen* biedt een een-op-veel-vorm van communicatie, in een *publiceren/abonneren* patroon. Kan handig zijn voor het schalen van grote aantallen ontvangers, elk gepubliceerde bericht beschikbaar wordt gesteld aan elk abonnement dat is geregistreerd bij het onderwerp. Berichten worden verzonden naar een onderwerp en geleverd aan een of meer gekoppelde abonnementen, afhankelijk van filterregels kunnen worden ingesteld op basis van per abonnement. De abonnementen kunnen meer filters gebruiken om te beperken van de berichten die ze willen ontvangen. Berichten worden verzonden naar een onderwerp op dezelfde manier als ze worden verzonden naar een wachtrij, maar berichten worden niet ontvangen van het onderwerp rechtstreeks. In plaats daarvan worden ze ontvangen van abonnementen. Het onderwerpabonnement van een lijkt op een virtuele wachtrij die kopieën van de berichten die worden verzonden naar het onderwerp ontvangt. Berichten worden ontvangen van een abonnement identiek aan de manier waarop die ze worden ontvangen van een wachtrij.

Ter vergelijking, de functionaliteit bericht verzenden van een wachtrij rechtstreeks aan een onderwerp en de functionaliteit hiervan bericht ontvangen die zijn toegewezen aan een abonnement. Deze functie betekent onder andere dat abonnementen dezelfde patronen die eerder in deze sectie met betrekking tot wachtrijen beschreven ondersteunen: concurrerend consumenten, tijdelijke ontkoppeling, load leveling en taakverdeling.

### <a name="create-topics-and-subscriptions"></a>Onderwerpen en abonnementen maken

Maken van een onderwerp is vergelijkbaar met het maken van een wachtrij, zoals beschreven in de vorige sectie. U vervolgens berichten verzenden met de [TopicClient](/dotnet/api/microsoft.azure.servicebus.topicclient) klasse. Voor het ontvangen van berichten, maakt u een of meer abonnementen naar het onderwerp. Vergelijkbaar met wachtrijen, berichten worden ontvangen van een abonnement met een [SubscriptionClient](/dotnet/api/microsoft.azure.servicebus.subscriptionclient) object in plaats van een [QueueClient](/dotnet/api/microsoft.azure.servicebus.queueclient) object. Maak de abonnement-client, waarbij de naam van het onderwerp, de naam van het abonnement en (optioneel) de receive-modus als parameters doorgegeven. 

Voor een volledige werken voorbeeld, Zie de [BasicSendReceiveUsingTopicSubscriptionClient voorbeeld](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/GettingStarted/Microsoft.Azure.ServiceBus/BasicSendReceiveUsingTopicSubscriptionClient) op Github.

### <a name="rules-and-actions"></a>Regels en acties

In veel scenario's, moeten de berichten met specifieke kenmerken op verschillende manieren worden verwerkt. U kunt abonnementen op berichten zoeken die zijn gewenst eigenschappen en klik vervolgens op bepaalde wijzigingen aanbrengen in de eigenschappen configureren zodat deze verwerking. Bij het Service Bus-abonnementen Zie alle berichten die naar het onderwerp, kunt u alleen een subset van die berichten kopiëren naar de abonnementenwachtrij voor virtuele. Dit filteren wordt gerealiseerd met abonnementsfilters. Dergelijke wijzigingen worden genoemd *filteren acties*. Wanneer een abonnement is gemaakt, kunt u een filterexpressie die wordt toegepast op de eigenschappen van het bericht, de Systeemeigenschappen opgeven (bijvoorbeeld **Label**) en aangepaste toepassingseigenschappen (bijvoorbeeld  **StoreName**.) De filterexpressie SQL is optioneel in dit geval; zonder een filterexpressie SQL wordt geen filteractie gedefinieerd voor een abonnement worden uitgevoerd op alle berichten voor dat abonnement.

Voor een volledige werken voorbeeld, Zie de [TopicSubscriptionWithRuleOperationsSample voorbeeld](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/GettingStarted/Microsoft.Azure.ServiceBus/TopicSubscriptionWithRuleOperationsSample) op GitHub.

Zie voor meer informatie over mogelijke filterwaarden, de documentatie voor de [SqlFilter](/dotnet/api/microsoft.azure.servicebus.sqlfilter) en [SqlRuleAction](/dotnet/api/microsoft.azure.servicebus.sqlruleaction) klassen. 

## <a name="next-steps"></a>Volgende stappen

Zie de volgende geavanceerde onderwerpen voor meer informatie en voorbeelden van het gebruik van Service Bus-berichtenservice:

* [Overzicht van Service Bus-berichten](service-bus-messaging-overview.md)
* [Snelstartgids: Verzenden en ontvangen berichten met behulp van de Azure-portal en .NET](service-bus-quickstart-portal.md)
* [Zelfstudie: Bijwerken inventarisatie met Azure portal en onderwerpen/abonnementen](service-bus-tutorial-topics-subscriptions-portal.md)


