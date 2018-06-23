---
title: Web-API-interface in kennis exploratie Service API | Microsoft Docs
description: De web-API-interface gebruiken voor het maken van een rijke, semantische zoekfunctie in het Knowledge exploratie Service (KES) API in cognitieve Services.
services: cognitive-services
author: bojunehsu
manager: stesp
ms.service: cognitive-services
ms.component: knowledge-exploration
ms.topic: article
ms.date: 03/26/2016
ms.author: paulhsu
ms.openlocfilehash: 16c5680eb4f249a5d37e6b90eea92cfff7090eef
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344462"
---
# <a name="web-api-interface"></a>Web-API-Interface
De modelbestanden gemaakt door de kennis exploratie-Service worden gehost en toegankelijk zijn via een reeks web-API's.  De API's kan worden gehost op de lokale machine met de [ `host_service` ](CommandLine.md#host_service-command) opdracht of kan worden geïmplementeerd op een Azure-cloud service met behulp van de [ `deploy_service` ](CommandLine.md#deploy_service-command) opdracht.  Beide technieken beschikbaar de volgende API-eindpunten:
* [*interpreteren* ](interpretMethod.md) – een queryreeks van natuurlijke taal worden geïnterpreteerd. Retourneert interpretaties met aantekeningen voor een perfecte ervaring bij het automatisch voltooien van tekst die de gebruiker in een zoekvak typt.
* [*evalueren* ](evaluateMethod.md) – resulteert en retourneert de uitvoer van een structured query-expressie.
* [*calchistogram* ](calchistogramMethod.md) – berekent een histogram van de kenmerkwaarden voor objecten die zijn geretourneerd door een structured query-expressie.

Samen worden gebruikt, toestaan deze API-methoden voor het maken van een uitgebreide semantische zoekfunctie.  Een queryreeks van natuurlijke taal, krijgt de *interpreteren* methode biedt aantekeningen versies van de invoer voor de query met structured query-expressies, op basis van de onderliggende grammatica- en -gegevens.  De *evalueren* methode de structured query-expressie wordt geëvalueerd en retourneert de overeenkomende index-objecten voor weergave.  De *calchistogram* methode berekent de kenmerk-waarde distributies als u wilt filteren en verfijning inschakelen.

**Voorbeeld**

In een domein academic publicaties, als een gebruiker de tekenreeks "latente s" de *interpreteren* methode krijgt u een reeks gerangschikte interpretaties, opmaken dat de gebruiker mogelijk zoekt het sleutelwoord 'latente semantische analyse', de titel 'latente structuur analysis', of andere expressies die beginnen met "latente s".  Deze informatie kan worden gebruikt om te leiden snel de gebruiker naar de gewenste zoekresultaten.

Dit domein, de *evalueren* methode kan worden gebruikt voor het ophalen van een reeks die overeenkomt met publicaties vanuit de academic index en de *calchistogram* methode kan worden gebruikt voor het berekenen van de distributie van kenmerk waarden voor de overeenkomende publicaties, die kunnen worden gebruikt voor verdere filter en de zoekresultaten te verfijnen.

Houd er rekening mee dat de REST API-aanroepen ter verbetering van de leesbaarheid van de voorbeelden tekens (zoals spaties) die niet URL-codering zijn bevatten. Uw code moet de juiste URL-coderingen toepassen.
