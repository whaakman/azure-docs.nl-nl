---
title: Schakelen tussen weergeven en bewerken van modus voor rapporten in Power BI Workspace Collections | Microsoft Docs
description: Informatie over het schakelen tussen weergeven en de modus voor uw rapporten in Power BI Workspace Collections bewerken.
services: power-bi-embedded
author: markingmyname
ROBOTS: NOINDEX
ms.service: power-bi-embedded
ms.topic: article
ms.workload: powerbi
ms.date: 09/20/2017
ms.author: maghan
ms.openlocfilehash: 9a12940f08a11b66a0d09937d5ab8fab7304734f
ms.sourcegitcommit: 039263ff6271f318b471c4bf3dbc4b72659658ec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/06/2019
ms.locfileid: "55754333"
---
# <a name="toggle-between-view-and-edit-mode-for-reports-in-power-bi-workspace-collections"></a>Schakelen tussen weergeven en bewerken van modus voor rapporten in Power BI-Werkruimteverzamelingen

Informatie over het schakelen tussen weergeven en de modus voor uw rapporten in Power BI Workspace Collections bewerken.

> [!IMPORTANT]
> Power BI Workspace Collections is afgeschaft en is beschikbaar tot juni 2018 of tot de datum die in uw contract wordt aangegeven. Om onderbreking van uw toepassing te voorkomen, wordt u geadviseerd om een migratie naar Power BI Embedded te plannen. Voor meer informatie over het migreren van gegevens naar Power BI Embedded raadpleegt u [How to migrate Power BI Workspace Collections content to Power BI Embedded](https://powerbi.microsoft.com/documentation/powerbi-developer-migrate-from-powerbi-embedded/) (Inhoud van Power BI-werkruimteverzamelingen migreren naar Power BI Embedded).

## <a name="creating-an-access-token"></a>Een toegangstoken maken

U moet een toegangstoken dat u de mogelijkheid biedt zowel bekijken en bewerken van een rapport maken. Als u wilt bewerken en opslaan van een rapport, moet u de **Report.ReadWrite** token machtiging. Zie voor meer informatie, [Authenticating en autoriseren in Power BI Workspace Collections](app-token-flow.md).

> [!NOTE]
> Hiermee kunt u om te bewerken en wijzigingen in een bestaand rapport opslaan. Als u ook de functie ondersteunende **OpslaanAls**, moet u extra machtigingen opgeven. Zie voor meer informatie, [Scopes](app-token-flow.md#scopes).

```
using Microsoft.PowerBI.Security;

// rlsUsername and roles are optional
string scopes = "Report.ReadWrite";
PowerBIToken embedToken = PowerBIToken.CreateReportEmbedTokenForCreation(workspaceCollectionName, workspaceId, datasetId, null, null, scopes);

var token = embedToken.Generate("{access key}");
```

## <a name="embed-configuration"></a>Configuratie insluiten

U moet opgeven van machtigingen en een weergavemodus om te zien van het opslaan in de bewerkingsmodus knop. Zie voor meer informatie, [configuratiedetails insluiten](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Embed-Configuration-Details).

Bijvoorbeeld, in JavaScript:

```
   <div id="reportContainer"></div>

    // Get models. Models, it contains enums that can be used.
    var models = window['powerbi-client'].models;

    // Embed configuration used to describe the what and how to embed.
    // This object is used when calling powerbi.embed.
    // This also includes settings and options such as filters.
    // You can find more information at https://github.com/Microsoft/PowerBI-JavaScript/wiki/Embed-Configuration-Details.
    var config= {
        type: 'report',
        accessToken: 'eyJ0eXAiO...Qron7qYpY9MI',
        embedUrl: 'https://embedded.powerbi.com/appTokenReportEmbed',
        id:  '5dac7a4a-4452-46b3-99f6-a25915e0fe55',
        permissions: models.Permissions.ReadWrite /*both save & save as buttons will be visible*/,
        viewMode: models.ViewMode.View,
        settings: {
            filterPaneEnabled: true,
            navContentPaneEnabled: true
        }
    };

    // Get a reference to the embedded report HTML element
    var reportContainer = $('#reportContainer')[0];

    // Embed the report and display it within the div container.
    var report = powerbi.embed(reportContainer, config);
```

Hiermee geeft u aan dat het rapport insluiten in de weergavemodus op basis van **weergavemodus** wordt ingesteld op **modellen. ViewMode.View**.

## <a name="view-mode"></a>Weergavemodus

U kunt de volgende JavaScript gebruiken om te schakelen naar de modus weergeven, als u zich in de modus bewerken.

```
// Get a reference to the embedded report HTML element
var reportContainer = $('#reportContainer')[0];

// Get a reference to the embedded report.
report = powerbi.get(reportContainer);

// Switch to view mode.
report.switchMode("view");

```

## <a name="edit-mode"></a>De modus bewerken

U kunt de volgende JavaScript gebruiken om over te schakelen naar de bewerkingsmodus, als u in de weergave modus.

```
// Get a reference to the embedded report HTML element
var reportContainer = $('#reportContainer')[0];

// Get a reference to the embedded report.
report = powerbi.get(reportContainer);

// Switch to edit mode.
report.switchMode("edit");

```

## <a name="see-also"></a>Zie ook

[Aan de slag met het voorbeeld](get-started-sample.md)  
[Een rapport insluiten](embed-report.md)  
[Authenticating and authorizing in Power BI Workspace Collections](app-token-flow.md) (Verifiëren en autoriseren in Power BI Workspace Collections)  
[CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN)  
[Voorbeeld van ingesloten JavaScript](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
[Git-opslagplaats voor Power BI-CSharp](https://github.com/Microsoft/PowerBI-CSharp)  
[Git-opslagplaats voor Power BI-knooppunt](https://github.com/Microsoft/PowerBI-Node)  

Nog vragen? [Probeer de Power BI-community](http://community.powerbi.com/)
