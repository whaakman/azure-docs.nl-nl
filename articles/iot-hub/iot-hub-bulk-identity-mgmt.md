---
title: Import/export van Azure IoT Hub apparaat-id's | Microsoft Docs
description: Het gebruik van de Azure IoT service SDK bulksgewijs bewerkingen op basis van het id-register voor het importeren en exporteren van apparaat-id's uit te voeren. Importbewerkingen kunnen u maken, bijwerken en verwijderen van apparaat-id's in één bulkbewerking.
author: robinsh
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 07/03/2017
ms.author: robin.shahan
ms.openlocfilehash: 5ef34fb039d35ff714e249a6ac107e6ec615093e
ms.sourcegitcommit: 15e9613e9e32288e174241efdb365fa0b12ec2ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/28/2019
ms.locfileid: "57010987"
---
# <a name="manage-your-iot-hub-device-identities-in-bulk"></a>Uw IoT-Hub apparaatidentiteiten bulksgewijs beheren

Elke IoT-hub is een id-register dat u per apparaat om resources te maken in de service kunt gebruiken. Het id-register kunt u toegang tot het apparaat gerichte eindpunten beheren. In dit artikel wordt beschreven hoe u importeren en exporteren van apparaat-id's in bulk naar en van een id-register.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Importeren en exporteren-bewerkingen plaatsvinden in de context van *taken* die het mogelijk om uit te voeren bulksgewijs servicebewerkingen op basis van een IoT-hub.

De **RegistryManager** klasse bevat de **ExportDevicesAsync** en **ImportDevicesAsync** methoden die gebruikmaken van de **taak** framework. Deze methoden kunnen u om te exporteren, importeren en synchroniseren van het geheel van een id-register van IoT hub.

In dit onderwerp beschrijft het gebruik van de **RegistryManager** klasse en **taak** system om uit te voeren bulksgewijs invoer en uitvoer van apparaten en naar een IoT-hub-identiteitsregister. U kunt ook de Azure IoT Hub Device Provisioning Service gebruiken om in te schakelen zero-touch, just-in-time inrichting naar een of meer IoT-hubs zonder menselijke tussenkomst. Zie voor meer informatie, de [documentatie over service inrichten](/azure/iot-dps).


## <a name="what-are-jobs"></a>Wat zijn taken?

Registerbewerkingen voor identiteit gebruiken de **taak** systeem wanneer de bewerking:

* Heeft een potentieel lange uitvoeringstijden vergeleken met standaard runtime-bewerkingen.

* Retourneert een grote hoeveelheid gegevens naar de gebruiker.

In plaats van één API-aanroep wachten of te blokkeren op het resultaat van de bewerking, de bewerking maakt u asynchroon een **taak** voor deze IoT-hub. De bewerking vervolgens onmiddellijk retourneert een **JobProperties** object.

De volgende C#-codefragment laat zien hoe een exporttaak maken:

```csharp
// Call an export job on the IoT Hub to retrieve all devices
JobProperties exportJob = await 
  registryManager.ExportDevicesAsync(containerSasUri, false);
```

> [!NOTE]
> Gebruik de **RegistryManager** klasse in uw C#-code, voegt u de **Microsoft.Azure.Devices** NuGet-pakket aan uw project. De **RegistryManager** klasse bevindt zich in de **Microsoft.Azure.Devices** naamruimte.

U kunt de **RegistryManager** klasse om de status van de **taak** met behulp van de geretourneerde **JobProperties** metagegevens. Het maken van een exemplaar van de **RegistryManager** klasse, gebruikt u de **CreateFromConnectionString** methode.

```csharp
RegistryManager registryManager =
  RegistryManager.CreateFromConnectionString("{your IoT Hub connection string}");
```

Zoek de verbindingsreeks voor uw IoT-hub in Azure portal:

- Ga naar uw IoT-hub.

- Selecteer **beleid voor gedeelde toegang**.

- Selecteer een beleid, rekening houdend met de machtigingen die u nodig hebt.

- Kopieer de connectionstring vanuit het deelvenster aan de rechterkant van het scherm.

De volgende C#-codefragment laat zien hoe gegevens op te vragen om de vijf seconden om te zien als de taak is uitgevoerd:

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

