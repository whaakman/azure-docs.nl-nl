---
title: Een aangepaste regel maken in Azure IoT Suite | Microsoft Docs
description: Het maken van een aangepaste regel in een vooraf geconfigureerde IoT Suite-oplossing.
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 562799dc-06ea-4cdd-b822-80d1f70d2f09
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: dobett
ms.openlocfilehash: d58c27234ea05a82aaa3e8d72f70c1449980df09
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-custom-rule-in-the-remote-monitoring-preconfigured-solution"></a>Een aangepaste regel maken in de vooraf geconfigureerde oplossing voor externe controle

## <a name="introduction"></a>Inleiding

In de vooraf geconfigureerde oplossingen, configureert u [regels waarmee wordt geactiveerd wanneer een telemetrie waarde voor een apparaat een specifieke drempel bereikt][lnk-builtin-rule]. [Gebruik dynamische telemetrie met externe controle vooraf geconfigureerde oplossing] [ lnk-dynamic-telemetry] wordt beschreven hoe u kunt aangepaste telemetrie waarden toevoegen, zoals *ExternalTemperature* aan uw oplossing. In dit artikel laat zien hoe aangepaste regel maken voor dynamische telemetrie typen in uw oplossing.

Deze zelfstudie wordt een eenvoudige Node.js gesimuleerd apparaat om dynamische telemetrie verzendt naar de back-end van de vooraf geconfigureerde oplossing te genereren. U voegt u aangepaste regels in de **RemoteMonitoring** Visual Studio-oplossing en implementeren van deze aangepaste back-end voor uw Azure-abonnement.

Voor deze zelfstudie hebt u het volgende nodig:

* Een actief Azure-abonnement. Als u geen account hebt, kunt u binnen een paar minuten een account voor de gratis proefversie maken. Zie [Gratis proefversie van Azure][lnk_free_trial] voor meer informatie.
* [Node.js] [ lnk-node] versie 0.12.x of hoger naar een gesimuleerd apparaat maakt.
* Visual Studio 2015 of Visual Studio 2017 te wijzigen van de vooraf geconfigureerde oplossing terug eindigen met uw nieuwe regels.

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

Noteer de naam van de oplossing die u hebt gekozen voor uw implementatie. Verderop in deze zelfstudie moet u de naam van deze oplossing.

[!INCLUDE [iot-suite-send-external-temperature](../../includes/iot-suite-send-external-temperature.md)]

U kunt de Node.js-consoletoepassing stoppen wanneer u hebt geverifieerd dat het verzendt **ExternalTemperature** telemetrie naar de vooraf geconfigureerde oplossing. Houd het consolevenster open omdat u deze Node.js-consoletoepassing opnieuw uitvoeren nadat u de aangepaste regel aan de oplossing toevoegen.

## <a name="rule-storage-locations"></a>Opslaglocaties voor regel

Informatie over de regels worden bewaard op twee locaties:

* **DeviceRulesNormalizedTable** tabel – deze tabel bevat een genormaliseerde verwijzing naar de regels die zijn gedefinieerd door de portal van de oplossing. Als de oplossingsportal apparaat regels weergegeven, wordt deze tabel voor de regeldefinities opgevraagd.
* **DeviceRules** blob – deze blob slaat de regels die zijn gedefinieerd voor alle apparaten geregistreerd en is gedefinieerd als een verwijzing naar de invoer voor de Azure Stream Analytics-taken.
 
Wanneer u een bestaande regel bijwerken of een nieuwe regel in de oplossingsportal definiëren, zijn de tabel en de blob bijgewerkt, zodat de wijzigingen. De regeldefinitie weergegeven in de portal is afkomstig uit de tabel-store en de regeldefinitie waarnaar wordt verwezen door de Stream Analytics-taken afkomstig zijn van de blob. 

## <a name="update-the-remotemonitoring-visual-studio-solution"></a>Update de RemoteMonitoring Visual Studio-oplossing

De volgende stappen ziet u hoe u wijzigt de RemoteMonitoring Visual Studio-oplossing voor het opnemen van een nieuwe regel die gebruikmaakt van de **ExternalTemperature** telemetrie vanaf het gesimuleerde apparaat verzonden:

