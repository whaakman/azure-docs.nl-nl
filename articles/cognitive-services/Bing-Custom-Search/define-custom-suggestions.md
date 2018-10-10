---
title: Aangepaste automatische suggesties suggesties - Bing Custom Search definiëren
titlesuffix: Azure Cognitive Services
description: Hierin wordt beschreven hoe het configureren van aangepaste automatische suggestie met aangepaste suggesties
services: cognitive-services
author: brapel
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: conceptual
ms.date: 09/28/2017
ms.author: v-brapel
ms.openlocfilehash: 738f0d6ea083b59cddf408941cda81e980daa334
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2018
ms.locfileid: "48883187"
---
# <a name="configure-your-custom-autosuggest-experience"></a>Configureren van uw aangepaste automatische suggesties-ervaring

Aangepaste automatische suggesties retourneert een lijst met voorgestelde query zoekreeksen die relevant voor uw zoekervaring zijn. De voorgestelde queryreeksen zijn gebaseerd op een gedeeltelijke querytekenreeks waarmee de gebruiker in het zoekvak in. De lijst bevat maximaal 10 suggesties. 

U opgeven of om terug te keren alleen aangepaste suggesties of naar alle Bing suggesties. Als u Bing suggesties opneemt, worden aangepaste suggesties weergegeven voor de Bing-suggesties. Als u onvoldoende relevante suggesties opgeeft, is het mogelijk dat de geretourneerde lijst met suggesties Bing suggesties niet bevat. Suggesties voor Bing zijn altijd in de context van uw aangepaste zoekinstantie. 

Als u wilt zoeken Querysuggesties voor uw exemplaar configureren, klikt u op de **Automatische suggesties** tabblad.  

> [!NOTE]
> Deze functie wilt gebruiken, moet u abonneren op aangepaste zoekopdrachten op het juiste niveau (Zie [prijzen](https://azure.microsoft.com/pricing/details/cognitive-services/bing-custom-search/)).

Duurt maximaal 24 uur voor suggesties worden weergegeven in de voor-eindpunt (API of gehoste gebruikersinterface).

## <a name="enable-bing-suggestions"></a>Bing suggesties inschakelen

Om in te schakelen Bing suggesties, schakelen de **Bing Automatische suggesties** schuifregelaar naar de op positie. De schuifregelaar wordt blauw.

## <a name="add-your-own-suggestions"></a>Uw eigen suggesties toevoegen

Als u wilt toevoegen de suggesties van uw eigen query-tekenreeks, toevoegen aan de lijst onder **zelfgedefinieerde suggesties**. Na het toevoegen van een suggestie wilt doen in de lijst, drukt u op ENTER of klik op de **+** pictogram. U kunt het voorstel opgeven in een andere taal. U kunt maximaal 5000 Querysuggesties tekenreeks toevoegen.

## <a name="upload-suggestions"></a>Suggesties voor uploaden

Desgewenst kunt u een lijst met suggesties uit een bestand uploaden. Het bestand moet een search-queryreeks per regel bevatten. Upload het bestand, klik op het uploadpictogram en selecteert u het bestand te uploaden. De service extraheert de suggesties uit het bestand en voegt deze toe aan de lijst.

## <a name="remove-suggestions"></a>Suggesties verwijderen

Als u wilt verwijderen een querysuggestie-tekenreeks, klikt u op het pictogram verwijderen naast het voorstel die u wilt verwijderen.

## <a name="block-suggestions"></a>Suggesties blokkeren

Als u Bing suggesties opneemt, kunt u een lijst met zoekreeksen van query's die u niet wilt dat Bing om terug te keren toevoegen. Als u wilt toevoegen geblokkeerde queryreeksen, klikt u op **Show geblokkeerd suggesties**. De queryreeks kunt toevoegen aan de lijst en druk op ENTER of klik op de **+** pictogram. U kunt maximaal 50 geblokkeerde queryreeksen toevoegen.



[!INCLUDE[publish or revert](./includes/publish-revert.md)]

>[!NOTE]  
>Het duurt maximaal 24 uur voor aangepaste automatische suggesties configuratiewijzigingen worden doorgevoerd.


## <a name="enabling-autosuggest-in-hosted-ui"></a>Inschakelen van Automatische suggestie in gehoste gebruikersinterface

Als tekenreeks Querysuggesties voor uw gehoste-gebruikersinterface, klikt u op **gebruikersinterface die wordt gehost**. Schuif omlaag naar de **aanvullende configuratie** sectie. Onder **webzoekopdrachten**, selecteer **op** voor **inschakelen Automatische suggestie**. Als u wilt inschakelen Automatische suggesties, moet u een indeling die een zoekvak bevat.


## <a name="calling-the-autosuggest-api"></a>Aanroepen van de Automatische suggestie-API

Als u de voorgestelde querytekenreeksen met behulp van de Bing Custom Search-API, verzendt een `GET` aanvraag naar het volgende eindpunt.

```
GET https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/Suggestions 
```

Het antwoord bevat een lijst met `SearchAction` objecten die de voorgestelde querytekenreeksen bevatten.

```
        {  
            "displayText" : "sailing lessons seattle",  
            "query" : "sailing lessons seattle",  
            "searchKind" : "CustomSearch"  
        },  
```

Alle suggesties bevat een `displayText` en `query` veld. De `displayText` veld bevat de voorgestelde query-tekenreeks die u gebruikt voor het vullen van het zoekvak in de vervolgkeuzelijst.

Als de gebruiker een voorgestelde query-tekenreeks in de vervolgkeuzelijst selecteert, gebruikt u de gebruikte queryreeks in de `query` veld bij het aanroepen van de [Bing Custom Search-API](overview.md).


## <a name="next-steps"></a>Volgende stappen

- [Aangepaste suggesties ophalen](./get-custom-suggestions.md)
- [Zoeken naar uw aangepaste exemplaar](./search-your-custom-view.md)
- [Configureren en gebruiken van aangepaste gehoste UI](./hosted-ui.md)