---
title: Over het maken van de gebruiker gedefinieerde functies in Azure, digitale dubbels | Microsoft Docs
description: Richtlijn voor het maken van de gebruiker gedefinieerde functies, vergelijkingsprogramma voor en roltoewijzingen met digitale dubbels van Azure.
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 12/27/2018
ms.author: alinast
ms.custom: seodec18
ms.openlocfilehash: 91c0b5700fbc648f1fcd1355a438694cecc07a04
ms.sourcegitcommit: fd488a828465e7acec50e7a134e1c2cab117bee8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2019
ms.locfileid: "53993398"
---
# <a name="how-to-create-user-defined-functions-in-azure-digital-twins"></a>Over het maken van de gebruiker gedefinieerde functies in Azure, digitale dubbels

[De gebruiker gedefinieerde functies](./concepts-user-defined-functions.md) gebruikers in staat te configureren van aangepaste logica van binnenkomende berichten over Telemetrie en metagegevens van de ruimtelijke graph worden uitgevoerd. Gebruikers kunnen ook gebeurtenissen verzenden naar vooraf gedefinieerde [eindpunten](./how-to-egress-endpoints.md).

Deze handleiding vindt u een voorbeeld laat zien hoe u voor het detecteren van en waarschuwing voor elke lezen die groter is dan een bepaalde temperatuur van gebeurtenissen ontvangen temperatuur.

[!INCLUDE [Digital Twins Management API](../../includes/digital-twins-management-api.md)]

## <a name="client-library-reference"></a>Naslaginformatie over-clientbibliotheek

Functies die beschikbaar zijn als Help-methoden in de runtime van de gebruiker gedefinieerde functies worden vermeld in de [naslaginformatie over-clientbibliotheek](./reference-user-defined-functions-client-library.md) document.

## <a name="create-a-matcher"></a>Een matcher maken

Vergelijkingsprogramma voor zijn graph-objecten die te bepalen wat de gebruiker gedefinieerde functies worden uitgevoerd voor een bepaalde telemetrieberichten.

- Geldige matcher voorwaarde vergelijkingen:

  - `Equals`
  - `NotEquals`
  - `Contains`

- Geldige matcher voorwaarde doelen:

  - `Sensor`
  - `SensorDevice`
  - `SensorSpace`

Het volgende voorbeeld matcher resulteert in waar op een sensor telemetrie-gebeurtenis met `"Temperature"` als de waarde van het gegevenstype. U kunt meerdere vergelijkingsprogramma voor op een door de gebruiker gedefinieerde functie maken met het maken van een geverifieerde HTTP POST-aanvraag naar:

```plaintext
YOUR_MANAGEMENT_API_URL/matchers
```

Met JSON-hoofdtekst:

```JSON
{
  "Name": "Temperature Matcher",
  "Conditions": [
    {
      "target": "Sensor",
      "path": "$.dataType",
      "value": "\"Temperature\"",
      "comparison": "Equals"
    }
  ],
  "SpaceId": "YOUR_SPACE_IDENTIFIER"
}
```

| Waarde | Vervangen door |
| --- | --- |
| YOUR_SPACE_IDENTIFIER | De serverregio waarin het exemplaar wordt gehost |

## <a name="create-a-user-defined-function"></a>Een door de gebruiker gedefinieerde functie maken

Nadat de vergelijkingsprogramma voor zijn gemaakt, upload het codefragment functie met de volgende HTTP geverifieerde **POST** aanvraag:

```plaintext
YOUR_MANAGEMENT_API_URL/userdefinedfunctions
```

> [!IMPORTANT]
> - Controleer of dat de headers omvatten: `Content-Type: multipart/form-data; boundary="USER_DEFINED_BOUNDARY"`.
> - De opgegeven hoofdtekst wordt gedeeltelijk:
>   - Het eerste deel bevat de vereiste UDF-metagegevens.
>   - Het tweede gedeelte bevat de logica van de compute JavaScript.
> - In de **USER_DEFINED_BOUNDARY** sectie, vervangt de **spaceId** (`YOUR_SPACE_IDENTIFIER`) en **vergelijkingsprogramma voor**(`YOUR_MATCHER_IDENTIFIER`) waarden.
> - Houd er rekening mee de JavaScript-UDF als opgegeven `Content-Type: text/javascript`.