1. Als u dit nog niet hebt gedaan, kloon de **azure-iot-remote-monitoring** opslagplaats naar een geschikte locatie op uw lokale computer met de volgende Git-opdracht:

    ```
    git clone https://github.com/Azure/azure-iot-remote-monitoring.git
    ```

2. Open in Visual Studio het bestand RemoteMonitoring.sln in uw lokale exemplaar van de **azure-iot-remote-monitoring** opslagplaats.

3. Open het bestand Infrastructure\Models\DeviceRuleBlobEntity.cs en voeg een **ExternalTemperature** eigenschap als volgt:

    ```csharp
    public double? Temperature { get; set; }
    public double? Humidity { get; set; }
    public double? ExternalTemperature { get; set; }
    ```

4. Voeg in hetzelfde bestand, een **ExternalTemperatureRuleOutput** eigenschap als volgt:

    ```csharp
    public string TemperatureRuleOutput { get; set; }
    public string HumidityRuleOutput { get; set; }
    public string ExternalTemperatureRuleOutput { get; set; }
    ```

5. Open het bestand Infrastructure\Models\DeviceRuleDataFields.cs en voeg de volgende **ExternalTemperature** eigenschap na de bestaande **vochtigheid** eigenschap:

    ```csharp
    public static string ExternalTemperature
    {
        get { return "ExternalTemperature"; }
    }
    ```

6. Werk in hetzelfde bestand de **_availableDataFields** methode om te nemen **ExternalTemperature** als volgt:

    ```csharp
    private static List<string> _availableDataFields = new List<string>
    {                    
        Temperature, Humidity, ExternalTemperature
    };
    ```

7. Open het bestand Infrastructure\Repository\DeviceRulesRepository.cs en wijzig de **BuildBlobEntityListFromTableRows** methode als volgt:

    ```csharp
    else if (rule.DataField == DeviceRuleDataFields.Humidity)
    {
        entity.Humidity = rule.Threshold;
        entity.HumidityRuleOutput = rule.RuleOutput;
    }
    else if (rule.DataField == DeviceRuleDataFields.ExternalTemperature)
    {
      entity.ExternalTemperature = rule.Threshold;
      entity.ExternalTemperatureRuleOutput = rule.RuleOutput;
    }
    ```

## <a name="rebuild-and-redeploy-the-solution"></a>Opnieuw maken en implementeren van de oplossing.

U kunt nu de bijgewerkte oplossing implementeren voor uw Azure-abonnement.

1. Open een opdrachtprompt met verhoogde bevoegdheid en navigeer naar de hoofdmap van de lokale kopie van de azure-iot-remote-monitoring-opslagplaats.

2. Voor het implementeren van uw bijgewerkte oplossing, voer de volgende opdracht vervangen door **{implementatienaam}** met de naam van de implementatie van uw vooraf geconfigureerde oplossing die u eerder hebt genoteerd:

    ```
    build.cmd cloud release {deployment name}
    ```

## <a name="update-the-stream-analytics-job"></a>Bijwerken van de Stream Analytics-taak

Wanneer de implementatie voltooid is, kunt u de Stream Analytics-taak voor het gebruik van de nieuwe regeldefinities bijwerken.

1. Ga in de Azure-portal naar de resourcegroep die uw resources vooraf geconfigureerde oplossing bevat. Deze resourcegroep heeft dezelfde naam die u hebt opgegeven voor de oplossing tijdens de implementatie.

2. Navigeer naar de {implementatienaam}-regels Stream Analytics-taak. 

3. Klik op **stoppen** stoppen van de Stream Analytics-taak wordt uitgevoerd. (U moet wachten voor de streaming-taak stoppen voordat u kunt de query bewerken).

