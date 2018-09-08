---
title: 'Bing Custom Search: Definieer aangepaste automatische suggesties suggesties | Microsoft Docs'
description: Hierin wordt beschreven hoe het configureren van aangepaste automatische suggestie met aangepaste suggesties
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.technology: bing-custom-search
ms.topic: article
ms.date: 09/28/2017
ms.author: v-brapel
ms.openlocfilehash: e7a62a79bdc2e486fb6bfca34eb4addeba2bde0e
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44158310"
---
# <a name="configure-your-custom-autosuggest-experience"></a>Configureren van uw aangepaste automatische suggesties-ervaring
Als u geabonneerd op aangepaste zoekopdrachten op het juiste niveau (Zie de [prijspagina's](https://azure.microsoft.com/pricing/details/cognitive-services/bing-custom-search/)), kunt u de zoeksuggesties gemaakt in uw aangepaste zoekopdrachten-ervaring aanpassen. Aangepaste automatische suggesties retourneert een lijst met voorgestelde query's op basis van een gedeeltelijke querytekenreeks waarmee de gebruiker. Met aangepaste automatische suggesties, kunt u aangepaste zoeksuggesties relevant zijn voor uw zoekervaring opgeven. U opgeven of om terug te keren alleen aangepaste suggesties of naar alle Bing suggesties. Als u Bing suggesties opneemt, worden aangepaste suggesties weergegeven voor de Bing-suggesties. Suggesties voor Bing zijn beperkt tot de context van uw aangepaste zoekinstantie.

## <a name="configure-custom-autosuggest"></a>Configureren van aangepaste automatische suggesties
Gebruik de volgende instructies voor het configureren van aangepaste automatische suggesties voor uw aangepaste zoekinstantie.

1.  Aanmelden bij [Custom Search](https://customsearch.ai).
2.  Klik op de instantie van een aangepaste zoekopdrachten. Zie voor het maken van een exemplaar [maken van uw eerste exemplaar van de Bing Custom Search](quick-start.md).
3.  Klik op de **Automatische suggesties** tabblad.

## <a name="enable-bing-suggestions"></a>Bing suggesties inschakelen
Om in te schakelen Bing suggesties, schakelen de **Bing Automatische suggesties** schuifregelaar naar de op positie. De schuifregelaar wordt blauw.

## <a name="add-suggestions"></a>Suggesties toevoegen
Als u wilt toevoegen een suggestie wilt doen, voert u deze in het tekstvak. Druk op ENTER of klik op de **+** pictogram. Aangepaste suggesties kunnen in elke taal en voor Bing suggesties wordt weergegeven.

## <a name="upload-suggestions"></a>Suggesties voor uploaden
U kunt een lijst met suggesties uit een bestand uploaden. Elke suggestie plaatsen op een afzonderlijke regel. Klik op het uploadpictogram en selecteer het bestand.

## <a name="remove-suggestions"></a>Suggesties verwijderen
Als u wilt verwijderen een suggestie wilt doen, klikt u op het pictogram verwijderen naast het voorstel die u wilt verwijderen.

[!INCLUDE [publish or revert](./includes/publish-revert.md)]

  >[!NOTE]  
  >Het duurt maximaal 24 uur voor aangepaste automatische suggesties configuratiewijzigingen worden doorgevoerd.

## <a name="next-steps"></a>Volgende stappen

- [Aangepaste suggesties ophalen](./get-custom-suggestions.md)
- [Zoeken naar uw aangepaste exemplaar](./search-your-custom-view.md)
- [Configureren en gebruiken van aangepaste gehoste UI](./hosted-ui.md)