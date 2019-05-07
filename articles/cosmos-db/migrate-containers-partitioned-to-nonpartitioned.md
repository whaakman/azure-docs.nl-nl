---
title: Azure Cosmos DB-containers niet-gepartitioneerde migreren naar gepartitioneerde containers
description: Informatie over het migreren van alle bestaande niet-gepartitioneerde containers in gepartitioneerde containers.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: mjbrown
ms.openlocfilehash: e8aaee8cb7df794652b2c560c5250921c1a4e060
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65160633"
---
# <a name="migrate-non-partitioned-containers-to-partitioned-containers"></a>Migreren van niet-gepartitioneerde containers naar gepartitioneerde containers

Azure Cosmos DB ondersteunt het maken van containers zonder partitiesleutel. U kunt op dit moment niet-gepartitioneerde containers maken met behulp van Azure CLI en Azure Cosmos DB SDK's (.Net, Java, NodeJs) waarvoor een versie lager dan of gelijk zijn aan 2.x. U kunt geen niet-gepartitioneerde containers met behulp van de Azure portal maken. Echter, niet-gepartitioneerde containers zijn niet elastische en opslagcapaciteit van 10 GB en doorvoer limiet van 10 K RU/s is opgelost.

De containers niet-gepartitioneerde zijn verouderd en moet u uw bestaande niet-gepartitioneerde containers migreren naar gepartitioneerde containers schaal van de opslag en doorvoer. Azure Cosmos DB biedt een mechanisme voor het systeem gedefinieerd voor het migreren van uw niet-gepartitioneerde containers naar gepartitioneerde containers. Dit document wordt uitgelegd hoe alle bestaande niet-gepartitioneerde containers automatisch gemigreerd naar gepartitioneerde containers worden. U kunt profiteren van de functie voor automatische migratie alleen als u de V3-versie van de SDK's in alle talen.

> [!NOTE] 
> U kunt Azure Cosmos DB MongoDB- en Gremlin-API-accounts op dit moment niet migreren met behulp van de stappen in dit document beschreven. 

## <a name="migrate-container-using-the-system-defined-partition-key"></a>Migreren van de container met behulp van het systeem gedefinieerde partitiesleutel

Ter ondersteuning van de migratie, Azure Cosmos DB wordt gedefinieerd door het systeem gedefinieerde partitiesleutel naam `/_partitionkey` voor de containers die geen een partitiesleutel hebben. U kunt de definitie van de partitie niet wijzigen nadat de containers worden gemigreerd. Bijvoorbeeld, wordt de definitie van een container die wordt gemigreerd naar een gepartitioneerde container als volgt zijn: 

```json
{
    "Id": "CollId" 
  "partitionKey": {
    "paths": [
      "/_partitionKey"
    ],
    "kind": "Hash"
  },
}
```
 
Nadat de container wordt gemigreerd, kunt u documenten maken door het invullen van de `_partitionKey` eigenschap samen met de andere eigenschappen van het document. De `_partitionKey` eigenschap vertegenwoordigt de partitiesleutel van uw documenten. 

Het kiezen van de juiste partitiesleutel is belangrijk dat u optimaal gebruikmaken van de ingerichte doorvoer. Zie voor meer informatie, [u bij het kiezen van een partitiesleutel](partitioning-overview.md) artikel. 

> [!NOTE]
> U kunt profiteren van het systeem gedefinieerde partitiesleutel alleen als u de meest recente/V3-versie van de SDK's in alle talen.

Het volgende voorbeeld ziet u een voorbeeld van code te maken van een document met het systeem gedefinieerde partitiesleutel dat document lezen:

**JSON-weergave van het document**

