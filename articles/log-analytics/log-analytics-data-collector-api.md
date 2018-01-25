---
title: Meld u Analytics HTTP-gegevensverzamelaar API | Microsoft Docs
description: Log Analytics HTTP Data Collector API kunt u POST JSON-gegevens toevoegen aan de opslagplaats logboekanalyse van elke client die u kunt de REST-API aanroepen. Dit artikel wordt beschreven hoe u de API en voorbeelden van hoe u gegevens publiceert met behulp van verschillende programmeertalen heeft.
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: 
ms.assetid: a831fd90-3f55-423b-8b20-ccbaaac2ca75
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2018
ms.author: bwren
ms.openlocfilehash: 88d9c4b23eb676743c004c0d1b3ab45f6cd66055
ms.sourcegitcommit: 28178ca0364e498318e2630f51ba6158e4a09a89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="send-data-to-log-analytics-with-the-http-data-collector-api-public-preview"></a>Gegevens verzenden naar logboekanalyse met de HTTP-API van Data Collector (openbare preview)
In dit artikel laat zien hoe de HTTP-gegevens Collector API gebruiken om gegevens te verzenden met logboekanalyse van een REST-API-client.  Dit wordt beschreven hoe gegevens die door het script of een toepassing verzameld opmaken, opnemen in een aanvraag en die aanvraag geautoriseerd door logboekanalyse hebben.  Voorbeelden zijn bedoeld voor PowerShell, C# en Python.

> [!NOTE]
> Log Analytics HTTP Data Collector API is openbare preview.

## <a name="concepts"></a>Concepten
U kunt HTTP Data Collector API gebruiken om gegevens te verzenden met logboekanalyse vanaf elke client die een REST-API kan aanroepen.  Dit wordt mogelijk een runbook in Azure Automation die management verzamelt mogelijk gegevens van Azure of een andere cloud wordt aangegeven of een alternatieve beheersysteem dat Log Analytics gebruikt om te consolideren en analyseren van gegevens.

Alle gegevens in de opslagplaats Log Analytics wordt opgeslagen als een record met een bepaald recordtype.  U uw gegevens worden verzonden naar de API van HTTP-Data Collector als meerdere records in JSON-notatie.  Wanneer u de gegevens verzendt, wordt een afzonderlijke record gemaakt in de opslagplaats voor elke record in de nettolading van de aanvraag.


![Overzicht van de HTTP-gegevensverzamelaar](media/log-analytics-data-collector-api/overview.png)



## <a name="create-a-request"></a>Een aanvraag maken
Voor het gebruik van de API van HTTP-Data Collector, moet u een POST-aanvraag met de gegevens worden verzonden in de notatie JSON (JavaScript Object) maken.  De volgende drie tabellen worden de kenmerken die vereist voor elke aanvraag zijn. Elk kenmerk in meer detail later in dit artikel worden beschreven.

### <a name="request-uri"></a>Aanvraag-URI
| Kenmerk | Eigenschap |
|:--- |:--- |
| Methode |POST |
| URI |https://\<CustomerId\>.ods.opinsights.azure.com/api/logs?api-version=2016-04-01 |
| Inhoudstype |application/json |

### <a name="request-uri-parameters"></a>De parameters van de aanvraag-URI
| Parameter | Beschrijving |
|:--- |:--- |
| Klant-id |De unieke id voor de Microsoft Operations Management Suite-werkruimte. |
| Resource |De naam van de API-resource: / api/Logboeken. |
| API-versie |De versie van de API voor gebruik met deze aanvraag. Het is momenteel 2016-04-01. |

### <a name="request-headers"></a>Aanvraagheaders
| Koptekst | Beschrijving |
|:--- |:--- |
| Autorisatie |De autorisatie-handtekening. U kunt later in dit artikel lezen over het maken van een HMAC SHA256-header. |
| Log-Type |Geef het recordtype van de gegevens die wordt verzonden. Het logboektype ondersteunt momenteel alleen alfanumerieke tekens. Het ondersteunt geen numerieke waarden of speciale tekens. |
| x-ms-date |De datum waarop de aanvraag is verwerkt in RFC 1123-indeling. |
| time-generated-field |De naam van een veld in de gegevens die de tijdstempel van het gegevensitem bevat. Als u een veld opgeven en vervolgens de inhoud ervan worden gebruikt voor **TimeGenerated**. Als dit veld niet wordt opgegeven, de standaardwaarde voor **TimeGenerated** is de tijd die het bericht wordt ingenomen. De inhoud van het berichtenveld moeten volgen de ISO 8601-notatie jjjj-MM-ssZ. |

