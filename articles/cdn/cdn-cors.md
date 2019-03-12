---
title: Azure CDN gebruiken met CORS | Microsoft Docs
description: Leer hoe u met Cross-Origin Resource Sharing (CORS) voor het Azure Content Delivery Network (CDN) om te gebruiken.
services: cdn
documentationcenter: ''
author: zhangmanling
manager: erikre
editor: ''
ms.assetid: 86740a96-4269-4060-aba3-a69f00e6f14e
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 3c8fab85d71f5f81bbf81bc3dd7a22d6c0b7f11b
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57551837"
---
# <a name="using-azure-cdn-with-cors"></a>Azure CDN gebruiken met CORS
## <a name="what-is-cors"></a>Wat is CORS?
CORS (Cross-Origin Resource Sharing) is een HTTP-functie waarmee een webtoepassing die wordt uitgevoerd in een bepaald domein te krijgen tot bronnen in een ander domein. Als u wilt verminderen de kans op aanvallen via cross-site scripting, alle moderne webbrowsers implementeren van een beveiligingsbeperking bekend als [beleid voor zelfde oorsprong](https://www.w3.org/Security/wiki/Same_Origin_Policy).  Dit voorkomt dat een webpagina van aanroepen van API's in een ander domein.  CORS biedt een veilige manier om toe te staan een bron (het domein van oorsprong) API's aanroepen in een andere oorsprong.

## <a name="how-it-works"></a>Hoe werkt het?
Er zijn twee soorten CORS-aanvragen, *eenvoudige aanvragen* en *complexe aanvragen.*

### <a name="for-simple-requests"></a>Voor eenvoudige aanvragen:

1. De browser verzendt de CORS-aanvraag met een extra **oorsprong** header HTTP-aanvraag. De waarde van deze header is de oorsprong die de bovenliggende pagina, die is gedefinieerd als de combinatie van behandeld *-protocol,* *domein* en *poort.*  Als een pagina van https://www.contoso.com probeert te krijgen tot gegevens van een gebruiker in de oorsprong fabrikam.com, de volgende aanvraagheader naar fabrikam.com worden verzonden:

   `Origin: https://www.contoso.com`

2. De server reageert met een van de volgende:

   * Een **Access-Control-Allow-Origin** -header in de reactie waarmee wordt aangegeven welke bronsite is toegestaan. Bijvoorbeeld:

     `Access-Control-Allow-Origin: https://www.contoso.com`

   * Een HTTP-foutcode 403 als de server niet toe de aanvraag voor cross-origin dat staat nadat u hebt gecontroleerd van de header van oorsprong

   * Een **Access-Control-Allow-Origin** -header met een jokerteken waarmee alle oorsprongen:

     `Access-Control-Allow-Origin: *`

### <a name="for-complex-requests"></a>Voor complexe aanvragen:

De aanvraag voor een complexe is een CORS-aanvraag waarin de browser is vereist voor het verzenden van een *voorbereidende aanvraag* (dat wil zeggen, een voorlopige test) voordat de werkelijke CORS-aanvraag wordt verzonden. De voorbereidende aanvraag toestemming wordt gevraagd de server als de oorspronkelijke CORS aanvragen kan worden voortgezet en wordt een `OPTIONS` aanvraag voor de dezelfde URL.

> [!TIP]
> Zie voor meer informatie over CORS flows en veel voorkomende valkuilen de [handleiding voor het CORS voor REST-API's](https://www.moesif.com/blog/technical/cors/Authoritative-Guide-to-CORS-Cross-Origin-Resource-Sharing-for-REST-APIs/).
>
>

## <a name="wildcard-or-single-origin-scenarios"></a>Jokertekens of één oorsprong scenario 's
CORS in Azure CDN werkt automatisch zonder aanvullende configuratie als de **Access-Control-Allow-Origin** header is ingesteld op het jokerteken (*) of een één oorsprong.  Het CDN wordt de eerste reactie in cache en elke volgende keer dat dezelfde koptekst wordt gebruikt.

Als al aanvragen aan het CDN voordat u CORS in uw bron wordt ingesteld aangebracht zijn, moet u het wissen van inhoud op uw eindpunt inhoud laden van de inhoud met de **Access-Control-Allow-Origin** header.

## <a name="multiple-origin-scenarios"></a>Scenario's met meerdere oorsprong
Als u toestaan dat een specifieke lijst met oorsprongen voor CORS worden toegestaan wilt, krijgt u dingen iets gecompliceerder. Het probleem treedt op wanneer het CDN slaat de **Access-Control-Allow-Origin** -header voor de eerste CORS-oorsprong.  Wanneer een andere CORS-oorsprong een volgende aanvraag, maakt het CDN de in de cache moet fungeren **Access-Control-Allow-Origin** koptekst, die niet overeen met.  Er zijn verschillende manieren om dit te corrigeren.

### <a name="azure-cdn-premium-from-verizon"></a>Azure CDN Premium van Verizon
De beste manier om dit te gebruiken is **Azure CDN Premium van Verizon**, waarmee wordt aangegeven dat sommige geavanceerde functies. 

Moet u [maakt u een regel](cdn-rules-engine.md) om te controleren of de **oorsprong** header op de aanvraag.  Als het een geldige oorsprong, de regel wordt ingesteld de **Access-Control-Allow-Origin** -header met de oorsprong die is opgegeven in de aanvraag.  Als de oorsprong is opgegeven in de **oorsprong** header is niet toegestaan, de regel moet laat de **Access-Control-Allow-Origin** header, waardoor de browser om de aanvraag af te wijzen. 

Er zijn twee manieren om dit te doen met de regelengine. In beide gevallen wordt de **Access-Control-Allow-Origin** koptekst met behulp van de oorsprong van de bestandsserver wordt genegeerd en regels-engine van het CDN beheert volledig. de toegestane oorsprongen voor CORS.

#### <a name="one-regular-expression-with-all-valid-origins"></a>Een reguliere expressie met alle geldige oorsprongen
In dit geval maakt u een reguliere expressie met alle van de oorsprongen die u wilt toestaan: 

    https?:\/\/(www\.contoso\.com|contoso\.com|www\.microsoft\.com|microsoft.com\.com)$

> [!TIP]
> **Azure CDN Premium van Verizon** maakt gebruik van [Perl compatibel reguliere expressies](https://pcre.org/) als de engine voor reguliere expressies.  U kunt een hulpprogramma zoals [reguliere expressies 101](https://regex101.com/) voor het valideren van de reguliere expressie.  Houd er rekening mee dat het teken '/' is ongeldig in reguliere expressies en hoeft niet te worden weergegeven, maar escape-teken wordt beschouwd als een best practice en door sommige validators reguliere expressie wordt verwacht.
> 
> 

Als de reguliere expressie die overeenkomt met de regel wordt vervangen door de **Access-Control-Allow-Origin** koptekst (indien aanwezig) met behulp van de oorsprong met de oorsprong die de aanvraag heeft verzonden.  U kunt ook aanvullende CORS-headers, zoals toevoegen **Access-Control-toestaan-Methods**.

![Voorbeeld van de regels met reguliere expressie](./media/cdn-cors/cdn-cors-regex.png)

#### <a name="request-header-rule-for-each-origin"></a>Regel voor het aanvragen header voor de oorsprong.
In plaats van reguliere expressies, kunt u een afzonderlijke regel voor elke bron die u wilt verlenen met behulp van in plaats daarvan maakt de **aanvragen Header jokertekens** [overeenkomen met de voorwaarde](https://msdn.microsoft.com/library/mt757336.aspx#Anchor_1). Net als bij de methode reguliere expressie stelt de zelfstandig regelengine de CORS-headers. 

![Voorbeeld van de regels zonder reguliere expressie](./media/cdn-cors/cdn-cors-no-regex.png)

> [!TIP]
> In het bovenstaande voorbeeld wordt het gebruik van het jokerteken * vertelt de regelengine zodat deze overeenkomt met HTTP en HTTPS.
> 
> 

### <a name="azure-cdn-standard-profiles"></a>Azure standard CDN-profielen
Op de standaard Azure CDN-profielen (**Azure CDN Standard van Microsoft**, **Azure CDN Standard van Akamai**, en **Azure CDN Standard van Verizon**), de enige mechanisme zonder het gebruik van de oorsprong jokerteken is het gebruik van verschillende oorsprongen toestaan [queryreeksen opslaan in cache](cdn-query-string.md). De query-tekenreeks-instelling voor het CDN-eindpunt inschakelen en vervolgens een unieke queryreeks gebruiken voor aanvragen van elk domein. In dat geval zal resulteren in het CDN caching van een afzonderlijke-object voor elke unieke queryreeks. Deze benadering is echter niet ideaal zijn, omdat dit meerdere kopieën van hetzelfde bestand in de cache opgeslagen op het CDN leidt.  

