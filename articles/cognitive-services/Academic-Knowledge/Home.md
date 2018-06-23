---
title: Academic Knowledge API voor Microsoft Academic grafiek | Microsoft Docs
description: Gebruik de Academic Knowledge API gebruikersquery interpreteren en uitgebreide informatie ophalen van de grafiek Academic in cognitieve Microsoft-Services.
services: cognitive-services
author: mvorvoreanu
manager: kuansanw
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: article
ms.date: 03/27/2017
ms.author: mivorvor
ms.openlocfilehash: e241f9a87cd58b62eafd754bd3cb4283aa0a1e92
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344728"
---
# <a name="academic-knowledge-api"></a>Academic Knowledge-API

Welkom bij de Academic Knowledge API. Met deze service kunt u query’s van gebruikers interpreteren voor academische doeleinden, en kunt u uitgebreide informatie ophalen uit Microsoft Academic Graph (MAG). De magnetische knowledge base is een web-scale heterogene entiteit grafiek bestaat uit de entiteiten die model wetenschappelijke activiteiten: veld onderzoek, auteur, instelling, papier, u wilt en gebeurtenis. 

De gegevens magnetische mijn verborgen in de Bing-web-index als een intern kennisdatabase van Bing. Als gevolg van continu Bing indexeren, bevat deze API van nieuwe gegevens via het Web na detectie en indexeren door Bing. Op basis van deze gegevensset, de Academic Knowledge API's kunnen een dialoogvenster kennis aangestuurde, interactieve die naadloos reactieve zoeken met proactieve suggestie ervaringen, uitgebreide research papier grafiek zoekresultaten en histogram distributies van combineert de kenmerkwaarden voor een aantal documenten en gerelateerde entiteiten.

Zie voor meer informatie over Academic Microsoft Graph [ http://aka.ms/academicgraph ](http://aka.ms/academicgraph).

De Academic Knowledge API is verplaatst van cognitieve Services Preview naar cognitieve Services Labs. De nieuwe startpagina voor het project is: [ https://labs.cognitive.microsoft.com/en-us/project-academic-knowledge ](https://labs.cognitive.microsoft.com/en-us/project-academic-knowledge). Uw bestaande API-sleutel blijven werken totdat 24th mei 2018. Genereer een nieuwe API-sleutel na deze datum. Houd er rekening mee dat betaald voorbeeld zijn niet langer beschikbaar nadat u uw bestaande sleutel verloopt. Neem contact op met ons team als u de gratis laag van de API is niet voldoende is voor uw doeleinden. 

## <a name="features"></a>Functies
De Academic Knowledge API bestaat uit vier gerelateerde REST-eindpunten:  
  1. **interpreteren** – een queryreeks van natuurlijke taal gebruiker worden geïnterpreteerd. Retourneert interpretaties met aantekeningen voor een perfecte ervaring bij het automatisch voltooien van tekst die de gebruiker in een zoekvak typt.  
  2. **evalueren** : een query-expressie wordt geëvalueerd en Academic Knowledge entiteit resultaten retourneert.  
  3. **calchistogram** – berekent een histogram van de distributie van kenmerkwaarden voor de academic entiteiten die zijn geretourneerd door een query-expressie, zoals de distributie van bronnen door jaar voor een auteur.  
  4. **grafiek zoeken** – zoekopdrachten voor een gegeven grafiek patroon en de overeenkomende entiteit resultaten geretourneerd.

Deze API-methoden kunnen samen worden gebruikt, u een uitgebreide zoekervaring voor semantische maken. Een queryreeks van de gebruiker krijgt de **interpreteren** methode biedt u een aantekeningen versie van de query en een structured query-expressie, tijdens het uitvoeren van query van de gebruiker op basis van de semantiek van de onderliggende academic (optioneel) gegevens. Bijvoorbeeld, als een gebruiker typt de tekenreeks *latente s*, wordt de **interpreteren** methode krijgt u een reeks gerangschikte interpretaties, opmaken dat de gebruiker mogelijk aan het zoeken voor het veld van onderzoek  *latente semantische analyse*, het papier *latente structuur analysis*, of een andere entiteit expressies die beginnen met *latente s*. Deze informatie kan worden gebruikt om te leiden snel de gebruiker naar de gewenste zoekresultaten.

De **evalueren** methode kan worden gebruikt voor het ophalen van een reeks die overeenkomt met entiteiten van de academic knowledge base, artikel en de **calchistogram** methode kan worden gebruikt voor het berekenen van de distributie van kenmerkwaarden voor een set papier entiteiten die kunnen worden gebruikt voor verdere de zoekresultaten te filteren.        

De **grafiek zoeken** methode heeft twee modi: *json* en *lambda*. De *json* modus gebruik van de grafiek jokertekens volgens de grafiek patronen opgegeven door een JSON-object kunt uitvoeren. De *lambda* modus serverzijde berekeningen kunt uitvoeren tijdens de grafiek traversals volgens de gebruiker opgegeven lambda-expressies.

## <a name="getting-started"></a>Aan de slag 
Zie de subonderwerpen aan de linkerkant voor gedetailleerde documentatie.  Houd er rekening mee dat de REST API-aanroepen ter verbetering van de leesbaarheid van de voorbeelden tekens (zoals spaties) die niet URL-codering zijn bevatten.  Uw code moet de juiste URL-coderingen toepassen.
