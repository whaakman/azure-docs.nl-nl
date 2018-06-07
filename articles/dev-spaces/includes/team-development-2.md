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
ms.openlocfilehash: f6245a97f5d94c90e022ac509b61da477f4d9494
ms.sourcegitcommit: 3017211a7d51efd6cd87e8210ee13d57585c7e3b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/06/2018
ms.locfileid: "34823829"
---
### <a name="run-the-service"></a>De service wordt uitgevoerd

1. Druk op F5 (of type `azds up` in het venster Terminal) de service uit te voeren. De service wordt automatisch uitgevoerd in uw nieuwe geselecteerde ruimte `scott`. 
1. U kunt bevestigen dat de service wordt uitgevoerd in een eigen ruimte door het uitvoeren van `azds resource list` opnieuw. Eerst, ziet u een exemplaar van `mywebapi` wordt nu uitgevoerd in de `scott` ruimte (de versie die wordt uitgevoerd in de `default` ruimte nog steeds actief is, maar deze niet wordt weergegeven). Ten tweede het access point-URL voor `webfrontend` heeft nu het voorvoegsel `scott.s.`. Deze URL is uniek voor de `scott` adresruimte en betekent dat aanvragen die worden verzonden naar de URL' scott' proberen eerst om te routeren naar services in de `scott` ruimte, en als er geen specifieke `scott` exemplaar voor de service aanvragen terugvallen op services in de `default` ruimte.

```
Name         Space     Chart              Ports   Updated     Access Points
-----------  --------  -----------------  ------  ----------  -------------
mywebapi     scott     mywebapi-0.1.0     80/TCP  15s ago     http://localhost:61466
webfrontend  default  webfrontend-0.1.0  80/TCP  5h ago      http://scott.s.webfrontend-contosodev.1234abcdef.eastus.aksapp.io
```

![](../media/common/space-routing.png)

Deze ingebouwde functionaliteit van Azure Dev spaties kunt u testen end-to-end in de ruimte van een gedeelde dev code zonder elke ontwikkelaar de volledige stack van services in de ruimte opnieuw te maken. Deze routering is vereist op uw app-code voor het doorsturen doorgeven kopteksten, zoals geïllustreerd in de vorige stap van deze handleiding.

### <a name="test-code-in-a-space"></a>Code testen in een ruimte
Voor het testen van uw nieuwe versie van `mywebapi` met `webfrontend`, open uw browser naar de URL van de punt openbare toegang voor webfrontend en Ga naar de pagina over. Hier ziet u het nieuwe bericht weergegeven.

Verwijder nu het `scott.s.` deel uit van de URL en vernieuw de browser. U ziet het oude gedrag (optrad door de `mywebapi` versie die wordt gebruikt `default`)