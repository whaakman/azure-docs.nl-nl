---
title: Rapporten opslaan in Power BI werkruimte verzamelingen | Microsoft Docs
description: Informatie over het opslaan van rapporten in Power BI werkruimte verzamelingen. Hiervoor moet de juiste machtigingen om te kunnen werken met succes.
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
ms.openlocfilehash: 94a72ba4478aa317b4a1930b2894c1346d0590c1
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="save-reports-in-power-bi-workspace-collections"></a>Rapporten opslaan in Power BI werkruimte verzamelingen

Informatie over het opslaan van rapporten in Power BI werkruimte verzamelingen. Rapporten op te slaan, is de juiste machtigingen om te kunnen werken met succes vereist.

> [!IMPORTANT]
> Power BI Workspace Collections is afgeschaft en is beschikbaar tot juni 2018 of tot de datum die in uw contract wordt aangegeven. Om onderbreking van uw toepassing te voorkomen, wordt u geadviseerd om een migratie naar Power BI Embedded te plannen. Voor meer informatie over het migreren van gegevens naar Power BI Embedded raadpleegt u [How to migrate Power BI Workspace Collections content to Power BI Embedded](https://powerbi.microsoft.com/documentation/powerbi-developer-migrate-from-powerbi-embedded/) (Inhoud van Power BI-werkruimteverzamelingen migreren naar Power BI Embedded).

In Power BI werkruimte verzamelingen, kunt u bestaande rapporten bewerken en opslaan. U kunt ook een nieuw rapport maken en opslaan als een nieuw rapport maken.

Om een rapport opslaat, moet u eerst een token voor het specifieke rapport maken met de juiste bereiken:

* Om in te schakelen opslaan Report.ReadWrite is bereik vereist
* Als u wilt opslaan als, zijn Report.Read en Workspace.Report.Copy scopes vereist
* Als u wilt opslaan en op te slaan als inschakelen, zijn Report.ReadWrite en Workspace.Report.Copy vereist

Respectievelijk zodat het recht opslaan of opslaan als knoppen in het bestandsmenu u de juiste machtigingen in de configuratie insluiten opgeven moet wanneer u het rapport insluiten:

* modellen. Permissions.ReadWrite
* modellen. Permissions.Copy
* modellen. Permissions.All

> [!NOTE]
> Uw toegangstoken moet ook de juiste bereiken. Zie voor meer informatie [Scopes](app-token-flow.md#scopes).

## <a name="embed-report-in-edit-mode"></a>Rapport insluiten in de bewerkingsmodus

Stel dat u wilt een rapport insluiten in de bewerkingsmodus in uw app, dus u hoeft alleen de juiste eigenschappen doorgeven in insluiten configuratie en powerbi.embed() aanroepen. Geef de machtigingen en een viewMode om te zien van het opslaan en opslaan als knoppen in de bewerkingsmodus. Zie voor meer informatie [insluiten configuratiedetails](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Embed-Configuration-Details).

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
        permissions: models.Permissions.All /*both save & save as buttons will be visible*/,
        viewMode: models.ViewMode.Edit,
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

Een rapport is nu ingesloten in uw app in de bewerkingsmodus.

## <a name="save-report"></a>Rapport opslaan

Nadat het rapport insluiten in de bewerkingsmodus met de juiste token en machtigingen, kunt u het rapport opslaan in het bestandsmenu of van javascript:

```
 // Get a reference to the embedded report.
    report = powerbi.get(reportContainer);

 // Save report
    report.save();
```

## <a name="save-as"></a>Opslaan als

```
// Get a reference to the embedded report.
    report = powerbi.get(reportContainer);
    
    var saveAsParameters = {
        name: "newReport"
    };

    // SaveAs report
    report.saveAs(saveAsParameters);
```

> [!IMPORTANT]
> Alleen na *opslaan als* is een nieuw rapport gemaakt. Na het opslaan is het canvas nog steeds de oude rapport in de bewerkingsmodus en niet op het nieuwe rapport weergegeven. Sluit het nieuwe rapport dat is gemaakt. Het nieuwe rapport insluiten een nieuw toegangstoken is vereist als ze zijn gemaakt per rapport.

Vervolgens moet u het laden van het nieuwe rapport na een *opslaan als*. Laden van het nieuwe rapport is vergelijkbaar met een rapport insluiten.

```
<div id="reportContainer"></div>
  
var embedConfiguration = {
        accessToken: 'eyJ0eXAiO...Qron7qYpY9MJ',
        embedUrl: 'https://embedded.powerbi.com/appTokenReportEmbed',
        reportId: '5dac7a4a-4452-46b3-99f6-a25915e0fe54',
    };
    
    // Grab the reference to the div HTML element that will host the report
    var reportContainer = $('#reportContainer')[0];

    // Embed report
    var report = powerbi.embed(reportContainer, embedConfiguration);
```

## <a name="see-also"></a>Zie ook

[Aan de slag met het voorbeeld](get-started-sample.md)  
[Een rapport insluiten](embed-report.md)  
[Een nieuw rapport maken op basis van een gegevensset](create-report-from-dataset.md)  
[Authenticating and authorizing in Power BI Workspace Collections](app-token-flow.md) (Verifiëren en autoriseren in Power BI Workspace Collections)  
[Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)  
[Voorbeeld van ingesloten JavaScript](https://microsoft.github.io/PowerBI-JavaScript/demo/)  

Nog vragen? [Probeer de Power BI-community](http://community.powerbi.com/)

