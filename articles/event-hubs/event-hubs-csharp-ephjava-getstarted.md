---
title: Aan de slag met Event Hubs in C# | Microsoft Docs
description: Volg deze zelfstudie om aan de slag te gaan met Azure Event Hubs; verzenden van gebeurtenissen in C# en ze in Java ontvangen met behulp van de EventProcessorHost.
services: event-hubs
documentationcenter: 
author: jtaubensee
manager: timlt
editor: 
ms.assetid: 059fb733-a397-400e-8e43-0c7ea5930b8b
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 01/04/2017
ms.author: jotaub;sethm
translationtype: Human Translation
ms.sourcegitcommit: c9aa7cebab12eafe71476ee45ef578d3201e69e5
ms.openlocfilehash: e7c38aa5a778d5e8a653e753e5794d7c7da31dc1


---
# <a name="get-started-with-event-hubs"></a>Aan de slag met Event Hubs
[!INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>Inleiding
Event Hubs is een service die grote hoeveelheden gebeurtenisgegevens (telemetrie) van verbonden apparaten en toepassingen verwerkt. Nadat u gegevens in Event Hubs hebt verzameld, kunt u de gegevens opslaan met behulp van een opslagcluster of transformeren met een provider van realtime-analyses. Deze functie voor grootschalige gebeurtenisverzameling en -verwerking is een belangrijk onderdeel van de architectuur van moderne toepassingen, met inbegrip van het Internet der dingen (IoT).

In deze zelfstudie kunt u zien hoe u Azure Portal gebruikt om een Event Hub te maken. U ziet ook hoe u berichten in een Event Hub verzamelt met een consoletoepassing die is geschreven in C# en hoe u deze berichten parallel ophaalt met de Java Event Processor Host-bibliotheek.

Om deze handleiding volledig door te kunnen nemen, hebt u het volgende nodig:

* [Microsoft Visual Studio](http://visualstudio.com)
* Een actief Azure-account. <br/>Als u geen Azure-account hebt, kunt u binnen een paar minuten een gratis account maken. Zie [Gratis proefversie van Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F target="_blank") voor meer informatie.

[!INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[!INCLUDE [service-bus-event-hubs-get-started-send-csharp](../../includes/service-bus-event-hubs-get-started-send-csharp.md)]

[!INCLUDE [service-bus-event-hubs-get-started-receive-ephjava](../../includes/service-bus-event-hubs-get-started-receive-ephjava.md)]

## <a name="run-the-applications"></a>De toepassingen uitvoeren
U kunt nu de toepassingen gaan uitvoeren.

1. Voer het project **Ontvanger** uit.
   
   ![][21]
2. Voer het project **Afzender** uit.
   
   ![][22]

## <a name="next-steps"></a>Volgende stappen
Nu u een werkende toepassing hebt gebouwd die een Event Hub maakt en gegevens verzendt en ontvangt, kunt u naar de volgende scenario's gaan:

* Een complete [voorbeeldtoepassing die gebruikmaakt van Event Hubs][sample application that uses Event Hubs].
* Het voorbeeld [Verwerking van de gebeurtenis Uitschalen met Event Hubs][Scale out Event Processing with Event Hubs].
* [Event Hubs-overzicht][Event Hubs overview]

<!-- Images. -->
[21]: ./media/event-hubs-csharp-ephjava-getstarted/ephjava.png
[22]: ./media/event-hubs-csharp-ephjava-getstarted/cs-send.png

<!-- Links -->
[Event Hubs overview]: event-hubs-overview.md
[sample application that uses Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Scale out Event Processing with Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3



<!--HONumber=Dec16_HO1-->


