---
title: Een nieuw rapport maken van een gegevensset in Power BI Workspace Collections | Microsoft Docs
description: Power BI-Werkruimteverzameling rapporten kunnen nu worden gemaakt uit een gegevensset in uw eigen toepassing.
services: power-bi-workspace-collections
ms.service: power-bi-workspace-collections
author: markingmyname
ms.author: maghan
ms.topic: article
ms.workload: powerbi
ms.date: 09/20/2017
ms.openlocfilehash: 92f75a15374738d02972adc012b3a28918d7825d
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/27/2019
ms.locfileid: "58518810"
---
# <a name="create-a-new-report-from-a-dataset-in-power-bi-workspace-collections"></a>Een nieuw rapport maken van een gegevensset in Power BI-Werkruimteverzamelingen

Power BI-Werkruimteverzameling rapporten kunnen nu worden gemaakt uit een gegevensset in uw eigen toepassing.

> [!IMPORTANT]
> Power BI Workspace Collections is afgeschaft en is beschikbaar tot juni 2018 of tot de datum die in uw contract wordt aangegeven. Om onderbreking van uw toepassing te voorkomen, wordt u geadviseerd om een migratie naar Power BI Embedded te plannen. Voor meer informatie over het migreren van gegevens naar Power BI Embedded raadpleegt u [How to migrate Power BI Workspace Collections content to Power BI Embedded](https://powerbi.microsoft.com/documentation/powerbi-developer-migrate-from-powerbi-embedded/) (Inhoud van Power BI-werkruimteverzamelingen migreren naar Power BI Embedded).

De verificatiemethode is vergelijkbaar met die van het insluiten van een rapport. Deze is gebaseerd op toegangstokens die specifiek voor een gegevensset zijn. Tokens die worden gebruikt voor PowerBI.com worden uitgegeven door Azure Active Directory (AAD). Power BI-Werkruimteverzameling tokens worden uitgegeven door uw eigen toepassing.

Bij het maken van een ingesloten rapport, zijn de tokens die zijn uitgegeven voor een specifieke gegevensset. Tokens moeten worden gekoppeld aan de ingesloten URL op hetzelfde element zodat elk heeft een unieke token. Als u wilt maken van een rapport ingesloten *Dataset.Read en Workspace.Report.Create* bereiken moeten worden opgegeven in het toegangstoken.

## <a name="create-access-token-needed-to-create-new-report"></a>Toegangstoken voor het maken van nieuw rapport maken

Power BI Workspace Collections gebruiken een ingesloten token, dat HMAC is ondertekend JSON-Webtokens. De tokens zijn ondertekend door de toegangssleutel van uw Power BI-Werkruimteverzameling. Sluit tokens, standaard, worden gebruikt voor alleen-lezen toegang tot een rapport insluiten in een toepassing. Sluit tokens worden uitgegeven voor een specifiek rapport en moet worden gekoppeld aan een ingesloten URL.

Toegangstokens moeten worden gemaakt op de server als de toegangssleutels voor het teken/versleutelen van de tokens worden gebruikt. Zie voor meer informatie over het maken van een toegangstoken [Authenticating en autoriseren met Power BI Workspace Collections](app-token-flow.md). U kunt ook bekijken de [CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN) methode. Hier volgt een voorbeeld van hoe dit eruit met de .NET SDK voor Power BI.

In dit voorbeeld hebben we onze gegevensset-ID die we maken van het nieuwe rapport willen op. Er moet ook de bereiken voor toevoegen *Dataset.Read en Workspace.Report.Create*.

De *PowerBIToken klasse* vereist dat u installeert de [Power BI Core NuGut pakket](https://www.nuget.org/packages/Microsoft.PowerBI.Core/).

**NuGet-pakket installeren**

```powershell
Install-Package Microsoft.PowerBI.Core
```

**C#-code**

```csharp
using Microsoft.PowerBI.Security;

// rlsUsername and roles are optional
string scopes = "Dataset.Read Workspace.Report.Create";
PowerBIToken embedToken = PowerBIToken.CreateReportEmbedTokenForCreation(workspaceCollectionName, workspaceId, datasetId, null, null, scopes);

var token = embedToken.Generate("{access key}");
```

## <a name="create-a-new-blank-report"></a>Maak een nieuw, leeg rapport

Als u wilt een nieuw rapport maakt, moet de configuratie maken worden opgegeven. Hier moeten ook het toegangstoken, de embedURL en de datasetID die we willen het rapport op basis van maken. Hiervoor moet u het nuget installeren [Power BI JavaScript-pakket](https://www.nuget.org/packages/Microsoft.PowerBI.JavaScript/). De embedUrl gedraagt zich https://embedded.powerbi.com/appTokenReportEmbed.

> [!NOTE]
> U kunt de [JavaScript rapport ingesloten voorbeeld](https://microsoft.github.io/PowerBI-JavaScript/demo/) om functionaliteit te testen. Het biedt ook voorbeelden van code voor de verschillende bewerkingen die beschikbaar zijn.

**NuGet-pakket installeren**

```powershell
Install-Package Microsoft.PowerBI.JavaScript
```

**JavaScript-code**

```html
<div id="reportContainer"></div>

<script>
var embedCreateConfiguration = {
        accessToken: 'eyJ0eXAiO...Qron7qYpY9MI',
        embedUrl: 'https://embedded.powerbi.com/appTokenReportEmbed',
        datasetId: '5dac7a4a-4452-46b3-99f6-a25915e0fe55',
    };
    
    // Grab the reference to the div HTML element that will host the report
    var reportContainer = $('#reportContainer')[0];

    // Create report
    var report = powerbi.createReport(reportContainer, embedCreateConfiguration);
</script>
```

Aanroepen van *powerbi.createReport()* maakt een leeg canvas in de bewerkingsmodus worden weergegeven in de *div* element.

![Nieuw, leeg rapport](media/create-report-from-dataset/create-new-report.png)

## <a name="save-new-reports"></a>Nieuwe rapporten opslaan

Het rapport is niet gemaakt totdat u de **opslaan als** bewerking. Dit kan worden gedaan vanuit het bestandsmenu of van JavaScript.

```javascript
 // Get a reference to the embedded report.
    report = powerbi.get(reportContainer);
    
    var saveAsParameters = {
        name: "newReport"
    };

    // SaveAs report
    report.saveAs(saveAsParameters);
```

> [!IMPORTANT]
> Een rapport wordt gemaakt nadat **opslaan als** wordt genoemd. Nadat u hebt opgeslagen, wordt het canvas nog steeds de gegevensset weergegeven in de bewerkingsmodus en niet het rapport. U moet het nieuwe rapport laden, net als andere rapporten.

![Menu - bestand opslaan als](media/create-report-from-dataset/save-new-report.png)

## <a name="load-the-new-report"></a>Het nieuwe rapport laden

Om te communiceren met het nieuwe rapport dat u insluiten in dezelfde manier als de toepassing wordt ingesloten een gewoon rapport wilt, wat betekent dat, een nieuw token specifiek voor het nieuwe rapport moet worden afgegeven en roep vervolgens de methode insluiten.

```html
<div id="reportContainer"></div>
<script>
var embedConfiguration = {
        accessToken: 'eyJ0eXAiO...Qron7qYpY9MJ',
        embedUrl: 'https://embedded.powerbi.com/appTokenReportEmbed',
        reportId: '5dac7a4a-4452-46b3-99f6-a25915e0fe54',
    };
    
    // Grab the reference to the div HTML element that will host the report
    var reportContainer = $('#reportContainer')[0];

    // Embed report
    var report = powerbi.embed(reportContainer, embedConfiguration);
</script>
```

## <a name="automate-save-and-load-of-a-new-report-using-the-saved-event"></a>Automatiseer opslaan en laden van een nieuw rapport met behulp van de gebeurtenis 'opgeslagen'

Als u wilt het automatiseren van 'opslaan als' en klikt u vervolgens het laden van het nieuwe rapport, kunt u het gebruik van de gebeurtenis 'opgeslagen'. Deze gebeurtenis wordt geactiveerd wanneer de opslagbewerking bewerking is voltooid en retourneert een Json-object met de nieuwe reportId, de naam van rapport, de oude reportId (als er een is) en als de bewerking opslaan is of opslaan.

```json
{
  "reportObjectId": "5dac7a4a-4452-46b3-99f6-a25915e0fe54",
  "reportName": "newReport",
  "saveAs": true,
  "originalReportObjectId": null
}
```

Voor het automatiseren van het proces kunt u luisteren op de 'opgeslagen' gebeurtenis, duren voordat de nieuwe reportId, maken van het nieuwe token en het nieuwe rapport insluiten met het.

```html
<div id="reportContainer"></div>
<script>
var embedCreateConfiguration = {
        accessToken: 'eyJ0eXAiO...Qron7qYpY9MI',
        embedUrl: 'https://embedded.powerbi.com/appTokenReportEmbed',
        datasetId: '5dac7a4a-4452-46b3-99f6-a25915e0fe55',
    };
    
    // Grab the reference to the div HTML element that will host the report
    var reportContainer = $('#reportContainer')[0];

    // Create report
    var report = powerbi.createReport(reportContainer, embedCreateConfiguration);


   var saveAsParameters = {
        name: "newReport"
    };

    // SaveAs report
    report.saveAs(saveAsParameters);

    // report.on will add an event handler which prints to Log window.
    report.on("saved", function(event) {
        
         // get new Token
         var newReportId =  event.detail.reportObjectId;

        // create new Token. This is a function that the application should provide
        var newToken = createAccessToken(newReportId,scopes /*provide the wanted scopes*/);
        
        
    var embedConfiguration = {
        accessToken: newToken ,
        embedUrl: 'https://embedded.powerbi.com/appTokenReportEmbed',
        reportId: newReportId,
    };

    // Embed report
    var report = powerbi.embed(reportContainer, embedConfiguration);
       
   // report.off removes a given event handler if it exists.
   report.off("saved");
    });
</script>
```

## <a name="see-also"></a>Zie ook

[Aan de slag met het voorbeeld](get-started-sample.md)  
[Rapporten opslaan](save-reports.md)  
[Een rapport insluiten](embed-report.md)  
[Authenticating and authorizing in Power BI Workspace Collections](app-token-flow.md) (Verifiëren en autoriseren in Power BI Workspace Collections)  
[Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)  
[Voorbeeld van ingesloten JavaScript](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
[Power BI Core NuGut-pakket](https://www.nuget.org/packages/Microsoft.PowerBI.Core/)  
[Power BI JavaScript-pakket](https://www.nuget.org/packages/Microsoft.PowerBI.JavaScript/)  

Nog vragen? [Probeer de Power BI-community](https://community.powerbi.com/)
