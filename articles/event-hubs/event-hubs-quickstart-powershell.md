---
title: 'Azure-snelstart: Een event hub maken met behulp van PowerShell | Microsoft Docs'
description: In deze snelstart wordt beschreven hoe u een event hub maakt met behulp van Azure PowerShell en vervolgens gebeurtenissen verzendt en ontvangt met behulp van .NET Standard SDK.
services: event-hubs
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.service: event-hubs
ms.devlang: na
ms.topic: quickstart
ms.custom: mvc
ms.date: 08/16/2018
ms.author: shvija
ms.openlocfilehash: 9f0a6a910ab9b9589e09b6c8e1e7b16f7e63c5c8
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51258966"
---
# <a name="quickstart-create-an-event-hub-using-azure-powershell"></a>Snelstart: Een event hub maken met behulp van Azure PowerShell

Azure Event Hubs is een big data-platform voor het streamen van gegevens en een gebeurtenisopneemservice die miljoenen gebeurtenissen per seconde kan opnemen en verwerken. Event Hubs kan gebeurtenissen, gegevens of telemetrie die wordt geproduceerd door gedistribueerde software en apparaten verwerken en opslaan. Gegevens die naar een Event Hub worden verzonden, kunnen worden omgezet en opgeslagen via een provider voor realtime analytische gegevens of batchverwerking/opslagadapters. Zie [Overzicht van Event Hubs](event-hubs-about.md) en [Functies van Event Hubs](event-hubs-features.md) voor een gedetailleerd overzicht van Event Hubs.

In deze snelstart maakt u een Event Hub met behulp van Azure PowerShell.

## <a name="prerequisites"></a>Vereisten

Het volgende moet zijn geïnstalleerd om deze zelfstudie te voltooien:

- Azure-abonnement. Als u nog geen abonnement hebt, [maakt u een gratis account][] voordat u begint.
- [Visual Studio 2017 update 3 (versie 15.3, 26730.01)](https://www.visualstudio.com/vs) of hoger.
- [.NET Standard SDK](https://www.microsoft.com/net/download/windows), versie 2.0 of hoger.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u PowerShell lokaal gebruikt, moet u de nieuwste versie van PowerShell uitvoeren om deze snelstart te kunnen voltooien. Als u Azure PowerShell wilt installeren of upgraden, raadpleegt u [Azure PowerShell installeren en configureren](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-5.7.0).

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Een resourcegroep is een logische verzameling van Azure-resources. U hebt een resourcegroep nodig om een Event Hub te maken. 

In het volgende voorbeeld wordt een resourcegroep gemaakt in de regio US - oost. Vervang `myResourceGroup` door de naam van de resourcegroep die u wilt gebruiken:

```azurepowershell-interactive
New-AzureRmResourceGroup –Name myResourceGroup –Location eastus
```

## <a name="create-an-event-hubs-namespace"></a>Een Event Hubs-naamruimte maken

Zodra uw resourcegroep is gemaakt, maakt u een Event Hubs-naamruimte in die resourcegroep. Een Event Hubs-naamruimte biedt een unieke Fully Qualified Domain Name waarin uw Event Hub kunt maken. Vervang `namespace_name` door een unieke naam voor de naamruimte:

```azurepowershell-interactive
New-AzureRmEventHubNamespace -ResourceGroupName myResourceGroup -NamespaceName namespace_name -Location eastus
```

## <a name="create-an-event-hub"></a>Een Event Hub maken

Nu u een ​​Event Hubs-naamruimte hebt, maakt u een Event Hub binnen die naamruimte:

```azurepowershell-interactive
New-AzureRmEventHub -ResourceGroupName myResourceGroup -NamespaceName namespace_name -EventHubName eventhub_name
```

Gefeliciteerd! U hebt Azure PowerShell gebruikt om een ​​Event Hubs-naamruimte en een Event Hub binnen die naamruimte te maken. 

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u de Event Hubs-naamruimte gemaakt en voorbeeldtoepassingen gebruikt om gebeurtenissen te verzenden en ontvangen vanuit uw Event Hub. Zie de volgende zelfstudies voor stapsgewijze instructies voor het verzenden van gebeurtenissen naar of ontvangen van gebeurtenissen vanuit een Event Hub: 

- **Gebeurtenissen verzenden naar een Event Hub**: [.NET Standard](event-hubs-dotnet-standard-getstarted-send.md), [.NET Framework](event-hubs-dotnet-framework-getstarted-send.md), [Java](event-hubs-java-get-started-send.md), [Python](event-hubs-python-get-started-send.md), [Node.js](event-hubs-node-get-started-send.md), [Go](event-hubs-go-get-started-send.md), [C](event-hubs-c-getstarted-send.md)
- **Gebeurtenissen ontvangen vanuit een Event Hub**: [.NET Standard](event-hubs-dotnet-standard-getstarted-receive-eph.md), [.NET Framework](event-hubs-dotnet-framework-getstarted-receive-eph.md), [Java](event-hubs-java-get-started-receive-eph.md), [Python](event-hubs-python-get-started-receive.md), [Node.js](event-hubs-node-get-started-receive.md), [Go](event-hubs-go-get-started-receive-eph.md), [Apache Storm](event-hubs-storm-getstarted-receive.md)

[maakt u een gratis account]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio
[Install and Configure Azure PowerShell]: https://docs.microsoft.com/powershell/azure/install-azurerm-ps
[New-AzureRmResourceGroup]: https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup
[fully qualified domain name]: https://wikipedia.org/wiki/Fully_qualified_domain_name
[3]: ./media/event-hubs-quickstart-powershell/sender1.png
[4]: ./media/event-hubs-quickstart-powershell/receiver1.png
[5]: ./media/event-hubs-quickstart-powershell/metrics.png
