---
title: Import/export van Azure IoT Hub apparaat-id's | Microsoft Docs
description: Het gebruik van de service Azure IoT SDK Bulksgewijze bewerkingen op basis van het register van de id's importeren en exporteren van apparaat-id's uit te voeren. Importbewerkingen kunnen u maken, bijwerken en verwijderen van apparaat-id's in bulk.
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 2ade1494-45ea-46a7-ade7-cf6e11ce62da
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/03/2017
ms.author: dobett
ms.openlocfilehash: d2a6660b93fee1e1fc24269eb7075e5243ce88ed
ms.sourcegitcommit: c4cc4d76932b059f8c2657081577412e8f405478
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2018
---
# <a name="manage-your-iot-hub-device-identities-in-bulk"></a>Beheren van uw IoT Hub apparaat-id's in bulk

Elke IoT-hub heeft een id-register die kunt u per apparaat om resources te maken in de service. De id-register kunt u ook toegang tot het apparaat gerichte-eindpunten. In dit artikel wordt beschreven hoe importeren en exporteren van apparaat-id's in bulk naar en van een id-register.

Importeren en exporteren bewerkingen plaatsvinden in de context van *taken* waarmee u kunt het uitvoeren van de service bulkbewerkingen tegen een IoT-hub.

De **RegistryManager** klasse bevat de **ExportDevicesAsync** en **ImportDevicesAsync** methoden die gebruikmaken van de **taak** framework. Deze methoden kunnen u exporteren, importeren en synchroniseren van het geheel van een id-register van IoT hub.

In dit onderwerp beschrijft het gebruik van de **RegistryManager** klasse en **taak** system bulkimport en uitvoer van apparaten en naar een IoT-hub identiteitsregister uit te voeren. U kunt ook de Azure IoT Hub apparaat inrichtingsservice gebruiken om in te schakelen zonder tussenkomst, just-in-time inrichten van een of meer IoT hubs zonder menselijke tussenkomst. Zie voor meer informatie, de [documentatie service inrichten][lnk-dps].

## <a name="what-are-jobs"></a>Wat zijn taken?

Bewerkingen in het systeemregister identiteit gebruiken de **taak** systeem wanneer de bewerking:

* Heeft mogelijk veel tijd in uitvoering vergeleken met standaard runtime-bewerkingen.
* Retourneert een grote hoeveelheid gegevens en de gebruiker.

In plaats van één API-aanroep nog of blokkeren van het resultaat van de bewerking, de bewerking maakt u asynchroon een **taak** voor deze iothub. De bewerking vervolgens onmiddellijk retourneert een **JobProperties** object.

De volgende C#-codefragment wordt getoond hoe een exporttaak maken:

```csharp
// Call an export job on the IoT Hub to retrieve all devices
JobProperties exportJob = await registryManager.ExportDevicesAsync(containerSasUri, false);
```

> [!NOTE]
> Gebruik de **RegistryManager** klasse in uw C#-code, het toevoegen van de **Microsoft.Azure.Devices** NuGet-pakket aan uw project. De **RegistryManager** klasse bevindt zich in de **Microsoft.Azure.Devices** naamruimte.

U kunt de **RegistryManager** klasse om de status van de **taak** met behulp van de geretourneerde **JobProperties** metagegevens.

De volgende C#-codefragment wordt getoond hoe elke vijf seconden pollen om te zien als de taak is uitgevoerd:

```csharp
// Wait until job is finished
while(true)
{
  exportJob = await registryManager.GetJobAsync(exportJob.JobId);
  if (exportJob.Status == JobStatus.Completed || 
      exportJob.Status == JobStatus.Failed ||
      exportJob.Status == JobStatus.Cancelled)
  {
    // Job has finished executing
    break;
  }

  await Task.Delay(TimeSpan.FromSeconds(5));
}
```

## <a name="export-devices"></a>Exporteren van apparaten

