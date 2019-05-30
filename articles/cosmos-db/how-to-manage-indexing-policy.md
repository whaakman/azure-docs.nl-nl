---
title: Indexing policies in Azure Cosmos DB beheren
description: Informatie over het beheren van de indexing-beleid in Azure Cosmos DB
author: ThomasWeiss
ms.service: cosmos-db
ms.topic: sample
ms.date: 05/23/2019
ms.author: thweiss
ms.openlocfilehash: 05fd369cfebba03c814507f82755fa6cb6a89400
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/30/2019
ms.locfileid: "66386798"
---
# <a name="manage-indexing-policies-in-azure-cosmos-db"></a>Indexing policies in Azure Cosmos DB beheren

In Azure Cosmos DB worden gegevens zijn geïndexeerd met het volgende [indexeringsbeleid](index-policy.md) die zijn gedefinieerd voor elke container. De standaardbeleidsregels voor indexering van beleid voor nieuwe containers wordt afgedwongen bereik indexen voor een tekenreeks of getal en ruimtelijke indexen voor een GeoJSON-object van het type punt. Dit beleid kan worden genegeerd:

- Vanuit Azure portal
- Met behulp van de Azure CLI
- met behulp van een van de SDK 's

Een [indexeren beleidsupdates](index-policy.md#modifying-the-indexing-policy) activeert een transformatie index. De voortgang van deze transformatie kan ook worden bijgehouden in de SDK's.

> [!NOTE]
> Als onderdeel van de SDK en Portal-upgrade, zijn we in het beleid van de index om uit te lijnen met een nieuwe indexindeling dat we hebben geïmplementeerd voor nieuwe containers ontwikkeling. Met deze nieuwe indeling, worden alle primitieve gegevenstypen geïndexeerd als bereik met de volledige precisie (-1). Daarom zijn de typen van de index en de precisie niet beschikbaar in de gebruiker niet meer. Gebruikers in de toekomst moet gewoon paden naar de sectie includedPaths toevoegen en indexKinds en precisie genegeerd. Deze wijziging heeft geen invloed op prestaties en u kunt doorgaan met het indexeringsbeleid met dezelfde syntaxis bijwerken. U kunt echter ook doorgaan met alle voorbeelden in de documentatie van onze bestaande index Groepsbeleid bij te werken.

## <a name="use-the-azure-portal"></a>Azure Portal gebruiken

Azure Cosmos-containers opslaan hun indexeringsbeleid als een JSON-document dat de Azure portal kunt u rechtstreeks bewerken.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).

1. Maak een nieuw Azure Cosmos DB-account of selecteer een bestaand account.

1. Open de **Data Explorer** deelvenster en selecteer de container die u wilt werken.

1. Klik op **schaal en instellingen**.

