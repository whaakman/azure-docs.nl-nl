---
title: Azure Functions voor foutafhandeling richtlijnen | Microsoft Docs
description: Bevat algemene richtlijnen voor het afhandelen van fouten die optreden in tijdens het uitvoeren van uw functies en koppelingen naar onderwerpen binding-specifieke fouten.
services: functions
cloud: ''
documentationcenter: ''
author: craigshoemaker
manager: jeconnoc
ms.assetid: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
origin.date: 02/01/2018
ms.date: 11/22/2018
ms.author: v-junlch
ms.openlocfilehash: bf54d312de5625a7fa44cea4d5107e83cf15583c
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "62105505"
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

<!-- Update_Description: update metedata properties -->