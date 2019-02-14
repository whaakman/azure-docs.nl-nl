---
title: Configureren van een gehoste gebruikersinterface voor Bing Custom Search | Microsoft Docs
titlesuffix: Azure Cognitive Services
description: Gebruik dit artikel om te configureren en het integreren van een gehoste gebruikersinterface voor Bing Custom Search.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: conceptual
ms.date: 02/12/2019
ms.author: aahi
ms.openlocfilehash: fbe865a5d9ef6c44b80c811a023c86f6446c3bb8
ms.sourcegitcommit: de81b3fe220562a25c1aa74ff3aa9bdc214ddd65
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56233877"
---
# <a name="configure-your-hosted-ui-experience"></a>Configureren van uw gehoste gebruikersinterface-ervaring

Bing Custom Search biedt een gehoste-gebruikersinterface die u eenvoudig in uw webpagina's en web-apps als een JavaScript-codefragment integreren kunt. Gebruik Bing Custom Search-portal kunt kunt u de lay-out, kleur en opties voor het doorzoeken van de gebruikersinterface configureren.



## <a name="configure-the-custom-hosted-ui"></a>De aangepaste gehoste UI configureren

Volg deze stappen voor het configureren van een gehoste UI voor uw webtoepassingen. Als u wijzigingen aanbrengt, geeft het deelvenster aan de rechterkant u een voorbeeld van uw gebruikersinterface. De weergegeven lijst met zoekresultaten zijn niet de werkelijke resultaten voor uw exemplaar.