1. Wijzigen van de indexering beleid JSON-document (Zie de voorbeelden [hieronder](#indexing-policy-examples))

1. Klik op **opslaan** wanneer u klaar bent.

![Indexeren beheren met behulp van de Azure-portal](./media/how-to-manage-indexing-policy/indexing-policy-portal.png)

## <a name="use-the-azure-cli"></a>Azure CLI gebruiken

De [az cosmosdb collection update](/cli/azure/cosmosdb/collection#az-cosmosdb-collection-update) opdracht vanuit de Azure CLI kunt u de JSON-definitie van een container indexeringsbeleid vervangen:

```azurecli-interactive
az cosmosdb collection update \
    --resource-group-name $resourceGroupName \
    --name $accountName \
    --db-name $databaseName \
    --collection-name $containerName \
    --indexing-policy "{\"indexingMode\": \"consistent\", \"includedPaths\": [{ \"path\": \"/*\", \"indexes\": [{ \"dataType\": \"String\", \"kind\": \"Range\" }] }], \"excludedPaths\": [{ \"path\": \"/headquarters/employees/?\" } ]}"
```

## <a name="use-the-net-sdk-v2"></a>Gebruik de .NET SDK-V2

De `DocumentCollection` object uit de [.NET SDK v2](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/) (Zie [in deze snelstartgids](create-sql-api-dotnet.md) met betrekking tot het gebruik ervan) wordt aangegeven dat een `IndexingPolicy` eigenschap waarmee u wijzigen de `IndexingMode` en toevoegen of verwijderen van `IncludedPaths` en `ExcludedPaths`.

```csharp
// retrieve the container's details
ResourceResponse<DocumentCollection> containerResponse = await client.ReadDocumentCollectionAsync(UriFactory.CreateDocumentCollectionUri("database", "container"));
// set the indexing mode to Consistent
containerResponse.Resource.IndexingPolicy.IndexingMode = IndexingMode.Consistent;
// add an excluded path
containerResponse.Resource.IndexingPolicy.ExcludedPaths.Add(new ExcludedPath { Path = "/headquarters/employees/?" });
// update the container with our changes
await client.ReplaceDocumentCollectionAsync(containerResponse.Resource);
```

Voor het volgen van de voortgang van de transformatie index doorgeven een `RequestOptions` -object dat Hiermee stelt u de `PopulateQuotaInfo` eigenschap `true`.

```csharp
// retrieve the container's details
ResourceResponse<DocumentCollection> container = await client.ReadDocumentCollectionAsync(UriFactory.CreateDocumentCollectionUri("database", "container"), new RequestOptions { PopulateQuotaInfo = true });
// retrieve the index transformation progress from the result
long indexTransformationProgress = container.IndexTransformationProgress;
```

## <a name="use-the-java-sdk"></a>Gebruik de Java SDK

De `DocumentCollection` object uit de [Java SDK](https://mvnrepository.com/artifact/com.microsoft.azure/azure-cosmosdb) (Zie [in deze snelstartgids](create-sql-api-java.md) met betrekking tot het gebruik ervan) beschrijft `getIndexingPolicy()` en `setIndexingPolicy()` methoden. De `IndexingPolicy` object ze bewerken kunt u de indexering modus wijzigen en toevoegen of verwijderen opgenomen en uitgesloten paden.

```java
// retrieve the container's details
Observable<ResourceResponse<DocumentCollection>> containerResponse = client.readCollection(String.format("/dbs/%s/colls/%s", "database", "container"), null);
containerResponse.subscribe(result -> {
    DocumentCollection container = result.getResource();
    IndexingPolicy indexingPolicy = container.getIndexingPolicy();
    // set the indexing mode to Consistent
    indexingPolicy.setIndexingMode(IndexingMode.Consistent);
    Collection<ExcludedPath> excludedPaths = new ArrayList<>();
    ExcludedPath excludedPath = new ExcludedPath();
    excludedPath.setPath("/*");
    // add an excluded path
    excludedPaths.add(excludedPath);
    indexingPolicy.setExcludedPaths(excludedPaths);
    // update the container with our changes
    client.replaceCollection(container, null);
});
```

Voor het volgen van de voortgang van de transformatie van de index op een container, geven een `RequestOptions` -object dat aanvragen van de Quotuminformatie om te worden ingevuld, klikt u vervolgens ophalen van de waarde van de `x-ms-documentdb-collection-index-transformation-progress` response-header.

```java
// set the RequestOptions object
RequestOptions requestOptions = new RequestOptions();
requestOptions.setPopulateQuotaInfo(true);
// retrieve the container's details
Observable<ResourceResponse<DocumentCollection>> containerResponse = client.readCollection(String.format("/dbs/%s/colls/%s", "database", "container"), requestOptions);
containerResponse.subscribe(result -> {
    // retrieve the index transformation progress from the response headers
    String indexTransformationProgress = result.getResponseHeaders().get("x-ms-documentdb-collection-index-transformation-progress");
});
```

## <a name="use-the-nodejs-sdk"></a>De Node.js-SDK gebruiken

De `ContainerDefinition` interface van [Node.js SDK](https://www.npmjs.com/package/@azure/cosmos) (Zie [in deze snelstartgids](create-sql-api-nodejs.md) met betrekking tot het gebruik ervan) wordt aangegeven dat een `indexingPolicy` eigenschap waarmee u wijzigen de `indexingMode` en toevoegen of verwijderen van `includedPaths` en `excludedPaths`.

```javascript
// retrieve the container's details
const containerResponse = await client.database('database').container('container').read();
// set the indexing mode to Consistent
containerResponse.body.indexingPolicy.indexingMode = "consistent";
// add an excluded path
containerResponse.body.indexingPolicy.excludedPaths.push({ path: '/headquarters/employees/?' });
// update the container with our changes
const replaceResponse = await client.database('database').container('container').replace(containerResponse.body);
```

Doorgeven om bij te houden de voortgang van de transformatie van de index op een container, een `RequestOptions` -object dat Hiermee stelt u de `populateQuotaInfo` eigenschap `true`, haal vervolgens de waarde van de `x-ms-documentdb-collection-index-transformation-progress` response-header.

```javascript
// retrieve the container's details
const containerResponse = await client.database('database').container('container').read({
    populateQuotaInfo: true
});
// retrieve the index transformation progress from the response headers
const indexTransformationProgress = replaceResponse.headers['x-ms-documentdb-collection-index-transformation-progress'];
```

## <a name="use-the-python-sdk"></a>Gebruik de Python-SDK

Wanneer u de [Python SDK](https://pypi.org/project/azure-cosmos/) (Zie [in deze snelstartgids](create-sql-api-python.md) met betrekking tot het gebruik ervan), de containerconfiguratie van de wordt beheerd als een woordenlijst. Uit deze woordenlijst is het mogelijk voor toegang tot het indexeringsbeleid en de kenmerken ervan.

```python
containerPath = 'dbs/database/colls/collection'
# retrieve the container's details
container = client.ReadContainer(containerPath)
# set the indexing mode to Consistent
container['indexingPolicy']['indexingMode'] = 'consistent'
# add an excluded path
container['indexingPolicy']['excludedPaths'] = [{"path" : "/headquarters/employees/?"}]
# update the container with our changes
response = client.ReplaceContainer(containerPath, container)
```

## <a name="indexing-policy-examples"></a>Indexeren van beleid voor voorbeelden

Hier volgen enkele voorbeelden van de indexing-beleid wordt weergegeven in de JSON-indeling, dit is hoe ze worden weergegeven op de Azure-portal. De parameters die kunnen worden ingesteld via de Azure CLI of een SDK.

### <a name="opt-out-policy-to-selectively-exclude-some-property-paths"></a>Opt-out beleid selectief enkele eigenschappaden uitsluiten
```
    {
        "indexingMode": "consistent",
        "includedPaths": [
            {
                "path": "/*",
                "indexes": [
                    {
                        "kind": "Range",
                        "dataType": "Number"
                    },
                    {
                        "kind": "Range",
                        "dataType": "String"
                    },
                    {
                        "kind": "Spatial",
                        "dataType": "Point"
                    }
                ]
            }
        ],
        "excludedPaths": [
            {
                "path": "/path/to/single/excluded/property/?"
            },
            {
                "path": "/path/to/root/of/multiple/excluded/properties/*"
            }
        ]
    }
```

### <a name="opt-in-policy-to-selectively-include-some-property-paths"></a>Beleid voor aanmelden enkele eigenschappaden selectief wilt opnemen
```
    {
        "indexingMode": "consistent",
        "includedPaths": [
            {
                "path": "/path/to/included/property/?",
                "indexes": [
                    {
                        "kind": "Range",
                        "dataType": "Number"
                    }
                ]
            },
            {
                "path": "/path/to/root/of/multiple/included/properties/*",
                "indexes": [
                    {
                        "kind": "Range",
                        "dataType": "Number"
                    }
                ]
            }
        ],
        "excludedPaths": [
            {
                "path": "/*"
            }
        ]
    }
```

Opmerking: Het wordt afgeraden om te gebruiken een **opt-out** indexeringsbeleid proactief zodat Azure Cosmos DB indexeren van een nieuwe eigenschap die kan worden toegevoegd aan uw model.

### <a name="using-a-spatial-index-on-a-specific-property-path-only"></a>Met behulp van een ruimtelijke index op een bepaalde eigenschap alleen-pad
```
    {
        "indexingMode": "consistent",
        "includedPaths": [
            {
                "path": "/*",
                "indexes": [
                    {
                        "kind": "Range",
                        "dataType": "Number"
                    },
                    {
                        "kind": "Range",
                        "dataType": "String"
                    }
                ]
            },
            {
                "path": "/path/to/geojson/property/?",
                "indexes": [
                    {
                        "kind": "Spatial",
                        "dataType": "Point"
                    }
                ]
            }
        ],
        "excludedPaths": []
    }
```

### <a name="excluding-all-property-paths-but-keeping-indexing-active"></a>Met uitzondering van alle eigenschappaden maar actieve externe worden indexeren

Dit beleid kan worden gebruikt in situaties waar de [Time-to-Live (TTL) functie](time-to-live.md) is actief, maar geen secundaire index is vereist (voor Azure Cosmos DB gebruiken als een zuivere key-value store).
```
    {
        "indexingMode": "consistent",
        "includedPaths": [],
        "excludedPaths": [{
            "path": "/*"
        }]
    }
```

### <a name="no-indexing"></a>Er is geen indexeren
```
    {
        "indexingMode": "none"
    }
```

## <a name="composite-indexing-policy-examples"></a>Samengestelde indexering beleid-voorbeelden

Naast opnemen of uitsluiten van paden voor afzonderlijke eigenschappen, kunt u ook een samengestelde index opgeven. Als u wilt uitvoeren van een query met een `ORDER BY` component voor meerdere eigenschappen, een [samengestelde index](index-policy.md#composite-indexes) deze eigenschappen is vereist.

### <a name="composite-index-defined-for-name-asc-age-desc"></a>Samengestelde index gedefinieerd voor (asc naam, leeftijd desc):
```
    {  
        "automatic":true,
        "indexingMode":"Consistent",
        "includedPaths":[  
            {  
                "path":"/*"
            }
        ],
        "excludedPaths":[  

        ],
        "compositeIndexes":[  
            [  
                {  
                    "path":"/name",
                    "order":"ascending"
                },
                {  
                    "path":"/age",
                    "order":"descending"
                }
            ]
        ]
    }
```

Deze samengestelde index zou kunnen ondersteunen de volgende twee query's:

Query #1:
```sql
    SELECT *
    FROM c
    ORDER BY name asc, age desc    
```

Query #2:
```sql
    SELECT *
    FROM c
    ORDER BY name desc, age asc
```

### <a name="composite-index-defined-for-name-asc-age-asc-and-name-asc-age-desc"></a>Samengestelde index gedefinieerd voor (asc naam, leeftijd asc) en (asc naam, leeftijd desc):

U kunt meerdere verschillende samengestelde indexen binnen de dezelfde indexeringsbeleid definiëren. 
```
    {  
        "automatic":true,
        "indexingMode":"Consistent",
        "includedPaths":[  
            {  
                "path":"/*"
            }
        ],
        "excludedPaths":[  

        ],
        "compositeIndexes":[  
            [  
                {  
                    "path":"/name",
                    "order":"ascending"
                },
                {  
                    "path":"/age",
                    "order":"ascending"
                }
            ]
            [  
                {  
                    "path":"/name",
                    "order":"ascending"
                },
                {  
                    "path":"/age",
                    "order":"descending"
                }
            ]
        ]
    }
```

### <a name="composite-index-defined-for-name-asc-age-asc"></a>Samengestelde index gedefinieerd voor (asc naam, leeftijd asc):

Dit is optioneel om op te geven van de volgorde. Indien niet opgegeven, wordt de volgorde van oplopende.
```
{  
        "automatic":true,
        "indexingMode":"Consistent",
        "includedPaths":[  
            {  
                "path":"/*"
            }
        ],
        "excludedPaths":[  

        ],
        "compositeIndexes":[  
            [  
                {  
                    "path":"/name",
                },
                {  
                    "path":"/age",
                }
            ]
        ]
}
```

## <a name="next-steps"></a>Volgende stappen

Lees meer over indexeren in de volgende artikelen:

- [Indexering-overzicht](index-overview.md)
- [Indexeringsbeleid](index-policy.md)
