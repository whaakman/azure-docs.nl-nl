---
title: Interactie met rapporten met behulp van de JavaScript-API | Microsoft Docs
description: Met behulp van de JavaScript-API van Power BI kunt u eenvoudig Power BI-rapporten insluiten in uw toepassingen.
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ROBOTS: NOINDEX
ms.assetid: bdd885d3-1b00-4dcf-bdff-531eb1f97bfb
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 09/20/2017
ms.author: asaxton
ms.openlocfilehash: 62a95807c35fcba15a8e5ffdf340a307dd22a642
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="interact-with-power-bi-reports-using-the-javascript-api"></a>Interactie met Power BI-rapporten met behulp van de JavaScript-API

Met behulp van de JavaScript-API van Power BI kunt u eenvoudig Power BI-rapporten insluiten in uw toepassingen. Dankzij de API kunnen uw toepassingen via programmacode communiceren met verschillende rapportelementen, zoals pagina's en filters. Door deze interactiviteit zijn Power BI-rapporten een meer geïntegreerd onderdeel van uw toepassing.

> [!IMPORTANT]
> Power BI Workspace Collections is afgeschaft en is beschikbaar tot juni 2018 of tot de datum die in uw contract wordt aangegeven. Om onderbreking van uw toepassing te voorkomen, wordt u geadviseerd om een migratie naar Power BI Embedded te plannen. Voor meer informatie over het migreren van gegevens naar Power BI Embedded raadpleegt u [How to migrate Power BI Workspace Collections content to Power BI Embedded](https://powerbi.microsoft.com/documentation/powerbi-developer-migrate-from-powerbi-embedded/) (Inhoud van Power BI-werkruimteverzamelingen migreren naar Power BI Embedded).

U kunt een Power BI-rapport insluiten in uw toepassing met behulp van een iframe dat wordt gehost als onderdeel van de toepassing. Het iframe fungeert als een grens tussen uw toepassing en het rapport, zoals u kunt zien in de volgende afbeelding:

![iFrame van Power BI-werkruimteverzameling zonder Javascript-API](media/interact-with-reports/iframe-without-javacript.png)

Dankzij het iframe gaat het insluiten veel eenvoudiger, maar zonder de JavaScript-API is er geen interactie mogelijk tussen het rapport en de toepassing. Door dit gebrek aan interactie lijkt het alsof het rapport niet echt deel uitmaakt van de toepassing. Het rapport en de toepassing moeten echt met elkaar kunnen communiceren, zoals in de volgende afbeelding:

![iframe van Power BI-werkruimteverzameling met Javascript-API](media/interact-with-reports/iframe-with-javascript.png)

De JavaScript-API van Power BI maakt het mogelijk code te schrijven die de iframe-grens veilig kan passeren. Hierdoor kan de toepassing via programmacode een actie uitvoeren in een rapport en luisteren naar gebeurtenissen van de acties die gebruikers in een rapport uitvoeren.

## <a name="what-can-you-do-with-the-power-bi-javascript-api"></a>Wat kunt u doen met de JavaScript-API van Power BI?

Met de JavaScript-API kunt u rapporten beheren, navigeren naar pagina's in een rapport, een rapport filteren en insluitgebeurtenissen verwerken. In het volgende diagram ziet u de structuur van de API.

![Diagram van JavaScript-API van Power BI](media/interact-with-reports/javascript-api-diagram.png)

### <a name="manage-reports"></a>Rapporten beheren
Met de Javascript-API kunt u gedrag beheren op rapportniveau en op paginaniveau:

* Een specifiek Power BI-rapport veilig insluiten in uw toepassing: probeer de [demotoepassing over insluiten](http://azure-samples.github.io/powerbi-angular-client/#/scenario1)
  * Toegangstoken instellen
* Het rapport configureren
  * Het filterdeelvenster en het deelvenster voor paginanavigatie in- en uitschakelen: probeer de [demotoepassing over het bijwerken van instellingen](http://azure-samples.github.io/powerbi-angular-client/#/scenario6)
  * Standaardwaarden instellen voor pagina's en filters: probeer de [demo over het selecteren van standaardinstellingen](http://azure-samples.github.io/powerbi-angular-client/#/scenario5)
* De schermvullende modus openen en sluiten

[Meer informatie over het insluiten van een rapport](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Embedding-Basics)

### <a name="navigate-to-pages-in-a-report"></a>Navigeren naar pagina's in een rapport
Via de JavaScript-API kunt u alle pagina's in een rapport detecteren en de huidige pagina instellen. Probeer de [demotoepassing over navigatie](http://azure-samples.github.io/powerbi-angular-client/#/scenario3).

[Meer informatie over paginanavigatie](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Page-Navigation)

### <a name="filter-a-report"></a>Een rapport filteren
De JavaScript-API biedt eenvoudige en geavanceerde filtermogelijkheden voor ingesloten rapporten en rapportpagina’s. Probeer de [demotoepassing over filteren](http://azure-samples.github.io/powerbi-angular-client/#/scenario4) en bekijk hier wat inleidende code.

#### <a name="basic-filters"></a>Basisfilters
Een basisfilter wordt geplaatst in een kolom of hiërarchieniveau en bevat een lijst met waarden die moeten worden opgenomen of uitgesloten.

```
const basicFilter: pbi.models.IBasicFilter = {
  $schema: "http://powerbi.com/product/schema#basic",
  target: {
    table: "Store",
    column: "Count"
  },
  operator: "In",
  values: [1,2,3,4]
}
```

#### <a name="advanced-filters"></a>Geavanceerde filters
Geavanceerde filters gebruiken de logische operator AND of OR en accepteren één of twee voorwaarden, elk met hun eigen operator en waarde. Ondersteunde voorwaarden zijn:

* None
* LessThan
* LessThanOrEqual
* GreaterThan
* GreaterThanOrEqual
* Contains
* DoesNotContain
* StartsWith
* DoesNotStartWith
* Is
* IsNot
* IsBlank
* IsNotBlank

```
const advancedFilter: pbi.models.IAdvancedFilter = {
  $schema: "http://powerbi.com/product/schema#advanced",
  target: {
    table: "Store",
    column: "Name"
  },
  logicalOperator: "Or",
  conditions: [
    {
      operator: "Contains",
      value: "Wash"
    },
    {
      operator: "Contains",
      value: "Park"
    }
  ]
}
```

[Meer informatie over filteren](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Filters)

### <a name="handling-events"></a>Gebeurtenissen verwerken

Naast het verzenden van gegevens naar het iframe kan uw toepassing ook gegevens ontvangen over de volgende gebeurtenissen die afkomstig zijn van het iframe:

* Embed
  * loaded
  * error
* Rapporten
  * pageChanged
  * dataSelected (binnenkort)

[Meer informatie over het verwerken van gebeurtenissen](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Handling-Events)

## <a name="next-steps"></a>Volgende stappen

Bekijk de volgende koppelingen voor meer informatie over de JavaScript-API van Power BI:

* [Wiki over JavaScript-API](https://github.com/Microsoft/PowerBI-JavaScript/wiki)
* [Naslaginformatie over objectmodel](https://microsoft.github.io/powerbi-models/modules/_models_.html)
* [Livedemo](https://microsoft.github.io/PowerBI-JavaScript/demo/)