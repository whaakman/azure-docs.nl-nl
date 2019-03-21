---
title: Uw gegevens worden geëxporteerd naar Azure Blob Storage | Microsoft Docs
description: Het exporteren van gegevens vanuit uw Azure IoT Central-toepassing naar Azure Blob Storage
services: iot-central
author: viv-liu
ms.author: viviali
ms.date: 12/07/2018
ms.topic: conceptual
ms.service: iot-central
manager: peterpr
ms.openlocfilehash: f6e44b21a2a2e174ffa49073fdeb8cc96910a69e
ms.sourcegitcommit: ab6fa92977255c5ecbe8a53cac61c2cd2a11601f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58295076"
---
# <a name="export-your-data-to-azure-blob-storage"></a>Uw gegevens worden geëxporteerd naar Azure Blob Storage

*In dit onderwerp is bedoeld voor beheerders.*

In dit artikel wordt beschreven hoe u van de continue export-functie in Azure IoT Central periodiek gegevens exporteren naar uw **Azure Blob storage-account**. U kunt exporteren **metingen**, **apparaten**, en **apparaatsjablonen** tot bestanden in Apache Avro-indeling. De geëxporteerde gegevens kunnen worden gebruikt voor analyses koude pad, zoals modellen voor training in Azure Machine Learning of op de lange termijn trendanalyse in Microsoft Power BI.

> [!Note]
> Nogmaals, als u continue gegevensexport inschakelt, krijgt u alleen de gegevens vanaf dat moment standaardtarieven. Gegevens kunnen op dit moment niet worden hersteld gedurende een periode wanneer voortdurende gegevensexport uitgeschakeld is. Als u wilt meer historische gegevens behouden, moet u voortdurende gegevensexport vroeg inschakelen.


## <a name="prerequisites"></a>Vereisten

- U moet een beheerder in uw IoT Central-toepassing


## <a name="set-up-export-destination"></a>Export doel instellen

Als u een bestaande opslag om te exporteren naar niet hebt, volgt u deze stappen:

## <a name="create-storage-account"></a>Storage-account maken

