---
title: Een aangepaste regel maken in Azure IoT Suite | Microsoft Docs
description: Het maken van een aangepaste regel in een vooraf geconfigureerde IoT Suite-oplossing.
services: ''
suite: iot-suite
documentationcenter: ''
author: dominicbetts
manager: timlt
editor: ''
ms.assetid: 562799dc-06ea-4cdd-b822-80d1f70d2f09
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2017
ms.author: dobett
ms.openlocfilehash: 9bf2a13035de141766fd935966ce18459dccdaab
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38590518"
---
# <a name="create-a-custom-rule-in-the-remote-monitoring-preconfigured-solution"></a>Maak een aangepaste regel in de vooraf geconfigureerde oplossing voor externe controle

## <a name="introduction"></a>Inleiding

In de vooraf geconfigureerde oplossingen, kunt u configureren [regels die wordt geactiveerd wanneer een telemetrie-waarde voor een apparaat een bepaalde drempelwaarde bereikt][lnk-builtin-rule]. [Dynamische telemetrie gebruiken met de externe controle vooraf geconfigureerde oplossing] [ lnk-dynamic-telemetry] wordt beschreven hoe u kunt aangepaste telemetriewaarden toevoegen, zoals *ExternalTemperature* aan uw oplossing. In dit artikel wordt beschreven hoe u aangepaste regel voor dynamisch telemetrietypen maken in uw oplossing.

In deze zelfstudie wordt een eenvoudige Node.js gesimuleerd apparaat om dynamische telemetrie te verzenden naar de vooraf geconfigureerde oplossing voor back-end te genereren. U voegt u aangepaste regels in de **RemoteMonitoring** Visual Studio-oplossing en implementeren van deze aangepaste back-end voor uw Azure-abonnement.

Voor deze zelfstudie hebt u het volgende nodig:

* Een actief Azure-abonnement. Als u geen account hebt, kunt u binnen een paar minuten een account voor de gratis proefversie maken. Zie [Gratis proefversie van Azure][lnk_free_trial] voor meer informatie.
* [Node.js] [ lnk-node] versie 0.12.x of later naar een gesimuleerd apparaat maakt.
* Visual Studio 2015 of Visual Studio 2017 om te wijzigen van de vooraf geconfigureerde oplossing terug eindigen op uw nieuwe regels.

[!INCLUDE [iot-suite-v1-provision-remote-monitoring](../../includes/iot-suite-v1-provision-remote-monitoring.md)]

Noteer de naam van de oplossing die u hebt gekozen voor uw implementatie. Verderop in deze zelfstudie moet u de naam van deze oplossing.

[!INCLUDE [iot-suite-v1-send-external-temperature](../../includes/iot-suite-v1-send-external-temperature.md)]

U kunt de Node.js-consoletoepassing stoppen wanneer u hebt geverifieerd dat het verzendt **ExternalTemperature** telemetrie naar de vooraf geconfigureerde oplossing. Houd het consolevenster geopend, omdat u deze Node.js-consoletoepassing opnieuw uitvoeren nadat u de aangepaste regel aan de oplossing toevoegen.

## <a name="rule-storage-locations"></a>Regel opslaglocaties

Informatie over regels worden bewaard in twee locaties:

* **DeviceRulesNormalizedTable** tabel – deze tabel bevat een genormaliseerde verwijzing naar de regels die zijn gedefinieerd door de portal van de oplossing. Wanneer de oplossingsportal regels voor apparaten weergegeven, vraagt het in deze tabel voor de regeldefinities.
* **DeviceRules** blob – deze blob slaat alle regels gedefinieerd voor alle apparaten geregistreerde en wordt gedefinieerd als een verwijzing invoer voor de Azure Stream Analytics-taken.
 
Wanneer u een bestaande regel bijwerken of definiëren van een nieuwe regel in de portal van de oplossing, worden de tabel- en blob bijgewerkt om de wijzigingen weer te geven. De regeldefinitie van de weergegeven in de portal afkomstig is van de tabel-store en de regeldefinitie waarnaar wordt verwezen door de Stream Analytics-taken afkomstig zijn uit de blob. 

## <a name="update-the-remotemonitoring-visual-studio-solution"></a>Update de RemoteMonitoring Visual Studio-oplossing

De volgende stappen laten zien over het wijzigen van de RemoteMonitoring Visual Studio-oplossing voor het opnemen van een nieuwe regel die gebruikmaakt van de **ExternalTemperature** telemetrie vanaf het gesimuleerde apparaat verzonden:

1. Als u hebt nog niet gedaan, kloon de **azure-iot-remote-monitoring** opslagplaats naar een geschikte locatie op uw lokale computer met de volgende Git-opdracht:

    ```
    git clone https://github.com/Azure/azure-iot-remote-monitoring.git
    ```

