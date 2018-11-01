---
title: Azure Service Bus dubbele berichten detecteren | Microsoft Docs
description: Detecteren van dubbele Service Bus-berichten
services: service-bus-messaging
documentationcenter: ''
author: clemensv
manager: timlt
editor: ''
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2018
ms.author: spelluru
ms.openlocfilehash: 5241020b1db3797891ae13da54cc9225bbd4619b
ms.sourcegitcommit: ae45eacd213bc008e144b2df1b1d73b1acbbaa4c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2018
ms.locfileid: "50741264"
---
# <a name="duplicate-detection"></a>Detectie van duplicaten

Als een toepassing vanwege mislukt een fatale fout onmiddellijk na het verzendt een bericht en het exemplaar van de opnieuw gestart per ongeluk is van mening dat de levering van de voorafgaande berichten zich niet heeft voorgedaan, een latere verzenden zorgt ervoor dat hetzelfde bericht in twee keer worden weergegeven in het systeem.

Het is ook mogelijk dat een fout op het niveau van de client of het netwerk eerder even optreden en geretourneerd naar de client voor een verzonden bericht worden vastgelegd in de wachtrij, met de bevestiging is mislukt. In dit scenario maakt de client bij twijfel over de uitkomst van de bewerking voor het verzenden.

Detectie van duplicaten neemt de twijfel buiten deze situaties door in te schakelen van de afzender heeft hetzelfde bericht en de wachtrij of onderwerp verwijdert alle dubbele exemplaren.

Het inschakelen van detectie van duplicaten helpt met het bijhouden van de toepassing gecontroleerde *MessageId* van alle berichten die worden verzonden naar een wachtrij of onderwerp tijdens een opgegeven periode. Als er een nieuw bericht wordt verzonden met *MessageId* die is vastgelegd tijdens de periode, het bericht wordt gerapporteerd als geaccepteerd (de bewerking voor het verzenden is geslaagd), maar de onlangs verzonden bericht direct wordt genegeerd en verwijderd. Er zijn geen andere onderdelen van het bericht dat andere dan de *MessageId* worden beschouwd.

Toepassingsbeheer van de id is het essentieel is, omdat alleen dat de toepassing om te koppelen in staat stelt de *MessageId* aan een proces bedrijfscontext waaruit deze opnieuw worden voorspelbare samengesteld wanneer er een fout optreedt.

Voor een bedrijfsproces waarin meerdere berichten worden verzonden tijdens het verwerken van de context van een toepassing, de *MessageId* mogelijk een samenstelling van de id van de context op toepassingsniveau, zoals een aankoopordernummer en de onderwerp van het bericht, bijvoorbeeld **12345.2017/betaling**.

De *MessageId* kunnen altijd worden sommige GUID, maar de id voor het bedrijfsproces verankering levert voorspelbare herhaalbaarheid, die vereist is voor een effectief gebruik te maken van de functie voor detectie van duplicaten.

## <a name="enable-duplicate-detection"></a>Duplicaatdetectie inschakelen

In de portal voor de functie is ingeschakeld tijdens het maken van een entiteit met de **duplicaatdetectie inschakelen** selectievakje standaard uitgeschakeld is. De instelling voor het maken van nieuwe onderwerpen is gelijk.

![][1]

> [!IMPORTANT]
> U kunt geen in-of uitschakelen detectie van duplicaten nadat de wachtrij is gemaakt. U kunt dit alleen doen op het moment van het maken van de wachtrij. 

Via een programma, het instellen van de markering op in de [QueueDescription.requiresDuplicateDetection](/dotnet/api/microsoft.servicebus.messaging.queuedescription.requiresduplicatedetection#Microsoft_ServiceBus_Messaging_QueueDescription_RequiresDuplicateDetection) eigenschap op het volledige .NET API-framework. Met de Azure Resource Manager-API, de waarde is ingesteld met de [queueProperties.requiresDuplicateDetection](/azure/templates/microsoft.servicebus/namespaces/queues#property-values) eigenschap.

De geschiedenis van detectie van duplicaten tijd standaard ingesteld op 30 seconden voor wachtrijen en onderwerpen, met een maximale waarde van zeven dagen. U kunt deze instelling in het venster van de eigenschappen wachtrij en onderwerp in de Azure-portal wijzigen.

![][2]

Via een programma, kunt u de grootte van de detectie van duplicaten venster gedurende welke de bericht-id's worden bewaard, met behulp van de [QueueDescription.DuplicateDetectionHistoryTimeWindow](/dotnet/api/microsoft.servicebus.messaging.queuedescription.duplicatedetectionhistorytimewindow#Microsoft_ServiceBus_Messaging_QueueDescription_DuplicateDetectionHistoryTimeWindow) eigenschap met de volledige .NET Framework-API . Met de Azure Resource Manager-API, de waarde is ingesteld met de [queueProperties.duplicateDetectionHistoryTimeWindow](/azure/templates/microsoft.servicebus/namespaces/queues#property-values) eigenschap.

Duplicaatdetectie inschakelen en de grootte van het venster rechtstreeks van invloed zijn op de doorvoer van de wachtrij (en onderwerp), omdat alle vastgelegde bericht-id's moeten worden vergeleken met de nieuw ingediende bericht-id.

Lager beïnvloed te houden van het venster kleine betekent dat minder bericht-id's moeten worden bewaard en vergeleken, en de doorvoer is. Voor hoge doorvoer entiteiten waarvoor de detectie van duplicaten, moet u het venster zo klein mogelijk houden.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Service Bus-berichten, de volgende onderwerpen:

* [Service Bus-wachtrijen, -onderwerpen en -abonnementen](service-bus-queues-topics-subscriptions.md)
* [Aan de slag met Service Bus-wachtrijen](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus-onderwerpen en -abonnementen gebruiken](service-bus-dotnet-how-to-use-topics-subscriptions.md)

[1]: ./media/duplicate-detection/create-queue.png
[2]: ./media/duplicate-detection/queue-prop.png
