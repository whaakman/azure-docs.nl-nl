---
title: Vooraf gebouwde domein verwijzing-LUIS
titleSuffix: Azure Cognitive Services
description: Referentie voor de vooraf gemaakte domeinen vooraf gemaakte verzamelingen van intenties en entiteiten van Language Understanding Intelligent Services (LUIS zijn).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 05/07/2019
ms.author: diberry
ms.openlocfilehash: 6e701396191ec8bf66ece0e2858b9f32857e7b17
ms.sourcegitcommit: 13a289ba57cfae728831e6d38b7f82dae165e59d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/09/2019
ms.locfileid: "68933587"
---
# <a name="prebuilt-domain-reference-for-your-luis-app"></a>Vooraf ontwikkelde domein referentie voor uw LUIS-app
Deze referentie bevat informatie over de [vooraf gemaakte domeinen](luis-how-to-use-prebuilt-domains.md), die zijn vooraf gemaakte verzamelingen van intenties en entiteiten die LUIS biedt.

[Aangepaste domeinen](luis-how-to-start-new-app.md), daarentegen, beginnen met geen intents en modellen. U kunt een vooraf gedefinieerde domein intenties en entiteiten toevoegen aan een aangepast model.

## <a name="supported-domains-across-cultures"></a>Ondersteunde domeinen in cult uren

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
|Agenda|Agenda is alles over persoonlijke vergaderingen en afspraken, _geen_ open bare gebeurtenissen (zoals planningen voor de wereld) of algemene kalenders (zoals de dag waarop het vandaag is, wat betekent dat het gaat om een dag van arbeid).|
|Communicatie|Aanvragen voor het maken van aanroepen, het verzenden van teksten of expres berichten, het zoeken en toevoegen van contact personen en diverse andere communicatie aanvragen (doorgaans uitgaand). Contact persoon naam alleen query's die geen deel uitmaken van het communicatie domein.|
|Email|E-mail is een subdomein van het communicatie domein. Het hoofd zakelijk bevat aanvragen voor het verzenden en ontvangen van berichten via e-mail.|
|HomeAutomation|Het domein HomeAutomation biedt intenties en entiteiten die betrekking hebben op het beheren van Smart-Home-apparaten. Hoofd zakelijk ondersteunt de controle opdracht met betrekking tot verlichting en airconditioning, maar heeft een aantal generalisatie mogelijkheden voor andere elektrische apparaten.|
|Opmerkingen|Opmerking: het domein biedt intenties en entiteiten voor het maken van notities en het schrijven van items voor gebruikers.|
|Plaatsen|Locaties zijn onder andere bedrijven, instituten, restaurants, open bare ruimten en adressen. Het domein biedt ondersteuning voor het zoeken naar en vragen over de informatie van een open bare locatie, zoals location, Operating hours and distance.|
|RestaurantReservation|Domein voor restaurant reservering ondersteunt intenties voor het verwerken van reserve ringen voor restaurants.|
|Taken|ToDo-domein bevat typen taak lijsten die gebruikers kunnen toevoegen, markeren en verwijderen van hun TODO-items.|
|Nutsbedrijven|Het domein hulpprogram ma's is een algemeen domein tussen alle LUIS-modellen met algemene intenties en uitingen in verschillen scenario's.|
|Weer|Weer domein is gericht op het controleren van weers omstandigheden en adviseurs met de locatie en tijd of de controle tijd op basis van weer situaties.|
|Web|Het webdomein biedt de intentie en entiteiten voor het zoeken naar een website.|
