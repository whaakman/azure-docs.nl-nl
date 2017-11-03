---
title: Schakelen tussen weergeven en bewerken van de modus voor rapporten in Power BI werkruimte verzamelingen | Microsoft Docs
description: Informatie over het schakelen tussen weergeven en bewerken van de modus voor uw rapporten in Power BI werkruimte verzamelingen.
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
ms.openlocfilehash: e66778697f3f4f2f065d2757b3b60ac2699c0334
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="toggle-between-view-and-edit-mode-for-reports-in-power-bi-workspace-collections"></a>Schakelen tussen weergeven en bewerken van de modus voor rapporten in Power BI werkruimte verzamelingen

Informatie over het schakelen tussen weergeven en bewerken van de modus voor uw rapporten in Power BI werkruimte verzamelingen.

> [!IMPORTANT]
> Power BI Workspace Collections is afgeschaft en is beschikbaar tot juni 2018 of tot de datum die in uw contract wordt aangegeven. Om onderbreking van uw toepassing te voorkomen, wordt u geadviseerd om een migratie naar Power BI Embedded te plannen. Voor meer informatie over het migreren van gegevens naar Power BI Embedded raadpleegt u [How to migrate Power BI Workspace Collections content to Power BI Embedded](https://powerbi.microsoft.com/documentation/powerbi-developer-migrate-from-powerbi-embedded/) (Inhoud van Power BI-werkruimteverzamelingen migreren naar Power BI Embedded).

## <a name="creating-an-access-token"></a>Maken van een toegangstoken

U moet een toegangstoken dat u de mogelijkheid biedt zowel weergeven en bewerken van een rapport maken. Als u wilt bewerken en een rapport opslaan, moet u de **Report.ReadWrite** token machtiging. Zie voor meer informatie [Authenticating en autorisatie in Power BI werkruimte verzamelingen](app-token-flow.md).

> [!NOTE]
> Hiermee kunt u bewerken en opslaan van wijzigingen aan een bestaand rapport. Als u ook de functie van ondersteunende wilt **OpslaanAls**, moet u extra machtigingen opgeven. Zie voor meer informatie [Scopes](app-token-flow.md#scopes).

```
using Microsoft.PowerBI.Security;

// rlsUsername and roles are optional
string scopes = "Report.ReadWrite";
PowerBIToken embedToken = PowerBIToken.CreateReportEmbedTokenForCreation(workspaceCollectionName, workspaceId, datasetId, null, null, scopes);

var token = embedToken.Generate("{access key}");
```

## <a name="embed-configuration"></a>Configuratie insluiten

U moet machtigingen en een viewMode om te zien van het opslagpunt opgeven knop in de bewerkingsmodus. Zie voor meer informatie [insluiten configuratiedetails](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Embed-Configuration-Details).

Bijvoorbeeld in JavaScript:

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

Dit geeft aan dat het rapport insluiten in de weergavemodus op basis van **viewMode** wordt ingesteld op **modellen. ViewMode.View**.

## <a name="view-mode"></a>Weergavemodus

U kunt de volgende JavaScript overschakelen naar weergavemodus, als u zich in de modus bewerken.

```
// Get a reference to the embedded report HTML element
var reportContainer = $('#reportContainer')[0];

// Get a reference to the embedded report.
report = powerbi.get(reportContainer);

// Switch to view mode.
report.switchMode("view");

```

## <a name="edit-mode"></a>De modus bewerken

Als u in de weergave modus, kunt u de volgende JavaScript voor het overschakelen naar de bewerkingsmodus.

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
[CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_)  
[Voorbeeld van ingesloten JavaScript](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
[Power BI-CSharp Git-opslagplaats](https://github.com/Microsoft/PowerBI-CSharp)  
[Power BI-knooppunt Git-opslagplaats](https://github.com/Microsoft/PowerBI-Node)  

Nog vragen? [Probeer de Power BI-community](http://community.powerbi.com/)