1. Maak een [nieuw opslagaccount in Azure portal](https://ms.portal.azure.com/#create/Microsoft.StorageAccount-ARM). U kunt meer informatie in [Azure Storage-docs](https://aka.ms/blobdocscreatestorageaccount).
2. Kies voor het accounttype **algemeen** of **Blob storage**.
3. Kies een abonnement. 

    > [!Note] 
    > U kunt nu gegevens exporteren naar andere abonnementen die zijn **niet hetzelfde** als voor uw betalen per gebruik IoT Central-toepassing. U verbinding maakt met een verbindingsreeks in dit geval.

4. Maak een container in uw opslagaccount. Ga naar uw opslagaccount. Onder **Blob-Service**, selecteer **door Blobs Bladeren**. Selecteer **+ Container** boven aan een nieuwe container maken


## <a name="set-up-continuous-data-export"></a>Voortdurende gegevensexport instellen

Nu dat u een opslaglocatie hebt voor het exporteren van gegevens, volg deze stappen voor het instellen van continue gegevensexport. 

1. Meld u aan uw IoT Central-toepassing.

2. Selecteer in het menu links **continue gegevensexport**.

    > [!Note]
    > Als er geen continue Export van gegevens in het menu links, bent u niet een beheerder in uw app. Neem contact op met een beheerder voor het instellen van het exporteren van gegevens.

    ![Nieuwe cde Event Hub maken](media/howto-export-data/export_menu.PNG)

3. Selecteer de **+ nieuw** knop in de rechterbovenhoek. Kies **Azure Blob Storage** als de bestemming of het exporteren. 

    > [!NOTE] 
    > Het maximum aantal uitvoer per app is vijf. 

    ![Maken van nieuwe voortdurende gegevensexport](media/howto-export-data/export_new.PNG)

4. Selecteer in de vervolgkeuzelijst uw **Opslagaccount naamruimte**. U kunt ook de laatste optie kiezen in de lijst die is **een verbindingsreeks invoeren**. 

    > [!NOTE] 
    > U ziet alleen de naamruimten van de Storage-Accounts in de **hetzelfde abonnement als uw app IoT Central**. Als u exporteren naar een bestemming buiten dit abonnement wilt, kiest u **een verbindingsreeks invoeren** en raadpleegt u stap 5.

    > [!NOTE] 
    > Voor zeven dagen proefversie apps, de enige manier om het configureren van doorlopende gegevens exporteren, is via een verbindingsreeks. Dit komt doordat zeven dagen proefversie apps nog geen een gekoppelde Azure-abonnement.

    ![Nieuwe cde Event Hub maken](media/howto-export-data/export-create-blob.png)

5. (Optioneel) Als u ervoor hebt gekozen **een verbindingsreeks invoeren**, een nieuwe verschijnt u plak de verbindingsreeks. Om op te halen van de verbindingsreeks voor uw:
    - Storage-account, gaat u naar het opslagaccount in Azure portal.
        - Onder **instellingen**, selecteer **toegangssleutels**
        - Kopieer de verbindingsreeks key1 of de key2-verbindingsreeks
 
6. Een Container kiezen uit de vervolgkeuzelijst.

7. Onder **gegevens naar de export**, elk type gegevens wilt exporteren door het type in te stellen **op**.

6. Als u wilt inschakelen voortdurende gegevensexport, zorg ervoor dat **gegevensexport** is **op**. Selecteer **Opslaan**.

  ![Voortdurende gegevensexport configureren](media/howto-export-data/export-list-blob.png)

7. Na een paar minuten verschijnt uw gegevens in uw gekozen bestemming.


## <a name="export-to-azure-blob-storage"></a>Exporteren naar Azure Blob-opslag

Metingen, apparaten en sjablonen apparaatgegevens worden geëxporteerd naar uw opslagaccount eenmaal per minuut, met elk bestand met de batch wijzigingen sinds de laatste bestand geëxporteerd. De geëxporteerde gegevens [Apache Avro](https://avro.apache.org/docs/current/index.html) formatteren en aan drie mappen worden geëxporteerd. De standaardpaden in uw storage-account zijn:
- Berichten: {container}/measurements/{hubname}/{YYYY}/{MM}/{dd}/{hh}/{mm}/{filename}.avro
- Apparaten: {container}/devices/{YYYY}/{MM}/{dd}/{hh}/{mm}/{filename}.avro
- Apparaatsjablonen: {container}/deviceTemplates/{YYYY}/{MM}/{dd}/{hh}/{mm}/{filename}.avro

### <a name="measurements"></a>Metingen

De geëxporteerde metingen gegevens hebben de nieuwe berichten ontvangen met IoT Central van alle apparaten in die tijd. De geëxporteerde bestanden gebruiken dezelfde indeling als de berichtbestanden geëxporteerd door [IoT Hub-berichtroutering](https://docs.microsoft.com/azure/iot-hub/iot-hub-csharp-csharp-process-d2c) naar Blob-opslag.

> [!NOTE]
> De apparaten die het verzenden van de metingen worden vertegenwoordigd door de apparaat-id's (Zie de volgende gedeelten). Als u de namen van de apparaten, de apparaat-momentopnamen te exporteren. Elke berichtenrecord correleren met behulp van de **connectionDeviceId** die overeenkomt met de **deviceId** van de record van apparaat.

Het volgende voorbeeld ziet u een record in een gedecodeerde Avro-bestand:

```json
{
    "EnqueuedTimeUtc": "2018-06-11T00:00:08.2250000Z",
    "Properties": {},
    "SystemProperties": {
        "connectionDeviceId": "<connectionDeviceId>",
        "connectionAuthMethod": "{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\",\"acceptingIpFilterRule\":null}",
        "connectionDeviceGenerationId": "<generationId>",
        "enqueuedTime": "2018-06-11T00:00:08.2250000Z"
    },
    "Body": "{\"humidity\":80.59100954598546,\"magnetometerX\":0.29451796907056726,\"magnetometerY\":0.5550332126050068,\"magnetometerZ\":-0.04116681874733441,\"connectivity\":\"connected\",\"opened\":\"triggered\"}"
}
```

### <a name="devices"></a>Apparaten

Wanneer voortdurende gegevensexport eerst is ingeschakeld, wordt een momentopname van een enkel met alle apparaten wordt geëxporteerd. Elk apparaat bevat:
- `id` van het apparaat in IoT Central
- `name` van het apparaat
- `deviceId` van [Device Provisioning Service](https://aka.ms/iotcentraldocsdps)
- Sjabloon van apparaatgegevens
- Waarden van eigenschappen
- Waarden in te stellen

Een nieuwe momentopname is eenmaal per minuut geschreven. De snapshot bevat:

- Nieuwe apparaten toegevoegd sinds de laatste momentopname.
- Apparaten met gewijzigde eigenschappen en waarden in te stellen sinds de laatste momentopname.

> [!NOTE]
> Apparaten die zijn verwijderd, omdat de laatste momentopname worden niet geëxporteerd. De momentopnamen hebt op dit moment geen indicatoren voor verwijderde apparaten.
>
> De apparaat-sjabloon die elk apparaat hoort wordt vertegenwoordigd door een sjabloon voor apparaat-ID. Als u de naam van de sjabloon van het apparaat, exporteert u de momentopnamen van de sjabloon apparaat.

Een record in het gedecodeerde Avro-bestand kan er als volgt uitzien:

```json
{
    "id": "<id>",
    "name": "Refrigerator 2",
    "simulated": true,
    "deviceId": "<deviceId>",
    "deviceTemplate": {
        "id": "<template id>",
        "version": "1.0.0"
    },
    "properties": {
        "cloud": {
            "location": "New York",
            "maintCon": true,
            "tempThresh": 20
        },
        "device": {
            "lastReboot": "2018-02-09T22:22:47.156Z"
        }
    },
    "settings": {
        "device": {
            "fanSpeed": 0
        }
    }
}
```

### <a name="device-templates"></a>Apparaatsjablonen

Wanneer voortdurende gegevensexport eerst is ingeschakeld, wordt een momentopname van een enkel met alle apparaatsjablonen geëxporteerd. Elke sjabloon apparaat bevat:
- `id` van de sjabloon voor apparaat
- `name` van de sjabloon voor apparaat
- `version` van de sjabloon voor apparaat
- Meting gegevenstypen en min/max-waarden.
- Typen eigenschappen voor gegevens en standaardwaarden.
- Gegevenstypen en standaardwaarden instellen.

Een nieuwe momentopname is eenmaal per minuut geschreven. De snapshot bevat:

- Nieuwe apparaatsjablonen toegevoegd sinds de laatste momentopname.
- Apparaatsjablonen met gewijzigde metingen, eigenschap en het instellen van definities sinds de laatste momentopname.

> [!NOTE]
> Apparaatsjablonen verwijderd sinds de laatste momentopname worden niet geëxporteerd. De momentopnamen hebt op dit moment geen indicatoren voor verwijderde sjablonen.

Een record in het gedecodeerde Avro-bestand kan er als volgt:

```json
{
    "id": "<id>",
    "name": "Refrigerated Vending Machine",
    "version": "1.0.0",
    "measurements": {
        "telemetry": {
            "humidity": {
                "dataType": "double",
                "name": "Humidity"
            },
            "magnetometerX": {
                "dataType": "double",
                "name": "Magnetometer X"
            },
            "magnetometerY": {
                "dataType": "double",
                "name": "Magnetometer Y"
            },
            "magnetometerZ": {
                "dataType": "double",
                "name": "Magnetometer Z"
            }
        },
        "states": {
            "connectivity": {
                "dataType": "enum",
                "name": "Connectivity"
            }
        },
        "events": {
            "opened": {
                "name": "Door Opened",
                "category": "informational"
            }
        }
    },
    "settings": {
        "device": {
            "fanSpeed": {
                "dataType": "double",
                "name": "Fan Speed",
                "initialValue": 0
            }
        }
    },
    "properties": {
        "cloud": {
            "location": {
                "dataType": "string",
                "name": "Location",
                "initialValue": "Seattle"
            },
            "maintCon": {
                "dataType": "boolean",
                "name": "Maintenance Contract",
                "initialValue": true
            },
            "tempThresh": {
                "dataType": "double",
                "name": "Temperature Alert Threshold",
                "initialValue": 30
            }
        },
        "device": {
            "lastReboot": {
                "dataType": "dateTime",
                "name": "Last Reboot"
            }
        }
    }
}
```

## <a name="read-exported-avro-files"></a>Lezen geëxporteerd Avro-bestanden

Avro is een binaire indeling, zodat de bestanden in hun onbewerkte status kunnen niet worden gelezen. De bestanden kunnen worden gedecodeerd naar JSON-indeling. De volgende voorbeelden laten zien hoe de metingen, apparaten en apparaatsjablonen Avro-bestanden parseren. De voorbeelden komen overeen met de voorbeelden die worden beschreven in de vorige sectie.

### <a name="read-avro-files-by-using-python"></a>Avro-bestanden lezen met behulp van Python

#### <a name="install-pandas-and-the-pandavro-package"></a>Pandas en het pakket pandavro installeren

```python
pip install pandas
pip install pandavro
```

#### <a name="parse-a-measurements-avro-file"></a>Een metingen Avro-bestand parseren

```python
import json
import pandavro as pdx
import pandas as pd

def parse(filePath):
    # Pandavro loads the Avro file into a pandas DataFrame
    # where each record is a single row.
    measurements = pdx.from_avro(filePath)

    # This example creates a new DataFrame and loads a series
    # for each column that's mapped into a column in our new DataFrame.
    transformed = pd.DataFrame()

    # The SystemProperties column contains a dictionary
    # with the device ID located under the connectionDeviceId key.
    transformed["device_id"] = measurements["SystemProperties"].apply(lambda x: x["connectionDeviceId"])

    # The Body column is a series of UTF-8 bytes that is stringified
    # and parsed as JSON. This example pulls the humidity property
    # from each column to get the humidity field.
    transformed["humidity"] = measurements["Body"].apply(lambda x: json.loads(bytes(x).decode('utf-8'))["humidity"])

    # Finally, print the new DataFrame with our device IDs and humidities.
    print(transformed)

```

#### <a name="parse-a-devices-avro-file"></a>Een apparaten Avro-bestand parseren

```python
import json
import pandavro as pdx
import pandas as pd

def parse(filePath):
    # Pandavro loads the Avro file into a pandas DataFrame
    # where each record is a single row.
    devices = pdx.from_avro(filePath)

    # This example creates a new DataFrame and loads a series
    # for each column that's mapped into a column in our new DataFrame.
    transformed = pd.DataFrame()

    # The device ID is available in the id column.
    transformed["device_id"] = devices["deviceId"]

    # The template ID and version are present in a dictionary under
    # the deviceTemplate column.
    transformed["template_id"] = devices["deviceTemplate"].apply(lambda x: x["id"])
    transformed["template_version"] = devices["deviceTemplate"].apply(lambda x: x["version"])

    # The fanSpeed setting value is located in a nested dictionary
    # under the settings column.
    transformed["fan_speed"] = devices["settings"].apply(lambda x: x["device"]["fanSpeed"])

    # Finally, print the new DataFrame with our device and template
    # information, along with the value of the fan speed.
    print(transformed)

```

#### <a name="parse-a-device-templates-avro-file"></a>Een apparaat sjablonen Avro-bestand parseren

```python
import json
import pandavro as pdx
import pandas as pd

def parse(filePath):
    # Pandavro loads the Avro file into a pandas DataFrame
    # where each record is a single row.
    templates = pdx.from_avro(filePath)

    # This example creates a new DataFrame and loads a series
    # for each column that's mapped into a column in our new DataFrame.
    transformed = pd.DataFrame()

    # The template and version are available in the id and version columns.
    transformed["template_id"] = templates["id"]
    transformed["template_version"] = templates["version"]

    # The fanSpeed setting value is located in a nested dictionary
    # under the settings column.
    transformed["fan_speed"] = templates["settings"].apply(lambda x: x["device"]["fanSpeed"])

    # Finally, print the new DataFrame with our device and template
    # information, along with the value of the fan speed.
    print(transformed)
```

### <a name="read-avro-files-by-using-c"></a>Lezen Avro-bestanden met behulp vanC#

#### <a name="install-the-microsofthadoopavro-package"></a>Installeer het pakket Microsoft.Hadoop.Avro

```csharp
Install-Package Microsoft.Hadoop.Avro -Version 1.5.6
```

#### <a name="parse-a-measurements-avro-file"></a>Een metingen Avro-bestand parseren

```csharp
using Microsoft.Hadoop.Avro;
using Microsoft.Hadoop.Avro.Container;
using Newtonsoft.Json;

public static async Task Run(string filePath)
{
    using (var fileStream = File.OpenRead(filePath))
    {
        using (var reader = AvroContainer.CreateGenericReader(fileStream))
        {
            // For one Avro container, where a container can contain multiple blocks,
            // loop through each block in the container.
            while (reader.MoveNext())
            {
                // Loop through the Avro records in the block and extract the fields.
                foreach (AvroRecord record in reader.Current.Objects)
                {
                    var systemProperties = record.GetField<IDictionary<string, object>>("SystemProperties");
                    var deviceId = systemProperties["connectionDeviceId"] as string;
                    Console.WriteLine("Device ID: {0}", deviceId);

                    using (var stream = new MemoryStream(record.GetField<byte[]>("Body")))
                    {
                        using (var streamReader = new StreamReader(stream, Encoding.UTF8))
                        {
                            var body = JsonSerializer.Create().Deserialize(streamReader, typeof(IDictionary<string, dynamic>)) as IDictionary<string, dynamic>;
                            var humidity = body["humidity"];
                            Console.WriteLine("Humidity: {0}", humidity);
                        }
                    }
                }
            }
        }
    }
}
```

#### <a name="parse-a-devices-avro-file"></a>Een apparaten Avro-bestand parseren

```csharp
using Microsoft.Hadoop.Avro;
using Microsoft.Hadoop.Avro.Container;

public static async Task Run(string filePath)
{
    using (var fileStream = File.OpenRead(filePath))
    {
        using (var reader = AvroContainer.CreateGenericReader(fileStream))
        {
            // For one Avro container, where a container can contain multiple blocks,
            // loop through each block in the container.
            while (reader.MoveNext())
            {
                // Loop through the Avro records in the block and extract the fields.
                foreach (AvroRecord record in reader.Current.Objects)
                {
                    // Get the field value directly. You can also yield return
                    // records and make the function IEnumerable<AvroRecord>.
                    var deviceId = record.GetField<string>("deviceId");

                    // The device template information is stored in a sub-record
                    // under the deviceTemplate field.
                    var deviceTemplateRecord = record.GetField<AvroRecord>("deviceTemplate");
                    var templateId = deviceTemplateRecord.GetField<string>("id");
                    var templateVersion = deviceTemplateRecord.GetField<string>("version");

                    // The settings and properties are nested two levels deep.
                    // The first level indicates settings or properties.
                    // The second level indicates the type of setting or property.
                    var settingsRecord = record.GetField<AvroRecord>("settings");
                    var deviceSettingsRecord = settingsRecord.GetField<IDictionary<string, dynamic>>("device");
                    var fanSpeed = deviceSettingsRecord["fanSpeed"];
                    
                    Console.WriteLine(
                        "Device ID: {0}, Template ID: {1}, Template Version: {2}, Fan Speed: {3}",
                        deviceId,
                        templateId,
                        templateVersion,
                        fanSpeed
                    );
                }
            }
        }
    }
}

```

#### <a name="parse-a-device-templates-avro-file"></a>Een apparaat sjablonen Avro-bestand parseren

```csharp
using Microsoft.Hadoop.Avro;
using Microsoft.Hadoop.Avro.Container;

public static async Task Run(string filePath)
{
    using (var fileStream = File.OpenRead(filePath))
    {
        using (var reader = AvroContainer.CreateGenericReader(fileStream))
        {
            // For one Avro container, where a container can contain multiple blocks,
            // loop through each block in the container.
            while (reader.MoveNext())
            {
                // Loop through the Avro records in the block and extract the fields.
                foreach (AvroRecord record in reader.Current.Objects)
                {
                    // Get the field value directly. You can also yield return
                    // records and make the function IEnumerable<AvroRecord>.
                    var id = record.GetField<string>("id");
                    var version = record.GetField<string>("version");

                    // The settings and properties are nested two levels deep.
                    // The first level indicates settings or properties.
                    // The second level indicates the type of setting or property.
                    var settingsRecord = record.GetField<AvroRecord>("settings");
                    var deviceSettingsRecord = settingsRecord.GetField<IDictionary<string, dynamic>>("device");
                    var fanSpeed = deviceSettingsRecord["fanSpeed"];
                    
                    Console.WriteLine(
                        "ID: {1}, Version: {2}, Fan Speed: {3}",
                        id,
                        version,
                        fanSpeed
                    );
                }
            }
        }
    }
}
```

### <a name="read-avro-files-by-using-javascript"></a>Avro-bestanden lezen met behulp van Javascript

#### <a name="install-the-avsc-package"></a>Installeer het pakket avsc

```javascript
npm install avsc
```

#### <a name="parse-a-measurements-avro-file"></a>Een metingen Avro-bestand parseren

```javascript
const avro = require('avsc');

// Read the Avro file. Parse the device ID and humidity from each record.
async function parse(filePath) {
    const records = await load(filePath);
    for (const record of records) {
        // Fetch the device ID from the system properties.
        const deviceId = record.SystemProperties.connectionDeviceId;

        // Convert the body from a buffer to a string and parse it.
        const body = JSON.parse(record.Body.toString());

        // Get the humidty property from the body.
        const humidity = body.humidity;

        // Log the retrieved device ID and humidity.
        console.log(`Device ID: ${deviceId}`);
        console.log(`Humidity: ${humidity}`);
    }
}

function load(filePath) {
    return new Promise((resolve, reject) => {
        // The file decoder emits each record as a data event on a stream.
        // Collect the records into an array and return them at the end.
        const records = [];
        avro.createFileDecoder(filePath)
            .on('data', record => { records.push(record); })
            .on('end', () => resolve(records))
            .on('error', reject);
    });
}
```

#### <a name="parse-a-devices-avro-file"></a>Een apparaten Avro-bestand parseren

```javascript
const avro = require('avsc');

// Read the Avro file. Parse the device and template identification
// information and the fanSpeed setting for each device record.
async function parse(filePath) {
    const records = await load(filePath);
    for (const record of records) {
        // Fetch the device ID from the deviceId property.
        const deviceId = record.deviceId;

        // Fetch the template ID and version from the deviceTemplate property.
        const deviceTemplateId = record.deviceTemplate.id;
        const deviceTemplateVersion = record.deviceTemplate.version;

        // Get the fanSpeed from the nested device settings property.
        const fanSpeed = record.settings.device.fanSpeed;

        // Log the retrieved device ID and humidity.
        console.log(`deviceID: ${deviceId}, Template ID: ${deviceTemplateId}, Template Version: ${deviceTemplateVersion}, Fan Speed: ${fanSpeed}`);
    }
}

function load(filePath) {
    return new Promise((resolve, reject) => {
        // The file decoder emits each record as a data event on a stream.
        // Collect the records into an array and return them at the end.
        const records = [];
        avro.createFileDecoder(filePath)
            .on('data', record => { records.push(record); })
            .on('end', () => resolve(records))
            .on('error', reject);
    });
}
```

#### <a name="parse-a-device-templates-avro-file"></a>Een apparaat sjablonen Avro-bestand parseren

```javascript
const avro = require('avsc');

// Read the Avro file. Parse the device and template identification
// information and the fanSpeed setting for each device record.
async function parse(filePath) {
    const records = await load(filePath);
    for (const record of records) {
        // Fetch the template ID and version from the id and verison properties.
        const templateId = record.id;
        const templateVersion = record.version;

        // Get the fanSpeed from the nested device settings property.
        const fanSpeed = record.settings.device.fanSpeed;

        // Log the retrieved device id and humidity.
        console.log(`Template ID: ${templateId}, Template Version: ${templateVersion}, Fan Speed: ${fanSpeed}`);
    }
}

function load(filePath) {
    return new Promise((resolve, reject) => {
        // The file decoder emits each record as a data event on a stream.
        // Collect the records into an array and return them at the end.
        const records = [];
        avro.createFileDecoder(filePath)
            .on('data', record => { records.push(record); })
            .on('end', () => resolve(records))
            .on('error', reject);
    });
}
```

## <a name="next-steps"></a>Volgende stappen

Nu dat u hoe u uw gegevens te exporteren weet, gaat u verder met de volgende stap:

> [!div class="nextstepaction"]
> [Hoe u uw gegevens in Power BI visualiseren](howto-connect-powerbi.md)
