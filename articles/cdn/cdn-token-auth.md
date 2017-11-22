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
ms.date: 11/17/2017
ms.author: mezha
ms.openlocfilehash: f6d008a92677d28d0184e64637dcb2e093299519
ms.sourcegitcommit: 4ea06f52af0a8799561125497f2c2d28db7818e7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/21/2017
---
# <a name="securing-azure-content-delivery-network-assets-with-token-authentication"></a>Azure Content Delivery Network activa met tokenverificatie beveiligen

[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>Overzicht

Tokenverificatie is een mechanisme waarmee u om te voorkomen dat het Azure Content Delivery Network (CDN) voor de activa aan niet-geautoriseerde clients. Tokenverificatie gewoonlijk wordt uitgevoerd om te voorkomen dat 'hotlinking' van inhoud, waarop een andere website, zoals een mededelingenbord uw assets zonder toestemming gebruikt. Hotlinking kunnen een invloed hebben op de kosten voor het leveren van inhoud. Doordat tokenverificatie op CDN aanvragen door CDN edge-server geverifieerd voordat het CDN de inhoud biedt. 

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
    
Het volgende stroomdiagram wordt beschreven hoe Azure CDN valideert een clientaanvraag wanneer tokenverificatie is geconfigureerd op de CDN-eindpunt.

![De token validatielogica CDN](./media/cdn-token-auth/cdn-token-auth-validation-logic.png)

## <a name="setting-up-token-authentication"></a>Token verificatie instellen

1. Van de [Azure-portal](https://portal.azure.com), blader naar uw CDN-profiel en klik vervolgens op **beheren** starten van de aanvullende portal.

    ![Knop voor CDN-profiel beheren](./media/cdn-token-auth/cdn-manage-btn.png)

2. Beweeg de muisaanwijzer over **HTTP grote**, klikt u vervolgens op **Token Auth** in het doel. U kunt vervolgens instellen van de versleutelingssleutel en versleuteling parameters als volgt:

    1. Een of meer versleutelingssleutels maken. Een coderingssleutel is hoofdlettergevoelig en kan een combinatie van alfanumerieke tekens bevatten. Andere soorten tekens, inclusief spaties, zijn niet toegestaan. De maximale lengte is 250 tekens. Om ervoor te zorgen dat uw coderingssleutels willekeurige zijn, kunt u deze maken met behulp van de [OpenSSL hulpprogramma](https://www.openssl.org/). 

       Het hulpprogramma OpenSSL heeft de volgende syntaxis:

       ```rand -hex <key length>```

       Bijvoorbeeld:

       ```OpenSSL> rand -hex 32``` 

       Om te voorkomen uitvaltijd, maakt u een primaire en een back-sleutel. Een back-sleutel biedt ononderbroken toegang tot uw inhoud wanneer de primaire sleutel wordt bijgewerkt.
    
    2. Voer een unieke coderingssleutel in de **primaire sleutel** in en voert u desgewenst een back-sleutel in de **back-sleutel** vak.

    3. Selecteer de versie van de minimale codering voor elke sleutel van de **minimumversie versleuteling** lijst en klik vervolgens op **Update**:
       - **V2**: geeft aan dat de sleutel voor het genereren versie 2.0 en 3.0 tokens kan worden gebruikt. Gebruik deze optie alleen als u zijn overgang van een oudere versie 2.0-versleutelingssleutel met een sleutel versie 3.0.
       - **V3**: (aanbevolen) geeft aan dat de sleutel kan alleen worden gebruikt voor het genereren versie 3.0-tokens.

    ![CDN-setup van token verificatiesleutel](./media/cdn-token-auth/cdn-token-auth-setupkey.png)
    
    4. Gebruik het hulpprogramma coderen voor versleuteling parameters instellen en het genereren van een token. U kunt met het hulpprogramma versleutelen toestaan of weigeren van aanvragen op basis van de verlooptijd, land, verwijzende site, het protocol en client-IP (in een willekeurige combinatie). Hoewel er geen limiet voor het aantal en de combinatie van parameters die kunnen worden gecombineerd om een token, is de totale lengte van een token beperkt tot 512 tekens. 

       ![CDN hulpprogramma versleutelen](./media/cdn-token-auth/cdn-token-auth-encrypttool.png)

       Voer waarden in voor een of meer van de volgende parameters versleuteling in de **versleutelen hulpprogramma** sectie: 

       > [!div class="mx-tdCol2BreakAll"] 
       > <table>
       > <tr>
       >   <th>Parameternaam</th> 
       >   <th>Beschrijving</th>
       > </tr>
       > <tr>
       >    <td><b>ec_expire</b></td>
       >    <td>Een verlooptijd wijst naar een token, waarna het token verloopt. Aanvragen verzonden nadat de vervaltijd worden geweigerd. Deze parameter wordt gebruikt voor een Unix-tijdstempel die is gebaseerd op het aantal seconden sinds de standaard Unix-epoche van `1/1/1970 00:00:00 GMT`. (U kunt online hulpprogramma's gebruiken om te converteren tussen stnd. tijd- en Unix-tijd.)> 
       >    Bijvoorbeeld, als u wilt dat het token verloopt op `12/31/2016 12:00:00 GMT`, voer de waarde van het tijdstempel Unix `1483185600`. 
       > </tr>
       > <tr>
       >    <td><b>ec_url_allow</b></td> 
       >    <td>U kunt aangeven van tokens, voor een bepaalde asset of het pad. Dit beperkt de toegang tot aanvragen waarvan de URL beginnen met een specifieke relatief pad. URL's zijn hoofdlettergevoelig. Invoer van meerdere paden door elk pad scheiden met komma's; Voeg geen spaties. Afhankelijk van uw vereisten kunt u verschillende waarden instellen voor verschillende niveau van toegang.> 
       >    Bijvoorbeeld: voor de URL `http://www.mydomain.com/pictures/city/strasbourg.png`, deze aanvragen zijn toegestaan voor de volgende invoerwaarden: 
       >    <ul>
       >       <li>Invoerwaarde `/`: alle aanvragen zijn toegestaan.</li>
       >       <li>Invoerwaarde `/pictures`, mogen de volgende aanvragen: <ul>
       >          <li>`http://www.mydomain.com/pictures.png`</li>
       >          <li>`http://www.mydomain.com/pictures/city/strasbourg.png`</li>
       >          <li>`http://www.mydomain.com/picturesnew/city/strasbourgh.png`</li>
       >       </ul></li>
       >       <li>Invoerwaarde `/pictures/`: alleen aanvragen die de `/pictures/` pad zijn toegestaan. Bijvoorbeeld `http://www.mydomain.com/pictures/city/strasbourg.png`.</li>
       >       <li>Invoerwaarde `/pictures/city/strasbourg.png`: alleen aanvragen voor deze specifieke pad en de asset zijn toegestaan.</li>
       >    </ul>
       > </tr>
       > <tr>
       >    <td><b>ec_country_allow</b></td> 
       >    <td>U kunt alleen aanvragen die afkomstig uit een of meer opgegeven landen zijn. Aanvragen die afkomstig van alle andere landen zijn worden geweigerd. Gebruik van twee letters [landcode ISO 3166](https://msdn.microsoft.com/library/mt761717.aspx) voor elk land en elkaar met een komma te scheiden; Voeg een spatie. Bijvoorbeeld, als u toegang toestaan via alleen in de Verenigde Staten en Frankrijk wilt, voert `US,FR`.</td>
       > </tr>
       > <tr>
       >    <td><b>ec_country_deny</b></td> 
       >    <td>Verzoeken die afkomstig uit een of meer opgegeven landen zijn geweigerd. Aanvragen die afkomstig van alle andere landen zijn zijn toegestaan. De uitvoering is hetzelfde als de <b>ec_country_allow</b> parameter. Als een landcode die aanwezig zijn in zowel de <b>ec_country_allow</b> en <b>ec_country_deny</b> parameters, de <b>ec_country_allow</b> parameter voorrang.</td>
       > </tr>
       > <tr>
       >    <td><b>ec_ref_allow</b></td>
       >    <td>Staat alleen aanvragen van de opgegeven verwijzende site. Een verwijzende identificeert de URL van de webpagina die is gekoppeld aan de aangevraagde resource. Het protocol niet opnemen in de parameterwaarde.>    
       >    De volgende typen van de invoer zijn toegestaan:
       >    <ul>
       >       <li>Een hostnaam of een hostnaam en een pad.</li>
       >       <li>Meerdere verwijzingen. Als u wilt meerdere verwijzingen toevoegen, scheidt u elke verwijzende site met een komma; Voeg geen een spatie. Als u een waarde van verwijzende site opgeven, maar de verwijzende site-informatie niet in de aanvraag als gevolg van de browserconfiguratie van de wordt verzonden, wordt standaard de aanvraag geweigerd.</li> 
       >       <li>Aanvragen met verwijzende site-informatie ontbreekt of is leeg. Standaard de <b>ec_ref_allow</b> parameter dergelijke aanvragen worden geblokkeerd. Als deze aanvragen zijn toegestaan, ofwel de tekst 'ontbreekt', of voer een lege waarde (via een afsluitende komma).</li> 
       >       <li>Subdomeinen. Als u wilt toestaan subdomeinen, geeft u een sterretje (\*). Bijvoorbeeld, om toe te staan alle subdomeinen van `contoso.com`, voer `*.contoso.com`.</li>
       >    </ul>     
       >    Bijvoorbeeld, om toegang toestaan voor aanvragen van `www.contoso.com`, alle subdomeinen onder `contoso2.com`, en voert u aanvragen met verwijzingen leeg is of ontbreekt, `www.contoso.com,*.contoso.com,missing`.</td>
       > </tr>
       > <tr> 
       >    <td><b>ec_ref_deny</b></td>
       >    <td>Hiermee worden aanvragen van de opgegeven verwijzingsserver geweigerd. De uitvoering is hetzelfde als de <b>ec_ref_allow</b> parameter. Als een verwijzende site aanwezig is in zowel de <b>ec_ref_allow</b> en <b>ec_ref_deny</b> parameters, de <b>ec_ref_allow</b> parameter voorrang.</td>
       > </tr>
       > <tr> 
       >    <td><b>ec_proto_allow</b></td> 
       >    <td>U kunt alleen aanvragen van het opgegeven protocol. Geldige waarden zijn `http`, `https`, of `http,https`.</td>
       > </tr>
       > <tr>
       >    <td><b>ec_proto_deny</b></td>
       >    <td>Hiermee worden aanvragen van het opgegeven protocol geweigerd. De uitvoering is hetzelfde als de <b>ec_proto_allow</b> parameter. Als een protocol aanwezig in zowel is de <b>ec_proto_allow</b> en <b>ec_proto_deny</b> parameters, de <b>ec_proto_allow</b> parameter voorrang.</td>
       > </tr>
       > <tr>
       >    <td><b>ec_clientip</b></td>
       >    <td>Beperkt de toegang tot de opgegeven aanvrager IP-adres. Zowel IPV4 als IPV6 worden ondersteund. U kunt een aanvraag voor één IP-adres of IP-adressen die zijn gekoppeld aan een specifiek subnet opgeven. Bijvoorbeeld: `11.22.33.0/22` kunnen aanvragen van IP-adressen 11.22.32.1-11.22.35.254.</td>
       > </tr>
       > </table>

    5. Nadat u klaar bent met het invoeren van parameterwaarden voor versleuteling, selecteert u een sleutel voor het versleutelen (als u een primaire en een back-sleutel hebt gemaakt) van de **sleutel te versleutelen** lijst.
    
    6. Selecteer de versie van een versleuteling van de **versleuteling versie** lijst: **V2** voor versie 2 of **V3** voor versie 3 (aanbevolen). 

    7. Klik op **versleutelen** om het token te genereren.

    Het token wordt gegenereerd, wordt weergegeven in de **Token gegenereerd** vak. Voor het gebruik van het token, voegt u deze als een queryreeks aan het einde van het bestand in de URL-pad. Bijvoorbeeld `http://www.domain.com/content.mov?a4fbc3710fd3449a7c99986b`.
        
    8. Test indien gewenst uw token met het hulpprogramma decoderen zodat u uw token parameters kunt weergeven. De waarde van het token in plakken de **Token gebruikt voor het ontsleutelen** vak. Selecteer de versleutelingssleutel van te gebruiken de **sleutel te ontsleutelen** lijst en klik vervolgens op **ontsleutelen**.

    Nadat het token wordt ontsleuteld, de parameters worden weergegeven in de **oorspronkelijke Parameters** vak.

    9. Eventueel aanpassen van het type antwoordcode die worden geretourneerd wanneer een aanvraag wordt geweigerd. Selecteer **ingeschakeld**, selecteer vervolgens de antwoordcode van de **antwoordcode** lijst. **Kopnaam** automatisch ingesteld op **locatie**. Klik op **opslaan** voor het implementeren van de nieuwe antwoordcode. Voor bepaalde reactiecodes, u moet ook de URL invoeren van de foutpagina weergegeven in de **headerwaarde** vak. De **403** antwoordcode (verboden) is standaard geselecteerd. 

3. Onder **HTTP grote**, klikt u op **regelengine**. De regelengine voor kunt u paden voor het toepassen van de functie, de functie tokenverificatie inschakelen en aanvullende verificatie-gerelateerde functies token inschakelen definiëren. Zie voor meer informatie [regels engine verwijzing](cdn-rules-engine-reference.md).

    1. Selecteer een bestaande regel of een nieuwe regel maken om te definiëren van de asset of het pad waarvan u wilt toepassen tokenverificatie. 
    2. Token als verificatie wilt inschakelen voor een regel, selecteer  **[Token Auth](cdn-rules-engine-reference-features.md#token-auth)**  van de **functies** lijst, en selecteer vervolgens **ingeschakeld**. Klik op **Update** als u een regel wilt bijwerken of **toevoegen** als u een regel wilt maken.
        
    ![Voorbeeld van CDN regels engine tokenverificatie inschakelen](./media/cdn-token-auth/cdn-rules-engine-enable2.png)

4. U kunt ook aanvullende verificatie-gerelateerde functies token inschakelen in de regelengine. Als u een van de volgende functies selecteert in de **functies** lijst, en selecteer vervolgens **ingeschakeld**.
    
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
