---
title: Microsoft Graph-bindingen voor Azure Functions
description: Het gebruik van Microsoft Graph triggers en bindingen in de Azure Functions begrijpen.
services: functions
author: mattchenderson
manager: cfowler
editor: 
ms.service: functions
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 12/20/2017
ms.author: mahender
ms.openlocfilehash: 63b94c0a9b77a3f3a6fd394a130bf8f132d51369
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2018
---
# <a name="microsoft-graph-bindings-for-azure-functions"></a>Microsoft Graph-bindingen voor Azure Functions

In dit artikel wordt uitgelegd hoe configureren en werken met Microsoft Graph triggers en bindingen in de Azure Functions. Met deze, kunt u Azure Functions werken met gegevens, inzichten en gebeurtenissen van de [Microsoft Graph](https://graph.microsoft.io).

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

Microsoft Graph-bindingen zijn beschikbaar via _binding extensies_. Binding-uitbreidingen zijn optionele onderdelen naar de Azure Functions-runtime. Deze sectie wordt beschreven hoe u de token auth-uitbreidingen voor Microsoft Graph en instelt.

### <a name="enabling-functions-20-preview"></a>Inschakelen van functies 2.0 preview

Binding extensies zijn alleen beschikbaar voor Azure Functions 2.0 preview. 

Zie voor meer informatie over het instellen van een functie-app gebruiken preview 2.0-versie van de runtime van Functions [doel van de runtime versie 2.0](functions-versions.md#target-the-version-20-runtime).

### <a name="installing-the-extension"></a>Installeren van de extensie

Als u wilt installeren een uitbreiding van de Azure-portal, gaat u naar een sjabloon of binding ernaar verwijst. Maak een nieuwe functie en kies het 'Microsoft Graph'-scenario in het selectiescherm van de sjabloon. Selecteer een van de sjablonen in dit scenario. U kunt ook gaat u naar het tabblad 'Integreren' van een bestaande functie en selecteer een van de bindingen die in dit artikel.

In beide gevallen wordt een waarschuwing weergegeven waarmee de extensie te kunnen worden geïnstalleerd. Klik op **installeren** om op te halen van de extensie.

> [!Note] 
> Elke uitbreiding hoeft slechts één keer per functie-app worden geïnstalleerd. Een plan verbruik kan 10 minuten duren voordat het installatieproces van het in de portal.

Als u Visual Studio gebruikt, kunt u de uitbreidingen kunt krijgen door het installeren van deze NuGet-pakketten:
- [Microsoft.Azure.WebJobs.Extensions.AuthTokens](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.AuthTokens/)
- [Microsoft.Azure.WebJobs.Extensions.MicrosoftGraph](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.MicrosoftGraph/)

### <a name="configuring-authentication--authorization"></a>Configureren van verificatie / autorisatie

De bindingen die worden beschreven in dit artikel vereisen een identiteit moet worden gebruikt. Hierdoor kan Microsoft Graph voor het afdwingen van machtigingen en interacties controleren. De identiteit kan een gebruiker die toegang hebben tot uw toepassing of de toepassing zelf zijn. Instellen voor het configureren van deze identiteit [App Service-verificatie / autorisatie](https://docs.microsoft.com/azure/app-service/app-service-authentication-overview) met Azure Active Directory. U moet ook om de machtigingen van een resource die uw functies vereisen.

> [!Note] 
> De uitbreiding voor Microsoft Graph biedt alleen ondersteuning voor Azure AD-verificatie. Gebruikers moeten zich aanmelden met een account voor werk of school.

Als u de Azure-portal gebruikt, ziet u een waarschuwing onder de prompt voor het installeren van de extensie. De waarschuwing wordt u gevraagd te configureren van App Service-verificatie / autorisatie- en aanvraag geen machtigingen voor de sjabloon of de binding is vereist. Klik op **Azure AD configureren nu** of **machtigingen nu toevoegen** zo nodig.



<a name="token-input"></a>
## <a name="auth-token"></a>Auth-token

De binding auth token invoer een Azure AD-token ophalen voor een bepaalde bron en biedt dit toe aan uw code als een tekenreeks. De bron kan bestaan uit een waarvoor de toepassing machtigingen heeft. 

Deze sectie bevat de volgende subgedeelten:

* [Voorbeeld](#auth-token---example)
* [Kenmerken](#auth-token---attributes)
* [Configuratie](#auth-token---configuration)
* [Gebruik](#auth-token---usage)

### <a name="auth-token---example"></a>Auth-token - voorbeeld

Zie het voorbeeld taalspecifieke:

* [C# script (.csx)](#auth-token---c-script-example)
* [JavaScript](#auth-token---javascript-example)

#### <a name="auth-token---c-script-example"></a>Auth-token - voorbeeld van C#-script

Het volgende voorbeeld wordt informatie over gebruikersprofielen.

De *function.json* -bestand definieert een HTTP-trigger met een token invoer binding:

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

De C#-scriptcode het token gebruikt voor het maken van een HTTP-aanroep naar de Microsoft Graph en retourneert het resultaat:

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

#### <a name="auth-token---javascript-example"></a>Auth-token - JavaScript-voorbeeld

Het volgende voorbeeld wordt informatie over gebruikersprofielen.

De *function.json* -bestand definieert een HTTP-trigger met een token invoer binding:

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
      "name": "res",
      "type": "http",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

De JavaScript-code het token gebruikt voor het maken van een HTTP-aanroep naar de Microsoft Graph en retourneert het resultaat.

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

### <a name="auth-token---attributes"></a>Auth-token - kenmerken

In [C#-klassebibliotheken](functions-dotnet-class-library.md), gebruiken de [Token](https://github.com/Azure/azure-functions-microsoftgraph-extension/blob/master/src/TokenBinding/TokenAttribute.cs) kenmerk, die is gedefinieerd in NuGet-pakket [Microsoft.Azure.WebJobs.Extensions.AuthTokens](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.AuthTokens/).

### <a name="auth-token---configuration"></a>Auth-token - configuratie

De volgende tabel beschrijft de binding-configuratie-eigenschappen die u instelt in de *function.json* bestand en de `Token` kenmerk.

|de eigenschap Function.JSON | De kenmerkeigenschap |Beschrijving|
|---------|---------|----------------------|
|**naam**||Vereist: de naam van de variabele in functiecode gebruikt voor het auth-token. Zie [binding van een code met behulp van een token auth invoer](#token-input-code).|
|**type**||Vereist - moet worden ingesteld op `token`.|
|**richting**||Vereist - moet worden ingesteld op `in`.|
|**identiteit**|**Identity**|Vereist: de identiteit die wordt gebruikt voor de actie uitvoeren. Een van de volgende waarden kan zijn:<ul><li><code>userFromRequest</code>-Alleen geldig met [HTTP-trigger]. Maakt gebruik van de identiteit van de aanvragende gebruiker.</li><li><code>userFromId</code>-Maakt gebruik van de identiteit van een gebruiker eerder is geregistreerd met de opgegeven ID. Zie de <code>userId</code> eigenschap.</li><li><code>userFromToken</code>-De identiteit die wordt vertegenwoordigd door het opgegeven token gebruikt. Zie de <code>userToken</code> eigenschap.</li><li><code>clientCredentials</code>-De identiteit van de functie-app gebruikt.</li></ul>|
|**gebruikers-id**|**Gebruikers-id**  |Indien nodig en alleen als _identiteit_ is ingesteld op `userFromId`. Een gebruiker principal-ID die is gekoppeld aan een gebruiker eerder is geregistreerd.|
|**userToken**|**UserToken**|Indien nodig en alleen als _identiteit_ is ingesteld op `userFromToken`. Een token geldig voor de functie-app. |
|**Resource**|**resource**|Vereist: een Azure AD-bron-URL waarvoor het token wordt aangevraagd.|

<a name="token-input-code"></a>
### <a name="auth-token---usage"></a>Auth-token - gebruik

De binding zelf geen elke Azure AD-machtigingen nodig, maar afhankelijk van hoe het token wordt gebruikt, moet u mogelijk aanvullende machtigingen. Controleer de vereisten van de resource die u van plan bent voor toegang tot aan het token.

Het token wordt altijd geverifieerd op de code als een tekenreeks.




<a name="excel-input"></a>
## <a name="excel-input"></a>Excel-invoer

De invoer binding voor Excel-tabel leest de inhoud van een Excel-tabel die is opgeslagen in OneDrive.

Deze sectie bevat de volgende subgedeelten:

* [Voorbeeld](#excel-input---example)
* [Kenmerken](#excel-input---attributes)
* [Configuratie](#excel-input---configuration)
* [Gebruik](#excel-input---usage)

### <a name="excel-input---example"></a>Excel-invoer - voorbeeld

Zie het voorbeeld taalspecifieke:

* [C# script (.csx)](#excel-input---c-script-example)
* [JavaScript](#excel-input---javascript-example)

#### <a name="excel-input---c-script-example"></a>Excel invoer - voorbeeld van C#-script

De volgende *function.json* -bestand definieert een HTTP-trigger met een Excel-invoer binding:

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

De volgende C#-scriptcode leest de inhoud van de opgegeven tabel en retourneert ze aan de gebruiker:

```csharp
using System.Net;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Extensions.Primitives; 

public static IActionResult Run(HttpRequest req, string[][] excelTableData, TraceWriter log)
{
    return new OkObjectResult(excelTableData);
}
```

#### <a name="excel-input---javascript-example"></a>Excel invoer - JavaScript-voorbeeld

De volgende *function.json* -bestand definieert een HTTP-trigger met een Excel-invoer binding:

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
      "name": "res",
      "type": "http",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

De volgende JavaScript-code leest de inhoud van de opgegeven tabel en retourneert ze aan de gebruiker.

```js
module.exports = function (context, req) {
    context.res = {
        body: context.bindings.excelTableData
    };
    context.done();
};
```

### <a name="excel-input---attributes"></a>Excel invoer - kenmerken

In [C#-klassebibliotheken](functions-dotnet-class-library.md), gebruiken de [Excel](https://github.com/Azure/azure-functions-microsoftgraph-extension/blob/master/src/MicrosoftGraphBinding/Bindings/ExcelAttribute.cs) kenmerk, die is gedefinieerd in NuGet-pakket [Microsoft.Azure.WebJobs.Extensions.MicrosoftGraph](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.MicrosoftGraph/).

### <a name="excel-input---configuration"></a>Excel-invoer - configuratie

De volgende tabel beschrijft de binding-configuratie-eigenschappen die u instelt in de *function.json* bestand en de `Excel` kenmerk.

|de eigenschap Function.JSON | De kenmerkeigenschap |Beschrijving|
|---------|---------|----------------------|
|**naam**||Vereist: de naam van de variabele in functiecode gebruikt voor de Excel-tabel. Zie [binding van een code met behulp van een Excel-tabel invoer](#excel-input-code).|
|**type**||Vereist - moet worden ingesteld op `excel`.|
|**richting**||Vereist - moet worden ingesteld op `in`.|
|**identiteit**|**Identity**|Vereist: de identiteit die wordt gebruikt voor de actie uitvoeren. Een van de volgende waarden kan zijn:<ul><li><code>userFromRequest</code>-Alleen geldig met [HTTP-trigger]. Maakt gebruik van de identiteit van de aanvragende gebruiker.</li><li><code>userFromId</code>-Maakt gebruik van de identiteit van een gebruiker eerder is geregistreerd met de opgegeven ID. Zie de <code>userId</code> eigenschap.</li><li><code>userFromToken</code>-De identiteit die wordt vertegenwoordigd door het opgegeven token gebruikt. Zie de <code>userToken</code> eigenschap.</li><li><code>clientCredentials</code>-De identiteit van de functie-app gebruikt.</li></ul>|
|**gebruikers-id**|**Gebruikers-id**  |Indien nodig en alleen als _identiteit_ is ingesteld op `userFromId`. Een gebruiker principal-ID die is gekoppeld aan een gebruiker eerder is geregistreerd.|
|**userToken**|**UserToken**|Indien nodig en alleen als _identiteit_ is ingesteld op `userFromToken`. Een token geldig voor de functie-app. |
|**pad**|**Pad**|Vereist: het pad in OneDrive naar de Excel-werkmap.|
|**worksheetName**|**WorksheetName**|Het werkblad waarin de tabel is gevonden.|
|**tableName**|**TableName**|De naam van de tabel. Als niet wordt opgegeven, wordt de inhoud van het werkblad worden gebruikt.|

<a name="excel-input-code"></a>
### <a name="excel-input---usage"></a>Excel invoer - gebruik

Deze binding worden de volgende Azure AD-machtigingen vereist:
|Resource|Machtiging|
|--------|--------|
|Microsoft Graph|Gebruikersbestanden lezen|

De binding bevat de volgende typen aan .NET-functies:
- String []]
- Microsoft.Graph.WorkbookTable
- Aangepaste objecttypen (met behulp van de structurele modelbinding)










<a name="excel-output"></a>
## <a name="excel-output"></a>Excel-uitvoer

De Excel uitvoer binding Hiermee wijzigt u de inhoud van een Excel-tabel die is opgeslagen in OneDrive.

Deze sectie bevat de volgende subgedeelten:

* [Voorbeeld](#excel-output---example)
* [Kenmerken](#excel-output---attributes)
* [Configuratie](#excel-output---configuration)
* [Gebruik](#excel-output---usage)

### <a name="excel-output---example"></a>Excel-uitvoer - voorbeeld

Zie het voorbeeld taalspecifieke:

* [C# script (.csx)](#excel-output---c-script-example)
* [JavaScript](#excel-output---javascript-example)

#### <a name="excel-output---c-script-example"></a>Excel output - voorbeeld van C#-script

Het volgende voorbeeld worden de rijen toegevoegd aan een Excel-tabel.

De *function.json* -bestand definieert een HTTP-trigger met een Excel-binding uitvoer:

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

De C#-scriptcode voegt een nieuwe rij aan de tabel (ervan uitgegaan dat er één kolom) op basis van de invoer van de query-tekenreeks:

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

#### <a name="excel-output---javascript-example"></a>Excel output - JavaScript-voorbeeld

Het volgende voorbeeld worden de rijen toegevoegd aan een Excel-tabel.

De *function.json* -bestand definieert een HTTP-trigger met een Excel-binding uitvoer:

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
      "name": "res",
      "type": "http",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

De volgende JavaScript-code wordt toegevoegd een nieuwe rij aan de tabel (ervan uitgegaan dat er één kolom) op basis van de invoer van de queryreeks.

```js
module.exports = function (context, req) {
    context.bindings.newExcelRow = {
        text: req.query.text
        // Add other properties for additional columns here
    }
    context.done();
};
```

### <a name="excel-output---attributes"></a>Excel output - kenmerken

In [C#-klassebibliotheken](functions-dotnet-class-library.md), gebruiken de [Excel](https://github.com/Azure/azure-functions-microsoftgraph-extension/blob/master/src/MicrosoftGraphBinding/Bindings/ExcelAttribute.cs) kenmerk, die is gedefinieerd in NuGet-pakket [Microsoft.Azure.WebJobs.Extensions.MicrosoftGraph](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.MicrosoftGraph/).

### <a name="excel-output---configuration"></a>Excel-uitvoer - configuratie

De volgende tabel beschrijft de binding-configuratie-eigenschappen die u instelt in de *function.json* bestand en de `Excel` kenmerk.

|de eigenschap Function.JSON | De kenmerkeigenschap |Beschrijving|
|---------|---------|----------------------|
|**naam**||Vereist: de naam van de variabele in functiecode gebruikt voor het auth-token. Zie [binding van een code met behulp van een Excel-tabel uitvoer](#excel-output-code).|
|**type**||Vereist - moet worden ingesteld op `excel`.|
|**richting**||Vereist - moet worden ingesteld op `out`.|
|**identiteit**|**Identity**|Vereist: de identiteit die wordt gebruikt voor de actie uitvoeren. Een van de volgende waarden kan zijn:<ul><li><code>userFromRequest</code>-Alleen geldig met [HTTP-trigger]. Maakt gebruik van de identiteit van de aanvragende gebruiker.</li><li><code>userFromId</code>-Maakt gebruik van de identiteit van een gebruiker eerder is geregistreerd met de opgegeven ID. Zie de <code>userId</code> eigenschap.</li><li><code>userFromToken</code>-De identiteit die wordt vertegenwoordigd door het opgegeven token gebruikt. Zie de <code>userToken</code> eigenschap.</li><li><code>clientCredentials</code>-De identiteit van de functie-app gebruikt.</li></ul>|
|**Gebruikers-id** |**gebruikers-id** |Indien nodig en alleen als _identiteit_ is ingesteld op `userFromId`. Een gebruiker principal-ID die is gekoppeld aan een gebruiker eerder is geregistreerd.|
|**userToken**|**UserToken**|Indien nodig en alleen als _identiteit_ is ingesteld op `userFromToken`. Een token geldig voor de functie-app. |
|**pad**|**Pad**|Vereist: het pad in OneDrive naar de Excel-werkmap.|
|**worksheetName**|**WorksheetName**|Het werkblad waarin de tabel is gevonden.|
|**tableName**|**TableName**|De naam van de tabel. Als niet wordt opgegeven, wordt de inhoud van het werkblad worden gebruikt.|
|**updateType**|**UpdateType**|Vereist: het type wijziging aanbrengen in de tabel. Een van de volgende waarden kan zijn:<ul><li><code>update</code>-Vervangt de inhoud van de tabel in OneDrive.</li><li><code>append</code>-De nettolading van de toegevoegd aan het einde van de tabel in OneDrive door het maken van nieuwe rijen.</li></ul>|

<a name="excel-output-code"></a>
### <a name="excel-output---usage"></a>Excel output - gebruik

Deze binding worden de volgende Azure AD-machtigingen vereist:
|Resource|Machtiging|
|--------|--------|
|Microsoft Graph|Volledige toegang tot gebruikersbestanden|

De binding bevat de volgende typen aan .NET-functies:
- String []]
- Newtonsoft.Json.Linq.JObject
- Microsoft.Graph.WorkbookTable
- Aangepaste objecttypen (met behulp van de structurele modelbinding)





<a name="onedrive-input"></a>
## <a name="file-input"></a>Bestandsinvoer

De invoer binding OneDrive bestand leest de inhoud van een bestand opgeslagen in OneDrive.

Deze sectie bevat de volgende subgedeelten:

* [Voorbeeld](#file-input---example)
* [Kenmerken](#file-input---attributes)
* [Configuratie](#file-input---configuration)
* [Gebruik](#file-input---usage)

### <a name="file-input---example"></a>Bestand van de invoer - voorbeeld

Zie het voorbeeld taalspecifieke:

* [C# script (.csx)](#file-input---c-script-example)
* [JavaScript](#file-input---javascript-example)

#### <a name="file-input---c-script-example"></a>Invoer - bestand voorbeeld van C#-script

Het volgende voorbeeld wordt een bestand dat is opgeslagen in OneDrive gelezen.

De *function.json* -bestand definieert een HTTP-trigger met een invoer binding voor OneDrive-bestand:

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

De C#-scriptcode leest het bestand dat is opgegeven in de queryreeks en registreert de lengte:

```csharp
using System.Net;

public static void Run(HttpRequestMessage req, Stream myOneDriveFile, TraceWriter log)
{
    log.Info(myOneDriveFile.Length.ToString());
}
```

#### <a name="file-input---javascript-example"></a>Invoer - bestand JavaScript-voorbeeld

Het volgende voorbeeld wordt een bestand dat is opgeslagen in OneDrive gelezen.

De *function.json* -bestand definieert een HTTP-trigger met een invoer binding voor OneDrive-bestand:

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
      "name": "res",
      "type": "http",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

De volgende JavaScript-code leest het bestand dat is opgegeven in de queryreeks en retourneert de lengte.

```js
module.exports = function (context, req) {
    context.res = {
        body: context.bindings.myOneDriveFile.length
    };
    context.done();
};
```

### <a name="file-input---attributes"></a>Invoer - bestand kenmerken

In [C#-klassebibliotheken](functions-dotnet-class-library.md), gebruiken de [OneDrive](https://github.com/Azure/azure-functions-microsoftgraph-extension/blob/master/src/MicrosoftGraphBinding/Bindings/OneDriveAttribute.cs) kenmerk, die is gedefinieerd in NuGet-pakket [Microsoft.Azure.WebJobs.Extensions.MicrosoftGraph](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.MicrosoftGraph/).

### <a name="file-input---configuration"></a>Bestand van de invoer - configuratie

De volgende tabel beschrijft de binding-configuratie-eigenschappen die u instelt in de *function.json* bestand en de `OneDrive` kenmerk.

|de eigenschap Function.JSON | De kenmerkeigenschap |Beschrijving|
|---------|---------|----------------------|
|**naam**||Vereist: de naam van de variabele gebruikt in de functiecode voor het bestand. Zie [binding van een code met behulp van een bestand OneDrive invoer](#onedrive-input-code).|
|**type**||Vereist - moet worden ingesteld op `onedrive`.|
|**richting**||Vereist - moet worden ingesteld op `in`.|
|**identiteit**|**Identity**|Vereist: de identiteit die wordt gebruikt voor de actie uitvoeren. Een van de volgende waarden kan zijn:<ul><li><code>userFromRequest</code>-Alleen geldig met [HTTP-trigger]. Maakt gebruik van de identiteit van de aanvragende gebruiker.</li><li><code>userFromId</code>-Maakt gebruik van de identiteit van een gebruiker eerder is geregistreerd met de opgegeven ID. Zie de <code>userId</code> eigenschap.</li><li><code>userFromToken</code>-De identiteit die wordt vertegenwoordigd door het opgegeven token gebruikt. Zie de <code>userToken</code> eigenschap.</li><li><code>clientCredentials</code>-De identiteit van de functie-app gebruikt.</li></ul>|
|**gebruikers-id**|**Gebruikers-id**  |Indien nodig en alleen als _identiteit_ is ingesteld op `userFromId`. Een gebruiker principal-ID die is gekoppeld aan een gebruiker eerder is geregistreerd.|
|**userToken**|**UserToken**|Indien nodig en alleen als _identiteit_ is ingesteld op `userFromToken`. Een token geldig voor de functie-app. |
|**pad**|**Pad**|Vereist: het pad in OneDrive op het bestand.|

<a name="onedrive-input-code"></a>
### <a name="file-input---usage"></a>Invoer - bestand gebruik

Deze binding worden de volgende Azure AD-machtigingen vereist:
|Resource|Machtiging|
|--------|--------|
|Microsoft Graph|Gebruikersbestanden lezen|

De binding bevat de volgende typen aan .NET-functies:
- Byte]
- Stream
- tekenreeks
- Microsoft.Graph.DriveItem






<a name="onedrive-output"></a>
## <a name="file-output"></a>Bestandsuitvoer

Het bestand OneDrive uitvoer binding Hiermee wijzigt u de inhoud van een bestand opgeslagen in OneDrive.

Deze sectie bevat de volgende subgedeelten:

* [Voorbeeld](#file-output---example)
* [Kenmerken](#file-output---attributes)
* [Configuratie](#file-output---configuration)
* [Gebruik](#file-output---usage)

### <a name="file-output---example"></a>Bestand uitvoer - voorbeeld

Zie het voorbeeld taalspecifieke:

* [C# script (.csx)](#file-output---c-script-example)
* [JavaScript](#file-output---javascript-example)

#### <a name="file-output---c-script-example"></a>Output - bestand voorbeeld van C#-script

Het volgende voorbeeld worden geschreven naar een bestand dat is opgeslagen in OneDrive.

De *function.json* -bestand definieert een HTTP-trigger met een OneDrive binding uitvoer:

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

De C#-scriptcode tekst opgehaald uit de queryreeks en schrijft deze naar een tekstbestand (FunctionsTest.txt zoals gedefinieerd in het voorgaande voorbeeld) in de hoofdmap van de aanroeper OneDrive:

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

#### <a name="file-output---javascript-example"></a>Output - bestand JavaScript-voorbeeld

Het volgende voorbeeld worden geschreven naar een bestand dat is opgeslagen in OneDrive.

De *function.json* -bestand definieert een HTTP-trigger met een OneDrive binding uitvoer:

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
      "name": "res",
      "type": "http",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

De JavaScript-code opgehaald van de tekst van de queryreeks en schrijft deze naar een tekstbestand (FunctionsTest.txt zoals gedefinieerd in de bovenstaande config) in de hoofdmap van de aanroeper OneDrive.

```js
module.exports = function (context, req) {
    context.bindings.myOneDriveFile = req.query.text;
    context.done();
};
```

### <a name="file-output---attributes"></a>Output - bestand kenmerken

In [C#-klassebibliotheken](functions-dotnet-class-library.md), gebruiken de [OneDrive](https://github.com/Azure/azure-functions-microsoftgraph-extension/blob/master/src/MicrosoftGraphBinding/Bindings/OneDriveAttribute.cs) kenmerk, die is gedefinieerd in NuGet-pakket [Microsoft.Azure.WebJobs.Extensions.MicrosoftGraph](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.MicrosoftGraph/).

### <a name="file-output---configuration"></a>Bestand uitvoer - configuratie

De volgende tabel beschrijft de binding-configuratie-eigenschappen die u instelt in de *function.json* bestand en de `OneDrive` kenmerk.

|de eigenschap Function.JSON | De kenmerkeigenschap |Beschrijving|
|---------|---------|----------------------|
|**naam**||Vereist: de naam van de variabele gebruikt in de functiecode voor het bestand. Zie [binding van een code met behulp van een bestand OneDrive uitvoer](#onedrive-output-code).|
|**type**||Vereist - moet worden ingesteld op `onedrive`.|
|**richting**||Vereist - moet worden ingesteld op `out`.|
|**identiteit**|**Identity**|Vereist: de identiteit die wordt gebruikt voor de actie uitvoeren. Een van de volgende waarden kan zijn:<ul><li><code>userFromRequest</code>-Alleen geldig met [HTTP-trigger]. Maakt gebruik van de identiteit van de aanvragende gebruiker.</li><li><code>userFromId</code>-Maakt gebruik van de identiteit van een gebruiker eerder is geregistreerd met de opgegeven ID. Zie de <code>userId</code> eigenschap.</li><li><code>userFromToken</code>-De identiteit die wordt vertegenwoordigd door het opgegeven token gebruikt. Zie de <code>userToken</code> eigenschap.</li><li><code>clientCredentials</code>-De identiteit van de functie-app gebruikt.</li></ul>|
|**Gebruikers-id** |**gebruikers-id** |Indien nodig en alleen als _identiteit_ is ingesteld op `userFromId`. Een gebruiker principal-ID die is gekoppeld aan een gebruiker eerder is geregistreerd.|
|**userToken**|**UserToken**|Indien nodig en alleen als _identiteit_ is ingesteld op `userFromToken`. Een token geldig voor de functie-app. |
|**pad**|**Pad**|Vereist: het pad in OneDrive op het bestand.|

<a name="onedrive-output-code"></a>
#### <a name="file-output---usage"></a>Output - bestand gebruik

Deze binding worden de volgende Azure AD-machtigingen vereist:
|Resource|Machtiging|
|--------|--------|
|Microsoft Graph|Volledige toegang tot gebruikersbestanden|

De binding bevat de volgende typen aan .NET-functies:
- Byte]
- Stream
- tekenreeks
- Microsoft.Graph.DriveItem





<a name="outlook-output"></a>
## <a name="outlook-output"></a>Outlook-uitvoer

De Outlook-bericht uitvoer binding verzendt een e-mailbericht via Outlook.

Deze sectie bevat de volgende subgedeelten:

* [Voorbeeld](#outlook-output---example)
* [Kenmerken](#outlook-output---attributes)
* [Configuratie](#outlook-output---configuration)
* [Gebruik](#outlook-outnput---usage)

### <a name="outlook-output---example"></a>Uitvoer van de Outlook - voorbeeld

Zie het voorbeeld taalspecifieke:

* [C# script (.csx)](#outlook-output---c-script-example)
* [JavaScript](#outlook-output---javascript-example)

#### <a name="outlook-output---c-script-example"></a>Outlook output - voorbeeld van C#-script

Het volgende voorbeeld verzendt een e-mail via Outlook.

De *function.json* -bestand definieert een HTTP-trigger met een Outlook-bericht binding uitvoer:

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

De C#-scriptcode verzendt een e-mailbericht van de aanroeper naar een ontvanger die is opgegeven in de query-tekenreeks:

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

#### <a name="outlook-output---javascript-example"></a>Output - Outlook JavaScript-voorbeeld

Het volgende voorbeeld verzendt een e-mail via Outlook.

De *function.json* -bestand definieert een HTTP-trigger met een Outlook-bericht binding uitvoer:

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

De JavaScript-code verzendt een e-mailbericht van de aanroeper naar een ontvanger die is opgegeven in de query-tekenreeks:

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

### <a name="outlook-output---attributes"></a>Outlook output - kenmerken

In [C#-klassebibliotheken](functions-dotnet-class-library.md), gebruiken de [Outlook](https://github.com/Azure/azure-functions-microsoftgraph-extension/blob/master/src/MicrosoftGraphBinding/Bindings/OutlookAttribute.cs) kenmerk, die is gedefinieerd in NuGet-pakket [Microsoft.Azure.WebJobs.Extensions.MicrosoftGraph](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.MicrosoftGraph/).

### <a name="outlook-output---configuration"></a>Uitvoer van de Outlook - configuratie

De volgende tabel beschrijft de binding-configuratie-eigenschappen die u instelt in de *function.json* bestand en de `Outlook` kenmerk.

|de eigenschap Function.JSON | De kenmerkeigenschap |Beschrijving|
|---------|---------|----------------------|
|**naam**||Vereist: de naam van de variabele in functiecode gebruikt voor het e-mailbericht. Zie [binding van een code met behulp van een Outlook-bericht uitvoer](#outlook-output-code).|
|**type**||Vereist - moet worden ingesteld op `outlook`.|
|**richting**||Vereist - moet worden ingesteld op `out`.|
|**identiteit**|**Identity**|Vereist: de identiteit die wordt gebruikt voor de actie uitvoeren. Een van de volgende waarden kan zijn:<ul><li><code>userFromRequest</code>-Alleen geldig met [HTTP-trigger]. Maakt gebruik van de identiteit van de aanvragende gebruiker.</li><li><code>userFromId</code>-Maakt gebruik van de identiteit van een gebruiker eerder is geregistreerd met de opgegeven ID. Zie de <code>userId</code> eigenschap.</li><li><code>userFromToken</code>-De identiteit die wordt vertegenwoordigd door het opgegeven token gebruikt. Zie de <code>userToken</code> eigenschap.</li><li><code>clientCredentials</code>-De identiteit van de functie-app gebruikt.</li></ul>|
|**gebruikers-id**|**Gebruikers-id**  |Indien nodig en alleen als _identiteit_ is ingesteld op `userFromId`. Een gebruiker principal-ID die is gekoppeld aan een gebruiker eerder is geregistreerd.|
|**userToken**|**UserToken**|Indien nodig en alleen als _identiteit_ is ingesteld op `userFromToken`. Een token geldig voor de functie-app. |

<a name="outlook-output-code"></a>
### <a name="outlook-output---usage"></a>Outlook output - gebruik

Deze binding worden de volgende Azure AD-machtigingen vereist:
|Resource|Machtiging|
|--------|--------|
|Microsoft Graph|E-mail verzenden als gebruiker|

De binding bevat de volgende typen aan .NET-functies:
- Microsoft.Graph.Message
- Newtonsoft.Json.Linq.JObject
- tekenreeks
- Aangepaste objecttypen (met behulp van de structurele modelbinding)






## <a name="webhooks"></a>Webhooks

Webhooks kunt u om te reageren op gebeurtenissen in de Microsoft Graph. Ter ondersteuning van webhooks functies nodig zijn om te maken, vernieuwen en reageren op _webhook abonnementen_. Een volledige webhook-oplossing is een combinatie van de volgende bindingen vereist:
- Een [Microsoft Graph webhook trigger](#webhook-trigger) kunt u om te reageren op een binnenkomende webhook.
- Een [abonnement voor Microsoft Graph-webhook invoer binding](#webhook-input) kunt u de lijst met bestaande abonnementen en eventueel vernieuwen.
- Een [abonnement voor Microsoft Graph-webhook uitvoer binding](#webhook-output) kunt u maken of verwijderen van de webhook-abonnementen.

De bindingen zelf hoeven niet alle machtigingen die Azure AD, maar u moet machtigingen die relevant zijn voor het brontype dat u reageren wilt op aanvragen. Zie voor een lijst waarvan machtigingen nodig voor elk resourcetype [abonnement machtigingen](https://developer.microsoft.com/graph/docs/api-reference/v1.0/api/subscription_post_subscriptions#permissions).

Zie voor meer informatie over webhooks [werken met webhooks in Microsoft Graph].





## <a name="webhook-trigger"></a>Webhook-trigger

De Microsoft Graph webhook trigger kan een functie om te reageren op een binnenkomende webhook van de Microsoft Graph. Elk exemplaar van deze trigger kunt reageren op een Microsoft Graph resourcetype.

Deze sectie bevat de volgende subgedeelten:

* [Voorbeeld](#webhook-trigger---example)
* [Kenmerken](#webhook-trigger---attributes)
* [Configuratie](#webhook-trigger---configuration)
* [Gebruik](#webhook-trigger---usage)

### <a name="webhook-trigger---example"></a>Webhook-trigger - voorbeeld

Zie het voorbeeld taalspecifieke:

* [C# script (.csx)](#webhook-trigger---c-script-example)
* [JavaScript](#webhook-trigger---javascript-example)

#### <a name="webhook-trigger---c-script-example"></a>Webhook trigger - voorbeeld van C#-script

Het volgende voorbeeld verwerkt webhooks voor inkomende berichten van Outlook. Gebruik van een webhook activeert u [een abonnement maken](#webhook-output---example), en kunt u [vernieuwen van het abonnement](#webhook-subscription-refresh) om te voorkomen dat deze verloopt.

De *function.json* -bestand definieert de trigger van een webhook:

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

De C#-scriptcode reageert op binnenkomende e-mailberichten en registreert de hoofdtekst van die worden verzonden door de ontvanger en 'Azure Functions' die in het onderwerp:

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

#### <a name="webhook-trigger---javascript-example"></a>Webhook trigger - JavaScript-voorbeeld

Het volgende voorbeeld verwerkt webhooks voor inkomende berichten van Outlook. Gebruik van een webhook activeert u [een abonnement maken](#webhook-output---example), en kunt u [vernieuwen van het abonnement](#webhook-subscription-refresh) om te voorkomen dat deze verloopt.

De *function.json* -bestand definieert de trigger van een webhook:

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

De JavaScript-code reageert op binnenkomende e-mailberichten en registreert de hoofdtekst van die worden verzonden door de ontvanger en 'Azure Functions' die in het onderwerp:

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

### <a name="webhook-trigger---attributes"></a>Webhook trigger - kenmerken

In [C#-klassebibliotheken](functions-dotnet-class-library.md), gebruiken de [GraphWebHookTrigger](https://github.com/Azure/azure-functions-microsoftgraph-extension/blob/master/src/MicrosoftGraphBinding/Bindings/GraphWebHookTriggerAttribute.cs) kenmerk, die is gedefinieerd in NuGet-pakket [Microsoft.Azure.WebJobs.Extensions.MicrosoftGraph](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.MicrosoftGraph/).

### <a name="webhook-trigger---configuration"></a>Webhook-trigger - configuratie

De volgende tabel beschrijft de binding-configuratie-eigenschappen die u instelt in de *function.json* bestand en de `GraphWebHookTrigger` kenmerk.

|de eigenschap Function.JSON | De kenmerkeigenschap |Beschrijving|
|---------|---------|----------------------|
|**naam**||Vereist: de naam van de variabele in functiecode gebruikt voor het e-mailbericht. Zie [binding van een code met behulp van een Outlook-bericht uitvoer](#outlook-output-code).|
|**type**||Vereist - moet worden ingesteld op `graphWebhook`.|
|**richting**||Vereist - moet worden ingesteld op `trigger`.|
|**resourceType**|**ResourceType**|Vereist: de grafiek resource waarvoor deze functie moet reageren op webhooks. Een van de volgende waarden kan zijn:<ul><li><code>#Microsoft.Graph.Message</code>-wijzigingen in de Outlook-berichten.</li><li><code>#Microsoft.Graph.DriveItem</code>-wijzigingen in OneDrive hoofdmap items.</li><li><code>#Microsoft.Graph.Contact</code>-wijzigingen die zijn aangebracht aan persoonlijke contactpersonen in Outlook.</li><li><code>#Microsoft.Graph.Event</code>-wijzigingen in de Outlook-agenda-items.</li></ul>|

> [!Note]
> Een functie-app kan alleen hebben voor een functie die is geregistreerd voor een bepaalde `resourceType` waarde.

### <a name="webhook-trigger---usage"></a>Webhook trigger - gebruik

De binding bevat de volgende typen aan .NET-functies:
- Microsoft Graph SDK typen relevant zijn voor het resourcetype, zoals `Microsoft.Graph.Message` of `Microsoft.Graph.DriveItem`.
- Aangepaste objecttypen (met behulp van de structurele modelbinding)




<a name="webhook-input"></a>
## <a name="webhook-input"></a>Webhook invoer

De binding van Microsoft Graph-webhook-invoer kunt u voor het ophalen van de lijst met abonnementen die worden beheerd door deze functie-app. De binding leest van de functie app-opslag, zodat deze niet overeen met komt andere abonnementen gemaakt op basis van buiten de app.

Deze sectie bevat de volgende subgedeelten:

* [Voorbeeld](#webhook-input---example)
* [Kenmerken](#webhook-input---attributes)
* [Configuratie](#webhook-input---configuration)
* [Gebruik](#webhook-input---usage)

### <a name="webhook-input---example"></a>Webhook-invoer - voorbeeld

Zie het voorbeeld taalspecifieke:

* [C# script (.csx)](#webhook-input---c-script-example)
* [JavaScript](#webhook-input---javascript-example)

#### <a name="webhook-input---c-script-example"></a>Invoer - Webhook voorbeeld van C#-script

Het volgende voorbeeld alle abonnementen voor de aanvragende gebruiker opgehaald en worden ze verwijderd.

De *function.json* -bestand definieert een HTTP-trigger met een abonnement invoer-binding en de uitvoer van een abonnement binding die gebruikmaakt van de verwijderactie:

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

De C#-scriptcode opgehaald van de abonnementen en worden ze verwijderd:

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

#### <a name="webhook-input---javascript-example"></a>Invoer - Webhook JavaScript-voorbeeld

Het volgende voorbeeld alle abonnementen voor de aanvragende gebruiker opgehaald en worden ze verwijderd.

De *function.json* -bestand definieert een HTTP-trigger met een abonnement invoer-binding en de uitvoer van een abonnement binding die gebruikmaakt van de verwijderactie:

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

De JavaScript-code opgehaald van de abonnementen en worden ze verwijderd:

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

### <a name="webhook-input---attributes"></a>Invoer - Webhook kenmerken

In [C#-klassebibliotheken](functions-dotnet-class-library.md), gebruiken de [GraphWebHookSubscription](https://github.com/Azure/azure-functions-microsoftgraph-extension/blob/master/src/MicrosoftGraphBinding/Bindings/GraphWebHookSubscriptionAttribute.cs) kenmerk, die is gedefinieerd in NuGet-pakket [Microsoft.Azure.WebJobs.Extensions.MicrosoftGraph](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.MicrosoftGraph/).

### <a name="webhook-input---configuration"></a>Webhook-invoer - configuratie

De volgende tabel beschrijft de binding-configuratie-eigenschappen die u instelt in de *function.json* bestand en de `GraphWebHookSubscription` kenmerk.

|de eigenschap Function.JSON | De kenmerkeigenschap |Beschrijving|
|---------|---------|----------------------|
|**naam**||Vereist: de naam van de variabele in functiecode gebruikt voor het e-mailbericht. Zie [binding van een code met behulp van een Outlook-bericht uitvoer](#outlook-output-code).|
|**type**||Vereist - moet worden ingesteld op `graphWebhookSubscription`.|
|**richting**||Vereist - moet worden ingesteld op `in`.|
|**filter**|**Filter**| Indien ingesteld op `userFromRequest`, en vervolgens de binding wordt alleen abonnementen die eigendom zijn van de aanvragende gebruiker ophalen (alleen geldig met [HTTP-trigger]).| 

### <a name="webhook-input---usage"></a>Webhook invoer - gebruik

De binding bevat de volgende typen aan .NET-functies:
- String]
- Matrices van aangepaste object-type
- Newtonsoft.Json.Linq.JObject]
- Microsoft.Graph.Subscription]





## <a name="webhook-output"></a>Webhook-uitvoer

Het abonnement webhook uitvoer bindingen kunt u maken, verwijderen en vernieuwen van de webhook-abonnementen in de Microsoft Graph.

Deze sectie bevat de volgende subgedeelten:

* [Voorbeeld](#webhook-output---example)
* [Kenmerken](#webhook-output---attributes)
* [Configuratie](#webhook-output---configuration)
* [Gebruik](#webhook-output---usage)

### <a name="webhook-output---example"></a>Uitvoer van de Webhook - voorbeeld

Zie het voorbeeld taalspecifieke:

* [C# script (.csx)](#webhook-output---c-script-example)
* [JavaScript](#webhook-output---javascript-example)

#### <a name="webhook-output---c-script-example"></a>Output - Webhook voorbeeld van C#-script

Het volgende voorbeeld maakt een abonnement. U kunt [vernieuwen van het abonnement](#webhook-subscription-refresh) om te voorkomen dat deze verloopt.

De *function.json* -bestand definieert een HTTP-trigger met de uitvoer van een abonnement binding met de actie maken:

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

De C#-scriptcode registreert een webhook die deze functie-app ontvangt een melding wanneer de aanvragende gebruiker een Outlook-bericht ontvangt:

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

#### <a name="webhook-output---javascript-example"></a>Output - Webhook JavaScript-voorbeeld

Het volgende voorbeeld maakt een abonnement. U kunt [vernieuwen van het abonnement](#webhook-subscription-refresh) om te voorkomen dat deze verloopt.

De *function.json* -bestand definieert een HTTP-trigger met de uitvoer van een abonnement binding met de actie maken:

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

De JavaScript-code registreert een webhook die deze functie-app ontvangt een melding wanneer de aanvragende gebruiker een Outlook-bericht ontvangt:

```js
const uuidv4 = require('uuid/v4');

module.exports = function (context, req) {
    context.bindings.clientState = uuidv4();
    context.done();
};
```

### <a name="webhook-output---attributes"></a>Output - Webhook kenmerken

In [C#-klassebibliotheken](functions-dotnet-class-library.md), gebruiken de [GraphWebHookSubscription](https://github.com/Azure/azure-functions-microsoftgraph-extension/blob/master/src/MicrosoftGraphBinding/Bindings/GraphWebHookSubscriptionAttribute.cs) kenmerk, die is gedefinieerd in NuGet-pakket [Microsoft.Azure.WebJobs.Extensions.MicrosoftGraph](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.MicrosoftGraph/).

### <a name="webhook-output---configuration"></a>Uitvoer van de Webhook - configuratie

De volgende tabel beschrijft de binding-configuratie-eigenschappen die u instelt in de *function.json* bestand en de `GraphWebHookSubscription` kenmerk.

|de eigenschap Function.JSON | De kenmerkeigenschap |Beschrijving|
|---------|---------|----------------------|
|**naam**||Vereist: de naam van de variabele in functiecode gebruikt voor het e-mailbericht. Zie [binding van een code met behulp van een Outlook-bericht uitvoer](#outlook-output-code).|
|**type**||Vereist - moet worden ingesteld op `graphWebhookSubscription`.|
|**richting**||Vereist - moet worden ingesteld op `out`.|
|**identiteit**|**Identity**|Vereist: de identiteit die wordt gebruikt voor de actie uitvoeren. Een van de volgende waarden kan zijn:<ul><li><code>userFromRequest</code>-Alleen geldig met [HTTP-trigger]. Maakt gebruik van de identiteit van de aanvragende gebruiker.</li><li><code>userFromId</code>-Maakt gebruik van de identiteit van een gebruiker eerder is geregistreerd met de opgegeven ID. Zie de <code>userId</code> eigenschap.</li><li><code>userFromToken</code>-De identiteit die wordt vertegenwoordigd door het opgegeven token gebruikt. Zie de <code>userToken</code> eigenschap.</li><li><code>clientCredentials</code>-De identiteit van de functie-app gebruikt.</li></ul>|
|**gebruikers-id**|**Gebruikers-id**  |Indien nodig en alleen als _identiteit_ is ingesteld op `userFromId`. Een gebruiker principal-ID die is gekoppeld aan een gebruiker eerder is geregistreerd.|
|**userToken**|**UserToken**|Indien nodig en alleen als _identiteit_ is ingesteld op `userFromToken`. Een token geldig voor de functie-app. |
|**actie**|**Actie**|Vereist: Hiermee geeft u de binding van de actie moet uitvoeren. Een van de volgende waarden kan zijn:<ul><li><code>create</code>-Registreert een nieuw abonnement.</li><li><code>delete</code>-Worden verwijderd van een opgegeven abonnement.</li><li><code>refresh</code>-Een opgegeven abonnement om te voorkomen dat deze verlopen vernieuwt.</li></ul>|
|**subscriptionResource**|**SubscriptionResource**|Indien nodig en alleen als de _actie_ is ingesteld op `create`. Hiermee geeft u de Microsoft Graph-resource die wordt gecontroleerd op wijzigingen. Zie [werken met webhooks in Microsoft Graph]. |
|**changeType**|**ChangeType**|Indien nodig en alleen als de _actie_ is ingesteld op `create`. Geeft het type wijziging in de geabonneerde resource die wordt er een melding. De ondersteunde waarden zijn: `created`, `updated`, `deleted`. Meerdere waarden kunnen worden gecombineerd met behulp van een door komma's gescheiden lijst.|

### <a name="webhook-output---usage"></a>Webhook output - gebruik

De binding bevat de volgende typen aan .NET-functies:
- tekenreeks
- Microsoft.Graph.Subscription




<a name="webhook-examples"></a>
## <a name="webhook-subscription-refresh"></a>Webhook-abonnement vernieuwen

Er zijn twee benaderingen mogelijk abonnementen te vernieuwen:

- Gebruik de toepassings-id te bekommeren om alle abonnementen. Hiervoor toestemming van een Azure Active Directory-beheerder. Dit kan worden gebruikt door alle talen die worden ondersteund door Azure Functions.
- Gebruik van de identiteit die is gekoppeld aan elk abonnement door handmatig het binden van elke gebruiker-ID. Hiervoor moet een aantal aangepaste code uit te voeren van de binding. Dit kan alleen worden gebruikt door de .NET-functies.

Deze sectie bevat een voorbeeld voor elk van deze methoden:

* [Voorbeeld van App-id](#webhook-subscription-refresh---app-identity-example)
* [Voorbeeld van de identiteit van gebruiker](#webhook-subscription-refresh---user-identity-example)

### <a name="webhook-subscription-refresh---app-identity-example"></a>Voorbeeld van app-identity-Webhook abonnement vernieuwen-

Zie het voorbeeld taalspecifieke:

* [C# script (.csx)](#app-identity-refresh---c-script-example)
* [JavaScript](#app-identity-refresh---javascript-example)

### <a name="app-identity-refresh---c-script-example"></a>App voor vernieuwen identity - voorbeeld van C#-script

Het volgende voorbeeld wordt de identiteit van een abonnement te vernieuwen.

De *function.json* definieert een timertrigger met een abonnement invoer binding- en een abonnement binding uitvoer:

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

De C#-scriptcode vernieuwt de abonnementen:

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

### <a name="app-identity-refresh---c-script-example"></a>App voor vernieuwen identity - voorbeeld van C#-script

Het volgende voorbeeld wordt de identiteit van een abonnement te vernieuwen.

De *function.json* definieert een timertrigger met een abonnement invoer binding- en een abonnement binding uitvoer:

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

De JavaScript-code wordt vernieuwd de abonnementen:

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

### <a name="webhook-subscription-refresh---user-identity-example"></a>Webhook abonnement vernieuwen - gebruiker identiteit voorbeeld

Het volgende voorbeeld wordt een identiteit van de gebruiker een abonnement te vernieuwen.

De *function.json* bestand definieert een timertrigger en past de abonnement-invoer-binding met de functiecode omleiding:

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

De C#-scriptcode vernieuwt de abonnementen en maakt de uitvoer-binding in code, met behulp van de identiteit van elke gebruiker:

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

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over Azure functions triggers en bindingen](functions-triggers-bindings.md)

[HTTP-trigger]: functions-bindings-http-webhook.md
[werken met webhooks in Microsoft Graph]: https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/webhooks
