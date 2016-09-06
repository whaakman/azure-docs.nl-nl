<properties
   pageTitle="Interactie met rapporten met behulp van de JavaScript-API | Microsoft Azure"
   description="Power BI Embedded, interactie met rapporten met behulp van de JavaScript-API"
   services="power-bi-embedded"
   documentationCenter=""
   authors="jocaplan"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="08/26/2016"
   ms.author="jocaplan"/>

# Interactie met Power BI-rapporten met behulp van de JavaScript-API

Met behulp van de JavaScript-API van Power BI kunt u eenvoudig Power BI-rapporten insluiten in uw toepassingen. Dankzij de API kunnen uw toepassingen via programmacode communiceren met verschillende rapportelementen, zoals pagina's en filters. Door deze interactiviteit zijn Power BI-rapporten een meer geïntegreerd onderdeel van uw toepassing.

U kunt een Power BI-rapport insluiten in uw toepassing met behulp van een iframe dat wordt gehost als onderdeel van de toepassing. Het iframe fungeert als grens tussen uw toepassing en het rapport, zoals u ziet in de volgende afbeelding. 

![Ingesloten Power BI-iframe zonder Javascript-API](media\powerbi-embedded-interact-with-reports\powerbi-embedded-interact-report-1.png)

Dankzij het iframe gaat het insluiten veel eenvoudiger, maar zonder de JavaScript-API is er geen interactie mogelijk tussen het rapport en de toepassing. Door dit gebrek aan interactie lijkt het alsof het rapport niet echt deel uitmaakt van de toepassing. Het rapport en de toepassing moeten echt met elkaar kunnen communiceren, zoals in de volgende afbeelding.

![Ingesloten Power BI-iframe met Javascript-API](media\powerbi-embedded-interact-with-reports\powerbi-embedded-interact-report-2.png)

De JavaScript-API van Power BI maakt het mogelijk code te schrijven die de iframe-grens veilig kan passeren. Hierdoor kan de toepassing via programmacode een actie uitvoeren in een rapport en luisteren naar gebeurtenissen van de acties die gebruikers in een rapport uitvoeren.

## Wat kunt u doen met de JavaScript-API van Power BI?
Met de JavaScript-API kunt u rapporten beheren, navigeren naar pagina's in een rapport, een rapport filteren en insluitgebeurtenissen verwerken. In het volgende diagram ziet u de structuur van de API.

![Diagram van JavaScript-API van Power BI](media\powerbi-embedded-interact-with-reports\powerbi-embedded-interact-report-3.png)


### Rapporten beheren
Met de Javascript-API kunt u gedrag beheren op rapportniveau en op paginaniveau:

- Een specifiek Power BI-rapport veilig insluiten in uw toepassing: probeer de [demotoepassing over insluiten](http://azure-samples.github.io/powerbi-angular-client/#/scenario1)
  - Toegangstoken instellen
- Het rapport configureren
  - Het filterdeelvenster en het deelvenster voor paginanavigatie in- en uitschakelen: probeer de [demotoepassing over het bijwerken van instellingen](http://azure-samples.github.io/powerbi-angular-client/#/scenario6)
  - Standaardwaarden instellen voor pagina's en filters: probeer de [demo over het selecteren van standaardinstellingen](http://azure-samples.github.io/powerbi-angular-client/#/scenario5)
- De schermvullende modus openen en sluiten

[Meer informatie over het insluiten van een rapport](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Embedding-Basics)


### Navigeren naar pagina's in een rapport
Via de JavaScript-API kunt u alle pagina's in een rapport detecteren en de huidige pagina instellen. Probeer de [demotoepassing over navigatie](http://azure-samples.github.io/powerbi-angular-client/#/scenario3).

[Meer informatie over paginanavigatie](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Page-Navigation)

### Een rapport filteren
De JavaScript-API biedt eenvoudige en geavanceerde filtermogelijkheden voor ingesloten rapporten en rapportpagina’s. Probeer de [demotoepassing over filteren](http://azure-samples.github.io/powerbi-angular-client/#/scenario4) en bekijk hier wat inleidende code.  


#### Basisfilters
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


#### Geavanceerde filters
Geavanceerde filters gebruiken de logische operator AND of OR en accepteren één of twee voorwaarden, elk met hun eigen operator en waarde. Ondersteunde voorwaarden zijn:

- None
- LessThan
- LessThanOrEqual
- GreaterThan
- GreaterThanOrEqual
- Contains
- DoesNotContain
- StartsWith
- DoesNotStartWith
- Is
- IsNot
- IsBlank
- IsNotBlank

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


### Gebeurtenissen verwerken
Naast het verzenden van gegevens naar het iframe kan uw toepassing ook gegevens ontvangen over de volgende gebeurtenissen die afkomstig zijn van het iframe:

- Embed
  - loaded
  - error
- Rapporten
  - pageChanged
  - dataSelected (binnenkort)

[Meer informatie over het verwerken van gebeurtenissen](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Handling-Events)


## Volgende stappen
Bekijk de volgende koppelingen voor meer informatie over de JavaScript-API van Power BI:

- [Wiki over JavaScript-API](https://github.com/Microsoft/PowerBI-JavaScript/wiki)
- [Naslaginformatie over objectmodel](https://microsoft.github.io/powerbi-models/modules/_models_.html)
- Voorbeelden
  - [Angular](http://azure-samples.github.io/powerbi-angular-client)
  - [Ember](https://github.com/Microsoft/powerbi-ember)
- [Livedemo](https://microsoft.github.io/PowerBI-JavaScript/demo/)



<!--HONumber=ago16_HO5-->


