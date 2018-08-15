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
ms.openlocfilehash: 8d0de9b027cff375526ef570e88540acfed3ef1c
ms.sourcegitcommit: 4ea0cea46d8b607acd7d128e1fd4a23454aa43ee
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/15/2018
ms.locfileid: "40129088"
---
### <a name="run-the-service"></a>De service uitvoeren

1. Druk op F5 (of typ `azds up` in het Terminal-venster) om de service uit te voeren. De service wordt automatisch uitgevoerd in uw zojuist geselecteerde ruimte `default/scott`. 
1. U kunt controleren of uw service in een eigen ruimte wordt uitgevoerd door nogmaals de opdracht `azds list-up` uit te voeren. Ten eerste ziet u nu dat er een exemplaar van `mywebapi` wordt uitgevoerd in de ruimte `default/scott` (de versie in de ruimte `default` wordt nog steeds uitgevoerd, maar wordt niet vermeld). Ten tweede heeft de URL van het toegangspunt voor `webfrontend` het voorvoegsel 'scott.s.'. Deze URL is uniek voor de ruimte `default/scott`. De speciale URL betekent dat aanvragen die worden verzonden naar de 'scott'-URL eerst naar de services in de ruimte `default/scott` worden gerouteerd. Als dit mislukt, worden ze alsnog naar services in de ruimte `default` geleid.

```
Name         Space          Chart              Ports   Updated     Access Points
-----------  --------       -----------------  ------  ----------  -------------
mywebapi     default/scott  mywebapi-0.1.0     80/TCP  15s ago     http://localhost:61466
webfrontend  default        webfrontend-0.1.0  80/TCP  5h ago      http://scott.s.webfrontend-contosodev.1234abcdef.eastus.aksapp.io
```

![](../media/common/space-routing.png)

Deze ingebouwde functie van Azure Dev Spaces stelt u in staat om code te testen in een gedeelde ruimte, zonder dat elke ontwikkelaar telkens de volledige groep services opnieuw moet maken in hun eigen ruimte. Voor deze routering moeten doorgifteheaders worden doorgestuurd in uw app-code, zoals uitgelegd in de vorige stap van deze handleiding.

### <a name="test-code-in-a-space"></a>Code testen in een ruimte
Als u de nieuwe versie van `mywebapi` samen met `webfrontend` wilt testen, opent u de URL van het openbare toegangspunt voor `webfrontend` in de browser en gaat u naar de pagina About. Hier wordt nu uw nieuwe bericht weergegeven.

Verwijder nu het gedeelte 'scott.s.' uit de URL en vernieuw de browser. U zou nu het oude gedrag weer moeten zien (waarbij de `mywebapi`-versie wordt uitgevoerd in `default`)
