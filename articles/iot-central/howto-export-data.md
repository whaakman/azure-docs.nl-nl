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
ms.openlocfilehash: 5b9564dfe40f292d289ee9ed680e816771d0b0ed
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/27/2018
ms.locfileid: "39282871"
---
# <a name="export-your-data-in-azure-iot-central"></a>Uw gegevens in Azure IoT Central exporteren

Continue Export-gegevens periodiek gegevens exporteren naar uw Azure Blob Storage-account gebruiken. Kies voor het exporteren van **metingen**, **apparaten**, en **apparaatsjablonen** in bestanden van [Apache AVRO](https://avro.apache.org/docs/current/index.html) indeling. Gebruik de geëxporteerde gegevens voor analyse van koude pad, zoals modellen voor training in Azure Machine Learning of op de lange termijn trendanalyse in Power BI.

> [!Note]
> Als u continue Export van gegevens inschakelt, krijgt u alleen de gegevens die afkomstig zijn vanaf dat moment en hoger. Er is momenteel geen manier voor het ophalen van gegevens uit als continue Export van gegevens is uitgeschakeld. Schakel continue gegevensexport vroeg om meer historische gegevens te behouden.

## <a name="prerequisites"></a>Vereisten

- Een uitgebreide 30-daagse proefversie-app of een betaald app
- Azure-account met Azure-abonnement
- Hetzelfde Azure-account is een beheerder in uw app IoT Central
- De dezelfde Azure-account heeft machtigingen voor het maken van een storage-account of toegang tot een bestaand opslagaccount in hetzelfde Azure-abonnement

## <a name="types-of-data-to-export"></a>Typen gegevens exporteren

### <a name="measurements"></a>Metingen

De metingen die apparaten verzenden geëxporteerd naar uw Storage-account. Metingen gegevens geëxporteerd ongeveer eens per minuut, met alle nieuwe berichten die zijn ontvangen door IoT Central van alle apparaten in dat tijdvenster. De geëxporteerde AVRO-bestanden zich in dezelfde indeling als de bestanden die berichten zijn geëxporteerd door [IoT Hub-berichtroutering](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-csharp-csharp-process-d2c) naar blob-opslag.

> [!NOTE]
> De apparaten die de metingen verzonden worden vertegenwoordigd door de apparaat-id's (Zie hieronder). Als u de namen van de apparaten, moet u momentopnamen van de apparaten te exporteren. U kunt elk berichtenrecord met behulp van de connectionDeviceId die overeenkomt met de id van het apparaat correleren.

Dit is een voorbeeld van een record in het gedecodeerde AVRO-bestand.

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

Wanneer u eerst op de continue Export van gegevens inschakelt, wordt een momentopname van een enkel met alle apparaten wordt geëxporteerd. Dit omvat:
- Apparaat-id 's
- Apparaatnamen
- Apparaat sjabloon-id
- Waarden van eigenschappen
- Waarden voor instellingen

Ongeveer eens per minuut, is een nieuwe momentopname geschreven met:

- De nieuwe apparaten die zijn toegevoegd sinds de laatste momentopname
- Apparaten waarop eigenschappen en instellingen voor waarden die zijn gewijzigd sinds de laatste momentopname

> [!NOTE]
> Apparaten die zijn verwijderd sinds de laatste momentopname worden niet geëxporteerd. Er is geen indicator in de momentopnamen voor apparaten die op dit moment zijn verwijderd.

> [!NOTE]
> De apparaat-sjabloon die elk apparaat hoort wordt vertegenwoordigd door een sjabloon voor apparaat-ID. Als u de naam van de sjabloon van het apparaat, moet u momentopnamen van apparaat-sjabloon te exporteren.

Elke record in het gedecodeerde AVRO-bestand ziet er als volgt:

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

Wanneer u eerst op de continue Export van gegevens inschakelt, wordt een momentopname van een enkel met alle apparaatsjablonen geëxporteerd. Dit omvat: 
- Apparaat sjabloon-id
- Gegevenstypen van de meting en min/max-waarden
- Eigenschappen van gegevenstypen en standaardwaarden
- Instellingen voor gegevenstypen en standaardwaarden

Ongeveer eens per minuut, is een nieuwe momentopname geschreven met:

- Het nieuwe apparaatsjablonen die zijn toegevoegd sinds de laatste momentopname
- Apparaatsjablonen die was metingen, eigenschappen en definities van de instellingen die zijn gewijzigd sinds de laatste momentopname

> [!NOTE]
> Apparaatsjablonen die zijn verwijderd sinds de laatste momentopname worden niet geëxporteerd. Er is geen indicator in de momentopnamen voor apparaatsjablonen die op dit moment zijn verwijderd.

Elke record in het gedecodeerde AVRO-bestand ziet er als volgt:

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

## <a name="how-to-set-up-data-export"></a>Over het instellen van de gegevens exporteren

1. Als u dit niet al hebt, maakt u een Azure Storage-account **in de Azure-abonnement dat uw app in**. [Klik hier](https://ms.portal.azure.com/#create/Microsoft.StorageAccount-ARM) gaan bij de Azure portal en maak een nieuw Azure Storage-account.

    - Kies *algemeen* of *Blob storage* soorten account
    - Selecteer het abonnement dat uw app IoT Central is in. Als u het abonnement niet ziet, moet u zich aanmelden bij een andere Azure-account of vraag voor toegang tot het abonnement.
    - U kunt een bestaande resourcegroep kiezen of een nieuwe maken. Meer informatie over [over het maken van een nieuw opslagaccount.](https://aka.ms/blobdocscreatestorageaccount)

2. Maak een container in uw opslagaccount om uw IoT Central-gegevens te exporteren. Ga naar uw Storage-account door Blobs Bladeren -> en een nieuwe container maken. ![Een containerinstallatiekopie maken](media/howto-export-data/createcontainer.png)

3. Zich aanmelden bij uw IoT Central-toepassing met behulp van de dezelfde Azure-account.
1. Ga naar de beheer -> voortdurende gegevensexport.
![IoT Central CDE](media/howto-export-data/continuousdataexport.PNG)
1. Met behulp van de vervolgkeuzelijsten, kies uw opslagaccount en Container. Gebruik de Schakelknoppen vervolgens om in te schakelen in of uit de verschillende typen gegevens om te exporteren.
1. Ten slotte continue Export van gegevens met behulp van de wisselknop inschakelen en klik op 'Opslaan'.
1. Wacht een paar minuten en ziet u uw gegevens worden weergegeven in uw opslagaccount. U kunt navigeren naar uw Storage-account, selecteert u blobs bladeren, uw container en ziet u drie mappen. De standaardpaden voor de AVRO-bestanden met de verschillende typen gegevens zijn:
- Berichten: **{container}/measurements/{hubname}/{YYYY}/{MM}/{dd}/{hh}/{mm}/00.avro**
- Apparaten: **{container}/devices/{hubname}/{YYYY}/{MM}/{dd}/{hh}/{mm}/00.avro**
- Apparaatsjablonen: **{container}/deviceTemplates/{hubname}/{YYYY}/{MM}/{dd}/{hh}/{mm}/00.avro**

## <a name="how-to-read-exported-avro-files"></a>Het lezen van geëxporteerd AVRO-bestanden

AVRO is een binaire indeling, zodat de bestanden in hun onbewerkte status kunnen niet worden gelezen. Ze kunnen worden gedecodeerd naar JSON-indeling. De volgende voorbeelden laten zien hoe de metingen, apparaten en apparaatsjablonen AVRO-bestanden met behulp van de bovenstaande voorbeelden worden geparseerd.

## <a name="python"></a>Python

### <a name="install-pandas-and-the-pandaavro-package"></a>Pandas en het pakket PandaAvro installeren:

```python
pip install pandas
pip install pandavro
```
### <a name="parse-measurements-avro-file"></a>Metingen AVRO-bestand parseren

```python
import json
import pandavro as pdx
import pandas as pd

def parse(filePath):
    # Pandavro loads the avro file into a pandas DataFrame where each record is
    # a single row
    measurements = pdx.from_avro(filePath)

    # In this example, we create a new DataFrame and load a series for each
    # column we map into a column in our new DataFrame.
    transformed = pd.DataFrame()

    # The SystemProperties column contains a dictionary with the device id
    # located under the "connectionDeviceId" key.
    transformed["device_id"] = measurements["SystemProperties"].apply(lambda x: x["connectionDeviceId"])

    # The Body column is a series of utf-8 bytes that is stringified and parsed
    # as json. In this example, we pull the "humidity" property off of each one
    # to get the humidity field.
    transformed["humidity"] = measurements["Body"].apply(lambda x: json.loads(bytes(x).decode('utf-8'))["humidity"])

    # Finally, we print the new DataFrame with our device ids and humidities
    print(transformed)

```
### <a name="parse-devices-avro-file"></a>Apparaten AVRO-bestand parseren

```python
import json
import pandavro as pdx
import pandas as pd

def parse(filePath):
    # Pandavro loads the avro file into a pandas DataFrame where each record is
    # a single row
    devices = pdx.from_avro(filePath)

    # In this example, we create a new DataFrame and load a series for each
    # column we map into a column in our new DataFrame.
    transformed = pd.DataFrame()

    # The device id is available directly in the "id" column
    transformed["device_id"] = devices["id"]

    # The template id and version are present in a dictionary under the
    # deviceTemplate column
    transformed["template_id"] = devices["deviceTemplate"].apply(lambda x: x["id"])
    transformed["template_version"] = devices["deviceTemplate"].apply(lambda x: x["version"])

    # The fanSpeed setting value is located in a nested dictionary under the
    # settings column
    transformed["fan_speed"] = devices["settings"].apply(lambda x: x["device"]["fanSpeed"])

    # Finally, we print the new DataFrame with our device and template
    # information, along with the value of the fan speed
    print(transformed)

```

### <a name="parse-device-templates-avro-file"></a>Apparaat sjablonen AVRO-bestand parseren

```python
import json
import pandavro as pdx
import pandas as pd

def parse(filePath):
    # Pandavro loads the avro file into a pandas DataFrame where each record is
    # a single row
    templates = pdx.from_avro(filePath)

    # In this example, we create a new DataFrame and load a series for each
    # column we map into a column in our new DataFrame.
    transformed = pd.DataFrame()

    # The template and version are available directly in the "id" and "version"
    # columns, respectively
    transformed["template_id"] = templates["id"]
    transformed["template_version"] = templates["version"]

    # The fanSpeed setting value is located in a nested dictionary under the
    # settings column
    transformed["fan_speed"] = templates["settings"].apply(lambda x: x["device"]["fanSpeed"])

    # Finally, we print the new DataFrame with our device and template
    # information, along with the value of the fan speed
    print(transformed)
```

## <a name="c"></a>C#

### <a name="install-microsofthadoopavro"></a>Microsoft.Hadoop.Avro installeren

```csharp
Install-Package Microsoft.Hadoop.Avro -Version 1.5.6
```

### <a name="parse-measurements-avro-file"></a>Metingen AVRO-bestand parseren

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
            // For one Avro Container, it may contain multiple blocks
            // Loop through each block within the container
            while (reader.MoveNext())
            {
                // Loop through Avro record inside the block and extract the fields
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

### <a name="parse-devices-avro-file"></a>Apparaten AVRO-bestand parseren

```csharp
using Microsoft.Hadoop.Avro;
using Microsoft.Hadoop.Avro.Container;

public static async Task Run(string filePath)
{
    using (var fileStream = File.OpenRead(filePath))
    {
        using (var reader = AvroContainer.CreateGenericReader(fileStream))
        {
            // For one Avro Container, it may contains multiple blocks
            // Loop through each block within the container
            while (reader.MoveNext())
            {
                // Loop through Avro record inside the block and extract the
                // fields from it
                foreach (AvroRecord record in reader.Current.Objects)
                {
                    // Get the field value directly. You can also yield return
                    // record and make the function IEnumerable<AvroRecord>
                    var deviceId = record.GetField<string>("id");

                    // The device template information is stored in a sub-record
                    // under the "deviceTemplate" field.
                    var deviceTemplateRecord = record.GetField<AvroRecord>("deviceTemplate");
                    var templateId = deviceTemplateRecord.GetField<string>("id");
                    var templateVersion = deviceTemplateRecord.GetField<string>("version");

                    // The settings and properties are nested two levels deep,
                    // with the first level indicating settings or properties
                    // and the second indicating the kind of setting/property
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
### <a name="parse-device-templates-avro-file"></a>Apparaat sjablonen AVRO-bestand parseren

```csharp
using Microsoft.Hadoop.Avro;
using Microsoft.Hadoop.Avro.Container;

public static async Task Run(string filePath)
{
    using (var fileStream = File.OpenRead(filePath))
    {
        using (var reader = AvroContainer.CreateGenericReader(fileStream))
        {
            // For one Avro Container, it may contains multiple blocks
            // Loop through each block within the container
            while (reader.MoveNext())
            {
                // Loop through Avro record inside the block and extract the
                // fields from it
                foreach (AvroRecord record in reader.Current.Objects)
                {
                    // Get the field value directly. You can also yield return
                    // record and make the function IEnumerable<AvroRecord>
                    var id = record.GetField<string>("id");
                    var version = record.GetField<string>("version");

                    // The settings and properties are nested two levels deep,
                    // with the first level indicating settings or properties
                    // and the second indicating the kind of setting/property
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

## <a name="javascript"></a>Javascript

### <a name="install-avsc"></a>Avsc installeren

```javascript
npm install avsc
```

### <a name="parse-measurements-avro-file"></a>Metingen AVRO-bestand parseren

```javascript
const avro = require('avsc');

// Read the avro file and parse the device id and humidity from each record
async function parse(filePath) {
    const records = await load(filePath);
    for (const record of records) {
        // Fetch the device id from the system properties
        const deviceId = record.SystemProperties.connectionDeviceId;

        // Convert the Body from a Buffer to a string and parse it
        const body = JSON.parse(record.Body.toString());

        // Get the humidty property off of the body
        const humidity = body.humidity;

        // Log the device id and humidity we found
        console.log(`Device ID: ${deviceId}`);
        console.log(`Humidity: ${humidity}`);
    }
}

function load(filePath) {
    return new Promise((resolve, reject) => {
        // The file decoder emits each record as a data event on a stream. We
        // collect them up into an array and return them when we get to the end.
        const records = [];
        avro.createFileDecoder(filePath)
            .on('data', record => { records.push(record); })
            .on('end', () => resolve(records))
            .on('error', reject);
    });
}
```

### <a name="parse-devices-avro-file"></a>Apparaten AVRO-bestand parseren

```javascript
const avro = require('avsc');

// Read the avro file and parse the device and template identification info as
// well as the fanSpeed setting for each device record.
async function parse(filePath) {
    const records = await load(filePath);
    for (const record of records) {
        // Fetch the device id from the id property
        const deviceId = record.id;

        // Fetch the tempalte id and version from the deviceTemplate property
        const deviceTemplateId = record.deviceTemplate.id;
        const deviceTemplateVersion = record.deviceTemplate.version;

        // Get the fanSpeed off the nested device settings property
        const fanSpeed = record.settings.device.fanSpeed;

        // Log the device id and humidity we found
        console.log(`ID: ${deviceId}, Template ID: ${deviceTemplateId}, Template Version: ${deviceTemplateVersion}, Fan Speed: ${fanSpeed}`);
    }
}

function load(filePath) {
    return new Promise((resolve, reject) => {
        // The file decoder emits each record as a data event on a stream. We
        // collect them up into an array and return them when we get to the end.
        const records = [];
        avro.createFileDecoder(filePath)
            .on('data', record => { records.push(record); })
            .on('end', () => resolve(records))
            .on('error', reject);
    });
}
```

### <a name="parse-device-templates-avro-file"></a>Apparaat sjablonen AVRO-bestand parseren

```javascript
const avro = require('avsc');

// Read the avro file and parse the device and template identification info as
// well as the fanSpeed setting for each device record.
async function parse(filePath) {
    const records = await load(filePath);
    for (const record of records) {
        // Fetch the template id and version from the id and verison properties
        const templateId = record.id;
        const templateVersion = record.version;

        // Get the fanSpeed off the nested device settings property
        const fanSpeed = record.settings.device.fanSpeed;

        // Log the device id and humidity we found
        console.log(`Template ID: ${templateId}, Template Version: ${templateVersion}, Fan Speed: ${fanSpeed}`);
    }
}

function load(filePath) {
    return new Promise((resolve, reject) => {
        // The file decoder emits each record as a data event on a stream. We
        // collect them up into an array and return them when we get to the end.
        const records = [];
        avro.createFileDecoder(filePath)
            .on('data', record => { records.push(record); })
            .on('end', () => resolve(records))
            .on('error', reject);
    });
}
```

## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt geleerd om uw gegevens te exporteren, volgt de voorgestelde volgende stap:

> [!div class="nextstepaction"]
> [Hoe u uw gegevens in Power BI visualiseren](howto-connect-powerbi.md)