## <a name="authorization"></a>Autorisatie
Elk verzoek aan de API van Log Analytics HTTP Data Collector moet een autorisatie-header bevatten. Voor een aanvraag voor verificatie, moet u de aanvraag met de primaire of de secundaire sleutel voor de werkruimte die de aanvraag wordt ingediend ondertekenen. Vervolgens moet die handtekening doorgegeven als onderdeel van de aanvraag.   

Dit is de indeling voor de autorisatie-header:

```
Authorization: SharedKey <WorkspaceID>:<Signature>
```

*WorkspaceID* is de unieke id voor de Operations Management Suite-werkruimte. *Handtekening* is een [HMAC Hash-based Message Authentication Code ()](https://msdn.microsoft.com/library/system.security.cryptography.hmacsha256.aspx) die is gemaakt op basis van de aanvraag en vervolgens wordt berekend met behulp van de [algoritme SHA256](https://msdn.microsoft.com/library/system.security.cryptography.sha256.aspx). Vervolgens coderen u deze met behulp van Base64-codering.

Gebruik deze notatie voor het coderen van de **SharedKey** handtekening tekenreeks:

```
StringToSign = VERB + "\n" +
                  Content-Length + "\n" +
               Content-Type + "\n" +
                  x-ms-date + "\n" +
                  "/api/logs";
```

Hier volgt een voorbeeld van een tekenreeks in handtekening:

```
POST\n1024\napplication/json\nx-ms-date:Mon, 04 Apr 2016 08:00:00 GMT\n/api/logs
```

Wanneer u de tekenreeks handtekening hebt, deze met behulp van het algoritme HMAC SHA256 op de UTF-8-gecodeerde tekenreeks coderen en vervolgens het resultaat als Base64 coderen. Gebruik deze indeling:

```
Signature=Base64(HMAC-SHA256(UTF8(StringToSign)))
```

De voorbeelden in de volgende secties hebben voorbeeldcode voor hulp bij het maken van een autorisatie-header.

## <a name="request-body"></a>Aanvraagtekst
De hoofdtekst van het bericht moet zich in JSON. Er moet een of meer records met de eigenschap naam / waarde-paren opnemen in deze indeling:

```
{
"property1": "value1",
" property 2": "value2"
" property 3": "value3",
" property 4": "value4"
}
```

U kunt meerdere records samen in één aanvraag batch met behulp van de volgende indeling. Alle records moet hetzelfde type zijn.

```
{
"property1": "value1",
" property 2": "value2"
" property 3": "value3",
" property 4": "value4"
},
{
"property1": "value1",
" property 2": "value2"
" property 3": "value3",
" property 4": "value4"
}
```

## <a name="record-type-and-properties"></a>Recordtype en de eigenschappen
U kunt een aangepaste recordtype definiëren bij het indienen van gegevens via de API van Log Analytics HTTP Data Collector. U kunt gegevens op dit moment kan niet schrijven naar bestaande recordtypen die zijn gemaakt door andere gegevenstypen en oplossingen. Log Analytics leest de binnenkomende gegevens en maakt vervolgens eigenschappen die overeenkomen met de gegevenstypen van de waarden die u invoert.

Elke aanvraag naar het Log Analytics-API moet bevatten een **Logboektype** header met de naam voor het recordtype. Het achtervoegsel **_CL** wordt automatisch toegevoegd aan de naam die u invoert om te onderscheiden van andere typen logboek als een aangepast logboek. Bijvoorbeeld, als u de naam invoeren **MyNewRecordType**, logboekanalyse een record gemaakt met het type **MyNewRecordType_CL**. Dit zorgt ervoor dat er geen conflicten tussen typenamen die door de gebruiker is gemaakt en die in de huidig of toekomstig Microsoft-oplossingen hebt verzonden zijn.

Voor het gegevenstype van de eigenschap voegt Log Analytics het achtervoegsel voor de naam van de eigenschap. Als een eigenschap een null-waarde bevat, wordt de eigenschap is niet opgenomen in deze record. Deze tabel bevat de eigenschap gegevenstype en de bijbehorende achtervoegsel:

| Het gegevenstype eigenschap | Achtervoegsel |
|:--- |:--- |
| Tekenreeks |_s |
| Boole-waarde |_b |
| Double |_d |
| Datum/tijd |_t |
| GUID |_g |

Het gegevenstype dat Log Analytics voor elke eigenschap gebruikt, is afhankelijk van of het recordtype dat voor de nieuwe record al bestaat.

* Als het recordtype dat niet bestaat, maakt Log Analytics is een nieuwe. Log Analytics maakt gebruik van de JSON van de typeverwijzing vaststellen welk gegevenstype voor elke eigenschap voor de nieuwe record.
* Als het recordtype bestaat, probeert Log Analytics maakt u een nieuwe record op basis van bestaande eigenschappen. Als het gegevenstype voor een eigenschap in de nieuwe record komt niet overeen met en kan niet worden geconverteerd naar het bestaande type, of als de record een eigenschap die niet bestaat bevat, Log Analytics maakt een nieuwe eigenschap met het achtervoegsel voor de relevante.

Deze vermelding verzending zou bijvoorbeeld een record maken met drie eigenschappen **number_d**, **boolean_b**, en **string_s**:

![Voorbeeldrecord 1](media/log-analytics-data-collector-api/record-01.png)

Als u vervolgens de volgende post met alle waarden die zijn opgemaakt als tekenreeksen hebt ingediend, worden de eigenschappen zou niet wijzigen. Deze waarden kunnen worden geconverteerd naar bestaande gegevenstypen:

![Voorbeeldrecord 2](media/log-analytics-data-collector-api/record-02.png)

Maar als u vervolgens het volgende indienen hebt aangebracht, Log Analytics maakt de nieuwe eigenschappen **boolean_d** en **string_d**. Deze waarden kunnen niet worden geconverteerd:

![Voorbeeldrecord 3](media/log-analytics-data-collector-api/record-03.png)

Als u de volgende vermelding vervolgens ingediend voordat het recordtype dat is gemaakt, Log Analytics maakt een record met drie eigenschappen **gunstig**, **boolean_s**, en **string_s**. In dit item wordt elk van de oorspronkelijke waarden opgemaakt als een tekenreeks:

![Voorbeeldrecord 4](media/log-analytics-data-collector-api/record-04.png)

## <a name="data-limits"></a>Gegevenslimieten
Er zijn enkele beperkingen rond de gegevens op de verzameling Log Analytics-gegevens API geplaatst.

* Maximaal 30 MB per post naar Log Analytics Data Collector API. Dit is een maximale grootte voor een enkele post. Als de gegevens van één die boekt groter is dan 30 MB, moet u de gegevens maximaal kleinere gegevenssegmenten grootte splitsen en ze gelijktijdig te verzenden.
* Maximum van 32 KB limiet voor veldwaarden. Als de veldwaarde groter dan 32 KB is, worden de gegevens afgekapt.
* Aanbevolen maximumaantal velden voor een bepaald type is 50. Dit is een limiet van bruikbaarheid en zoeken ervaring perspectief.  

## <a name="return-codes"></a>Retourcodes
De HTTP-statuscode 200 betekent dat de aanvraag is ontvangen voor verwerking. Dit betekent dat de bewerking is voltooid.

Deze tabel bevat de volledige reeks statuscodes die de service mogelijk geretourneerd:

| Code | Status | Foutcode | Beschrijving |
|:--- |:--- |:--- |:--- |
| 200 |OK | |De aanvraag is geaccepteerd. |
| 400 |Ongeldig verzoek |InactiveCustomer |De werkruimte is gesloten. |
| 400 |Ongeldig verzoek |InvalidApiVersion |De API-versie die u hebt opgegeven, is niet herkend door de service. |
| 400 |Ongeldig verzoek |InvalidCustomerId |De opgegeven werkruimte-ID is ongeldig. |
| 400 |Ongeldig verzoek |InvalidDataFormat |Ongeldige JSON is ingediend. De antwoordtekst mogelijk meer informatie over het oplossen van de fout. |
| 400 |Ongeldig verzoek |InvalidLogType |Het logboektype opgegeven opgenomen speciale tekens of cijfers. |
| 400 |Ongeldig verzoek |MissingApiVersion |De API-versie is niet opgegeven. |
| 400 |Ongeldig verzoek |MissingContentType |Het type inhoud is niet opgegeven. |
| 400 |Ongeldig verzoek |MissingLogType |Het type van de logboekbestanden vereiste waarde is niet opgegeven. |
| 400 |Ongeldig verzoek |UnsupportedContentType |Het type inhoud is niet ingesteld op **application/json**. |
| 403 |Verboden |InvalidAuthorization |De service kan niet verifiëren van de aanvraag. Controleer of de sleutel voor de werkruimte-ID en de verbinding zijn geldig. |
| 404 |Niet gevonden | | Op de opgegeven URL is onjuist of de aanvraag is te groot. |
| 429 |Te veel aanvragen | | De service ondervindt een grote hoeveelheid gegevens uit uw account. Probeer de aanvraag later opnieuw. |
| 500 |Interne serverfout |UnspecifiedError |De service heeft een interne fout aangetroffen. Probeer de aanvraag. |
| 503 |De service is niet beschikbaar |ServiceUnavailable |De service is momenteel niet beschikbaar is om aanvragen te ontvangen. Probeer uw aanvraag. |

## <a name="query-data"></a>Querygegevens
Query uitvoeren op gegevens verzonden door de Log Analytics HTTP Collector API van Data, zoekt u records met **Type** die gelijk is aan de **LogType** waarde die u hebt opgegeven, worden toegevoegd aan de **_CL**. Als u gebruikt bijvoorbeeld **MyCustomLog**, zou u alle records met geretourneerd **Type = MyCustomLog_CL**.

>[!NOTE]
> Als uw werkruimte is bijgewerkt naar de [nieuwe logboekanalyse querytaal](log-analytics-log-search-upgrade.md), en vervolgens de bovenstaande query zou Wijzig in het volgende.

> `MyCustomLog_CL`

## <a name="sample-requests"></a>Voorbeeld aanvragen
In de volgende secties vindt u voorbeelden van hoe u gegevens verzenden naar de API van Log Analytics HTTP Data Collector met behulp van verschillende programmeertalen.

Voer deze stappen om de variabelen voor de autorisatie-header voor elk voorbeeld:

1. Selecteer in de Operations Management Suite-portal, de **instellingen** tegel en selecteer vervolgens de **verbonden bronnen** tabblad.
2. Aan de rechterkant van **werkruimte-ID**, selecteer het pictogram kopiëren en plak de-ID als de waarde van de **klant-ID** variabele.
3. Aan de rechterkant van **primaire sleutel**, selecteer het pictogram kopiëren en plak de-ID als de waarde van de **gedeelde sleutel** variabele.

U kunt ook kunt u de variabelen voor de Logboektype en JSON-gegevens.

### <a name="powershell-sample"></a>PowerShell-voorbeeld
```
# Replace with your Workspace ID
$CustomerId = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"  

# Replace with your Primary Key
$SharedKey = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"

# Specify the name of the record type that you'll be creating
$LogType = "MyRecordType"

# Specify a field with the created time for the records
$TimeStampField = "DateValue"


# Create two records with the same set of properties to create
$json = @"
[{  "StringValue": "MyString1",
    "NumberValue": 42,
    "BooleanValue": true,
    "DateValue": "2016-05-12T20:00:00.625Z",
    "GUIDValue": "9909ED01-A74C-4874-8ABF-D2678E3AE23D"
},
{   "StringValue": "MyString2",
    "NumberValue": 43,
    "BooleanValue": false,
    "DateValue": "2016-05-12T20:00:00.625Z",
    "GUIDValue": "8809ED01-A74C-4874-8ABF-D2678E3AE23D"
}]
"@

# Create the function to create the authorization signature
Function Build-Signature ($customerId, $sharedKey, $date, $contentLength, $method, $contentType, $resource)
{
    $xHeaders = "x-ms-date:" + $date
    $stringToHash = $method + "`n" + $contentLength + "`n" + $contentType + "`n" + $xHeaders + "`n" + $resource

    $bytesToHash = [Text.Encoding]::UTF8.GetBytes($stringToHash)
    $keyBytes = [Convert]::FromBase64String($sharedKey)

    $sha256 = New-Object System.Security.Cryptography.HMACSHA256
    $sha256.Key = $keyBytes
    $calculatedHash = $sha256.ComputeHash($bytesToHash)
    $encodedHash = [Convert]::ToBase64String($calculatedHash)
    $authorization = 'SharedKey {0}:{1}' -f $customerId,$encodedHash
    return $authorization
}


# Create the function to create and post the request
Function Post-LogAnalyticsData($customerId, $sharedKey, $body, $logType)
{
    $method = "POST"
    $contentType = "application/json"
    $resource = "/api/logs"
    $rfc1123date = [DateTime]::UtcNow.ToString("r")
    $contentLength = $body.Length
    $signature = Build-Signature `
        -customerId $customerId `
        -sharedKey $sharedKey `
        -date $rfc1123date `
        -contentLength $contentLength `
        -fileName $fileName `
        -method $method `
        -contentType $contentType `
        -resource $resource
    $uri = "https://" + $customerId + ".ods.opinsights.azure.com" + $resource + "?api-version=2016-04-01"

    $headers = @{
        "Authorization" = $signature;
        "Log-Type" = $logType;
        "x-ms-date" = $rfc1123date;
        "time-generated-field" = $TimeStampField;
    }

    $response = Invoke-WebRequest -Uri $uri -Method $method -ContentType $contentType -Headers $headers -Body $body -UseBasicParsing
    return $response.StatusCode

}

# Submit the data to the API endpoint
Post-LogAnalyticsData -customerId $customerId -sharedKey $sharedKey -body ([System.Text.Encoding]::UTF8.GetBytes($json)) -logType $logType  
```

### <a name="c-sample"></a>C#-voorbeeld
```
using System;
using System.Net;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Security.Cryptography;
using System.Text;
using System.Threading.Tasks;

namespace OIAPIExample
{
    class ApiExample
    {
        // An example JSON object, with key/value pairs
        static string json = @"[{""DemoField1"":""DemoValue1"",""DemoField2"":""DemoValue2""},{""DemoField3"":""DemoValue3"",""DemoField4"":""DemoValue4""}]";

        // Update customerId to your Operations Management Suite workspace ID
        static string customerId = "xxxxxxxx-xxx-xxx-xxx-xxxxxxxxxxxx";

        // For sharedKey, use either the primary or the secondary Connected Sources client authentication key   
        static string sharedKey = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx";

        // LogName is name of the event type that is being submitted to Log Analytics
        static string LogName = "DemoExample";

        // You can use an optional field to specify the timestamp from the data. If the time field is not specified, Log Analytics assumes the time is the message ingestion time
        static string TimeStampField = "";

        static void Main()
        {
            // Create a hash for the API signature
            var datestring = DateTime.UtcNow.ToString("r");
            var jsonBytes = Encoding.UTF8.GetBytes(message);
            string stringToHash = "POST\n" + jsonBytes.Length + "\napplication/json\n" + "x-ms-date:" + datestring + "\n/api/logs";
            string hashedString = BuildSignature(stringToHash, sharedKey);
            string signature = "SharedKey " + customerId + ":" + hashedString;

            PostData(signature, datestring, json);
        }

        // Build the API signature
        public static string BuildSignature(string message, string secret)
        {
            var encoding = new System.Text.ASCIIEncoding();
            byte[] keyByte = Convert.FromBase64String(secret);
            byte[] messageBytes = encoding.GetBytes(message);
            using (var hmacsha256 = new HMACSHA256(keyByte))
            {
                byte[] hash = hmacsha256.ComputeHash(messageBytes);
                return Convert.ToBase64String(hash);
            }
        }

        // Send a request to the POST API endpoint
        public static void PostData(string signature, string date, string json)
        {
            try
            {
                string url = "https://" + customerId + ".ods.opinsights.azure.com/api/logs?api-version=2016-04-01";

                System.Net.Http.HttpClient client = new System.Net.Http.HttpClient();
                client.DefaultRequestHeaders.Add("Accept", "application/json");
                client.DefaultRequestHeaders.Add("Log-Type", LogName);
                client.DefaultRequestHeaders.Add("Authorization", signature);
                client.DefaultRequestHeaders.Add("x-ms-date", date);
                client.DefaultRequestHeaders.Add("time-generated-field", TimeStampField);

                System.Net.Http.HttpContent httpContent = new StringContent(json, Encoding.UTF8);
                httpContent.Headers.ContentType = new MediaTypeHeaderValue("application/json");
                Task<System.Net.Http.HttpResponseMessage> response = client.PostAsync(new Uri(url), httpContent);

                System.Net.Http.HttpContent responseContent = response.Result.Content;
                string result = responseContent.ReadAsStringAsync().Result;
                Console.WriteLine("Return Result: " + result);
            }
            catch (Exception excep)
            {
                Console.WriteLine("API Post Exception: " + excep.Message);
            }
        }
    }
}

```

### <a name="python-sample"></a>Python-voorbeeld
```
import json
import requests
import datetime
import hashlib
import hmac
import base64

# Update the customer ID to your Operations Management Suite workspace ID
customer_id = 'xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'

# For the shared key, use either the primary or the secondary Connected Sources client authentication key   
shared_key = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"

# The log type is the name of the event that is being submitted
log_type = 'WebMonitorTest'

# An example JSON web monitor object
json_data = [{
   "slot_ID": 12345,
    "ID": "5cdad72f-c848-4df0-8aaa-ffe033e75d57",
    "availability_Value": 100,
    "performance_Value": 6.954,
    "measurement_Name": "last_one_hour",
    "duration": 3600,
    "warning_Threshold": 0,
    "critical_Threshold": 0,
    "IsActive": "true"
},
{   
    "slot_ID": 67890,
    "ID": "b6bee458-fb65-492e-996d-61c4d7fbb942",
    "availability_Value": 100,
    "performance_Value": 3.379,
    "measurement_Name": "last_one_hour",
    "duration": 3600,
    "warning_Threshold": 0,
    "critical_Threshold": 0,
    "IsActive": "false"
}]
body = json.dumps(json_data)

#####################
######Functions######  
#####################

# Build the API signature
def build_signature(customer_id, shared_key, date, content_length, method, content_type, resource):
    x_headers = 'x-ms-date:' + date
    string_to_hash = method + "\n" + str(content_length) + "\n" + content_type + "\n" + x_headers + "\n" + resource
    bytes_to_hash = bytes(string_to_hash).encode('utf-8')  
    decoded_key = base64.b64decode(shared_key)
    encoded_hash = base64.b64encode(hmac.new(decoded_key, bytes_to_hash, digestmod=hashlib.sha256).digest())
    authorization = "SharedKey {}:{}".format(customer_id,encoded_hash)
    return authorization

# Build and send a request to the POST API
def post_data(customer_id, shared_key, body, log_type):
    method = 'POST'
    content_type = 'application/json'
    resource = '/api/logs'
    rfc1123date = datetime.datetime.utcnow().strftime('%a, %d %b %Y %H:%M:%S GMT')
    content_length = len(body)
    signature = build_signature(customer_id, shared_key, rfc1123date, content_length, method, content_type, resource)
    uri = 'https://' + customer_id + '.ods.opinsights.azure.com' + resource + '?api-version=2016-04-01'

    headers = {
        'content-type': content_type,
        'Authorization': signature,
        'Log-Type': log_type,
        'x-ms-date': rfc1123date
    }

    response = requests.post(uri,data=body, headers=headers)
    if (response.status_code >= 200 and response.status_code <= 299):
        print 'Accepted'
    else:
        print "Response code: {}".format(response.status_code)

post_data(customer_id, shared_key, body, log_type)
```

## <a name="next-steps"></a>Volgende stappen
- Gebruik de [Log-API van zoekservice](log-analytics-log-search-api.md) gegevens ophalen uit de opslagplaats voor logboekanalyse.
