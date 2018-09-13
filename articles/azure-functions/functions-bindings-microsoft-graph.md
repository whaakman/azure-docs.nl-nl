---
title: Microsoft Graph-bindingen voor Azure Functions
description: Over het gebruik van Microsoft Graph-triggers en bindingen in Azure Functions.
services: functions
author: mattchenderson
manager: jeconnoc
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 12/20/2017
ms.author: mahender
ms.openlocfilehash: 3d6d4f2e3d89e1d8abf647b21e35fcdfec020b1d
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "44722262"
---
# <a name="microsoft-graph-bindings-for-azure-functions"></a>Microsoft Graph-bindingen voor Azure Functions

In dit artikel wordt uitgelegd hoe u kunt configureren en werken met Microsoft Graph-triggers en bindingen in Azure Functions. Met deze, kunt u Azure Functions gebruiken om te werken met gegevens, inzichten en gebeurtenissen van de [Microsoft Graph](https://graph.microsoft.io).

De Microsoft Graph-extensie biedt de volgende bindingen:
- Een [-verificatietoken Invoerbinding](#token-input) kunt u communiceren met alle Microsoft Graph-API.
- Een [Excel-tabel Invoerbinding](#excel-input) kunt u gegevens uit Excel niet lezen.
- Een [Excel-tabel-Uitvoerbinding](#excel-output) kunt u Excel-gegevens wijzigen.
- Een [OneDrive-bestand Invoerbinding](#onedrive-input) kunt u bestanden lezen uit OneDrive.
- Een [OneDrive-bestand-Uitvoerbinding](#onedrive-output) kunt u om te schrijven naar bestanden in OneDrive.
- Een [Outlook-bericht-Uitvoerbinding](#outlook-output) kunt u e-mailbericht via Outlook verzenden.
- Een verzameling van [Microsoft Graph-webhook-triggers en bindingen](#webhooks) kunt u reageren op gebeurtenissen uit de Microsoft Graph.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

> [!Note]
> Microsoft Graph-bindingen zijn momenteel in Preview-versie van Azure Functions versie 2.x. Ze worden niet ondersteund in functies versie 1.x.

## <a name="packages"></a>Pakketten

De Invoerbinding auth-token is opgegeven in de [Microsoft.Azure.WebJobs.Extensions.AuthTokens](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.AuthTokens/) NuGet-pakket. De Microsoft Graph-bindingen zijn opgegeven in de [Microsoft.Azure.WebJobs.Extensions.MicrosoftGraph](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.MicrosoftGraph/) pakket. Broncode voor de pakketten is in de [azure-functions-microsoftgraph-extension](https://github.com/Azure/azure-functions-microsoftgraph-extension/) GitHub-opslagplaats.

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2.md)]

## <a name="setting-up-the-extensions"></a>Instellen van de extensies

Microsoft Graph-bindingen zijn beschikbaar via _binding extensies_. Bindinguitbreidingen zijn optionele componenten in de Azure Functions-runtime. Deze sectie wordt beschreven hoe u het instellen van de Microsoft Graph en auth-token extensies.

### <a name="enabling-functions-20-preview"></a>Functions 2.0 preview inschakelen

Binding-extensies zijn alleen beschikbaar voor Azure Functions 2.0 preview. 

Zie voor meer informatie over het instellen van een functie-app met de Preview-versie 2.0-versie van de Functions-runtime [hoe gericht op versies van Azure Functions-runtime](set-runtime-version.md).

### <a name="installing-the-extension"></a>De extensie installeren

Als u wilt een extensie installeren vanuit de Azure-portal, gaat u naar een sjabloon of binding die hiernaar verwijst. Maak een nieuwe functie en in het selectiescherm van de sjabloon, kies het scenario "Microsoft Graph". Selecteer een van de sjablonen in dit scenario. U kunt ook het geval is, gaat u naar het tabblad "Integratie" van een bestaande functie en selecteer een van de bindingen die in dit artikel besproken.

In beide gevallen wordt een waarschuwing weergegeven dat aangeeft dat de extensie worden geïnstalleerd. Klik op **installeren** om op te halen van de extensie. Elke uitbreiding moet slechts één keer per functie-app worden geïnstalleerd. 

> [!Note] 
> In de portal tijdens het installatieproces kan maximaal tien minuten duren in een verbruiksabonnement.

Als u Visual Studio gebruikt, kunt u de extensies krijgen door het installeren van [de NuGet-pakketten die eerder in dit artikel worden vermeld](#packages).

### <a name="configuring-authentication--authorization"></a>Configureren van verificatie / autorisatie

De bindingen die worden beschreven in dit artikel is vereist voor een identiteit die moet worden gebruikt. Hiermee wordt de Microsoft Graph te dwingen, machtigingen en interacties controleren. De identiteit kan een gebruiker toegang tot uw toepassing of de toepassing zelf zijn. Instellen voor het configureren van deze identiteit [App Service-verificatie / autorisatie](https://docs.microsoft.com/azure/app-service/app-service-authentication-overview) met Azure Active Directory. U moet ook alle machtigingen voor resources die uw functies vereisen aanvragen.

> [!Note] 
> De Microsoft Graph-extensie biedt alleen ondersteuning voor Azure AD-verificatie. Gebruikers moeten zich aanmelden met een account voor werk- of schoolaccount.

Als u de Azure-portal gebruikt, ziet u een waarschuwing weergegeven onder de prompt voor het installeren van de extensie. De waarschuwing vraagt u het configureren van App Service-verificatie / autorisatie en de aanvraag machtigingen voor de sjabloon of binding is vereist. Klik op **configureren Azure AD is nu** of **machtigingen nu toevoegen** indien van toepassing.



<a name="token-input"></a>
## <a name="auth-token"></a>Auth-token

De Invoerbinding auth-token wordt een Azure AD-token voor een bepaalde resource en stelt deze toe aan uw code als een tekenreeks. De bron kan bestaan uit een waarvoor de toepassing heeft machtigingen. 

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

Het volgende voorbeeld wordt informatie uit gebruikersprofielen.

De *function.json* -bestand definieert een HTTP-trigger met een token Invoerbinding:

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

De C#-scriptcode gebruikt het token voor het maken van een HTTP-aanroep naar de Microsoft Graph en retourneert het resultaat:

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

Het volgende voorbeeld wordt informatie uit gebruikersprofielen.

De *function.json* -bestand definieert een HTTP-trigger met een token Invoerbinding:

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

De JavaScript-code gebruikt het token voor het maken van een HTTP-aanroep naar de Microsoft Graph en retourneert het resultaat.

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

In [C#-klassebibliotheken](functions-dotnet-class-library.md), gebruikt u de [Token](https://github.com/Azure/azure-functions-microsoftgraph-extension/blob/master/src/TokenBinding/TokenAttribute.cs) kenmerk.

### <a name="auth-token---configuration"></a>Auth-token - configuratie

De volgende tabel beschrijft de binding configuratie-eigenschappen die u instelt in de *function.json* bestand en de `Token` kenmerk.

|de eigenschap Function.JSON | De kenmerkeigenschap |Beschrijving|
|---------|---------|----------------------|
|**De naam**||Vereist: de naam van de variabele in functiecode gebruikt voor het verificatietoken. Zie [met behulp van een verificatietoken Invoerbinding vanuit code](#token-input-code).|
|**type**||Vereist: moet worden ingesteld op `token`.|
|**direction**||Vereist: moet worden ingesteld op `in`.|
|**Identiteit**|**Identity**|Vereist: de identiteit die wordt gebruikt de actie uit te voeren. Een van de volgende waarden kan zijn:<ul><li><code>userFromRequest</code> -Alleen geldig voor [HTTP-trigger]. Maakt gebruik van de identiteit van de aanroepende gebruiker.</li><li><code>userFromId</code> -Maakt gebruik van de identiteit van een eerder aangemelde gebruiker met de opgegeven ID. Zie de <code>userId</code> eigenschap.</li><li><code>userFromToken</code> -Maakt gebruik van de identiteit die wordt vertegenwoordigd door het opgegeven token. Zie de <code>userToken</code> eigenschap.</li><li><code>clientCredentials</code> -Maakt gebruik van de identiteit van de functie-app.</li></ul>|
|**Gebruikers-id**|**Gebruikers-id**  |Vereist als en alleen als _identiteit_ is ingesteld op `userFromId`. Een gebruiker principal-ID die is gekoppeld aan een eerder aangemelde gebruiker.|
|**userToken**|**UserToken**|Vereist als en alleen als _identiteit_ is ingesteld op `userFromToken`. Een token geldig is voor de functie-app. |
|**Resource**|**resource**|Vereist: de URL van een Azure AD-resource waarvoor het token wordt aangevraagd.|

<a name="token-input-code"></a>
### <a name="auth-token---usage"></a>Auth-token - gebruik

De binding zelf geen eventuele Azure AD-machtigingen vereist, maar afhankelijk van hoe het token wordt gebruikt, moet u mogelijk aanvullende machtigingen. Controleer de vereisten van de resource die u wilt openen met het token.

Het token wordt altijd weergegeven voor de code als een tekenreeks.




<a name="excel-input"></a>
## <a name="excel-input"></a>Excel-invoer

De binding van Excel-tabel-invoer leest de inhoud van een Excel-tabel die zijn opgeslagen in OneDrive.

Deze sectie bevat de volgende subgedeelten:

* [Voorbeeld](#excel-input---example)
* [Kenmerken](#excel-input---attributes)
* [Configuratie](#excel-input---configuration)
* [Gebruik](#excel-input---usage)

### <a name="excel-input---example"></a>Excel-input - voorbeeld

Zie het voorbeeld taalspecifieke:

* [C# script (.csx)](#excel-input---c-script-example)
* [JavaScript](#excel-input---javascript-example)

#### <a name="excel-input---c-script-example"></a>Excel invoer - voorbeeld van C#-script

De volgende *function.json* -bestand definieert een HTTP-trigger met een Excel-Invoerbinding:

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

De volgende C#-scriptcode leest van de inhoud van de opgegeven tabel en retourneert ze aan de gebruiker:

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

De volgende *function.json* -bestand definieert een HTTP-trigger met een Excel-Invoerbinding:

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

In [C#-klassebibliotheken](functions-dotnet-class-library.md), gebruikt u de [Excel](https://github.com/Azure/azure-functions-microsoftgraph-extension/blob/master/src/MicrosoftGraphBinding/Bindings/ExcelAttribute.cs) kenmerk.

### <a name="excel-input---configuration"></a>Excel-input - configuratie

De volgende tabel beschrijft de binding configuratie-eigenschappen die u instelt in de *function.json* bestand en de `Excel` kenmerk.

|de eigenschap Function.JSON | De kenmerkeigenschap |Beschrijving|
|---------|---------|----------------------|
|**De naam**||Vereist: de naam van de variabele die wordt gebruikt in de functiecode voor de Excel-tabel. Zie [Invoerbinding met behulp van een Excel-tabel vanuit code](#excel-input-code).|
|**type**||Vereist: moet worden ingesteld op `excel`.|
|**direction**||Vereist: moet worden ingesteld op `in`.|
|**Identiteit**|**Identity**|Vereist: de identiteit die wordt gebruikt de actie uit te voeren. Een van de volgende waarden kan zijn:<ul><li><code>userFromRequest</code> -Alleen geldig voor [HTTP-trigger]. Maakt gebruik van de identiteit van de aanroepende gebruiker.</li><li><code>userFromId</code> -Maakt gebruik van de identiteit van een eerder aangemelde gebruiker met de opgegeven ID. Zie de <code>userId</code> eigenschap.</li><li><code>userFromToken</code> -Maakt gebruik van de identiteit die wordt vertegenwoordigd door het opgegeven token. Zie de <code>userToken</code> eigenschap.</li><li><code>clientCredentials</code> -Maakt gebruik van de identiteit van de functie-app.</li></ul>|
|**Gebruikers-id**|**Gebruikers-id**  |Vereist als en alleen als _identiteit_ is ingesteld op `userFromId`. Een gebruiker principal-ID die is gekoppeld aan een eerder aangemelde gebruiker.|
|**userToken**|**UserToken**|Vereist als en alleen als _identiteit_ is ingesteld op `userFromToken`. Een token geldig is voor de functie-app. |
|**Pad**|**Pad**|Vereist: het pad naar de Excel-werkmap in OneDrive.|
|**worksheetName**|**WorksheetName**|Het werkblad waarin de tabel is gevonden.|
|**Tabelnaam**|**Tabelnaam**|De naam van de tabel. Indien niet opgegeven, wordt de inhoud van het werkblad worden gebruikt.|

<a name="excel-input-code"></a>
### <a name="excel-input---usage"></a>Excel invoer - gebruik

Deze binding hoort vereist de volgende Azure AD-machtigingen:
|Resource|Machtiging|
|--------|--------|
|Microsoft Graph|Gebruikersbestanden lezen|

De binding wordt aangegeven dat de volgende typen .NET-functies:
- String []]
- Microsoft.Graph.WorkbookTable
- Aangepaste objecttypen (met behulp van de binding van de structurele model)










<a name="excel-output"></a>
## <a name="excel-output"></a>Excel-uitvoer

De Excel Uitvoerbinding Hiermee wijzigt u de inhoud van een Excel-tabel die zijn opgeslagen in OneDrive.

Deze sectie bevat de volgende subgedeelten:

* [Voorbeeld](#excel-output---example)
* [Kenmerken](#excel-output---attributes)
* [Configuratie](#excel-output---configuration)
* [Gebruik](#excel-output---usage)

### <a name="excel-output---example"></a>Excel-uitvoer - voorbeeld

Zie het voorbeeld taalspecifieke:

* [C# script (.csx)](#excel-output---c-script-example)
* [JavaScript](#excel-output---javascript-example)

#### <a name="excel-output---c-script-example"></a>Excel uitvoer - voorbeeld van C#-script

Het volgende voorbeeld worden de rijen toegevoegd aan een Excel-tabel.

De *function.json* -bestand definieert een HTTP-trigger met een Excel-Uitvoerbinding:

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

De code van het C# script voegt een nieuwe rij aan de tabel (ervan uitgaande dat één kolom worden) op basis van invoer van de query-tekenreeks:

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

#### <a name="excel-output---javascript-example"></a>Excel uitvoer - JavaScript-voorbeeld

Het volgende voorbeeld worden de rijen toegevoegd aan een Excel-tabel.

De *function.json* -bestand definieert een HTTP-trigger met een Excel-Uitvoerbinding:

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

De volgende JavaScript-code voegt een nieuwe rij aan de tabel (ervan uitgaande dat één kolom worden) op basis van invoer van de query-tekenreeks.

```js
module.exports = function (context, req) {
    context.bindings.newExcelRow = {
        text: req.query.text
        // Add other properties for additional columns here
    }
    context.done();
};
```

### <a name="excel-output---attributes"></a>Excel uitvoer - kenmerken

In [C#-klassebibliotheken](functions-dotnet-class-library.md), gebruikt u de [Excel](https://github.com/Azure/azure-functions-microsoftgraph-extension/blob/master/src/MicrosoftGraphBinding/Bindings/ExcelAttribute.cs) kenmerk.

### <a name="excel-output---configuration"></a>Excel-uitvoer - configuratie

De volgende tabel beschrijft de binding configuratie-eigenschappen die u instelt in de *function.json* bestand en de `Excel` kenmerk.

|de eigenschap Function.JSON | De kenmerkeigenschap |Beschrijving|
|---------|---------|----------------------|
|**De naam**||Vereist: de naam van de variabele in functiecode gebruikt voor het verificatietoken. Zie [met behulp van een Excel-tabel-Uitvoerbinding vanuit code](#excel-output-code).|
|**type**||Vereist: moet worden ingesteld op `excel`.|
|**direction**||Vereist: moet worden ingesteld op `out`.|
|**Identiteit**|**Identity**|Vereist: de identiteit die wordt gebruikt de actie uit te voeren. Een van de volgende waarden kan zijn:<ul><li><code>userFromRequest</code> -Alleen geldig voor [HTTP-trigger]. Maakt gebruik van de identiteit van de aanroepende gebruiker.</li><li><code>userFromId</code> -Maakt gebruik van de identiteit van een eerder aangemelde gebruiker met de opgegeven ID. Zie de <code>userId</code> eigenschap.</li><li><code>userFromToken</code> -Maakt gebruik van de identiteit die wordt vertegenwoordigd door het opgegeven token. Zie de <code>userToken</code> eigenschap.</li><li><code>clientCredentials</code> -Maakt gebruik van de identiteit van de functie-app.</li></ul>|
|**Gebruikers-id** |**Gebruikers-id** |Vereist als en alleen als _identiteit_ is ingesteld op `userFromId`. Een gebruiker principal-ID die is gekoppeld aan een eerder aangemelde gebruiker.|
|**userToken**|**UserToken**|Vereist als en alleen als _identiteit_ is ingesteld op `userFromToken`. Een token geldig is voor de functie-app. |
|**Pad**|**Pad**|Vereist: het pad naar de Excel-werkmap in OneDrive.|
|**worksheetName**|**WorksheetName**|Het werkblad waarin de tabel is gevonden.|
|**Tabelnaam**|**Tabelnaam**|De naam van de tabel. Indien niet opgegeven, wordt de inhoud van het werkblad worden gebruikt.|
|**updateType**|**updateType**|Vereist: het type wijziging aanbrengen in de tabel. Een van de volgende waarden kan zijn:<ul><li><code>update</code> -Vervangt de inhoud van de tabel in OneDrive.</li><li><code>append</code> -De nettolading van de toegevoegd aan het einde van de tabel in OneDrive door het maken van nieuwe rijen.</li></ul>|

<a name="excel-output-code"></a>
### <a name="excel-output---usage"></a>Excel uitvoer - gebruik

Deze binding hoort vereist de volgende Azure AD-machtigingen:
|Resource|Machtiging|
|--------|--------|
|Microsoft Graph|Volledige toegang tot gebruikersbestanden|

De binding wordt aangegeven dat de volgende typen .NET-functies:
- String []]
- Newtonsoft.Json.Linq.JObject
- Microsoft.Graph.WorkbookTable
- Aangepaste objecttypen (met behulp van de binding van de structurele model)





<a name="onedrive-input"></a>
## <a name="file-input"></a>Bestanden invoeren

De Invoerbinding OneDrive-bestand wordt gelezen van de inhoud van een bestand dat is opgeslagen in OneDrive.

Deze sectie bevat de volgende subgedeelten:

* [Voorbeeld](#file-input---example)
* [Kenmerken](#file-input---attributes)
* [Configuratie](#file-input---configuration)
* [Gebruik](#file-input---usage)

### <a name="file-input---example"></a>Het bestand van de invoer - voorbeeld

Zie het voorbeeld taalspecifieke:

* [C# script (.csx)](#file-input---c-script-example)
* [JavaScript](#file-input---javascript-example)

#### <a name="file-input---c-script-example"></a>Bestandsinvoer - voorbeeld van C#-script

Het volgende voorbeeld wordt een bestand dat is opgeslagen in OneDrive gelezen.

De *function.json* -bestand definieert een HTTP-trigger met een OneDrive-bestand-Invoerbinding:

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

De C#-scriptcode leest het bestand dat is opgegeven in de querytekenreeks en logboeken van de lengte:

```csharp
using System.Net;

public static void Run(HttpRequestMessage req, Stream myOneDriveFile, TraceWriter log)
{
    log.Info(myOneDriveFile.Length.ToString());
}
```

#### <a name="file-input---javascript-example"></a>Invoer - bestand JavaScript-voorbeeld

Het volgende voorbeeld wordt een bestand dat is opgeslagen in OneDrive gelezen.

De *function.json* -bestand definieert een HTTP-trigger met een OneDrive-bestand-Invoerbinding:

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

De volgende JavaScript-code leest het bestand dat is opgegeven in de query en retourneert de lengte.

```js
module.exports = function (context, req) {
    context.res = {
        body: context.bindings.myOneDriveFile.length
    };
    context.done();
};
```

### <a name="file-input---attributes"></a>Bestandsinvoer - kenmerken

In [C#-klassebibliotheken](functions-dotnet-class-library.md), gebruikt u de [OneDrive](https://github.com/Azure/azure-functions-microsoftgraph-extension/blob/master/src/MicrosoftGraphBinding/Bindings/OneDriveAttribute.cs) kenmerk.

### <a name="file-input---configuration"></a>Het bestand van de invoer - configuratie

De volgende tabel beschrijft de binding configuratie-eigenschappen die u instelt in de *function.json* bestand en de `OneDrive` kenmerk.

|de eigenschap Function.JSON | De kenmerkeigenschap |Beschrijving|
|---------|---------|----------------------|
|**De naam**||Vereist: de naam van de variabele in functiecode gebruikt voor het bestand. Zie [Invoerbinding met behulp van een OneDrive-bestand vanuit code](#onedrive-input-code).|
|**type**||Vereist: moet worden ingesteld op `onedrive`.|
|**direction**||Vereist: moet worden ingesteld op `in`.|
|**Identiteit**|**Identity**|Vereist: de identiteit die wordt gebruikt de actie uit te voeren. Een van de volgende waarden kan zijn:<ul><li><code>userFromRequest</code> -Alleen geldig voor [HTTP-trigger]. Maakt gebruik van de identiteit van de aanroepende gebruiker.</li><li><code>userFromId</code> -Maakt gebruik van de identiteit van een eerder aangemelde gebruiker met de opgegeven ID. Zie de <code>userId</code> eigenschap.</li><li><code>userFromToken</code> -Maakt gebruik van de identiteit die wordt vertegenwoordigd door het opgegeven token. Zie de <code>userToken</code> eigenschap.</li><li><code>clientCredentials</code> -Maakt gebruik van de identiteit van de functie-app.</li></ul>|
|**Gebruikers-id**|**Gebruikers-id**  |Vereist als en alleen als _identiteit_ is ingesteld op `userFromId`. Een gebruiker principal-ID die is gekoppeld aan een eerder aangemelde gebruiker.|
|**userToken**|**UserToken**|Vereist als en alleen als _identiteit_ is ingesteld op `userFromToken`. Een token geldig is voor de functie-app. |
|**Pad**|**Pad**|Vereist: het pad naar het bestand in OneDrive.|

<a name="onedrive-input-code"></a>
### <a name="file-input---usage"></a>Bestandsinvoer - gebruik

Deze binding hoort vereist de volgende Azure AD-machtigingen:
|Resource|Machtiging|
|--------|--------|
|Microsoft Graph|Gebruikersbestanden lezen|

De binding wordt aangegeven dat de volgende typen .NET-functies:
- byte[]
- Stream
- tekenreeks
- Microsoft.Graph.DriveItem






<a name="onedrive-output"></a>
## <a name="file-output"></a>Uitvoer in een bestand

De OneDrive-bestand Uitvoerbinding Hiermee wijzigt u de inhoud van een bestand dat is opgeslagen in OneDrive.

Deze sectie bevat de volgende subgedeelten:

* [Voorbeeld](#file-output---example)
* [Kenmerken](#file-output---attributes)
* [Configuratie](#file-output---configuration)
* [Gebruik](#file-output---usage)

### <a name="file-output---example"></a>Het bestand van de uitvoer - voorbeeld

Zie het voorbeeld taalspecifieke:

* [C# script (.csx)](#file-output---c-script-example)
* [JavaScript](#file-output---javascript-example)

#### <a name="file-output---c-script-example"></a>Bestand uitvoer - voorbeeld van C#-script

Het volgende voorbeeld schrijft naar een bestand dat is opgeslagen in OneDrive.

De *function.json* -bestand definieert een HTTP-trigger met een OneDrive-Uitvoerbinding:

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

De C#-scriptcode tekst opgehaald uit de queryreeks en schrijft deze naar een tekstbestand (FunctionsTest.txt zoals gedefinieerd in het vorige voorbeeld) in de hoofdmap van OneDrive van de oproepende functie:

```csharp
using System.Net;
using System.Text;

public static async Task Run(HttpRequest req, TraceWriter log, Stream myOneDriveFile)
{
    string data = req.Query
        .FirstOrDefault(q => string.Compare(q.Key, "text", true) == 0)
        .Value;
    await myOneDriveFile.WriteAsync(Encoding.UTF8.GetBytes(data), 0, data.Length);
    myOneDriveFile.Close();
    return;
}
```

#### <a name="file-output---javascript-example"></a>Uitvoer - bestand JavaScript-voorbeeld

Het volgende voorbeeld schrijft naar een bestand dat is opgeslagen in OneDrive.

De *function.json* -bestand definieert een HTTP-trigger met een OneDrive-Uitvoerbinding:

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

De JavaScript-code opgehaald van de tekst van de query-tekenreeks en schrijft deze naar een tekstbestand (FunctionsTest.txt zoals gedefinieerd in de bovenstaande configuratie) in de hoofdmap van OneDrive van de oproepende functie.

```js
module.exports = function (context, req) {
    context.bindings.myOneDriveFile = req.query.text;
    context.done();
};
```

### <a name="file-output---attributes"></a>Bestand uitvoer - kenmerken

In [C#-klassebibliotheken](functions-dotnet-class-library.md), gebruikt u de [OneDrive](https://github.com/Azure/azure-functions-microsoftgraph-extension/blob/master/src/MicrosoftGraphBinding/Bindings/OneDriveAttribute.cs) kenmerk.

### <a name="file-output---configuration"></a>Het bestand van de uitvoer - configuratie

De volgende tabel beschrijft de binding configuratie-eigenschappen die u instelt in de *function.json* bestand en de `OneDrive` kenmerk.

|de eigenschap Function.JSON | De kenmerkeigenschap |Beschrijving|
|---------|---------|----------------------|
|**De naam**||Vereist: de naam van de variabele die wordt gebruikt in de functiecode voor bestand. Zie [met behulp van een OneDrive-bestand-Uitvoerbinding vanuit code](#onedrive-output-code).|
|**type**||Vereist: moet worden ingesteld op `onedrive`.|
|**direction**||Vereist: moet worden ingesteld op `out`.|
|**Identiteit**|**Identity**|Vereist: de identiteit die wordt gebruikt de actie uit te voeren. Een van de volgende waarden kan zijn:<ul><li><code>userFromRequest</code> -Alleen geldig voor [HTTP-trigger]. Maakt gebruik van de identiteit van de aanroepende gebruiker.</li><li><code>userFromId</code> -Maakt gebruik van de identiteit van een eerder aangemelde gebruiker met de opgegeven ID. Zie de <code>userId</code> eigenschap.</li><li><code>userFromToken</code> -Maakt gebruik van de identiteit die wordt vertegenwoordigd door het opgegeven token. Zie de <code>userToken</code> eigenschap.</li><li><code>clientCredentials</code> -Maakt gebruik van de identiteit van de functie-app.</li></ul>|
|**Gebruikers-id** |**Gebruikers-id** |Vereist als en alleen als _identiteit_ is ingesteld op `userFromId`. Een gebruiker principal-ID die is gekoppeld aan een eerder aangemelde gebruiker.|
|**userToken**|**UserToken**|Vereist als en alleen als _identiteit_ is ingesteld op `userFromToken`. Een token geldig is voor de functie-app. |
|**Pad**|**Pad**|Vereist: het pad naar het bestand in OneDrive.|

<a name="onedrive-output-code"></a>
#### <a name="file-output---usage"></a>Bestand uitvoer - gebruik

Deze binding hoort vereist de volgende Azure AD-machtigingen:
|Resource|Machtiging|
|--------|--------|
|Microsoft Graph|Volledige toegang tot gebruikersbestanden|

De binding wordt aangegeven dat de volgende typen .NET-functies:
- byte[]
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

#### <a name="outlook-output---c-script-example"></a>Outlook uitvoer - voorbeeld van C#-script

Het volgende voorbeeld verzendt een e-mailbericht via Outlook.

De *function.json* -bestand definieert een HTTP-trigger met een Outlook-Uitvoerbinding bericht:

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

De C#-scriptcode verzendt een e-mailbericht van de oproepende functie aan een ontvanger die is opgegeven in de query-tekenreeks:

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

#### <a name="outlook-output---javascript-example"></a>Outlook uitvoer - JavaScript-voorbeeld

Het volgende voorbeeld verzendt een e-mailbericht via Outlook.

De *function.json* -bestand definieert een HTTP-trigger met een Outlook-Uitvoerbinding bericht:

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

De JavaScript-code verzendt een e-mailbericht van de oproepende functie aan een ontvanger die is opgegeven in de query-tekenreeks:

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

### <a name="outlook-output---attributes"></a>Outlook uitvoer - kenmerken

In [C#-klassebibliotheken](functions-dotnet-class-library.md), gebruikt u de [Outlook](https://github.com/Azure/azure-functions-microsoftgraph-extension/blob/master/src/MicrosoftGraphBinding/Bindings/OutlookAttribute.cs) kenmerk.

### <a name="outlook-output---configuration"></a>Uitvoer van de Outlook - configuratie

De volgende tabel beschrijft de binding configuratie-eigenschappen die u instelt in de *function.json* bestand en de `Outlook` kenmerk.

|de eigenschap Function.JSON | De kenmerkeigenschap |Beschrijving|
|---------|---------|----------------------|
|**De naam**||Vereist: de naam van de variabele in functiecode gebruikt voor het e-mailbericht. Zie [met behulp van een Outlook-bericht-Uitvoerbinding vanuit code](#outlook-output-code).|
|**type**||Vereist: moet worden ingesteld op `outlook`.|
|**direction**||Vereist: moet worden ingesteld op `out`.|
|**Identiteit**|**Identity**|Vereist: de identiteit die wordt gebruikt de actie uit te voeren. Een van de volgende waarden kan zijn:<ul><li><code>userFromRequest</code> -Alleen geldig voor [HTTP-trigger]. Maakt gebruik van de identiteit van de aanroepende gebruiker.</li><li><code>userFromId</code> -Maakt gebruik van de identiteit van een eerder aangemelde gebruiker met de opgegeven ID. Zie de <code>userId</code> eigenschap.</li><li><code>userFromToken</code> -Maakt gebruik van de identiteit die wordt vertegenwoordigd door het opgegeven token. Zie de <code>userToken</code> eigenschap.</li><li><code>clientCredentials</code> -Maakt gebruik van de identiteit van de functie-app.</li></ul>|
|**Gebruikers-id**|**Gebruikers-id**  |Vereist als en alleen als _identiteit_ is ingesteld op `userFromId`. Een gebruiker principal-ID die is gekoppeld aan een eerder aangemelde gebruiker.|
|**userToken**|**UserToken**|Vereist als en alleen als _identiteit_ is ingesteld op `userFromToken`. Een token geldig is voor de functie-app. |

<a name="outlook-output-code"></a>
### <a name="outlook-output---usage"></a>Outlook uitvoer - gebruik

Deze binding hoort vereist de volgende Azure AD-machtigingen:
|Resource|Machtiging|
|--------|--------|
|Microsoft Graph|E-mail verzenden als gebruiker|

De binding wordt aangegeven dat de volgende typen .NET-functies:
- Microsoft.Graph.Message
- Newtonsoft.Json.Linq.JObject
- tekenreeks
- Aangepaste objecttypen (met behulp van de binding van de structurele model)






## <a name="webhooks"></a>Webhooks

Webhooks kunt u om te reageren op gebeurtenissen in de Microsoft Graph. Ter ondersteuning van webhooks, functies nodig zijn om te maken, vernieuwen en reageren op _webhookabonnementen_. Een volledige webhook-oplossing vereist een combinatie van de volgende bindingen:
- Een [Microsoft Graph-webhook-trigger](#webhook-trigger) kunt u om te reageren op een binnenkomende webhook.
- Een [Microsoft Graph-webhookabonnement Invoerbinding](#webhook-input) kunt u bestaande abonnementen weergeven en ze desgewenst te vernieuwen.
- Een [Microsoft Graph-webhookabonnement-Uitvoerbinding](#webhook-output) kunt u maken of verwijderen van de webhookabonnementen.

De bindingen zelf hoeven niet alle Azure AD-machtigingen, maar u moet machtigingen die relevant zijn voor het resourcetype dat u wilt om te reageren op aanvragen. Zie voor een lijst van deze machtigingen nodig voor elk resourcetype [abonnementsmachtigingen](https://developer.microsoft.com/en-us/graph/docs/api-reference/v1.0/api/subscription_post_subscriptions).

Zie voor meer informatie over webhooks [werken met webhooks in Microsoft Graph].





## <a name="webhook-trigger"></a>Webhook-trigger

De Microsoft Graph-webhook-trigger kunt een functie om te reageren op een binnenkomende webhook van de Microsoft Graph. Elk exemplaar van deze trigger kan reageren op een resourcetype voor Microsoft Graph.

Deze sectie bevat de volgende subgedeelten:

* [Voorbeeld](#webhook-trigger---example)
* [Kenmerken](#webhook-trigger---attributes)
* [Configuratie](#webhook-trigger---configuration)
* [Gebruik](#webhook-trigger---usage)

### <a name="webhook-trigger---example"></a>Webhook-trigger - voorbeeld

Zie het voorbeeld taalspecifieke:

* [C# script (.csx)](#webhook-trigger---c-script-example)
* [JavaScript](#webhook-trigger---javascript-example)

#### <a name="webhook-trigger---c-script-example"></a>Webhook-trigger - voorbeeld van C#-script

Webhooks voor Outlook-berichten worden verwerkt door het volgende voorbeeld. Gebruik van een webhook activeert u [maken van een abonnement](#webhook-output---example), en kunt u [vernieuwen van het abonnement](#webhook-subscription-refresh) om te voorkomen dat deze verloopt.

De *function.json* -bestand definieert een webhook-trigger:

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

De C#-scriptcode reageert op binnenkomende e-mailberichten en logboeken van de instantie van die zijn verzonden door de ontvanger en die 'Azure Functions' in het onderwerp bevat:

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

#### <a name="webhook-trigger---javascript-example"></a>Webhook-trigger - JavaScript-voorbeeld

Webhooks voor Outlook-berichten worden verwerkt door het volgende voorbeeld. Gebruik van een webhook activeert u [maken van een abonnement](#webhook-output---example), en kunt u [vernieuwen van het abonnement](#webhook-subscription-refresh) om te voorkomen dat deze verloopt.

De *function.json* -bestand definieert een webhook-trigger:

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

De JavaScript-code reageert op binnenkomende e-mailberichten en logboeken van de instantie van die zijn verzonden door de ontvanger en die 'Azure Functions' in het onderwerp bevat:

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

### <a name="webhook-trigger---attributes"></a>Webhook-trigger - kenmerken

In [C#-klassebibliotheken](functions-dotnet-class-library.md), gebruikt u de [GraphWebHookTrigger](https://github.com/Azure/azure-functions-microsoftgraph-extension/blob/master/src/MicrosoftGraphBinding/Bindings/GraphWebHookTriggerAttribute.cs) kenmerk.

### <a name="webhook-trigger---configuration"></a>Webhook-trigger - configuratie

De volgende tabel beschrijft de binding configuratie-eigenschappen die u instelt in de *function.json* bestand en de `GraphWebHookTrigger` kenmerk.

|de eigenschap Function.JSON | De kenmerkeigenschap |Beschrijving|
|---------|---------|----------------------|
|**De naam**||Vereist: de naam van de variabele in functiecode gebruikt voor het e-mailbericht. Zie [met behulp van een Outlook-bericht-Uitvoerbinding vanuit code](#outlook-output-code).|
|**type**||Vereist: moet worden ingesteld op `graphWebhook`.|
|**direction**||Vereist: moet worden ingesteld op `trigger`.|
|**resourceType**|**ResourceType**|Vereist: de grafiekresource waarvoor deze functie moet reageren op webhooks. Een van de volgende waarden kan zijn:<ul><li><code>#Microsoft.Graph.Message</code> -wijzigingen in de Outlook-berichten.</li><li><code>#Microsoft.Graph.DriveItem</code> -wijzigingen in OneDrive hoofdmap items.</li><li><code>#Microsoft.Graph.Contact</code> -wijzigingen aan persoonlijke contactpersonen in Outlook.</li><li><code>#Microsoft.Graph.Event</code> -wijzigingen in Outlook agenda-items.</li></ul>|

> [!Note]
> Een functie-app kan slechts één functie dat is geregistreerd voor hebben een bepaald `resourceType` waarde.

### <a name="webhook-trigger---usage"></a>Webhook-trigger - gebruik

De binding wordt aangegeven dat de volgende typen .NET-functies:
- Microsoft Graph-SDK relevant zijn voor het resourcetype gegevenstypen zoals `Microsoft.Graph.Message` of `Microsoft.Graph.DriveItem`.
- Aangepaste objecttypen (met behulp van de binding van de structurele model)




<a name="webhook-input"></a>
## <a name="webhook-input"></a>Webhook-invoer

De Microsoft Graph-webhook-Invoerbinding kunt u de lijst met abonnementen die worden beheerd door deze functie-app op te halen. De binding wordt gelezen uit de opslag van functie-app, zodat deze niet overeen met komt andere abonnementen die zijn gemaakt op basis van buiten de app.

Deze sectie bevat de volgende subgedeelten:

* [Voorbeeld](#webhook-input---example)
* [Kenmerken](#webhook-input---attributes)
* [Configuratie](#webhook-input---configuration)
* [Gebruik](#webhook-input---usage)

### <a name="webhook-input---example"></a>Invoer van de Webhook - voorbeeld

Zie het voorbeeld taalspecifieke:

* [C# script (.csx)](#webhook-input---c-script-example)
* [JavaScript](#webhook-input---javascript-example)

#### <a name="webhook-input---c-script-example"></a>Webhook invoer - voorbeeld van C#-script

Het volgende voorbeeld haalt alle abonnementen voor de aanroepende gebruiker en worden verwijderd.

De *function.json* bestand definieert een HTTP-trigger met een Invoerbinding abonnement en een abonnement-Uitvoerbinding die gebruikmaakt van de verwijderingsactie:

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

De C#-scriptcode de abonnementen opgehaald en worden verwijderd:

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

#### <a name="webhook-input---javascript-example"></a>Webhook invoer - JavaScript-voorbeeld

Het volgende voorbeeld haalt alle abonnementen voor de aanroepende gebruiker en worden verwijderd.

De *function.json* bestand definieert een HTTP-trigger met een Invoerbinding abonnement en een abonnement-Uitvoerbinding die gebruikmaakt van de verwijderingsactie:

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

De JavaScript-code opgehaald van de abonnementen en worden verwijderd:

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

### <a name="webhook-input---attributes"></a>Webhook invoer - kenmerken

In [C#-klassebibliotheken](functions-dotnet-class-library.md), gebruikt u de [GraphWebHookSubscription](https://github.com/Azure/azure-functions-microsoftgraph-extension/blob/master/src/MicrosoftGraphBinding/Bindings/GraphWebHookSubscriptionAttribute.cs) kenmerk.

### <a name="webhook-input---configuration"></a>Invoer van de Webhook - configuratie

De volgende tabel beschrijft de binding configuratie-eigenschappen die u instelt in de *function.json* bestand en de `GraphWebHookSubscription` kenmerk.

|de eigenschap Function.JSON | De kenmerkeigenschap |Beschrijving|
|---------|---------|----------------------|
|**De naam**||Vereist: de naam van de variabele in functiecode gebruikt voor het e-mailbericht. Zie [met behulp van een Outlook-bericht-Uitvoerbinding vanuit code](#outlook-output-code).|
|**type**||Vereist: moet worden ingesteld op `graphWebhookSubscription`.|
|**direction**||Vereist: moet worden ingesteld op `in`.|
|**filter**|**Filter**| Indien ingesteld op `userFromRequest`, en vervolgens de binding alleen abonnementen die eigendom zijn van de aanroepende gebruiker haalt (alleen geldig voor [HTTP-trigger]).| 

### <a name="webhook-input---usage"></a>Webhook invoer - gebruik

De binding wordt aangegeven dat de volgende typen .NET-functies:
- String]
- Aangepaste object type matrices
- Newtonsoft.Json.Linq.JObject[]
- Microsoft.Graph.Subscription[]





## <a name="webhook-output"></a>Webhook-uitvoer

Het webhookabonnement Uitvoerbinding kunt u maken, verwijderen en in de Microsoft Graph-webhookabonnementen vernieuwen.

Deze sectie bevat de volgende subgedeelten:

* [Voorbeeld](#webhook-output---example)
* [Kenmerken](#webhook-output---attributes)
* [Configuratie](#webhook-output---configuration)
* [Gebruik](#webhook-output---usage)

### <a name="webhook-output---example"></a>Uitvoer van de Webhook - voorbeeld

Zie het voorbeeld taalspecifieke:

* [C# script (.csx)](#webhook-output---c-script-example)
* [JavaScript](#webhook-output---javascript-example)

#### <a name="webhook-output---c-script-example"></a>Webhook uitvoer - voorbeeld van C#-script

Het volgende voorbeeld wordt een abonnement. U kunt [vernieuwen van het abonnement](#webhook-subscription-refresh) om te voorkomen dat deze verloopt.

De *function.json* -bestand definieert een HTTP-trigger met een abonnement-Uitvoerbinding met behulp van de actie maken:

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
      "name": "clientState",
      "direction": "out",
      "action": "create",
      "subscriptionResource": "me/mailFolders('Inbox')/messages",
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

De C#-scriptcode registreert een webhook zodat deze functie-app wordt op de hoogte wanneer de aanroepende gebruiker een Outlook-bericht ontvangt:

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

#### <a name="webhook-output---javascript-example"></a>Webhook uitvoer - JavaScript-voorbeeld

Het volgende voorbeeld wordt een abonnement. U kunt [vernieuwen van het abonnement](#webhook-subscription-refresh) om te voorkomen dat deze verloopt.

De *function.json* -bestand definieert een HTTP-trigger met een abonnement-Uitvoerbinding met behulp van de actie maken:

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
      "name": "clientState",
      "direction": "out",
      "action": "create",
      "subscriptionResource": "me/mailFolders('Inbox')/messages",
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

De JavaScript-code registreert een webhook zodat deze functie-app wordt op de hoogte wanneer de aanroepende gebruiker een Outlook-bericht ontvangt:

```js
const uuidv4 = require('uuid/v4');

module.exports = function (context, req) {
    context.bindings.clientState = uuidv4();
    context.done();
};
```

### <a name="webhook-output---attributes"></a>Webhook uitvoer - kenmerken

In [C#-klassebibliotheken](functions-dotnet-class-library.md), gebruikt u de [GraphWebHookSubscription](https://github.com/Azure/azure-functions-microsoftgraph-extension/blob/master/src/MicrosoftGraphBinding/Bindings/GraphWebHookSubscriptionAttribute.cs) kenmerk.

### <a name="webhook-output---configuration"></a>Uitvoer van de Webhook - configuratie

De volgende tabel beschrijft de binding configuratie-eigenschappen die u instelt in de *function.json* bestand en de `GraphWebHookSubscription` kenmerk.

|de eigenschap Function.JSON | De kenmerkeigenschap |Beschrijving|
|---------|---------|----------------------|
|**De naam**||Vereist: de naam van de variabele in functiecode gebruikt voor het e-mailbericht. Zie [met behulp van een Outlook-bericht-Uitvoerbinding vanuit code](#outlook-output-code).|
|**type**||Vereist: moet worden ingesteld op `graphWebhookSubscription`.|
|**direction**||Vereist: moet worden ingesteld op `out`.|
|**Identiteit**|**Identity**|Vereist: de identiteit die wordt gebruikt de actie uit te voeren. Een van de volgende waarden kan zijn:<ul><li><code>userFromRequest</code> -Alleen geldig voor [HTTP-trigger]. Maakt gebruik van de identiteit van de aanroepende gebruiker.</li><li><code>userFromId</code> -Maakt gebruik van de identiteit van een eerder aangemelde gebruiker met de opgegeven ID. Zie de <code>userId</code> eigenschap.</li><li><code>userFromToken</code> -Maakt gebruik van de identiteit die wordt vertegenwoordigd door het opgegeven token. Zie de <code>userToken</code> eigenschap.</li><li><code>clientCredentials</code> -Maakt gebruik van de identiteit van de functie-app.</li></ul>|
|**Gebruikers-id**|**Gebruikers-id**  |Vereist als en alleen als _identiteit_ is ingesteld op `userFromId`. Een gebruiker principal-ID die is gekoppeld aan een eerder aangemelde gebruiker.|
|**userToken**|**UserToken**|Vereist als en alleen als _identiteit_ is ingesteld op `userFromToken`. Een token geldig is voor de functie-app. |
|**Actie**|**Actie**|Vereist: Hiermee geeft u de binding van de actie moet worden uitgevoerd. Een van de volgende waarden kan zijn:<ul><li><code>create</code> -Een nieuw abonnement registreert.</li><li><code>delete</code> -Verwijdert een opgegeven abonnement.</li><li><code>refresh</code> -Hiermee vernieuwt u een opgegeven abonnement om te voorkomen dat deze verloopt.</li></ul>|
|**subscriptionResource**|**subscriptionResource**|Vereist als en alleen als de _actie_ is ingesteld op `create`. Hiermee geeft u de Microsoft Graph-resource die wordt gecontroleerd op wijzigingen. Zie [werken met webhooks in Microsoft Graph]. |
|**changeType**|**ChangeType**|Vereist als en alleen als de _actie_ is ingesteld op `create`. Geeft het type wijziging in de geabonneerde resource waarmee een melding wordt geactiveerd. De ondersteunde waarden zijn: `created`, `updated`, `deleted`. Meerdere waarden kunnen worden gecombineerd met behulp van een door komma's gescheiden lijst.|

### <a name="webhook-output---usage"></a>Webhook uitvoer - gebruik

De binding wordt aangegeven dat de volgende typen .NET-functies:
- tekenreeks
- Microsoft.Graph.Subscription




<a name="webhook-examples"></a>
## <a name="webhook-subscription-refresh"></a>Webhook-abonnement vernieuwen

Er zijn twee methoden voor het vernieuwen van abonnementen:

- Gebruik de toepassings-id te bekommeren om alle abonnementen. Hiervoor moet de toestemming van een Azure Active Directory-beheerder. Dit kan worden gebruikt door alle talen die worden ondersteund door Azure Functions.
- Gebruikmaken van de identiteit die is gekoppeld aan elk abonnement door handmatig binden elke gebruikers-ID. Hiervoor moet een aangepaste code voor het uitvoeren van de binding. Dit kan alleen worden gebruikt door de .NET-functies.

Deze sectie bevat een voorbeeld voor elk van deze methoden:

* [Voorbeeld van App-id](#webhook-subscription-refresh---app-identity-example)
* [Voorbeeld van de gebruiker-id](#webhook-subscription-refresh---user-identity-example)

### <a name="webhook-subscription-refresh---app-identity-example"></a>Vernieuwen van Webhookabonnement - voorbeeld van app-id

Zie het voorbeeld taalspecifieke:

* [C# script (.csx)](#app-identity-refresh---c-script-example)
* [JavaScript](#app-identity-refresh---javascript-example)

### <a name="app-identity-refresh---c-script-example"></a>App-id vernieuwen - voorbeeld van C#-script

Het volgende voorbeeld wordt de identiteit van een abonnement te vernieuwen.

De *function.json* definieert een timertrigger met een abonnement Invoerbinding en een abonnement op Storage-Uitvoerbinding is:

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

De C#-scriptcode Hiermee vernieuwt u de abonnementen:

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

### <a name="app-identity-refresh---c-script-example"></a>App-id vernieuwen - voorbeeld van C#-script

Het volgende voorbeeld wordt de identiteit van een abonnement te vernieuwen.

De *function.json* definieert een timertrigger met een abonnement Invoerbinding en een abonnement op Storage-Uitvoerbinding is:

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

De abonnementen Hiermee vernieuwt u de JavaScript-code:

```js
// This template uses application permissions and requires consent from an Azure Active Directory admin.
// See https://go.microsoft.com/fwlink/?linkid=858780

module.exports = function (context) {
    const existing = context.bindings.existingSubscriptions;
    var toRefresh = [];
    for (var i = 0; i < existing.length; i++) {
        context.log(`Refreshing subscription ${existing[i]}`);
        toRefresh.push(existing[i]);
    }
    context.bindings.subscriptionsToRefresh = toRefresh;
    context.done();
};
```

### <a name="webhook-subscription-refresh---user-identity-example"></a>Vernieuwen van Webhookabonnement - voorbeeld van de gebruiker-id

Het volgende voorbeeld wordt de identiteit van de gebruiker een abonnement te vernieuwen.

De *function.json* bestand definieert een timertrigger en wordt de abonnement-Invoerbinding om de functiecode te uitgesteld:

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

De C#-scriptcode Hiermee vernieuwt u de abonnementen en maakt de Uitvoerbinding in code, met behulp van de identiteit van elke gebruiker:

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
> [Meer informatie over Azure functions-triggers en bindingen](functions-triggers-bindings.md)

[HTTP-trigger]: functions-bindings-http-webhook.md
[Werken met webhooks in Microsoft Graph]: https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/webhooks
