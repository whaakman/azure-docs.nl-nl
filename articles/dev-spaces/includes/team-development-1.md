---
title: bestand opnemen
description: bestand opnemen
ms.custom: include file
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: ghogen
ms.author: ghogen
ms.date: 05/11/2018
ms.topic: include
manager: douge
ms.openlocfilehash: 0d3bdb6cc56b90d1975af73be1bb8cc1f73e1213
ms.sourcegitcommit: 4ea0cea46d8b607acd7d128e1fd4a23454aa43ee
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/15/2018
ms.locfileid: "40129096"
---
Tot nu toe hebt u de code van de toepassing uitgevoerd alsof u de enige ontwikkelaar bent die werkt aan de app. In deze sectie leert u hoe teamontwikkeling wordt gestroomlijnd met Azure Dev Spaces:
* Laat een team ontwikkelaars in dezelfde omgeving werken door ze naar behoefte in een gedeelde ontwikkelruimte of in individuele ontwikkelruimten te laten werken.
* Biedt ondersteuning om alle ontwikkelaars hun code geïsoleerd te laten herhalen, zonder dat ze het gevaar lopen dat ze andermans code breken.
* Test code end-to-end, voordat u de code doorvoert, zonder nagebootste code te maken of afhankelijkheden te simuleren.

### <a name="challenges-with-developing-microservices"></a>Uitdagingen bij het ontwikkelen van microservices
Momenteel is uw voorbeeldcode niet erg complex. Maar in de pratijk krijgt u bij het ontwikkelen al snel te maken met uitdagingen wanneer u meer services toevoegt en het ontwikkelteam groeit.

Stel, bijvoorbeeld, dat u werkt aan een service die werkt met een heel aantal andere services.

- Alles lokaal uitvoeren voor ontwikkeling is dan misschien niet zo realistisch. Uw ontwikkelmachine heeft mogelijk niet genoeg resources om de hele app uit te voeren. Of misschien heeft uw app eindpunten die openbaar bereikbaar moeten zijn (bijvoorbeeld als de app reageert op een webhook vanuit een SaaS-app).

- U kunt proberen om alleen de services uit te voeren waarvan u afhankelijk bent, maar dit betekent dat u bekend moet zijn met de volledige sluiting van afhankelijkheden (bijvoorbeeld de afhankelijkheden van afhankelijkheden). Of misschien weet u zo gauw niet hoe u de afhankelijkheden bouwt en uitvoert omdat u niet met ze hebt gewerkt.
- Sommige ontwikkelaars maken gebruik van het simuleren of nabootsen van veel van hun serviceafhankelijkheden. Deze aanpak kan soms helpen, maar het beheren van deze nabootsingen brengt ook al snel uitdagingen op het gebied van ontwikkelen met zich mee. Bovendien laat deze aanpak uw ontwikkelomgeving er heel anders uitzien dan de productieomgeving, waardoor subtiele fouten kunnen optreden.
- Een direct gevolg is dat end-to-endtesten moeilijk wordt. De integratie kan alleen realistisch worden getest na een doorvoering, wat betekent dat problemen pas later in de ontwikkelcyclus zichtbaar worden.

![](../media/common/microservices-challenges.png)


### <a name="work-in-a-shared-dev-space"></a>In een gedeelde ontwikkelruimte werken
Met Azure Dev Spaces kunt u een *gedeelde* ontwikkelruimte instellen in Azure. Elke ontwikkelaar kan zich richten op het eigen deel van de toepassing, en kan *vooraf doorgevoerde code* iteratief doorvoeren in een ontwikkelruimte die alle andere services en cloudresources al bevat waarvan de scenario’s afhankelijk zijn. Afhankelijkheden zijn altijd up-to-date en ontwikkelaars werken op een manier die de productie weerspiegelt.

### <a name="work-in-your-own-space"></a>Werken in uw eigen ruimte
Terwijl u code ontwikkelt voor uw service en voordat u klaar bent om deze uit te geven, is de codestatus meestal niet in orde. U bent de code nog steeds iteratief aan het vormen en testen, en aan het experimenten met oplossingen. Azure Dev Spaces biedt het concept van een **ruimte**, waardoor u geïsoleerd kunt werken, zonder dat u de code van teamleden per ongeluk breekt.

> [!Note]
> Sluit voordat u verdergaat all VS Code-vensters voor beide services en voer `azds up -d` uit in elke hoofdmap van de service. (Dit is een Preview-beperking.)

Laten we eens beter kijken naar waar de services momenteel worden uitgevoerd. Voer de opdracht `azds list-up` uit en de uitvoer is vergelijkbaar met de volgende uitvoer:

```
Name                          DevSpace  Type     Updated      Status
----------------------------  --------  -------  -----------  ----------------
mywebapi                      default   Service  10m 1s ago   Running
mywebapi-54f9cf5b59-bjnkm     default   Pod      10m 4s ago   Running
webfrontend-5b697958d6-b6v96  default   Pod      26m 38s ago  Init:1/3:mindaro-build
```

In de kolom DevSpace wordt aangegeven dat beide services in een ruimte met de naam `default` worden uitgevoerd. Iedereen die de openbare URL opent en naar de web-app navigeert, roept het codepad aan dat u eerder hebt geschreven en dat actief is in beide services. Stel nu dat u door wilt gaan met de ontwikkeling van `mywebapi`. Hoe kunt u wijzigingen aanbrengen in de code en die testen zonder andere ontwikkelaars te onderbreken die dezelfde ontwikkelruimte gebruiken? Hiervoor moet u uw eigen ruimte instellen.

### <a name="create-a-dev-space"></a>Een ontwikkelruimte maken
Om uw eigen versie van `mywebapi` uit te voeren in een andere ruimte dan `default`, kunt u uw eigen ruimte maken met de volgende opdracht:

``` 
azds space select --name scott
```

Selecteer `default` als de **bovenliggende ontwikkelruimte** als u daarom wordt gevraagd. Dit betekent dat onze nieuwe ruimte, `default/scott`, wordt afgeleid van de ruimte `default`. Binnenkort zien we hoe dit ons helpt met testen. 

In bovenstaand voorbeeld heb ik mijn naam gebruikt voor een nieuwe ruimte zodat die herkenbaar is voor mijn collega’s als de ruimte waarin ik werk. U kunt het echter noemen zoals u wilt en flexibel zijn voor wat betreft de betekenis, zoals ‘sprint4’ of ‘demo’.

Voer de opdracht `azds space list` uit om een lijst te krijgen van alle ruimten in de ontwikkelomgeving. Naast de momenteel geselecteerde ruimte verschijnt een asterisk (*). In uw geval is de ruimte met de naam 'default/scott' automatisch geselecteerd toen deze werd gemaakt. U kunt op elk moment een andere ruimte selecteren met de opdracht `azds space select`.
