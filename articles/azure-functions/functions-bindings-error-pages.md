---
title: Azure Functions voor foutafhandeling richtlijnen | Microsoft Docs
description: Bevat algemene richtlijnen voor het afhandelen van fouten die optreden in tijdens het uitvoeren van uw functies en koppelingen naar onderwerpen binding-specifieke fouten.
services: functions
cloud: ''
documentationcenter: ''
author: craigshoemaker
manager: gwallace
ms.assetid: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 02/01/2018
ms.author: cshoe
ms.openlocfilehash: ef8f2d5a63f7924097362f6aa0ebc78cc0f6455f
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/01/2019
ms.locfileid: "67480712"
---
# <a name="azure-functions-error-handling"></a>Azure Functions-foutafhandeling

Dit onderwerp bevat algemene richtlijnen voor het afhandelen van fouten die optreden tijdens het uitvoeren van uw functies. Het bevat ook koppelingen naar de onderwerpen over binding-specifieke fouten die zich kunnen voordoen. 

## <a name="handing-errors-in-functions"></a>Verwerking van fouten in functies
[!INCLUDE [bindings errors intro](../../includes/functions-bindings-errors-intro.md)]

 
## <a name="binding-error-codes"></a>Binding-foutcodes

Bij het integreren met Azure-services, kunt u fouten gegenereerd die afkomstig van de API's van de onderliggende services zijn mogelijk. Koppelingen naar de fout documentatie-code voor deze services kunnen u vinden in de **uitzonderingen en retourcodes** sectie van de volgende trigger en onderwerpen met naslaginformatie binding:

+ [Azure Cosmos DB](functions-bindings-cosmosdb.md#exceptions-and-return-codes)

+ [Blob Storage](functions-bindings-storage-blob.md#exceptions-and-return-codes)

+ [Event Hubs](functions-bindings-event-hubs.md#exceptions-and-return-codes)

+ [Notification Hubs](functions-bindings-notification-hubs.md#exceptions-and-return-codes)

+ [Queue Storage](functions-bindings-storage-queue.md#exceptions-and-return-codes)

+ [Service Bus](functions-bindings-service-bus.md#exceptions-and-return-codes)

+ [Table Storage](functions-bindings-storage-table.md#exceptions-and-return-codes)
