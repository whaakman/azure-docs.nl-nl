---
title: Verklarende woordenlijst - QnA Maker
titleSuffix: Azure Cognitive Services
description: De QnA Maker-service heeft veel nieuwe voorwaarden van machine learning en natuurlijke taal verwerken, evenals specifieke services. Deze lijst krijgt u inzicht in deze voorwaarden.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 01/14/2019
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: e86ea22208ccd08c0268fda0cc47d5127b4568a1
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55226465"
---
# <a name="glossary-for-qna-maker-knowledge-base-and-service"></a>Verklarende woordenlijst voor QnA Maker knowledge base en service

## <a name="qna-maker-service"></a>QnA Maker-Service
Een QnA Maker-service is een vereiste is om te beginnen met QnA Maker. Het aanschaffen van een laag QnA Maker stelt u de resources in uw Azure-abonnement voor het maken en beheren van uw knowledge base. Elk gebruikersaccount QnA Maker kunt u meerdere QnA Maker-services in hun Azure-abonnement maken.

## <a name="knowledge-base"></a>Knowledge Base
Een kennisdatabase is de opslagplaats van vragen en antwoorden die zijn gemaakt, onderhouden en via de QnA Maker gebruikt. Elke laag QnA Maker kan worden gebruikt voor meerdere Knowledge bases.

## <a name="endpoint"></a>Eindpunt
Een op REST-gebaseerde HTTP-eindpunt uw knowledge base-inhoud die kan worden geïntegreerd in uw toepassing, meestal een chatbot onderhoud. 

## <a name="test-knowledge-base"></a>Knowledge Base testen
Een kennisdatabase heeft twee statussen - Test en gepubliceerd. De test knowledge base is de versie die bewerkt, opgeslagen en getest voor nauwkeurigheid en volledigheid van antwoorden wordt. De eindgebruiker van uw toepassing/chatbot niet van invloed op wijzigingen in de test knowledge base.

## <a name="published-knowledge-base"></a>Gepubliceerde Knowledge Base
Een kennisdatabase heeft twee statussen - Test- en gepubliceerd.  De gepubliceerde knowledge base is de versie die wordt gebruikt in uw bot/toepassing voor chatten. De actie van de publicatie van een kennisdatabase wordt de inhoud van de Test knowledge base geplaatst in de gepubliceerde versie van de knowledge base. Omdat de gepubliceerde knowledge base de versie die gebruikmaakt van de toepassing via het eindpunt is, moet zorgvuldig worden uitgevoerd om ervoor te zorgen dat de inhoud correct en goed geteste is.

## <a name="query"></a>Query’s uitvoeren
De aanvraag voor een gebruiker is de vraag of de eindgebruiker of tester vraagt van de knowledge base. De query is vaak in een paar trefwoorden die staan voor de vraag of de indeling van een natuurlijke taal.

## <a name="response"></a>Antwoord
Het antwoord is het antwoord van de knowledge base, op basis van het meest geschikt is voor de query voor een bepaalde gebruiker opgehaald.

## <a name="confidence-score"></a>Betrouwbaarheidsscore
De betrouwbaarheidsscore van het antwoord is een numerieke waarde tussen 0 en 100, 100 wordt een exacte query overeenkomst tussen gebruiker en een vraag in de knowledge base, die het antwoord is het antwoord juist, geschikt voor een bepaalde gebruiker-query. Antwoorden zijn doorgaans gerangschikt op de betrouwbaarheidsscore en de categorie met de hogere betrouwbaarheidsscore wordt behandeld als het standaardantwoord.
