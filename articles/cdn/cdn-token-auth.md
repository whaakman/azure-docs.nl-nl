---
title: Beveiligen van Azure CDN activa met tokenverificatie | Microsoft Docs
description: Gebruik tokenverificatie ter beveiliging van toegang tot uw assets Azure CDN.
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
ms.date: 11/11/2016
ms.author: mezha
ms.openlocfilehash: 88956b324a543c5347e16b1278f6b2179a3b9c24
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/03/2017
---
# <a name="securing-azure-content-delivery-network-assets-with-token-authentication"></a>Azure Content Delivery Network activa met tokenverificatie beveiligen

[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>Overzicht

Tokenverificatie is een mechanisme waarmee u om te voorkomen dat het Azure Content Delivery Network (CDN) voor de activa aan niet-geautoriseerde clients. Tokenverificatie gewoonlijk wordt uitgevoerd om te voorkomen dat 'hotlinking' van inhoud, waarop een andere website, vaak een mededelingenbord uw assets zonder toestemming gebruikt. Hotlinking kunnen een invloed hebben op de kosten voor het leveren van inhoud. Als u deze functie op CDN inschakelt, worden aanvragen geverifieerd door rand CDN POP's voordat het CDN de inhoud biedt. 

## <a name="how-it-works"></a>Hoe werkt het?

Tokenverificatie controleert u of aanvragen worden gegenereerd door een vertrouwde site doordat aanvragen voor een token waarde bevatten die gecodeerd bevat informatie over de aanvrager. Inhoud is geleverd voor een aanvrager alleen als de gecodeerde gegevens voldoet aan de vereisten; anders worden aanvragen geweigerd. U kunt de vereisten instellen met behulp van een of meer van de volgende parameters:

- Land: toestaan of weigeren van aanvragen die afkomstig van bepaalde landen zijn.  [Lijst met geldige landcodes.](https://msdn.microsoft.com/library/mt761717.aspx) 
- URL: dat alleen bepaalde asset of het pad om aan te vragen.  
- Host: toestaan of weigeren-aanvragen via de opgegeven hosts in de aanvraagheader.
- Verwijzende site: toestaan of weigeren opgegeven verwijzende site om aan te vragen.
- IP-adres: ervoor zorgen dat alleen aanvragen die afkomstig van specifieke IP-adres of IP-subnet zijn.
- Protocol: toestaan of blokkeren aanvragen op basis van het protocol dat wordt gebruikt om aan te vragen van de inhoud.
- Verlooptijd: een bepaalde datum en tijd om ervoor te zorgen dat een koppeling alleen geldig voor een beperkte periode blijven toewijzen.

Zie voor meer informatie de gedetailleerde configuratie-voorbeelden voor elke parameter in [tokenverificatie instellen](#setting-up-token-authentication).

Nadat u een gecodeerde token genereren, kunt u deze toevoegt als een queryreeks aan het einde van het bestand URL-pad. Bijvoorbeeld `http://www.domain.com/content.mov?a4fbc3710fd3449a7c99986b`.

## <a name="reference-architecture"></a>Referentiearchitectuur

Het volgende werkstroomdiagram wordt beschreven hoe de CDN tokenverificatie gebruikt om te werken met uw webtoepassing.

![CDN-tokenverificatie werkstroom](./media/cdn-token-auth/cdn-token-auth-workflow2.png)

## <a name="token-validation-logic-on-cdn-endpoint"></a>Token validatielogica op CDN-eindpunt
    
Het volgende stroomdiagram wordt beschreven hoe Azure CDN clientaanvraag valideert wanneer tokenverificatie is geconfigureerd op de CDN-eindpunt.

![De token validatielogica CDN](./media/cdn-token-auth/cdn-token-auth-validation-logic.png)

## <a name="setting-up-token-authentication"></a>Token verificatie instellen

1. Van de [Azure-portal](https://portal.azure.com), blader naar uw CDN-profiel en klik vervolgens op **beheren** starten van de aanvullende portal.

    ![Knop voor CDN-profiel beheren](./media/cdn-rules-engine/cdn-manage-btn.png)

2. Beweeg de muisaanwijzer over **HTTP grote**, en klik vervolgens op **Token Auth** in het doel. U instellen de versleutelingssleutel en de parameters van de versleuteling in op dit tabblad.

    1. Voer een unieke coderingssleutel voor **primaire sleutel**.  Voer een andere voor **back-up-sleutel**

        ![CDN-setup van token verificatiesleutel](./media/cdn-token-auth/cdn-token-auth-setupkey.png)
    
    2. Versleuteling parameters met versleuteling hulpprogramma instellen (toestaan of weigeren van aanvragen op basis van de verlooptijd, land, verwijzende, protocol, client-IP. U kunt elke combinatie.)

        ![CDN hulpprogramma versleutelen](./media/cdn-token-auth/cdn-token-auth-encrypttool.png)

        - EC-verloopt: wijst een Vervaltijd van een token na een opgegeven periode. Aanvragen verzonden nadat de vervaltijd worden geweigerd. Deze parameter gebruikt Unix tijdstempel (op basis van seconden sinds standaard epoche van 1/1/1970 00:00:00 GMT. U kunt online hulpprogramma's gebruiken voor conversie tussen stnd. tijd- en Unix-tijd.) Bijvoorbeeld, als u wilt dat voor het instellen van het token verloopt op `12/31/2016 12:00:00 GMT`, gebruikt u de waarde van het tijdstempel Unix `1483185600`als volgt.
    
        ![CDN ec_expire voorbeeld](./media/cdn-token-auth/cdn-token-auth-expire2.png)
    
        - EC url toestaan: u kunt aangeven van tokens, voor een bepaalde asset of het pad. Dit beperkt de toegang tot aanvragen waarvan de URL beginnen met een specifieke relatief pad. U kunt meerdere paden elk pad scheiden met komma's invoeren. URL's zijn hoofdlettergevoelig. Afhankelijk van het vereiste kunt u andere waarde instellen voor verschillende niveau van toegang. Hieronder vindt u een aantal scenario's:
        
            Als u een URL: http://www.mydomain.com/pictures/city/strasbourg.png. Zie invoerwaarde "" en de toegang dienovereenkomstig serviceniveau

            1. Invoerwaarde '/': alle aanvragen kunnen worden
            2. Invoerwaarde '/ afbeeldingen': alle volgende aanvragen worden toestaan
            
                - http://www.mydomain.com/Pictures.PNG
                - http://www.mydomain.com/Pictures/City/Strasbourg.PNG
                - http://www.mydomain.com/picturesnew/City/strasbourgh.PNG
            3. Waarde '/ afbeeldingen /' worden ingevoerd: alleen aanvragen voor /pictures/ wordt toegestaan
            4. Invoerwaarde ' / pictures/city/strasbourg.png ': kunt u alleen aanvragen voor dit activum
    
        ![CDN ec_url_allow voorbeeld](./media/cdn-token-auth/cdn-token-auth-url-allow4.png)
    
        - EC land is toegestaan: kunt alleen aanvragen die afkomstig uit een of meer opgegeven landen zijn. Aanvragen die afkomstig van alle andere landen zijn worden geweigerd. Landcode gebruiken voor het instellen van de parameters en elke landcode scheiden met komma's. Als u wilt toegang toestaan via Verenigde Staten en Frankrijk, voer ons FR in de kolom als hieronder.  
        
        ![CDN ec_country_allow voorbeeld](./media/cdn-token-auth/cdn-token-auth-country-allow.png)

        - EC land weigeren: aanvragen die afkomstig zijn uit een of meer opgegeven landen. Aanvragen die afkomstig van alle andere landen zijn kunnen worden. Landcode gebruiken voor het instellen van de parameters en elke landcode scheiden met komma's. Als u toegang weigeren van de Verenigde Staten en Frankrijk wilt, voer ons FR in de kolom.
    
        - ec_ref_allow: kunnen aanvragen van de opgegeven verwijzingsserver alleen. Een verwijzende identificeert de URL van de webpagina die is gekoppeld aan de aangevraagde resource. Het protocol niet opnemen in de parameterwaarde verwijzende site. De volgende typen van de invoer zijn toegestaan voor de waarde van parameter:
           - Een hostnaam of een hostnaam en een pad.
           - Meerdere verwijzingen. Als u wilt toevoegen in meerdere verwijzingen, elke verwijzende site met een komma te scheiden. Als u een waarde van verwijzende site opgeven, maar de verwijzende site-informatie niet in de aanvraag als gevolg van de browserconfiguratie van de wordt verzonden, worden deze aanvragen standaard geweigerd. 
           - Aanvragen met ontbrekende verwijzende site-informatie. Als u wilt toestaan dat dergelijke aanvragen, de tekst 'ontbreekt' of voer een lege waarde. 
           - Subdomeinen. Geef een sterretje (*) zodat subdomeinen. Bijvoorbeeld, om toe te staan alle subdomeinen van `consoto.com` Voer `*.consoto.com`. 
           
          Het volgende voorbeeld ziet u de invoer voor toegang toestaan voor aanvragen van `www.consoto.com`, alle subdomeinen onder `consoto2.com`, en aanvragen met verwijzingen leeg is of ontbreekt.
        
          ![CDN ec_ref_allow voorbeeld](./media/cdn-token-auth/cdn-token-auth-referrer-allow2.png)
    
        - EC ref weigeren: Hiermee worden aanvragen van de opgegeven verwijzende geweigerd. Raadpleeg de details en voorbeeld in parameter 'ec-ref-toestaan'.
         
        - EC protocol toestaan: kunt alleen aanvragen van het opgegeven protocol. Bijvoorbeeld, http of https.
        
        ![CDN ec_proto_allow voorbeeld](./media/cdn-token-auth/cdn-token-auth-url-allow4.png)
            
        - EC protocol weigeren: Hiermee worden aanvragen van het opgegeven protocol geweigerd. Bijvoorbeeld, http of https.
    
        - EC-client-IP: de toegang beperkt tot opgegeven aanvrager IP-adres. Zowel IPV4 als IPV6 worden ondersteund. U kunt één aanvraag IP-adres of IP-subnet opgeven.
            
        ![CDN ec_clientip voorbeeld](./media/cdn-token-auth/cdn-token-auth-clientip.png)
        
    3. U kunt uw token met het hulpprogramma beschrijving testen.

    4. U kunt ook het type reactie die wordt geretourneerd naar de gebruiker wanneer de aanvraag is geweigerd. We gebruiken standaard 403.

3. Klik nu op **regelengine** tabblad onder **HTTP grote**. U kunt op dit tabblad paden voor het toepassen van de functie, de functie tokenverificatie inschakelen en aanvullende verificatie-gerelateerde functies token inschakelen definiëren.

    - Gebruik 'Als' kolom asset of het pad dat u wilt toepassen tokenverificatie definiëren. 
    - Hiermee voegt 'Token Auth' uit de vervolgkeuzelijst functie token-verificatie inschakelen.
        
    ![Voorbeeld van CDN regels engine tokenverificatie inschakelen](./media/cdn-token-auth/cdn-rules-engine-enable2.png)

4. In de **regelengine** tabblad, zijn er enkele aanvullende functies die u kunt inschakelen.
    
    - Autorisatiecode DOS-token: bepaalt het type van de reactie die wordt geretourneerd naar de gebruiker wanneer een aanvraag wordt geweigerd. Regels die u hier instelt, wordt de instelling DOS-code op het tabblad token auth overschreven.
    - Token auth negeren: Hiermee wordt bepaald of de URL die wordt gebruikt voor het valideren van token hoofdlettergevoelig wordt.
    - Token auth-parameter: Wijzig de naam van het token auth Querytekenreeksparameter met in de aangevraagde URL. 
        
    ![CDN-regels in de engine tokenverificatie instellingen voorbeeld](./media/cdn-token-auth/cdn-rules-engine2.png)

5. Uw token dat is een toepassing die een token voor de verificatiefuncties op basis van tokens genereert, kunt u aanpassen. Broncode kan hier worden geopend in [GitHub](https://github.com/VerizonDigital/ectoken).
Beschikbare talen zijn onder andere:
    
    - C
    - C#
    - PHP
    - Perl
    - Java
    - Python    


## <a name="azure-cdn-features-and-provider-pricing"></a>Azure CDN-functies en prijzen-provider

Zie de [overzicht van CDN](cdn-overview.md) onderwerp.
