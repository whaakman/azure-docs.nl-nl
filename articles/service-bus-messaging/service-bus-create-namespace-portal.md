---
title: Een Service Bus-naamruimte maken met behulp van Azure Portal | Microsoft Docs
description: Lees hoe u een Service Bus-naamruimte maakt met Azure Portal.
services: service-bus-messaging
documentationcenter: .net
author: jtaubensee
manager: timlt
editor: 
ms.assetid: fbb10e62-b133-4851-9d27-40bd844db3ba
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: get-started-article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 11/30/2016
ms.author: jotaub
translationtype: Human Translation
ms.sourcegitcommit: 4a972b9b8b52a90f27afda98d8bdc661016d1fe1
ms.openlocfilehash: ba7093f8a2c06ab4cecf11207174a4871435ae64
ms.lasthandoff: 02/28/2017


---
# <a name="create-a-service-bus-namespace-using-the-azure-portal"></a>Een Service Bus-naamruimte maken met de Azure-portal
Een naamruimte is een algemene container voor alle berichtenonderdelen. Er kunnen zich meerdere wachtrijen en onderwerpen in één enkele naamruimte bevinden, en naamruimten fungeren vaak als toepassingscontainers. Er zijn momenteel twee verschillende manieren om een Service Bus-naamruimte te maken.

1. Azure-portal (dit artikel)
2. [Resource Manager-sjablonen][create-namespace-using-arm]

## <a name="create-a-namespace-in-the-azure-portal"></a>Een naamruimte in de Azure-portal maken
[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

Gefeliciteerd. U hebt nu een naamruimte voor Service Bus-berichten gemaakt.

## <a name="next-steps"></a>Volgende stappen
Bekijk onze [GitHub-voorbeelden][github-samples] die enkele meer geavanceerde functies van de Azure Service Bus-berichtenservice tonen.

[create-namespace-using-arm]: service-bus-resource-manager-overview.md
[github-samples]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples

