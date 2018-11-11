---
title: Gebruik van Protocol Buffers met apparaatsimulatie - Azure | Microsoft Docs
description: In deze handleiding leert u hoe u Protocol Buffers gebruiken om te serialiseren van telemetrie vanaf de oplossingsverbetering voor Apparaatsimulatie verzonden.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.custom: mvc
ms.date: 11/06/2018
ms.author: dobett
ms.openlocfilehash: 64470a1497a287f4cc2c3ef3ed29986382aeac9b
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/08/2018
ms.locfileid: "51285031"
---
# <a name="serialize-telemetry-using-protocol-buffers"></a>Telemetrie met behulp van Protocol Buffers serialiseren

Protocol Buffers (Protobuf) is een binaire serialisatie-indeling voor gestructureerde gegevens. Protobuf is ontworpen om te benadrukken eenvoud en prestaties met een doel dat kleiner en sneller dan in XML.

Apparaatsimulatie ondersteunt de **proto3** versie van het protocol buffers taal.

Omdat Protobuf vereist gecompileerde code om de gegevens te serialiseren is, moet u een aangepaste versie van Apparaatsimulatie bouwen.

De stappen in deze procedure-naar-handleiding leert u hoe aan:

1. Een ontwikkelomgeving voorbereiden
1. Met de indeling Protobuf in een Apparaatmodel opgeven
1. De indeling van uw Protobuf definiëren
1. Protobuf klassen genereren
1. Lokaal testen

## <a name="prerequisites"></a>Vereisten

Als u wilt volgen de stappen in deze handleiding, hebt u het volgende nodig:

