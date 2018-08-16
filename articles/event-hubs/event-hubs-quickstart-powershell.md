---
title: 'Azure-snelstart: Gebeurtenisstromen verwerken met behulp van Powershell | Microsoft Docs'
description: In deze snelstart wordt beschreven hoe u gebeurtenissen van Azure Event Hubs kunt verzenden en ontvangen met behulp van PowerShell en een .NET-voorbeeldtoepassing.
services: event-hubs
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.service: event-hubs
ms.devlang: na
ms.topic: quickstart
ms.custom: mvc
ms.date: 06/26/2018
ms.author: shvija
ms.openlocfilehash: fd2b8b2e8bd075e029a07519ced186424798fd9c
ms.sourcegitcommit: d0ea925701e72755d0b62a903d4334a3980f2149
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/09/2018
ms.locfileid: "40003607"
---
# <a name="quickstart-process-event-streams-using-powershell-and-net-standard"></a>Snelstart: Gebeurtenisstromen verwerken met behulp van PowerShell en .NET Standard | Microsoft Docs

Azure Event Hubs is een uiterst schaalbaar platform voor het streamen van gegevens en een opneemservice die miljoenen gebeurtenissen per seconde kan opnemen en verwerken. Deze snelstart laat zien hoe u een Event Hub maakt met behulp van Azure PowerShell en vervolgens verzendt naar en ontvangt van een Event Hub met behulp van de .NET Standard-SDK.

U hebt een Azure-abonnement nodig om deze snelstart te voltooien. Als u nog geen abonnement hebt, [maakt u een gratis account][] voordat u begint.

## <a name="prerequisites"></a>Vereisten

Het volgende moet zijn geïnstalleerd om deze zelfstudie te voltooien:

