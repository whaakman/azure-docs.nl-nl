---
title: Een Service Bus-naamruimte maken met de Azure-portal | Microsoft Docs
description: Een Service Bus-naamruimte maken met de Azure-portal.
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
ms.date: 06/27/2017
ms.author: sethm
ms.translationtype: Human Translation
ms.sourcegitcommit: 857267f46f6a2d545fc402ebf3a12f21c62ecd21
ms.openlocfilehash: ffb134085c8555b22a317213622ca6c9490497d8
ms.contentlocale: nl-nl
ms.lasthandoff: 06/28/2017

---
# <a name="create-a-service-bus-namespace-using-the-azure-portal"></a>Een Service Bus-naamruimte maken met de Azure-portal

Een naamruimte is een scoping container voor alle berichtenonderdelen. Er kunnen zich meerdere wachtrijen en onderwerpen in één naamruimte bevinden, en naamruimten fungeren vaak als toepassingscontainers. Er zijn twee manieren om een Service Bus-naamruimte te maken:

1. Azure-portal (dit artikel)
2. [Resource Manager-sjablonen][create-namespace-using-arm]

## <a name="create-a-namespace-in-the-azure-portal"></a>Een naamruimte in de Azure-portal maken

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

Gefeliciteerd. U hebt nu een naamruimte voor Service Bus-berichten gemaakt.

## <a name="next-steps"></a>Volgende stappen

Bekijk onze [GitHub-voorbeelden][github-samples], die enkele meer geavanceerde functies van de Azure Service Bus Messaging-service tonen.

[create-namespace-using-arm]: service-bus-resource-manager-overview.md
[github-samples]: https://github.com/Azure/azure-service-bus/tree/master/samples

