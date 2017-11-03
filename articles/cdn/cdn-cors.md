---
title: Azure CDN gebruiken met CORS | Microsoft Docs
description: Informatie over het gebruik van het Azure Content Delivery Network (CDN) aan met Cross-Origin-Resource delen (CORS).
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 86740a96-4269-4060-aba3-a69f00e6f14e
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 7070397f6e69b21add75bad8220f0b8ebe36d266
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="using-azure-cdn-with-cors"></a>Azure CDN gebruiken met CORS
## <a name="what-is-cors"></a>Wat is CORS?
CORS (Cross Origin Resource Sharing) is een HTTP-functie waarmee een webtoepassing in een domein met toegang tot bronnen in een ander domein. Om het risico te verkleinen van aanvallen via cross-site scripting, alle moderne webbrowsers implementeren een beveiligingsbeperkingen bekend als [dezelfde oorsprong beleid](http://www.w3.org/Security/wiki/Same_Origin_Policy).  Dit voorkomt dat een webpagina van aanroepen API's in een ander domein.  CORS biedt een veilige manier om toe te staan van één bron (het domein van de oorsprong) API's in een andere bron aanroepen.

## <a name="how-it-works"></a>Hoe werkt het?
Er zijn twee soorten CORS aanvragen, *eenvoudige aanvragen* en *complexe aanvragen.*

### <a name="for-simple-requests"></a>Voor eenvoudige aanvragen:

1. De browser verzendt de CORS-aanvraag met een extra **oorsprong** header HTTP-aanvraag. De waarde van deze header is de oorsprong die de bovenliggende pagina, die is gedefinieerd als de combinatie van behandeld *protocol* *domein,* en *poort.*  Wanneer een pagina uit https://www.contoso.com probeert te krijgen tot de gegevens van een gebruiker in de oorsprong fabrikam.com, zou u de volgende aanvraagheader verzonden naar fabrikam.com:

   `Origin: https://www.contoso.com`

2. De server reageert met het volgende:

   * Een **Access Control-toestaan-oorsprong** header in zijn reactie waarmee wordt aangegeven welke bronsite is toegestaan. Bijvoorbeeld:

     `Access-Control-Allow-Origin: https://www.contoso.com`

   * Een HTTP-foutcode zoals 403 als de server niet toe de cross-origin-aanvraag dat staat na het controleren van de header van oorsprong

   * Een **Access Control-toestaan-oorsprong** koptekst met een jokerteken waarmee alle oorsprongen:

     `Access-Control-Allow-Origin: *`

### <a name="for-complex-requests"></a>Voor complexe aanvragen:

Een complexe aanvraag is een aanvraag CORS waarvoor de browser is vereist voor het verzenden van een *voorbereidende aanvraag* (dat wil zeggen een voorlopige test) voordat u de werkelijke CORS-aanvraag verzendt. De voorbereidende aanvraag toestemming wordt gevraagd de server als de oorspronkelijke CORS aanvragen kan worden voortgezet en wordt een `OPTIONS` aanvraag naar dezelfde URL.

> [!TIP]
> Voor meer informatie over de CORS-stromen en verrassingen, bekijkt u de [handleiding voor het CORS voor REST-API's](https://www.moesif.com/blog/technical/cors/Authoritative-Guide-to-CORS-Cross-Origin-Resource-Sharing-for-REST-APIs/).
>
>

## <a name="wildcard-or-single-origin-scenarios"></a>Jokertekens of één oorsprong scenario 's
CORS op Azure CDN werkt automatisch zonder aanvullende configuratie als de **Access Control-toestaan-oorsprong** header is ingesteld op het jokerteken (*) of een enkel oorsprong.  De CDN cache worden opgeslagen door de eerste reactie en volgende aanvragen gebruik van dezelfde kop.

Als aanvragen al naar de CDN voordat CORS wordt ingesteld aangebracht zijn op de uw oorsprong, moet u opschonen van de inhoud van de inhoud van uw eindpunt opnieuw laden van de inhoud met de **Access Control-toestaan-oorsprong** header.

## <a name="multiple-origin-scenarios"></a>Scenario's met meerdere oorsprong
Als u toestaan dat een specifieke lijst met oorsprongen wilt moet worden toegestaan voor CORS, krijgen die dingen iets gecompliceerder. Het probleem treedt op wanneer de CDN plaatst de **Access Control-toestaan-oorsprong** -header voor de eerste CORS-oorsprong.  Wanneer een andere CORS-oorsprong een volgende aanvraag indient, de CDN fungeert de in de cache **Access Control-toestaan-oorsprong** koptekst, die niet overeen met.  Er zijn verschillende manieren om dit te corrigeren.

### <a name="azure-cdn-premium-from-verizon"></a>Azure CDN Premium van Verizon
De beste manier om dit te gebruiken is **Azure CDN Premium van Verizon**, die beschrijft sommige geavanceerde functies. 

U moet [maakt u een regel](cdn-rules-engine.md) om te controleren de **oorsprong** header voor de aanvraag.  Als het een geldige oorsprong, de regel stelt de **Access Control-toestaan-oorsprong** header met de opgegeven in de aanvraag van de oorsprong.  Als de oorsprong is opgegeven in de **oorsprong** header is niet toegestaan, de regel moet weglaten de **Access Control-toestaan-oorsprong** header waardoor de browser om de aanvraag af te wijzen. 

Er zijn twee manieren om dit te doen met de regelengine.  In beide gevallen moet de **Access Control-toestaan-oorsprong** koptekst van de oorsprong van de bestandsserver volledig wordt genegeerd, de toegestane oorsprongen van CORS volledig worden beheerd door het CDN regelengine.

#### <a name="one-regular-expression-with-all-valid-origins"></a>Een reguliere expressie met alle geldige oorsprongen
In dit geval maakt u een reguliere expressie waarin alle van de oorsprongen die u wilt toestaan: 

    https?:\/\/(www\.contoso\.com|contoso\.com|www\.microsoft\.com|microsoft.com\.com)$

> [!TIP]
> **Azure CDN van Verizon** gebruikt [Perl compatibel reguliere expressies](http://pcre.org/) als de engine voor reguliere expressies.  U kunt een hulpprogramma zoals [reguliere expressies 101](https://regex101.com/) valideren van de reguliere expressie.  Houd er rekening mee dat het teken '/' is ongeldig in reguliere expressies en niet moet worden voorafgegaan, echter escape-teken wordt beschouwd als een best practice en wordt verwacht door sommige validatiefuncties regex.
> 
> 

Als de reguliere expressie overeenkomt, de regel wordt vervangen door de **Access Control-toestaan-oorsprong** header (indien aanwezig) vanuit de oorsprong met de oorsprong die de aanvraag heeft verzonden.  U kunt ook aanvullende CORS-kopteksten, zoals toevoegen **-besturingselement-toestaan-toegangsmethoden**.

![Voorbeeld van de regels met de reguliere expressie](./media/cdn-cors/cdn-cors-regex.png)

#### <a name="request-header-rule-for-each-origin"></a>Aanvraag-header-regel voor elke oorsprong.
In plaats van reguliere expressies, kunt u een afzonderlijke regel voor elke bron die u wilt toestaan met behulp van in plaats daarvan maakt de **aanvraag-Header jokertekens** [overeenkomen met de voorwaarde](https://msdn.microsoft.com/library/mt757336.aspx#Anchor_1). Net als bij de methode reguliere expressie, stelt de motor regels de CORS-headers. 

![Voorbeeld van de regels zonder reguliere expressie](./media/cdn-cors/cdn-cors-no-regex.png)

> [!TIP]
> In het bovenstaande voorbeeld wordt het gebruik van het jokerteken * vertelt regelengine voor de overeenkomen met zowel HTTP als HTTPS.
> 
> 

### <a name="azure-cdn-standard"></a>Azure CDN Standard
Op Azure CDN Standard profielen, is het enige mechanisme om toe te staan voor meerdere oorsprongen zonder het gebruik van de oorsprong jokerteken is het gebruik van [query opslaan in cache](cdn-query-string.md).  U moet query tekenreeks instelling inschakelen voor het CDN-eindpunt en een unieke queryreeks vervolgens gebruiken voor aanvragen van elk domein. Dit resulteert in de cache van een afzonderlijk object voor elke unieke queryreeks CDN. Deze aanpak is echter niet ideaal, zoals het tot meerdere exemplaren van hetzelfde bestand in cache op de CDN leiden zal.  

