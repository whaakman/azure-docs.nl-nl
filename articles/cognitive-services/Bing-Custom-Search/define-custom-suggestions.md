---
title: 'Bing aangepaste zoekactie: Suggesties voor Automatische suggestie van aangepaste definiëren | Microsoft Docs'
description: Beschrijft hoe u configureert aangepaste voor Automatische suggestie met aangepaste suggesties
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.technology: bing-custom-search
ms.topic: article
ms.date: 09/28/2017
ms.author: v-brapel
ms.openlocfilehash: a41b4e5b6c268ec68488c6764d4192cf8d2345a4
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344896"
---
# <a name="configure-your-custom-autosuggest-experience"></a>De ervaring van uw aangepaste automatische suggestie configureren
Als u zich hebt geabonneerd aangepaste zoekactie op het juiste niveau (Zie de [prijzen van pagina's](https://azure.microsoft.com/pricing/details/cognitive-services/bing-custom-search/)), kunt u de zoeksuggesties aangebracht in uw aangepaste zoekervaring aanpassen. Aangepaste automatische suggestie retourneert een lijst met voorgestelde query's op basis van een gedeeltelijke queryreeks waarmee de gebruiker. Met aangepaste voor Automatische suggestie, kunt u aangepaste zoeksuggesties relevant zijn voor de zoekfunctie opgeven. U opgeven om terug te keren alleen aangepaste suggesties of naar alle Bing suggesties. Als u suggesties Bing opneemt, worden aangepaste suggesties verschijnen vóór de Bing-suggesties. Bing suggesties zijn beperkt tot de context van het exemplaar van uw aangepaste zoekactie.

## <a name="configure-custom-autosuggest"></a>Aangepaste configureren voor Automatische suggestie van
Gebruik de volgende instructies om te configureren voor Automatische suggestie van aangepaste voor uw aangepaste zoekactie-exemplaar.

1.  Aanmelden bij [aangepaste zoekactie](https://customsearch.ai).
2.  Klik op de instantie van een aangepaste zoekactie. Zie het maken van een exemplaar [maken van uw eerste exemplaar van Bing aangepaste zoekactie](quick-start.md).
3.  Klik op de **Automatische suggestie** tabblad.

## <a name="enable-bing-suggestions"></a>Bing suggesties inschakelen
Om in te schakelen Bing suggesties, schakelt de **automatische Bing suggesties** schuifregelaar naar de op positie. De schuifregelaar wordt blauw.

## <a name="add-suggestions"></a>Suggesties toevoegen
Als u wilt een suggestie toevoegen, moet u het invoeren in het tekstvak. Druk op ENTER, of klik op de **+** pictogram. Aangepaste suggesties kunnen in elke taal en voordat Bing suggesties wordt weergegeven.

## <a name="upload-suggestions"></a>Suggesties uploaden
U kunt een lijst met suggesties uit een bestand uploaden. Elke suggestie plaatsen op een aparte regel. Klik op het pictogram uploaden en selecteer uw bestand.

## <a name="remove-suggestions"></a>Suggesties verwijderen
Als u wilt een suggestie verwijderen, klikt u op het pictogram verwijderen naast de suggestie die u wilt verwijderen.

[!INCLUDE[publish or revert](./includes/publish-revert.md)]

  >[!NOTE]  
  >Het kan tot 24 uur voor wijzigingen in de configuratie voor Automatische suggestie van aangepaste kracht te laten duren.

## <a name="next-steps"></a>Volgende stappen

- [Aangepaste suggesties ophalen](./get-custom-suggestions.md)
- [Uw aangepaste exemplaar zoeken](./search-your-custom-view.md)
- [Configureren en gebruiken van aangepaste gehoste UI](./hosted-ui.md)