- [Visual Studio 2017 update 3 (versie 15.3, 26730.01)](http://www.visualstudio.com/vs) of hoger.
- [.NET Standard SDK](https://www.microsoft.com/net/download/windows), versie 2.0 of hoger.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u PowerShell lokaal gebruikt, moet u de nieuwste versie van PowerShell uitvoeren om deze snelstart te kunnen voltooien. Als u Azure PowerShell wilt installeren of upgraden, raadpleegt u [Azure PowerShell installeren en configureren](https://docs.microsoft.com/en-us/powershell/azure/install-azurerm-ps?view=azurermps-5.7.0).

## <a name="provision-resources"></a>Resources inrichten

### <a name="create-a-resource-group"></a>Een resourcegroep maken

Een resourcegroep is een logische verzameling van Azure-resources. U hebt een resourcegroep nodig om een Event Hub te maken. 

In het volgende voorbeeld wordt een resourcegroep gemaakt in de regio US - oost. Vervang `myResourceGroup` door de naam van de resourcegroep die u wilt gebruiken:

```azurepowershell-interactive
New-AzureRmResourceGroup –Name myResourceGroup –Location eastus
```

### <a name="create-an-event-hubs-namespace"></a>Een Event Hubs-naamruimte maken

Zodra uw resourcegroep is gemaakt, maakt u een Event Hubs-naamruimte in die resourcegroep. Een Event Hubs-naamruimte biedt een unieke Fully Qualified Domain Name waarin uw Event Hub kunt maken. Vervang `namespace_name` door een unieke naam voor de naamruimte:

```azurepowershell-interactive
New-AzureRmEventHubNamespace -ResourceGroupName myResourceGroup -NamespaceName namespace_name -Location eastus
```

### <a name="create-an-event-hub"></a>Een Event Hub maken

Nu u een ​​Event Hubs-naamruimte hebt, maakt u een Event Hub binnen die naamruimte:

```azurepowershell-interactive
New-AzureRmEventHub -ResourceGroupName myResourceGroup -NamespaceName namespace_name -EventHubName eventhub_name
```

### <a name="create-a-storage-account-for-event-processor-host"></a>Een opslagaccount voor Event Processor Host maken

Event Processor Host vereenvoudigt het ontvangen van gebeurtenissen van Event Hubs door permanente controlepunten en parallelle ontvangsten te beheren. Voor controlepunten heeft Event Processor Host een opslagaccount nodig. Voer de volgende opdrachten uit als u een opslagaccount wilt maken en de bijbehorende sleutels wilt ophalen:

```azurepowershell-interactive
# Create a standard general purpose storage account 
New-AzureRmStorageAccount -ResourceGroupName myResourceGroup -Name storage_account_name -Location eastus -SkuName Standard_LRS 
e
# Retrieve the storage account key for accessing it
Get-AzureRmStorageAccountKey -ResourceGroupName myResourceGroup -Name storage_account_name
```

### <a name="get-the-connection-string"></a>De verbindingsreeks ophalen

Een verbindingsreeks is vereist om verbinding te maken met uw Event Hub en gebeurtenissen te verwerken. U kunt een verbindingsreeks ophalen door de volgende opdrachten uit te voeren:

```azurepowershell-interactive
Get-AzureRmEventHubKey -ResourceGroupName myResourceGroup -NamespaceName namespace_name -Name RootManageSharedAccessKey
```

## <a name="stream-into-event-hubs"></a>Streamen in Event Hubs

U kunt nu beginnen met streamen naar uw Event Hubs. De voorbeelden kunnen worden gedownload of uit Git worden gekloond vanuit de [Event Hubs-opslagplaats](https://github.com/Azure/azure-event-hubs)

### <a name="ingest-events"></a>Gebeurtenissen opnemen

Als u wilt gaan beginnen met het streamen van gebeurtenissen, downloadt u [SampleSender](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender) uit GitHub of kloont u de [Event Hubs GitHub-opslagplaats](https://github.com/Azure/azure-event-hubs) door de volgende opdracht te typen:

```bash
git clone https://github.com/Azure/azure-event-hubs.git
```

Navigeer naar de map \azure-event-hubs\samples\DotNet\Microsoft.Azure.EventHubs\SampleSender en laad het bestand SampleSender.sln in Visual Studio.

Voeg vervolgens Nuget-pakket [Microsoft.Azure.EventHubs](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) NuGet-pakket aan het project toe.

Vervang in het bestand Program.cs de volgende tijdelijke aanduidingen door de naam van uw Event Hub en de verbindingsreeks:

```C#
private const string EhConnectionString = "Event Hubs connection string";
private const string EhEntityPath = "Event Hub name";

```

Bouw nu het voorbeeld en voer het uit. Zoals u ziet worden de gebeurtenissen in uw Event Hub nu opgenomen:

![][3]

### <a name="receive-and-process-events"></a>Gebeurtenissen ontvangen en verwerken

Download nu het ontvangstvoorbeeld van Event Processor Host, waar de berichten worden ontvangen die u net hebt verzonden. Download [SampleEphReceiver](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver) uit GitHub of kloon de [Event Hubs GitHub-opslagplaats](https://github.com/Azure/azure-event-hubs) door de volgende opdracht te typen:

```bash
git clone https://github.com/Azure/azure-event-hubs.git
```

Navigeer naar de map \azure-event-hubs\samples\DotNet\Microsoft.Azure.EventHubs\SampleEphReceiver en laad het oplossingsbestand SampleEphReceiver.sln in Visual Studio.

Voeg vervolgens de NuGet-pakketten [Microsoft.Azure.EventHubs](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) en [Microsoft.Azure.EventHubs.Processor](https://www.nuget.org/packages/Microsoft.Azure.EventHubs.Processor/) toe aan het project.

Vervang de volgende constanten door de bijbehorende waarden in het bestand Program.cs:

```C#
private const string EventHubConnectionString = "Event Hubs connection string";
private const string EventHubName = "Event Hub name";
private const string StorageContainerName = "Storage account container name";
private const string StorageAccountName = "Storage account name";
private const string StorageAccountKey = "Storage account key";
```

Bouw nu het voorbeeld en voer het uit. Zoals u ziet worden de gebeurtenissen nu in uw voorbeeldtoepassing ontvangen:

![][4]

In Azure Portal kunt u als volgt de snelheid zien waarmee gebeurtenissen worden verwerkt voor een bepaalde Event Hubs-naamruimte:

![][5]

## <a name="clean-up-resources"></a>Resources opschonen

Nadat u deze snelstart hebt voltooid, kunt u de resourcegroep evenals de naamruimte, het opslagaccount en de Event Hub erin verwijderen. Vervang `myResourceGroup` door de naam van de resourcegroep die u hebt gemaakt. 

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u de Event Hubs-naamruimte en andere resources gemaakt die zijn vereist voor het verzenden en ontvangen van gebeurtenissen van uw Event Hub. Ga verder met de volgende zelfstudie voor meer informatie:

> [!div class="nextstepaction"]
> [Gegevensafwijkingen visualiseren in Event Hubs-gegevensstromen](event-hubs-tutorial-visualize-anomalies.md)

[maakt u een gratis account]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio
[Install and Configure Azure PowerShell]: https://docs.microsoft.com/powershell/azure/install-azurerm-ps
[New-AzureRmResourceGroup]: https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup
[fully qualified domain name]: https://wikipedia.org/wiki/Fully_qualified_domain_name
[3]: ./media/event-hubs-quickstart-powershell/sender1.png
[4]: ./media/event-hubs-quickstart-powershell/receiver1.png
[5]: ./media/event-hubs-quickstart-powershell/metrics.png