Gebruik de **ExportDevicesAsync** methode voor het exporteren van het geheel van een IoT hub identiteitenregister in op een [Azure Storage](../storage/index.yml) blob-container gebruiken een [Shared Access Signature](../storage/common/storage-security-guide.md#data-plane-security).

Deze methode kunt u betrouwbare back-ups van uw gegevens van een apparaat in een blob-container die u beheert die te maken.

De **ExportDevicesAsync** methode vereist twee parameters:

* Een *tekenreeks* die een URI van een blob-container bevat. Deze URI moet een SAS-token die schrijftoegang tot de container verleent bevatten. De taak wordt een blok-blob gemaakt in deze container voor het opslaan van de gegevens van het apparaat geserialiseerde exporteren. Deze machtigingen moet worden opgenomen in het SAS-token:

   ```csharp
   SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Delete
   ```

* Een *Booleaanse* die aangeeft dat als u wilt uitsluiten van verificatiesleutels van uw gegevens exporteren. Als **false**, verificatiesleutels zijn opgenomen in de uitvoer van de export. Anders sleutels worden geëxporteerd als **null**.

De volgende C#-codefragment ziet u hoe u een taak voor het exporteren met verificatiesleutels apparaat in de gegevens exporteren te starten en vervolgens wordt gecontroleerd op voltooiing:

```csharp
// Call an export job on the IoT Hub to retrieve all devices
JobProperties exportJob = await registryManager.ExportDevicesAsync(containerSasUri, false);

// Wait until job is finished
while(true)
{
    exportJob = await registryManager.GetJobAsync(exportJob.JobId);
    if (exportJob.Status == JobStatus.Completed || 
        exportJob.Status == JobStatus.Failed ||
        exportJob.Status == JobStatus.Cancelled)
    {
    // Job has finished executing
    break;
    }

    await Task.Delay(TimeSpan.FromSeconds(5));
}
```

De taak wordt de uitvoer ervan weergegeven in de opgegeven blob-container opgeslagen als een blok-blob met de naam **devices.txt**. De uitvoergegevens bestaat uit JSON-geserialiseerd apparaatgegevens, met één apparaat per regel.

Het volgende voorbeeld ziet u de uitvoergegevens:

```json
{"id":"Device1","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device2","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device3","eTag":"MA==","status":"disabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device4","eTag":"MA==","status":"disabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device5","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
```

Als een apparaat dubbele gegevens heeft, zijn de gegevens twin ook geëxporteerd samen met de gegevens van het apparaat. Het volgende voorbeeld ziet deze indeling. Alle gegevens van de regel 'twinETag' tot aan het einde zijn twin gegevens.

```json
{
   "id":"export-6d84f075-0",
   "eTag":"MQ==",
   "status":"enabled",
   "statusReason":"firstUpdate",
   "authentication":null,
   "twinETag":"AAAAAAAAAAI=",
   "tags":{
      "Location":"LivingRoom"
   },
   "properties":{
      "desired":{
         "Thermostat":{
            "Temperature":75.1,
            "Unit":"F"
         },
         "$metadata":{
            "$lastUpdated":"2017-03-09T18:30:52.3167248Z",
            "$lastUpdatedVersion":2,
            "Thermostat":{
               "$lastUpdated":"2017-03-09T18:30:52.3167248Z",
               "$lastUpdatedVersion":2,
               "Temperature":{
                  "$lastUpdated":"2017-03-09T18:30:52.3167248Z",
                  "$lastUpdatedVersion":2
               },
               "Unit":{
                  "$lastUpdated":"2017-03-09T18:30:52.3167248Z",
                  "$lastUpdatedVersion":2
               }
            }
         },
         "$version":2
      },
      "reported":{
         "$metadata":{
            "$lastUpdated":"2017-03-09T18:30:51.1309437Z"
         },
         "$version":1
      }
   }
}
```

Als u toegang tot deze gegevens in de code nodig hebt, kunt u eenvoudig deserialiseren deze gegevens met de **ExportImportDevice** klasse. Het volgende C#-codefragment wordt getoond hoe apparaatgegevens die eerder is geëxporteerd naar een blok-blob lezen:

```csharp
var exportedDevices = new List<ExportImportDevice>();

using (var streamReader = new StreamReader(await blob.OpenReadAsync(AccessCondition.GenerateIfExistsCondition(), null, null), Encoding.UTF8))
{
  while (streamReader.Peek() != -1)
  {
    string line = await streamReader.ReadLineAsync();
    var device = JsonConvert.DeserializeObject<ExportImportDevice>(line);
    exportedDevices.Add(device);
  }
}
```

> [!NOTE]
> U kunt ook de **GetDevicesAsync** methode van de **RegistryManager** klasse voor het ophalen van een lijst van uw apparaten. Deze methode heeft echter een vaste limiet van 1000 van het aantal apparaatobjecten die worden geretourneerd. Het verwachte gebruiksvoorbeeld voor de **GetDevicesAsync** methode is voor ontwikkelscenario's om te helpen bij foutopsporing en wordt niet aanbevolen voor productieworkloads.

## <a name="import-devices"></a>Apparaten importeren

De **ImportDevicesAsync** methode in de **RegistryManager** klasse kunt u bulksgewijs importeren en synchroniseren van bewerkingen uitvoeren in een id-register van IoT hub. Als de **ExportDevicesAsync** methode, de **ImportDevicesAsync** methode gebruikt de **taak** framework.

Wees voorzichtig met het **ImportDevicesAsync** methode omdat naast het inrichten van nieuwe apparaten in het identiteitenregister, kan het ook bijwerken en verwijderen van bestaande apparaten.

> [!WARNING]
> Een importbewerking kan niet ongedaan worden gemaakt. Altijd back-up van uw bestaande gegevens met behulp van de **ExportDevicesAsync** methode naar een andere blob-container voordat u bulksgewijs wijzigt in het identiteitsregister.

De **ImportDevicesAsync** methode heeft twee parameters:

* Een *tekenreeks* die een URI van bevat een [Azure Storage](../storage/index.yml) blob-container om te gebruiken als *invoer* toe aan de job. Deze URI moet een SAS-token die leestoegang tot de container verleent bevatten. Deze container moet een blob met de naam bevatten **devices.txt** die de geserialiseerde apparaatgegevens importeren in de register-id's bevat. De importgegevens moet bevatten apparaatgegevens in dezelfde JSON-indeling die de **ExportImportDevice** taak wordt gebruikt bij het maken van een **devices.txt** blob. Deze machtigingen moet worden opgenomen in het SAS-token:

   ```csharp
   SharedAccessBlobPermissions.Read
   ```
* Een *tekenreeks* die een URI van bevat een [Azure Storage](https://azure.microsoft.com/documentation/services/storage/) blob-container om te gebruiken als *uitvoer* van de taak. De taak wordt een blok-blob gemaakt in deze container voor het opslaan van alle informatie over de fout van de voltooide import **taak**. Deze machtigingen moet worden opgenomen in het SAS-token:

   ```csharp
   SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Delete
   ```

> [!NOTE]
> De twee parameters kunnen verwijzen naar dezelfde blobcontainer. De afzonderlijke parameters inschakelen gewoon meer controle over uw gegevens als de uitvoer-container, aanvullende machtigingen vereist.

De volgende C#-codefragment wordt getoond hoe een import-taak starten:

```csharp
JobProperties importJob = await registryManager.ImportDevicesAsync(containerSasUri, containerSasUri);
```

Deze methode kan ook worden gebruikt voor het importeren van de gegevens voor het apparaat. De indeling voor de gegevensinvoer is hetzelfde als de notatie die in de **ExportDevicesAsync** sectie. Op deze manier kunt u de geëxporteerde gegevens opnieuw importeren. De **$metadata** is optioneel.

## <a name="import-behavior"></a>Gedrag importeren

U kunt de **ImportDevicesAsync** methode moet worden uitgevoerd de volgende bewerkingen voor het bulksgewijs in de register-id's:

* Registratie van nieuwe apparaten bulksgewijs
* Bulksgewijs verwijderingen van bestaande apparaten
* Bulksgewijs statuswijzigingen (in- of uitschakelen van apparaten)
* Toewijzing van nieuwe apparaten verificatiesleutels bulksgewijs
* Bulksgewijs automatisch opnieuw genereren van sleutels voor apparaat-verificatie
* Bulk-update van dubbele gegevens

U kunt een combinatie van de voorgaande bewerkingen binnen één uitvoeren **ImportDevicesAsync** aanroepen. Bijvoorbeeld, kunt u nieuwe apparaten registreren en verwijderen of bijwerken van bestaande apparaten op hetzelfde moment. Wanneer gebruikt in combinatie met de **ExportDevicesAsync** methode maakt u al uw apparaten volledig uit een IoT-hub kunt migreren naar een andere.

Als het importbestand twin metagegevens bevat, overschrijft deze metagegevens van de bestaande twin-metagegevens. Als het importbestand geen twin metagegevens, klikt u vervolgens alleen bevat de `lastUpdateTime` metagegevens worden bijgewerkt met de huidige tijd.

Gebruik het optionele **ImportMode %** eigenschap in de serialisatiegegevens importeren voor elk apparaat om te bepalen van de import proces per apparaat. De **ImportMode %** eigenschap heeft de volgende opties:

| ImportMode % | Beschrijving |
| --- | --- |
| **createOrUpdate** |Als een apparaat niet met de opgegeven bestaat **id**, het zojuist is ingeschreven. <br/>Als het apparaat al bestaat, bestaande gegevens wordt overschreven met de opgegeven invoergegevens zonder betrekking tot de **ETag** waarde. <br> De gebruiker kan eventueel twin gegevens samen met de apparaatgegevens opgeven. De twin etag, wordt indien opgegeven, verwerkt onafhankelijk van het apparaat etag. Als er niet overeen met de bestaande twin etag komt, wordt een fout naar het logboekbestand geschreven. |
| **maken** |Als een apparaat niet met de opgegeven bestaat **id**, het zojuist is ingeschreven. <br/>Als het apparaat al bestaat, wordt er een fout naar het logboekbestand geschreven. <br> De gebruiker kan eventueel twin gegevens samen met de apparaatgegevens opgeven. De twin etag, wordt indien opgegeven, verwerkt onafhankelijk van het apparaat etag. Als er niet overeen met de bestaande twin etag komt, wordt een fout naar het logboekbestand geschreven. |
| **bijwerken** |Als er al een apparaat met de opgegeven bestaat **id**, bestaande gegevens wordt overschreven met de opgegeven invoergegevens zonder betrekking tot de **ETag** waarde. <br/>Als het apparaat niet bestaat nog, wordt er een fout naar het logboekbestand geschreven. |
| **updateIfMatchETag** |Als er al een apparaat met de opgegeven bestaat **id**, bestaande gegevens wordt overschreven met de opgegeven invoergegevens alleen als er een **ETag** overeenkomen. <br/>Als het apparaat niet bestaat nog, wordt er een fout naar het logboekbestand geschreven. <br/>Als er een **ETag** niet-overeenkomende, een fout wordt geschreven naar het logboekbestand. |
| **createOrUpdateIfMatchETag** |Als een apparaat niet met de opgegeven bestaat **id**, het zojuist is ingeschreven. <br/>Als het apparaat al bestaat, bestaande gegevens overschreven met de opgegeven invoergegevens alleen als er een **ETag** overeenkomen. <br/>Als er een **ETag** niet-overeenkomende, een fout wordt geschreven naar het logboekbestand. <br> De gebruiker kan eventueel twin gegevens samen met de apparaatgegevens opgeven. De twin etag, wordt indien opgegeven, verwerkt onafhankelijk van het apparaat etag. Als er niet overeen met de bestaande twin etag komt, wordt een fout naar het logboekbestand geschreven. |
| **verwijderen** |Als er al een apparaat met de opgegeven bestaat **id**, wordt deze verwijderd zonder betrekking tot de **ETag** waarde. <br/>Als het apparaat niet bestaat nog, wordt er een fout naar het logboekbestand geschreven. |
| **deleteIfMatchETag** |Als er al een apparaat met de opgegeven bestaat **id**, wordt deze verwijderd alleen als er een **ETag** overeenkomen. Als het apparaat niet bestaat nog, wordt er een fout naar het logboekbestand geschreven. <br/>Als er een ETag komt niet overeen, wordt er een fout naar het logboekbestand geschreven. |

> [!NOTE]
> Als de serialisatiegegevens geen expliciet definieert een **ImportMode %** vlag voor een apparaat, wordt standaard **createOrUpdate** tijdens het importeren.

## <a name="import-devices-example--bulk-device-provisioning"></a>Voorbeeld van de apparaten te importeren: bulksgewijs apparaten inrichten

De volgende C#-codevoorbeeld ziet u hoe u kunt meerdere apparaat-id's genereren die:

* Verificatiesleutels bevatten.
* Deze apparaatgegevens schrijven naar een blok-blob.
* Importeer de apparaten in het identiteitsregister.

```csharp
// Provision 1,000 more devices
var serializedDevices = new List<string>();

for (var i = 0; i < 1000; i++)
{
  // Create a new ExportImportDevice
  // CryptoKeyGenerator is in the Microsoft.Azure.Devices.Common namespace
  var deviceToAdd = new ExportImportDevice()
  {
    Id = Guid.NewGuid().ToString(),
    Status = DeviceStatus.Enabled,
    Authentication = new AuthenticationMechanism()
    {
      SymmetricKey = new SymmetricKey()
      {
        PrimaryKey = CryptoKeyGenerator.GenerateKey(32),
        SecondaryKey = CryptoKeyGenerator.GenerateKey(32)
      }
    },
    ImportMode = ImportMode.Create
  };

  // Add device to the list
  serializedDevices.Add(JsonConvert.SerializeObject(deviceToAdd));
}

// Write the list to the blob
var sb = new StringBuilder();
serializedDevices.ForEach(serializedDevice => sb.AppendLine(serializedDevice));
await blob.DeleteIfExistsAsync();

using (CloudBlobStream stream = await blob.OpenWriteAsync())
{
  byte[] bytes = Encoding.UTF8.GetBytes(sb.ToString());
  for (var i = 0; i < bytes.Length; i += 500)
  {
    int length = Math.Min(bytes.Length - i, 500);
    await stream.WriteAsync(bytes, i, length);
  }
}

// Call import using the blob to add new devices
// Log information related to the job is written to the same container
// This normally takes 1 minute per 100 devices
JobProperties importJob = await registryManager.ImportDevicesAsync(containerSasUri, containerSasUri);

// Wait until job is finished
while(true)
{
  importJob = await registryManager.GetJobAsync(importJob.JobId);
  if (importJob.Status == JobStatus.Completed || 
      importJob.Status == JobStatus.Failed ||
      importJob.Status == JobStatus.Cancelled)
  {
    // Job has finished executing
    break;
  }

  await Task.Delay(TimeSpan.FromSeconds(5));
}
```

## <a name="import-devices-example--bulk-deletion"></a>Importeren van apparaten voorbeeld – bulksgewijs verwijderen

De volgende voorbeeldcode laat zien hoe u de apparaten die u met behulp van het vorige codevoorbeeld toegevoegd verwijderen:

```csharp
// Step 1: Update each device's ImportMode to be Delete
sb = new StringBuilder();
serializedDevices.ForEach(serializedDevice =>
{
  // Deserialize back to an ExportImportDevice
  var device = JsonConvert.DeserializeObject<ExportImportDevice>(serializedDevice);

  // Update property
  device.ImportMode = ImportMode.Delete;

  // Re-serialize
  sb.AppendLine(JsonConvert.SerializeObject(device));
});

// Step 2: Write the new import data back to the block blob
await blob.DeleteIfExistsAsync();
using (CloudBlobStream stream = await blob.OpenWriteAsync())
{
  byte[] bytes = Encoding.UTF8.GetBytes(sb.ToString());
  for (var i = 0; i < bytes.Length; i += 500)
  {
    int length = Math.Min(bytes.Length - i, 500);
    await stream.WriteAsync(bytes, i, length);
  }
}

// Step 3: Call import using the same blob to delete all devices
importJob = await registryManager.ImportDevicesAsync(containerSasUri, containerSasUri);

// Wait until job is finished
while(true)
{
  importJob = await registryManager.GetJobAsync(importJob.JobId);
  if (importJob.Status == JobStatus.Completed || 
      importJob.Status == JobStatus.Failed ||
      importJob.Status == JobStatus.Cancelled)
  {
    // Job has finished executing
    break;
  }

  await Task.Delay(TimeSpan.FromSeconds(5));
}
```

## <a name="get-the-container-sas-uri"></a>De container SAS URI ophalen

Het volgende codevoorbeeld ziet u het genereren van een [SAS-URI](../storage/blobs/storage-dotnet-shared-access-signature-part-2.md) met lezen, schrijven en verwijderen van machtigingen voor een blob-container:

```csharp
static string GetContainerSasUri(CloudBlobContainer container)
{
  // Set the expiry time and permissions for the container.
  // In this case no start time is specified, so the
  // shared access signature becomes valid immediately.
  var sasConstraints = new SharedAccessBlobPolicy();
  sasConstraints.SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24);
  sasConstraints.Permissions = 
    SharedAccessBlobPermissions.Write | 
    SharedAccessBlobPermissions.Read | 
    SharedAccessBlobPermissions.Delete;

  // Generate the shared access signature on the container,
  // setting the constraints directly on the signature.
  string sasContainerToken = container.GetSharedAccessSignature(sasConstraints);

  // Return the URI string for the container,
  // including the SAS token.
  return container.Uri + sasContainerToken;
}
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd hoe u andere bulkbewerkingen tegen de register-id's uitvoeren in een IoT-hub. Volg deze koppelingen voor meer informatie over het beheren van Azure IoT Hub:

* [IoT Hub metrische gegevens][lnk-metrics]
* [Bewerkingen controleren][lnk-monitor]

Als u wilt de mogelijkheden van IoT Hub verder verkennen, Zie:

* [Ontwikkelaarshandleiding voor IoT Hub][lnk-devguide]
* [AI implementeren op Edge-apparaten met Azure IoT Edge][lnk-iotedge]

Als u wilt verkennen met behulp van de IoT Hub apparaat inrichtingsservice om in te schakelen zonder tussenkomst, Zie just-in-time-inrichting: 

* [Azure IoT Hub apparaat-Service inricht][lnk-dps]


[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: ../iot-edge/tutorial-simulate-device-linux.md
[lnk-dps]: https://azure.microsoft.com/documentation/services/iot-dps
