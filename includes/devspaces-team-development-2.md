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
ms.openlocfilehash: e0f768b876b49ec006ce98decf121d73d334b6d8
ms.sourcegitcommit: 70550d278cda4355adffe9c66d920919448b0c34
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58439516"
---
### <a name="run-the-service"></a>De service uitvoeren

Voor het uitvoeren van de service, druk op F5 (of het type `azds up` in de Terminal-venster) de service uit te voeren. De service wordt automatisch uitgevoerd in uw zojuist geselecteerde _dev/scott_-ruimte. Bevestigen dat uw service wordt uitgevoerd in een eigen ruimte door het uitvoeren van `azds list-up`:

```cmd
$ azds list-up

Name                      DevSpace  Type     Updated  Status
mywebapi                  scott     Service  3m ago   Running
webfrontend               dev       Service  26m ago  Running
```

U ziet een exemplaar van *mywebapi* wordt nu uitgevoerd de _dev/scott_ ruimte. De versie die wordt uitgevoerd in _dev_ wordt nog uitgevoerd, maar deze niet wordt vermeld.

Lijst van de URL's voor de huidige ruimte door het uitvoeren van `azds list-uris`.

```cmd
$ azds list-uris

Uri                                                                        Status
-------------------------------------------------------------------------  ---------
http://localhost:53831 => mywebapi.scott:80                                Tunneled
http://scott.s.dev.webfrontend.6364744826e042319629.ce.azds.io/  Available
```

U ziet de URL van de punt openbare toegang voor *webfrontend* wordt voorafgegaan door *scott.s*. Deze URL is uniek voor de _dev/scott_-ruimte. Deze URL-voorvoegsel vertelt de controller voor binnenkomend verkeer te routeren van aanvragen naar de _dev/scott_ versie van een service. Wanneer een aanvraag met deze URL wordt verwerkt door Dev spaties, probeert de Controller voor binnenkomend verkeer eerst te routeren van de aanvraag voor de *webfrontend* service in de _dev/scott_ ruimte. Als dat mislukt, wordt de aanvraag worden doorgestuurd naar de *webfrontend* service in de _dev_ ruimte als terugval. Ook ziet er is een localhost-URL voor toegang tot de service via localhost met behulp van de Kubernetes *poort-zone voor forward* functionaliteit. Zie voor meer informatie over routering in Azure Dev spaties en URL's, [hoe Azure Dev spaties werkt en is geconfigureerd](../articles/dev-spaces/how-dev-spaces-works.md).



![Ruimte routering](../articles/dev-spaces/media/common/Space-Routing.png)

Deze ingebouwde functie van Azure Dev Spaces stelt u in staat om code te testen in een gedeelde ruimte, zonder dat elke ontwikkelaar telkens de volledige groep services opnieuw moet maken in hun eigen ruimte. Voor deze routering moeten doorgifteheaders worden doorgestuurd in uw app-code, zoals uitgelegd in de vorige stap van deze handleiding.

### <a name="test-code-in-a-space"></a>Code testen in een ruimte
Als u de nieuwe versie van *mywebapi* wilt testen samen met *webfrontend*, opent u de URL van het openbare toegangspunt voor *webfrontend* in de browser en gaat u naar de pagina Over. Hier wordt nu uw nieuwe bericht weergegeven.

Verwijder nu het gedeelte 'scott.s.' uit de URL en vernieuw de browser. U zou nu het oude gedrag weer moeten zien (waarbij de *mywebapi*-versie wordt uitgevoerd in _dev_).

Zodra u een _dev_-ruimte hebt die altijd de meest recente wijzigingen bevat en ervan uitgaande dat uw toepassing is ontworpen om te profiteren van de DevSpace-routering op basis van ruimte, zoals beschreven in dit gedeelte van de zelfstudie, wordt het in het ideale geval gemakkelijk om te zien hoe Dev Spaces aanzienlijk kan helpen bij het testen van nieuwe functies in de context van de grotere toepassing. In plaats van dat u _alle_ services naar uw privéruimte moet implementeren, kunt u een privéruimte maken die is afgeleid van _dev_, en alleen de services aanroepen waaraan u daadwerkelijk werkt. De routeringsinfrastructuur van Dev Spaces zorgt voor de rest door alle services die deze kan vinden buiten uw privéruimte, terwijl de standaardwaarden terug worden gezet naar de meest recent uitgevoerde versie in de _dev_-ruimte. En nog beter: _meerdere_ ontwikkelaars kunnen actief verschillende services op hetzelfde moment in hun eigen ruimte ontwikkelen zonder elkaar te onderbreken.

### <a name="well-done"></a>Dat is dus gelukt.
U hebt de introductiehandleiding voltooid! U hebt geleerd hoe u:

> [!div class="checklist"]
> * Azure Dev Spaces instellen met een beheerd Kubernetes-cluster in Azure.
> * Iteratief code ontwikkelen in containers.
> * Twee afzonderlijke services ontwikkelen en de DNS-servicedetectie van Kubernetes gebruiken om een andere service aan te roepen.
> * Uw code op een productieve manier ontwikkelen en testen in een teamomgeving.
> * Zorg voor een basislijn van de functionaliteit van Dev-spaties van om eenvoudig geïsoleerde wijzigingen te testen in de context van een grotere microservicetoepassing

Nu dat u Azure Dev spaties, hebt verkend [de adresruimte van uw dev delen met een teamlid](../articles/dev-spaces/how-to/share-dev-spaces.md) en beginnen met het samenwerken.

## <a name="clean-up"></a>Opruimen
Als u een exemplaar van Azure Dev Spaces in een cluster volledig wilt verwijderen, waaronder alle ontwikkelruimtes en de actieve services die erin worden uitgevoerd, gebruikt u de opdracht `az aks remove-dev-spaces`. Houd er rekening mee dat deze actie niet kan worden teruggedraaid. U kunt later opnieuw ondersteuning voor Azure Dev Spaces toevoegen aan het cluster, maar u moet dan helemaal opnieuw beginnen. Uw oude services en ruimtes worden niet hersteld.

In het volgende voorbeeld worden de Azure Dev Spaces-controllers in uw actieve abonnement vermeld en wordt vervolgens de Azure Dev Spaces-controller die is gekoppeld aan het AKS-cluster 'myaks' in de resourcegroep 'myaks-rg' verwijderd.

```cmd
    azds controller list
    az aks remove-dev-spaces --name myaks --resource-group myaks-rg
```