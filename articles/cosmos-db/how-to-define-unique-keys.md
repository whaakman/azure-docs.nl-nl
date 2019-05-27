---
title: Unieke sleutels voor een Azure Cosmos-container definiëren
description: Meer informatie over het uniek sleutels voor een Azure Cosmos-container definiëren
author: ThomasWeiss
ms.service: cosmos-db
ms.topic: sample
ms.date: 05/23/2019
ms.author: thweiss
ms.openlocfilehash: fb9872d2fd41066899ff9198915d573bfb4a0b84
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2019
ms.locfileid: "66240977"
---
# <a name="define-unique-keys-for-an-azure-cosmos-container"></a>Unieke sleutels voor een Azure Cosmos-container definiëren

In dit artikel geeft de verschillende manieren voor het definiëren van [unieke sleutels](unique-keys.md) bij het maken van een Azure Cosmos-container. Het is momenteel mogelijk is voor deze bewerking via de Azure-portal of via een van de SDK's.

## <a name="use-the-azure-portal"></a>Azure Portal gebruiken

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).

1. [Maak een nieuw Azure Cosmos-account](create-sql-api-dotnet.md#create-account) of Selecteer een bestaande resourcegroep.

1. Open de **Data Explorer** deelvenster en selecteer de container die u wilt werken.

1. Klik op **nieuwe Container**.

1. In de **Container toevoegen** dialoogvenster, klikt u op **+ unieke sleutel toevoegen** een unieke sleutel vermelding toe te voegen.

1. Voer de paden van de unieke key-beperking

1. Indien nodig, meer vermeldingen voor de unieke sleutel toevoegen door te klikken op **+ unieke sleutel toevoegen**

![Schermafbeelding van de unieke key-beperking vermelding op Azure-portal](./media/how-to-define-unique-keys/unique-keys-portal.png)

## <a name="use-the-net-sdk-v2"></a>Gebruik de .NET SDK-V2

Bij het maken van een nieuwe container met de [.NET SDK v2](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/), een `UniqueKeyPolicy` object kan worden gebruikt voor het definiëren van unique key-beperkingen.

```csharp
client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("database"), new DocumentCollection
{
    Id = "container",
    UniqueKeyPolicy = new UniqueKeyPolicy
    {
        UniqueKeys = new Collection<UniqueKey>(new List<UniqueKey>
        {
            new UniqueKey { Paths = new Collection<string>(new List<string> { "/firstName", "/lastName", "emailAddress" }) },
            new UniqueKey { Paths = new Collection<string>(new List<string> { "/address/zipCode" }) }
        })
    }
});
```

## <a name="use-the-java-sdk"></a>Gebruik de Java SDK

Bij het maken van een nieuwe container met de [Java SDK](https://mvnrepository.com/artifact/com.microsoft.azure/azure-cosmosdb), een `UniqueKeyPolicy` object kan worden gebruikt voor het definiëren van unique key-beperkingen.

```java
// create a new DocumentCollection object
DocumentCollection container = new DocumentCollection();
container.setId("container");
// create array of strings and populate them with the unique key paths
Collection<String> uniqueKey1Paths = new ArrayList<String>();
uniqueKey1Paths.add("/firstName");
uniqueKey1Paths.add("/lastName");
uniqueKey1Paths.add("/emailAddress");
Collection<String> uniqueKey2Paths = new ArrayList<String>();
uniqueKey2Paths.add("/address/zipCode");
// create UniqueKey objects and set their paths
UniqueKey uniqueKey1 = new UniqueKey();
UniqueKey uniqueKey2 = new UniqueKey();
uniqueKey1.setPaths(uniqueKey1Paths);
uniqueKey2.setPaths(uniqueKey2Paths);
// create a new UniqueKeyPolicy object and set its unique keys
UniqueKeyPolicy uniqueKeyPolicy = new UniqueKeyPolicy();
Collection<UniqueKey> uniqueKeys = new ArrayList<UniqueKey>();
uniqueKeys.add(uniqueKey1);
uniqueKeys.add(uniqueKey2);
uniqueKeyPolicy.setUniqueKeys(uniqueKeys);
// set the unique key policy
container.setUniqueKeyPolicy(uniqueKeyPolicy);
// create the container
client.createCollection(String.format("/dbs/%s", "database"), container, null);
```

## <a name="use-the-nodejs-sdk"></a>De Node.js-SDK gebruiken

Bij het maken van een nieuwe container met de [Node.js SDK](https://www.npmjs.com/package/@azure/cosmos), een `UniqueKeyPolicy` object kan worden gebruikt voor het definiëren van unique key-beperkingen.

```javascript
client.database('database').containers.create({
    id: 'container',
    uniqueKeyPolicy: {
        uniqueKeys: [
            { paths: ['/firstName', '/lastName', '/emailAddress'] },
            { paths: ['/address/zipCode'] }
        ]
    }
});
```

## <a name="use-the-python-sdk"></a>Gebruik de Python-SDK

Bij het maken van een nieuwe container met de [Python SDK](https://pypi.org/project/azure-cosmos/), unique key-beperkingen kunnen worden opgegeven als onderdeel van de woordenlijst die wordt doorgegeven als parameter.

```python
client.CreateContainer('dbs/' + config['DATABASE'], {
    'id': 'container',
    'uniqueKeyPolicy': {
        'uniqueKeys': [
            { 'paths': ['/firstName', '/lastName', '/emailAddress'] },
            { 'paths': ['/address/zipCode'] }
        ]
    }
})
```

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [partitioneren](partition-data.md)
- Verken [hoe indexering werkt](index-overview.md)