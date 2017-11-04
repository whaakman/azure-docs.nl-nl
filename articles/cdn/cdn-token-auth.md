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
ms.openlocfilehash: b1bcaf14e9805afa82c765092b62201f58c7cbc4
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/04/2017
---
# <a name="securing-azure-content-delivery-network-assets-with-token-authentication"></a>Azure Content Delivery Network activa met tokenverificatie beveiligen

[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>Overzicht

Tokenverificatie is een mechanisme waarmee u om te voorkomen dat het Azure Content Delivery Network (CDN) voor de activa aan niet-geautoriseerde clients. Tokenverificatie gewoonlijk wordt uitgevoerd om te voorkomen dat 'hotlinking' van inhoud, waarop een andere website, vaak een mededelingenbord uw assets zonder toestemming gebruikt. Hotlinking kunnen een invloed hebben op de kosten voor het leveren van inhoud. Als u deze functie op CDN inschakelt, worden aanvragen geverifieerd door rand CDN POP's voordat het CDN de inhoud biedt. 

## <a name="how-it-works"></a>Hoe werkt het?

Tokenverificatie controleert u of aanvragen worden gegenereerd door een vertrouwde site doordat aanvragen voor een token waarde bevatten die gecodeerd bevat informatie over de aanvrager. Inhoud is geleverd voor een aanvrager alleen als de gecodeerde gegevens voldoet aan de vereisten; anders worden aanvragen geweigerd. U kunt de vereisten instellen met behulp van een of meer van de volgende parameters:

- Land: Toestaan of weigeren van aanvragen die afkomstig uit de landen die is opgegeven zijn door hun [landcode](https://msdn.microsoft.com/library/mt761717.aspx).
- URL: Toestaan dat alleen aanvragen die overeenkomen met de opgegeven asset of het pad.
- Host: Toestaan of weigeren van aanvragen die gebruikmaken van de opgegeven hosts in de aanvraagheader.
- Verwijzende site: Toestaan of weigeren van aanvraag van de opgegeven verwijzende site.
- IP-adres: alleen aanvragen die afkomstig van specifieke IP-adres of IP-subnet zijn toestaan.
- Protocol: Toestaan of weigeren op basis van het protocol dat wordt gebruikt om aan te vragen van de inhoud aanvragen.
- Verlooptijd: een bepaalde datum en tijd om ervoor te zorgen dat er een koppeling geldig alleen voor een beperkte periode blijft toewijzen.

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

    1. Voer een unieke coderingssleutel voor **primaire sleutel** en voer een andere voor **back-up sleutel**.

        ![CDN-setup van token verificatiesleutel](./media/cdn-token-auth/cdn-token-auth-setupkey.png)
    
    2. Versleuteling parameters met het hulpprogramma versleutelen instellen. U kunt met het hulpprogramma versleutelen toestaan of weigeren van aanvragen op basis van de verlooptijd, land, verwijzende site, het protocol en client-IP (in een willekeurige combinatie).

        ![CDN hulpprogramma versleutelen](./media/cdn-token-auth/cdn-token-auth-encrypttool.png)

       - ec_expire: een verlooptijd wijst naar een token, waarna het token verloopt. Aanvragen verzonden nadat de vervaltijd worden geweigerd. Deze parameter wordt gebruikt voor een Unix-tijdstempel die is gebaseerd op het aantal seconden sinds de standard-epoche van `1/1/1970 00:00:00 GMT`. (U kunt online hulpprogramma's gebruiken om te converteren tussen stnd. tijd- en Unix-tijd.) Bijvoorbeeld, als u wilt dat voor het instellen van het token verloopt op `12/31/2016 12:00:00 GMT`, gebruikt u de waarde van het tijdstempel Unix `1483185600`als volgt. 
    
         ![CDN ec_expire voorbeeld](./media/cdn-token-auth/cdn-token-auth-expire2.png)
    
       - ec_url_allow: u kunt aangeven van tokens, voor een bepaalde asset of het pad. Dit beperkt de toegang tot aanvragen waarvan de URL beginnen met een specifieke relatief pad. URL's zijn hoofdlettergevoelig. Voer meerdere paden door elk pad scheiden met komma's. Afhankelijk van uw vereisten kunt u verschillende waarden instellen voor verschillende niveau van toegang. 
        
          Bijvoorbeeld: voor de URL `http://www.mydomain.com/pictures/city/strasbourg.png`, de invoerwaarde en het toegangsniveau als volgt instellen:

           - Invoerwaarde `"/"`: alle aanvragen zijn toegestaan.
           - Invoerwaarde `"/pictures`. De volgende aanvragen zijn toegestaan:
              - `http://www.mydomain.com/pictures.png`
              - `http://www.mydomain.com/pictures/city/strasbourg.png`
              - `http://www.mydomain.com/picturesnew/city/strasbourgh.png`
            - Invoerwaarde `/pictures/`: alleen aanvragen voor `/pictures/` zijn toegestaan. Bijvoorbeeld `http://www.mydomain.com/pictures/city/strasbourg.png`.
            - Invoerwaarde `/pictures/city/strasbourg.png`: alleen aanvragen voor deze specifieke activa zijn toegestaan.
    
          ![CDN ec_url_allow voorbeeld](./media/cdn-token-auth/cdn-token-auth-url-allow4.png)
    
       - ec_country_allow: kunt alleen aanvragen die afkomstig uit een of meer opgegeven landen zijn. Aanvragen die afkomstig van alle andere landen zijn worden geweigerd. Landcodes en elkaar met een komma te scheiden. Bijvoorbeeld, als u wilt toegang toestaan via alleen in de Verenigde Staten en Frankrijk, voer ons FR in het veld als volgt.  
        
           ![CDN ec_country_allow voorbeeld](./media/cdn-token-auth/cdn-token-auth-country-allow.png)

       - ec_country_deny: aanvragen die afkomstig uit een of meer opgegeven landen zijn. Aanvragen die afkomstig van alle andere landen zijn zijn toegestaan. Landcodes en elkaar met een komma te scheiden. Als u toegang weigeren van de Verenigde Staten en Frankrijk wilt, voer ons FR in het veld.
    
       - ec_ref_allow: staat alleen aanvragen van de opgegeven verwijzende site. Een verwijzende identificeert de URL van de webpagina die is gekoppeld aan de aangevraagde resource. Het protocol niet opnemen in de parameterwaarde verwijzende site. De volgende typen van de invoer zijn toegestaan voor de waarde van parameter:
           - Een hostnaam of een hostnaam en een pad.
           - Meerdere verwijzingen. Als u wilt toevoegen in meerdere verwijzingen, elke verwijzende site met een komma te scheiden. Als u een waarde van verwijzende site opgeven, maar de verwijzende site-informatie niet in de aanvraag als gevolg van de browserconfiguratie van de wordt verzonden, worden deze aanvragen standaard geweigerd. 
           - Aanvragen met ontbrekende verwijzende site-informatie. Als u wilt toestaan dat dergelijke aanvragen, de tekst 'ontbreekt' of voer een lege waarde. 
           - Subdomeinen. Geef een sterretje (*) zodat subdomeinen. Bijvoorbeeld, om toe te staan alle subdomeinen van `consoto.com` Voer `*.consoto.com`. 
           
          Het volgende voorbeeld ziet u de invoer voor toegang toestaan voor aanvragen van `www.consoto.com`, alle subdomeinen onder `consoto2.com`, en aanvragen met verwijzingen leeg is of ontbreekt.
        
          ![CDN ec_ref_allow voorbeeld](./media/cdn-token-auth/cdn-token-auth-referrer-allow2.png)
    
       - ec_ref_deny: Hiermee worden aanvragen van de opgegeven verwijzingsserver geweigerd. De implementatie is hetzelfde als de parameter ec_ref_allow.
         
       - ec_proto_allow: kunt alleen aanvragen van het opgegeven protocol. Bijvoorbeeld, HTTP of HTTPS.
        
         ![CDN ec_proto_allow voorbeeld](./media/cdn-token-auth/cdn-token-auth-url-allow4.png)
            
       - ec_proto_deny: Hiermee worden aanvragen van het opgegeven protocol geweigerd. Bijvoorbeeld, HTTP of HTTPS.
    
       - ec_clientip: de toegang beperkt tot de opgegeven aanvrager IP-adres. Zowel IPV4 als IPV6 worden ondersteund. U kunt een aanvraag voor één IP-adres of een IP-subnet opgeven.
            
         ![CDN ec_clientip voorbeeld](./media/cdn-token-auth/cdn-token-auth-clientip.png)
        
    3. Test indien gewenst uw token met het hulpprogramma beschrijving.

    4. Eventueel aanpassen van het type reactie die worden geretourneerd wanneer een aanvraag wordt geweigerd. Standaard wordt-antwoordcode 403 gebruikt.

3. Onder **HTTP grote**, klikt u op **regelengine**. De regelengine voor kunt u paden voor het toepassen van de functie, de functie tokenverificatie inschakelen en aanvullende verificatie-gerelateerde functies token inschakelen definiëren.

    1. Gebruik de **als** kolom voor het definiëren van de asset of het pad waarvan u wilt toepassen tokenverificatie. 
    2. Token als verificatie wilt inschakelen, selecteert u **Token Auth** van de **functies** vervolgkeuzelijst.
        
    ![Voorbeeld van CDN regels engine tokenverificatie inschakelen](./media/cdn-token-auth/cdn-rules-engine-enable2.png)

4. In de regelengine zijn er enkele aanvullende functies die u kunt inschakelen:
    
    - **Token DOS autorisatiecode**: bepaalt het type van de reactie die aan een gebruiker worden geretourneerd wanneer een aanvraag wordt geweigerd. Regels hier ingestelde instelling overschreven door de DOS-code in de **Token Auth** tabblad.
    - **URL-aanvraag met negeren Auth token**: Hiermee wordt bepaald of de URL die wordt gebruikt voor het valideren van het token hoofdlettergevoelig is.
    - **Token Auth Parameter**: wijzigt de naam van het token auth querytekenreeksparameter die wordt weergegeven in de aangevraagde URL. 
        
    ![CDN-regels in de engine tokenverificatie instellingen voorbeeld](./media/cdn-token-auth/cdn-rules-engine2.png)

5. Uw token, dat is een toepassing die een token voor de verificatiefuncties op basis van tokens genereert, kunt u aanpassen. Broncode kan worden geopend in [GitHub](https://github.com/VerizonDigital/ectoken).
Beschikbare talen zijn onder andere:
    
    - C
    - C#
    - PHP
    - Perl
    - Java
    - Python    


## <a name="azure-cdn-features-and-provider-pricing"></a>Azure CDN-functies en prijzen-provider

Zie voor informatie [overzicht van CDN](cdn-overview.md).
