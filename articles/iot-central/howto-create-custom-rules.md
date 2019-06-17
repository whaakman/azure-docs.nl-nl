---
title: Azure IoT Central uitbreiden met aangepaste regels en meldingen | Microsoft Docs
description: Als ontwikkelaar oplossing een IoT Central-toepassing voor het verzenden van e-mailmeldingen wanneer een apparaat wordt gestopt met het verzenden van telemetrie te configureren. Deze oplossing maakt gebruik van Azure Stream Analytics en Azure Functions.
author: dominicbetts
ms.author: dobett
ms.date: 05/14/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: philmea
ms.openlocfilehash: 5248b9546ffe931b72123778d0d23574e5238405
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66742419"
---
# <a name="extend-azure-iot-central-with-custom-rules-that-send-notifications"></a>Azure IoT Central uitbreiden met aangepaste regels die meldingen verzenden

In deze gebruiksaanwijzing ziet u, als de ontwikkelaar van een oplossing voor het uitbreiden van uw IoT Central-toepassing met aangepaste regels en meldingen. Het voorbeeld ziet een melding naar een operator verzonden wanneer een apparaat niet meer telemetrie te verzenden. De oplossing maakt gebruik van een [Azure Stream Analytics](https://docs.microsoft.com/azure/stream-analytics/) query om te detecteren wanneer een apparaat is gestopt met het verzenden van telemetrie. De Stream Analytics-taak maakt gebruik van [Azure Functions](https://docs.microsoft.com/azure/azure-functions/) e-mails te sturen met behulp van [SendGrid](https://sendgrid.com/docs/for-developers/partners/microsoft-azure/).

In deze gebruiksaanwijzing laat zien hoe u deze al met de ingebouwde regels en acties doen kunt van IoT Central overschrijden.

In deze handleiding leert u hoe u:

* Telemetrie van een IoT Central-toepassing met Stream *voortdurende gegevensexport*.
* Een Stream Analytics-query die wordt gedetecteerd wanneer een apparaat is gestopt met het verzenden van gegevens maken.
* Een e-mail verzenden met behulp van de Azure Functions en SendGrid-services.

## <a name="prerequisites"></a>Vereisten

Voor de stappen in deze handleiding, hebt u een actief Azure-abonnement nodig.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

### <a name="iot-central-application"></a>IoT Central-toepassing

Maken van een IoT Central-toepassing van de [Azure IoT Central - mijn toepassingen](https://aka.ms/iotcentral) pagina met de volgende instellingen:

| Instelling | Value |
| ------- | ----- |
| Betalingsschema | Betalen per gebruik |
| Toepassingsjabloon | Contoso-voorbeeld |
| De naam van de toepassing | Accepteer de standaardwaarde of Kies uw eigen naam |
| URL | Accepteer de standaardwaarde of Kies uw eigen unieke URL-voorvoegsel |
| Directory | Uw Azure Active Directory-tenant |
| Azure-abonnement | Uw Azure-abonnement |
| Regio | US - oost |

De voorbeelden en schermopnamen in dit artikel gebruiken de **VS-Oost** regio. Kies een locatie dicht bij u en zorg ervoor dat u al uw resources in dezelfde regio maken.

### <a name="resource-group"></a>Resourcegroep

Gebruik de [Azure portal om een resourcegroep te maken](https://portal.azure.com/#create/Microsoft.ResourceGroup) met de naam **DetectStoppedDevices** bevatten de andere resources die u maakt. Uw Azure-resources in dezelfde locatie als uw IoT Central-toepassing maken.

### <a name="event-hubs-namespace"></a>Event Hubs-naamruimte

Gebruik de [Azure portal om te maken van een Event Hubs-naamruimte](https://portal.azure.com/#create/Microsoft.EventHub) met de volgende instellingen:

| Instelling | Waarde |
| ------- | ----- |
| Name    | Kies de naamruimtenaam van uw |
| Prijscategorie | Basic |
| Abonnement | Uw abonnement |
| Resourcegroep | DetectStoppedDevices |
| Locatie | US - oost |
| Doorvoereenheden | 1 |

### <a name="stream-analytics-job"></a>Stream Analytics-taak

Gebruik de [Azure portal om te maken van een Stream Analytics-taak](https://portal.azure.com/#create/Microsoft.StreamAnalyticsJob) met de volgende instellingen:

| Instelling | Waarde |
| ------- | ----- |
| Name    | Kies de taaknaam van uw |
| Abonnement | Uw abonnement |
| Resourcegroep | DetectStoppedDevices |
| Locatie | US - oost |
| Hostingomgeving | Cloud |
| Streaming-eenheden | 3 |

### <a name="function-app"></a>Function App

Gebruik de [Azure portal om te maken van een functie-app](https://portal.azure.com/#create/Microsoft.FunctionApp) met de volgende instellingen:

| Instelling | Value |
| ------- | ----- |
| Naam van app    | Kies de naam van uw functie-app |
| Abonnement | Uw abonnement |
| Resourcegroep | DetectStoppedDevices |
| OS | Windows |
| Hostingabonnement | Verbruiksabonnement |
| Locatie | US - oost |
| Runtimestack | .NET |
| Opslag | Nieuwe maken |

### <a name="sendgrid-account"></a>SendGrid-account

Gebruik de [Azure portal een SendGrid-account maken](https://portal.azure.com/#create/Sendgrid.sendgrid) met de volgende instellingen:

| Instelling | Waarde |
| ------- | ----- |
| Name    | Kies de naam van uw SendGrid-account |
| Wachtwoord | Een wachtwoord maken |
| Abonnement | Uw abonnement |
| Resourcegroep | DetectStoppedDevices |
| Prijscategorie | F1 Free |
| Contactgegevens | De vereiste gegevens invullen |

Tijdens het maken van de vereiste resources, uw **DetectStoppedDevices** resourcegroep ziet eruit als in de volgende schermafbeelding:

![De resourcegroep ' gestopt ' apparaten detecteren](media/howto-create-custom-rules/resource-group.png)

## <a name="create-an-event-hub"></a>Een Event Hub maken

U kunt een toepassing IoT Central telemetrie continu exporteren naar een event hub configureren. In deze sectie maakt u een event hub voor het ontvangen van telemetrie van uw IoT Central-toepassing. De event hub biedt de telemetrie naar uw Stream Analytics-taak voor verwerking.

1. In de Azure-portal, gaat u naar uw Event Hubs-naamruimte en selecteer **+ Event Hub**.
1. Naam van uw event hub **centralexport**, en selecteer **maken**.

Uw Event Hubs-naamruimte ziet eruit als in de volgende schermafbeelding:

![Event Hubs-naamruimte](media/howto-create-custom-rules/event-hubs-namespace.png)

## <a name="get-sendgrid-api-key"></a>SendGrid-API-sleutel ophalen

Uw functie-app moet een SendGrid-API-sleutel voor het verzenden van e-mailberichten. Een SendGrid-API-sleutel maken:

1. Navigeer in de Azure-portal naar uw SendGrid-account. Kies vervolgens **beheren** voor toegang tot uw SendGrid-account.
1. Kies in uw SendGrid-account, **instellingen**, klikt u vervolgens **API-sleutels**. Kies **API-sleutel maken**:

    ![SendGrid-API-sleutel maken](media/howto-create-custom-rules/sendgrid-api-keys.png)

1. Op de **API-sleutel maken** pagina, maakt u een sleutel met de naam **AzureFunctionAccess** met **volledige toegang tot de** machtigingen.
1. Noteer de API-sleutel, u hebt deze nodig wanneer u uw functie-app configureren.

## <a name="define-the-function"></a>De functie definiëren

Deze oplossing maakt gebruik van een Azure Functions-app voor het verzenden van een e-mailmelding wanneer de Stream Analytics-taak een gestopte apparaat wordt gedetecteerd. Uw functie-app maken:

1. In de Azure-portal, gaat u naar de **App Service** exemplaar de **DetectStoppedDevices** resourcegroep.
1. Selecteer **+** om een nieuwe functie te maken.
1. Op de **KIEST u een ONTWIKKELOMGEVING** pagina, kies **portal** en selecteer vervolgens **doorgaan**.
1. Op de **CREATE A FUNCTION** pagina, kies **Webhook + API** en selecteer vervolgens **maken**.

De portal maakt een standaardfunctie met de naam **HttpTrigger1**:

![Standaard HTTP-triggerfunctie](media/howto-create-custom-rules/default-function.png)

### <a name="configure-function-bindings"></a>Functiebindingen configureren

Voor het verzenden van e-mailberichten met SendGrid, moet u de bindingen voor uw functie als volgt configureren:

1. Selecteer **integreren**, kiest u de uitvoer **HTTP ($return)** , en selecteer vervolgens **verwijderen**.
1. Kies **+ nieuwe uitvoer**, en kies vervolgens **SendGrid**, en kies vervolgens **Selecteer**. Kies **installeren** de SendGrid-extensie installeren.
1. Wanneer de installatie is voltooid, selecteert u **retourwaarde van functie gebruiken**. Voeg een geldig **naar adres** voor het ontvangen van e-mailmeldingen.  Toevoegen van een geldige **verzendadres** om te gebruiken als de afzender voor e-mailbericht.
1. Selecteer **nieuwe** naast **App-instelling voor SendGrid API-sleutel**. Voer **SendGridAPIKey** als de sleutel en de SendGrid-API-sleutel die u eerder hebt genoteerd als de waarde. Selecteer vervolgens **Maken**.
1. Kies **opslaan** om op te slaan de SendGrid-bindingen voor uw functie.

De instellingen integreren er uitzien zoals in de volgende schermafbeelding:

![Functie-app-integraties](media/howto-create-custom-rules/function-integrate.png)

### <a name="add-the-function-code"></a>De functiecode toevoegen

Voor het implementeren van uw functie toevoegen de C# code voor het parseren van de binnenkomende HTTP-aanvraag en verzenden van de e-mailberichten als volgt:

1. Kies de **HttpTrigger1** werken in uw functie-app en vervang de C# code door de volgende code:

    ```csharp
    #r "Newtonsoft.Json"
    #r "..\bin\SendGrid.dll"

    using System;
    using SendGrid.Helpers.Mail;
    using Microsoft.Azure.WebJobs.Host;
    using Microsoft.AspNetCore.Mvc;
    using Microsoft.Extensions.Primitives;
    using Newtonsoft.Json;

    public static SendGridMessage Run(HttpRequest req, ILogger log)
    {
        string requestBody = new StreamReader(req.Body).ReadToEnd();
        log.LogInformation(requestBody);
        var notifications = JsonConvert.DeserializeObject<IList<Notification>>(requestBody);

        SendGridMessage message = new SendGridMessage();
        message.Subject = "Contoso device notification";

        var content = "The following device(s) have stopped sending telemetry:<br/><br/><table><tr><th>Device ID</th><th>Time</th></tr>";
        foreach(var notification in notifications) {
            log.LogInformation($"No message received - Device: {notification.deviceid}, Time: {notification.time}");
            content += $"<tr><td>{notification.deviceid}</td><td>{notification.time}</td></tr>";
        }
        content += "</table>";
        message.AddContent("text/html", content);

        return message;
    }

    public class Notification
    {
        public string deviceid { get; set; }
        public string time { get; set; }
    }
    ```

    Mogelijk ziet u een foutbericht weergegeven totdat u de nieuwe code opslaat.

1. Selecteer **opslaan** om op te slaan van de functie.

### <a name="test-the-function-works"></a>Test de functie werkt

De functie te testen in de portal, kiest u eerst **logboeken** aan de onderkant van de code-editor. Kies vervolgens **Test** aan de rechterkant van de code-editor. Gebruik de volgende JSON als de **aanvraagtekst**:

```json
[{"deviceid":"test-device-1","time":"2019-05-02T14:23:39.527Z"},{"deviceid":"test-device-2","time":"2019-05-02T14:23:50.717Z"},{"deviceid":"test-device-3","time":"2019-05-02T14:24:28.919Z"}]
```

De functie logboekberichten worden weergegeven in de **logboeken** Configuratiescherm:

![De uitvoer van functie](media/howto-create-custom-rules/function-app-logs.png)

Na een paar minuten de **naar** e-mailadres ontvangt een e-mailbericht met de volgende inhoud:

```txt
The following device(s) have stopped sending telemetry:

Device ID   Time
test-device-1   2019-05-02T14:23:39.527Z
test-device-2   2019-05-02T14:23:50.717Z
test-device-3   2019-05-02T14:24:28.919Z
```

## <a name="add-stream-analytics-query"></a>Stream Analytics-query toevoegen

Deze oplossing maakt gebruik van een Stream Analytics-query om te detecteren wanneer een apparaat wordt gestopt met het verzenden van telemetrie voor meer dan 120 seconden. De query gebruikt de telemetrie van de event hub als invoer. De taak verzendt de queryresultaten naar de functie-app. In deze sectie configureert u de Stream Analytics-taak:

1. In de Azure-portal, gaat u naar uw Stream analytics-taak onder **taken topologie** Selecteer **invoer**, kiest u **+ Stroominvoer toevoegen**, en kies vervolgens **gebeurtenis Hub**.
1. Gebruik de informatie in de volgende tabel om te configureren van de invoer met behulp van de event hub die u eerder hebt gemaakt, en kies vervolgens **opslaan**:

    | Instelling | Value |
    | ------- | ----- |
    | Invoeralias | centraltelemetry |
    | Abonnement | Uw abonnement |
    | Event hub-naamruimte | Uw Event Hub-naamruimte |
    | Event Hub-naam | Gebruik bestaande - **centralexport** |

1. Onder **taken topologie**, selecteer **uitvoer**, kiest u **+ toevoegen**, en kies vervolgens **Azure-functie**.
1. Gebruik de informatie in de volgende tabel om de uitvoer configureren, en kies vervolgens **opslaan**:

    | Instelling | Value |
    | ------- | ----- |
    | Uitvoeralias | EmailNotification |
    | Abonnement | Uw abonnement |
    | Function App | Uw functie-app |
    | Function  | HttpTrigger1 |

1. Onder **taken topologie**, selecteer **Query** en vervang de bestaande query door de volgende SQL:

    ```sql
    with
    LeftSide as
    (
        SELECT
        -- Get the device ID from the message metadata and create a column
        GetMetadataPropertyValue([centraltelemetry], '[EventHub].[IoTConnectionDeviceId]') as deviceid1, 
        EventEnqueuedUtcTime AS time1
        FROM
        -- Use the event enqueued time for time-based operations
        [centraltelemetry] TIMESTAMP BY EventEnqueuedUtcTime
    ),
    RightSide as
    (
        SELECT
        -- Get the device ID from the message metadata and create a column
        GetMetadataPropertyValue([centraltelemetry], '[EventHub].[IoTConnectionDeviceId]') as deviceid2, 
        EventEnqueuedUtcTime AS time2
        FROM
        -- Use the event enqueued time for time-based operations
        [centraltelemetry] TIMESTAMP BY EventEnqueuedUtcTime
    )

    SELECT
        LeftSide.deviceid1 as deviceid,
        LeftSide.time1 as time
    INTO
        [emailnotification]
    FROM
        LeftSide
        LEFT OUTER JOIN
        RightSide 
        ON
        LeftSide.deviceid1=RightSide.deviceid2 AND DATEDIFF(second,LeftSide,RightSide) BETWEEN 1 AND 120
        where
        -- Find records where a device didn't send a message 120 seconds
        RightSide.deviceid2 is NULL
    ```

1. Selecteer **Opslaan**.
1. Voor het starten van de Stream Analytics-taak, kies **overzicht**, klikt u vervolgens **Start**, klikt u vervolgens **nu**, en vervolgens **Start**:

    ![Stream Analytics](media/howto-create-custom-rules/stream-analytics.png)

## <a name="configure-export-in-iot-central"></a>Configureren van uitvoer in IoT Central

Navigeer naar de [IoT Central-toepassing](https://aka.ms/iotcentral) u gemaakt op basis van de sjabloon Contoso. In deze sectie configureert u de toepassing om de telemetrie van de gesimuleerde apparaten naar uw event hub streamen. De export configureren:

1. Navigeer naar de **continue gegevensexport** weergeeft, schakelt **+ nieuw**, en vervolgens **Azure Event Hubs**.
1. Gebruik de volgende instellingen naar de export configureren en selecteer vervolgens **opslaan**:

    | Instelling | Value |
    | ------- | ----- |
    | Weergavenaam | Exporteren naar Eventhubs |
    | Enabled | Aan |
    | Event Hubs-naamruimte | De naam van uw Event Hubs-naamruimte |
    | Event Hub | centralexport |
    | Metingen | Aan |
    | Apparaten | Uit |
    | Apparaatsjablonen | Uit |

![Configuratie voor continue export](media/howto-create-custom-rules/cde-configuration.png)

Wacht totdat de status van de export **met** voordat u doorgaat.

## <a name="test"></a>Testen

Als u wilt testen van de oplossing, kunt u de voortdurende gegevensexport van IoT Central aan gesimuleerde gestopt apparaten uitschakelen:

1. Navigeer in uw IoT Central-toepassing naar de **continue gegevensexport** pagina en selecteer de **exporteren naar Event Hubs** configuratie exporteren.
1. Stel **ingeschakeld** naar **uit** en kies **opslaan**.
1. Nadat ten minste twee minuten, de **naar** e-mailadres ontvangt u een of meer e-mailberichten die lijkt op het volgende voorbeeld inhoud:

    ```txt
    The following device(s) have stopped sending telemetry:

    Device ID   Time
    7b169aee-c843-4d41-9f25-7a02671ee659    2019-05-09T14:28:59.954Z
    ```

## <a name="tidy-up"></a>Opruimen

Als u wilt opruimen na deze instructies en onnodige kosten voorkomen, verwijdert de **DetectStoppedDevices** resourcegroep in Azure portal.

U kunt de IoT Central-toepassing uit verwijderen de **Management** pagina in de toepassing.

## <a name="next-steps"></a>Volgende stappen

In deze handleiding, hebt u geleerd hoe u:

* Telemetrie van een IoT Central-toepassing met Stream *voortdurende gegevensexport*.
* Een Stream Analytics-query die wordt gedetecteerd wanneer een apparaat is gestopt met het verzenden van gegevens maken.
* Een e-mail verzenden met behulp van de Azure Functions en SendGrid-services.

Nu dat u hoe u aangepaste regels en meldingen maakt weet, wordt de voorgestelde volgende stap is te leren hoe u [uitbreiden Azure IoT Central met aangepaste analytics](howto-create-custom-analytics.md).
