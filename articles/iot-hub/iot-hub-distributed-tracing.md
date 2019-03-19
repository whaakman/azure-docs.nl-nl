---
title: Correlatie-id's toevoegen aan IoT-berichten met gedistribueerde tracering (preview)
description: ''
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 02/06/2019
ms.author: jlian
ms.openlocfilehash: 0553bd904cfaabaefce4e6ab3f7fbf5d356922d3
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58100357"
---
# <a name="trace-azure-iot-device-to-cloud-messages-with-distributed-tracing-preview"></a>Traceringsberichten Azure IoT-apparaat-naar-cloud met gedistribueerde tracering (preview)

Microsoft Azure IoT Hub biedt momenteel ondersteuning voor gedistribueerde tracering als een [preview-functie](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

IoT Hub is een van de eerste Azure-services voor de ondersteuning van gedistribueerde tracering. Als u meer Azure-services ondersteunen gedistribueerde tracering, zult u kunt traceringsberichten IoT in de Azure-services die betrokken zijn bij uw oplossing. Zie voor een achtergrond op gedistribueerde tracering [gedistribueerde tracering](../azure-monitor/app/distributed-tracing.md).

Gedistribueerde tracering voor IoT Hub kunt u de testomgeving:

- Nauwkeurig bewaken van de stroom van elk bericht via IoT Hub met behulp van [trace context](https://github.com/w3c/trace-context). Deze trace-context bevat de correlatie-id's waarmee u kunt gebeurtenissen van het ene correleren met gebeurtenissen uit een ander onderdeel. Deze kan worden toegepast voor een subset of alle berichten met IoT-apparaat met behulp van [apparaatdubbel](iot-hub-devguide-device-twins.md).
- Automatisch aanmelden de trace-context [diagnostische logboeken van Azure Monitor](iot-hub-monitor-resource-health.md).
- Meten en begrijpen van de berichtenstroom en de latentie van apparaten naar IoT Hub en routering eindpunten.
- Overweegt hoe wilt u gedistribueerde tracering voor de niet-Azure-services implementeren in uw IoT-oplossing.

In dit artikel gebruikt u de [Azure IoT device-SDK voor C](./iot-hub-device-sdk-c-intro.md) met gedistribueerde tracering. Ondersteuning voor gedistribueerde tracering wordt nog steeds uitgevoerd voor de andere SDK's.

## <a name="prerequisites"></a>Vereisten

- De Preview-versie van gedistribueerde tracering is momenteel alleen ondersteund voor IoT-Hubs die zijn gemaakt in de volgende regio's:

  - **Noord-Europa**
  - **Zuidoost-Azië**
  - **VS-West 2**

- In dit artikel wordt ervan uitgegaan dat u bekend bent met berichten over telemetrie verzenden naar uw IoT-hub. Zorg ervoor dat u hebt voltooid, de [verzenden van telemetrie C snelstartgids](./quickstart-send-telemetry-c.md).

- Een apparaat te registreren bij uw IoT hub (beschikbaar in elke snelstartgids stappen) en noteert u de verbindingsreeks.

- Installeer de meest recente versie van [Git](https://git-scm.com/download/).

## <a name="configure-iot-hub"></a>IoT Hub configureren

In deze sectie configureert u een IoT-Hub voor logboekregistratie van gedistribueerde tracering kenmerken (correlatie-id's en tijdstempels).

1. Navigeer naar uw IoT-hub in de [Azure-portal](https://portal.azure.com/).

1. In het linkerdeelvenster voor uw IoT-hub, schuif omlaag naar de **bewaking** sectie en klikt u op **diagnostische instellingen**.

1. Als diagnostische instellingen zijn niet is ingeschakeld, klikt u op **diagnostische gegevens inschakelen**. Als u diagnostische instellingen al hebt ingeschakeld, klikt u op **diagnostische instelling toevoegen**.

1. In de **naam** en voer een naam op voor een nieuwe diagnostische instelling. Bijvoorbeeld, **DistributedTracingSettings**.

1. Kies een of meer van de volgende opties om te bepalen waar de logboekregistratie wordt verzonden:

    - **Archiveren naar een opslagaccount**: Configureer een opslagaccount voor de logboekregistratie informatie bevatten.
    - **Stream naar een event hub**: Configureer een event hub bevat gegevens in het logboek.
    - **Verzenden naar Log Analytics**: Configureer een log analytics-werkruimte als u wilt de logboekregistratie informatie bevatten.

1. In de **Log** sectie, selecteert u de bewerkingen die u wilt dat gegevens voor logboekregistratie.

    Zorg ervoor dat u **DistributedTracing**, en configureer een **retentie** voor hoeveel dagen u wilt dat de logboekregistratie bewaard. Logboekbehoud heeft invloed op de kosten voor opslag.

    ![Schermopname die laat zien waar de categorie DistributedTracing is bedoeld voor IoT diagnostische instellingen](./media/iot-hub-distributed-tracing/diag-logs.png)

1. Klik op **opslaan** voor de nieuwe instelling.

1. (Optioneel) Om te zien van de berichten naar verschillende locaties stromen, instellen van [routeringsregels op ten minste twee verschillende eindpunten](iot-hub-devguide-messages-d2c.md).

Zodra de logboekregistratie is ingeschakeld, registreert IoT Hub een logboek wanneer een bericht weergegeven met geldige eigenschappen is aangetroffen in een van de volgende situaties:

- De berichten bij IoT-Hub gateway aankomt.
- Het bericht is verwerkt door de IoT-Hub.
- Het bericht wordt doorgestuurd naar aangepaste eindpunten. Routering moet zijn ingeschakeld.

Zie voor meer informatie over deze logboeken en hun schema's, [gedistribueerde tracering in IoT Hub diagnostische logboeken](iot-hub-monitor-resource-health.md#distributed-tracing-preview).

## <a name="set-up-device"></a>Apparaat instellen

In dit gedeelte bereidt u een ontwikkelomgeving voor gebruik met de [Azure IoT C SDK](https://github.com/Azure/azure-iot-sdk-c). Vervolgens, wijzigt u een van de voorbeelden voor het inschakelen van gedistribueerde tracering op berichten over telemetrie van uw apparaat te maken.

Deze instructies zijn voor het bouwen van het voorbeeld op Windows. Zie voor andere omgevingen [Compileer de SDK voor C](https://github.com/Azure/azure-iot-sdk-c/blob/master/iothub_client/readme.md#compile) of [voorverpakt C-SDK voor de ontwikkeling van de Platform-specifieke](https://github.com/Azure/azure-iot-sdk-c/blob/master/iothub_client/readme.md#prepackaged-c-sdk-for-platform-specific-development).

### <a name="clone-the-source-code-and-initialize"></a>Klonen van de broncode en initialiseren

1. Installeer ['Desktop development with C++'-werkbelasting](https://docs.microsoft.com/cpp/build/vscpp-step-0-installation?view=vs-2017) voor Visual Studio 2015 of 2017.

1. Installeer [CMake](https://cmake.org/). Zorg ervoor dat deel uitmaakt van uw `PATH` door te typen `cmake -version` vanaf een opdrachtprompt.

1. Open een opdrachtprompt of Git Bash-shell. Voer de volgende opdracht uit voor het klonen van de [Azure IoT C SDK](https://github.com/Azure/azure-iot-sdk-c) GitHub-opslagplaats:

    ```cmd
    git clone https://github.com/Azure/azure-iot-sdk-c.git --recursive -b public-preview
    ```

    De grootte van deze opslagplaats is momenteel ongeveer 220 MB. Deze bewerking kan enkele minuten in beslag nemen.

1. Maak de submap `cmake` in de hoofdmap van de Git-opslagplaats en navigeer naar die map.

    ```cmd
    cd azure-iot-sdk-c    
    mkdir cmake
    cd cmake
    cmake ..
    ```

    Als `cmake` uw C++-compiler niet kan vinden kunnen er build optreden tijdens het uitvoeren van de bovenstaande opdracht. Als dit gebeurt, voert u deze opdracht uit bij de [Visual Studio-opdrachtprompt](https://docs.microsoft.com/dotnet/framework/tools/developer-command-prompt-for-vs). 

    Zodra het bouwen is voltooid, zijn de laatste paar uitvoerregels vergelijkbaar met de volgende uitvoer:

    ```cmd
    $ cmake ..
    -- Building for: Visual Studio 15 2017
    -- Selecting Windows SDK version 10.0.16299.0 to target Windows 10.0.17134.
    -- The C compiler identification is MSVC 19.12.25835.0
    -- The CXX compiler identification is MSVC 19.12.25835.0

    ...

    -- Configuring done
    -- Generating done
    -- Build files have been written to: E:/IoT Testing/azure-iot-sdk-c/cmake
    ```

### <a name="edit-the-send-telemetry-sample-to-enable-distributed-tracing"></a>Bewerken van het voorbeeld van de telemetrie verzenden voor gedistribueerde tracering inschakelen

1. Een editor gebruiken om te openen de `azure-iot-sdk-c/iothub_client/samples/iothub_ll_telemetry_sample/iothub_ll_telemetry_sample.c` bronbestand.

1. Zoek de declaratie van de `connectionString` constante:

    [!code-c[](~/samples-iot-distributed-tracing/iothub_ll_telemetry_sample-c/iothub_ll_telemetry_sample.c?name=snippet_config&highlight=2)]

    Vervang de waarde van de `connectionString` constante met de apparaatverbindingsreeks u genoteerd in de [Registreer een apparaat](./quickstart-send-telemetry-c.md#register-a-device) sectie van de [verzenden van telemetrie C snelstartgids](./quickstart-send-telemetry-c.md).

1. Wijzig de `MESSAGE_COUNT` definiëren voor `5000`:

    [!code-c[](~/samples-iot-distributed-tracing/iothub_ll_telemetry_sample-c/iothub_ll_telemetry_sample.c?name=snippet_config&highlight=3)]

1. Zoek de regel van code die wordt aangeroepen `IoTHubDeviceClient_LL_SetConnectionStatusCallback` voor het registreren van een verbinding status retouraanroepfunctie voordat de berichtenlus van het verzenden. Voeg de code onder die regel toe zoals hieronder wordt weergegeven om aan te roepen `IoTHubDeviceClient_LL_EnablePolicyConfiguration` gedistribueerde tracering voor het apparaat:

    [!code-c[](~/samples-iot-distributed-tracing/iothub_ll_telemetry_sample-c/iothub_ll_telemetry_sample.c?name=snippet_tracing&highlight=5)]

    De `IoTHubDeviceClient_LL_EnablePolicyConfiguration` functie kan beleidsregels voor specifieke IoTHub-functies die zijn geconfigureerd via [apparaatdubbels](./iot-hub-devguide-device-twins.md). Eenmaal `POLICY_CONFIGURATION_DISTRIBUTED_TRACING` is ingeschakeld met de regel van de bovenstaande code, het gedrag van de tracering van het apparaat gedistribueerde tracering van wijzigingen op het dubbele apparaat weer.

1. Toevoegen om te voorkomen dat de voorbeeldapp die wordt uitgevoerd zonder gebruik te maken van uw quotum aanvragen, een vertraging van één seconde aan het einde van de bericht-lus verzenden:

    [!code-c[](~/samples-iot-distributed-tracing/iothub_ll_telemetry_sample-c/iothub_ll_telemetry_sample.c?name=snippet_sleep&highlight=8)]

### <a name="compile-and-run"></a>Compileren en uitvoeren

1. Navigeer naar de *iothub_ll_telemetry_sample* projectmap uit de map CMake (`azure-iot-sdk-c/cmake`) u eerder hebt gemaakt en compileren van het voorbeeld:

    ```cmd
    cd iothub_client/samples/iothub_ll_telemetry_sample
    cmake --build . --target iothub_ll_telemetry_sample --config Debug
    ```

1. Voer de toepassing uit. Het apparaat verzendt telemetrie gedistribueerde tracering ondersteunen.

    ```cmd
    Debug/iothub_ll_telemetry_sample.exe
    ```

1. Houd de app die wordt uitgevoerd. (Optioneel) ziet u het bericht wordt verzonden naar IoT Hub door te kijken in het consolevenster.

<!-- For a client app that can receive sampling decisions from the cloud, check out [this sample](https://aka.ms/iottracingCsample).  -->

### <a name="using-third-party-clients"></a>Met behulp van externe clients

Als u geen gebruik van de C-SDK maakt en nog steeds wilt gedistribueerde tracering voor IoT-Hub bekijken, maken het bericht bevat een `tracestate` eigenschap van de toepassing met de aanmaaktijd van het bericht in de notatie van de unix-tijdstempel. Bijvoorbeeld `tracestate=timestamp=1539243209`. Voor het beheren van het percentage van de berichten die deze eigenschap bevat logica om te luisteren naar gebeurtenissen, zoals apparaatdubbel werkt bij cloud geïnitieerde te implementeren.

## <a name="update-sampling-options"></a>Opties voor het bijwerken van lijnen 

Als u wilt wijzigen van het percentage van de berichten vanuit de cloud worden getraceerd, moet u het dubbele apparaat bijwerken. U kunt dit met inbegrip van de JSON-editor in de portal en de IoT Hub-service-SDK op meerdere manieren uitvoeren. De volgende subsecties vindt u voorbeelden.

### <a name="update-using-the-portal"></a>Bijwerken met behulp van de portal

1. Navigeer naar uw IoT-hub in [Azure-portal](https://portal.azure.com/), klikt u vervolgens op **IoT-apparaten**.

1. Klik op het apparaat.

1. Zoek naar **inschakelen gedistribueerde tracering (preview)** en selecteer vervolgens **inschakelen**.

    ![Gedistribueerde tracering inschakelen in Azure portal](./media/iot-hub-distributed-tracing/azure-portal.png)

1. Kies een **samplefrequentie** tussen 0 en 100%.

1. Klik op **Opslaan**.

1. Wacht een paar seconden en klikt u op **vernieuwen**, en vervolgens als is bevestigd door het apparaat, een pictogram synchroniseren met een vinkje wordt weergegeven.

1. Ga terug naar het consolevenster voor de app telemetrie-bericht. Ziet u berichten worden verzonden met `tracestate` in de toepassingseigenschappen van de.

    ![Status traceren](./media/iot-hub-distributed-tracing/MicrosoftTeams-image.png)

1. (Optioneel) De samplingfrequentie op een andere waarde en Zie dat de wijziging in de frequentie waarmee berichten omvatten `tracestate` in de toepassingseigenschappen van de.

### <a name="update-using-azure-iot-hub-toolkit-for-vs-code"></a>Bijwerken met behulp van Azure IoT Hub-Toolkit voor Visual Studio Code

1. Visual Studio Code installeren en installeer de nieuwste versie van Azure IoT Hub-Toolkit voor Visual Studio-Code [hier](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools).

1. VS Code opent en [instellen van IoT Hub-verbindingsreeks](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit#user-content-prerequisites).

1. Vouw het apparaat en zoek naar **gedistribueerde tracering instelling (Preview)**. Klik hieronder op **gedistribueerde tracering instelling bijwerken (Preview)** van subknooppunt.

    ![Gedistribueerde tracering inschakelen in Azure IoT Hub Toolkit](./media/iot-hub-distributed-tracing/update-distributed-tracing-setting-1.png)

1. Selecteer in het pop-upvenster **inschakelen**, druk op Enter om te bevestigen van 100 als samplingfrequentie.

    ![Samplingmodus bijwerken](./media/iot-hub-distributed-tracing/update-distributed-tracing-setting-2.png)

    ![Steekproeffrequentie bijwerken](./media/iot-hub-distributed-tracing/update-distributed-tracing-setting-3.png)

### <a name="bulk-update-for-multiple-devices"></a>Bulk-update voor meerdere apparaten

Gebruiken voor het bijwerken van de configuratie van de gedistribueerde tracering sampling voor meerdere apparaten, [automatische apparaatconfiguratie](iot-hub-auto-device-config.md). Zorg ervoor dat u dit schema dubbele volgen:

```json
{
    "properties": {
        "desired": {
            "azureiot*com^dtracing^1": {
                "sampling_mode": 1,
                "sampling_rate": 100
            }
        }
    }
}
```

| De naam van element | Vereist | Type | Description |
|-----------------|----------|---------|-----------------------------------------------------|
| `sampling_mode` | Ja | Geheel getal | Waarden van de twee modi worden momenteel ondersteund om in te schakelen van steekproeven in of uit. `1` is ingeschakeld en `2` is uitgeschakeld. |
| `sampling_rate` | Ja | Geheel getal | Deze waarde is een percentage. Alleen waarden tussen `0` naar `100` (inclusief) zijn toegestaan.  |

## <a name="query-and-visualize"></a>Opvragen en visualiseren

Query om te zien van alle traces die zijn geregistreerd door een IoT-Hub, de log-store die u hebt geselecteerd in de diagnostische instellingen. Dit gedeelte doorloopt samen met een aantal andere opties.

### <a name="query-using-log-analytics"></a>Met Log Analytics-query uitvoeren

Als u hebt ingesteld [Log Analytics met diagnostische logboeken](../azure-monitor/platform/diagnostic-logs-stream-log-store.md), query door te zoeken naar Logboeken in de `DistributedTracing` categorie. Deze query geeft bijvoorbeeld de traceringen die zijn geregistreerd:

```Kusto
// All distributed traces 
AzureDiagnostics 
| where Category == "DistributedTracing" 
| project TimeGenerated, Category, OperationName, Level, CorrelationId, DurationMs, properties_s 
| order by TimeGenerated asc  
```

Voorbeeld van de logboeken zoals aangegeven in Log Analytics:

| TimeGenerated | OperationName | Categorie | Niveau | CorrelationId | DurationMs | Properties |
|--------------------------|---------------|--------------------|---------------|---------------------------------------------------------|------------|------------------------------------------------------------------------------------------------------------------------------------------|
| 2018-02-22T03:28:28.633Z | DiagnosticIoTHubD2C | DistributedTracing | Informatief | 00-8cd869a412459a25f5b4f31311223344-0144d2590aacd909-01 |  | {"deviceId":"AZ3166","messageSize":"96","callerLocalTimeUtc":"2018-02-22T03:27:28.633Z","calleeLocalTimeUtc":"2018-02-22T03:27:28.687Z"} |
| 2018-02-22T03:28:38.633Z | DiagnosticIoTHubIngress | DistributedTracing | Informatief | 00-8cd869a412459a25f5b4f31311223344-349810a9bbd28730-01 | 20 | {"isRoutingEnabled":"false","parentSpanId":"0144d2590aacd909"} |
| 2018-02-22T03:28:48.633Z | DiagnosticIoTHubEgress | DistributedTracing | Informatief | 00-8cd869a412459a25f5b4f31311223344-349810a9bbd28730-01 | 23 | {"endpointType":"EventHub","endpointName":"myEventHub", "parentSpanId":"0144d2590aacd909"} |

Zie voor meer informatie over de verschillende typen logboeken, [diagnostische logboeken van Azure IoT Hub](iot-hub-monitor-resource-health.md#distributed-tracing-preview).

### <a name="application-map"></a>Toepassingskaart

Als u wilt de stroom van IoT-berichten visualiseren, instellen van het Toepassingsoverzicht voorbeeld-app. De voorbeeld-app verzendt de logboeken gedistribueerde tracering [Toepassingsoverzicht](../application-insights/app-insights-app-map.md) met behulp van een Azure-functie en een Event Hub.

> [!div class="button"]
> <a href="https://github.com/Azure-Samples/e2e-diagnostic-provision-cli" target="_blank">Ophalen van het voorbeeld op Github</a>

Deze onderstaande afbeelding ziet u gedistribueerde tracering in App-kaart met drie routering eindpunten:

![IoT gedistribueerde tracering in App-kaart](./media/iot-hub-distributed-tracing/app-map.png)

## <a name="understand-azure-iot-distributed-tracing"></a>Informatie over dat Azure IoT gedistribueerde tracering

### <a name="context"></a>Context

Veel IoT-oplossingen, met inbegrip van onze eigen [referentiearchitectuur](https://aka.ms/iotrefarchitecture) (alleen Engels), volgt u in het algemeen een variant van de [microservice-architectuur](https://docs.microsoft.com/azure/architecture/microservices/). Wanneer een IoT-oplossing complexere groeit, krijgt u uiteindelijk met behulp van meer dan tien of meer microservices. Deze microservices al dan niet van Azure. Kan moeilijk zijn om dicht waar IoT-berichten zijn neer te zetten of te vertragen. U hebt bijvoorbeeld een IoT-oplossing die gebruikmaakt van 5 verschillende Azure-services en 1500 actieve apparaten. Elk apparaat verzendt 10-apparaat-naar-cloud-berichten per seconde (voor een totaal van 15.000 berichten/seconde), maar u ziet dat uw web-app alleen 10.000 berichten per seconde ziet. Waar is het probleem? Hoe kunt u het overmatig vinden?

### <a name="distributed-tracing-pattern-in-microservice-architecture"></a>Patroon voor gedistribueerde tracering microservice-architectuur

Als u wilt de stroom van een IoT-bericht naar verschillende services reconstrueren, elke service moet doorgeven een *correlatie-ID* die een unieke identificatie van het bericht. Zodra verzameld in een gecentraliseerde systeem, wordt er correlatie-id's kunnen u om te zien van een berichtenstroom. Deze methode wordt aangeroepen de [gedistribueerde tracering patroon](https://docs.microsoft.com/azure/architecture/microservices/logging-monitoring#distributed-tracing).

Ter ondersteuning van grotere acceptatie voor gedistribueerde tracering, Microsoft aanbiedt [W3C standard voorstel voor gedistribueerde tracering](https://w3c.github.io/trace-context/).

### <a name="iot-hub-support"></a>Ondersteuning voor IoT Hub

Eenmaal is ingeschakeld, wordt ondersteuning voor gedistribueerde tracering voor IoT-Hub volgen dit patroon:

1. Een bericht wordt op het IoT-apparaat gegenereerd.
1. De IoT-apparaat besluit (met behulp van de cloud) dat dit bericht moet worden toegewezen aan een trace-context.
1. De SDK wordt toegevoegd een `tracestate` die aan de eigenschap van de toepassing bericht bevat de timestamp van het maken van het bericht.
1. De IoT-apparaat wordt het bericht verzonden naar IoT Hub.
1. Het bericht binnenkomt bij IoT hub gateway.
1. Zoekt naar IoT Hub de `tracestate` in de berichteigenschappen van de toepassing en controles om te zien of deze in de juiste indeling.
1. Als dus IoT Hub wordt gegenereerd en registreert de `trace-id` en `span-id` tot diagnostische logboeken van Azure Monitor in de categorie `DiagnosticIoTHubD2C`.
1. Als de berichtverwerking is voltooid, IoT-Hub genereert een andere `span-id` en registreert deze samen met de bestaande `trace-id` onder de categorie `DiagnosticIoTHubIngress`.
1. Als routering is ingeschakeld voor het bericht, IoT-Hub schrijft deze naar het eindpunt van de aangepaste en andere logboeken `span-id` met dezelfde `trace-id` onder de categorie `DiagnosticIoTHubEgress`.
1. De bovenstaande stappen worden herhaald voor elk bericht dat wordt gegenereerd.

## <a name="public-preview-limits-and-considerations"></a>Openbare preview-beperkingen en overwegingen

- Voorstel voor W3C Trace Context standard is momenteel een concept werken.
- Op dit moment is de enige talen ondersteund door de client-SDK een C.
- Cloud-naar-apparaat dubbele mogelijkheid is niet beschikbaar voor [IoT Hub basic-laag](iot-hub-scaling.md#basic-and-standard-tiers). IoT Hub wordt echter nog steeds aanmelden naar Azure Monitor als er een juist samengesteld trace context-header.
- Om ervoor te zorgen efficiënte bewerking, wordt de IoT Hub een vertraging in de frequentie van logboekregistratie die als onderdeel van gedistribueerde tracering optreden kan opleggen.

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over het algemeen gedistribueerde tracering-patroon in microservices, [Microservice-architectuurpatroon: gedistribueerde tracering](https://microservices.io/patterns/observability/distributed-tracing.html).
- Om in te stellen de configuratie van gedistribueerde traceringsinstellingen toepassen op een groot aantal apparaten, Zie [configureren en controleren van IoT-apparaten op schaal](iot-hub-auto-device-config.md).
- Zie voor meer informatie over Azure Monitor, [wat is Azure Monitor?](../azure-monitor/overview.md).
