---
title: Azure Functions Microsoft Graph-bindingen | Microsoft Docs
description: Het gebruik van Microsoft Graph triggers en bindingen in de Azure Functions begrijpen.
services: functions
author: mattchenderson
manager: cfowler
editor: 
ms.service: functions
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 09/19/2017
ms.author: mahender
ms.openlocfilehash: 8cf2e4e9e9007549dbdc931b4485c4230c536479
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-functions-microsoft-graph-bindings"></a>Azure Functions Microsoft Graph-bindingen
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

In dit artikel wordt uitgelegd hoe configureren en werken met Microsoft Graph triggers en bindingen in de Azure Functions.
Met deze, kunt u Azure Functions werken met gegevens, inzichten en gebeurtenissen van de [Microsoft Graph](https://graph.microsoft.io).

De uitbreiding voor Microsoft Graph biedt de volgende bindingen:
- Een [auth token invoer binding](#token-input) kunt u communiceren met Microsoft Graph API.
- Een [Excel-tabel invoer binding](#excel-input) kunt u lezen van gegevens vanuit Excel.
- Een [Excel-tabel uitvoer binding](#excel-output) kunt u Excel-gegevens te wijzigen.
- Een [OneDrive bestand invoer binding](#onedrive-input) kunt u lezen van bestanden vanaf OneDrive.
- Een [OneDrive bestand uitvoer binding](#onedrive-output) kunt u bestanden schrijven in OneDrive.
- Een [Outlook-bericht uitvoer binding](#outlook-output) kunt u per e-mail via Outlook.
- Een verzameling van [Microsoft Graph webhook triggers en bindingen](#webhooks) kunt u om te reageren op gebeurtenissen van Microsoft Graph.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

> [!Note] 
> Microsoft Graph-bindingen zijn momenteel in preview.

## <a name="setting-up-the-extensions"></a>Instellen van de extensies

Microsoft Graph-bindingen zijn beschikbaar via _binding extensies_. Binding-uitbreidingen zijn optionele onderdelen naar de Azure Functions-runtime. Deze sectie wordt beschreven hoe u voor het instellen van de token auth-uitbreidingen voor Microsoft Graph en.

### <a name="enabling-functions-20-preview"></a>Inschakelen van functies 2.0 preview

Binding extensies zijn alleen alleen beschikbaar voor Azure Functions 2.0 preview. 

[!INCLUDE [functions-set-runtime-version](../../includes/functions-set-runtime-version.md)]

Zie voor meer informatie, [hoe gericht op Azure Functions-runtime-versies](functions-versions.md).

### <a name="installing-the-extension"></a>Installeren van de extensie

Voor het installeren van een uitbreiding van de Azure-portal, moet u navigeren naar een sjabloon of die verwijst naar deze binding. Maak een nieuwe functie en kies het 'Microsoft Graph'-scenario in het selectiescherm van de sjabloon. Selecteer een van de sjablonen in dit scenario. U kunt ook gaat u naar het tabblad 'Integreren' van een bestaande functie en selecteer een van de bindingen die in dit onderwerp worden behandeld.

In beide gevallen wordt een waarschuwing weergegeven waarmee de extensie te kunnen worden geïnstalleerd. Klik op **installeren** om op te halen van de extensie.

> [!Note] 
> Elke uitbreiding hoeft slechts één keer per functie-app worden geïnstalleerd. Een plan verbruik kan 10 minuten duren voordat het installatieproces van het in de portal.

Als u Visual Studio gebruikt, kunt u de uitbreidingen kunt krijgen door het installeren van deze NuGet-pakketten:
- [Microsoft.Azure.WebJobs.Extensions.AuthTokens](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.AuthTokens/)
- [Microsoft.Azure.WebJobs.Extensions.MicrosoftGraph](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.MicrosoftGraph/)

### <a name="configuring-app-service-authentication--authorization"></a>Configureren van App Service-verificatie / autorisatie

De bindingen die worden beschreven in dit onderwerp vereisen een identiteit moet worden gebruikt. Hierdoor kan Microsoft Graph voor het afdwingen van machtigingen en interacties controleren. De identiteit kan een gebruiker die toegang hebben tot uw toepassing of de toepassing zelf zijn. Voor het configureren van deze identiteit, moet u instellen [App Service-verificatie / autorisatie](https://docs.microsoft.com/azure/app-service/app-service-authentication-overview) met Azure Active Directory. U moet ook om de machtigingen van een resource die uw functies vereisen.

> [!Note] 
> De uitbreiding voor Microsoft Graph biedt alleen ondersteuning voor AAD-verificaties. Gebruikers moeten zich aanmelden met een account voor werk of school.

Als u de Azure-portal onder de prompt voor het installeren van de extensie, ziet u een waarschuwing die u voor het configureren van App Service-verificatie wordt gevraagd / autorisatie- en aanvraag geen machtigingen voor de sjabloon of de binding is vereist. Klik op **AAD nu configureren** of **machtigingen nu toevoegen** zo nodig.






<a name="token-input"></a>
## <a name="auth-token-input-binding"></a>Auth token invoer binding

Deze binding een AAD-token ophalen voor een bepaalde bron en biedt dit toe aan uw code als een tekenreeks. De bron kan bestaan uit een waarvoor de toepassing machtigingen heeft. 

### <a name="configuring-an-auth-token-input-binding"></a>Een binding auth token invoer configureren

De binding zelf geen alle AAD-machtigingen nodig, maar afhankelijk van hoe het token wordt gebruikt, moet u mogelijk aanvullende machtigingen. Controleer de vereisten van de resource die u van plan bent voor toegang tot aan het token.

De binding ondersteunt de volgende eigenschappen:

|Eigenschap|Beschrijving|
|--------|--------|
|**naam**|Vereist: de naam van de variabele in functiecode gebruikt voor het auth-token. Zie [binding van een code met behulp van een token auth invoer](#token-input-code).|
|**type**|Vereist - moet worden ingesteld op `token`.|
|**richting**|Vereist - moet worden ingesteld op `in`.|
|**identiteit**|Vereist: de identiteit die wordt gebruikt voor de actie uitvoeren. Een van de volgende waarden kan zijn:<ul><li><code>userFromRequest</code>-Alleen geldig met [HTTP-trigger]. Maakt gebruik van de identiteit van de aanvragende gebruiker.</li><li><code>userFromId</code>-Maakt gebruik van de identiteit van een gebruiker eerder is geregistreerd met de opgegeven ID. Zie de <code>userId</code> eigenschap.</li><li><code>userFromToken</code>-De identiteit die wordt vertegenwoordigd door het opgegeven token gebruikt. Zie de <code>userToken</code> eigenschap.</li><li><code>clientCredentials</code>-De identiteit van de functie-app gebruikt.</li></ul>|
|**gebruikers-id** |Indien nodig en alleen als _identiteit_ is ingesteld op `userFromId`. Een gebruiker principal-ID die is gekoppeld aan een gebruiker eerder is geregistreerd.|
|**userToken**|Indien nodig en alleen als _identiteit_ is ingesteld op `userFromToken`. Een token geldig voor de functie-app. |
|**resource**|Vereist: een AAD-bron-URL waarvoor het token wordt aangevraagd.|

<a name="token-input-code"></a>
### <a name="using-an-auth-token-input-binding-from-code"></a>Met behulp van een auth token invoer-binding van code

Het token wordt altijd geverifieerd op de code als een tekenreeks.

#### <a name="sample-getting-user-profile-information"></a>Voorbeeld: Gebruikersprofielgegevens ophalen

Stel dat u hebt de volgende function.json die definieert een HTTP-trigger met een token invoer binding:

```json
{
  "bindings": [
    {
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "type": "token",
      "direction": "in",
      "name": "graphToken",
      "resource": "https://graph.microsoft.com",
      "identity": "userFromRequest"
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

De volgende C#-voorbeeld gebruikt het token voor het maken van een HTTP-aanroep naar de Microsoft Graph en retourneert het resultaat:

```csharp
using System.Net; 
using System.Net.Http; 
using System.Net.Http.Headers; 

public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, string graphToken, TraceWriter log)
{
    HttpClient client = new HttpClient();
    client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", graphToken);
    return await client.GetAsync("https://graph.microsoft.com/v1.0/me/");
}
```

Het volgende voorbeeld met JS het token gebruikt voor het maken van een HTTP-aanroep naar de Microsoft Graph en retourneert het resultaat. In de `function.json` hierboven wijzigen `$return` naar `res` eerste.

```js
const rp = require('request-promise');

module.exports = function (context, req) {
    let token = "Bearer " + context.bindings.graphToken;

    let options = {
        uri: 'https://graph.microsoft.com/v1.0/me/',
        headers: {
            'Authorization': token
        }
    };
    
    rp(options)
        .then(function(profile) {
            context.res = {
                body: profile
            };
            context.done();
        })
        .catch(function(err) {
            context.res = {
                status: 500,
                body: err
            };
            context.done();
        });
};
```





<a name="excel-input"></a>
## <a name="excel-table-input-binding"></a>Invoer binding voor Excel-tabel

Deze binding leest de inhoud van een Excel-tabel die is opgeslagen in OneDrive.

### <a name="configuring-an-excel-table-input-binding"></a>Een Excel-tabel invoer binding configureren

Deze binding worden de volgende AAD-machtigingen vereist:
|Resource|Machtiging|
|--------|--------|
|Microsoft Graph|Gebruikersbestanden lezen|

De binding ondersteunt de volgende eigenschappen:

|Eigenschap|Beschrijving|
|--------|--------|
|**naam**|Vereist: de naam van de variabele in functiecode gebruikt voor de Excel-tabel. Zie [binding van een code met behulp van een Excel-tabel invoer](#excel-input-code).|
|**type**|Vereist - moet worden ingesteld op `excel`.|
|**richting**|Vereist - moet worden ingesteld op `in`.|
|**identiteit**|Vereist: de identiteit die wordt gebruikt voor de actie uitvoeren. Een van de volgende waarden kan zijn:<ul><li><code>userFromRequest</code>-Alleen geldig met [HTTP-trigger]. Maakt gebruik van de identiteit van de aanvragende gebruiker.</li><li><code>userFromId</code>-Maakt gebruik van de identiteit van een gebruiker eerder is geregistreerd met de opgegeven ID. Zie de <code>userId</code> eigenschap.</li><li><code>userFromToken</code>-De identiteit die wordt vertegenwoordigd door het opgegeven token gebruikt. Zie de <code>userToken</code> eigenschap.</li><li><code>clientCredentials</code>-De identiteit van de functie-app gebruikt.</li></ul>|
|**gebruikers-id** |Indien nodig en alleen als _identiteit_ is ingesteld op `userFromId`. Een gebruiker principal-ID die is gekoppeld aan een gebruiker eerder is geregistreerd.|
|**userToken**|Indien nodig en alleen als _identiteit_ is ingesteld op `userFromToken`. Een token geldig voor de functie-app. |
|**pad**|Vereist: het pad in OneDrive naar de Excel-werkmap.|
|**worksheetName**|Het werkblad waarin de tabel is gevonden.|
|**tableName**|De naam van de tabel. Als niet wordt opgegeven, wordt de inhoud van het werkblad worden gebruikt.|

<a name="excel-input-code"></a>
### <a name="using-an-excel-table-input-binding-from-code"></a>Met behulp van een tabel Excel binding invoer van code

De binding bevat de volgende typen aan .NET-functies:
- String []]
- Microsoft.Graph.WorkbookTable
- Aangepaste objecttypen (met behulp van de structurele modelbinding)

#### <a name="sample-reading-an-excel-table"></a>Voorbeeld: Een Excel-tabel lezen

Stel dat u hebt de volgende function.json die definieert een HTTP-trigger met een Excel-invoer binding:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "type": "excel",
      "direction": "in",
      "name": "excelTableData",
      "path": "{query.workbook}",
      "identity": "UserFromRequest",
      "tableName": "{query.table}"
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

De volgende C#-voorbeeld voegt de inhoud van de opgegeven tabel leest en vrijgegeven, gaan ze naar de gebruiker:

```csharp
using System.Net;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Extensions.Primitives; 

public static IActionResult Run(HttpRequest req, string[][] excelTableData, TraceWriter log)
{
    return new OkObjectResult(excelTableData);
}
```

Het volgende voorbeeld met JS voegt de inhoud van de opgegeven tabel leest en vrijgegeven, gaan ze naar de gebruiker. In de `function.json` hierboven wijzigen `$return` naar `res` eerste.

```js
module.exports = function (context, req) {
    context.res = {
        body: context.bindings.excelTableData
    };
    context.done();
};
```

<a name="excel-output"></a>
## <a name="excel-table-output-binding"></a>Excel-tabel uitvoer binding

Deze binding Hiermee wijzigt u de inhoud van een Excel-tabel die is opgeslagen in OneDrive.

### <a name="configuring-an-excel-table-output-binding"></a>Binding voor het configureren van een Excel-tabel uitvoer

Deze binding worden de volgende AAD-machtigingen vereist:
|Resource|Machtiging|
|--------|--------|
|Microsoft Graph|Volledige toegang tot gebruikersbestanden|

De binding ondersteunt de volgende eigenschappen:

|Eigenschap|Beschrijving|
|--------|--------|
|**naam**|Vereist: de naam van de variabele in functiecode gebruikt voor het auth-token. Zie [binding van een code met behulp van een Excel-tabel uitvoer](#excel-output-code).|
|**type**|Vereist - moet worden ingesteld op `excel`.|
|**richting**|Vereist - moet worden ingesteld op `out`.|
|**identiteit**|Vereist: de identiteit die wordt gebruikt voor de actie uitvoeren. Een van de volgende waarden kan zijn:<ul><li><code>userFromRequest</code>-Alleen geldig met [HTTP-trigger]. Maakt gebruik van de identiteit van de aanvragende gebruiker.</li><li><code>userFromId</code>-Maakt gebruik van de identiteit van een gebruiker eerder is geregistreerd met de opgegeven ID. Zie de <code>userId</code> eigenschap.</li><li><code>userFromToken</code>-De identiteit die wordt vertegenwoordigd door het opgegeven token gebruikt. Zie de <code>userToken</code> eigenschap.</li><li><code>clientCredentials</code>-De identiteit van de functie-app gebruikt.</li></ul>|
|**gebruikers-id** |Indien nodig en alleen als _identiteit_ is ingesteld op `userFromId`. Een gebruiker principal-ID die is gekoppeld aan een gebruiker eerder is geregistreerd.|
|**userToken**|Indien nodig en alleen als _identiteit_ is ingesteld op `userFromToken`. Een token geldig voor de functie-app. |
|**pad**|Vereist: het pad in OneDrive naar de Excel-werkmap.|
|**worksheetName**|Het werkblad waarin de tabel is gevonden.|
|**tableName**|De naam van de tabel. Als niet wordt opgegeven, wordt de inhoud van het werkblad worden gebruikt.|
|**updateType**|Vereist: het type wijziging aanbrengen in de tabel. Een van de volgende waarden kan zijn:<ul><li><code>update</code>-Vervangt de inhoud van de tabel in OneDrive.</li><li><code>append</code>-De nettolading van de toegevoegd aan het einde van de tabel in OneDrive door het maken van nieuwe rijen.</li></ul>|

<a name="excel-output-code"></a>
### <a name="using-an-excel-table-output-binding-from-code"></a>Binding van een code met behulp van een Excel-tabel uitvoer

De binding bevat de volgende typen aan .NET-functies:
- String []]
- Newtonsoft.Json.Linq.JObject
- Microsoft.Graph.WorkbookTable
- Aangepaste objecttypen (met behulp van de structurele modelbinding)

#### <a name="sample-adding-rows-to-an-excel-table"></a>Voorbeeld: Rijen toevoegen aan een Excel-tabel

Stel dat u hebt de volgende function.json die een HTTP-trigger met een Excel definieert-binding uitvoer:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "name": "newExcelRow",
      "type": "excel",
      "direction": "out",
      "identity": "userFromRequest",
      "updateType": "append",
      "path": "{query.workbook}",
      "tableName": "{query.table}"
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    }
  ],
  "disabled": false
}
```


De volgende C#-voorbeeld voegt een nieuwe rij aan de tabel (ervan uitgegaan dat er één kolom) op basis van de invoer van de query-tekenreeks:

```csharp
using System.Net;
using System.Text;

public static async Task Run(HttpRequest req, IAsyncCollector<object> newExcelRow, TraceWriter log)
{
    string input = req.Query
        .FirstOrDefault(q => string.Compare(q.Key, "text", true) == 0)
        .Value;
    await newExcelRow.AddAsync(new {
        Text = input
        // Add other properties for additional columns here
    });
    return;
}
```

De volgende JS voorbeeld voegt een nieuwe rij aan de tabel (ervan uitgegaan dat er één kolom) op basis van de invoer van de queryreeks. In de `function.json` hierboven wijzigen `$return` naar `res` eerste.

```js
module.exports = function (context, req) {
    context.bindings.newExcelRow = {
        text: req.query.text
        // Add other properties for additional columns here
    }
    context.done();
};
```




<a name="onedrive-input"></a>
## <a name="onedrive-file-input-binding"></a>OneDrive bestand invoer binding

Deze binding leest de inhoud van een bestand opgeslagen in OneDrive.

### <a name="configuring-a-onedrive-file-input-binding"></a>Configureren van een invoer binding voor OneDrive-bestand

Deze binding worden de volgende AAD-machtigingen vereist:
|Resource|Machtiging|
|--------|--------|
|Microsoft Graph|Gebruikersbestanden lezen|

De binding ondersteunt de volgende eigenschappen:

|Eigenschap|Beschrijving|
|--------|--------|
|**naam**|Vereist: de naam van de variabele gebruikt in de functiecode voor het bestand. Zie [binding van een code met behulp van een bestand OneDrive invoer](#onedrive-input-code).|
|**type**|Vereist - moet worden ingesteld op `onedrive`.|
|**richting**|Vereist - moet worden ingesteld op `in`.|
|**identiteit**|Vereist: de identiteit die wordt gebruikt voor de actie uitvoeren. Een van de volgende waarden kan zijn:<ul><li><code>userFromRequest</code>-Alleen geldig met [HTTP-trigger]. Maakt gebruik van de identiteit van de aanvragende gebruiker.</li><li><code>userFromId</code>-Maakt gebruik van de identiteit van een gebruiker eerder is geregistreerd met de opgegeven ID. Zie de <code>userId</code> eigenschap.</li><li><code>userFromToken</code>-De identiteit die wordt vertegenwoordigd door het opgegeven token gebruikt. Zie de <code>userToken</code> eigenschap.</li><li><code>clientCredentials</code>-De identiteit van de functie-app gebruikt.</li></ul>|
|**gebruikers-id** |Indien nodig en alleen als _identiteit_ is ingesteld op `userFromId`. Een gebruiker principal-ID die is gekoppeld aan een gebruiker eerder is geregistreerd.|
|**userToken**|Indien nodig en alleen als _identiteit_ is ingesteld op `userFromToken`. Een token geldig voor de functie-app. |
|**pad**|Vereist: het pad in OneDrive op het bestand.|

<a name="onedrive-input-code"></a>
### <a name="using-a-onedrive-file-input-binding-from-code"></a>Met behulp van een bestand OneDrive binding invoer van code

De binding bevat de volgende typen aan .NET-functies:
- Byte]
- Stroom
- Tekenreeks
- Microsoft.Graph.DriveItem

#### <a name="sample-reading-a-file-from-onedrive"></a>Voorbeeld: Een bestand lezen van OneDrive

Stel dat u hebt de volgende function.json die definieert een HTTP-trigger met een invoer binding OneDrive:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "name": "myOneDriveFile",
      "type": "onedrive",
      "direction": "in",
      "path": "{query.filename}",
      "identity": "userFromRequest"
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

De volgende C#-voorbeeld het bestand dat is opgegeven in de queryreeks leest en hiermee de duur:

```csharp
using System.Net;

public static void Run(HttpRequestMessage req, Stream myOneDriveFile, TraceWriter log)
{
    log.Info(myOneDriveFile.Length.ToString());
}
```

Het volgende voorbeeld met JS leest het bestand dat is opgegeven in de queryreeks en retourneert de lengte. In de `function.json` hierboven wijzigen `$return` naar `res` eerste.

```js
module.exports = function (context, req) {
    context.res = {
        body: context.bindings.myOneDriveFile.length
    };
    context.done();
};
```


<a name="onedrive-output"></a>
## <a name="onedrive-file-output-binding"></a>OneDrive bestand uitvoer binding

Deze binding Hiermee wijzigt u de inhoud van een bestand opgeslagen in OneDrive.

### <a name="configuring-a-onedrive-file-output-binding"></a>Configureren van een OneDrive uitvoer bestand binding

Deze binding worden de volgende AAD-machtigingen vereist:
|Resource|Machtiging|
|--------|--------|
|Microsoft Graph|Volledige toegang tot gebruikersbestanden|

De binding ondersteunt de volgende eigenschappen:

|Eigenschap|Beschrijving|
|--------|--------|
|**naam**|Vereist: de naam van de variabele gebruikt in de functiecode voor het bestand. Zie [binding van een code met behulp van een bestand OneDrive uitvoer](#onedrive-output-code).|
|**type**|Vereist - moet worden ingesteld op `onedrive`.|
|**richting**|Vereist - moet worden ingesteld op `out`.|
|**identiteit**|Vereist: de identiteit die wordt gebruikt voor de actie uitvoeren. Een van de volgende waarden kan zijn:<ul><li><code>userFromRequest</code>-Alleen geldig met [HTTP-trigger]. Maakt gebruik van de identiteit van de aanvragende gebruiker.</li><li><code>userFromId</code>-Maakt gebruik van de identiteit van een gebruiker eerder is geregistreerd met de opgegeven ID. Zie de <code>userId</code> eigenschap.</li><li><code>userFromToken</code>-De identiteit die wordt vertegenwoordigd door het opgegeven token gebruikt. Zie de <code>userToken</code> eigenschap.</li><li><code>clientCredentials</code>-De identiteit van de functie-app gebruikt.</li></ul>|
|**gebruikers-id** |Indien nodig en alleen als _identiteit_ is ingesteld op `userFromId`. Een gebruiker principal-ID die is gekoppeld aan een gebruiker eerder is geregistreerd.|
|**userToken**|Indien nodig en alleen als _identiteit_ is ingesteld op `userFromToken`. Een token geldig voor de functie-app. |
|**pad**|Vereist: het pad in OneDrive op het bestand.|

<a name="onedrive-output-code"></a>
### <a name="using-a-onedrive-file-output-binding-from-code"></a>Met behulp van een OneDrive uitvoer bestand binding van code

De binding bevat de volgende typen aan .NET-functies:
- Byte]
- Stroom
- Tekenreeks
- Microsoft.Graph.DriveItem

#### <a name="sample-writing-to-a-file-in-onedrive"></a>Voorbeeld: Schrijven naar een bestand in OneDrive

Stel dat u hebt de volgende function.json die een HTTP-trigger met een OneDrive definieert binding uitvoer:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "name": "myOneDriveFile",
      "type": "onedrive",
      "direction": "out",
      "path": "FunctionsTest.txt",
      "identity": "userFromRequest"
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

De volgende C#-voorbeeld tekst opgehaald uit de queryreeks en schrijft deze naar een tekstbestand (FunctionsTest.txt zoals gedefinieerd in de bovenstaande config) in de hoofdmap van de aanroeper OneDrive:

```csharp
using System.Net;
using System.Text;

public static async Task Run(HttpRequest req, TraceWriter log, Stream myOneDriveFile)
{
    string data = req.Query
        .FirstOrDefault(q => string.Compare(q.Key, "text", true) == 0)
        .Value;
    await myOneDriveFile.WriteAsync(Encoding.UTF8.GetBytes(data), 0, data.Length);
    return;
}
```
Het volgende voorbeeld met JS tekst opgehaald uit de queryreeks en schrijft deze naar een tekstbestand (FunctionsTest.txt zoals gedefinieerd in de config hierboven) in de hoofdmap van de aanroeper OneDrive. In de `function.json` hierboven wijzigen `$return` naar `res` eerste.

```js
module.exports = function (context, req) {
    context.bindings.myOneDriveFile = req.query.text;
    context.done();
};
```



<a name="outlook-output"></a>
## <a name="outlook-message-output-binding"></a>Outlook-bericht uitvoer binding

Verzendt een e-mailbericht via Outlook.

### <a name="configuring-an-outlook-message-output-binding"></a>Binding voor het configureren van een Outlook-bericht uitvoer

Deze binding worden de volgende AAD-machtigingen vereist:
|Resource|Machtiging|
|--------|--------|
|Microsoft Graph|E-mail verzenden als gebruiker|

De binding ondersteunt de volgende eigenschappen:

|Eigenschap|Beschrijving|
|--------|--------|
|**naam**|Vereist: de naam van de variabele in functiecode gebruikt voor het e-mailbericht. Zie [binding van een code met behulp van een Outlook-bericht uitvoer](#outlook-output-code).|
|**type**|Vereist - moet worden ingesteld op `outlook`.|
|**richting**|Vereist - moet worden ingesteld op `out`.|
|**identiteit**|Vereist: de identiteit die wordt gebruikt voor de actie uitvoeren. Een van de volgende waarden kan zijn:<ul><li><code>userFromRequest</code>-Alleen geldig met [HTTP-trigger]. Maakt gebruik van de identiteit van de aanvragende gebruiker.</li><li><code>userFromId</code>-Maakt gebruik van de identiteit van een gebruiker eerder is geregistreerd met de opgegeven ID. Zie de <code>userId</code> eigenschap.</li><li><code>userFromToken</code>-De identiteit die wordt vertegenwoordigd door het opgegeven token gebruikt. Zie de <code>userToken</code> eigenschap.</li><li><code>clientCredentials</code>-De identiteit van de functie-app gebruikt.</li></ul>|
|**gebruikers-id** |Indien nodig en alleen als _identiteit_ is ingesteld op `userFromId`. Een gebruiker principal-ID die is gekoppeld aan een gebruiker eerder is geregistreerd.|
|**userToken**|Indien nodig en alleen als _identiteit_ is ingesteld op `userFromToken`. Een token geldig voor de functie-app. |

<a name="outlook-output-code"></a>
### <a name="using-an-outlook-message-output-binding-from-code"></a>Binding van een code met behulp van een Outlook-bericht uitvoer

De binding bevat de volgende typen aan .NET-functies:
- Microsoft.Graph.Message
- Newtonsoft.Json.Linq.JObject
- Tekenreeks
- Aangepaste objecttypen (met behulp van de structurele modelbinding)

#### <a name="sample-sending-an-email-through-outlook"></a>Voorbeeld: Verzenden van een e-mail via Outlook

Stel dat u hebt de volgende function.json die een HTTP-trigger met een Outlook-bericht definieert binding uitvoer:

```json
{
  "bindings": [
    {
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "name": "message",
      "type": "outlook",
      "direction": "out",
      "identity": "userFromRequest"
    }
  ],
  "disabled": false
}
```

De volgende C#-voorbeeld wordt een e-mail van de aanroeper verzonden naar een ontvanger die is opgegeven in de query-tekenreeks:

```csharp
using System.Net;

public static void Run(HttpRequest req, out Message message, TraceWriter log)
{ 
    string emailAddress = req.Query["to"];
    message = new Message(){
        subject = "Greetings",
        body = "Sent from Azure Functions",
        recipient = new Recipient() {
            address = emailAddress
        }
    };
}

public class Message {
    public String subject {get; set;}
    public String body {get; set;}
    public Recipient recipient {get; set;}
}

public class Recipient {
    public String address {get; set;}
    public String name {get; set;}
}
```

Het volgende voorbeeld met JS verzendt een e-mailbericht van de aanroeper naar een ontvanger die is opgegeven in de query-tekenreeks:

```js
module.exports = function (context, req) {
    context.bindings.message = {
        subject: "Greetings",
        body: "Sent from Azure Functions with JavaScript",
        recipient: {
            address: req.query.to 
        } 
    };
    context.done();
};
```





<a name="webhooks"></a>
## <a name="microsoft-graph-webhook-bindings"></a>Microsoft Graph webhook bindingen

Webhooks kunt u om te reageren op gebeurtenissen in de Microsoft Graph. Ter ondersteuning van webhooks functies nodig zijn om te maken, vernieuwen en reageren op _webhook abonnementen_. Een volledige webhook-oplossing wordt een combinatie van de volgende bindingen vereisen:
- Een [Microsoft Graph webhook trigger](#webhook-trigger) kunt u om te reageren op een binnenkomende webhook.
- Een [abonnement voor Microsoft Graph-webhook invoer binding](#webhook-input) kunt u de lijst met bestaande abonnementen en eventueel vernieuwen.
- Een [abonnement voor Microsoft Graph-webhook uitvoer binding](#webhook-output) kunt u maken of verwijderen van de webhook-abonnementen.

De bindingen zelf hoeven niet alle AAD-machtigingen, maar u moet machtigingen die relevant zijn voor het brontype dat u reageren wilt op aanvragen. Zie voor een lijst waarvan machtigingen nodig voor elk resourcetype [abonnement machtigingen](https://developer.microsoft.com/en-us/graph/docs/api-reference/v1.0/api/subscription_post_subscriptions#permissions).

Zie voor meer informatie over webhooks [werken met webhooks in Microsoft Graph].





<a name="webhook-trigger"></a>
### <a name="microsoft-graph-webhook-trigger"></a>Microsoft Graph webhook trigger

Deze trigger kunt een functie om te reageren op een binnenkomende webhook van de Microsoft Graph. Elk exemplaar van deze trigger kunt reageren op een Microsoft Graph resourcetype.

#### <a name="configuring-a-microsoft-graph-webhook-trigger"></a>Configureren van een webhook Microsoft Graph trigger

De binding ondersteunt de volgende eigenschappen:

|Eigenschap|Beschrijving|
|--------|--------|
|**naam**|Vereist: de naam van de variabele in functiecode gebruikt voor het e-mailbericht. Zie [binding van een code met behulp van een Outlook-bericht uitvoer](#outlook-output-code).|
|**type**|Vereist - moet worden ingesteld op `graphWebhook`.|
|**richting**|Vereist - moet worden ingesteld op `trigger`.|
|**resourceType**|Vereist: de grafiek resource waarvoor deze functie moet reageren op webhooks. Een van de volgende waarden kan zijn:<ul><li><code>#Microsoft.Graph.Message</code>-wijzigingen in de Outlook-berichten.</li><li><code>#Microsoft.Graph.DriveItem</code>-wijzigingen in OneDrive hoofdmap items.</li><li><code>#Microsoft.Graph.Contact - changes made to personal contacts in Outlook.</code></li><li><code>#Microsoft.Graph.Event - changes made to Outlook calendar items.</code></li></ul>|

> [!Note]
> Een functie-app kan alleen hebben voor een functie die is geregistreerd voor een bepaalde `resourceType` waarde.

#### <a name="using-a-microsoft-graph-webhook-trigger-from-code"></a>Met behulp van een trigger Microsoft Graph webhook van code

De binding bevat de volgende typen aan .NET-functies:
- Microsoft Graph SDK-typen die relevant zijn voor de resource typt, bijvoorbeeld Microsoft.Graph.Message, Microsoft.Graph.DriveItem
- Aangepaste objecttypen (met behulp van de structurele modelbinding)

Zie voor voorbeelden van het gebruik van deze binding in code [Microsoft Graph webhook voorbeelden](#webhook-samples).



<a name="webhook-input"></a>
### <a name="microsoft-graph-webhook-subscription-input-binding"></a>Abonnement voor Microsoft Graph-webhook invoer binding

Deze binding kunt u voor het ophalen van de lijst met abonnementen die worden beheerd door deze functie-app. De binding worden gelezen uit de functie app-opslag en komt niet overeen met andere abonnementen gemaakt op basis van buiten de app.

#### <a name="configuring-a-microsoft-graph-webhook-subscription-input-binding"></a>Een invoer binding voor Microsoft Graph webhook-abonnement configureren

De binding ondersteunt de volgende eigenschappen:

|Eigenschap|Beschrijving|
|--------|--------|
|**naam**|Vereist: de naam van de variabele in functiecode gebruikt voor het e-mailbericht. Zie [binding van een code met behulp van een Outlook-bericht uitvoer](#outlook-output-code).|
|**type**|Vereist - moet worden ingesteld op `graphWebhookSubscription`.|
|**richting**|Vereist - moet worden ingesteld op `in`.|
|**filter**| Indien ingesteld op `userFromRequest`, en vervolgens de binding wordt alleen abonnementen die eigendom zijn van de aanvragende gebruiker ophalen (alleen geldig met [HTTP-trigger]).| 

#### <a name="using-a-microsoft-graph-webhook-subscription-input-binding-from-code"></a>Met behulp van een Microsoft Graph webhook abonnement invoer binding van code

De binding bevat de volgende typen aan .NET-functies:
- String]
- Matrices van aangepaste object-type
- Newtonsoft.Json.Linq.JObject]
- Microsoft.Graph.Subscription]

Zie voor voorbeelden van het gebruik van deze binding in code [Microsoft Graph webhook voorbeelden](#webhook-samples).


<a name="webhook-output"></a>
### <a name="microsoft-graph-webhook-subscription-output-binding"></a>Abonnement voor Microsoft Graph-webhook uitvoer binding

Deze binding kunt u maken, verwijderen en vernieuwen van de webhook-abonnementen in de Microsoft Graph.

#### <a name="configuring-a-microsoft-graph-webhook-subscription-output-binding"></a>Configureren van een webhook Microsoft Graph uitvoer abonnement binding

De binding ondersteunt de volgende eigenschappen:

|Eigenschap|Beschrijving|
|--------|--------|
|**naam**|Vereist: de naam van de variabele in functiecode gebruikt voor het e-mailbericht. Zie [binding van een code met behulp van een Outlook-bericht uitvoer](#outlook-output-code).|
|**type**|Vereist - moet worden ingesteld op `graphWebhookSubscription`.|
|**richting**|Vereist - moet worden ingesteld op `out`.|
|**identiteit**|Vereist: de identiteit die wordt gebruikt voor de actie uitvoeren. Een van de volgende waarden kan zijn:<ul><li><code>userFromRequest</code>-Alleen geldig met [HTTP-trigger]. Maakt gebruik van de identiteit van de aanvragende gebruiker.</li><li><code>userFromId</code>-Maakt gebruik van de identiteit van een gebruiker eerder is geregistreerd met de opgegeven ID. Zie de <code>userId</code> eigenschap.</li><li><code>userFromToken</code>-De identiteit die wordt vertegenwoordigd door het opgegeven token gebruikt. Zie de <code>userToken</code> eigenschap.</li><li><code>clientCredentials</code>-De identiteit van de functie-app gebruikt.</li></ul>|
|**gebruikers-id** |Indien nodig en alleen als _identiteit_ is ingesteld op `userFromId`. Een gebruiker principal-ID die is gekoppeld aan een gebruiker eerder is geregistreerd.|
|**userToken**|Indien nodig en alleen als _identiteit_ is ingesteld op `userFromToken`. Een token geldig voor de functie-app. |
|**actie**|Vereist: Hiermee geeft u de binding van de actie moet uitvoeren. Een van de volgende waarden kan zijn:<ul><li><code>create</code>-Registreert een nieuw abonnement.</li><li><code>delete</code>-Worden verwijderd van een opgegeven abonnement.</li><li><code>refresh</code>-Een opgegeven abonnement om te voorkomen dat deze verlopen vernieuwt.</li></ul>|
|**subscriptionResource**|Indien nodig en alleen als de _actie_ is ingesteld op `create`. Hiermee geeft u de Microsoft Graph-resource die wordt gecontroleerd op wijzigingen. Zie [werken met webhooks in Microsoft Graph]. |
|**changeType**|Indien nodig en alleen als de _actie_ is ingesteld op `create`. Geeft het type wijziging in de geabonneerde resource die wordt er een melding. De ondersteunde waarden zijn: `created`, `updated`, `deleted`. Meerdere waarden kunnen worden gecombineerd met behulp van een door komma's gescheiden lijst.|

#### <a name="using-a-microsoft-graph-webhook-subscription-output-binding-from-code"></a>Met behulp van een webhook Microsoft Graph uitvoer abonnement binding van code

De binding bevat de volgende typen aan .NET-functies:
- Tekenreeks
- Microsoft.Graph.Subscription

Zie voor voorbeelden van het gebruik van deze binding in code [Microsoft Graph webhook voorbeelden](#webhook-samples).
 
<a name="webhook-samples"></a>
### <a name="microsoft-graph-webhook-samples"></a>Microsoft Graph webhook-voorbeelden

#### <a name="sample-creating-a-subscription"></a>Voorbeeld: Een abonnement maken

Stel dat u hebt de volgende function.json die definieert een HTTP-trigger met de uitvoer van een abonnement binding met de actie maken:

```json
{
  "bindings": [
    {
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "type": "graphwebhook",
      "name": "clientState",
      "direction": "out",
      "action": "create",
      "listen": "me/mailFolders('Inbox')/messages",
      "changeTypes": [
        "created"
      ],
      "identity": "userFromRequest"
    },
    {
      "type": "http",
      "name": "$return",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

De volgende C#-voorbeeld registreert een webhook die deze functie-app ontvangt een melding wanneer de aanvragende gebruiker een Outlook-bericht ontvangt:

```csharp
using System;
using System.Net;

public static HttpResponseMessage run(HttpRequestMessage req, out string clientState, TraceWriter log)
{
  log.Info("C# HTTP trigger function processed a request.");
    clientState = Guid.NewGuid().ToString();
    return new HttpResponseMessage(HttpStatusCode.OK);
}
```

De volgende JS voorbeeld registreert een webhook die deze functie-app ontvangt een melding wanneer de aanvragende gebruiker een Outlook-bericht ontvangt:

```js
const uuidv4 = require('uuid/v4');

module.exports = function (context, req) {
    context.bindings.clientState = uuidv4();
    context.done();
};
```

#### <a name="sample-handling-notifications"></a>Voorbeeld: Meldingen verwerken

Stel dat u hebt de volgende function.json die grafiek webhook trigger definieert voor het afhandelen van Outlook-berichten:

```json
{
  "bindings": [
    {
      "name": "msg",
      "type": "GraphWebhookTrigger",
      "direction": "in",
      "resourceType": "#Microsoft.Graph.Message"
    }
  ],
  "disabled": false
}
```

De volgende C#-voorbeeld reageert op binnenkomende e-mailberichten en registreert de hoofdtekst van die worden verzonden door de ontvanger en 'Azure Functions' die in het onderwerp:

```csharp
#r "Microsoft.Graph"
using Microsoft.Graph;
using System.Net;

public static async Task Run(Message msg, TraceWriter log)  
{
    log.Info("Microsoft Graph webhook trigger function processed a request.");

    // Testable by sending oneself an email with the subject "Azure Functions" and some text body
    if (msg.Subject.Contains("Azure Functions") && msg.From.Equals(msg.Sender)) {
        log.Info($"Processed email: {msg.BodyPreview}");
    }
}
```

Het volgende voorbeeld met JS reageert op binnenkomende e-mailberichten en registreert de hoofdtekst van die worden verzonden door de ontvanger en 'Azure Functions' die in het onderwerp:

```js
module.exports = function (context) {
    context.log("Microsoft Graph webhook trigger function processed a request.");
    const msg = context.bindings.msg
    // Testable by sending oneself an email with the subject "Azure Functions" and some text body
    if((msg.subject.indexOf("Azure Functions") > -1) && (msg.from === msg.sender) ) {
      context.log(`Processed email: ${msg.bodyPreview}`);
    }
    context.done();
};
```

#### <a name="sample-deleting-subscriptions"></a>Voorbeeld: Abonnementen verwijderen

Stel dat u hebt de volgende function.json die definieert een HTTP-trigger met een abonnement invoer-binding en de uitvoer van een abonnement binding met de verwijderactie:

```json
{
  "bindings": [
    {
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "type": "graphWebhookSubscription",
      "name": "existingSubscriptions",
      "direction": "in",
      "filter": "userFromRequest"
    },
    {
      "type": "graphWebhookSubscription",
      "name": "subscriptionsToDelete",
      "direction": "out",
      "action": "delete",
      "identity": "userFromRequest"
    },
    {
      "type": "http",
      "name": "res",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

De volgende C#-voorbeeld haalt alle abonnementen voor de aanvragende gebruiker en worden ze verwijderd:

```csharp
using System.Net;

public static async Task Run(HttpRequest req, string[] existingSubscriptions, IAsyncCollector<string> subscriptionsToDelete, TraceWriter log)
{
    log.Info("C# HTTP trigger function processed a request.");
    foreach (var subscription in existingSubscriptions)
    {
        log.Info($"Deleting subscription {subscription}");
        await subscriptionsToDelete.AddAsync(subscription);
    }
}
```

Het volgende voorbeeld met JS haalt alle abonnementen voor de aanvragende gebruiker en worden ze verwijderd:

```js
module.exports = function (context, req) {
    const existing = context.bindings.existingSubscriptions;
    var toDelete = [];
    for (var i = 0; i < existing.length; i++) {
        context.log(`Deleting subscription ${existing[i]}`);
        todelete.push(existing[i]);
    }
    context.bindings.subscriptionsToDelete = toDelete;
    context.done();
};
```

#### <a name="sample-refreshing-subscriptions"></a>Voorbeeld: Vernieuwen van abonnementen

Er zijn twee benaderingen mogelijk abonnementen te vernieuwen:
- Gebruik de toepassings-id te bekommeren om alle abonnementen. Hiervoor toestemming van een Azure Active Directory-beheerder. Dit kan worden gebruikt door alle talen die worden ondersteund door Azure Functions.
- Gebruik van de identiteit die is gekoppeld aan elk abonnement door handmatig het binden van elke gebruiker-ID. Hiervoor moet een aantal aangepaste code uit te voeren van de binding. Dit kan alleen worden gebruikt door de .NET-functies.

Beide opties worden hieronder weergegeven.

**Met behulp van de toepassings-id**

Stel dat u hebt de volgende function.json die een timertrigger met een abonnement invoer binding abonnement uitvoer binding definieert, met behulp van de toepassings-id:

```json
{
  "bindings": [
    {
      "name": "myTimer",
      "type": "timerTrigger",
      "direction": "in",
      "schedule": "0 * * */2 * *"
    },
    {
      "type": "graphWebhookSubscription",
      "name": "existingSubscriptions",
      "direction": "in"
    },
    {
      "type": "graphWebhookSubscription",
      "name": "subscriptionsToRefresh",
      "direction": "out",
      "action": "refresh",
      "identity": "clientCredentials"
    }
  ],
  "disabled": false
}
```

De volgende C#-voorbeeld vernieuwt de abonnementen op een timer, met behulp van de toepassings-id:

```csharp
using System;

public static void Run(TimerInfo myTimer, string[] existingSubscriptions, ICollector<string> subscriptionsToRefresh, TraceWriter log)
{
    // This template uses application permissions and requires consent from an Azure Active Directory admin.
    // See https://go.microsoft.com/fwlink/?linkid=858780
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}");
    foreach (var subscription in existingSubscriptions)
    {
      log.Info($"Refreshing subscription {subscription}");
      subscriptionsToRefresh.Add(subscription);
    }
}
```

Het volgende voorbeeld met JS vernieuwt de abonnementen op een timer, met behulp van de toepassings-id:

```js
// This template uses application permissions and requires consent from an Azure Active Directory admin.
// See https://go.microsoft.com/fwlink/?linkid=858780

module.exports = function (context) {
    const existing = context.bindings.existingSubscriptions;
    var toRefresh = [];
    for (var i = 0; i < existing.length; i++) {
        context.log(`Deleting subscription ${existing[i]}`);
        todelete.push(existing[i]);
    }
    context.bindings.subscriptionsToRefresh = toRefresh;
    context.done();
};
```

**Met behulp van dynamische gebruikers-id 's**

Voor de optie alternatieve uitstellende binding aan het abonnement Stel dat u hebt de volgende function.json die een timertrigger definieert de binding met de functiecode invoer:

```json
{
  "bindings": [
    {
      "name": "myTimer",
      "type": "timerTrigger",
      "direction": "in",
      "schedule": "0 * * */2 * *"
    },
    {
      "type": "graphWebhookSubscription",
      "name": "existingSubscriptions",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

De volgende C#-voorbeeld wordt de abonnementen op een timer, met behulp van de identiteit van elke gebruiker maken in de uitvoer-binding met code vernieuwd:
```csharp
using System;

public static async Task Run(TimerInfo myTimer, UserSubscription[] existingSubscriptions, IBinder binder, TraceWriter log)
{
  log.Info($"C# Timer trigger function executed at: {DateTime.Now}");
    foreach (var subscription in existingSubscriptions)
    {
        // binding in code to allow dynamic identity
        using (var subscriptionsToRefresh = await binder.BindAsync<IAsyncCollector<string>>(
            new GraphWebhookSubscriptionAttribute() {
                Action = "refresh",
                Identity = "userFromId",
                UserId = subscription.UserId
            }
        ))
        {
            log.Info($"Refreshing subscription {subscription}");
            await subscriptionsToRefresh.AddAsync(subscription);
        }

    }
}

public class UserSubscription {
    public string UserId {get; set;}
    public string Id {get; set;}
}
```



[HTTP-trigger]: functions-bindings-http-webhook.md
[werken met webhooks in Microsoft Graph]: https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/webhooks
