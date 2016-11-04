---
title: Een Service Bus-naamruimte maken met behulp van de Azure-portal | Microsoft Docs
description: U hebt een naamruimte nodig om aan de slag te gaan met Service Bus. Zo maakt u een naamruimte in de Azure-portal.
services: service-bus
documentationcenter: .net
author: jtaubensee
manager: timlt
editor: ''

ms.service: service-bus
ms.devlang: tbd
ms.topic: get-started-article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 08/22/2016
ms.author: jotaub

---
# <a name="create-a-service-bus-namespace-using-the-azure-portal"></a>Een Service Bus-naamruimte maken met de Azure-portal
Een naamruimte is een algemene container voor alle berichtenonderdelen. Er kunnen zich meerdere wachtrijen en onderwerpen in één enkele naamruimte bevinden, en naamruimten fungeren vaak als toepassingscontainers. Er zijn momenteel twee verschillende manieren om een Service Bus-naamruimte te maken.

1. Azure-portal (dit artikel)
2. [Resource Manager-sjablonen][create-namespace-using-arm]

## <a name="create-a-namespace-in-the-azure-portal"></a>Een naamruimte in de Azure-portal maken
[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

Gefeliciteerd. U hebt nu een naamruimte voor Service Bus-berichten gemaakt.

## <a name="next-steps"></a>Volgende stappen
Ga naar onze [GitHub-voorbeelden][github-voorbeelden] die enkele meer geavanceerde functies van Azure Service Bus-berichten tonen.

[create-namespace-using-arm]: service-bus-resource-manager-overview.md
[github-samples]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples


<!--HONumber=Oct16_HO3-->


