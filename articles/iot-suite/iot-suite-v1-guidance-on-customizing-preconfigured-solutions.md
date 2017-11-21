---
title: Aanpassen van vooraf geconfigureerde oplossingen | Microsoft Docs
description: Biedt richtlijnen voor het aanpassen van de vooraf geconfigureerde Azure IoT Suite-oplossingen.
services: 
suite: iot-suite
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 4653ae53-4110-4a10-bd6c-7dc034c293a8
ms.service: iot-suite
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2017
ms.author: corywink
ms.openlocfilehash: ba965b9bc23b96adb2b1b7c9306cb7f508f820bf
ms.sourcegitcommit: f67f0bda9a7bb0b67e9706c0eb78c71ed745ed1d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/20/2017
---
# <a name="customize-a-preconfigured-solution"></a>Een vooraf geconfigureerde oplossing aanpassen

De vooraf geconfigureerde oplossingen die zijn opgegeven met de Azure IoT Suite laten zien dat de services in de suite die samenwerken voor het leveren van een end-to-end-oplossing. Vanaf deze beginpunt zijn er verschillende plaatsen in die u kunt uitbreiden en aanpassen van de oplossing voor specifieke scenario's. De volgende secties worden deze common aanpassing punten.

## <a name="find-the-source-code"></a>De broncode vinden

De broncode voor de vooraf geconfigureerde oplossingen is beschikbaar op GitHub in de volgende opslagplaatsen:

