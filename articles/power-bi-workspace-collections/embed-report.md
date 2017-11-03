---
title: Een rapport insluiten in Azure Power BI werkruimte verzamelingen | Microsoft Docs
description: Informatie over het insluiten van een rapport dat zich in Power BI werkruimte verzamelingen in uw toepassing.
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ROBOTS: NOINDEX
ms.assetid: 
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 09/20/2017
ms.author: asaxton
ms.openlocfilehash: 56e7ca90132527c0ef9d4bd478e99b75ca055272
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="embed-a-report-in-power-bi-workspace-collections"></a>Een rapport insluiten in Power BI werkruimte verzamelingen

Informatie over het insluiten van een rapport dat zich in Power BI werkruimte verzamelingen in uw toepassing.

> [!IMPORTANT]
> Power BI Workspace Collections is afgeschaft en is beschikbaar tot juni 2018 of tot de datum die in uw contract wordt aangegeven. Om onderbreking van uw toepassing te voorkomen, wordt u geadviseerd om een migratie naar Power BI Embedded te plannen. Voor meer informatie over het migreren van gegevens naar Power BI Embedded raadpleegt u [How to migrate Power BI Workspace Collections content to Power BI Embedded](https://powerbi.microsoft.com/documentation/powerbi-developer-migrate-from-powerbi-embedded/) (Inhoud van Power BI-werkruimteverzamelingen migreren naar Power BI Embedded).

Laten we zien hoe u daadwerkelijk een rapport insluiten in uw toepassing. Hierbij wordt verondersteld dat er al een rapport dat bestaat in een werkruimte in uw werkruimteverzameling. Als u deze stap nog niet hebt gedaan, Zie [aan de slag met Power BI werkruimte verzamelingen](get-started.md).

U kunt de .NET (C#) of Node.js-SDK, samen met JavaScript, gemakkelijk maken voor uw toepassing met Power BI werkruimte verzamelingen.

## <a name="using-the-access-keys-to-use-rest-apis"></a>Met behulp van de toegangssleutels REST-API's gebruiken

Om de REST-API aanroept, kunt u de toegangssleutel die u via de Azure-portal voor een bepaalde werkruimteverzameling krijgen kunt doorgeven. Zie voor meer informatie [aan de slag met Power BI werkruimte verzamelingen](get-started.md).

## <a name="get-a-report-id"></a>Een rapport-ID ophalen

Elke toegangstoken is gebaseerd op een rapport. U moet de opgegeven rapport-id niet ophalen voor het rapport dat u wilt insluiten. Dit kan worden gedaan op basis van aanroepen naar de [Get Reports](https://msdn.microsoft.com/library/azure/mt711510.aspx) REST-API. Hiermee herstelt u de lijst-id en de url insluiten. Dit kan worden gedaan met behulp van de Power BI .NET SDK of de REST-API rechtstreeks aan te roepen.

### <a name="using-the-power-bi-net-sdk"></a>Met behulp van de Power BI .NET SDK

Wanneer u de .NET SDK, moet u een token referentie die is gebaseerd op de toegangssleutel die u via de Azure portal krijgen maken. Dit vereist dat u installeert de [Power BI API NuGet-pakket](https://www.nuget.org/profiles/powerbi).

**NuGet-pakket installeren**

```
Install-Package Microsoft.PowerBI.Api
```

**C#-code**

```
using Microsoft.PowerBI.Api.V1;
using Microsoft.Rest;

var credentials = new TokenCredentials("{access key}", "AppKey");
var client = new PowerBIClient(credentials);
client.BaseUri = new Uri(https://api.powerbi.com);

var reports = (IList<Report>)client.Reports.GetReports(workspaceCollectionName, workspaceId).Value;

// Select the report that you want to work with from the collection of reports.
```

### <a name="calling-the-rest-api-directly"></a>Rechtstreeks aanroepen van de REST-API

```
System.Net.WebRequest request = System.Net.WebRequest.Create("https://api.powerbi.com/v1.0/collections/{collectionName}/workspaces/{workspaceId}/Reports") as System.Net.HttpWebRequest;

request.Method = "GET";
request.ContentLength = 0;
request.Headers.Add("Authorization", String.Format("AppKey {0}", accessToken.Value));

using (var response = request.GetResponse() as System.Net.HttpWebResponse)
{
    using (var reader = new System.IO.StreamReader(response.GetResponseStream()))
    {
        // Work with JSON response to get the report you want to work with.
    }

}
```

## <a name="create-an-access-token"></a>Maken van een toegangstoken

Gebruik van Power BI werkruimte verzamelingen sluit tokens, JSON Web Tokens die HMAC zijn ondertekend. De tokens zijn ondertekend door de toegangssleutel van uw Power BI-Werkruimteverzameling. Sluit tokens, standaard, worden gebruikt om alleen-lezen toegang bieden tot een rapport insluiten in een toepassing. Sluit tokens worden uitgegeven voor een specifiek rapport en moeten worden gekoppeld aan een URL insluiten.

Toegangstokens moeten worden gemaakt op de server, zoals de toegangssleutels voor het teken/versleutelen van de tokens worden gebruikt. Zie voor meer informatie over het maken van een toegangstoken [Authenticating en autoriseren met Power BI werkruimte verzamelingen](app-token-flow.md). U kunt ook bekijken de [CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_) methode. Hier volgt een voorbeeld van hoe dit eruitziet met de .NET SDK voor Power BI.

U de ID die u eerder hebt opgehaald. Zodra de insluittoken is gemaakt, wordt u vervolgens de toegangssleutel voor het genereren van het token dat u kunt gebruiken vanuit het perspectief van javascript gebruiken. De *PowerBIToken klasse* vereist dat u installeert de [Power BI Core NuGut pakket](https://www.nuget.org/packages/Microsoft.PowerBI.Core/).

**NuGet-pakket installeren**

```
Install-Package Microsoft.PowerBI.Core
```

**C#-code**

```
using Microsoft.PowerBI.Security;

// rlsUsername, roles and scopes are optional.
embedToken = PowerBIToken.CreateReportEmbedToken(workspaceCollectionName, workspaceId, reportId, rlsUsername, roles, scopes);

var token = embedToken.Generate("{access key}");
```

### <a name="adding-permission-scopes-to-embed-tokens"></a>Scopes van de machtiging voor het insluiten van tokens toevoegen

Wanneer u insluiten tokens gebruikt, kunt u beperkt gebruik van de resources die u toegang te geven. Daarom kunt u een token met bereik machtigingen genereren. Zie voor meer informatie [Scopes](app-token-flow.md#scopes)

## <a name="embed-using-javascript"></a>Insluiten met JavaScript

Nadat u het toegangstoken en de lijst-ID hebt, kunnen we het rapport met JavaScript insluiten. Dit vereist dat u het NuGet installeert [Power BI JavaScript pakket](https://www.nuget.org/packages/Microsoft.PowerBI.JavaScript/). De embedUrl worden alleen https://embedded.powerbi.com/appTokenReportEmbed.

> [!NOTE]
> U kunt de [JavaScript rapport insluiten voorbeeld](https://microsoft.github.io/PowerBI-JavaScript/demo/) om functionaliteit te testen. Het biedt ook voorbeelden van code voor de verschillende bewerkingen die beschikbaar zijn.

**NuGet-pakket installeren**

```
Install-Package Microsoft.PowerBI.JavaScript
```

**JavaScript-code**

```
<script src="/scripts/powerbi.js"></script>
<div id="reportContainer"></div>

var embedConfiguration = {
    type: 'report',
    accessToken: 'eyJ0eXAiO...Qron7qYpY9MI',
    id: '5dac7a4a-4452-46b3-99f6-a25915e0fe55',
    embedUrl: 'https://embedded.powerbi.com/appTokenReportEmbed'
};

var $reportContainer = $('#reportContainer');
var report = powerbi.embed($reportContainer.get(0), embedConfiguration);
```

### <a name="set-the-size-of-embedded-elements"></a>De grootte van de ingesloten elementen instellen

Het rapport wordt automatisch op basis van de grootte van de container worden ingesloten. U kunt de standaardgrootte van het ingesloten item overschrijven, gewoon een CSS-klasse-kenmerk of inline stijlen voor breedte en hoogte toevoegen.

## <a name="see-also"></a>Zie ook

[Aan de slag met het voorbeeld](get-started-sample.md)  
[Authenticating and authorizing in Power BI Workspace Collections](app-token-flow.md) (Verifiëren en autoriseren in Power BI Workspace Collections)  
[CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_)  
[Voorbeeld van ingesloten JavaScript](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
[Power BI JavaScript-pakket](https://www.nuget.org/packages/Microsoft.PowerBI.JavaScript/)  
[Power BI API NuGet-pakket](https://www.nuget.org/profiles/powerbi)
[Power BI Core NuGut-pakket](https://www.nuget.org/packages/Microsoft.PowerBI.Core/)  
[Power BI-CSharp Git-opslagplaats](https://github.com/Microsoft/PowerBI-CSharp)  
[Power BI-knooppunt Git-opslagplaats](https://github.com/Microsoft/PowerBI-Node)  

Nog vragen? [Probeer de Power BI-community](http://community.powerbi.com/)