* Visual Studio Code. U kunt downloaden [Visual Studio-Code voor Mac, Linux en Windows](https://code.visualstudio.com/download).
* .NET core. U kunt downloaden [.NET Core voor Mac, Linux en Windows](https://www.microsoft.com/net/download).
* Postman. U kunt downloaden [Postman voor Mac, windows of Linux](https://www.getpostman.com/apps).
* Een [IoT-hub die zijn geïmplementeerd in uw Azure-abonnement](../iot-hub/iot-hub-create-through-portal.md). U moet de IoT-hub-verbindingsreeks voor de stappen in deze handleiding. U kunt de verbindingsreeks ophalen vanuit de Azure-portal.
* Een [Cosmos DB-database geïmplementeerd in uw Azure-abonnement](../cosmos-db/create-sql-api-dotnet.md#create-a-database-account) die gebruikmaakt van de SQL-API en die geconfigureerd voor [sterke consistentie](../cosmos-db/manage-account.md). U moet de verbindingsreeks van de Cosmos DB-database voor de stappen in deze handleiding. U kunt de verbindingsreeks ophalen vanuit de Azure-portal.
* Een [Azure storage-account geïmplementeerd voor uw Azure-abonnement](../storage/common/storage-quickstart-create-account.md). U moet de verbindingsreeks de storage-account voor de stappen in deze handleiding. U kunt de verbindingsreeks ophalen vanuit de Azure-portal.

## <a name="prepare-your-development-environment"></a>Uw ontwikkelomgeving voorbereiden

Voer de volgende taken als u wilt uw ontwikkelomgeving voorbereiden:

* Download de bron voor het apparaat simuleren-microservice.
* Download de broncode voor de opslag-adapter-microservice.
* De opslag-adapter microservice lokaal uitvoeren.

De instructies in dit artikel wordt ervan uitgegaan dat u gebruikmaakt van Windows. Als u een ander besturingssysteem gebruikt, moet u mogelijk enkele van de bestandspaden en opdrachten op basis van uw omgeving aanpassen.

### <a name="download-the-microservices"></a>Download de microservices

Downloaden en pak deze uit de [Microservices voor externe bewaking](https://github.com/Azure/remote-monitoring-services-dotnet/archive/master.zip) vanuit GitHub naar een geschikte locatie op uw lokale computer. Deze opslagplaats bevat de opslag-adapter microservice die u nodig hebt voor deze instructies.

Downloaden en pak deze uit de [apparaat simulatie microservice](https://github.com/Azure/device-simulation-dotnet/archive/master.zip) vanuit GitHub naar een geschikte locatie op uw lokale computer.

### <a name="run-the-storage-adapter-microservice"></a>De opslag-adapter microservice uitvoeren

Open in Visual Studio Code, de **remote-monitoring-services-dotnet-master\storage-adapter** map. Klik op een **herstellen** knoppen om op te lossen van niet-omgezette afhankelijkheden.

Open de **.vscode/launch.json** bestands- en toewijzen van uw Cosmos DB-verbindingsreeks voor de **pc's\_STORAGEADAPTER\_DOCUMENTDB\_CONNSTRING** omgeving variabele.

> [!NOTE]
> Wanneer u de microservices lokaal op uw computer uitvoert, vereist deze nog steeds de instantie van een Cosmos DB in Azure goed te laten werken.

De opslag-adapter microservice als lokaal wilt uitvoeren, klikt u op **Debug \> Start Debugging**.

De **Terminal** venster in Visual Studio Code ziet u uitvoer van de actieve microservice, met inbegrip van een URL voor de web service-statuscontrole: <http://127.0.0.1:9022/v1/status>. Wanneer u naar dit adres navigeert, de status moet ' OK: Alive en goed '.

Laat u de opslag-adapter microservice in dit exemplaar van Visual Studio Code wordt uitgevoerd terwijl u de volgende stappen uitvoeren.

## <a name="define-your-device-model"></a>Uw Apparaatmodel definiëren

Open de **apparaat-simulatie-dotnet-master** map die u hebt gedownload van GitHub in een nieuw exemplaar van Visual Studio Code. Klik op een **herstellen** knoppen om op te lossen dat alle afhankelijkheden niet opgelost.

In deze procedure-naar-handleiding, moet u een nieuw Apparaatmodel voor het vastleggen van een asset maken:

1. Maak een nieuw apparaat model-bestand met de naam **assettracker 01.json** in de **Services\data\devicemodels** map.

1. De functionaliteit voor apparaten definiëren in het Apparaatmodel **assettracker 01.json** bestand. De sectie telemetrie van een model van het apparaat Protobuf moet:

    * De naam van de Protobuf klasse die u voor het apparaat genereren bevatten. De volgende sectie leest u hoe voor het genereren van deze klasse.
    * Protobuf opgeven als de berichtindeling.

    ```json
    {
      "SchemaVersion": "1.0.0",
      "Id": "assettracker-01",
      "Version": "0.0.1",
      "Name": "Asset Tracker",
      "Description": "An asset tracker with location, temperature, and humidity",
      "Protocol": "AMQP",
      "Simulation": {
        "InitialState": {
          "online": true,
          "latitude": 47.445301,
          "longitude": -122.296307,
          "temperature": 38.0,
          "humidity": 62.0
        },
        "Interval": "00:01:00",
        "Scripts": [
          {
            "Type": "javascript",
            "Path": "assettracker-01-state.js"
          }
        ]
      },
      "Properties": {
        "Type": "AssetTracker",
        "Location": "Field",
        "Latitude": 47.445301,
        "Longitude": -122.296307
      },
      "Telemetry": [
        {
          "Interval": "00:00:10",
          "MessageTemplate": "{\"latitude\":${latitude},\"longitude\":${longitude},\"temperature\":${temperature},\"humidity\":${humidity}}",
          "MessageSchema": {
            "Name": "assettracker-sensors;v1",
            "ClassName": "Microsoft.Azure.IoTSolutions.DeviceSimulation.Services.Models.Protobuf.AssetTracker",
            "Format": "Protobuf",
            "Fields": {
              "latitude": "double",
              "longitude": "double",
              "temperature": "double",
              "humidity": "double"
            }
          }
        }
      ]
    }
    ```

### <a name="create-device-behaviors-script"></a>Apparaat gedrag script maken

Het gedrag van script schrijven dat definieert het gedrag van uw apparaat. Zie voor meer informatie, [maken van een geavanceerde gesimuleerd apparaat](iot-accelerators-device-simulation-advanced-device.md).

## <a name="define-your-protobuf-format"></a>De indeling van uw Protobuf definiëren

Wanneer u een Apparaatmodel en de berichtindeling van uw hebt vastgesteld, kunt u een **protoco** bestand. In de **protoco** -bestand, u toevoegen:

* Een `csharp_namespace` die overeenkomt met de **ClassName** eigenschap in het Apparaatmodel van uw.
* Een bericht voor elke gegevensstructuur om te serialiseren.
* Een naam en type voor elk veld in het bericht.

1. Maak een nieuw bestand met de naam **assettracker.proto** in de **Services\Models\Protobuf\proto** map.

1. De syntaxis, naamruimte en in het schema definiëren de **protoco** bestand als volgt:

    ```proto
    syntax = "proto3";

    option csharp_namespace = "Microsoft.Azure.IoTSolutions.DeviceSimulation.Services.Models.Protobuf";

    message AssetTracker {
      double latitude=1;
      double longitude=2;
      double temperature=5;
      double humidity=6;
    }
    ```

De `=1`, `=2` markeringen in voor elk element in een unieke code maakt gebruik van het veld in de binaire codering opgeven. 1-15 cijfers vereisen een minder byte coderen dan hogere nummers.

## <a name="generate-the-protobuf-class"></a>De klasse Protobuf genereren

Wanneer u hebt een **protoco** -bestand de volgende stap is het genereren van de klassen die nodig zijn voor lezen en schrijven van berichten. Als u wilt deze stap hebt voltooid, moet u de **Protoc** Protobuf compiler.

1. [De compiler Protobuf downloaden vanuit GitHub](https://github.com/protocolbuffers/protobuf/releases/download/v3.4.0/protoc-3.4.0-win32.zip)

1. Voer de compiler opgeven van de bronmap, de doelmap en de naam van uw **protoco** bestand. Bijvoorbeeld:

    ```cmd
    protoc -I c:\temp\device-simulation-dotnet-master\Services\Models\Protobuf\proto --csharp_out=C:\temp\device-simulation-dotnet-master\Services\Models\Protobuf assettracker.proto
    ```

    Met deze opdracht genereert een **Assettracker.cs** -bestand in de **Services\Models\Protobuf** map.

## <a name="test-protobuf-locally"></a>Test Protobuf lokaal

In deze sectie maakt testen u het asset tracker-apparaat die u hebt gemaakt in de vorige secties lokaal.

### <a name="run-the-device-simulation-microservice"></a>De simulatie apparaat microservice uitvoeren

Open de **.vscode/launch.json** bestands- en toewijzen uw:

* IoT Hub-verbindingsreeks voor de **pc's\_IOTHUB\_CONNSTRING** omgevingsvariabele.
* Tekenreeks opslagaccountverbinding voor de **pc's\_AZURE\_opslag\_ACCOUNT** omgevingsvariabele.
* Cosmos DB-verbindingsreeks voor de **pc's\_STORAGEADAPTER\_DOCUMENTDB\_CONNSTRING** omgevingsvariabele.

Open de **WebService/Properties/launchSettings.json** bestands- en toewijzen uw:

* IoT Hub-verbindingsreeks voor de **pc's\_IOTHUB\_CONNSTRING** omgevingsvariabele.
* Tekenreeks opslagaccountverbinding voor de **pc's\_AZURE\_opslag\_ACCOUNT** omgevingsvariabele.
* Cosmos DB-verbindingsreeks voor de **pc's\_STORAGEADAPTER\_DOCUMENTDB\_CONNSTRING** omgevingsvariabele.

Open de **WebService\appsettings.ini** -bestand en wijzig de instellingen als volgt te werk:

#### <a name="configure-the-solution-to-include-your-new-device-model-files"></a>De oplossing configureren voor het opnemen van uw nieuwe apparaat model-bestanden

Standaard wordt niet uw nieuwe Apparaatmodel JSON en JS-bestanden worden gekopieerd naar de gemaakte oplossing. U moet ze expliciet opneemt.

Voeg een vermelding aan de **services\services.csproj** -bestand voor elk bestand dat u opnemen wilt. Bijvoorbeeld:

```xml
<None Update="data\devicemodels\assettracker-01.json">
    <CopyToOutputDirectory>Always</CopyToOutputDirectory>
</None>
<None Update="data\devicemodels\scripts\assettracker-01-state.js">
    <CopyToOutputDirectory>Always</CopyToOutputDirectory>
</None>
```

Als u wilt de microservices lokaal uitvoeren, klikt u op **Debug \> Start Debugging**.

De **Terminal** venster in Visual Studio Code ziet u uitvoer van de actieve microservice.

Laat u het apparaat simulatie microservice in dit exemplaar van Visual Studio Code wordt uitgevoerd terwijl u de volgende stappen uitvoeren.

### <a name="set-up-a-monitor-for-device-events"></a>Instellen van een monitor voor apparaatgebeurtenissen

In deze sectie maakt u de Azure CLI gebruiken voor het instellen van een van de monitor de telemetrie die is verzonden vanaf de apparaten die zijn verbonden met uw IoT-hub bekijken.

Het volgende script wordt ervan uitgegaan dat de naam van uw IoT-hub **apparaat-simulatie-test**.

```azurecli-interactive
# Install the IoT extension if it's not already installed
az extension add --name azure-cli-iot-ext

# Monitor telemetry sent to your hub
az iot hub monitor-events --hub-name device-simulation-test
```

Laat het controleprogramma voor gebeurtenissen uitgevoerd terwijl u de gesimuleerde apparaten testen.

### <a name="create-a-simulation-with-the-asset-tracker-device-type"></a>Maak een simulatie met het apparaattype voor tracering van asset

In deze sectie kunt u het hulpprogramma Postman gebruiken om aan te vragen van het apparaat simulatie microservice om uit te voeren een simulatie met behulp van het apparaattype activa bijhouden. Postman is een hulpprogramma waarmee u de REST-aanvragen verzenden naar een webservice.

Postman instellen:

1. Postman op uw lokale computer openen.

1. Klik op **bestand \> importeren**. Klik vervolgens op **bestanden kiezen**.

1. Selecteer **Apparaatsimulatie voor Azure IoT-oplossing accelerator.postman\_verzameling** en **Apparaatsimulatie voor Azure IoT-oplossing accelerator.postman\_omgeving** en Klik op **Open**.

1. Vouw de **Apparaatsimulatie voor Azure IoT-oplossingsversnellers** om weer te geven van de aanvragen die u kunt verzenden.

1. Klik op **geen omgeving** en selecteer **Apparaatsimulatie voor Azure IoT-oplossingsversnellers**.

U hebt nu een verzameling en geladen in de Postman-werkruimte die u gebruiken kunt om te communiceren met de apparaat-simulatie microservice-omgeving.

Configureren en de simulatie uitvoeren:

1. Selecteer in de Postman-verzameling **asset tracker simulatie** en klikt u op **verzenden**. Deze aanvraag maakt vier instanties van het apparaattype voor tracering van gesimuleerde asset.

1. De uitvoer van de monitor gebeurtenis in de Azure CLI-venster ziet u de telemetrie van de gesimuleerde apparaten.

Als u wilt de simulatie stoppen, selecteert u de **simulatie stoppen** aanvraag in Postman en klik op **verzenden**.

### <a name="clean-up-resources"></a>Resources opschonen

U kunt de twee lokaal uitgevoerde microservices in hun Visual Studio Code-exemplaren stoppen (**fouten opsporen in \> Stop Debugging**).

Als u de IoT Hub en Cosmos DB-instanties niet meer nodig hebt, verwijdert u deze uit uw Azure-abonnement om alle onnodige kosten te voorkomen.

## <a name="iot-hub-support"></a>Ondersteuning voor IoT-Hub

Groot aantal IoT Hub-functies ondersteunen geen systeemeigen Protobuf of andere binaire indeling. U kunt niet bijvoorbeeld routeren op basis van de berichtnettolading van het omdat IoT Hub kan niet worden verwerkt de berichtnettolading van het. U kunt echter routeren op basis van de berichtkoppen.

## <a name="next-steps"></a>Volgende stappen

Nu u hebt geleerd om aan te passen Apparaatsimulatie voor het gebruik van Protobuf om telemetrie te verzenden, de volgende stap is te leren nu naar [een aangepaste installatiekopie implementeren in de cloud](iot-accelerators-device-simulation-deploy-image.md).
