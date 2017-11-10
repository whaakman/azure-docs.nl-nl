---
title: Beveiligen van Azure CDN activa met tokenverificatie | Microsoft Docs
description: Informatie over het gebruik van tokenverificatie voor beveiligde toegang tot uw assets Azure CDN.
services: cdn
documentationcenter: .net
author: zhangmanling
manager: zhangmanling
editor: 
ms.assetid: 837018e3-03e6-4f9c-a23e-4b63d5707a64
ms.service: cdn
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 11/03/2017
ms.author: mezha
ms.openlocfilehash: 2f62c0c6783c3cdaf1ffda3299673071b8e4a6f2
ms.sourcegitcommit: dcf5f175454a5a6a26965482965ae1f2bf6dca0a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/10/2017
---
# <a name="securing-azure-content-delivery-network-assets-with-token-authentication"></a>Azure Content Delivery Network activa met tokenverificatie beveiligen

[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>Overzicht

Tokenverificatie is een mechanisme waarmee u om te voorkomen dat het Azure Content Delivery Network (CDN) voor de activa aan niet-geautoriseerde clients. Tokenverificatie gewoonlijk wordt uitgevoerd om te voorkomen dat 'hotlinking' van inhoud, waarop een andere website, vaak een mededelingenbord uw assets zonder toestemming gebruikt. Hotlinking kunnen een invloed hebben op de kosten voor het leveren van inhoud. Doordat tokenverificatie op CDN aanvragen door rand CDN POP's geverifieerd voordat het CDN de inhoud biedt. 

## <a name="how-it-works"></a>Hoe werkt het?

Tokenverificatie verifieert dat aanvragen worden gegenereerd door een vertrouwde site doordat aanvragen voor dat bevat informatie over de aanvrager van het gecodeerde token waarde bevat. Inhoud is geleverd voor een aanvrager alleen als de gecodeerde gegevens voldoet aan de vereisten; anders worden aanvragen geweigerd. U kunt de vereisten instellen met behulp van een of meer van de volgende parameters:

- Land: Toestaan of weigeren van aanvragen die afkomstig uit de landen die is opgegeven zijn door hun [landcode](https://msdn.microsoft.com/library/mt761717.aspx).
- URL: Toestaan dat alleen aanvragen die overeenkomen met de opgegeven asset of het pad.
- Host: Toestaan of weigeren van aanvragen die gebruikmaken van de opgegeven hosts in de aanvraagheader.
- Verwijzende site: Toestaan of weigeren van aanvraag van de opgegeven verwijzende site.
- IP-adres: alleen aanvragen die afkomstig van specifieke IP-adres of IP-subnet zijn toestaan.
- Protocol: Toestaan of weigeren op basis van het protocol dat wordt gebruikt om aan te vragen van de inhoud aanvragen.
- Verlooptijd: een bepaalde datum en tijd om ervoor te zorgen dat er een koppeling geldig alleen voor een beperkte periode blijft toewijzen.

Zie voor meer informatie de gedetailleerde configuratie-voorbeelden voor elke parameter in [tokenverificatie instellen](#setting-up-token-authentication).

## <a name="reference-architecture"></a>Referentiearchitectuur

Het volgende werkstroomdiagram wordt beschreven hoe de CDN tokenverificatie werken met uw web-app gebruikt.

![CDN-tokenverificatie werkstroom](./media/cdn-token-auth/cdn-token-auth-workflow2.png)

## <a name="token-validation-logic-on-cdn-endpoint"></a>Token validatielogica op CDN-eindpunt
    
Het volgende stroomdiagram wordt beschreven hoe Azure CDN clientaanvraag valideert wanneer tokenverificatie is geconfigureerd op de CDN-eindpunt.

![De token validatielogica CDN](./media/cdn-token-auth/cdn-token-auth-validation-logic.png)

## <a name="setting-up-token-authentication"></a>Token verificatie instellen

1. Van de [Azure-portal](https://portal.azure.com), blader naar uw CDN-profiel en klik vervolgens op **beheren** starten van de aanvullende portal.

    ![Knop voor CDN-profiel beheren](./media/cdn-rules-engine/cdn-manage-btn.png)

2. Beweeg de muisaanwijzer over **HTTP grote**, en klik vervolgens op **Token Auth** in het doel. U kunt vervolgens instellen van de versleutelingssleutel en versleuteling parameters als volgt:

    1. Een of meer versleutelingssleutels maken. Een coderingssleutel is hoofdlettergevoelig en kan een combinatie van alfanumerieke tekens bevatten. Andere soorten tekens, inclusief spaties, zijn niet toegestaan. De maximale lengte is 250 tekens. Om ervoor te zorgen dat uw coderingssleutels willekeurige zijn, wordt het aanbevolen dat u deze met het hulpprogramma OpenSSL maakt. Het hulpprogramma OpenSSL heeft de volgende syntaxis: `rand -hex <key length>`. Bijvoorbeeld `OpenSSL> rand -hex 32`. Om te voorkomen uitvaltijd, maakt u een primaire en een back-sleutel. Een back-sleutel biedt ononderbroken toegang tot uw inhoud wanneer de primaire sleutel wordt bijgewerkt.
    
    2. Voer een unieke coderingssleutel in de **primaire sleutel** in en voert u desgewenst een back-sleutel in de **back-sleutel** vak.

    3. Selecteer de versie van de minimale codering voor elke sleutel van de **minimumversie versleuteling** dropdown lijst en klik vervolgens op **Update**:
       - **V2**: geeft aan dat de sleutel voor het genereren versie 2.0 en 3.0 tokens kan worden gebruikt. Gebruik deze optie alleen als u zijn overgang van een oudere versie 2.0-versleutelingssleutel met een sleutel versie 3.0.
       - **V3**: (aanbevolen) geeft aan dat de sleutel kan alleen worden gebruikt voor het genereren versie 3.0-tokens.

    ![CDN-setup van token verificatiesleutel](./media/cdn-token-auth/cdn-token-auth-setupkey.png)
    
    4. Gebruik het hulpprogramma coderen voor versleuteling parameters instellen en het genereren van een token. U kunt met het hulpprogramma versleutelen toestaan of weigeren van aanvragen op basis van de verlooptijd, land, verwijzende site, het protocol en client-IP (in een willekeurige combinatie). Hoewel er geen limiet voor het aantal en de combinatie van parameters die kunnen worden gecombineerd om een token, is de totale lengte van een token beperkt tot 512 tekens. 

       ![CDN hulpprogramma versleutelen](./media/cdn-token-auth/cdn-token-auth-encrypttool.png)

       Voer waarden in voor een of meer van de volgende parameters versleuteling in de **versleutelen hulpprogramma** sectie:  

       - **ec_expire**: een verlooptijd wijst naar een token, waarna het token verloopt. Aanvragen verzonden nadat de vervaltijd worden geweigerd. Deze parameter wordt gebruikt voor een Unix-tijdstempel die is gebaseerd op het aantal seconden sinds de standard-epoche van `1/1/1970 00:00:00 GMT`. (U kunt online hulpprogramma's gebruiken om te converteren tussen stnd. tijd- en Unix-tijd.) Bijvoorbeeld, als u wilt dat het token verloopt op `12/31/2016 12:00:00 GMT`, gebruikt u de waarde van het tijdstempel Unix `1483185600`als volgt. 
    
         ![CDN ec_expire voorbeeld](./media/cdn-token-auth/cdn-token-auth-expire2.png)
    
       - **ec_url_allow**: u kunt aangeven van tokens, voor een bepaalde asset of het pad. Dit beperkt de toegang tot aanvragen waarvan de URL beginnen met een specifieke relatief pad. URL's zijn hoofdlettergevoelig. Voer meerdere paden door elk pad scheiden met komma's. Afhankelijk van uw vereisten kunt u verschillende waarden instellen voor verschillende niveau van toegang. 
        
         Bijvoorbeeld: voor de URL `http://www.mydomain.com/pictures/city/strasbourg.png`, deze aanvragen zijn toegestaan voor de volgende invoerwaarden:

         - Invoerwaarde `/`: alle aanvragen zijn toegestaan.
         - Invoerwaarde `/pictures`, mogen de volgende aanvragen:
            - `http://www.mydomain.com/pictures.png`
            - `http://www.mydomain.com/pictures/city/strasbourg.png`
            - `http://www.mydomain.com/picturesnew/city/strasbourgh.png`
         - Invoerwaarde `/pictures/`: alleen aanvragen die de `/pictures/` pad zijn toegestaan. Bijvoorbeeld `http://www.mydomain.com/pictures/city/strasbourg.png`.
         - Invoerwaarde `/pictures/city/strasbourg.png`: alleen aanvragen voor deze specifieke pad en de asset zijn toegestaan.
    
       - **ec_country_allow**: kunt alleen aanvragen die afkomstig uit een of meer opgegeven landen zijn. Aanvragen die afkomstig van alle andere landen zijn worden geweigerd. Landcodes en elkaar met een komma te scheiden. Bijvoorbeeld, als u wilt toegang toestaan via alleen in de Verenigde Staten en Frankrijk, voer ons FR in het vak als volgt.  
        
           ![CDN ec_country_allow voorbeeld](./media/cdn-token-auth/cdn-token-auth-country-allow.png)

       - **ec_country_deny**: aanvragen die afkomstig uit een of meer opgegeven landen zijn. Aanvragen die afkomstig van alle andere landen zijn zijn toegestaan. Landcodes en elkaar met een komma te scheiden. Als u toegang weigeren van de Verenigde Staten en Frankrijk wilt, voer ons FR in het vak.
    
       - **ec_ref_allow**: staat alleen aanvragen van de opgegeven verwijzende site. Een verwijzende identificeert de URL van de webpagina die is gekoppeld aan de aangevraagde resource. Het protocol niet opnemen in de parameterwaarde verwijzende site. De volgende typen van de invoer zijn toegestaan voor de waarde van parameter:
           - Een hostnaam of een hostnaam en een pad.
           - Meerdere verwijzingen. Als u wilt toevoegen in meerdere verwijzingen, elke verwijzende site met een komma te scheiden. Als u een waarde van verwijzende site opgeven, maar de verwijzende site-informatie niet in de aanvraag als gevolg van de browserconfiguratie van de wordt verzonden, wordt standaard de aanvraag geweigerd. 
           - Aanvragen met ontbrekende verwijzende site-informatie. Als u wilt toestaan dat dergelijke aanvragen, de tekst 'ontbreekt' of voer een lege waarde. 
           - Subdomeinen. Als u wilt toestaan subdomeinen, geeft u een sterretje (\*). Bijvoorbeeld, om toe te staan alle subdomeinen van `consoto.com`, voer `*.consoto.com`. 
           
          Het volgende voorbeeld ziet u de invoer voor toegang toestaan voor aanvragen van `www.consoto.com`, alle subdomeinen onder `consoto2.com`, en aanvragen met verwijzingen leeg is of ontbreekt.
        
          ![CDN ec_ref_allow voorbeeld](./media/cdn-token-auth/cdn-token-auth-referrer-allow2.png)
    
       - **ec_ref_deny**: Hiermee worden aanvragen van de opgegeven verwijzingsserver geweigerd. De implementatie is hetzelfde als de parameter ec_ref_allow.
         
       - **ec_proto_allow**: kunt alleen aanvragen van het opgegeven protocol. Bijvoorbeeld, HTTP of HTTPS.
            
       - **ec_proto_deny**: Hiermee worden aanvragen van het opgegeven protocol geweigerd. Bijvoorbeeld, HTTP of HTTPS.
    
       - **ec_clientip**: de toegang beperkt tot de opgegeven aanvrager IP-adres. Zowel IPV4 als IPV6 worden ondersteund. U kunt een aanvraag voor één IP-adres of een IP-subnet opgeven.
            
         ![CDN ec_clientip voorbeeld](./media/cdn-token-auth/cdn-token-auth-clientip.png)

    5. Nadat u klaar bent met het invoeren van parameterwaarden voor versleuteling, selecteert u een sleutel voor het versleutelen (als u een primaire en een back-sleutel hebt gemaakt) van de **sleutel te versleutelen** lijst.
    
    6. Selecteer de versie van een versleuteling van de **versleuteling versie** lijst: **V2** voor versie 2 of **V3** voor versie 3 (aanbevolen). Klik vervolgens op **versleutelen** om het token te genereren.

    Het token wordt gegenereerd, wordt weergegeven in de **Token gegenereerd** vak. Voor het gebruik van het token, voegt u deze als een queryreeks aan het einde van het bestand in de URL-pad. Bijvoorbeeld `http://www.domain.com/content.mov?a4fbc3710fd3449a7c99986b`.
        
    7. Test indien gewenst uw token met het hulpprogramma ontsleutelen. De waarde van het token in plakken de **Token gebruikt voor het ontsleutelen** vak. Selecteer het sleutelgebruik voor de versleuteling van de **sleutel te ontsleutelen** vervolgkeuzelijst lijst en klik vervolgens op **ontsleutelen**.

    Nadat het token wordt ontsleuteld, de parameters worden weergegeven in de **oorspronkelijke Parameters** vak.

    8. Eventueel aanpassen van het type antwoordcode die worden geretourneerd wanneer een aanvraag wordt geweigerd. Selecteer de code van de **antwoordcode** vervolgkeuzelijst en klik op **opslaan**. De **403** antwoordcode (verboden) is standaard geselecteerd. Voor bepaalde reactiecodes, u kunt ook de URL invoeren van de foutpagina weergegeven in de **headerwaarde** vak. 

3. Onder **HTTP grote**, klikt u op **regelengine**. De regelengine voor kunt u paden voor het toepassen van de functie, de functie tokenverificatie inschakelen en aanvullende verificatie-gerelateerde functies token inschakelen definiëren. Zie voor meer informatie [regels engine verwijzing](cdn-rules-engine-reference.md).

    1. Selecteer een bestaande regel of een nieuwe regel maken om te definiëren van de asset of het pad waarvan u wilt toepassen tokenverificatie. 
    2. Token als verificatie wilt inschakelen voor een regel, selecteer  **[Token Auth](cdn-rules-engine-reference-features.md#token-auth)**  van de **functies** vervolgkeuze-lijst, en selecteer vervolgens **ingeschakeld**. Klik op **Update** als u een regel wilt bijwerken of **toevoegen** als u een regel wilt maken.
        
    ![Voorbeeld van CDN regels engine tokenverificatie inschakelen](./media/cdn-token-auth/cdn-rules-engine-enable2.png)

4. U kunt ook aanvullende verificatie-gerelateerde functies token inschakelen in de regelengine. Als u een van de volgende functies selecteert in de **functies** vervolgkeuze-lijst, en selecteer vervolgens **ingeschakeld**.
    
    - **[Token DOS autorisatiecode](cdn-rules-engine-reference-features.md#token-auth-denial-code)**: bepaalt het type van de reactie die aan een gebruiker worden geretourneerd wanneer een aanvraag wordt geweigerd. Regels hier overschrijven de antwoordcode ingesteld in de **afhandeling van aangepaste DOS** sectie op de verificatiepagina op basis van tokens.
    - **[URL-aanvraag met negeren Auth token](cdn-rules-engine-reference-features.md#token-auth-ignore-url-case)**: Hiermee wordt bepaald of de URL die wordt gebruikt voor het valideren van het token hoofdlettergevoelig is.
    - **[Token Auth Parameter](cdn-rules-engine-reference-features.md#token-auth-parameter)**: wijzigt de naam van het token auth querytekenreeksparameter die wordt weergegeven in de aangevraagde URL. 
        
    ![CDN-regels in de engine tokenverificatie instellingen voorbeeld](./media/cdn-token-auth/cdn-rules-engine2.png)

5. U kunt uw token aanpassen door de toegang tot de broncode in [GitHub](https://github.com/VerizonDigital/ectoken).
Beschikbare talen zijn onder andere:
    
- C
- C#
- PHP
- Perl
- Java
- Python    

## <a name="azure-cdn-features-and-provider-pricing"></a>Azure CDN-functies en prijzen-provider

Zie voor meer informatie over functies [overzicht van CDN](cdn-overview.md). Zie voor meer informatie over prijzen [Content Delivery Network prijzen](https://azure.microsoft.com/pricing/details/cdn/).
