---
title: Vooraf gemaakte domeinverwijzing
titleSuffix: Azure
description: Referentie voor de vooraf gemaakte domeinen vooraf gemaakte verzamelingen van intenties en entiteiten van Language Understanding Intelligent Services (LUIS zijn).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 05/07/2019
ms.author: diberry
ms.openlocfilehash: 3265477108b7e74d65050408add6c5d5c94b4852
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2019
ms.locfileid: "65233895"
---
# <a name="prebuilt-domain-reference-for-your-luis-app"></a>Naslaginformatie voor vooraf gedefinieerde voor uw LUIS-app
Deze referentie bevat informatie over de [vooraf gemaakte domeinen](luis-how-to-use-prebuilt-domains.md), die zijn vooraf gemaakte verzamelingen van intenties en entiteiten die LUIS biedt.

[Aangepaste domeinen](luis-how-to-start-new-app.md), daarentegen, beginnen met geen intents en modellen. U kunt een vooraf gedefinieerde domein intenties en entiteiten toevoegen aan een aangepast model.

# <a name="supported-domains-across-cultures"></a>Ondersteunde domeinen voor culturen

De enige ondersteunde cultuur is Engels. 

<!--

The table below summarizes the currently supported domains. Support for English is usually more complete than others.


| Entity Type       | EN-US      | ZH-CN   | DE    | FR     | ES    | IT      | PT-BR |  JP  |      KO |        NL |    TR |
|:-----------------:|:-------:|:-------:|:-----:|:------:|:-----:|:-------:| :-------:| :-------:| :-------:| :-------:|  :-------:| 
| Calendar    | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | -      | -    | -    | -     | -  |
| Communication   | ✓    | -       | ✓    | ✓     | ✓     | ✓  | -  | -      | -    | -    | -     | -  |
| Email           | ✓    | ✓       | ✓   | ✓     | ✓     | ✓  | -  | -      | -    | -    | -     | -  |
| HomeAutomation           | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | -  | -      | -    | -    | -     | -  |
| Notes      | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | -  | -      | -    | -    | -     | -  |
| Places    | ✓    | -       | ✓    | ✓     | ✓     | ✓  | -  | -      | -    | -    | -     | -  |
| RestaurantReservation   | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | -  | -      | -    | -    | -     | -  |
| ToDo     | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | -  | -      | -    | -    | -     | -  |
| ToDo_IPA        | ✓    | ✓       | ✓    | ✓      | ✓     | ✓       | -  | -      | -    | -    | -     | -  |
| Utilities          | ✓    | ✓        | ✓    | ✓      | ✓     | ✓       | -  | -      | -    | -    | -     | -  |
| Weather        | ✓    | ✓        | ✓    | ✓      | ✓     | ✓       | -  | -      | -    | -    | -     | -  |
| Web    | ✓    | -        | ✓    | ✓      | ✓     | ✓       | -  | -      | -    | -    | -     | -  |
||||||||||||| 

-->

<br><br>

|Entiteitstype|description|
|--|--|
|Agenda|Kalender is over persoonlijke vergaderingen en afspraken, _niet_ openbare gebeurtenissen (zoals world cup schema's, agenda's van Seattle gebeurtenis of algemene agenda's (zoals welke dag het vandaag is, wat vallen begint, is wanneer Labor Day).|
|Communicatie|Aanvragen voor aanroepen, verzenden teksten en chatberichten, contactpersonen en verschillende andere communicatie-verzoeken met betrekking tot (algemeen uitgaand) toevoegen vinden. Neem contact op met de naam alleen query's behoren niet tot communicatie-domein.|
|E-mailen|E-mailadres is een subdomein van de communicatie-domein. Het bevat voornamelijk aanvragen te verzenden en ontvangen van berichten via e-mailberichten.|
|HomeAutomation|Het domein HomeAutomation biedt intenties en entiteiten met betrekking tot het smart home-apparaten beheren. Het hoofdzakelijk biedt ondersteuning voor de opdracht voor stroombeheer met betrekking tot verlichting en airconditioner maar sommige mogelijkheden generalisatie voor andere elektrische apparaten heeft.|
|Opmerkingen|Houd er rekening mee domein biedt intenties en entiteiten voor het maken van notities en schrijven van artikelen voor gebruikers.|
|Plaatsen|Locaties zijn bedrijven, instellingen, restaurants, ruimten voor algemeen gebruik en -adressen. Zoeken naar en vragen over de gegevens van een openbare locatie, zoals locatie, besturingssysteem met hoeveel uren en afstand biedt ondersteuning voor het domein.|
|RestaurantReservation|Intents ondersteunt restaurant reservering domein voor het verwerken van reserveringen voor restaurants.|
|Takenlijst|ToDo domein biedt typen taaklijsten voor gebruikers wilt toevoegen, markeren en hun todo-items verwijderen.|
|Nutsbedrijven|Hulpprogramma's voor domein is een algemene domein tussen alle LUIS vooraf gemaakte modellen die algemene intents en uitingen in verschil scenario's bevat.|
|Weer|Domein weer is gericht op de weersomstandigheden optreedt en aanbevelingen met de locatie en tijdstip controleren of tijd controleren door de weersomstandigheden.|
|Internet|Het domein Web biedt de intentie en entiteiten voor het zoeken naar een website.|
