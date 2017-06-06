---
title: Een Service Bus-naamruimte maken met behulp van Azure Portal | Microsoft Docs
description: Lees hoe u een Service Bus-naamruimte maakt met Azure Portal.
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: fbb10e62-b133-4851-9d27-40bd844db3ba
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: get-started-article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 03/23/2017
ms.author: sethm
ms.translationtype: Human Translation
ms.sourcegitcommit: 0bec803e4b49f3ae53f2cc3be6b9cb2d256fe5ea
ms.openlocfilehash: 5de92033eb7eb4fef8d27a215b3284ab80594065
ms.contentlocale: nl-nl
ms.lasthandoff: 03/24/2017


---
# <a name="create-a-service-bus-namespace-using-the-azure-portal"></a>Een Service Bus-naamruimte maken met de Azure-portal
Een naamruimte is een algemene container voor alle berichtenonderdelen. Er kunnen zich meerdere wachtrijen en onderwerpen in één naamruimte bevinden, en naamruimten fungeren vaak als toepassingscontainers. Er zijn twee manieren om een Service Bus-naamruimte te maken:

1. Azure-portal (dit artikel)
2. [Resource Manager-sjablonen][create-namespace-using-arm]

## <a name="create-a-namespace-in-the-azure-portal"></a>Een naamruimte in de Azure-portal maken
[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

Gefeliciteerd. U hebt nu een naamruimte voor Service Bus-berichten gemaakt.

## <a name="next-steps"></a>Volgende stappen
Bekijk onze [GitHub-voorbeelden][github-samples] die enkele meer geavanceerde functies van de Azure Service Bus-berichtenservice tonen.

[create-namespace-using-arm]: service-bus-resource-manager-overview.md
[github-samples]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples

