---
title: Azure Cosmos-containers te maken met grote partitiesleutel met behulp van de Azure-portal en de verschillende SDK's.
description: Informatie over het maken van een container in Azure Cosmos DB met grote partitiesleutel met behulp van Azure portal en andere SDK's.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 07/03/2019
ms.author: mjbrown
ms.openlocfilehash: bd1697378e5db0432d181f9f688ccc2468b306e7
ms.sourcegitcommit: d2785f020e134c3680ca1c8500aa2c0211aa1e24
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/04/2019
ms.locfileid: "67566024"
---
# <a name="create-containers-with-large-partition-key"></a>Maken van containers met grote partitiesleutel

Azure Cosmos DB maakt gebruik van partitionering op basis van een hash te bereiken horizontaal schalen van gegevens. Alle Azure Cosmos-containers die zijn gemaakt vóór 3 2019 mei gebruiken een hashfunctie die wordt berekend op basis van de eerste 100 bytes van de partitiesleutel hash. Als er meerdere partitiesleutels met de dezelfde eerste 100 bytes, worden klikt u vervolgens deze logische partities beschouwd als dezelfde logische partitie door de service. Dit kan leiden tot problemen, zoals onjuiste wordt het quotum van partitie en unieke indexen worden toegepast op de partitiesleutels. Grote partitiesleutels zijn geïntroduceerd om op te lossen dit probleem. Azure Cosmos DB nu ondersteunt grote partitiesleutels van waarden tot 2 KB.

Grote partitie sleutels worden ondersteund met behulp van de functionaliteit van een verbeterde versie van de hashfunctie, die een unieke hash uit grote partitie genereren kunt sleutels tot 2 KB. Deze hash-versie wordt ook aanbevolen voor scenario's met hoge partitie de kardinaliteit van sleutel, ongeacht de grootte van de partitiesleutel. De kardinaliteit van de sleutel van een partitie wordt gedefinieerd als het aantal unieke logische partities, bijvoorbeeld in de volgorde van ~ 30000 logische partities in een container. In dit artikel wordt beschreven hoe u een container maken met een grote partitie-sleutel met de Azure portal en de verschillende SDK's. 

## <a name="create-a-large-partition-key-net-sdk-v2"></a>Maken van een grote partitiesleutel (.Net SDK-V2)

Geef voor het maken van een container met een grote partitie-sleutel met de .NET SDK, de `PartitionKeyDefinitionVersion.V2` eigenschap. Het volgende voorbeeld ziet hoe u de eigenschap versie binnen het object PartitionKeyDefinition opgeven en instellen op PartitionKeyDefinitionVersion.V2.

```csharp
DocumentCollection collection = await newClient.CreateDocumentCollectionAsync(
database,
     new DocumentCollection
        {
           Id = Guid.NewGuid().ToString(),
           PartitionKey = new PartitionKeyDefinition
           {
             Paths = new Collection<string> {"/longpartitionkey" },
             Version = PartitionKeyDefinitionVersion.V2
           }
         },
      new RequestOptions { OfferThroughput = 400 });
```

## <a name="create-a-large-partition-key-azure-portal"></a>Maken van een grote partitiesleutel (Azure portal) 

Voor het maken van een grote partitiesleutel, terwijl u een nieuwe container met behulp van de Azure portal maken, Controleer de **mijn partitiesleutel is groter dan 100 bytes** optie. Standaard worden de nieuwe containers deelneemt aan het gebruik van de grote partitiesleutels. Schakel het selectievakje in als u niet nodig voor grote partitiesleutels hebt of als u toepassingen die worden uitgevoerd op de SDK-versie ouder is dan 1.18 hebt.

![Grote partitiesleutels met behulp van Azure portal maken](./media/large-partition-keys/large-partition-key-with-portal.png)

## <a name="create-a-large-partition-key-powershell"></a>Maken van een grote partitiesleutel (PowerShell)

Voor het maken van een container met een grote partitiesleutel met behulp van PowerShell, omvatten `"version" = 2` voor de `partitionKey` object.

```azurepowershell-interactive
# Create a Cosmos SQL API container with large partition key support (version 2)
$resourceGroupName = "myResourceGroup"
$containerName = "mycosmosaccount" + "/sql/" + "myDatabase" + "/" + "myContainer"

# Container with large partition key support (version = 2)
$containerProperties = @{
  "resource"=@{
    "id"=$containerName;
    "partitionKey"=@{
        "paths"=@("/myPartitionKey");
        "kind"="Hash";
        "version" = 2
    };
    "indexingPolicy"=@{
        "indexingMode"="Consistent";
        "includedPaths"= @(@{
            "path"="/*"
        });
        "excludedPaths"= @(@{
            "path"="/myPathToNotIndex/*"
        })
    }
  }
}

New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases/containers" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $containerName -PropertyObject $containerProperties
```

## <a name="supported-sdk-versions"></a>Ondersteunde versies van de SDK

De grote partitiesleutels worden ondersteund door de volgende minimaal vereiste versies van SDK's:

|SDK-type  | Minimale versie   |
|---------|---------|
|.Net     |    1.18     |
|Java-synchronisatie     |   2.4.0      |
|Async Java   |  2.5.0        |
| REST-API | versie die hoger is dan `2017-05-03` met behulp van de `x-ms-version` aanvraagheader.|

U kunt de containers op dit moment niet gebruiken met grote partitiesleutel binnen in Power BI en Azure Logic Apps. U kunt containers zonder een grote partitie-sleutel van deze toepassingen.

## <a name="next-steps"></a>Volgende stappen

* [Partitionering in Azure Cosmos DB](partitioning-overview.md)
* [Aanvraageenheden in Azure Cosmos DB](request-units.md)
* [Doorvoer voor containers en databases inrichten](set-throughput.md)
* [Werken met Azure Cosmos-account](account-overview.md)


