---
title: Wat is de Academic Knowledge-API?
titlesuffix: Azure Cognitive Services
description: Gebruik de Academic Knowledge-API voor het interpreteren van query's van gebruikers en het ophalen van waardevolle informatie uit de Academic Graph.
services: cognitive-services
author: darrine
manager: cgronlun
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: overview
ms.date: 10/30/2018
ms.author: darrine
ms.openlocfilehash: 8575b51bfc3f9013e984ac6c81352a7559439ff2
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55154547"
---
# <a name="academic-knowledge-api"></a>Academic Knowledge-API

Welkom bij de Academic Knowledge-API. Met deze service kunt u query’s van gebruikers interpreteren op academische doeleinden en waardevolle informatie ophalen uit de Microsoft Academic Graph (MAG). De MAG-kennisbank is een heterogene entiteitsgraaf op internet die bestaat uit entiteiten die wetenschappelijk activiteiten modelleren: onderzoeksgebied, auteur, instelling, paper, locatie en gebeurtenis. 

De MAG-gegevens worden afgeleid van de Bing-webindex en van een interne kennisbank van Bing. Als resultaat van de continue indexering door Bing, levert deze API altijd actuele informatie van internet na detectie en indexering door Bing. Op basis van deze gegevensset maken de Academic Knowledge-API's een kennisgedreven, interactieve dialoog mogelijk waarin reactief zoeken naadloos wordt gecombineerd met proactieve suggesties, waardevolle zoekresultaten uit de graaf met onderzoeksverslagen en histogramdistributies van de attribuutwaarden voor een set documenten en gerelateerde entiteiten.

Zie [http://aka.ms/academicgraph](https://aka.ms/academicgraph) voor meer informatie over de Microsoft Academic Graph.

De Academic Knowledge-API is verplaatst van Cognitive Services Preview naar Cognitive Services Labs. De nieuwe startpagina voor het project is [https://labs.cognitive.microsoft.com/en-us/project-academic-knowledge](https://labs.cognitive.microsoft.com/en-us/project-academic-knowledge). Uw bestaande API-sleutel blijft werken tot 24 mei 2018. Na deze datum moet u een nieuwe API-sleutel genereren. De betaalde preview-versie is niet meer beschikbaar wanneer uw bestaande sleutel is verlopen. Neem contact op met ons team als de gratis laag van de API niet toereikend is voor uw doeleinden. 

## <a name="features"></a>Functies
De Academic Knowledge-API bestaat uit vier gerelateerde REST-eindpunten:  
  1. **interpret**: Interpreteert een querytekenreeks van een gebruiker in natuurlijke taal. Retourneert interpretaties met aantekeningen voor een perfecte ervaring bij het automatisch voltooien van tekst die de gebruiker in een zoekvak typt.  
  2. **evaluate**: evalueert een query-expressie en retourneert resultaten in de vorm van Academic Knowledge-entiteiten.  
  3. **calchistogram**: berekent een histogram met de verdeling van attribuutwaarden voor de academische entiteiten die door een query-expressie zijn geretourneerd, zoals de verdeling van uitspraken per jaar van een bepaalde schrijver.  
  
Door deze API-methoden samen te gebruiken, kunt u een geavanceerde ervaring voor semantisch zoeken aanbieden. Aan de hand van een queryreeks van een gebruiker retourneert de methode **interpret** een versie met aantekeningen van de query en een gestructureerde query-expressie. Optioneel kan de query van de gebruiker worden voltooid op basis van de semantiek van de onderliggende academische gegevens. Als een gebruiker bijvoorbeeld de tekenreeks *latente s* typt, kan de methode **interpret** een set gerangschikte interpretaties retourneren die erop kan wijzen dat de gebruiker informatie wil over het onderzoeksgebied  *latente semantische analyse*, de paper *Latente structuuranalyse* of andere entiteitexpressies die beginnen met *latente s*. Deze informatie kan worden gebruikt om de gebruiker snel naar de gewenste zoekresultaten te leiden.

De methode **evaluate** kan worden gebruikt om een set overeenkomende paper-entiteiten op te halen uit de academische kennisbank, terwijl de methode **calchistogram** geschikt is voor het berekenen van de verdeling van attribuutwaarden voor een set paper-entiteiten die kan worden gebruikt om de zoekresultaten verder te filteren.        

## <a name="getting-started"></a>Aan de slag 
Raadpleeg de subonderwerpen aan de linkerkant voor gedetailleerde informatie.  Teneinde de voorbeelden beter leesbaar te maken, bevatten de aanroepen van de REST-API tekens (zoals spaties) die niet zijn gecodeerd als een URL.  In uw code moet echter wel de juiste URL-codering worden toegepast.
