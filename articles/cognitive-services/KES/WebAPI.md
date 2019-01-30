---
title: Web-API-interface - Knowledge Exploration Service API
titlesuffix: Azure Cognitive Services
description: De web API-interface gebruiken om te maken van een rijke en semantische zoekervaring in de Knowledge Exploration Service KES () API.
services: cognitive-services
author: bojunehsu
manager: cgronlun
ms.service: cognitive-services
ms.subservice: knowledge-exploration
ms.topic: conceptual
ms.date: 03/26/2016
ms.author: paulhsu
ms.openlocfilehash: 131d8d58982b5430063aa3dc7b3d1982cd430148
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55211301"
---
# <a name="web-api-interface"></a>Web-API-Interface

De modelbestanden die zijn gemaakt door de Knowledge Exploration Service kunnen worden gehost en toegankelijk via een reeks web-API's.  De API's kunnen worden gehost op de lokale machine met de [ `host_service` ](CommandLine.md#host_service-command) opdracht of kunnen worden geïmplementeerd naar een Azure-cloud service met behulp van de [ `deploy_service` ](CommandLine.md#deploy_service-command) opdracht.  Beide technieken beschikbaar maken van de volgende API-eindpunten:

* [*interpreteren* ](interpretMethod.md) – interpreteert een querytekenreeks van natuurlijke taal. Retourneert interpretaties met aantekeningen voor een perfecte ervaring bij het automatisch voltooien van tekst die de gebruiker in een zoekvak typt.
* [*evalueren* ](evaluateMethod.md) – resulteert en wordt de uitvoer van een gestructureerde query-expressie.
* [*calchistogram* ](calchistogramMethod.md) – berekent een histogram met kenmerkwaarden voor objecten die worden geretourneerd door een gestructureerde query-expressie.

Samen worden gebruikt, toestaan deze API-methoden voor het maken van een rijke semantische zoekervaring wordt geboden.  Een querytekenreeks van natuurlijke taal, krijgt de *interpreteren* methode biedt met aantekeningen versies van de invoer voor de query met gestructureerde query-expressies, op basis van de onderliggende gegevens van de grammatica en index.  De *evalueren* methode evalueert de structured query-expressie en retourneert de overeenkomende objecten in de index om weer te geven.  De *calchistogram* methode berekent de kenmerk-waarde distributies om te filteren en te verfijnen.

**Voorbeeld**

In een domein academische publicaties, als een gebruiker de tekenreeks "latente s", de *interpreteren* methode krijgt u een set gerangschikte interpretaties, stellen dat de gebruiker kan worden te zoeken naar het sleutelwoord 'latente semantische analyse', de titel "latente structuur analysis', of andere expressies die beginnen met"latente s".  Deze informatie kan worden gebruikt om de gebruiker snel naar de gewenste zoekresultaten te leiden.

Voor dit domein, de *evalueren* methode kan worden gebruikt om een set die overeenkomt met de publicaties van de academische index te halen en de *calchistogram* methode kan worden gebruikt voor het berekenen van de distributie van kenmerk waarden voor de overeenkomende publicaties, die kunnen worden gebruikt voor het verder filteren en verfijnen van de lijst met zoekresultaten.

Teneinde de voorbeelden beter leesbaar te maken, bevatten de aanroepen van de REST-API tekens (zoals spaties) die niet zijn gecodeerd als een URL. In uw code moet echter wel de juiste URL-codering worden toegepast.
