---
title: bestand opnemen
description: bestand opnemen
ms.custom: include file
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.subservice: azds-kubernetes
author: DrEsteban
ms.author: stevenry
ms.date: 12/17/2018
ms.topic: include
manager: yuvalm
ms.openlocfilehash: 40c1be20df845b975c023616e38cbb932c985735
ms.sourcegitcommit: 70550d278cda4355adffe9c66d920919448b0c34
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58439526"
---
# <a name="team-development-with-azure-dev-spaces"></a>Teamontwikkeling met Azure Dev Spaces

In deze zelfstudie leert u hoe een team van ontwikkelaars tegelijkertijd in hetzelfde Kubernetes-cluster kan samenwerken met behulp van Dev Spaces.

## <a name="learn-about-team-development"></a>Meer informatie over teamontwikkeling
Tot nu toe hebt u de code van de toepassing uitgevoerd alsof u de enige ontwikkelaar bent die werkt aan de app. In deze sectie leert u hoe teamontwikkeling wordt gestroomlijnd met Azure Dev Spaces:
* Laat een team ontwikkelaars in dezelfde omgeving werken door ze waar nodig in een gedeelde ontwikkelruimte of in individuele ontwikkelruimten te laten werken.
* Biedt ondersteuning om alle ontwikkelaars hun code geïsoleerd te laten herhalen, zonder dat ze het gevaar lopen dat ze andermans code breken.
* Test code end-to-end, voordat u de code doorvoert, zonder nagebootste code te maken of afhankelijkheden te simuleren.

### <a name="challenges-with-developing-microservices"></a>Uitdagingen bij het ontwikkelen van microservices
Momenteel is uw voorbeeldcode niet erg complex. Maar in de pratijk krijgt u bij het ontwikkelen al snel te maken met uitdagingen wanneer u meer services toevoegt en het ontwikkelteam groeit. Alles lokaal uitvoeren voor ontwikkeling is dan misschien niet zo realistisch.

* Uw ontwikkelcomputer heeft mogelijk niet voldoende resources om elke service die u nodig hebt onmiddellijk uit te voeren.
* Sommige services moeten wellicht openbaar bereikbaar zijn. Een service moet bijvoorbeeld een eindpunt hebben dat op een webhook reageert.
* Als u een subset met services wilt uitvoeren, moet u de volledige afhankelijkheidshiërarchie tussen al uw services kennen. Het kan lastig zijn om dat vast te stellen, met name als het aantal services toeneemt.
* Sommige ontwikkelaars maken gebruik van het simuleren of nabootsen van veel van hun serviceafhankelijkheden. Deze aanpak kan helpen, maar dergelijke nabootsingen zijn al gauw van invloed op de ontwikkelingskosten. Bovendien kan deze aanpak ertoe leiden dat uw ontwikkelomgeving er compleet anders uitziet dan de productieomgeving, waardoor er subtiele foutjes kunnen binnensluipen.
* Een direct gevolg is dat het testen van integratie op welke manier dan ook moeilijk wordt. De integratie kan alleen realistisch worden getest na een doorvoering, wat betekent dat problemen pas later in de ontwikkelcyclus zichtbaar worden.

    ![](../articles/dev-spaces/media/common/microservices-challenges.png)

### <a name="work-in-a-shared-dev-space"></a>In een gedeelde ontwikkelruimte werken
Met Azure Dev Spaces kunt u een *gedeelde* ontwikkelruimte instellen in Azure. Elke ontwikkelaar kan zich richten op het eigen deel van de toepassing, en kan *vooraf doorgevoerde code* iteratief doorvoeren in een ontwikkelruimte die alle andere services en cloudresources al bevat waarvan de scenario’s afhankelijk zijn. Afhankelijkheden zijn altijd up-to-date en ontwikkelaars werken op een manier die de productie weerspiegelt.

### <a name="work-in-your-own-space"></a>Werken in uw eigen ruimte
Terwijl u code ontwikkelt voor uw service en voordat u klaar bent om deze uit te geven, is de codestatus meestal niet in orde. U bent de code nog steeds iteratief aan het vormen en testen, en aan het experimenten met oplossingen. Azure Dev Spaces biedt het concept van een **ruimte**, waardoor u geïsoleerd kunt werken, zonder dat u de code van teamleden per ongeluk breekt.

## <a name="use-dev-spaces-for-team-development"></a>Dev Spaces gebruiken voor teamontwikkeling
We demonstreren deze ideeën aan de hand van een concreet voorbeeld met behulp van onze *webfrontend* -> *mywebapi*-voorbeeldtoepassing. We gebruiken een scenario waarin een ontwikkelaar, Scott, een wijziging moet aanbrengen in de *mywebapi*-service, en in die service *alleen*. De *webfrontend* hoeft niet te worden gewijzigd als onderdeel van de door Scott uitgevoerde update.