Gebruik de volgende JSON-hoofdtekst:

```plaintext
--USER_DEFINED_BOUNDARY
Content-Type: application/json; charset=utf-8
Content-Disposition: form-data; name="metadata"

{
  "spaceId": "YOUR_SPACE_IDENTIFIER",
  "name": "User Defined Function",
  "description": "The contents of this udf will be executed when matched against incoming telemetry.",
  "matchers": ["YOUR_MATCHER_IDENTIFIER"]
}
--USER_DEFINED_BOUNDARY
Content-Disposition: form-data; name="contents"; filename="userDefinedFunction.js"
Content-Type: text/javascript

function process(telemetry, executionContext) {
  // Code goes here.
}

--USER_DEFINED_BOUNDARY--
```

| Waarde | Vervangen door |
| --- | --- |
| USER_DEFINED_BOUNDARY | De naam van een meerdelige inhoud grens |
| YOUR_SPACE_IDENTIFIER | De ruimte-id  |
| YOUR_MATCHER_IDENTIFIER | De ID van de matcher die u wilt gebruiken |

### <a name="example-functions"></a>Voorbeeld van de functies

De sensor telemetrie lezen voor de sensor rechtstreeks met het gegevenstype ingesteld **temperatuur**, die is `sensor.DataType`:

```JavaScript
function process(telemetry, executionContext) {

  // Get sensor metadata
  var sensor = getSensorMetadata(telemetry.SensorId);

  // Retrieve the sensor value
  var parseReading = JSON.parse(telemetry.Message);

  // Set the sensor reading as the current value for the sensor.
  setSensorValue(telemetry.SensorId, sensor.DataType, parseReading.SensorValue);
}
```

De **telemetrie** parameter wordt aangegeven dat de **SensorId** en **bericht** kenmerken, overeenkomt met een bericht wordt verzonden door een sensor. De **executionContext** parameter wordt aangegeven dat de volgende kenmerken:

```csharp
var executionContext = new UdfExecutionContext
{
    EnqueuedTime = request.HubEnqueuedTime,
    ProcessorReceivedTime = request.ProcessorReceivedTime,
    UserDefinedFunctionId = request.UserDefinedFunctionId,
    CorrelationId = correlationId.ToString(),
};
```

In het volgende voorbeeld melden we een bericht als de sensor telemetrie lezen een vooraf opgegeven drempel wordt overschreden. Als de diagnostische instellingen zijn ingeschakeld op het exemplaar van Azure digitale Twins, worden ook logboeken van de gebruiker gedefinieerde functies doorgestuurd:

```JavaScript
function process(telemetry, executionContext) {

  // Retrieve the sensor value
  var parseReading = JSON.parse(telemetry.Message);

  // Example sensor telemetry reading range is greater than 0.5
  if(parseFloat(parseReading.SensorValue) > 0.5) {
    log(`Alert: Sensor with ID: ${telemetry.SensorId} detected an anomaly!`);
  }
}
```

De volgende code wordt een melding geactiveerd als het niveau van de temperatuur boven aan de vooraf gedefinieerde constante:

```JavaScript
function process(telemetry, executionContext) {

  // Retrieve the sensor value
  var parseReading = JSON.parse(telemetry.Message);

  // Define threshold
  var threshold = 70;

  // Trigger notification 
  if(parseInt(parseReading) > threshold) {
    var alert = {
      message: 'Temperature reading has surpassed threshold',
      sensorId: telemetry.SensorId,
      reading: parseReading
    };

    sendNotification(telemetry.SensorId, "Sensor", JSON.stringify(alert));
  }
}
```

Zie voor een meer complexe voorbeeldcode van de gebruiker gedefinieerde functie de [bezetting snelstartgids](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/actions/userDefinedFunctions/availability.js).

## <a name="create-a-role-assignment"></a>Een roltoewijzing maken

Een roltoewijzing voor de gebruiker gedefinieerde functie om uit te voeren onder maken. Als er geen roltoewijzing voor de gebruiker gedefinieerde functie bestaat, geen deze de juiste machtigingen om te communiceren met de API voor beheer of toegang hebben tot acties uitvoeren op de graph-objecten. Acties die door een gebruiker gedefinieerde functie kan worden uitgevoerd zijn opgegeven en wordt gedefinieerd via op rollen gebaseerd toegangsbeheer in de Azure digitale dubbels Management API's. De gebruiker gedefinieerde functies kunnen bijvoorbeeld binnen het bereik worden beperkt door bepaalde functies of bepaalde access control-paden op te geven. Zie voor meer informatie de [rollen gebaseerd toegangsbeheer](./security-role-based-access-control.md) documentatie.

