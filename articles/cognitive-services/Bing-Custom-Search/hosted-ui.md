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
ms.openlocfilehash: 7f2b97479ffcdb7ec8b3a1a635562d1fe68c3269
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44158404"
---
# <a name="configure-your-hosted-ui-experience"></a>Configureren van uw gehoste gebruikersinterface-ervaring
Na het configureren van uw exemplaar voor aangepast zoeken, kunt u de aangepaste zoeken-API om de zoekresultaten en ze weergeven in uw app aanroepen. Of, als de app een web-app is, kunt u de gehoste-gebruikersinterface waarmee u aangepaste zoekopdrachten.   

## <a name="configure-custom-hosted-ui"></a>Configureren van aangepaste gehoste UI
Gebruik de volgende instructies voor het configureren van een gehoste gebruikersinterface om op te nemen in uw web-app.
1.  Aanmelden bij Custom Search [portal](https://customsearch.ai).
2.  Klik op de instantie van een aangepaste zoekopdrachten. Zie voor het maken van een exemplaar [maken van uw eerste exemplaar van de Bing Custom Search](quick-start.md).
3.  Klik op de **gebruikersinterface die wordt gehost** tabblad.
4.  Selecteer een indeling.
    - Zoekbalk en de resultaten (standaard) &mdash; weer een zoekvak en zoekresultaten
    - Alleen resultaten &mdash; niet weergeven het zoekvak, alleen resultaten weergeven
    - Pop-over &mdash; niet weergeven het zoekvak, alleen resultaten weergeven in een sliding overlay
    
   > [!IMPORTANT]
   > Zorg ervoor dat u de queryparameter customConfig bij het selecteren van de **alleen resultaten** lay-out, Zie [queryparameters](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference#query-parameters).

5.  Onder **aanvullende configuraties**, geef de waarden waar nodig voor uw app. Deze instellingen zijn optioneel. Het effect van het toepassen of verwijderen, Zie het voorbeeldvenster aan de rechterkant.  Beschikbare opties zijn:
    - Web-zoekconfiguraties:
        - Web-resultaten ingeschakeld &mdash; bepaalt als zoekresultaten voor webpagina's worden geretourneerd
        - Schakel Automatische suggestie &mdash; bepaalt of aangepaste automatische suggesties is ingeschakeld
        - Web-resultaten per pagina &mdash; aantal zoekresultaten voor webpagina's om weer te geven op een tijdstip
        - Bijschrift bij afbeelding &mdash; bepaalt of installatiekopieën met zoekresultaten worden weergegeven
        - Markeer woorden &mdash; bepaalt of de resultaten worden weergegeven met de zoektermen in vet
    - Installatiekopie-zoekconfiguraties:
        - Afbeelding van resultaten ingeschakeld &mdash; bepaalt als resultaten worden geretourneerd
    - Diverse configuraties:
        - Titel pagina &mdash; tekst die wordt weergegeven in het gedeelte met de titel
        - Werkbalk thema &mdash; bepaalt de achtergrondkleur van het gedeelte met de titel
        - Box-tijdelijke aanduiding voor tekst zoeken &mdash; tekst die wordt weergegeven in het vak Zoeken vóór de invoer
        - Url voor de titel van de koppeling &mdash; doel voor de van Titelkoppeling
        - Url van logo &mdash; afbeelding die wordt weergegeven naast de titel 
        - Favicon url &mdash; pictogram weergegeven in de titelbalk van de browser

   > [!IMPORTANT]
   > Ten minste één van de afbeeldingen zoeken of zoeken op het Web moet zijn ingeschakeld.

6.  Voer de abonnementssleutel zoeken of kiezen in de vervolgkeuzelijst. De vervolgkeuzelijst wordt gevuld met sleutels uit uw account van Azure-abonnementen. Zie [Cognitive Services-API-account](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account).
7.  Als u Automatische suggesties hebt ingeschakeld, de automatische suggesties-abonnementssleutel invoeren of kies een van de vervolgkeuzelijst. De vervolgkeuzelijst wordt gevuld met sleutels uit uw account van Azure-abonnementen. Aangepaste automatische suggesties vereist een abonnement specifiec laag, Zie de [prijspagina's](https://azure.microsoft.com/pricing/details/cognitive-services/bing-custom-search/).

> [!NOTE]
> Als u wijzigingen in de configuratie van de aangepaste gehoste UI aanbrengt, bevat het deelvenster aan de rechterkant een visuele referentie voor wijzigingen in de. De weergegeven lijst met zoekresultaten zijn niet de werkelijke resultaten voor uw exemplaar

[!INCLUDE [publish or revert](./includes/publish-revert.md)]

## <a name="consume-custom-ui"></a>Aangepaste UI gebruiken
De gehoste-gebruikersinterface, ofwel gebruiken: 

- Het script in uw webpagina's opnemen
    ``` html
    <html>
        <body>
            <script type="text/javascript"
                id="bcs_js_snippet"            
                src="https://ui.customsearch.ai/api/ux/render?customConfig=<YOUR-CUSTOM-CONFIG-ID>&market=en-US&safeSearch=Moderate">            
            </script>
        </body>    
    </html>
    ```

- De opgegeven URL gebruiken `https://ui.customsearch.ai/hosted?customConfig=YOUR-CUSTOM-CONFIG-ID`

  > [!IMPORTANT]
  > De pagina kan uw privacyverklaring of andere kennisgevingen en voorwaarden niet weergeven. Geschiktheid voor uw gebruik kan variëren.  

Voor meer informatie, met inbegrip van uw aangepaste configuratie-ID, gaat u naar **eindpunten** onder de **productie** tabblad.

## <a name="next-steps"></a>Volgende stappen
- [Decoration markeringen gebruiken om te markeren van tekst](./hit-highlighting.md)
- [Pagina webpagina 's](./page-webpages.md)