_Zonder_ Dev Spaces zou Scott maar over een paar manieren beschikken om de update te ontwikkelen en te testen, en geen van alle is daarvoor ideaal:
* ALLE onderdelen lokaal uitvoeren. Hiervoor moet een krachtigere ontwikkelmachine met Docker worden geïnstalleerd, en wellicht ook MiniKube.
* ALLE onderdelen uitvoeren in een geïsoleerde naamruimte in de Kubernetes-cluster. Aangezien de *webfrontend* niet wordt gewijzigd, is dit een verspilling van clusterresources.
* ALLEEN *mywebapi* uitvoeren en handmatige REST-aanroepen doen om te testen. Hiermee wordt niet de volledige end-to-end-stroom getest.
* Op ontwikkeling gerichte code toevoegen aan de *webfrontend* waarmee de ontwikkelaar aanvragen kan verzenden naar een ander exemplaar van *mywebapi*. Dat maakt de *webfrontend*-service ingewikkelder.

### <a name="set-up-your-baseline"></a>De basislijn instellen
We moeten eerst een basislijn voor onze services implementeren. Deze implementatie vertegenwoordigt de laatst bekende goed werkende situatie. Daardoor kunt u eenvoudig het gedrag van uw lokale code vergelijken met de ingecheckte versie. Vervolgens maakt u een onderliggende ruimte op basis van deze basislijn zodat we onze wijzigingen van *mywebapi* kunnen testen binnen de context van de grotere toepassing.

1. De [Dev Spaces-voorbeeldtoepassing](https://github.com/Azure/dev-spaces) klonen: `git clone https://github.com/Azure/dev-spaces && cd dev-spaces`
1. De externe branch *azds_updates* uitchecken: `git checkout -b azds_updates origin/azds_updates`
1. Selecteer de _dev_-ruimte: `azds space select --name dev`. Wanneer u wordt gevraagd een bovenliggende dev-ruimte te selecteren, selecteert u _\<geen\>_.
1. Navigeer naar de _mywebapi_-map en voer deze opdracht uit: `azds up -d`
1. Navigeer naar de _webfrontend_-map en voer deze opdracht uit: `azds up -d`
1. Voer `azds list-uris` uit om het openbare eindpunt voor de _webfrontend_ weer te geven

> [!TIP]
> De bovenstaande stappen stellen handmatig een basislijn in, maar de teams kunnen het beste CI/CD gebruiken om automatisch uw basislijn up-to-date te houden met toegewezen code.
>
> Bekijk onze [handleiding voor het instellen van CI/CD met Azure DevOps](../articles/dev-spaces/how-to/setup-cicd.md) om een werkstroom te maken die vergelijkbaar is met het volgende diagram.
>
> ![Voorbeeld van CI/CD-diagram](../articles/dev-spaces/media/common/ci-cd-complex.png)

Op dit punt aangekomen zou uw basislijn moeten worden uitgevoerd. Voer de opdracht `azds list-up --all` uit en de uitvoer is vergelijkbaar met de volgende uitvoer:

```
$ azds list-up --all

Name                          DevSpace  Type     Updated  Status
----------------------------  --------  -------  -------  -------
mywebapi                      dev       Service  3m ago   Running
mywebapi-56c8f45d9-zs4mw      dev       Pod      3m ago   Running
webfrontend                   dev       Service  1m ago   Running
webfrontend-6b6ddbb98f-fgvnc  dev       Pod      1m ago   Running
```

In de kolom DevSpace wordt aangegeven dat beide services in een ruimte met de naam _dev_ worden uitgevoerd. Iedereen die de openbare URL opent en naar de web-app navigeert, roept het ingecheckte codepad aan dat in beide services wordt uitgevoerd. Stel nu dat u door wilt gaan met de ontwikkeling van _mywebapi_. Hoe kunt u wijzigingen aanbrengen in de code en die testen zonder andere ontwikkelaars te onderbreken die dezelfde ontwikkelruimte gebruiken? Hiervoor moet u uw eigen ruimte instellen.

### <a name="create-a-dev-space"></a>Een ontwikkelruimte maken
Om uw eigen versie van _mywebapi_ uit te voeren in een andere ruimte dan _dev_, kunt u uw eigen ruimte maken met de volgende opdracht:

```cmd
azds space select --name scott
```

Selecteer _dev_ als de **bovenliggende ontwikkelruimte** als u daarom wordt gevraagd. Dit betekent dat onze nieuwe ruimte, _dev/scott_, wordt afgeleid van de _dev_-ruimte. Binnenkort zien we hoe dit ons helpt met testen.

Ervan uitgaande dat deze inleiding nog steeds hypothetisch is, hebben we voor de nieuwe ruimte de naam _scott_ gebruikt, zodat collega's kunnen nagaan wie daarin aan het werk is. U kunt er elke naam aan geven, met de nodige flexibiliteit wat betreft de betekenis ervan, zoals _sprint4_ of _demo_. Hoe het ook zij, _dev_ fungeert als basislijn voor alle ontwikkelaars die aan een onderdeel van deze toepassing werken:

![](../articles/dev-spaces/media/common/ci-cd-space-setup.png)

Voer de opdracht `azds space list` uit om een lijst te krijgen van alle ruimten in de ontwikkelomgeving. In de kolom _Geselecteerd_ wordt aangegeven welke ruimte u momenteel hebt geselecteerd (true/false). In uw geval werd de ruimte met de naam _cott_ automatisch geselecteerd toen die werd gemaakt. U kunt op elk moment een andere ruimte selecteren met de opdracht `azds space select`.

Laten we eens kijken hoe dat in zijn werk gaat.