1. [Query uitvoeren op het systeem-API](./security-create-manage-role-assignments.md#all) voor alle rollen om op te halen van de rol-ID die u wilt toewijzen aan uw UDF. Doen door te maken van een geverifieerde HTTP GET-aanvraag naar:

    ```plaintext
    YOUR_MANAGEMENT_API_URL/system/roles
    ```
   Houd de gewenste rol-ID. Deze wordt doorgegeven als het JSON-hoofdtekst kenmerk **roleId** (`YOUR_DESIRED_ROLE_IDENTIFIER`) hieronder.

1. **object-id** (`YOUR_USER_DEFINED_FUNCTION_ID`) de UDF-id die eerder is gemaakt.
1. Zoek de waarde van **pad** (`YOUR_ACCESS_CONTROL_PATH`) door het opvragen van uw opslagruimten met `fullpath`.
1. Kopieer de geretourneerde `spacePaths` waarde. U gebruikt die hieronder. Een geverifieerde HTTP GET-aanvraag te maken:

    ```plaintext
    YOUR_MANAGEMENT_API_URL/spaces?name=YOUR_SPACE_NAME&includes=fullpath
    ```

    | Waarde | Vervangen door |
    | --- | --- |
    | YOUR_SPACE_NAME | De naam van de ruimte die u wilt gebruiken |

1. Plak de geretourneerde `spacePaths` waarde in **pad** een roltoewijzing UDF maken met het maken van een geverifieerde HTTP POST-aanvraag naar:

    ```plaintext
    YOUR_MANAGEMENT_API_URL/roleassignments
    ```
    Met JSON-hoofdtekst:

    ```JSON
    {
      "roleId": "YOUR_DESIRED_ROLE_IDENTIFIER",
      "objectId": "YOUR_USER_DEFINED_FUNCTION_ID",
      "objectIdType": "YOUR_USER_DEFINED_FUNCTION_TYPE_ID",
      "path": "YOUR_ACCESS_CONTROL_PATH"
    }
    ```

    | Waarde | Vervangen door |
    | --- | --- |
    | YOUR_DESIRED_ROLE_IDENTIFIER | De id voor de gewenste rol |
    | YOUR_USER_DEFINED_FUNCTION_ID | De ID voor de UDF die u wilt gebruiken |
    | YOUR_USER_DEFINED_FUNCTION_TYPE_ID | De ID op te geven de UDF-type |
    | YOUR_ACCESS_CONTROL_PATH | De access control-pad |

>[!TIP]
> Lees het artikel [maken en beheren van roltoewijzingen](./security-create-manage-role-assignments.md) voor meer informatie over de UDF met betrekking tot beheer van API-bewerkingen en eindpunten.

## <a name="send-telemetry-to-be-processed"></a>Verzenden van telemetrie moeten worden verwerkt

De sensor is gedefinieerd in de grafiek ruimtelijke intelligence verzendt telemetrie. De telemetrie activeert op zijn beurt de uitvoering van de gebruiker gedefinieerde functie dat is geüpload. De gegevensverwerker, neemt de telemetrie. Vervolgens wordt een planning worden uitgevoerd voor het aanroepen van de gebruiker gedefinieerde functie gemaakt.

1. De vergelijkingsprogramma voor voor de sensor die lezen is gegenereerd op basis van worden opgehaald.
1. Afhankelijk van de beoordeling van welke vergelijkingsprogramma voor zijn voltooid, de bijbehorende door de gebruiker gedefinieerde functies worden opgehaald.
1. Elke gebruiker gedefinieerde functie uitgevoerd.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [digitale dubbels Azure-eindpunten maken](./how-to-egress-endpoints.md) voor het verzenden van gebeurtenissen die moeten worden.

- Lees voor meer informatie over routering in Azure, digitale dubbels [Routing gebeurtenissen en -berichten](./concepts-events-routing.md).

- Controleer de [naslagdocumentatie voor client-bibliotheek](./reference-user-defined-functions-client-library.md).
