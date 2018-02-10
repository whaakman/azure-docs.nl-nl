---
title: Azure Functions foutafhandeling richtlijnen | Microsoft Docs
description: Bevat algemene richtlijnen voor het afhandelen van fouten die optreden in tijdens het uitvoeren van uw functies en koppelingen naar onderwerpen binding-specifieke fouten.
services: functions
cloud: 
documentationcenter: 
author: ggailey777
manager: cfowler
ms.assetid: 
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: multiple
ms.topic: article
ms.date: 02/01/2018
ms.author: glenga; cfowler
ms.openlocfilehash: 82cdc62b3070811186583fdf1ce5e6ce421ebc34
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2018
---
# <a name="azure-functions-error-handling"></a>Azure Functions foutafhandeling

Dit onderwerp bevat algemene richtlijnen voor het afhandelen van fouten die optreden tijdens het uitvoeren van uw functies. Het bevat ook koppelingen naar de onderwerpen over binding-specifieke fouten die zich kunnen voordoen. 

## <a name="handing-errors-in-functions"></a>Fouten in functies overdragen
[!INCLUDE [bindings errors intro](../../includes/functions-bindings-errors-intro.md)]

 
## <a name="binding-error-codes"></a>Binding-foutcodes

Bij het integreren met Azure-services, kunt u deze gebeurtenis treedt op fouten die afkomstig van de API's van de onderliggende services zijn mogelijk. Koppelingen naar de fout documentatie code voor deze services kunnen worden gevonden in de **uitzonderingen en retourcodes** sectie van de volgende trigger en naslaginformatie binding:

+ [Azure Cosmos DB](functions-bindings-cosmosdb.md#exceptions-and-return-codes)

+ [Blob Storage](functions-bindings-storage-blob.md#exceptions-and-return-codes)

+ [Event Hubs](functions-bindings-event-hubs.md#exceptions-and-return-codes)

+ [Notification Hubs](functions-bindings-notification-hubs.md#exceptions-and-return-codes)

+ [Queue Storage](functions-bindings-storage-queue.md#exceptions-and-return-codes)

+ [Service Bus](functions-bindings-service-bus.md#exceptions-and-return-codes)

+ [Table Storage](functions-bindings-storage-table.md#exceptions-and-return-codes)
