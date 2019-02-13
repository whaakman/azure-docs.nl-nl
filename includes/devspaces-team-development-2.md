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
ms.openlocfilehash: 749120446619bf682d02be0f9290a6d47540c16a
ms.sourcegitcommit: de32e8825542b91f02da9e5d899d29bcc2c37f28
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/02/2019
ms.locfileid: "55664368"
---
### <a name="run-the-service"></a>De service uitvoeren

1. Druk op F5 (of typ `azds up` in het Terminal-venster) om de service uit te voeren. De service wordt automatisch uitgevoerd in uw zojuist geselecteerde _dev/scott_-ruimte. 
1. U kunt controleren of uw service in een eigen ruimte wordt uitgevoerd door nogmaals de opdracht `azds list-up` uit te voeren. Ten eerste ziet u nu dat er een exemplaar van *mywebapi* wordt uitgevoerd in de _dev/scott_-ruimte (de versie in _dev_ wordt nog steeds uitgevoerd, maar wordt niet vermeld).

    ```
    Name                      DevSpace  Type     Updated  Status
    mywebapi                  scott     Service  3m ago   Running
    mywebapi-bb4f4ddd8-sbfcs  scott     Pod      3m ago   Running
    webfrontend               dev       Service  26m ago  Running
    ```

1. Voer `azds list-uris` uit, en u ziet de URL voor het toegangspunt voor*webfrontend*.

    ```
    Uri                                                                        Status
    -------------------------------------------------------------------------  ---------
    http://localhost:53831 => mywebapi.scott:80                                Tunneled
    http://dev.s.dev.webfrontend.6364744826e042319629.canadaeast.aksapp.io/  Available
    ```

1. In plaats van "dev.s." als URL-voorvoegsel, is deze vervangen door "scott.s." in uw webbrowser. U ziet dat deze bijgewerkte URL nog steeds wordt omgezet. Deze URL is uniek voor de _dev/scott_-ruimte. De speciale URL betekent dat aanvragen die worden verzonden naar de "Scott URL" eerst naar de services in de _dev/scott_-ruimte worden gerouteerd. Als dit mislukt, worden ze alsnog naar services in de _dev_-ruimte teruggezet.

<!--
TODO: replace 2 & 3 with below once bug#753164 and PR#158827 get pushed to production.

You can confirm that your service is running in its own space by running `azds list-up` again. First, you'll notice an instance of *mywebapi* is now running in the _dev/scott_ space (the version running in _dev_ is still running but it is not listed). If you run `azds list-uris`, you will notice that the access point URL for *webfrontend* is prefixed with the text "scott.s.". This URL is unique to the _dev/scott_ space. The special URL signifies that requests sent to the "Scott URL" will try to first route to services in the _dev/scott_ space, but if that fails, they will fall back to services in the _dev_ space.

```
Name                      DevSpace  Type     Updated  Status
mywebapi                  scott     Service  3m ago   Running
mywebapi-bb4f4ddd8-sbfcs  scott     Pod      3m ago   Running
webfrontend               dev       Service  26m ago  Running
```

```
Uri                                                                        Status
-------------------------------------------------------------------------  ---------
http://localhost:53831 => mywebapi.scott:80                                Tunneled
http://scott.s.dev.webfrontend.6364744826e042319629.canadaeast.aksapp.io/  Available
```
-->

![](../articles/dev-spaces/media/common/space-routing.png)

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

Nu u Azure Dev Spaces hebt verkend, kunt u [uw dev-ruimte delen met teamleden](../articles/dev-spaces/how-to/share-dev-spaces.md) en hen laten zien hoe eenvoudig het is om samen te werken.

## <a name="clean-up"></a>Opruimen
Als u een exemplaar van Azure Dev Spaces in een cluster volledig wilt verwijderen, waaronder alle ontwikkelruimtes en de actieve services die erin worden uitgevoerd, gebruikt u de opdracht `az aks remove-dev-spaces`. Houd er rekening mee dat deze actie niet kan worden teruggedraaid. U kunt later opnieuw ondersteuning voor Azure Dev Spaces toevoegen aan het cluster, maar u moet dan helemaal opnieuw beginnen. Uw oude services en ruimtes worden niet hersteld.

In het volgende voorbeeld worden de Azure Dev Spaces-controllers in uw actieve abonnement vermeld en wordt vervolgens de Azure Dev Spaces-controller die is gekoppeld aan het AKS-cluster 'myaks' in de resourcegroep 'myaks-rg' verwijderd.

```cmd
    azds controller list
    az aks remove-dev-spaces --name myaks --resource-group myaks-rg
```