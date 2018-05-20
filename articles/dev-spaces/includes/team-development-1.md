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
ms.openlocfilehash: 96a749c0cb59759e9294f52bd4f631d7fdc2275f
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
---
Tot nu toe hebt u uitgevoerd van uw toepassing code alsof u de enige ontwikkelaar werken op de app. In deze sectie leert u hoe Azure Dev spaties stroomlijnt het teamontwikkeling:
* Schakel in een team van ontwikkelaars kunnen werken in dezelfde ontwikkelomgeving.
* Biedt ondersteuning voor elke ontwikkelaar sequentieel op hun code in isolatie en geen last van anderen op te splitsen.
* Testen code end-to-end, voorafgaand aan code commit, zonder te hoeven maken mocks of afhankelijkheden simuleren.

## <a name="challenges-with-developing-microservices"></a>Uitdagingen bij het ontwikkelen van microservices
Uw voorbeeldtoepassing zeer complex is op dit moment niet. Maar in de praktijk-ontwikkeling uitdagingen snel ontstaan als u meer services toevoegen en uitbreiden van het ontwikkelteam.

Een afbeelding toevoegen aan een service die communiceert met andere services tientallen werkt.

- Het kan worden niet realistisch om alles voor ontwikkeling lokaal worden uitgevoerd. Uw Developer-computer heeft misschien niet voldoende resources om de hele app uitvoeren. Of wellicht uw app heeft eindpunten die moeten worden openbaar bereikbaar is (bijvoorbeeld uw app reageert op een webhook van een SaaS-app).

- U kunt proberen de services die u afhankelijk van zijn alleen worden uitgevoerd, maar dit betekent dat u zou moeten weten volledige sluiting afhankelijkheden (bijvoorbeeld de afhankelijkheden van afhankelijkheden). Of is een kwestie van het niet eenvoudig weten hoe het bouwen en uitvoeren van de afhankelijkheden omdat u niet werken.
- Sommige ontwikkelaars gebruik simuleren of mocking een groot deel van hun serviceafhankelijkheden. Deze aanpak kunt soms maar snel het beheren van deze mocks kunt uitvoeren op een eigen inspanning ontwikkeling. Plus, deze benadering leidt tot uw Developer-omgeving op zoek heel verschillend voor productie en subtiele fouten kunnen creep in.
- Hieruit volgt dat tijdens het doorzoeken van elk type van de end-to-end testen moeilijk. Testen van de integratie kan alleen in de praktijk zich voordoen na doorvoeren, wat betekent dat u problemen Zie verderop in het ontwikkelingsproces bevindt.

![](../media/common/microservices-challenges.png)


## <a name="work-in-a-shared-development-environment"></a>Werken in een gedeelde ontwikkelomgeving
Met Azure Dev spaties, kunt u instellen een *gedeelde* ontwikkelomgeving in Azure. Elke ontwikkelaar kunnen zich concentreren op slechts hun deel van de toepassing en iteratief kunt ontwikkelen *vooraf code doorvoert* in een omgeving die al bevat alle andere services en cloudbronnen die afhankelijk zijn van hun scenario's. Afhankelijkheden zijn altijd up-to-date en ontwikkelaars werken op een manier die productie weerspiegelt.

## <a name="work-in-your-own-space"></a>Werken in uw eigen ruimte
Tijdens het ontwikkelen van code voor uw service en voordat u klaar bent om te controleren in, code vaak niet in goede staat verkeren. U bent nog steeds iteratief vormgeven van, testen en over het experimenteren met oplossingen. Azure Dev spaties biedt het concept van een **ruimte**, waarmee u werkt in isolatie en zonder bang van de leden van uw team.

> [!Note]
> Voordat u verder, alle vensters voor VS-Code voor beide services sluit en voer vervolgens `azds up -d` in elk van de hoofdmap van de service. (Dit is een Preview-beperking.)

U gaat nu een nader bekijken waarin de services worden uitgevoerd. Voer de `azds list` opdracht, en u ziet de uitvoer is vergelijkbaar met het volgende:

```
Name         Space     Chart              Ports   Updated     Access Points
-----------  --------  -----------------  ------  ----------  -------------------------
mywebapi     default  mywebapi-0.1.0     80/TCP  2m ago     <not attached>
webfrontend  default  webfrontend-0.1.0  80/TCP  1m ago     https://webfrontend-contosodev.1234abcdef.westeurope.aksapp.io
```

De ruimte kolom ziet u dat beide services worden uitgevoerd in een ruimte met de naam `default`. Iedereen die Hiermee opent u de openbare URL en gaat u naar de web-app wordt aangeroepen het codepad al eerder aangegeven die wordt uitgevoerd voor beide services. Nu Stel dat u wilt doorgaan met het ontwikkelen van `mywebapi`. Hoe kunt u codewijzigingen aanbrengen en deze testen en andere ontwikkelaars die gebruik van de dev-omgeving maken niet onderbreken? Hiervoor moet u uw eigen space instellen.

## <a name="create-a-space"></a>Een spatie maken
Uitvoeren van uw eigen versie van `mywebapi` in een andere dan `default`, kunt u uw eigen ruimte met behulp van de volgende opdracht:

``` 
azds space create --name scott
```

Ik heb mijn naam voor de nieuwe ruimte gebruikt dan die van persoonsgegevens aan mijn peers is de ruimte die ik werk in in het bovenstaande voorbeeld, maar alles wat u graag en flexibel over wat het betekent, zoals 'sprint4' of 'demo.' aanroepen

Voer de `azds space list` opdracht om een lijst weer van alle spaties in de dev-omgeving. Een sterretje (*) weergegeven naast de momenteel geselecteerde ruimte. In het geval is de ruimte die met de naam 'scott' automatisch geselecteerd wanneer deze is gemaakt. U kunt nog een spatie selecteren op elk gewenst moment de `azds space select` opdracht.
