---
title: Aan de slag met een voorbeeld
description: In dit artikel maakt u kennis met het voorbeeld van Power BI Workspace Collections-get-gestart.
services: power-bi-embedded
author: markingmyname
ROBOTS: NOINDEX
ms.assetid: d8a9ef78-ad4e-4bc7-9711-89172dc5c548
ms.service: power-bi-embedded
ms.topic: article
ms.workload: powerbi
ms.date: 09/25/2017
ms.author: maghan
ms.openlocfilehash: 4a7da4a6e7b4b395063f7e4c9a3e97d610492ef6
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56882659"
---
# <a name="get-started-with-power-bi-workspace-collections-sample"></a>Aan de slag met Power BI Workspace Collections-voorbeeld

Met **Microsoft Power BI Workspace Collections**, kunt u Power BI-rapporten integreren in uw web- of mobiele toepassingen. In dit artikel stellen we u aan de **Power BI Workspace Collections** gestart get-voorbeeld.

> [!IMPORTANT]
> Power BI Workspace Collections is afgeschaft en is beschikbaar tot juni 2018 of tot de datum die in uw contract wordt aangegeven. Om onderbreking van uw toepassing te voorkomen, wordt u geadviseerd om een migratie naar Power BI Embedded te plannen. Voor meer informatie over het migreren van gegevens naar Power BI Embedded raadpleegt u [How to migrate Power BI Workspace Collections content to Power BI Embedded](https://powerbi.microsoft.com/documentation/powerbi-developer-migrate-from-powerbi-embedded/) (Inhoud van Power BI-werkruimteverzamelingen migreren naar Power BI Embedded).

Voordat we verdergaan, die u wilt opslaan in de volgende bronnen: Ze helpen u bij het Power BI-rapporten te integreren in de voorbeeld-app en uw eigen apps.

* [Voorbeeld werkruimte web-app](https://go.microsoft.com/fwlink/?LinkId=761493)
* [Power BI-werkruimte verzamelingen API-verwijzing](https://msdn.microsoft.com/library/azure/mt711507.aspx)
* [Power BI .NET-SDK](https://go.microsoft.com/fwlink/?LinkId=746472) (beschikbaar via NuGet)
* [Voorbeeld van het JavaScript-rapport insluiten](https://microsoft.github.io/PowerBI-JavaScript/demo)

> [!NOTE]
> Voordat u configureren kunt en uitvoeren aan de Power BI Workspace Collections voorbeeld is gestart, moet u ten minste één maken **Werkruimteverzameling** in uw Azure-abonnement. Voor meer informatie over het maken van een **Werkruimteverzameling** in Azure portal, Zie [aan de slag met Power BI Workspace Collections](get-started.md).

## <a name="configure-the-sample-app"></a>De voorbeeld-app kunt configureren

Laten we eens via uw Visual Studio-ontwikkelomgeving instellen voor toegang tot de onderdelen die nodig zijn om uit te voeren van de voorbeeld-app.

1. Downloaden en pak deze uit de [Power BI Workspace Collections - een rapport integreren in een web-app](https://go.microsoft.com/fwlink/?LinkId=761493) op GitHub.
2. Open **Power BI-embedded.sln** in Visual Studio. U moet mogelijk om uit te voeren de **-updatepakket** opdracht in de NuGet Package Manager-Console om bij te werken van de pakketten die worden gebruikt in deze oplossing.
3. Bouw de oplossing.
4. Voer de **ProvisionSample** console-app. In de voorbeeld-console-app inrichten van een werkruimte en een PBIX-bestand importeren.
5. Voor het inrichten van een nieuwe **werkruimte**, Selecteer optie 1, **beheer**, en selecteer vervolgens de optie 6, **een nieuwe werkruimte inrichten**
6. Voor het importeren van een nieuwe **rapport**, 2, de optie **rapporteren management**, en selecteer vervolgens de optie 3, **importeren PBIX-Desktop-bestand in een werkruimte**.

7. Voer uw **Werkruimteverzameling** naam, en **toegangssleutel**. U kunt deze krijgen in de **Azure-portal**. Voor meer informatie over hoe u aan uw **toegangssleutel**, Zie [weergave Power BI API-toegang tot sleutels](get-started.md#view-power-bi-api-access-keys) in aan de slag met Microsoft Power BI Embedded.

    ![Toegangssleutels in Azure portal](media/get-started-sample/azure-portal.png)
8. Kopieer en sla de zojuist gemaakte **werkruimte-ID** voor gebruik verderop in dit artikel. Na de **werkruimte-ID** is gemaakt, kunt u deze vinden de **Azure-portal**.

    ![Werkruimte-ID in Azure portal](media/get-started-sample/workspace-id.png)
9. Voor het importeren van een PBIX-bestand in uw **werkruimte**, Selecteer optie **6. PBIX-Desktop-bestand importeren in een bestaande werkruimte**. Als u een PBIX-bestand bij de hand hebt, kunt u downloaden de [Retail Analysis Sample PBIX](https://go.microsoft.com/fwlink/?LinkID=780547).
10. Als u hierom wordt gevraagd, voert u een beschrijvende naam voor uw **gegevensset**.

Hier ziet u een antwoord weergegeven zoals:

```
Checking import state... Publishing
Checking import state... Succeeded
```

> [!NOTE]
> Als uw PBIX-bestand geen directquery-verbindingen bevat, voert u de optie 7 om bij te werken van de verbindingsreeksen.

Op dit moment hebt u een Power BI PBIX-rapport dat is geïmporteerd in uw **werkruimte**. Nu gaan we kijken hoe u uitvoeren de **Power BI Workspace Collections** ophalen van de slag-voorbeeld-web-app.

## <a name="run-the-sample-web-app"></a>De voorbeeld-web-app uitvoeren

De voorbeeld-web-app is een voorbeeldtoepassing die Hiermee maakt u rapporten die zijn geïmporteerd in uw **werkruimte**. Hier volgt een voorbeeld van de web-app configureren.

1. In de **Power BI embedded** Visual Studio-oplossing met de rechtermuisknop op de **EmbedSample** webtoepassing en kies **instellen als opstartproject**.
2. In **web.config**, in de **EmbedSample** webtoepassing, bewerken de **appSettings**: **AccessKey**, **WorkspaceCollection** naam, en **WorkspaceId**.

    ```xml
    <appSettings>
        <add key="powerbi:AccessKey" value="" />
        <add key="powerbi:ApiUrl" value="https://api.powerbi.com" />
        <add key="powerbi:WorkspaceCollection" value="" />
        <add key="powerbi:WorkspaceId" value="" />
    </appSettings>
    ```
3. Voer de **EmbedSample** webtoepassing.

Zodra u uitvoert het **EmbedSample** webtoepassing, het linkernavigatievenster moet bevatten een **rapporten** menu. Als u wilt weergeven van het rapport dat u hebt geïmporteerd, vouw **rapporten**, en klikt u op een rapport. Als u hebt geïmporteerd de [Retail Analysis Sample PBIX](https://go.microsoft.com/fwlink/?LinkID=780547), de voorbeeldweb-app zou er als volgt:

![Voorbeeld Linkernavigatie in de voorbeeldtoepassing](media/get-started-sample/sample-left-nav.png)

Nadat u een rapport op de **EmbedSample** web-App moet er ongeveer uitzien dit:

![Voorbeeldrapport weergeven in toepassing](media/get-started-sample/sample-web-app.png)

## <a name="explore-the-sample-code"></a>Nu de voorbeeldcode verkennen

De **Microsoft Power BI Workspace Collections** voorbeeld is een voorbeeld-web-app waarin wordt uitgelegd hoe u integreert **Power BI** rapporten in uw app. Het ontwerppatroon voor een Model-View-Controller (MVC) gebruikt om de aanbevolen procedures. In deze sectie worden de onderdelen van de voorbeeldcode die u kunt nu bekijken binnen de **Power BI embedded** web-app-oplossing. Het patroon Model-View-Controller (MVC) scheidt het modelleren van het domein, de presentatie en de acties op basis van de invoer van de gebruiker in drie afzonderlijke klassen: Model, weergeven en beheren. Zie voor meer informatie over MVC, [meer informatie over ASP.NET](http://www.asp.net/mvc).

De **Microsoft Power BI Workspace Collections** voorbeeldcode als volgt worden gescheiden. Elke sectie bevat de bestandsnaam in de Power BI-embedded.sln-oplossing, zodat u de code eenvoudig in het voorbeeld vinden kunt.

> [!NOTE]
> In deze sectie wordt een samenvatting van de voorbeeldcode die laat zien hoe de code is geschreven. Laad de Power BI-embedded.sln-oplossing in Visual Studio als u het complete voorbeeld.

### <a name="model"></a>Model

Het voorbeeld bestaat uit een **ReportsViewModel** en **ReportViewModel**.

**ReportsViewModel.cs**: Hiermee geeft u Power BI-rapporten.

```csharp
public class ReportsViewModel
{
    public List<Report> Reports { get; set; }
}
```

**ReportViewModel.cs**: Hiermee geeft u een Power BI-rapport.

```csharp
public class ReportViewModel
{
    public IReport Report { get; set; }

    public string AccessToken { get; set; }
}
```

### <a name="connection-string"></a>Verbindingsreeks

De verbindingsreeks moet zich in de volgende indeling:

```
Data Source=tcp:MyServer.database.windows.net,1433;Initial Catalog=MyDatabase
```

Met behulp van algemene server en database kenmerken is mislukt. Bijvoorbeeld: Server=tcp:MyServer.database.windows.net,1433;Database=MyDatabase,

### <a name="view"></a>Weergave

De **weergave** beheert de weergave van Power BI **rapporten** en een Power BI- **rapport**.

**Reports.cshtml**: Herhalen **Model.Reports** maken een **ActionLink**. De **ActionLink** is als volgt samengesteld:

| Onderdeel | Description |
| --- | --- |
| Titel |De naam van het rapport. |
| QueryString |Een koppeling naar de rapport-ID. |
```cshtml
<div id="reports-nav" class="panel-collapse collapse">
    <div class="panel-body">
        <ul class="nav navbar-nav">
            @foreach (var report in Model.Reports)
            {
                var reportClass = Request.QueryString["reportId"] == report.Id ? "active" : "";
                <li class="@reportClass">
                    @Html.ActionLink(report.Name, "Report", new { reportId = report.Id })
                </li>
            }
        </ul>
    </div>
</div>
```
Report.cshtml: Stel de **Model.AccessToken**, en de Lambda-expressie voor **PowerBIReportFor**.

```cshtml
@model ReportViewModel

...

<div class="side-body padding-top">
    @Html.PowerBIAccessToken(Model.AccessToken)
    @Html.PowerBIReportFor(m => m.Report, new { style = "height:85vh" })
</div>
```

### <a name="controller"></a>Controller

**DashboardController.cs**: Hiermee maakt u een PowerBIClient doorgeven een **app-token**. Een JSON Web Token (JWT) is gegenereerd op basis van de **handtekeningsleutel** om op te halen de **referenties**. De **referenties** worden gebruikt voor het maken van een exemplaar van **PowerBIClient**. Zodra u een exemplaar van hebt **PowerBIClient**, kunt u GetReports() en GetReportsAsync() aanroepen.


CreatePowerBIClient()

```csharp
private IPowerBIClient CreatePowerBIClient()
{
    var credentials = new TokenCredentials(accessKey, "AppKey");
    var client = new PowerBIClient(credentials)
    {
        BaseUri = new Uri(apiUrl)
    };

    return client;
}
```

ActionResult Reports()

```csharp
public ActionResult Reports()
{
    using (var client = this.CreatePowerBIClient())
    {
        var reportsResponse = client.Reports.GetReports(this.workspaceCollection, this.workspaceId);

        var viewModel = new ReportsViewModel
        {
            Reports = reportsResponse.Value.ToList()
        };

        return PartialView(viewModel);
    }
}
```

Taak<ActionResult> rapport (reportId tekenreeks)

```csharp
public async Task<ActionResult> Report(string reportId)
{
    using (var client = this.CreatePowerBIClient())
    {
        var reportsResponse = await client.Reports.GetReportsAsync(this.workspaceCollection, this.workspaceId);
        var report = reportsResponse.Value.FirstOrDefault(r => r.Id == reportId);
        var embedToken = PowerBIToken.CreateReportEmbedToken(this.workspaceCollection, this.workspaceId, report.Id);

        var viewModel = new ReportViewModel
        {
            Report = report,
            AccessToken = embedToken.Generate(this.accessKey)
        };

        return View(viewModel);
    }
}
```

### <a name="integrate-a-report-into-your-app"></a>Een rapport integreren in uw app

Zodra u hebt een **rapport**, gebruikt u een **IFrame** om in te sluiten van de Power BI **rapport**. Hier volgt een codefragment van powerbi.js in de **Microsoft Power BI Workspace Collections** voorbeeld.

```javascript
init: function() {
    var embedUrl = this.getEmbedUrl();
    var iframeHtml = '<iframe style="width:100%;height:100%;" src="' + embedUrl + 
        '" scrolling="no" allowfullscreen="true"></iframe>';
    this.element.innerHTML = iframeHtml;
    this.iframe = this.element.childNodes[0];
    this.iframe.addEventListener('load', this.load.bind(this), false);
}
```

## <a name="filter-reports-embedded-in-your-application"></a>Filterrapporten in uw toepassing wordt opgenomen

U kunt een ingesloten rapport filteren met behulp van een URL-syntaxis. U doet dit door u toevoegt een **$filter** query-tekenreeksparameter met een **eq** operator op die uw iFrame src-url met het opgegeven filter. Hier volgt de syntaxis van de filter-query:

```
https://app.powerbi.com/reportEmbed
?reportId=d2a0ea38-...-9673-ee9655d54a4a&
$filter={tableName/fieldName}%20eq%20'{fieldValue}'
```

> [!NOTE]
> {tableName/fieldName} mag geen spaties of speciale tekens bevatten. De {fieldValue} accepteert één categorische waarde.  

## <a name="see-also"></a>Zie ook

[Algemene scenario's voor Microsoft Power BI-Werkruimteverzameling](scenarios.md)  
[Authenticating and authorizing in Power BI Workspace Collections](app-token-flow.md) (Verifiëren en autoriseren in Power BI Workspace Collections)  
[Een rapport insluiten](embed-report.md)  
[Een nieuw rapport maken op basis van een gegevensset](create-report-from-dataset.md)  
[Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)  
[Voorbeeld van ingesloten JavaScript](https://microsoft.github.io/PowerBI-JavaScript/demo/)  

Nog vragen? [Probeer de Power BI-community](http://community.powerbi.com/)
