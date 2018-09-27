---
title: Aanpassen van vooraf geconfigureerde oplossingen | Microsoft Docs
description: Biedt richtlijnen voor het aanpassen van de vooraf geconfigureerde Azure IoT Suite-oplossingen.
services: ''
suite: iot-suite
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: ''
ms.assetid: 4653ae53-4110-4a10-bd6c-7dc034c293a8
ms.service: iot-suite
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2017
ms.author: corywink
ms.openlocfilehash: cb5955111cb3954f71f11602042b5153ccee3473
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47106847"
---
# <a name="customize-a-preconfigured-solution"></a>Een vooraf geconfigureerde oplossing aanpassen

De vooraf geconfigureerde oplossingen die worden geleverd met de Azure IoT Suite illustratie van de services binnen de suite werken samen om een end-to-end-oplossing leveren. Vanaf dit punt vanaf zijn er verschillende plaatsen in die u kunt uitbreiden en aanpassen van de oplossing voor specifieke scenario's. De volgende secties beschrijven deze veelvoorkomende aanpassing punten.

## <a name="find-the-source-code"></a>De broncode zoeken

De broncode voor de vooraf geconfigureerde oplossingen is beschikbaar op GitHub in de volgende opslagplaatsen:

* Externe controle: [https://www.github.com/Azure/azure-iot-remote-monitoring](https://github.com/Azure/azure-iot-remote-monitoring)
* Voorspeld onderhoud: [https://github.com/Azure/azure-iot-predictive-maintenance](https://github.com/Azure/azure-iot-predictive-maintenance)
* Verbonden factory's: [https://github.com/Azure/azure-iot-connected-factory](https://github.com/Azure/azure-iot-connected-factory)

De broncode voor de vooraf geconfigureerde oplossingen wordt geleverd ter illustratie van de patronen en procedures die worden gebruikt voor het implementeren van de functionaliteit van de end-to-end van een IoT-oplossing met behulp van Azure IoT Suite. U vindt meer informatie over het bouwen en implementeren van de oplossingen in de GitHub-opslagplaatsen.

## <a name="change-the-preconfigured-rules"></a>De vooraf geconfigureerde regels wijzigen

De oplossing voor externe bewaking bevat drie [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) taken voor het afhandelen van apparaatgegevens, Telemetrie en voor regellogica in de oplossing.

De drie stream analytics-taken en hun syntaxis worden beschreven in de diepte in de [externe controle vooraf geconfigureerde oplossing voor](iot-suite-v1-remote-monitoring-sample-walkthrough.md). 

U kunt deze taken rechtstreeks als u wilt wijzigen van de logica bewerken, of specifieke logica toevoegen aan uw scenario. U vindt de Stream Analytics-taken als volgt:

1. Ga naar [Azure-portal](https://portal.azure.com).
2. Navigeer naar de resourcegroep met dezelfde naam als uw IoT-oplossing. 
3. Selecteer de Azure Stream Analytics-taak die u wilt wijzigen. 
4. De taak stoppen door te selecteren **stoppen** in de reeks opdrachten. 
5. Bewerk de invoer-, query- en uitvoer.
   
    Een eenvoudige wijziging is het wijzigen van de query voor de **regels** taak voor het gebruik van een **' < '** in plaats van een **' > '**. Nog steeds weergegeven in de oplossingsportal **' > '** wanneer u een regel bewerken, maar u ziet hoe het gedrag vanwege de wijziging in de onderliggende taak wordt gespiegeld.
6. Taak starten

> [!NOTE]
> Het dashboard externe controle, is afhankelijk van wat u specifieke gegevens, zodat het wijzigen van de taken leiden het dashboard tot kan mislukken.

## <a name="add-your-own-rules"></a>Uw eigen regels toevoegen

Naast het wijzigen van de vooraf geconfigureerde Azure Stream Analytics-taken, kunt u de Azure-portal voor nieuwe taken toevoegen of nieuwe query's toevoegen aan bestaande taken.

## <a name="customize-devices"></a>Apparaten aanpassen

Een van de meest voorkomende activiteiten van de extensie werkt met apparaten die specifiek is voor uw scenario. Er zijn verschillende methoden voor het werken met apparaten. Deze methoden omvatten het wijzigen van een gesimuleerd apparaat zodat deze overeenkomt met uw scenario of met behulp van de [IoT Device SDK] [ IoT Device SDK] uw fysieke apparaat verbinden met de oplossing.

Zie voor een stapsgewijze handleiding voor het toevoegen van apparaten, de [apparaten verbinding te maken van Iot Suite](iot-suite-v1-connecting-devices.md) artikel en de [externe controle C SDK voorbeeld](https://github.com/Azure/azure-iot-sdk-c/tree/master/serializer/samples/remote_monitoring). In dit voorbeeld is ontworpen voor gebruik met de vooraf geconfigureerde oplossing voor externe controle.

### <a name="create-your-own-simulated-device"></a>Uw eigen gesimuleerd apparaat maken

Opgenomen in de [voor externe controle broncode oplossing](https://github.com/Azure/azure-iot-remote-monitoring), is een .NET-simulator. Deze simulator is ingericht als onderdeel van de oplossing en u kunt wijzigen en verschillende metagegevens, telemetrie, verzenden en reageren op verschillende opdrachten en methoden.

De vooraf geconfigureerde simulator in de vooraf geconfigureerde oplossing voor externe controle een minder dynamische apparaat simuleert dat temperatuur en vochtigheid telemetrie verzendt. U kunt de simulator in de [Simulator.WebJob](https://github.com/Azure/azure-iot-remote-monitoring/tree/master/Simulator/Simulator.WebJob) project wanneer u de GitHub-opslagplaats hebt gesplitst.

### <a name="available-locations-for-simulated-devices"></a>Beschikbare locaties voor de gesimuleerde apparaten

De standaardset van locaties is in Seattle/Redmond, Washington, Verenigde Staten van Amerika. U kunt deze locaties in wijzigen [SampleDeviceFactory.cs][lnk-sample-device-factory].

### <a name="add-a-desired-property-update-handler-to-the-simulator"></a>De updatehandler van een gewenste eigenschap toevoegen aan de simulator

U kunt een waarde voor een gewenste eigenschap voor een apparaat in de oplossingsportal instellen. Het is de verantwoordelijkheid van het apparaat voor het afhandelen van de eigenschap wijzigingsaanvraag wanneer het apparaat de waarde van de gewenste eigenschap ophaalt. Ondersteuning voor een wijziging van de eigenschap waarde via een gewenste eigenschap toevoegen, moet u een handler toevoegen aan de simulator.

De simulator bevat handlers voor de **SetPointTemp** en **TelemetryInterval** eigenschappen die u door in te stellen bijwerken kunt de gewenste waarden in de portal van de oplossing.

Het volgende voorbeeld ziet u de handler voor de **SetPointTemp** gewenste eigenschap in de **CoolerDevice** klasse:

```csharp
protected async Task OnSetPointTempUpdate(object value)
{
    var telemetry = _telemetryController as ITelemetryWithSetPointTemperature;
    telemetry.SetPointTemperature = Convert.ToDouble(value);

    await SetReportedPropertyAsync(SetPointTempPropertyName, telemetry.SetPointTemperature);
}
```

Deze methode werkt de temperatuur van telemetrie punt en vervolgens rapporten de wijziging terug naar IoT Hub door een gerapporteerde eigenschap.

U kunt uw eigen handlers voor uw eigen eigenschappen toevoegen aan de hand van het patroon in het voorgaande voorbeeld.

U moet ook de gewenste eigenschap binden aan de handler zoals wordt weergegeven in het volgende voorbeeld van de **CoolerDevice** constructor:

```csharp
_desiredPropertyUpdateHandlers.Add(SetPointTempPropertyName, OnSetPointTempUpdate);
```

Houd er rekening mee dat **SetPointTempPropertyName** een constante is gedefinieerd als 'Config.SetPointTemp'.

### <a name="add-support-for-a-new-method-to-the-simulator"></a>Ondersteuning voor een nieuwe methode toevoegen aan de simulator

U kunt de simulator als u wilt toevoegen van ondersteuning voor een nieuwe [methode (directe methode)][lnk-direct-methods]. Er zijn twee belangrijke stappen vereist:

- De simulator moet op de hoogte stellen de IoT hub in de vooraf geconfigureerde oplossing met details van de methode.
- De simulator moet bevatten code voor het afhandelen van de aanroep van methode wanneer u vanuit aanroepen de **Apparaatdetails** deelvenster in de solution explorer of via een taak.

De externe controle vooraf geconfigureerde oplossing maakt gebruik van *gerapporteerde eigenschappen* details van de ondersteunde methoden naar IoT hub te verzenden. De back-end van de oplossing houdt een lijst van alle methoden die worden ondersteund door elk apparaat, samen met een geschiedenis van methodeaanroepen. U kunt deze informatie over apparaten weergeven en methoden aanroepen in de portal van de oplossing.

De IoT-hub melden dat een apparaat biedt ondersteuning voor een methode, het apparaat gegevens toe te voegen van de methode de **SupportedMethods** knooppunt in de gerapporteerde eigenschappen:

```json
"SupportedMethods": {
  "<method signature>": "<method description>",
  "<method signature>": "<method description>"
}
```

Handtekening van de methode heeft de volgende indeling: `<method name>--<parameter #0 name>-<parameter #1 type>-...-<parameter #n name>-<parameter #n type>`. Bijvoorbeeld, om op te geven de **InitiateFirmwareUpdate** methode verwacht de tekenreeksparameter van een met de naam **FwPackageURI**, gebruikt u de handtekening van de volgende methode:

```json
InitiateFirmwareUpate--FwPackageURI-string: "description of method"
```

Zie voor een lijst van ondersteunde parametertypen, de **CommandTypes** klasse in het project infrastructuur.

Als u wilt verwijderen van een methode, stelt u de methodehandtekening te `null` in de gerapporteerde eigenschappen.

> [!NOTE]
> Informatie over ondersteunde methoden in de back-end oplossing alleen worden bijgewerkt wanneer wordt ontvangen een *apparaatgegevens* bericht van het apparaat.

Het volgende codevoorbeeld van de **SampleDeviceFactory** klasse in het algemene project laat zien hoe u een methode toevoegen aan de lijst met **SupportedMethods** in de gerapporteerde eigenschappen die door het apparaat worden verzonden:

```csharp
device.Commands.Add(new Command(
    "InitiateFirmwareUpdate",
    DeliveryType.Method,
    "Updates device Firmware. Use parameter 'FwPackageUri' to specifiy the URI of the firmware file, e.g. https://iotrmassets.blob.core.windows.net/firmwares/FW20.bin",
    new[] { new Parameter("FwPackageUri", "string") }
));
```

Dit codefragment wordt de details van de **InitiateFirmwareUpdate** methode, met inbegrip van tekst die moet worden weergegeven in de portal van de oplossing en de details van de vereiste methode-parameters.

De simulator verzendt gerapporteerde eigenschappen, met inbegrip van de lijst met ondersteunde methoden voor IoT-Hub wanneer de simulator wordt gestart.

Een handler toevoegen aan de simulatorcode voor elke methode die wordt ondersteund. Ziet u de bestaande handlers in de **CoolerDevice** klasse in het project Simulator.WebJob. Het volgende voorbeeld ziet u de handler voor **InitiateFirmwareUpdate** methode:

```csharp
public async Task<MethodResponse> OnInitiateFirmwareUpdate(MethodRequest methodRequest, object userContext)
{
    if (_deviceManagementTask != null && !_deviceManagementTask.IsCompleted)
    {
        return await Task.FromResult(BuildMethodRespose(new
        {
            Message = "Device is busy"
        }, 409));
    }

    try
    {
        var operation = new FirmwareUpdate(methodRequest);
        _deviceManagementTask = operation.Run(Transport).ContinueWith(async task =>
        {
            // after firmware completed, we reset telemetry
            var telemetry = _telemetryController as ITelemetryWithTemperatureMeanValue;
            if (telemetry != null)
            {
                telemetry.TemperatureMeanValue = 34.5;
            }

            await UpdateReportedTemperatureMeanValue();
        });

        return await Task.FromResult(BuildMethodRespose(new
        {
            Message = "FirmwareUpdate accepted",
            Uri = operation.Uri
        }));
    }
    catch (Exception ex)
    {
        return await Task.FromResult(BuildMethodRespose(new
        {
            Message = ex.Message
        }, 400));
    }
}
```

Methode handler namen moeten beginnen met `On` gevolgd door de naam van de methode. De **methodRequest** parameter bevat de parameters die is doorgegeven met het aanroepen van de methode van de back-end van de oplossing. De geretourneerde waarde moet van het type **taak&lt;MethodResponse&gt;**. De **BuildMethodResponse** hulpprogramma methode helpt u bij het maken van de geretourneerde waarde.

Binnen de methode-handler, kunt u het volgende doen:

- Start een asynchrone taak.
- Ophalen van de gewenste eigenschappen van de *apparaatdubbel* in IoT Hub.
- Bijwerken van een enkele gerapporteerde eigenschap met de **SetReportedPropertyAsync** methode in de **CoolerDevice** klasse.
- Meerdere gerapporteerde eigenschappen bijwerken door het maken van een **TwinCollection** exemplaar en roepen de **Transport.UpdateReportedPropertiesAsync** methode.

Het vorige voorbeeld van de firmware-update wordt uitgevoerd de volgende stappen uit:

- Controleert of dat het apparaat kan de aanvraag van de firmware-update accepteren.
- Asynchroon initieert de firmware-update-bewerking en de telemetrie wordt opnieuw ingesteld wanneer de bewerking voltooid is.
- Retourneert onmiddellijk het bericht 'FirmwareUpdate geaccepteerd' om aan te geven van dat de aanvraag is geaccepteerd door het apparaat.

### <a name="build-and-use-your-own-physical-device"></a>Bouwen en uw eigen (fysieke)-apparaat gebruiken

De [Azure IoT SDK's](https://github.com/Azure/azure-iot-sdks) bieden bibliotheken voor het verbinden van verschillende typen apparaten (talen en besturingssystemen) in IoT-oplossingen.

## <a name="modify-dashboard-limits"></a>Dashboard limieten wijzigen

### <a name="number-of-devices-displayed-in-dashboard-dropdown"></a>Aantal apparaten weergegeven in de vervolgkeuzelijst voor dashboard

De standaardwaarde is 200. U kunt dit nummer in wijzigen [DashboardController.cs][lnk-dashboard-controller].

### <a name="number-of-pins-to-display-in-bing-map-control"></a>Het aantal pincodes dat wordt weer te geven in Bing Maps

De standaardwaarde is 200. U kunt dit nummer in wijzigen [TelemetryApiController.cs][lnk-telemetry-api-controller-01].

### <a name="time-period-of-telemetry-graph"></a>Periode van telemetrie-grafiek

De standaardwaarde is 10 minuten. U kunt deze waarde in wijzigen [TelmetryApiController.cs][lnk-telemetry-api-controller-02].

## <a name="feedback"></a>Feedback

Hebt u een aanpassing u zou willen zien in dit document vallen? Toevoegen van suggesties voor functies op [User Voice](https://feedback.azure.com/forums/321918-azure-iot), of een opmerking in dit artikel. 

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de opties voor het aanpassen van de vooraf geconfigureerde oplossingen:

* [Logische App aansluiten op uw Azure IoT Suite Remote Monitoring vooraf geconfigureerde oplossing][lnk-logicapp]
* [Dynamische telemetrie gebruiken bij de vooraf geconfigureerde oplossing voor externe controle][lnk-dynamic]
* [Metagegevens van apparaten informatie in de vooraf geconfigureerde oplossing voor externe controle][lnk-devinfo]
* [Hoe de gegevens van uw OPC UA-servers worden weergegeven in de oplossing voor verbonden factory aanpassen][lnk-cf-customize]

[lnk-logicapp]: iot-suite-v1-logic-apps-tutorial.md
[lnk-dynamic]: iot-suite-v1-dynamic-telemetry.md
[lnk-devinfo]: iot-suite-v1-remote-monitoring-device-info.md

[IoT Device SDK]: https://azure.microsoft.com/documentation/articles/iot-hub-sdks-summary/
[lnk-permissions]: iot-suite-v1-permissions.md
[lnk-dashboard-controller]: https://github.com/Azure/azure-iot-remote-monitoring/blob/3fd43b8a9f7e0f2774d73f3569439063705cebe4/DeviceAdministration/Web/Controllers/DashboardController.cs#L27
[lnk-telemetry-api-controller-01]: https://github.com/Azure/azure-iot-remote-monitoring/blob/3fd43b8a9f7e0f2774d73f3569439063705cebe4/DeviceAdministration/Web/WebApiControllers/TelemetryApiController.cs#L27
[lnk-telemetry-api-controller-02]: https://github.com/Azure/azure-iot-remote-monitoring/blob/e7003339f73e21d3930f71ceba1e74fb5c0d9ea0/DeviceAdministration/Web/WebApiControllers/TelemetryApiController.cs#L25 
[lnk-sample-device-factory]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Common/Factory/SampleDeviceFactory.cs#L40
[lnk-direct-methods]: ../iot-hub/iot-hub-devguide-direct-methods.md
[lnk-cf-customize]:../iot-accelerators/iot-accelerators-connected-factory-customize.md