```csharp
DeviceInformationItem = new DeviceInformationItem
{
   "id": "elevator/PugetSound/Building44/Floor1/1",
   "deviceId": "3cf4c52d-cc67-4bb8-b02f-f6185007a808",
   "_partitionKey": "3cf4c52d-cc67-4bb8-b02f-f6185007a808"
} 

public class DeviceInformationItem
{
    [JsonProperty(PropertyName = "id")]
    public string Id { get; set; }

    [JsonProperty(PropertyName = "deviceId")]
    public string DeviceId { get; set; }

    [JsonProperty(PropertyName = "_partitionKey")]
    public string PartitionKey {get {return this.DeviceId; set; }
}

CosmosContainer migratedContainer = database.Containers["testContainer"];

DeviceInformationItem deviceItem = new DeviceInformationItem() {
  Id = "1234", 
  DeviceId = "3cf4c52d-cc67-4bb8-b02f-f6185007a808"
} 

CosmosItemResponse<DeviceInformationItem > response = 
  await migratedContainer.Items.CreateItemAsync(
    deviceItem.PartitionKey, 
    deviceItem
  );

// Read back the document providing the same partition key
CosmosItemResponse<DeviceInformationItem> readResponse = 
  await migratedContainer.Items.ReadItemAsync<DeviceInformationItem>( 
    partitionKey:deviceItem.PartitionKey, 
    id: device.Id
  ); 

```

Zie voor het complete voorbeeld de [.Net-voorbeelden](https://github.com/Azure/azure-cosmos-dotnet-v3/tree/master/Microsoft.Azure.Cosmos.Samples/CodeSamples) GitHub-opslagplaats. 
                      
## <a name="migrate-the-documents"></a>Migreren van de documenten

Terwijl de containerdefinitie van de is uitgebreid met een partitie-eigenschap, de documenten in de container worden niet automatisch gemigreerd. Dit betekent dat de systeem-partitie-sleuteleigenschap `/_partitionKey` pad wordt niet automatisch toegevoegd aan de bestaande documenten. Moet u de bestaande documenten door te lezen van de documenten die zijn gemaakt zonder een partitiesleutel te partitioneren en ze terug met herschrijven `_partitionKey` eigenschap in de documenten. 

## <a name="access-documents-that-dont-have-a-partition-key"></a>Toegang tot documenten waarvoor geen een partitiesleutel

Toepassingen hebben toegang tot de bestaande documenten waarvoor een partitiesleutel met behulp van de specifieke eigenschap, genaamd 'CosmosContainerSettings.NonePartitionKeyValue' niet, dit is de waarde van de documenten niet gemigreerd. U kunt deze eigenschap gebruiken in alle CRUD en query bewerkingen. Het volgende voorbeeld wordt een voorbeeld één Document lezen uit de NonePartitionKey. 

```csharp
CosmosItemResponse<DeviceInformationItem> readResponse = 
await migratedContainer.Items.ReadItemAsync<DeviceInformationItem>( 
  partitionKey: CosmosContainerSettings.NonePartitionKeyValue, 
  id: device.Id
); 

```

Zie voor het complete voorbeeld over het partitioneren van de documenten, het [.Net-voorbeelden](https://github.com/Azure/azure-cosmos-dotnet-v3/tree/master/Microsoft.Azure.Cosmos.Samples/CodeSamples) GitHub-opslagplaats. 

## <a name="compatibility-with-sdks"></a>Compatibiliteit met SDK 's

Oudere versie van Azure Cosmos DB SDK's, zoals V2.x.x en V1.x.x geen ondersteuning voor de sleuteleigenschap van het systeem gedefinieerde partitie. Dus als u de containerdefinitie van de van een oudere SDK hebt gelezen, bevat de definitie van een partitie en deze containers gedragen exact hetzelfde als voorheen. Toepassingen die zijn gebouwd met de oudere versie van de SDK's blijven werken met niet-gepartitioneerde is zonder deze te wijzigen. 

Als een gemigreerde container wordt gebruikt door de meest recente/V3-versie van de SDK en u bent begonnen met het invullen van het systeem gedefinieerde partitiesleutel binnen de nieuwe documenten, u geen toegang tot (lezen, bijwerken, verwijderen, query's) documenten die van de oudere SDK's meer.

## <a name="next-steps"></a>Volgende stappen

* [Partitionering in Azure Cosmos DB](partitioning-overview.md)
* [Aanvraageenheden in Azure Cosmos DB](request-units.md)
* [Doorvoer voor containers en databases inrichten](set-throughput.md)
* [Werken met Azure Cosmos-account](account-overview.md)