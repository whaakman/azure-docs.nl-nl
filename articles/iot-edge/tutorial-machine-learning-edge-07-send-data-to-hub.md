---
title: Verzenden van apparaatgegevens via een transparante gateway - Machine Learning in Azure IoT Edge | Microsoft Docs
description: Gebruik uw ontwikkelcomputer als een gesimuleerde IoT Edge-apparaat om gegevens te verzenden naar de IoT Hub het apparaat door zich via een apparaat dat is geconfigureerd als een transparante gateway.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/13/2019
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 12793ff28bf13f26bc2cc3d436b644601fc48ac8
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67081383"
---
# <a name="tutorial-send-data-via-transparent-gateway"></a>Zelfstudie: Verzenden van gegevens via een transparante gateway

> [!NOTE]
> In dit artikel maakt deel uit van een reeks voor een zelfstudie over het gebruik van Azure Machine Learning op IoT Edge. Als u rechtstreeks in dit artikel zijn ontvangen, u wordt aangeraden te beginnen met de [eerste artikel](tutorial-machine-learning-edge-01-intro.md) in de reeks voor de beste resultaten.

In dit artikel gebruiken we nog een keer de ontwikkelcomputer als een gesimuleerd apparaat, maar gegevens rechtstreeks naar de IoT Hub verzenden in plaats van het apparaat gegevens verzendt naar de IoT Edge-apparaat dat is geconfigureerd als een transparante gateway.

We controleren de werking van de IoT Edge-apparaat terwijl het gesimuleerde apparaat gegevens verzendt. Als het apparaat is voltooid wordt uitgevoerd, we kijken naar de gegevens in onze storage-account valideren alles naar verwachting gewerkt.

Deze stap wordt doorgaans uitgevoerd door een ontwikkelaar van de cloud of het apparaat.

## <a name="review-device-harness"></a>Gebruikmaken van het apparaat controleren

Opnieuw gebruiken de [DeviceHarness project](tutorial-machine-learning-edge-03-generate-data.md) apparaat simuleren de downstream (of bladeren). Verbinding maken met een transparante gateway vereist twee extra dingen:

* Registreren van het certificaat om de downstream-apparaat maken (in dit geval de ontwikkelcomputer) vertrouwt de certificeringsinstantie die wordt gebruikt door de IoT Edge-runtime.
* De edge-gateway volledig gekwalificeerde domeinnaam (FQDN) toevoegen aan de apparaat-verbindingsreeks.

Bekijk de code om te zien hoe deze twee items worden geïmplementeerd.

1. Open Visual Studio Code op uw ontwikkelcomputer.

2. Gebruik **bestand** > **map openen...**  openen C:\\bron\\IoTEdgeAndMlSample\\DeviceHarness.

3. Bekijk de methode InstallCertificate() in Program.cs.

4. Houd er rekening mee dat als de code vindt het pad naar het certificaat, de CertificateManager.InstallCACert methode wordt voor het installeren van het certificaat op de machine.

5. Bekijk nu de GetIotHubDevice-methode van de klasse TurbofanDevice.

6. Wanneer de gebruiker Hiermee geeft u de FQDN-naam van het gebruik van de gateway de '-g "optie, wordt die waarde wordt doorgegeven aan deze methode als gatewayFqdn, die wordt toegevoegd aan de apparaat-verbindingsreeks.

   ```csharp
   connectionString = $"{connectionString};GatewayHostName={gatewayFqdn.ToLower()}";
   ```

## <a name="build-and-run-leaf-device"></a>Bouwen en uitvoeren van de leaf-apparaten

1. Bouw het project met het project DeviceHarness nog steeds openen in Visual Studio Code (Ctrl + Shift + B of **Terminal** > **bouwen taak uitvoeren...** ) en selecteer **bouwen** in het dialoogvenster.

2. De volledig gekwalificeerde domeinnaam (FQDN) voor uw edge-gateway vinden door te gaan met uw IoT Edge-apparaat virtuele machine in de portal en de waarde voor kopiëren **DNS-naam** uit het overzicht.

3. Open de terminal Visual Studio Code (**Terminal** > **nieuwe terminal**) en voer de volgende opdracht, vervangt `<edge_device_fqdn>` door de DNS-naam die u hebt gekopieerd uit de virtuele machine:

   ```cmd
   dotnet run -- --gateway-host-name "<edge_device_fqdn>" --certificate C:\edgecertificates\certs\azure-iot-test-only.root.ca.cert.pem --max-devices 1
   ```

4. De toepassing probeert te installeren van het certificaat op uw ontwikkelcomputer. Wanneer het geval is, accepteert u de beveiligingswaarschuwing.