4. Klik op **Query**. De query om op te nemen bewerken de **Selecteer** -instructie voor **ExternalTemperature**. Het volgende voorbeeld toont de volledige query met de nieuwe **Selecteer** instructie:

    ```
    WITH AlarmsData AS 
    (
    SELECT
         Stream.IoTHub.ConnectionDeviceId AS DeviceId,
         'Temperature' as ReadingType,
         Stream.Temperature as Reading,
         Ref.Temperature as Threshold,
         Ref.TemperatureRuleOutput as RuleOutput,
         Stream.EventEnqueuedUtcTime AS [Time]
    FROM IoTTelemetryStream Stream
    JOIN DeviceRulesBlob Ref ON Stream.IoTHub.ConnectionDeviceId = Ref.DeviceID
    WHERE
         Ref.Temperature IS NOT null AND Stream.Temperature > Ref.Temperature
     
    UNION ALL
     
    SELECT
         Stream.IoTHub.ConnectionDeviceId AS DeviceId,
         'Humidity' as ReadingType,
         Stream.Humidity as Reading,
         Ref.Humidity as Threshold,
         Ref.HumidityRuleOutput as RuleOutput,
         Stream.EventEnqueuedUtcTime AS [Time]
    FROM IoTTelemetryStream Stream
    JOIN DeviceRulesBlob Ref ON Stream.IoTHub.ConnectionDeviceId = Ref.DeviceID
    WHERE
         Ref.Humidity IS NOT null AND Stream.Humidity > Ref.Humidity
     
    UNION ALL
     
    SELECT
         Stream.IoTHub.ConnectionDeviceId AS DeviceId,
         'ExternalTemperature' as ReadingType,
         Stream.ExternalTemperature as Reading,
         Ref.ExternalTemperature as Threshold,
         Ref.ExternalTemperatureRuleOutput as RuleOutput,
         Stream.EventEnqueuedUtcTime AS [Time]
    FROM IoTTelemetryStream Stream
    JOIN DeviceRulesBlob Ref ON Stream.IoTHub.ConnectionDeviceId = Ref.DeviceID
    WHERE
         Ref.ExternalTemperature IS NOT null AND Stream.ExternalTemperature > Ref.ExternalTemperature
    )
     
    SELECT *
    INTO DeviceRulesMonitoring
    FROM AlarmsData
     
    SELECT *
    INTO DeviceRulesHub
    FROM AlarmsData
    ```

5. Klik op **opslaan** wijzigen van de query voor bijgewerkte regel.

6. Klik op **Start** starten van de Stream Analytics-taak opnieuw uit te voeren.

## <a name="add-your-new-rule-in-the-dashboard"></a>De nieuwe regel toevoegen in het dashboard

U kunt nu toevoegen de **ExternalTemperature** regel met een apparaat in het dashboard van de oplossing.

1. Navigeer naar de portal van de oplossing.

2. Navigeer naar de **apparaten** Configuratiescherm.

3. Zoek het aangepaste apparaat u hebt gemaakt die verzendt **ExternalTemperature** Telemetrie en klik op de **Apparaatdetails** -scherm, klikt u op **regel toevoegen**.

4. Selecteer **ExternalTemperature** in **gegevensveld**.

5. Stel **drempelwaarde** tot 56. Klik vervolgens op **regels opslaan en weergeven**.

6. Ga terug naar het dashboard om de geschiedenis van waarschuwingen weer te geven.

7. In het consolevenster u opengelaten, start u de Node.js-consoletoepassing om te beginnen met verzenden **ExternalTemperature** telemetrische gegevens.

8. U ziet dat de **geschiedenis van waarschuwingen** tabel ziet u nieuwe alarmen wanneer de nieuwe regel wordt geactiveerd.
 
## <a name="additional-information"></a>Aanvullende informatie

Het wijzigen van de operator  **>**  complexer en zich verder uitstrekt dan de stappen in deze zelfstudie. Terwijl u de Stream Analytics-taak voor het gebruik van de operator die u wijzigen kunt, opgetreden bij het weergeven die operator in de oplossingsportal is een complexere taak. 

## <a name="next-steps"></a>Volgende stappen
Nu dat u hebt gezien hoe u aangepaste regels maakt, kunt u meer informatie over de vooraf geconfigureerde oplossingen:

- [Logic App verbinden met uw Azure IoT Suite Remote Monitoring vooraf geconfigureerde oplossing][lnk-logic-app]
- [Metagegevens van apparaten informatie in de externe controle vooraf geconfigureerde oplossing][lnk-devinfo].

[lnk-devinfo]: iot-suite-remote-monitoring-device-info.md

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-node]: http://nodejs.org
[lnk-builtin-rule]: iot-suite-getstarted-preconfigured-solutions.md#view-alarms
[lnk-dynamic-telemetry]: iot-suite-dynamic-telemetry.md
[lnk-logic-app]: iot-suite-logic-apps-tutorial.md