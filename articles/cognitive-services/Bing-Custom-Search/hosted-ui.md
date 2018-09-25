---
title: 'Bing Custom Search: Zoek | Microsoft Docs'
description: Beschrijft hoe u configureert via de gebruikersinterface
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 09/28/2017
ms.author: v-brapel
ms.openlocfilehash: 2aec8ba969fb639f2d785a429441f6ed4bbf7dfb
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46987676"
---
# <a name="configure-your-hosted-ui-experience"></a>Configureren van uw gehoste gebruikersinterface-ervaring

Na het configureren van uw exemplaar voor aangepast zoeken, kunt u de aangepaste zoeken-API om de zoekresultaten en ze weergeven in uw app aanroepen. Of, als de app een web-app is, kunt u de gehoste-gebruikersinterface waarmee u aangepaste zoekopdrachten.   

## <a name="configure-custom-hosted-ui"></a>Configureren van aangepaste gehoste UI

Volg deze stappen voor het configureren van een gehoste UI voor uw web-app:

1. Aanmelden bij Custom Search [portal](https://customsearch.ai).  
  
2. Klik op de instantie van een aangepaste zoekopdrachten. Zie voor het maken van een exemplaar [maken van uw eerste exemplaar van de Bing Custom Search](quick-start.md).  

3. Klik op de **gebruikersinterface die wordt gehost** tabblad.  
  
4. Selecteer een indeling.
  
  - Zoekbalk en de resultaten (standaard) &mdash; deze lay-out is uw pagina traditionele zoeken met het zoekvak en een lijst met zoekresultaten.
  - Alleen resultaten &mdash; deze lay-out alleen zoekresultaten worden weergegeven. Deze lay-out weergegeven niet een zoekvak. U moet de query opgeven door de query-parameter toe te voegen (& q =\<querytekenreeks >) aan de aanvraag-URL in het JavaScript-fragment of HTML-eindpunt-koppeling.
  - Pop-over &mdash; deze lay-out biedt een zoekvak en de lijst met zoekresultaten in een sliding overlay worden weergegeven.
      
5. Selecteer een kleurenthema. De mogelijke thema's zijn: 
  
  - Klassiek
  - Donker
  - Skyline blauw

  Klik op elk van de thema's om te zien welk thema u het beste werkt met uw web-app. Als u nodig hebt voor het afstemmen van de kleurthema beter integreren met uw web-app, klikt u op **aanpassen thema**. Niet alle kleur configuraties van toepassing op alle lay-out-thema's. Als u wilt een kleur wijzigen, voert u de kleur van de hexadecimale RGB-waarde (bijvoorbeeld #366eb8) in het bijbehorende tekstvak. Of klik op de kleurknop en klik vervolgens op de tint die bij u past. 
  
  Na het wijzigen van een kleur, bekijk hoe de wijziging is van invloed op de preview-voorbeeld aan de rechterkant. U kunt altijd klikken op **standaardinstellingen herstellen** terug te gaan naar de standaardkleuren voor het geselecteerde thema.

  > [!NOTE]
  > Wanneer u de kleurthema wijzigt, houd rekening met toegankelijkheid bij het kiezen van kleuren.

5. Onder **aanvullende configuraties**, geef de waarden waar nodig voor uw app. Deze instellingen zijn optioneel. Het effect van het toepassen of verwijderen, Zie het voorbeeldvenster aan de rechterkant. Beschikbare opties zijn:  
  
  - Web-zoekconfiguraties:
    - Web-resultaten ingeschakeld &mdash; bepaalt of de zoeken op Internet is ingeschakeld (ziet u een Web-tabblad aan de bovenkant van de pagina).
    - Schakel Automatische suggestie &mdash; bepaalt of aangepaste automatische suggesties is ingeschakeld (Zie [prijzen](https://azure.microsoft.com/pricing/details/cognitive-services/bing-custom-search/) voor extra kosten).
    - Web-resultaten per pagina &mdash; aantal zoekresultaten voor webpagina's om weer te geven op een tijdstip (het maximum is 50 resultaten per pagina).
    - Bijschrift bij afbeelding &mdash; bepaalt of installatiekopieën met zoekresultaten worden weergegeven.
  
    De volgende configuraties worden weergegeven als u klikt op **geavanceerde configuraties weergeven**.  
  
    - Markeer woorden &mdash; bepaalt of de resultaten met zoektermen in vet worden weergegeven. 
    - Koppelingsdoel &mdash; bepaalt als de webpagina wordt geopend in een nieuw browsertabblad (leeg) of het tabblad met dezelfde browser (zelf) wanneer de gebruiker op een zoekresultaat. 

  - Installatiekopie-zoekconfiguraties:
    - Afbeelding van resultaten ingeschakeld &mdash; bepaalt of de afbeeldingen zoeken is ingeschakeld (ziet u een tabblad installatiekopieën aan de bovenkant van de pagina).   
    - Afbeelding van de resultaten per pagina &mdash; aantal zoekresultaten voor installatiekopieën om weer te geven op een tijdstip (het maximum aantal is 150 resultaten per pagina).  
  
    De volgende configuratie wordt weergegeven als u klikt op **geavanceerde configuraties weergeven**.  
  
    - Filters inschakelen &mdash; filters die de gebruiker gebruiken kan voor het filteren van de installatiekopieën die Bing retourneert toegevoegd. De gebruiker kan bijvoorbeeld de resultaten filteren voor alleen GIF-animaties.

  - Video's zoeken-configuraties:
    - Videoresultaten ingeschakeld &mdash; bepaalt of de video's zoeken is ingeschakeld (ziet u een tabblad video's aan de bovenkant van de pagina).  
    - Video resultaten per pagina &mdash; aantal video's zoekresultaten om weer te geven op een tijdstip (het maximum aantal is 150 resultaten per pagina).
  
    De volgende configuratie wordt weergegeven als u klikt op **geavanceerde configuraties weergeven**.  
  
    - Filters inschakelen &mdash; filters die de gebruiker gebruiken kan voor het filteren van de video's die Bing retourneert toegevoegd. De gebruiker kan bijvoorbeeld de resultaten voor video's met een specifieke oplossingsstatus of video's die zijn gedetecteerd in de afgelopen 24 uur filteren.

  - Diverse configuraties:
    - Titel pagina &mdash; tekst die wordt weergegeven in het gebied van de titel van de pagina met zoekresultaten (niet voor pop-over-indeling).
    - Werkbalk thema &mdash; bepaalt de achtergrondkleur van het titelgebied van de pagina met zoekresultaten.  
  
    De volgende configuraties worden weergegeven als u klikt op **geavanceerde configuraties weergeven**.  
  
    - Box-tijdelijke aanduiding voor tekst zoeken &mdash; tekst die wordt weergegeven in het vak Zoeken vóór de invoer.
    - Url voor de titel van de koppeling &mdash; doel voor de Titelkoppeling.
    - Url van logo &mdash; afbeelding die wordt weergegeven naast de titel. 
    - Favicon url &mdash; pictogram weergegeven in de titelbalk van de browser.  

    De volgende configuraties van toepassing alleen als u de gebruikersinterface die wordt gehost via het HTML-eindpunt (ze zijn niet van toepassing als u de JavaScript-fragment) gebruiken.
    
    - Paginatitel
    - Werkbalk thema
    - Titel van de URL van koppeling
    - Logo-URL
    - Faviicon-URL  
  
6. Voer de abonnementssleutel zoeken of kies een van de vervolgkeuzelijst. De vervolgkeuzelijst wordt gevuld met de sleutels van de abonnementen van uw Azure-account. Zie [Cognitive Services-API-account](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account).  

7. Als u Automatische suggesties hebt ingeschakeld, wordt de automatische suggesties-abonnementssleutel invoeren of kies een van de vervolgkeuzelijst. De vervolgkeuzelijst wordt gevuld met de sleutels van de abonnementen van uw Azure-account. Aangepaste automatische suggesties vereist een specifiek abonnement-laag, Zie de [prijzen](https://azure.microsoft.com/pricing/details/cognitive-services/bing-custom-search/).

> [!NOTE]
> Als u wijzigingen in de configuratie van de aangepaste gehoste UI aanbrengt, bevat het deelvenster aan de rechterkant een visuele referentie voor wijzigingen in de. De weergegeven lijst met zoekresultaten zijn niet de werkelijke resultaten voor uw exemplaar.

[!INCLUDE[publish or revert](./includes/publish-revert.md)]

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
  > - Mkt
  > - veilig zoeken
  > - taal instellen

  > [!IMPORTANT]
  > De pagina kan uw privacyverklaring of andere kennisgevingen en voorwaarden niet weergeven. Geschiktheid voor uw gebruik kan variëren.  

Voor meer informatie, met inbegrip van uw aangepaste configuratie-ID, gaat u naar **eindpunten** onder de **productie** tabblad.

## <a name="next-steps"></a>Volgende stappen

- [Decoration markeringen gebruiken om te markeren van tekst](./hit-highlighting.md)
- [Pagina webpagina 's](./page-webpages.md)