5. Wanneer u hierom wordt gevraagd voor het IoT Hub connection string klikt u op het weglatingsteken ( **...** ) op de Azure IoT Hub apparaten van het deelvenster en selecteert u **kopie IoT Hub-verbindingsreeks**. Plak de waarde in de terminal.

6. Hier ziet u uitvoer zoals:

   ```output
   Found existing device: Client_001
   Using device connection string: HostName=<your hub>.azure-devices.net;DeviceId=Client_001;SharedAccessKey=xxxxxxx; GatewayHostName=iotedge-xxxxxx.<region>.cloudapp.azure.com
   Device: 1 Message count: 50
   Device: 1 Message count: 100
   Device: 1 Message count: 150
   Device: 1 Message count: 200
   Device: 1 Message count: 250
   ```

   Houd er rekening mee de toevoeging van 'GatewayHostName' aan de apparaatverbindingsreeks, die ervoor zorgt dat het apparaat om te communiceren via de IoT Hub via de IoT Edge transparante gateway.

## <a name="check-output"></a>Controle-uitvoer

### <a name="iot-edge-device-output"></a>Uitvoer van IoT Edge-apparaat

De uitvoer van de module avroFileWriter kan gemakkelijk worden waargenomen door te kijken naar de IoT Edge-apparaat.

1. Voeg SSH toe aan uw IoT Edge-virtuele machine.

2. Zoek naar bestanden die worden weggeschreven naar de schijf.

   ```bash
   find /data/avrofiles -type f
   ```

3. Uitvoer van de opdracht ziet er als in het volgende voorbeeld:

   ```output
   /data/avrofiles/2019/4/18/22/10.avro
   ```

   U mogelijk meer dan één bestand, afhankelijk van de timing van de uitvoering.

4. Let op de tijdstempels. De module avroFileWriter worden de bestanden geüpload naar de cloud wanneer de tijd van laatste wijziging meer dan 10 minuten in het verleden is (Zie gewijzigd\_bestand\_time-out in uploader.py in de module avroFileWriter).

5. Zodra de 10 minuten zijn verstreken, moet de module de bestanden te uploaden. Als de upload voltooid is, worden de bestanden van de schijf verwijderd.

### <a name="azure-storage"></a>Azure Storage

We kunnen de resultaten van onze leaf-apparaten verzenden van gegevens door te kijken de opslagaccounts waar we verwachten dat gegevens worden gerouteerd observeren.

1. Open Visual Studio Code op de ontwikkelcomputer.

2. Navigeer in het deelvenster 'AZURE-opslag' in het venster verkennen, de structuur voor het vinden van uw storage-account.

3. Vouw de **Blobcontainers** knooppunt.

4. Van het werkprofiel op het vorige gedeelte van de zelfstudie, we erop rekenen dat de **ruldata** container berichten met de resterende Levensduur moet bevatten. Vouw de **ruldata** knooppunt.

5. Ziet u een of meer blob-bestanden met de naam, zoals: `<IoT Hub Name>/<partition>/<year>/<month>/<day>/<hour>/<minute>`.

6. Klik met de rechtermuisknop op een van de bestanden en kies **Blob downloaden** om op te slaan van het bestand naar uw ontwikkelmachine.

7. Vouw vervolgens de **uploadturbofanfiles** knooppunt. In het vorige artikel stellen we deze locatie als het doel voor bestanden die zijn geüpload door de module avroFileWriter.

8. Klik met de rechtermuisknop op de bestanden en kies **Blob downloaden** wilt opslaan op uw ontwikkelcomputer.

### <a name="read-avro-file-contents"></a>Inhoud van een bestand lezen Avro

We een eenvoudig opdrachtregelprogramma voor het lezen van een Avro-bestand en retourneert een JSON-tekenreeks van de berichten in het bestand opgenomen. In deze sectie wordt we installeren en uitvoeren.

1. Open een terminal in Visual Studio Code (**Terminal** > **nieuwe terminal**).

2. Hubavroreader installeren:

   ```cmd
   pip install c:\source\IoTEdgeAndMlSample\HubAvroReader
   ```

3. Hubavroreader gebruiken om te lezen van het Avro-bestand dat u hebt gedownload van **ruldata**.

   ```cmd
   hubavroreader <avro file with ath> | more
   ```

