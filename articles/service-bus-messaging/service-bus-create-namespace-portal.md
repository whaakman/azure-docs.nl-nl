---
title: Een Service Bus-naamruimte maken met Azure Portal | Microsoft Docs
description: Een Service Bus-naamruimte maken met Azure Portal.
services: service-bus-messaging
documentationcenter: .net
author: axisc
manager: timlt
editor: spelluru
ms.assetid: fbb10e62-b133-4851-9d27-40bd844db3ba
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: get-started-article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 01/23/2019
ms.author: aschhab
ms.openlocfilehash: c456fa392c3ba07ad26e300eff07a89fa849b354
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54856312"
---
# <a name="create-a-service-bus-namespace-using-the-azure-portal"></a>Een Service Bus-naamruimte maken met Azure Portal

Een naamruimte is een scoping container voor alle berichtenonderdelen. Er kunnen zich meerdere wachtrijen en onderwerpen in één naamruimte bevinden, en naamruimten fungeren vaak als toepassingscontainers. Er zijn twee manieren om een Service Bus-naamruimte te maken:

1. Azure Portal (dit artikel)
2. [Resource Manager-sjablonen][create-namespace-using-arm]

## <a name="create-a-namespace-in-the-azure-portal"></a>Een naamruimte in Azure Portal maken

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

Gefeliciteerd. U hebt nu een naamruimte voor Service Bus-berichten gemaakt.

## <a name="next-steps"></a>Volgende stappen

Bekijk de [GitHub-voorbeelden][github-samples] van Service Bus, die een aantal geavanceerdere functies van de Azure Service Bus Messaging-service tonen.

[create-namespace-using-arm]: service-bus-resource-manager-overview.md
[github-samples]: https://github.com/Azure/azure-service-bus/tree/master/samples
