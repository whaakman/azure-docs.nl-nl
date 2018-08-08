---
title: Exporteren van gegevens in Azure IoT Central | Microsoft Docs
description: Het exporteren van gegevens vanuit uw Azure IoT Central-toepassing
services: iot-central
author: viv-liu
ms.author: viviali
ms.date: 07/3/2018
ms.topic: article
ms.prod: azure-iot-central
manager: peterpr
ms.openlocfilehash: 3ca2bc56c03e5bbabbd9b2f17edc621bdd94b02f
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2018
ms.locfileid: "39622480"
---
# <a name="export-your-data-in-azure-iot-central"></a>Uw gegevens in Azure IoT Central exporteren

In dit artikel wordt beschreven hoe u de functie continue export gebruiken in Azure IoT Central periodiek gegevens exporteren naar uw Azure Blob storage-account. U kunt exporteren **metingen**, **apparaten**, en **apparaatsjablonen** naar bestanden met de [Apache AVRO](https://avro.apache.org/docs/current/index.html) indeling. De geëxporteerde gegevens kunnen worden gebruikt voor analyses koude pad, zoals modellen voor training in Azure Machine Learning of op de lange termijn trendanalyse in Microsoft Power BI.

> [!Note]
> Wanneer u doorlopend gegevens exporteren inschakelt, kunt u alleen de gegevens ophalen vanaf dat moment standaardtarieven. Gegevens kunnen op dit moment niet worden hersteld gedurende een periode wanneer voortdurende gegevensexport uitgeschakeld is. Als u wilt meer historische gegevens behouden, moet u voortdurende gegevensexport vroeg inschakelen.

## <a name="prerequisites"></a>Vereisten

- Een uitgebreide 30-daagse proefversie IoT Central-toepassing, of een betaalde toepassing.
- Een Azure-account met een Azure-abonnement.
- Hetzelfde Azure-account is een beheerder in uw IoT Central-toepassing.
- Hetzelfde Azure-account heeft machtigingen voor het maken van een storage-account of toegang tot een bestaand opslagaccount in hetzelfde Azure-abonnement.

## <a name="types-of-data-to-export"></a>Typen gegevens exporteren

### <a name="measurements"></a>Metingen

De metingen die apparaten verzenden worden naar uw opslagaccount eenmaal per minuut geëxporteerd. De gegevens hebben de nieuwe berichten ontvangen met IoT Central van alle apparaten in die tijd. De geëxporteerde AVRO-bestanden gebruiken dezelfde indeling als de berichtbestanden geëxporteerd door [IoT Hub-berichtroutering](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-csharp-csharp-process-d2c) naar Blob-opslag.

> [!NOTE]
> De apparaten die het verzenden van de metingen worden vertegenwoordigd door de apparaat-id's (Zie de volgende gedeelten). Als u de namen van de apparaten, de apparaat-momentopnamen te exporteren. Elke berichtenrecord correleren met behulp van de **connectionDeviceId** die overeenkomt met de apparaat-ID.

Het volgende voorbeeld ziet u een record in een gedecodeerde AVRO-bestand:

```json
{
    "EnqueuedTimeUtc": "2018-06-11T00:00:08.2250000Z",
    "Properties": {},
    "SystemProperties": {
        "connectionDeviceId": "2383d8ba-c98c-403a-b4d5-8963859643bb",
        "connectionAuthMethod": "{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\",\"acceptingIpFilterRule\":null}",
        "connectionDeviceGenerationId": "636614021491644195",
        "enqueuedTime": "2018-06-11T00:00:08.2250000Z"
    },
    "Body": "{\"humidity\":80.59100954598546,\"magnetometerX\":0.29451796907056726,\"magnetometerY\":0.5550332126050068,\"magnetometerZ\":-0.04116681874733441,\"connectivity\":\"connected\",\"opened\":\"triggered\"}"
}
```

### <a name="devices"></a>Apparaten

Wanneer voortdurende gegevensexport eerst is ingeschakeld, wordt een momentopname van een enkel met alle apparaten wordt geëxporteerd. De snapshot bevat:
- Apparaat-id's.
- Apparaatnamen.
- Apparaat sjabloon-id.
- Eigenschapswaarden.
- Waarden instellen.

Een nieuwe momentopname is eenmaal per minuut geschreven. De snapshot bevat:

- Nieuwe apparaten toegevoegd sinds de laatste momentopname.
- Apparaten met gewijzigde eigenschappen en waarden in te stellen sinds de laatste momentopname.

> [!NOTE]
> Apparaten die zijn verwijderd, omdat de laatste momentopname worden niet geëxporteerd. De momentopnamen hebt op dit moment geen indicatoren voor verwijderde apparaten.
>
> De apparaat-sjabloon die elk apparaat hoort wordt vertegenwoordigd door een sjabloon voor apparaat-ID. Als u de naam van de sjabloon van het apparaat, exporteert u de momentopnamen van de sjabloon apparaat.

Elke record in het gedecodeerde AVRO-bestand ziet eruit zoals:

```json
{
    "id": "2383d8ba-c98c-403a-b4d5-8963859643bb",
    "name": "Refrigerator 2",
    "simulated": true,
    "deviceTemplate": {
        "id": "c318d580-39fc-4aca-b995-843719821049",
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

Wanneer voortdurende gegevensexport eerst is ingeschakeld, wordt een momentopname van een enkel met alle apparaatsjablonen geëxporteerd. De snapshot bevat: 
- Apparaat sjabloon-id.
- Meting gegevenstypen en min/max-waarden.
- Typen eigenschappen voor gegevens en standaardwaarden.
- Gegevenstypen en standaardwaarden instellen.

Een nieuwe momentopname is eenmaal per minuut geschreven. De snapshot bevat:

- Nieuwe apparaatsjablonen toegevoegd sinds de laatste momentopname.
- Apparaatsjablonen met gewijzigde metingen, eigenschap en het instellen van definities sinds de laatste momentopname.

> [!NOTE]
> Apparaatsjablonen verwijderd sinds de laatste momentopname worden niet geëxporteerd. De momentopnamen hebt op dit moment geen indicatoren voor verwijderde sjablonen.

Elke record in het gedecodeerde AVRO-bestand ziet eruit zoals:

```json
{
    "id": "c318d580-39fc-4aca-b995-843719821049",
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

## <a name="set-up-continuous-data-export"></a>Voortdurende gegevensexport instellen

1. Als u geen Azure storage-account, [maken van een nieuw opslagaccount](https://ms.portal.azure.com/#create/Microsoft.StorageAccount-ARM) in Azure portal. De storage-account maken **in de Azure-abonnement waaraan uw toepassing IoT Central**.
    - Kies voor het accounttype **algemeen** of **Blob storage**.
    - Selecteer het abonnement waaraan uw IoT Central-toepassing. Als u het abonnement niet ziet, moet u mogelijk aanmelden bij een andere Azure-account of een aanvraag voor toegang tot het abonnement.
    - Kies een bestaande resourcegroep of maak een nieuwe. Meer informatie over [over het maken van een nieuw opslagaccount](https://aka.ms/blobdocscreatestorageaccount).

2. Maak een container in uw opslagaccount om uw IoT Central-gegevens te exporteren. Ga naar uw storage-account. Onder **Blob-Service**, selecteer **door Blobs Bladeren**. Selecteer **Container** om een nieuwe container te maken.

   ![Een container maken](media/howto-export-data/createcontainer.png)

3. Meld u aan uw IoT Central-toepassing met behulp van de dezelfde Azure-account.

4. Onder **beheer**, selecteer **gegevensexport**.

   ![Voortdurende gegevensexport configureren](media/howto-export-data/continuousdataexport.PNG)

5. In de **opslagaccount** vervolgkeuzelijst uw storage-account te selecteren. In de **Container** vervolgkeuzelijst vak, selecteert u de container. Onder **gegevens naar de export**, elk type gegevens wilt exporteren door het type in te stellen **op**.

6. Instellen om in te schakelen voortdurende gegevensexport, **gegevensexport** naar **op**. Selecteer **Opslaan**.

7. Na een paar minuten uw gegevens in uw storage-account worden weergegeven. Blader naar uw storage-account. Selecteer **door blobs Bladeren** > uw container. U ziet drie mappen voor het exporteren van gegevens. De standaardpaden voor de AVRO-bestanden met de exportgegevens zijn:
    - Berichten: {container}/measurements/{hubname}/{YYYY}/{MM}/{dd}/{hh}/{mm}/00.avro
    - Apparaten: {container}/devices/{hubname}/{YYYY}/{MM}/{dd}/{hh}/{mm}/00.avro
    - Apparaatsjablonen: {container}/deviceTemplates/{hubname}/{YYYY}/{MM}/{dd}/{hh}/{mm}/00.avro

## <a name="read-exported-avro-files"></a>Lezen geëxporteerd AVRO-bestanden

AVRO is een binaire indeling, zodat de bestanden in hun onbewerkte status kunnen niet worden gelezen. De bestanden kunnen worden gedecodeerd naar JSON-indeling. De volgende voorbeelden laten zien hoe de metingen, apparaten en apparaatsjablonen AVRO-bestanden parseren. De voorbeelden komen overeen met de voorbeelden die worden beschreven in de vorige sectie.

### <a name="read-avro-files-by-using-python"></a>AVRO-bestanden lezen met behulp van Python

#### <a name="install-pandas-and-the-pandavro-package"></a>Pandas en het pakket pandavro installeren

```python
pip install pandas
pip install pandavro
```

#### <a name="parse-a-measurements-avro-file"></a>Een metingen AVRO-bestand parseren

```python
import json
import pandavro as pdx
import pandas as pd

def parse(filePath):
    # Pandavro loads the AVRO file into a pandas DataFrame
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

#### <a name="parse-a-devices-avro-file"></a>Een apparaten AVRO-bestand parseren

```python
import json
import pandavro as pdx
import pandas as pd

def parse(filePath):
    # Pandavro loads the AVRO file into a pandas DataFrame
    # where each record is a single row.
    devices = pdx.from_avro(filePath)

    # This example creates a new DataFrame and loads a series
    # for each column that's mapped into a column in our new DataFrame.
    transformed = pd.DataFrame()

    # The device ID is available in the id column.
    transformed["device_id"] = devices["id"]

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

#### <a name="parse-a-device-templates-avro-file"></a>Een apparaat sjablonen AVRO-bestand parseren

```python
import json
import pandavro as pdx
import pandas as pd

def parse(filePath):
    # Pandavro loads the AVRO file into a pandas DataFrame
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

### <a name="read-avro-files-by-using-c"></a>Lezen AVRO-bestanden met behulp van C#

#### <a name="install-the-microsofthadoopavro-package"></a>Installeer het pakket Microsoft.Hadoop.Avro

```csharp
Install-Package Microsoft.Hadoop.Avro -Version 1.5.6
```

#### <a name="parse-a-measurements-avro-file"></a>Een metingen AVRO-bestand parseren

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
            // For one AVRO container, where a container can contain multiple blocks,
            // loop through each block in the container.
            while (reader.MoveNext())
            {
                // Loop through the AVRO records in the block and extract the fields.
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

#### <a name="parse-a-devices-avro-file"></a>Een apparaten AVRO-bestand parseren

```csharp
using Microsoft.Hadoop.Avro;
using Microsoft.Hadoop.Avro.Container;

public static async Task Run(string filePath)
{
    using (var fileStream = File.OpenRead(filePath))
    {
        using (var reader = AvroContainer.CreateGenericReader(fileStream))
        {
            // For one AVRO container, where a container can contain multiple blocks,
            // loop through each block in the container.
            while (reader.MoveNext())
            {
                // Loop through the AVRO records in the block and extract the fields.
                foreach (AvroRecord record in reader.Current.Objects)
                {
                    // Get the field value directly. You can also yield return
                    // records and make the function IEnumerable<AvroRecord>.
                    var deviceId = record.GetField<string>("id");

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
                        "ID: {0}, Template ID: {1}, Template Version: {2}, Fan Speed: {3}",
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

#### <a name="parse-a-device-templates-avro-file"></a>Een apparaat sjablonen AVRO-bestand parseren

```csharp
using Microsoft.Hadoop.Avro;
using Microsoft.Hadoop.Avro.Container;

public static async Task Run(string filePath)
{
    using (var fileStream = File.OpenRead(filePath))
    {
        using (var reader = AvroContainer.CreateGenericReader(fileStream))
        {
            // For one AVRO container, where a container can contain multiple blocks,
            // loop through each block in the container.
            while (reader.MoveNext())
            {
                // Loop through the AVRO records in the block and extract the fields.
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

### <a name="read-avro-files-by-using-javascript"></a>AVRO-bestanden lezen met behulp van Javascript

#### <a name="install-the-avsc-package"></a>Installeer het pakket avsc

```javascript
npm install avsc
```

#### <a name="parse-a-measurements-avro-file"></a>Een metingen AVRO-bestand parseren

```javascript
const avro = require('avsc');

// Read the AVRO file. Parse the device ID and humidity from each record.
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

#### <a name="parse-a-devices-avro-file"></a>Een apparaten AVRO-bestand parseren

```javascript
const avro = require('avsc');

// Read the AVRO file. Parse the device and template identification
// information and the fanSpeed setting for each device record.
async function parse(filePath) {
    const records = await load(filePath);
    for (const record of records) {
        // Fetch the device ID from the id property.
        const deviceId = record.id;

        // Fetch the template ID and version from the deviceTemplate property.
        const deviceTemplateId = record.deviceTemplate.id;
        const deviceTemplateVersion = record.deviceTemplate.version;

        // Get the fanSpeed from the nested device settings property.
        const fanSpeed = record.settings.device.fanSpeed;

        // Log the retrieved device ID and humidity.
        console.log(`ID: ${deviceId}, Template ID: ${deviceTemplateId}, Template Version: ${deviceTemplateVersion}, Fan Speed: ${fanSpeed}`);
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

#### <a name="parse-a-device-templates-avro-file"></a>Een apparaat sjablonen AVRO-bestand parseren

```javascript
const avro = require('avsc');

// Read the AVRO file. Parse the device and template identification
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
