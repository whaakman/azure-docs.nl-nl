---
title: Verklarende woordenlijst - cognitieve Services van Microsoft | Microsoft Docs
titleSuffix: Azure
description: Woordenlijst
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 04/21/2018
ms.author: saneppal
ms.openlocfilehash: e28cddec005cb6ba99b9f60d8b03a11f1bc97062
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345316"
---
# <a name="glossary"></a>Woordenlijst

## <a name="qna-maker-service"></a>QnA Maker-Service
Een QnA Maker-service is een vereiste aan de slag met QnA Maker. Het aanschaffen van een laag QnA Maker stelt u de resources in uw Azure-abonnement maken en beheren van uw knowledge base. Elk gebruikersaccount QnA Maker kunt meerdere QnA Maker-services in Azure-abonnement maken.

## <a name="knowledge-base"></a>Kennisdatabase
Een Knowledge base is de opslagplaats van vragen en antwoorden gemaakt, onderhouden en via QnA Maker gebruikt. Elke QnA Maker-laag kan worden gebruikt voor meerdere kennis-databases.

## <a name="endpoint"></a>Eindpunt
Een REST gebaseerde HTTP-eindpunt uw knowledge base-inhoud die kan worden geïntegreerd in uw toepassing, meestal een bot chat onderhoud. 

## <a name="test-knowledge-base"></a>Kennisdatabase testen
Een knowledge base heeft twee statussen - Test en gepubliceerd. De test knowledge base is de versie die bewerkt, opgeslagen en geteste voor nauwkeurigheid en volledigheid van antwoorden wordt. Wijzigingen in de test knowledge base is niet van invloed op de eindgebruiker van uw toepassing/chat bot.

## <a name="published-knowledge-base"></a>Gepubliceerd Knowledge Base
Een knowledge base heeft twee statussen - Test- en gepubliceerd.  De gepubliceerde knowledge base is de versie die wordt gebruikt in uw chat bot/toepassing. De actie van publicatie van een knowledge base plaatst de inhoud van de Test knowledge base in de gepubliceerde versie van de knowledge base. Omdat de gepubliceerde knowledge base de versie die de toepassing wordt gebruikt door het eindpunt is, moet nauwkeurig om ervoor te zorgen dat de inhoud correct en goed getest is.

## <a name="query"></a>Query’s uitvoeren
Een gebruikersquery is de vraag die de eindgebruiker of tester vraagt van de knowledge base. De query is vaak in een paar trefwoorden met daarin de vraag of de indeling van een natuurlijke taal.

## <a name="response"></a>Antwoord
Het antwoord is het antwoord opgehaald uit de knowledge base, op basis van het meest geschikt is voor een bepaalde gebruikersquery.

## <a name="confidence-score"></a>Betrouwbaarheidsscore
De score van de betrouwbaarheid van het antwoord is een numerieke waarde tussen 0 en 100, wordt een query exacte overeenkomst is tussen gebruikersquery en een vraag in kB, die het antwoord behandeld 100 is de juiste, de juiste reactie voor een bepaalde gebruikersquery. Antwoorden zijn doorgaans gerangschikt op de score vertrouwen en de categorie met de hogere betrouwbaarheid-score wordt behandeld als het standaardantwoord.