1. Aanmelden bij Bing Custom Search [portal](https://customsearch.ai).  
  
2. Selecteer uw exemplaar van Bing Custom Search.

3. Klik op het tabblad **Hosted UI**.  
  
4. Selecteer een lay-out.

    |  |  |
    |---------|---------|
    |Zoekbalk en de resultaten (standaard)    | Een zoekvak met zoekresultaten eronder weergegeven.         |
    |Alleen resultaten     | Geeft de zoekresultaten, zonder een zoekvak. Wanneer u deze lay-out, moet u de zoekopdracht (`&q=<query string>`). De queryparameter toevoegen aan de aanvraag-URL in het JavaScript-fragment of het eindpunt HTML-koppeling.        |
    |Pop-boven     | Biedt een zoekvak en de resultaten voor zoeken in een sliding overlay weergegeven.        |
    
5. Selecteer een kleurenthema. U kunt de kleuren aanpassen aan uw toepassing door te klikken op **aanpassen thema**. Als u wilt een kleur wijzigen, een hexadecimale RGB-waarde van de kleur invoeren (bijvoorbeeld `#366eb8`), of klik op de kleur Preview-versie.

  U kunt uw wijzigingen aan de rechterkant van de portal bekijken. Te klikken op **standaardinstellingen herstellen** uw wijzigingen aan de standaardkleuren voor het geselecteerde thema wordt hersteld.

  > [!NOTE]
  > Houd rekening met toegankelijkheid bij het kiezen van kleuren.

6. Onder **aanvullende configuraties**, geef de waarden waar nodig voor uw app. Deze instellingen zijn optioneel. Het effect van het toepassen of verwijderen, Zie het voorbeeldvenster aan de rechterkant. Beschikbare opties zijn:  

7. Voer de abonnementssleutel zoeken of kies een van de vervolgkeuzelijst. De vervolgkeuzelijst wordt gevuld met de sleutels van de abonnementen van uw Azure-account. Zie [Cognitive Services-API-account](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account).  

8. Als u Automatische suggesties hebt ingeschakeld, wordt de automatische suggesties-abonnementssleutel invoeren of kies een van de vervolgkeuzelijst. De vervolgkeuzelijst wordt gevuld met de sleutels van de abonnementen van uw Azure-account. Aangepaste automatische suggesties vereist een specifiek abonnement-laag, Zie de [prijzen](https://azure.microsoft.com/pricing/details/cognitive-services/bing-custom-search/).

[!INCLUDE [publish or revert](./includes/publish-revert.md)]

## <a name="consume-custom-ui"></a>Aangepaste UI gebruiken

De gehoste-gebruikersinterface, ofwel gebruiken: 

- Het script in uw webpagina's opnemen  
  
  ```html
  <html>
      <body>
          <script type="text/javascript" 
              id="bcs_js_snippet"
              src="https://ui.customsearch.ai /api/ux/rendering-js?customConfig=<YOUR-CUSTOM-CONFIG-ID>&market=en-US&safeSearch=Moderate&version=latest&q=">
          </script>
      </body>    
  </html>
  ```

- Of gebruik de volgende URL in een webbrowser.   
  
  `https://ui.customsearch.ai/hosted?customConfig=YOUR-CUSTOM-CONFIG-ID`  
  
  > [!NOTE]
  > De volgende queryparameters toevoegen aan de URL indien nodig. Zie voor meer informatie over deze parameters [Custom Search-API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference#query-parameters) verwijzing.
  >
  > - q
  > - mkt
  > - veilig zoeken
  > - setlang

  > [!IMPORTANT]
  > De pagina kan uw privacyverklaring of andere kennisgevingen en voorwaarden niet weergeven. Geschiktheid voor uw gebruik kan variëren.  

Voor meer informatie, met inbegrip van uw aangepaste configuratie-ID, gaat u naar **eindpunten** onder de **productie** tabblad.

## <a name="configuration-options"></a>Configuratie-opties

U kunt het gedrag van uw gehoste gebruikersinterface configureren door te klikken op **aanvullende configuraties**, en geef dezelfde waarden op. Deze instellingen zijn optioneel. Het effect van het toepassen of verwijderen, Zie het voorbeeldvenster aan de rechterkant. 

### <a name="web-search-configurations"></a>Web-zoekconfiguraties

|  |  |
|---------|---------|
|Webresultaten ingeschakeld    | Bepaalt of de zoeken op Internet is ingeschakeld (ziet u een Web-tabblad aan de bovenkant van de pagina)        |
|Schakel Automatische suggestie     | Hiermee bepaalt u als aangepaste automatische suggesties is ingeschakeld (Zie [prijzen](https://azure.microsoft.com/pricing/details/cognitive-services/bing-custom-search/) voor extra kosten).        |
|Resultaten per pagina    | Het aantal zoekresultaten voor webpagina's om weer te geven op een tijdstip (het maximum is 50 resultaten per pagina).        |
|Bijschrift van afbeelding   | Bepaalt of installatiekopieën met zoekresultaten worden weergegeven.|


De volgende configuraties worden weergegeven als u klikt op **geavanceerde configuraties weergeven**:


|  | |
|---------|---------|
|Markeer woorden     | Bepaalt of de resultaten worden weergegeven met de zoektermen vet weergegeven.         |
|Koppelingsdoel    |  Hiermee bepaalt u als de webpagina wordt geopend in een nieuw browsertabblad (leeg) of het tabblad met dezelfde browser (zelf) wanneer de gebruiker op een zoekresultaat.        |

### <a name="image-search-configurations"></a>Installatiekopie-zoekconfiguraties

| | |
|---------|---------|
|Afbeeldingsresultaten ingeschakeld     | Bepaalt of de afbeeldingen zoeken is ingeschakeld (ziet u een tabblad installatiekopieën aan de bovenkant van de pagina).            |
|Afbeeldingsresultaten per pagina     | Het aantal zoekresultaten voor installatiekopieën om weer te geven op een tijdstip (het maximum aantal is 150 resultaten per pagina).          |

De volgende configuratie wordt weergegeven als u klikt op **geavanceerde configuraties weergeven**.  
  
| | |
|---------|---------|
| Filters inschakelen     | Toevoegen van filters die de gebruiker gebruiken kunt voor het filteren van de installatiekopieën die Bing retourneert. De gebruiker kan bijvoorbeeld de resultaten filteren voor alleen GIF-animaties.|

### <a name="video-search-configurations"></a>Video's zoeken-configuraties

|  | |
|---------|---------|
|Videoresultaten ingeschakeld     | Bepaalt of de video's zoeken is ingeschakeld (ziet u een tabblad video's aan de bovenkant van de pagina).           |
|Video resultaten per pagina   | Het aantal resultaten om weer te geven op een tijdstip (het maximum aantal is 150 resultaten per pagina).        |

De volgende configuratie wordt weergegeven als u klikt op **geavanceerde configuraties weergeven**.  
  
|  | |
|---------|---------|
|Filters inschakelen    | Toevoegen van filters die de gebruiker gebruiken kunt voor het filteren van de video's die Bing retourneert. De gebruiker kan bijvoorbeeld de resultaten voor video's met een specifieke oplossingsstatus of video's die zijn gedetecteerd in de afgelopen 24 uur filteren.          |

### <a name="miscellaneous-configurations"></a>Diverse configuraties


| |  |
|---------|---------|
|Paginatitel   | De tekst die wordt weergegeven in het gebied van de titel van de pagina met zoekresultaten (niet voor pop-over-indeling).        |
|Werkbalk thema    | Bepaalt de achtergrondkleur van het titelgebied van de pagina met zoekresultaten. |

De volgende configuraties worden weergegeven als u klikt op **geavanceerde configuraties weergeven**.  

|Kolom1  |Kolom2  |
|---------|---------|
|Tijdelijke aanduiding vak Zoeken   | De tekst die wordt weergegeven in het vak Zoeken vóór de invoer.        |
|Url voor de titel van de koppeling    |Doel voor de Titelkoppeling.         |
|Logo-URL     | Afbeelding die wordt weergegeven naast de titel.         |
|Favicon    | Pictogram weergegeven in de titelbalk van de browser.          |

De volgende configuraties van toepassing alleen als u de gebruikersinterface die wordt gehost via het HTML-eindpunt (ze zijn niet van toepassing als u de JavaScript-fragment) gebruiken.

- Paginatitel
- Werkbalk thema
- Titel van de URL van koppeling
- Logo-URL
- Faviicon URL  

## <a name="next-steps"></a>Volgende stappen

- [Decoratiemarkeringen gebruiken om tekst te markeren](./hit-highlighting.md)
- [Bladeren door webpagina's](./page-webpages.md)