2. Open in Visual Studio het bestand RemoteMonitoring.sln uit het lokale exemplaar van de **azure-iot-remote-monitoring** opslagplaats.

3. Open het bestand Infrastructure\Models\DeviceRuleBlobEntity.cs en voeg een **ExternalTemperature** eigenschap als volgt te werk:

    ```csharp
    public double? Temperature { get; set; }
    public double? Humidity { get; set; }
    public double? ExternalTemperature { get; set; }
    ```

4. Voeg in hetzelfde bestand, een **ExternalTemperatureRuleOutput** eigenschap als volgt te werk:

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

6. Werk in hetzelfde bestand, de **_availableDataFields** methode om op te nemen **ExternalTemperature** als volgt:

    ```csharp
    private static List<string> _availableDataFields = new List<string>
    {                    
        Temperature, Humidity, ExternalTemperature
    };
    ```

7. Open het bestand Infrastructure\Repository\DeviceRulesRepository.cs en wijzig de **BuildBlobEntityListFromTableRows** methode als volgt te werk:

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

1. Open een opdrachtprompt met verhoogde bevoegdheid en navigeer naar de hoofdmap van uw lokale exemplaar van de azure-iot-remote-monitoring-opslagplaats.

2. Voor het implementeren van uw bijgewerkte oplossing, voer de volgende opdracht vervangen door **{implementatienaam}** met de naam van uw implementatie van de vooraf geconfigureerde oplossing die u eerder hebt genoteerd:

    ```
    build.cmd cloud release {deployment name}
    ```

## <a name="update-the-stream-analytics-job"></a>De Stream Analytics-taak bijwerken

Wanneer de implementatie voltooid is, kunt u de Stream Analytics-taak voor het gebruik van de nieuwe regeldefinities bijwerken.

1. Navigeer naar de resourcegroep waarin uw resources vooraf geconfigureerde oplossing in de Azure-portal. Deze resourcegroep heeft dezelfde naam die u hebt opgegeven voor de oplossing tijdens de implementatie.

2. Navigeer naar de {implementatienaam}-regels voor Stream Analytics-taak. 

3. Klik op **stoppen** stoppen van de Stream Analytics-taak wordt uitgevoerd. (U moet wachten voor de streaming-taak te stoppen, voordat u kunt de query bewerken).

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

5. Klik op **opslaan** te wijzigen van de regelquery voor bijgewerkte.

6. Klik op **Start** starten van de Stream Analytics-taak opnieuw uit te voeren.

## <a name="add-your-new-rule-in-the-dashboard"></a>De nieuwe regel in het dashboard toevoegen

U kunt nu toevoegen de **ExternalTemperature** regel op een apparaat op het dashboard van de oplossing.

1. Navigeer naar de portal van de oplossing.

2. Navigeer naar de **apparaten** deelvenster.

3. Zoek het aangepaste apparaat u hebt gemaakt die wordt verzonden **ExternalTemperature** Telemetrie en klik op de **Apparaatdetails** deelvenster, klikt u op **regel toevoegen**.

4. Selecteer **ExternalTemperature** in **gegevensveld**.

5. Stel **drempelwaarde** tot 56. Klik vervolgens op **regels opslaan en weergeven**.

6. Ga terug naar het dashboard om de geschiedenis van waarschuwingen weer te geven.

7. In het consolevenster u open blijven staan, start u de Node.js-consoletoepassing om te beginnen met het verzenden van **ExternalTemperature** telemetrische gegevens.

8. U ziet dat de **geschiedenis van waarschuwingen** tabel ziet u nieuwe waarschuwingen wanneer de nieuwe regel wordt geactiveerd.
 
## <a name="additional-information"></a>Aanvullende informatie

Wijzigen van de operator **>** is wat ingewikkelder en gaat verder dan de stappen in deze zelfstudie. U kunt de Stream Analytics-taak voor het gebruik van de operator die u wilt wijzigen, zetten op basis van operator in de oplossingsportal is een meer complexe taak. 

## <a name="next-steps"></a>Volgende stappen
Nu dat u hebt gezien hoe u aangepaste regels maken, kunt u meer informatie over de vooraf geconfigureerde oplossingen:

- [Logische App aansluiten op uw Azure IoT Suite Remote Monitoring vooraf geconfigureerde oplossing][lnk-logic-app]
- [Metagegevens van apparaten informatie in de externe controle vooraf geconfigureerde oplossing][lnk-devinfo].

[lnk-devinfo]: iot-suite-v1-remote-monitoring-device-info.md

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-node]: http://nodejs.org
[lnk-builtin-rule]: iot-suite-v1-getstarted-preconfigured-solutions.md#view-alarms
[lnk-dynamic-telemetry]: iot-suite-v1-dynamic-telemetry.md
[lnk-logic-app]: iot-suite-v1-logic-apps-tutorial.md