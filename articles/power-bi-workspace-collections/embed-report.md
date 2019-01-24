---
title: Een rapport insluiten in Azure Power BI Workspace Collections | Microsoft Docs
description: Leer hoe u een rapport dat is in Power BI Workspace Collections in uw toepassing insluit.
services: power-bi-embedded
author: markingmyname
ROBOTS: NOINDEX
ms.assetid: ''
ms.service: power-bi-embedded
ms.topic: article
ms.workload: powerbi
ms.date: 09/20/2017
ms.author: maghan
ms.openlocfilehash: caa877a265fb8665e062cc0069247bca0994c4bf
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54857094"
---
# <a name="embed-a-report-in-power-bi-workspace-collections"></a>Een rapport insluiten in Power BI-Werkruimteverzamelingen

Leer hoe u een rapport dat is in Power BI Workspace Collections in uw toepassing insluit.

> [!IMPORTANT]
> Power BI Workspace Collections is afgeschaft en is beschikbaar tot juni 2018 of tot de datum die in uw contract wordt aangegeven. Om onderbreking van uw toepassing te voorkomen, wordt u geadviseerd om een migratie naar Power BI Embedded te plannen. Voor meer informatie over het migreren van gegevens naar Power BI Embedded raadpleegt u [How to migrate Power BI Workspace Collections content to Power BI Embedded](https://powerbi.microsoft.com/documentation/powerbi-developer-migrate-from-powerbi-embedded/) (Inhoud van Power BI-werkruimteverzamelingen migreren naar Power BI Embedded).

We kijken hoe u daadwerkelijk een rapport insluiten in uw toepassing. Dit wordt ervan uitgegaan dat u hebt al een rapport dat zich in een werkruimte in de werkruimteverzameling van uw. Als u die stap nog niet hebt gedaan, Zie [aan de slag met Power BI Workspace Collections](get-started.md).

U kunt het .NET (C#) of de SDK voor Node.js, samen met JavaScript, eenvoudig uw toepassing met Power BI Workspace Collections gebruiken.

## <a name="using-the-access-keys-to-use-rest-apis"></a>Met behulp van de toegangssleutels REST-API's gebruiken

Als u wilt de REST-API aanroept, kunt u de toegangssleutel die u uit de Azure-portal voor een bepaalde werkruimteverzameling ophalen kunt doorgeven. Zie voor meer informatie, [aan de slag met Power BI Workspace Collections](get-started.md).

## <a name="get-a-report-id"></a>Een rapport-ID ophalen

Elke toegangstoken is gebaseerd op een rapport. U moet het opgegeven rapport-id niet ophalen voor het rapport dat u wilt insluiten. Dit kan worden gedaan op basis van aanroepen naar de [Get Reports](https://msdn.microsoft.com/library/azure/mt711510.aspx) REST-API. De rapport-id en de ingesloten url wordt dan geretourneerd. Dit kan worden gedaan met behulp van de Power BI .NET-SDK of de REST-API voor het rechtstreeks aanroepen.

### <a name="using-the-power-bi-net-sdk"></a>Met behulp van de Power BI .NET-SDK

Wanneer u de .NET SDK, moet u een token referentie die is gebaseerd op de toegangssleutel die u vanuit Azure portal maken. Dit is vereist dat u installeert de [Power BI API NuGet-pakket](https://www.nuget.org/profiles/powerbi).

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

### <a name="calling-the-rest-api-directly"></a>Het rechtstreeks aanroepen van de REST-API

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

## <a name="create-an-access-token"></a>Een toegangstoken maken

Power BI Workspace Collections gebruiken insluittokens, JSON-Webtokens HMAC zijn ondertekend. De tokens zijn ondertekend door de toegangssleutel van uw Power BI-Werkruimteverzameling. Sluit tokens, standaard, worden gebruikt voor alleen-lezen toegang tot een rapport insluiten in een toepassing. Sluit tokens worden uitgegeven voor een specifiek rapport en moet worden gekoppeld aan een ingesloten URL.

Toegangstokens moeten worden gemaakt op de server als de toegangssleutels voor het teken/versleutelen van de tokens worden gebruikt. Zie voor meer informatie over het maken van een toegangstoken [Authenticating en autoriseren met Power BI Workspace Collections](app-token-flow.md). U kunt ook bekijken de [CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN) methode. Hier volgt een voorbeeld van hoe dit eruit met de .NET SDK voor Power BI.

U gebruikt de rapport-ID die u eerder hebt opgehaald. Zodra het insluittoken is gemaakt, wordt u vervolgens de toegangssleutel voor het genereren van het token dat u kunt gebruiken vanuit het perspectief van javascript gebruiken. De *PowerBIToken klasse* vereist dat u installeert de [Power BI Core NuGut pakket](https://www.nuget.org/packages/Microsoft.PowerBI.Core/).

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

### <a name="adding-permission-scopes-to-embed-tokens"></a>Machtigingsbereiken om in te sluiten van tokens toevoegen

Wanneer u insluittokens gebruikt, kunt u slechts beperkt gebruik van de resources die u toegang te geven. Daarom kunt u een token met scoped machtigingen genereren. Zie voor meer informatie, [bereiken](app-token-flow.md#scopes)

## <a name="embed-using-javascript"></a>Insluiten met JavaScript

Nadat u het toegangstoken en de lijst-ID hebt, kunnen we het rapport met behulp van JavaScript insluiten. Hiervoor moet u het NuGet installeren [Power BI JavaScript-pakket](https://www.nuget.org/packages/Microsoft.PowerBI.JavaScript/). De embedUrl gedraagt zich https://embedded.powerbi.com/appTokenReportEmbed.

> [!NOTE]
> U kunt de [JavaScript rapport ingesloten voorbeeld](https://microsoft.github.io/PowerBI-JavaScript/demo/) om functionaliteit te testen. Het biedt ook voorbeelden van code voor de verschillende bewerkingen die beschikbaar zijn.

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

### <a name="set-the-size-of-embedded-elements"></a>Stel de grootte van ingesloten-elementen

Het rapport wordt automatisch op basis van de grootte van de container worden ingesloten. Als u wilt de standaardgrootte van het ingesloten object onderdrukken, moet u gewoon een CSS-klasse kenmerk of inline stijlen voor breedte en hoogte toevoegen.

## <a name="see-also"></a>Zie ook

[Aan de slag met het voorbeeld](get-started-sample.md)  
[Authenticating and authorizing in Power BI Workspace Collections](app-token-flow.md) (Verifiëren en autoriseren in Power BI Workspace Collections)  
[CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN)  
[Voorbeeld van ingesloten JavaScript](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
[Power BI JavaScript-pakket](https://www.nuget.org/packages/Microsoft.PowerBI.JavaScript/)  
[Power BI API NuGet-pakket](https://www.nuget.org/profiles/powerbi)
[Power BI Core NuGut-pakket](https://www.nuget.org/packages/Microsoft.PowerBI.Core/)  
[Git-opslagplaats voor Power BI-CSharp](https://github.com/Microsoft/PowerBI-CSharp)  
[Git-opslagplaats voor Power BI-knooppunt](https://github.com/Microsoft/PowerBI-Node)  

Nog vragen? [Probeer de Power BI-community](http://community.powerbi.com/)
