---
title: Azure Service Bus voor de detectie van dubbele berichten | Microsoft Docs
description: Detecteren van dubbele Service Bus-berichten
services: service-bus-messaging
documentationcenter: 
author: clemensv
manager: timlt
editor: 
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/27/2017
ms.author: sethm
ms.openlocfilehash: 91a6e62a03ffe39e456129ea78821250b65091e4
ms.sourcegitcommit: 6acb46cfc07f8fade42aff1e3f1c578aa9150c73
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/18/2017
---
# <a name="duplicate-detection"></a>Detectie van duplicaten

Als er optreedt bij een toepassing een fatale fout onmiddellijk na verzendt een bericht en het exemplaar van de opnieuw gestart daarom ten onrechte gelooft dat de levering van de voorafgaande berichten niet optreedt, een latere verzenden zorgt ervoor dat hetzelfde bericht worden weergegeven in het systeem twee keer.

Het is ook mogelijk voor een fout op het niveau van client- of eerder even optreden, en voor een bericht verzonden om te worden doorgevoerd in de wachtrij met de bevestiging die niet met succes terug naar de client te maken. Dit scenario maakt de client bij twijfel over de uitkomst van de verzendbewerking.

Detectie van duplicaten neemt de twijfel buiten deze situaties doordat de afzender opnieuw verzenden hetzelfde bericht en de wachtrij of onderwerp verwijdert alle dubbele exemplaren.

Het inschakelen van de detectie van duplicaten helpt bijhouden van de toepassing gecontroleerde *MessageId* van alle berichten die worden verzonden naar een wachtrij of onderwerp tijdens een opgegeven periode. Als een nieuw bericht wordt verzonden met een *MessageId* die al is geregistreerd tijdens de periode, het bericht wordt gerapporteerd als geaccepteerd (de verzendbewerking is geslaagd), maar het zojuist verzonden bericht onmiddellijk wordt genegeerd en verwijderd. Er zijn geen andere onderdelen van het bericht anders dan de *MessageId* worden beschouwd.

Besturingselementen van de toepassing van de id is essentieel omdat alleen waarmee de toepassing om te koppelen de *MessageId* naar een proces bedrijfscontext waaruit deze opnieuw worden voorspelbare samengesteld in geval van een storing.

Voor een bedrijfsproces waarin meerdere berichten worden verzonden tijdens het verwerken van de context van een toepassing, de *MessageId* mogelijk een samenstelling van de op toepassingsniveau context-id, zoals het nummer van de inkooporder, en de onderwerp van het bericht; bijvoorbeeld: **12345.2017/betaling**.

De *MessageId* kunnen altijd worden sommige GUID, maar de id van het bedrijfsproces verankeren levert het voorspelbare herhaalbaarheid die nodig is voor een effectief gebruik te maken van de detectie van duplicaten-functie.

## <a name="enable-duplicate-detection"></a>Detectie van dubbele inschakelen

In de portal voor de functie is ingeschakeld tijdens het maken van de entiteit met de **inschakelen detectie van duplicaten** selectievakje standaard uitgeschakeld is. De instelling voor het maken van nieuwe onderwerpen is gelijk.

![][1]

Via een programma, het instellen van de markering op in de [QueueDescription.requiresDuplicateDetection](/dotnet/api/microsoft.servicebus.messaging.queuedescription.requiresduplicatedetection#Microsoft_ServiceBus_Messaging_QueueDescription_RequiresDuplicateDetection) -eigenschap op het volledige framework .NET API. Met de Azure Resource Manager-API, de waarde is ingesteld met de [queueProperties.requiresDuplicateDetection](/azure/templates/microsoft.servicebus/namespaces/queues#property-values) eigenschap.

De geschiedenis van de detectie van duplicaten tijd standaard ingesteld op 30 seconden voor wachtrijen en onderwerpen, met een maximale waarde van 7 dagen. U kunt deze instelling in het venster van de eigenschappen wachtrij en onderwerp in de Azure portal kunt wijzigen.

![][2]

Via een programma, kunt u de grootte van de detectie van duplicaten venster gedurende welke de bericht-id's worden bewaard, met behulp van de [QueueDescription.DuplicateDetectionHistoryTimeWindow](/dotnet/api/microsoft.servicebus.messaging.queuedescription.duplicatedetectionhistorytimewindow#Microsoft_ServiceBus_Messaging_QueueDescription_DuplicateDetectionHistoryTimeWindow) eigenschap met de volledige .NET Framework-API . Met de Azure Resource Manager-API, de waarde is ingesteld met de [queueProperties.duplicateDetectionHistoryTimeWindow](/azure/templates/microsoft.servicebus/namespaces/queues#property-values) eigenschap.

Let op: inschakelen van de detectie van duplicaten en de grootte van het venster direct invloed op de doorvoer van wachtrij (en onderwerp), omdat alle vastgelegde bericht-id's moeten worden vergeleken met de nieuwe verzonden bericht-id.

Het venster kleine betekent dat minder bericht-id's moeten worden bewaard en overeenkomende en doorvoer is houden beïnvloed kleiner. Voor hoge doorvoersnelheid entiteiten waarvoor de detectie van duplicaten, moet u het venster zo klein mogelijk behouden.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Service Bus-berichtenservice, de volgende onderwerpen:

* [Grondbeginselen van Service Bus](service-bus-fundamentals-hybrid-solutions.md)
* [Service Bus-wachtrijen, -onderwerpen en -abonnementen](service-bus-queues-topics-subscriptions.md)
* [Aan de slag met Service Bus-wachtrijen](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus-onderwerpen en -abonnementen gebruiken](service-bus-dotnet-how-to-use-topics-subscriptions.md)

[1]: ./media/duplicate-detection/create-queue.png
[2]: ./media/duplicate-detection/queue-prop.png