* Externe controle: [https://www.github.com/Azure/azure-iot-remote-monitoring](https://github.com/Azure/azure-iot-remote-monitoring)
* Voorspeld onderhoud: [https://github.com/Azure/azure-iot-predictive-maintenance](https://github.com/Azure/azure-iot-predictive-maintenance)
* Verbonden factory: [https://github.com/Azure/azure-iot-connected-factory](https://github.com/Azure/azure-iot-connected-factory)

De broncode voor de vooraf geconfigureerde oplossingen is voor het demonstreren van de patronen en procedures die worden gebruikt voor het implementeren van de functionaliteit van de end-to-end van een IoT-oplossing met behulp van Azure IoT Suite opgegeven. U vindt meer informatie over het maken en implementeren van de oplossingen in de GitHub-opslagplaatsen.

## <a name="change-the-preconfigured-rules"></a>De vooraf geconfigureerde regels wijzigen

De oplossing voor externe controle omvat drie [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) taken voor het afhandelen van apparaatgegevens, Telemetrie en regellogica in de oplossing.

De drie stream analytics-taken en hun syntaxis worden beschreven in de diepte in de [externe controle oplossing walkthrough over vooraf geconfigureerde](iot-suite-v1-remote-monitoring-sample-walkthrough.md). 

U kunt deze taken om de logica alter rechtstreeks te bewerken of voeg specifieke logica toe aan uw scenario. U vindt de Stream Analytics-taken als volgt:

1. Ga naar [Azure-portal](https://portal.azure.com).
2. Navigeer naar de resourcegroep met dezelfde naam als uw IoT-oplossing. 
3. Selecteer de Azure Stream Analytics-taak die u wilt wijzigen. 
4. De taak stoppen als u selecteert **stoppen** in de reeks opdrachten. 
5. Bewerk de invoer-, query- en uitvoer.
   
    Een eenvoudige wijziging wordt gewijzigd van de query voor de **regels** taak voor het gebruik van een **' < '** in plaats van een **' > '**. Nog steeds weergegeven in de oplossingsportal **' > '** wanneer u een regel voor het bewerken, maar u ziet hoe het gedrag als gevolg van de wijziging in de onderliggende taak wordt gespiegeld.
6. Start de taak

> [!NOTE]
> Het dashboard voor externe controle, is afhankelijk van wat u specifieke gegevens dus wijzigen van de taken leiden het dashboard tot kan mislukken.

## <a name="add-your-own-rules"></a>Uw eigen regels toevoegen

Naast de vooraf geconfigureerde Azure Stream Analytics-taken te wijzigen, kunt u de Azure-portal nieuwe taken toevoegen of nieuwe query's toevoegen aan bestaande projecten.

## <a name="customize-devices"></a>Apparaten aanpassen

Een van de meest voorkomende extensie activiteiten werkt samen met apparaten die specifiek zijn voor uw scenario. Er zijn verschillende methoden voor het werken met apparaten. Deze methoden omvatten een gesimuleerd apparaat zodat deze overeenkomen met uw scenario te wijzigen of met behulp van de [IoT Device SDK] [ IoT Device SDK] uw fysieke apparaat aan de oplossing te sluiten.

Zie voor een stapsgewijze handleiding voor het toevoegen van apparaten, de [Iot Suite-verbinding maken met apparaten](iot-suite-v1-connecting-devices.md) artikel en de [van een remote monitoring C SDK voorbeeld](https://github.com/Azure/azure-iot-sdk-c/tree/master/serializer/samples/remote_monitoring). Dit voorbeeld is ontworpen voor gebruik met de vooraf geconfigureerde oplossing voor externe controle.

### <a name="create-your-own-simulated-device"></a>Uw eigen gesimuleerd apparaat kunt maken

Opgenomen in de [voor externe controle oplossing broncode](https://github.com/Azure/azure-iot-remote-monitoring), is een .NET-simulator. Deze simulator is ingericht als onderdeel van de oplossing en u kunt wijzigen en andere metagegevens, telemetrie verzenden en reageren op andere opdrachten en -methoden.

De vooraf geconfigureerde simulator in de vooraf geconfigureerde oplossing voor externe controle een koelervoorbeeld apparaat simuleert dat temperatuur en vochtigheid telemetrie verzendt. U kunt de simulator in wijzigen de [Simulator.WebJob](https://github.com/Azure/azure-iot-remote-monitoring/tree/master/Simulator/Simulator.WebJob) wanneer u hebt de GitHub-opslagplaats forked project.

### <a name="available-locations-for-simulated-devices"></a>Beschikbare locaties voor de gesimuleerde apparaten

Er is een reeks locaties in Seattle/Redmond, Washington, Verenigde Staten van Amerika. U kunt deze locaties in wijzigen [SampleDeviceFactory.cs][lnk-sample-device-factory].

### <a name="add-a-desired-property-update-handler-to-the-simulator"></a>Een updatehandler gewenste eigenschap toevoegen aan de simulator

U kunt een waarde voor een gewenste eigenschap voor een apparaat in de oplossingsportal instellen. Het is de verantwoordelijkheid van het apparaat voor het afhandelen van de eigenschap wijzigingsaanvraag wanneer het apparaat wordt de gewenste eigenschapswaarde opgehaald. U voegt ondersteuning voor een wijziging van de eigenschap waarde via een gewenste eigenschap, moet u een handler toevoegen aan de simulator.

De simulator handlers bevat voor de **SetPointTemp** en **TelemetryInterval** eigenschappen die u bijwerken door het instellen van kunt de gewenste waarden in de portal van de oplossing.

Het volgende voorbeeld ziet u de handler voor de **SetPointTemp** gewenst eigenschap in de **CoolerDevice** klasse:

```csharp
protected async Task OnSetPointTempUpdate(object value)
{
    var telemetry = _telemetryController as ITelemetryWithSetPointTemperature;
    telemetry.SetPointTemperature = Convert.ToDouble(value);

    await SetReportedPropertyAsync(SetPointTempPropertyName, telemetry.SetPointTemperature);
}
```

Deze methode werkt de telemetrie punt temperatuur en vervolgens meldt deze wijziging terug naar IoT Hub door een eigenschap gemeld.

U kunt uw eigen handlers voor uw eigen eigenschappen toevoegen aan de hand van het patroon in het voorgaande voorbeeld.

U moet ook de gewenste eigenschap binden aan de handler zoals weergegeven in het volgende voorbeeld uit de **CoolerDevice** constructor:

```csharp
_desiredPropertyUpdateHandlers.Add(SetPointTempPropertyName, OnSetPointTempUpdate);
```

Houd er rekening mee dat **SetPointTempPropertyName** een constante is gedefinieerd als 'Config.SetPointTemp'.

### <a name="add-support-for-a-new-method-to-the-simulator"></a>Ondersteuning voor een nieuwe methode toevoegen aan de simulator

U kunt de simulator om ondersteuning voor een nieuwe [methode (directe methode)][lnk-direct-methods]. Er zijn twee belangrijke stappen vereist:

- De simulator verwittigt de IoT-hub in de vooraf geconfigureerde oplossing met details van de methode.
- De simulator vergezeld gaan van code voor het afhandelen van de methodeaanroep van wanneer u vanuit aanroept de **Apparaatdetails** deelvenster in solution explorer of via een taak.

Externe controle vooraf geconfigureerde oplossing maakt gebruik van *eigenschappen gerapporteerd* details van ondersteunde methoden naar IoT-hub te verzenden. De back-end oplossing houdt een lijst van alle methoden die worden ondersteund door elk apparaat samen met een geschiedenis van methode aanroepen. U kunt deze informatie weergeven over apparaten en methoden aanroepen in de portal van de oplossing.

Melding van de IoT-hub die een apparaat ondersteunt een methode, het apparaat moet details van de methode voor het toevoegen de **SupportedMethods** knooppunt in de gerapporteerde eigenschappen:

```json
"SupportedMethods": {
  "<method signature>": "<method description>",
  "<method signature>": "<method description>"
}
```

De methodehandtekening heeft de volgende indeling: `<method name>--<parameter #0 name>-<parameter #1 type>-...-<parameter #n name>-<parameter #n type>`. Bijvoorbeeld, om op te geven de **InitiateFirmwareUpdate** methode verwacht een tekenreeksparameter met de naam **FwPackageURI**, gebruikt u de volgende methodehandtekening:

```json
InitiateFirmwareUpate--FwPackageURI-string: "description of method"
```

Zie voor een lijst van ondersteunde parametertypen de **CommandTypes** klasse in het project voor de infrastructuur.

Stel voor het verwijderen van een methode in de methodehandtekening op `null` in de eigenschappen van gemeld.

> [!NOTE]
> De back-end oplossing informatie over ondersteunde methodes alleen bijgewerkt wanneer het ontvangt een *apparaatgegevens* bericht van het apparaat.

Het volgende codevoorbeeld van de **SampleDeviceFactory** klasse in het algemene project ziet u hoe u een methode toevoegen aan de lijst met **SupportedMethods** in de gerapporteerde eigenschappen is verzonden door het apparaat:

```csharp
device.Commands.Add(new Command(
    "InitiateFirmwareUpdate",
    DeliveryType.Method,
    "Updates device Firmware. Use parameter 'FwPackageUri' to specifiy the URI of the firmware file, e.g. https://iotrmassets.blob.core.windows.net/firmwares/FW20.bin",
    new[] { new Parameter("FwPackageUri", "string") }
));
```

Dit codefragment voegt details van de **InitiateFirmwareUpdate** methode, met inbegrip van tekst moet worden weergegeven in de portal van de oplossing en de details van de vereiste methode-parameters.

De simulator verzendt gemelde eigenschappen, met inbegrip van de lijst met ondersteunde methoden, IoT-hub wanneer de simulator wordt gestart.

Een handler toevoegen aan de simulator code voor elke methode ondersteund. Ziet u de bestaande handlers in de **CoolerDevice** klasse in het project Simulator.WebJob. Het volgende voorbeeld ziet u de handler voor **InitiateFirmwareUpdate** methode:

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

Methode handler namen moeten beginnen met `On` gevolgd door de naam van de methode. De **methodRequest** parameter met de aanroepen van de methode van de back-end oplossing doorgegeven parameters bevat. De retourwaarde moet van het type **taak&lt;MethodResponse&gt;**. De **BuildMethodResponse** hulpprogramma methode kunt u de retourwaarde maken.

Binnen de handler methode kunt u het volgende doen:

- Een asynchrone taak gestart.
- Ophalen van de gewenste eigenschappen van de *apparaat twin* in IoT-Hub.
- Bijwerken van een enkele gemelde eigenschap met de **SetReportedPropertyAsync** methode in de **CoolerDevice** klasse.
- De eigenschappen van meerdere gemelde bijwerken door het maken van een **TwinCollection** exemplaar en het aanroepen van de **Transport.UpdateReportedPropertiesAsync** methode.

Het voorgaande voorbeeld van de firmware-update voert de volgende stappen uit:

- Controleert dat het apparaat kan de aanvraag voor firmware-update accepteren.
- Asynchroon initieert de updatebewerking firmware en de telemetrie weer ingesteld als de bewerking voltooid is.
- Retourneert onmiddellijk het bericht 'FirmwareUpdate geaccepteerd' om aan te geven van dat de aanvraag is geaccepteerd door het apparaat.

### <a name="build-and-use-your-own-physical-device"></a>Opbouwen en uw eigen (fysiek) apparaat gebruiken

De [Azure IoT SDK's](https://github.com/Azure/azure-iot-sdks) bibliotheken voor het verbinden van verschillende typen apparaten (talen en besturingssystemen) in IoT-oplossingen bieden.

## <a name="modify-dashboard-limits"></a>Limieten van dashboard wijzigen

### <a name="number-of-devices-displayed-in-dashboard-dropdown"></a>Aantal apparaten weergegeven in de vervolgkeuzelijst dashboard

De standaardwaarde is 200. U kunt dit nummer in wijzigen [DashboardController.cs][lnk-dashboard-controller].

### <a name="number-of-pins-to-display-in-bing-map-control"></a>Het aantal pincodes dat wordt weergegeven in het kaartbesturingselement van Bing

De standaardwaarde is 200. U kunt dit nummer in wijzigen [TelemetryApiController.cs][lnk-telemetry-api-controller-01].

### <a name="time-period-of-telemetry-graph"></a>De periode van telemetrie-grafiek

De standaardwaarde is 10 minuten. U kunt deze waarde in wijzigen [TelmetryApiController.cs][lnk-telemetry-api-controller-02].

## <a name="feedback"></a>Feedback

Hebt u een aanpassing u zou willen zien in dit document gedekte? Functie suggesties toevoegen aan [User Voice](https://feedback.azure.com/forums/321918-azure-iot), of de opmerking bij dit artikel. 

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de opties voor het aanpassen van de vooraf geconfigureerde oplossingen:

* [Logic App verbinden met uw Azure IoT Suite Remote Monitoring vooraf geconfigureerde oplossing][lnk-logicapp]
* [Dynamische telemetrie gebruiken met de vooraf geconfigureerde oplossing voor externe controle][lnk-dynamic]
* [Metagegevens van apparaten informatie in de vooraf geconfigureerde oplossing voor externe controle][lnk-devinfo]
* [Aanpassen hoe de gegevens van uw OPC UA-servers worden weergegeven in de verbonden factory-oplossing][lnk-cf-customize]

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
[lnk-cf-customize]: iot-suite-connected-factory-customize.md