## <a name="export-devices"></a>Apparaten exporteren

Gebruik de **ExportDevicesAsync** methode voor het exporteren van het geheel van een IoT hub-identiteitsregister naar een [Azure Storage](../storage/index.yml) blob-container met een [Shared Access Signature](../storage/common/storage-security-guide.md#data-plane-security).

Deze methode kunt u het maken van betrouwbare back-ups van uw gegevens van een apparaat in een blob-container die u beheert.

De **ExportDevicesAsync** methode worden twee parameters:

* Een *tekenreeks* die een URI van een blob-container bevat. Deze URI moet een SAS-token met schrijftoegang tot de container verleent bevatten. De taak wordt een blok-blob in deze container voor het opslaan van de gegevens van het apparaat geserialiseerde exporteren. De SAS-token moet deze machtigingen zijn:

   ```csharp
   SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.Read 
     | SharedAccessBlobPermissions.Delete
   ```

* Een *Booleaanse* die aangeeft dat als u wilt uitsluiten van verificatiesleutels van uw gegevens exporteren. Als **false**, verificatiesleutels zijn opgenomen in de uitvoer van de export. Anders sleutels worden geëxporteerd als **null**.

De volgende C#-codefragment laat zien hoe u een exporttaak met verificatiesleutels apparaat in de gegevens exporteren te starten en regelmatig wordt bevraagd voor voltooiing:

```csharp
// Call an export job on the IoT Hub to retrieve all devices
JobProperties exportJob = 
  await registryManager.ExportDevicesAsync(containerSasUri, false);

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

De taak slaat de uitvoer ervan weergegeven in de opgegeven blob-container als een blok-blob met de naam van de **devices.txt**. De uitvoergegevens bestaat uit JSON-geserialiseerd apparaatgegevens, met één apparaat per regel.

Het volgende voorbeeld ziet u de uitvoergegevens:

```json
{"id":"Device1","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device2","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device3","eTag":"MA==","status":"disabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device4","eTag":"MA==","status":"disabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device5","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
```

Als een apparaat dubbele gegevens heeft, klikt u vervolgens de dubbele gegevens ook samen met de apparaatgegevens geëxporteerd. Het volgende voorbeeld ziet deze indeling. Alle gegevens van de regel 'twinETag' totdat het einde dubbele gegevens.

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

Als u toegang tot deze gegevens in de code nodig hebt, kunt u eenvoudig deserialiseren deze gegevens met de **ExportImportDevice** klasse. De volgende C#-codefragment laat zien hoe om te lezen apparaatgegevens die eerder is geëxporteerd naar een blok-blob:

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

## <a name="import-devices"></a>Apparaten importeren

De **ImportDevicesAsync** methode in de **RegistryManager** klasse kunt u bulksgewijs importeren en synchroniseren van bewerkingen uitvoeren in een id-register van IoT hub. Net als de **ExportDevicesAsync** methode, de **ImportDevicesAsync** methode gebruikt de **taak** framework.

Let erop dat met behulp van de **ImportDevicesAsync** methode omdat naast het inrichten van nieuwe apparaten in uw id-register, kan deze ook bijwerken en verwijderen van bestaande apparaten.

> [!WARNING]
> Een importbewerking kan niet ongedaan worden gemaakt. Altijd back-up van uw bestaande gegevens met behulp van de **ExportDevicesAsync** methode naar een andere blob-container, voordat u bulksgewijs wijzigingen in uw id-register.

De **ImportDevicesAsync** methode heeft twee parameters:

* Een *tekenreeks* die een URI van bevat een [Azure Storage](../storage/index.yml) blob-container wilt gebruiken als *invoer* aan de job. Deze URI moet een SAS-token die leestoegang tot de container verleent bevatten. Deze container mag een blob met de naam van de **devices.txt** die de geserialiseerde apparaatgegevens importeren in uw register-id's bevat. De gegevens importeren moet bevatten informatie over de apparaten in dezelfde JSON-indeling die de **ExportImportDevice** taak wordt gebruikt bij het maken van een **devices.txt** blob. De SAS-token moet deze machtigingen zijn:

   ```csharp
   SharedAccessBlobPermissions.Read
   ```

* Een *tekenreeks* die een URI van bevat een [Azure Storage](https://azure.microsoft.com/documentation/services/storage/) blob-container wilt gebruiken als *uitvoer* van de taak. De taak wordt een blok-blob in deze container om op te slaan foutgegevens uit het voltooide importeren **taak**. De SAS-token moet deze machtigingen zijn:

   ```csharp
   SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.Read 
     | SharedAccessBlobPermissions.Delete
   ```

> [!NOTE]
> De twee parameters naar kunnen verwijzen met de dezelfde blob-container. De afzonderlijke parameters inschakelen gewoon meer controle over uw gegevens zoals de uitvoercontainer aanvullende machtigingen vereist.

De volgende C#-codefragment laat zien hoe een import-taak starten:

```csharp
JobProperties importJob = 
   await registryManager.ImportDevicesAsync(containerSasUri, containerSasUri);
```

Deze methode kan ook worden gebruikt voor het importeren van de gegevens voor het dubbele apparaat. De notatie voor de gegevensinvoer is hetzelfde als de notatie die wordt weergegeven in de **ExportDevicesAsync** sectie. Op deze manier kunt u de geëxporteerde gegevens opnieuw importeren. De **$metadata** is optioneel.

## <a name="import-behavior"></a>Gedrag importeren

U kunt de **ImportDevicesAsync** methode om uit te voeren van de volgende bulksgewijs bewerkingen in uw id-register:

* Registratie van nieuwe apparaten bulksgewijs
* Bulksgewijs verwijderen van bestaande apparaten
* Bulksgewijs verandert de status (in- of uitschakelen van apparaten)
* Toewijzing van nieuwe apparaat verificatiesleutels bulksgewijs
* Automatisch opnieuw genereren van verificatiesleutels apparaat bulksgewijs
* Dubbele gegevens bulksgewijs bijwerken

U kunt een willekeurige combinatie van de voorgaande bewerkingen in een enkel uitvoeren **ImportDevicesAsync** aanroepen. Bijvoorbeeld, kunt u nieuwe apparaten registreren en verwijderen of bijwerken van bestaande apparaten op hetzelfde moment. Wanneer gebruikt in combinatie met de **ExportDevicesAsync** methode, u kunt al uw apparaten volledig migreren vanaf een IoT-hub naar een andere.

Als het importbestand dubbele metagegevens bevat, overschrijft deze metagegevens van de metagegevens van de bestaande dubbel. Als het importbestand bevat geen dubbele metagegevens, klikt u vervolgens alleen de `lastUpdateTime` metagegevens wordt bijgewerkt met behulp van de huidige tijd.

Gebruik het optionele **ImportMode %** eigenschap in de serialisatie-gegevens importeren voor elk apparaat voor het beheren van de import proces per apparaat. De **ImportMode %** eigenschap heeft de volgende opties:

| importMode | Description |
| --- | --- |
| **createOrUpdate** |Als een apparaat niet met de opgegeven bestaat **id**, het zojuist is ingeschreven. <br/>Als het apparaat al bestaat, wordt de bestaande gegevens overschreven met de opgegeven invoergegevens zonder betrekking tot de **ETag** waarde. <br> De gebruiker kan eventueel dubbele gegevens samen met de apparaatgegevens opgeven. Etag van het dubbele, wordt indien opgegeven, verwerkt onafhankelijk van de etag van het apparaat. Als er een niet-overeenkomend met het bestaande dubbele etag is, wordt er een fout naar het logboekbestand geschreven. |
| **maken** |Als een apparaat niet met de opgegeven bestaat **id**, het zojuist is ingeschreven. <br/>Als het apparaat al bestaat, wordt er een fout naar het logboekbestand geschreven. <br> De gebruiker kan eventueel dubbele gegevens samen met de apparaatgegevens opgeven. Etag van het dubbele, wordt indien opgegeven, verwerkt onafhankelijk van de etag van het apparaat. Als er een niet-overeenkomend met het bestaande dubbele etag is, wordt er een fout naar het logboekbestand geschreven. |
| **Update** |Als er al een apparaat met de opgegeven bestaat **id**, bestaande gegevens wordt overschreven met de opgegeven invoergegevens zonder betrekking tot de **ETag** waarde. <br/>Als het apparaat niet bestaat nog, wordt er een fout naar het logboekbestand geschreven. |
| **updateIfMatchETag** |Als er al een apparaat met de opgegeven bestaat **id**, bestaande gegevens wordt overschreven met de opgegeven ingevoerde gegevens alleen als er een **ETag** overeenkomen. <br/>Als het apparaat niet bestaat nog, wordt er een fout naar het logboekbestand geschreven. <br/>Als er een **ETag** incompatibel, een fout wordt geschreven naar het logboekbestand. |
| **createOrUpdateIfMatchETag** |Als een apparaat niet met de opgegeven bestaat **id**, het zojuist is ingeschreven. <br/>Als het apparaat al bestaat, de bestaande gegevens wordt overschreven met de opgegeven ingevoerde gegevens alleen als er een **ETag** overeenkomen. <br/>Als er een **ETag** incompatibel, een fout wordt geschreven naar het logboekbestand. <br> De gebruiker kan eventueel dubbele gegevens samen met de apparaatgegevens opgeven. Etag van het dubbele, wordt indien opgegeven, verwerkt onafhankelijk van de etag van het apparaat. Als er een niet-overeenkomend met het bestaande dubbele etag is, wordt er een fout naar het logboekbestand geschreven. |
| **verwijderen** |Als er al een apparaat met de opgegeven bestaat **id**, wordt deze verwijderd zonder betrekking tot de **ETag** waarde. <br/>Als het apparaat niet bestaat nog, wordt er een fout naar het logboekbestand geschreven. |
| **deleteIfMatchETag** |Als er al een apparaat met de opgegeven bestaat **id**, wordt deze verwijderd alleen als er een **ETag** overeenkomen. Als het apparaat niet bestaat nog, wordt er een fout naar het logboekbestand geschreven. <br/>Als er een ETag komt niet overeen, wordt er een fout naar het logboekbestand geschreven. |

> [!NOTE]
> Als de serialisatiegegevens niet expliciet definieert een **ImportMode %** vlag voor een apparaat, wordt standaard **createOrUpdate** tijdens het importeren.

## <a name="import-devices-example--bulk-device-provisioning"></a>Voorbeeld van de apparaten te importeren: bulksgewijs apparaatinrichting

De volgende C#-codevoorbeeld laat zien hoe u voor het genereren van meerdere apparaat-id's die:

* Verificatiesleutels bevatten.
* Schrijven die gegevens van een apparaat naar een blok-blob.
* Importeer de apparaten in het id-register.

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
JobProperties importJob =
   await registryManager.ImportDevicesAsync(containerSasUri, containerSasUri);

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

## <a name="import-devices-example--bulk-deletion"></a>Voorbeeld van invoer apparaten – bulksgewijs verwijderen

Het volgende codevoorbeeld laat zien hoe de apparaten die u hebt toegevoegd met behulp van het vorige codevoorbeeld verwijderen:

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

## <a name="get-the-container-sas-uri"></a>Haal de SAS-URI

Het volgende codevoorbeeld ziet u hoe u voor het genereren van een [SAS-URI](../storage/blobs/storage-dotnet-shared-access-signature-part-2.md) met lezen, schrijven en verwijderen van machtigingen voor een blob-container:

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

In dit artikel hebt u geleerd hoe u bulksgewijs bewerkingen op het id-register in een IoT-hub uitvoert. Volg deze koppelingen voor meer informatie over het beheren van Azure IoT Hub:

* [Metrische gegevens van IoT Hub](iot-hub-metrics.md)
* [Controle van bewerkingen](iot-hub-operations-monitoring.md)

Als u wilt de mogelijkheden van IoT Hub verder verkennen, Zie:

* [Ontwikkelaarshandleiding voor IoT Hub](iot-hub-devguide.md)
* [AI implementeren op edge-apparaten met Azure IoT Edge](../iot-edge/tutorial-simulate-device-linux.md)

Om te verkennen met behulp van de IoT Hub Device Provisioning Service inschakelen zero-touch, just-in-time inrichting, Zie: 

* [Azure IoT Hub Device Provisioning Service](/azure/iot-dps)
