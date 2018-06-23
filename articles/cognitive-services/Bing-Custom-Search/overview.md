---
title: Wat is Bing aangepaste zoekactie? | Microsoft Docs
description: Biedt een totaaloverzicht van Bing aangepaste zoekactie
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 09/29/2017
ms.author: v-brapel
ms.openlocfilehash: 7cd61fc63d0d7734b842ed222c67c6753da9a418
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344907"
---
# <a name="what-is-bing-custom-search"></a>Wat is Bing aangepaste zoekactie?

Bing aangepaste zoeken kunt u voor het maken van de ervaringen op maat gemaakte zoeken naar onderwerpen die u interesseren. Bijvoorbeeld, als u een website die u een zoekfunctie kunt beschikt, kunt u de domeinen, websites en webpagina's met Bing worden gezocht. Uw gebruikers zien zoekresultaten wilt weergeven die zijn toegesneden op de ze in plaats van interesseren inhoud naar de pagina met zoekresultaten wilt weergeven die niet relevant inhoud bevatten.

Voor het maken van de aangepaste weergave van het web gebruikt u de aangepaste Bing-zoekopdracht [portal](https://customsearch.ai). De portal maakt u een aangepaste zoekactie-exemplaar waarin de domeinen, websites, en webpagina's die u wilt dat Bing om te zoeken en de websites die u niet wilt dat deze om te zoeken. Naast het opgeven van de URL's van de inhoud die u kent, kunt u de portal ook gebruiken vinden relevante inhoud die u wilt toevoegen.

De portal kunt u ook een specifieke webpagina op de bovenkant van het zoekresultaat vast als de gebruiker een bepaalde zoekterm invoert. 

Na het definiëren van uw exemplaar, kunt u aangepaste zoekactie integreren in uw website, bureaublad-app of mobiele app door de aangepaste zoeken-API aanroept. Als u een web gebaseerde site of toepassing hebt, kunt u de gehoste-gebruikersinterface weergegeven van de zoekinterface voor u.

De volgende afbeelding toont de eenvoud van de integratie van aangepaste zoekactie.

![afbeelding alt](./media/bcs-overview.png "hoe Bing aangepaste zoekactie werkt.")

## <a name="customize-search-suggestions"></a>Zoeksuggesties aanpassen

Als u zich hebt geabonneerd aangepaste zoekactie op het juiste niveau (Zie de [prijzen van pagina's](https://azure.microsoft.com/pricing/details/cognitive-services/bing-custom-search/)), kunt u de zoeksuggesties aangebracht in uw aangepaste zoekervaring aanpassen. De aangepaste voor Automatische suggestie API retourneert een lijst met voorgestelde query's op basis van een gedeeltelijke queryreeks waarmee de gebruiker. Met aangepaste voor Automatische suggestie, kunt u aangepaste zoeksuggesties relevant zijn voor de zoekfunctie opgeven. U opgeven of alleen aangepaste suggesties retourneren of Bing suggesties bevatten. Als Bing suggesties opgenomen zijn, worden aangepaste suggesties weergegeven voordat de suggesties die Bing biedt. Bing suggesties zijn beperkt tot de context van het exemplaar van uw aangepaste zoekactie.

## <a name="next-steps"></a>Volgende stappen

Om snel aan de slag, Zie [maken van uw eerste exemplaar van Bing aangepaste zoekactie](quick-start.md).

Zie voor meer informatie over de beschikbare opties voor het aanpassen van uw exemplaar van de zoekopdracht [definiëren van een aangepaste Zoekexemplaar](define-your-custom-view.md).

Vertrouwd raken met de [aangepaste Search API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference) verwijzing. De verwijzing bevat de lijst met eindpunten, kopteksten en queryparameters die u gebruiken wilt om aan te vragen van de zoekresultaten. Dit omvat ook de definities van de antwoord-objecten.

Zie voor meer informatie over het aanpassen van suggesties, [aangepaste zoeksuggesties definiëren](define-custom-suggestions.md).

Lees [Bing gebruiken en de vereisten van de weergave](./use-and-display-requirements.md) zodat u de regels over het gebruik van de zoekresultaten niet verbreken.