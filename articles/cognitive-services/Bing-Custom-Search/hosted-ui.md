---
title: ': Bing aangepaste Site zoeken | Microsoft Docs'
description: Beschrijft hoe u configureert gehoste gebruikersinterface
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 09/28/2017
ms.author: v-brapel
ms.openlocfilehash: 593ea4d23f8ddcec8efc4be632afa2aab1a5210f
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344906"
---
# <a name="configure-your-hosted-ui-experience"></a>Configureer uw gehoste UI-mogelijkheden
U kunt de aangepaste zoeken-API om de zoekresultaten en weer te geven in uw app aanroepen na het configureren van uw aangepaste zoekactie-exemplaar. Of als uw app in een WebApp is, kunt u de gehoste gebruikersinterface waarmee aangepaste zoekactie.   

## <a name="configure-custom-hosted-ui"></a>Aangepaste gehoste UI configureren
Gebruik de volgende instructies voor het configureren van een gehoste gebruikersinterface wilt opnemen in uw web-app.
1.  Aanmelden bij de aangepaste zoekactie [portal](https://customsearch.ai).
2.  Klik op de instantie van een aangepaste zoekactie. Zie het maken van een exemplaar [maken van uw eerste exemplaar van Bing aangepaste zoekactie](quick-start.md).
3.  Klik op de **UI gehost** tabblad.
4.  Selecteer een indeling.
    - Zoekbalk en resultaten (standaard) &mdash; een zoekvak weergeven en zoekresultaten
    - Alleen resultaten &mdash; niet een zoekvak weergeven, alleen resultaten weergeven
    - Pop-over &mdash; niet een zoekvak weergeven, alleen resultaten weergeven in een sliding overlay
    
   > [!IMPORTANT]
   > Zorg ervoor dat de queryparameter customConfig bij het selecteren van de **alleen resulteert** lay-out, Zie [queryparameters](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference#query-parameters).

5.  Onder **aanvullende configuraties**, waarden geschikt is voor uw app opgeven. Deze instellingen zijn optioneel. Het effect van toe te passen of te verwijderen, Zie het voorbeeldvenster aan de rechterkant.  Beschikbare opties zijn:
    - Web search configuraties:
        - Web-resultaten ingeschakeld &mdash; bepaalt als zoekresultaten worden geretourneerd
        - Inschakelen voor Automatische suggestie &mdash; bepaalt of voor Automatische suggestie van aangepaste is ingeschakeld
        - Web-resultaten per pagina &mdash; aantal zoekresultaten om weer te geven op een tijdstip
        - Afbeelding bijschrift &mdash; bepaalt of de afbeeldingen worden weergegeven met zoekresultaten
        - Markeer woorden &mdash; bepaalt of de resultaten met zoektermen vet worden weergegeven
    - Afbeelding zoekconfiguraties:
        - Afbeelding van resultaten ingeschakeld &mdash; bepaalt als afbeelding-zoekresultaten worden geretourneerd
    - Diverse configuraties:
        - Titel pagina &mdash; tekst die wordt weergegeven in het gebied van de titel pagina
        - Werkbalk thema &mdash; bepaalt de achtergrondkleur van het gebied van de titel pagina
        - Vak-tijdelijke aanduiding voor tekst zoeken &mdash; tekst die wordt weergegeven in het vak Zoeken vóór de invoer
        - Url voor de titel van de koppeling &mdash; doel voor de Titelkoppeling
        - Logo url &mdash; afbeelding die wordt weergegeven naast de titel 
        - Favicon url &mdash; pictogram dat wordt weergegeven in de titelbalk van de browser

   > [!IMPORTANT]
   > Ten minste één van de afbeelding of webzoekopdracht moet zijn ingeschakeld.

6.  Geef de zoeksleutel abonnement of maak een keuze uit de vervolgkeuzelijst. De vervolgkeuzelijst wordt gevuld met de sleutels van uw account Azure-abonnementen. Zie [cognitieve Services API account](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account).
7.  Als u Automatische suggestie hebt ingeschakeld, voert u de sleutel voor Automatische suggestie abonnement of maak een keuze uit de vervolgkeuzelijst. De vervolgkeuzelijst wordt gevuld met de sleutels van uw account Azure-abonnementen. Aangepaste automatische suggestie vereist een abonnement specifiec laag, Zie de [prijzen van pagina's](https://azure.microsoft.com/pricing/details/cognitive-services/bing-custom-search/).

> [!NOTE]
> Als u wijzigingen in de configuratie van de aangepaste gehoste UI aanbrengt, biedt het paneel aan de rechterkant een visuele referentie voor wijzigingen in de. De weergegeven lijst met zoekresultaten zijn niet werkelijke resultaten voor uw exemplaar

[!INCLUDE[publish or revert](./includes/publish-revert.md)]

## <a name="consume-custom-ui"></a>Aangepaste UI gebruiken
De gehoste-gebruikersinterface ofwel gebruiken: 

- Het script opnemen in uw webpagina
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
  > De pagina kan de privacyverklaring of andere meldingen en voorwaarden niet weergeven. Kan variëren, geschiktheid voor gebruik.  

Voor meer informatie, met inbegrip van uw aangepaste configuratie-ID, gaat u naar **eindpunten** onder de **productie** tabblad.

## <a name="next-steps"></a>Volgende stappen
- [Decoration markeringen gebruiken om te markeren van tekst](./hit-highlighting.md)
- [Pagina webpagina 's](./page-webpages.md)