---
title: Web-API-interface - Knowledge Exploration Service API
titlesuffix: Azure Cognitive Services
description: De web API-interface gebruiken om te maken van een rijke en semantische zoekervaring in de Knowledge Exploration Service KES () API.
services: cognitive-services
author: bojunehsu
manager: cgronlun
ms.service: cognitive-services
ms.component: knowledge-exploration
ms.topic: conceptual
ms.date: 03/26/2016
ms.author: paulhsu
ms.openlocfilehash: 5be39e8dce6aeeef32d20273c56650620d6fe986
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/18/2018
ms.locfileid: "46122022"
---
# <a name="web-api-interface"></a>Web-API-Interface

De modelbestanden die zijn gemaakt door de Knowledge Exploration Service kunnen worden gehost en toegankelijk via een reeks web-API's.  De API's kunnen worden gehost op de lokale machine met de [ `host_service` ](CommandLine.md#host_service-command) opdracht of kunnen worden geïmplementeerd naar een Azure-cloud service met behulp van de [ `deploy_service` ](CommandLine.md#deploy_service-command) opdracht.  Beide technieken beschikbaar maken van de volgende API-eindpunten:

* [*interpreteren* ](interpretMethod.md) – interpreteert een querytekenreeks van natuurlijke taal. Retourneert interpretaties met aantekeningen voor een perfecte ervaring bij het automatisch voltooien van tekst die de gebruiker in een zoekvak typt.
* [*evalueren* ](evaluateMethod.md) – resulteert en wordt de uitvoer van een gestructureerde query-expressie.
* [*calchistogram* ](calchistogramMethod.md) – berekent een histogram met kenmerkwaarden voor objecten die worden geretourneerd door een gestructureerde query-expressie.

Samen worden gebruikt, toestaan deze API-methoden voor het maken van een rijke semantische zoekervaring wordt geboden.  Een querytekenreeks van natuurlijke taal, krijgt de *interpreteren* methode biedt met aantekeningen versies van de invoer voor de query met gestructureerde query-expressies, op basis van de onderliggende gegevens van de grammatica en index.  De *evalueren* methode evalueert de structured query-expressie en retourneert de overeenkomende objecten in de index om weer te geven.  De *calchistogram* methode berekent de kenmerk-waarde distributies om te filteren en te verfijnen.

**Voorbeeld**

In een domein academische publicaties, als een gebruiker de tekenreeks "latente s", de *interpreteren* methode krijgt u een set gerangschikte interpretaties, stellen dat de gebruiker kan worden te zoeken naar het sleutelwoord 'latente semantische analyse', de titel "latente structuur analysis', of andere expressies die beginnen met"latente s".  Deze informatie kan worden gebruikt voor het snel de gebruiker naar de gewenste lijst met zoekresultaten.

Voor dit domein, de *evalueren* methode kan worden gebruikt om een set die overeenkomt met de publicaties van de academische index te halen en de *calchistogram* methode kan worden gebruikt voor het berekenen van de distributie van kenmerk waarden voor de overeenkomende publicaties, die kunnen worden gebruikt voor het verder filteren en verfijnen van de lijst met zoekresultaten.

Houd er rekening mee dat de REST API-aanroepen ter verbetering van de leesbaarheid van de voorbeelden tekens (zoals spaties) die niet gecodeerd met een URL zijn bevatten. Uw code moet de juiste URL-codering van toepassing.