4. Houd er rekening mee dat de hoofdtekst van het bericht ziet eruit als we naar verwachting in apparaat-ID en voorspeld resterende Levensduur.

   ```json
   {
       "Body": {
           "ConnectionDeviceId": "Client_001",
           "CorrelationId": "3d0bc256-b996-455c-8930-99d89d351987",
           "CycleTime": 1.0,
           "PredictedRul": 170.1723693909444
       },
       "EnqueuedTimeUtc": "<time>",
       "Properties": {
           "ConnectionDeviceId": "Client_001",
           "CorrelationId": "3d0bc256-b996-455c-8930-99d89d351987",
           "CreationTimeUtc": "01/01/0001 00:00:00",
           "EnqueuedTimeUtc": "01/01/0001 00:00:00"
       },
       "SystemProperties": {
           "connectionAuthMethod": "{\"scope\":\"module\",\"type\":\"sas\",\"issuer\":\"iothub\",\"acceptingIpFilterRule\":null}",
           "connectionDeviceGenerationId": "636857841798304970",
           "connectionDeviceId": "aaTurbofanEdgeDevice",
           "connectionModuleId": "turbofanRouter",
           "contentEncoding": "utf-8",
           "contentType": "application/json",
           "correlationId": "3d0bc256-b996-455c-8930-99d89d351987",
           "enqueuedTime": "<time>",
           "iotHubName": "mledgeiotwalkthroughhub"
       }
   }
   ```

5. Voer de dezelfde opdracht uit te geven van het Avro-bestand dat u hebt gedownload van **uploadturbofanfiles**.

6. Zoals verwacht, wordt deze berichten bevatten alle gegevens en operationele instellingen van het oorspronkelijke bericht. Deze gegevens kan worden gebruikt voor het verbeteren van het RUL-model op de edge-apparaat.

   ```json
   {
       "Body": {
           "CycleTime": 1.0,
           "OperationalSetting1": -0.0005000000237487257,
           "OperationalSetting2": 0.00039999998989515007,
           "OperationalSetting3": 100.0,
           "PredictedRul": 170.17236328125,
           "Sensor1": 518.6699829101562,
           "Sensor10": 1.2999999523162842,
           "Sensor11": 47.29999923706055,
           "Sensor12": 522.3099975585938,
           "Sensor13": 2388.010009765625,
           "Sensor14": 8145.31982421875,
           "Sensor15": 8.424599647521973,
           "Sensor16": 0.029999999329447746,
           "Sensor17": 391.0,
           "Sensor18": 2388.0,
           "Sensor19": 100.0,
           "Sensor2": 642.3599853515625,
           "Sensor20": 39.11000061035156,
           "Sensor21": 23.353700637817383,
           "Sensor3": 1583.22998046875,
           "Sensor4": 1396.8399658203125,
           "Sensor5": 14.619999885559082,
           "Sensor6": 21.610000610351562,
           "Sensor7": 553.969970703125,
           "Sensor8": 2387.9599609375,
           "Sensor9": 9062.169921875
       },
           "ConnectionDeviceId": "Client_001",
           "CorrelationId": "70df0c98-0958-4c8f-a422-77c2a599594f",
           "CreationTimeUtc": "0001-01-01T00:00:00+00:00",
           "EnqueuedTimeUtc": “<time>”
   }
   ```

## <a name="clean-up-resources"></a>Resources opschonen

Als u van plan bent om te verkennen van de resources die door deze end-to-end zelfstudie wordt gebruikt, wacht u totdat u klaar bent voor het opschonen van de resources die u hebt gemaakt. Als u niet van plan bent om door te gaan, gebruikt u de volgende stappen uit om ze te verwijderen:

1. Verwijderen van de resourcegroepen die zijn gemaakt voor het opslaan van de Dev VM, IoT Edge-VM, IoT-Hub, storage-account, machine learning-werkruimte-service (en -resources gemaakt: containerregister, application insights, sleutelkluis, storage-account).

2. Verwijderen van de machine learning-project in [Azure notitieblokken](https://notebooks.azure.com).

3. Als u hebt gekloond de opslagplaats lokaal, sluit alle vensters van PowerShell of in VS Code verwijzen naar de lokale opslagplaats, klikt u vervolgens de map te verwijderen.

4. Als u certificaten lokaal hebt gemaakt, verwijdert u de map c:\\edgeCertificates.

## <a name="next-steps"></a>Volgende stappen

In dit artikel, we onze ontwikkelcomputer gebruikt voor het simuleren van een leaf apparaat verzenden sensor- en operationele gegevens naar het edge-apparaat. We gevalideerd dat de modules op het apparaat doorgestuurd, geclassificeerd persistent gemaakt en de gegevens eerst geüpload door de real-time bewerking van het edge-apparaat controleren en vervolgens door te kijken naar de bestanden die zijn geüpload naar het opslagaccount.

Meer informatie vindt u op de volgende pagina's:

* [Een downstream-apparaat verbinden met Azure IoT Edge-gateway](how-to-connect-downstream-device.md)
* [Gegevens aan de rand met Azure Blob Storage Store op IoT Edge (preview)](how-to-store-data-blob.md)
