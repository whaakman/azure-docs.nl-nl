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
ms.openlocfilehash: 8580e19f1955becb2cfaee13742054af1a9edb7f
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47410447"
---
### <a name="run-the-service"></a>De service uitvoeren

1. Druk op F5 (of typ `azds up` in het Terminal-venster) om de service uit te voeren. De service wordt automatisch uitgevoerd in uw zojuist geselecteerde ruimte `default/scott`. 
1. U kunt controleren of uw service in een eigen ruimte wordt uitgevoerd door nogmaals de opdracht `azds list-up` uit te voeren. Ten eerste ziet u nu dat er een exemplaar van `mywebapi` wordt uitgevoerd in de ruimte `default/scott` (de versie in de ruimte `default` wordt nog steeds uitgevoerd, maar wordt niet vermeld). Als u `azds list-uris` uitvoert, ziet u dat de URL van het toegangspunt voor `webfrontend` het voorvoegsel 'scott.s.' krijgt. Deze URL is uniek voor de ruimte `default/scott`. De speciale URL betekent dat aanvragen die worden verzonden naar de 'scott'-URL eerst naar de services in de ruimte `default/scott` worden gerouteerd. Als dit mislukt, worden ze alsnog naar services in de ruimte `default` geleid.

```
Name                      DevSpace  Type     Updated  Status
------------------------  --------  -------  -------  -------
mywebapi                  scott     Service  3m ago   Running
mywebapi-bb4f4ddd8-sbfcs  scott     Pod      3m ago   Running
webfrontend               default   Service  26m ago  Running
```

```
Uri                                                            Status
-------------------------------------------------------------  ---------
http://localhost:53831 => mywebapi.scott:80                    Tunneled
http://webfrontend.6364744826e042319629.canadaeast.aksapp.io/  Available
```

![](../articles/dev-spaces/media/common/space-routing.png)

Deze ingebouwde functie van Azure Dev Spaces stelt u in staat om code te testen in een gedeelde ruimte, zonder dat elke ontwikkelaar telkens de volledige groep services opnieuw moet maken in hun eigen ruimte. Voor deze routering moeten doorgifteheaders worden doorgestuurd in uw app-code, zoals uitgelegd in de vorige stap van deze handleiding.

### <a name="test-code-in-a-space"></a>Code testen in een ruimte
Als u de nieuwe versie van `mywebapi` samen met `webfrontend` wilt testen, opent u de URL van het openbare toegangspunt voor `webfrontend` in de browser en gaat u naar de pagina About. Hier wordt nu uw nieuwe bericht weergegeven.

Verwijder nu het gedeelte 'scott.s.' uit de URL en vernieuw de browser. U zou nu het oude gedrag weer moeten zien (waarbij de `mywebapi`-versie wordt uitgevoerd in `default`).
