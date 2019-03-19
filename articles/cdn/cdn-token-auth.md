---
title: Beveiligen van Azure CDN-assets met tokenverificatie | Microsoft Docs
description: Informatie over het gebruik van tokenverificatie voor beveiligde toegang tot uw Azure CDN-assets.
services: cdn
documentationcenter: .net
author: zhangmanling
manager: zhangmanling
editor: ''
ms.assetid: 837018e3-03e6-4f9c-a23e-4b63d5707a64
ms.service: cdn
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 11/17/2017
ms.author: mezha
ms.openlocfilehash: 6ff7b018cea8bedd93d3c9aef3b3250ecf5251b4
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58123722"
---
# <a name="securing-azure-cdn-assets-with-token-authentication"></a>Beveiligen van Azure CDN-assets met tokenverificatie

[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>Overzicht

Tokenverificatie is een mechanisme waarmee u om te voorkomen dat het Azure Content Delivery Network (CDN) die activa voor niet-geautoriseerde clients. Tokenverificatie gewoonlijk wordt uitgevoerd om te voorkomen dat *hotlinking* van inhoud, waarin een andere website, zoals een prikbord maakt gebruik van uw activa zonder toestemming. Hotlinking kunnen een invloed hebben op de kosten voor de levering van inhoud. Tokenverificatie op CDN inschakelt, worden aanvragen door CDN edge-server geverifieerd, voordat het CDN de inhoud te leveren. 

## <a name="how-it-works"></a>Hoe werkt het?

Tokenverificatie controleert u of aanvragen worden gegenereerd door een vertrouwde site doordat aanvragen naar de waarde voor een token dat bevat informatie over de aanvrager kan gecodeerde bevatten. Content wordt geleverd voor een aanvrager alleen als de gecodeerde gegevens voldoet aan de vereisten; anders worden aanvragen geweigerd. U kunt de vereisten instellen met behulp van een of meer van de volgende parameters:

- Land/regio: Toestaan of weigeren van aanvragen die afkomstig van de landen die is opgegeven zijn door hun [landcode](https://msdn.microsoft.com/library/mt761717.aspx).
- URL: Alleen aanvragen die overeenkomen met de opgegeven asset of het pad toestaan.
- Host: Toestaan of weigeren van aanvragen die gebruikmaken van de opgegeven hosts in de aanvraagheader.
- Verwijzende site: Toestaan of weigeren van aanvraag van de opgegeven verwijzende site.
- IP-adres: Alleen aanvragen die afkomstig van specifieke IP-adres of IP-subnet zijn toestaan.
- Protocol: Toestaan of weigeren op basis van het protocol dat wordt gebruikt om aan te vragen van de inhoud.
- Vervaltijd: Toewijzen van een bepaalde datum en tijd om ervoor te zorgen dat een koppeling geldig alleen voor een beperkte periode blijft.

Zie voor meer informatie de gedetailleerde configuratie-voorbeelden voor elke parameter in [instellen van tokenverificatie](#setting-up-token-authentication).

>[!IMPORTANT] 
> Als het token autorisatie voor elk pad voor dit account is ingeschakeld, is standard-cachemodus de enige modus die kan worden gebruikt voor het opslaan van de query-tekenreeks. Zie [Cachegedrag in Azure CDN bepalen met queryreeksen](cdn-query-string-premium.md) voor meer informatie.

## <a name="reference-architecture"></a>Referentiearchitectuur

Het volgende werkstroomdiagram wordt beschreven hoe het CDN-token verificatie gebruikt om u te werken met uw web-app.

![Werkstroom voor CDN-tokenverificatie](./media/cdn-token-auth/cdn-token-auth-workflow2.png)

## <a name="token-validation-logic-on-cdn-endpoint"></a>Token validatielogica op CDN-eindpunt
    
Het volgende stroomdiagram wordt beschreven hoe een clientaanvraag door Azure CDN wordt gevalideerd als tokenverificatie voor CDN-eindpunt is geconfigureerd.

![Token validatielogica van CDN](./media/cdn-token-auth/cdn-token-auth-validation-logic.png)

## <a name="setting-up-token-authentication"></a>Tokenverificatie instellen

1. Uit de [Azure-portal](https://portal.azure.com), blader naar uw CDN-profiel, en selecteer vervolgens **beheren** om te starten van de aanvullende portal.

    ![Knop voor CDN-profiel beheren](./media/cdn-token-auth/cdn-manage-btn.png)

2. Beweeg de muisaanwijzer over **HTTP grote**en selecteer vervolgens **Token Authentication** in de flyout. U kunt vervolgens de versleutelingssleutel en versleuteling parameters als volgt instellen:

   1. Maak een of meer versleutelingssleutels. Een coderingssleutel is hoofdlettergevoelig en kan elke combinatie van alfanumerieke tekens bevatten. Alle andere soorten tekens, inclusief spaties, zijn niet toegestaan. De maximale lengte is 250 tekens. Om ervoor te zorgen dat uw versleutelingssleutels worden willekeurige, het wordt aanbevolen dat u ze hebt maken met behulp van de [OpenSSL hulpprogramma](https://www.openssl.org/). 

      Het hulpprogramma OpenSSL heeft de volgende syntaxis:

      ```rand -hex <key length>```

      Bijvoorbeeld:

      ```OpenSSL> rand -hex 32``` 

      Om te voorkomen uitvaltijd, zowel de primaire en een back-upsleutel te maken. Een back-upsleutel biedt ononderbroken toegang tot uw inhoud wanneer de primaire sleutel wordt bijgewerkt.
    
   2. Voer een unieke versleutelingssleutel in de **primaire sleutel** in en voert u een back-upsleutel in (optioneel) de **-back-Upsleutel** vak.

   3. Selecteer de versie van de minimale codering voor elke sleutel van de **minimumversie versleuteling** lijst en selecteer vervolgens **Update**:
      - **V2**: Geeft aan dat de sleutel kan worden gebruikt voor het genereren van versie 2.0 en 3.0-tokens. Gebruik deze optie alleen als u van de versleutelingssleutel van een oudere versie 2.0 worden overgezet naar een versie 3.0-sleutel.
      - **V3**: (Aanbevolen) Geeft aan dat de sleutel kan alleen worden gebruikt voor het genereren van tokens voor versie 3.0.

      ![CDN-setup van token verificatiesleutel](./media/cdn-token-auth/cdn-token-auth-setupkey.png)
    
   4. Gebruik het hulpprogramma coderen voor versleuteling parameters instellen en het genereren van een token. Met het hulpprogramma versleutelen, kunt u toestaan of weigeren op basis van de verlooptijd van, land, verwijzende site, protocol en client-IP-adres (in een combinatie). Hoewel er geen limiet voor het aantal en de combinatie van parameters die kunnen worden gecombineerd voor het token van een formulier, is de totale lengte van een token beperkt tot 512 tekens. 

      ![CDN hulpprogramma versleutelen](./media/cdn-token-auth/cdn-token-auth-encrypttool.png)

      Voer waarden in voor een of meer van de volgende parameters versleuteling in de **versleutelen hulpprogramma** sectie: 

      > [!div class="mx-tdCol2BreakAll"] 
      > <table>
      > <tr>
      >   <th>Parameternaam</th> 
      >   <th>Description</th>
      > </tr>
      > <tr>
      >    <td><b>ec_expire</b></td>
      >    <td>Een verlooptijd toegewezen aan een token, waarna het token is verlopen. Aanvragen verzonden nadat de verlooptijd van de zijn geweigerd. Deze parameter wordt gebruikt voor een Unix-timestamp, die is gebaseerd op het aantal seconden sinds de standaard Unix-epoche van `1/1/1970 00:00:00 GMT`. (U kunt online hulpprogramma's gebruiken om te schakelen tussen (standaardtijd) en de Unix-tijd.)> 
      >    Bijvoorbeeld, als u wilt dat het token verloopt op `12/31/2016 12:00:00 GMT`, voer de waarde van de Unix-timestamp, `1483185600`. 
      > </tr>
      > <tr>
      >    <td><b>ec_url_allow</b></td> 
      >    <td>Kunt u aan te passen van tokens, voor een bepaalde asset of een pad. Dit beperkt de toegang tot aanvragen waarvan de URL begint met een specifieke relatief pad. URL's zijn hoofdlettergevoelig. Meerdere paden invoeren door elk pad scheiden met een door komma's; Voeg geen spaties. Afhankelijk van uw vereisten, kunt u verschillende waarden instellen voor een ander niveau van toegang.> 
      >    Bijvoorbeeld, voor de URL `http://www.mydomain.com/pictures/city/strasbourg.png`, deze aanvragen zijn toegestaan voor de volgende invoerwaarden: 
      >    <ul>
      >       <li>Invoerwaarde `/`: Alle aanvragen zijn toegestaan.</li>
      >       <li>Invoerwaarde `/pictures`, de volgende aanvragen zijn toegestaan: <ul>
      >          <li>`http://www.mydomain.com/pictures.png`</li>
      >          <li>`http://www.mydomain.com/pictures/city/strasbourg.png`</li>
      >          <li>`http://www.mydomain.com/picturesnew/city/strasbourgh.png`</li>
      >       </ul></li>
      >       <li>Invoerwaarde `/pictures/`: Alleen aanvragen die de `/pictures/` pad zijn toegestaan. Bijvoorbeeld `http://www.mydomain.com/pictures/city/strasbourg.png`.</li>
      >       <li>Invoerwaarde `/pictures/city/strasbourg.png`: Alleen aanvragen voor deze specifieke pad en de activa zijn toegestaan.</li>
      >    </ul>
      > </tr>
      > <tr>
      >    <td><b>ec_country_allow</b></td> 
      >    <td>U kunt alleen aanvragen die afkomstig uit een of meer opgegeven landen zijn. Aanvragen die afkomstig uit alle overige landen zijn worden geweigerd. Gebruik van twee letters [3166 ISO-landcode](https://msdn.microsoft.com/library/mt761717.aspx) voor elk land en gescheiden van elkaar met een door komma's, Voeg een spatie. Bijvoorbeeld, als u toegang toestaan van alleen de Verenigde Staten en Frankrijk wilt, voert u `US,FR`.</td>
      > </tr>
      > <tr>
      >    <td><b>ec_country_deny</b></td> 
      >    <td>Hiermee worden aanvragen die afkomstig uit een of meer opgegeven landen zijn geweigerd. Aanvragen die afkomstig uit alle overige landen zijn zijn toegestaan. De implementatie is hetzelfde als de <b>ec_country_allow</b> parameter. Als een landcode aanwezig in zowel is de <b>ec_country_allow</b> en <b>ec_country_deny</b> parameters, de <b>ec_country_allow</b> parameter voorrang.</td>
      > </tr>
      > <tr>
      >    <td><b>ec_ref_allow</b></td>
      >    <td>Staat alleen aanvragen van de opgegeven verwijzende site. Een verwijzende site identificeert de URL van de webpagina die is gekoppeld aan de resource wordt aangevraagd. Neem het protocol niet in de waarde van parameter.> 
      >    De volgende soorten invoer zijn toegestaan:
      >    <ul>
      >       <li>Een hostnaam of een hostnaam en een pad.</li>
      >       <li>Meerdere verwijzende sites. Als u wilt meerdere verwijzende sites toevoegen, scheidt u elke verwijzende site met een komma; Voeg een spatie. Als u een waarde van de verwijzende site opgeven, maar de verwijzende site-informatie niet in de aanvraag vanwege de configuratie van de browser wordt verzonden, wordt standaard de aanvraag geweigerd.</li> 
      >       <li>Aanvragen met verwijzende site-informatie ontbreekt of is leeg. Standaard de <b>ec_ref_allow</b> parameter dergelijke aanvragen worden geblokkeerd. Als u wilt toestaan dat deze aanvragen, voert u een van beide de tekst 'ontbreekt', of een lege waarde invoeren (met behulp van een afsluitende komma).</li> 
      >       <li>Subdomeinen. Als u wilt toestaan dat subdomeinen, geeft u een sterretje (\*). Bijvoorbeeld, om toe te staan alle subdomeinen van `contoso.com`, voer `*.contoso.com`.</li>
      >    </ul> 
      >    Bijvoorbeeld, om toe te staan van toegang voor aanvragen van `www.contoso.com`, alle subdomeinen onder `contoso2.com`, en voert u aanvragen met verwijzende sites leeg is of ontbreekt, `www.contoso.com,*.contoso.com,missing`.</td>
      > </tr>
      > <tr> 
      >    <td><b>ec_ref_deny</b></td>
      >    <td>Hiermee worden aanvragen van de opgegeven verwijzende site geweigerd. De implementatie is hetzelfde als de <b>ec_ref_allow</b> parameter. Als een verwijzende site aanwezig is in zowel de <b>ec_ref_allow</b> en <b>ec_ref_deny</b> parameters, de <b>ec_ref_allow</b> parameter voorrang.</td>
      > </tr>
      > <tr> 
      >    <td><b>ec_proto_allow</b></td> 
      >    <td>U kunt alleen aanvragen van het opgegeven protocol. Geldige waarden zijn `http`, `https`, of `http,https`.</td>
      > </tr>
      > <tr>
      >    <td><b>ec_proto_deny</b></td>
      >    <td>Hiermee worden aanvragen van het opgegeven protocol geweigerd. De implementatie is hetzelfde als de <b>ec_proto_allow</b> parameter. Als een protocol aanwezig in zowel is de <b>ec_proto_allow</b> en <b>ec_proto_deny</b> parameters, de <b>ec_proto_allow</b> parameter voorrang.</td>
      > </tr>
      > <tr>
      >    <td><b>ec_clientip</b></td>
      >    <td>Toegang beperkt tot de IP-adres van de opgegeven aanvrager. Zowel IPV4 als IPV6 worden ondersteund. U kunt een aanvraag voor één IP-adres of IP-adressen die zijn gekoppeld aan een specifiek subnet opgeven. Bijvoorbeeld, `11.22.33.0/22` kunnen aanvragen van IP-adressen 11.22.32.1-11.22.35.254.</td>
      > </tr>
      > </table>

   5. Nadat u klaar bent met het invoeren van parameterwaarden voor versleuteling, selecteert u een sleutel voor het versleutelen van (als u zowel de primaire en een back-upsleutel hebt gemaakt) van de **sleutel te versleutelen** lijst.
    
   6. Selecteer een versleutelingsversie van de **Versleutelingsversie** lijst: **V2** voor versie 2 of **V3** voor versie 3 (aanbevolen). 

   7. Selecteer **versleutelen** om het token te genereren.

      Nadat het token is gegenereerd, wordt deze weergegeven in de **gegenereerd Token** vak. Voor het gebruik van het token, voegt u deze als een queryreeks aan het einde van het bestand in uw URL-pad. Bijvoorbeeld `http://www.domain.com/content.mov?a4fbc3710fd3449a7c99986b`.
        
   8. Test indien gewenst uw token met het hulpprogramma decoderen zodat u de parameters van uw token kunt bekijken. Plak de waarde voor de token in de **Token voor het ontsleutelen van** vak. Selecteer de versleutelingssleutel te gebruiken van de **sleutel te ontsleutelen** lijst en selecteer vervolgens **ontsleutelen**.

      Nadat het token wordt ontsleuteld, de volgende parameters worden weergegeven in de **oorspronkelijke Parameters** vak.

   9. (Optioneel) pas het type antwoordcode dat wordt geretourneerd wanneer een aanvraag wordt geweigerd. Selecteer **ingeschakeld**, selecteer vervolgens de responscode van de **antwoordcode** lijst. **De headernaam** wordt automatisch ingesteld op **locatie**. Selecteer **opslaan** voor het implementeren van de nieuwe antwoordcode. Voor bepaalde responscodes, u moet ook de URL opgeven van de foutpagina weergegeven in de **headerwaarde** vak. De **403** antwoordcode (verboden) is standaard geselecteerd. 

3. Onder **HTTP grote**, selecteer **regels-Engine**. U de regelengine gebruiken voor het definiëren van paden voor het toepassen van de functie, de functie tokenverificatie inschakelen en extra token verificatie-gerelateerde functies inschakelen. Zie voor meer informatie, [verwijzing regelengine](cdn-rules-engine-reference.md).

   1. Selecteer een bestaande regel of een nieuwe regel voor het definiëren van de asset of het pad voor die u wilt toepassen van tokenverificatie maken. 
   2. Als tokenverificatie voor een regel, schakelt **[Token Authentication](cdn-rules-engine-reference-features.md#token-auth)** uit de **functies** lijst en selecteer vervolgens **ingeschakeld**. Selecteer **Update** als u een regel wilt bijwerken of **toevoegen** als u een regel wilt maken.
        
      ![Voorbeeld van CDN regels-engine tokenverificatie inschakelen](./media/cdn-token-auth/cdn-rules-engine-enable2.png)

4. U kunt ook aanvullende verificatie-gerelateerde functies token inschakelen in de regelengine. Als u wilt inschakelen op een van de volgende functies, selecteert u deze in de **functies** lijst en selecteer vervolgens **ingeschakeld**.
    
   - **[Autorisatiecode denial-of token](cdn-rules-engine-reference-features.md#token-auth-denial-code)**: Bepaalt het type van de reactie die aan een gebruiker wordt geretourneerd wanneer een aanvraag wordt geweigerd. Hier regels overschrijven de responscode instellen in de **aangepaste denial-of verwerking** sectie op de verificatiepagina op basis van tokens.

   - **[Auth-token is hoofdlettergevoeligheid niet URL](cdn-rules-engine-reference-features.md#token-auth-ignore-url-case)**: Hiermee bepaalt u of de URL die wordt gebruikt voor het valideren van het token hoofdlettergevoelig is.

   - **[De Auth Parameter-token](cdn-rules-engine-reference-features.md#token-auth-parameter)**: Naam van het token Authentication queryreeks-parameter die wordt weergegeven in de aangevraagde URL. 
        
     ![Voorbeeld van instellingen voor tokenverificatie CDN de regelengine](./media/cdn-token-auth/cdn-rules-engine2.png)

5. U kunt uw token aanpassen door de toegang tot de broncode in [GitHub](https://github.com/VerizonDigital/ectoken).
   Beschikbare talen zijn onder andere:
    
   - C
   - C#
   - PHP
   - Perl
   - Java
   - Python 

## <a name="azure-cdn-features-and-provider-pricing"></a>Azure CDN-functies en prijzen-provider

Zie voor meer informatie over functies [Azure CDN-producteigenschappen](cdn-features.md). Zie voor meer informatie over de prijzen [prijzen van Content Delivery Network](https://azure.microsoft.com/pricing/details/cdn/).
