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
ms.openlocfilehash: 42b182c314795b1ebf69639ec7ac5583208dc7c1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="securing-azure-cdn-assets-with-token-authentication"></a>Azure CDN activa met tokenverificatie beveiligen

[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

##<a name="overview"></a>Overzicht

Tokenverificatie is een mechanisme waarmee u om te voorkomen dat Azure CDN voor de activa aan niet-geautoriseerde clients.  Dit gebeurt meestal om te voorkomen dat 'hotlinking' van inhoud, waarbij een andere website, vaak een mededelingenbord uw assets zonder toestemming gebruikt.  Dit kan een invloed hebben op de kosten voor het leveren van inhoud. Als u deze functie op CDN inschakelt, wordt door rand CDN POP's voordat de inhoud aanvragen worden geverifieerd. 

## <a name="how-it-works"></a>Hoe werkt het?

Tokenverificatie controleert u of aanvragen worden gegenereerd door een vertrouwde site doordat aanvragen voor een token waarde gecodeerde dat informatie bevat over de aanvrager bevatten. Inhoud kan alleen worden geleverd aan aanvrager wanneer de gecodeerde gegevens voldoen aan de vereisten, worden anders aanvragen geweigerd. U kunt de vereiste met behulp van een of meer parameters die hieronder instellen.

- Land: toestaan of weigeren van aanvragen die afkomstig van bepaalde landen zijn.  [Lijst met geldige landcodes.](https://msdn.microsoft.com/library/mt761717.aspx) 
- URL: dat alleen bepaalde asset of het pad om aan te vragen.  
- Host: toestaan of weigeren-aanvragen via de opgegeven hosts in de aanvraagheader.
- Verwijzende site: toestaan of weigeren opgegeven verwijzende site om aan te vragen.
- IP-adres: ervoor zorgen dat alleen aanvragen die afkomstig van specifieke IP-adres of IP-subnet zijn.
- Protocol: toestaan of blokkeren aanvragen op basis van het protocol dat wordt gebruikt om aan te vragen van de inhoud.
- Verlooptijd: een bepaalde datum en tijd om ervoor te zorgen dat een koppeling alleen geldig voor een beperkte periode blijven toewijzen.

Zie het voorbeeld van de gedetailleerde configuratie van elke parameter.

## <a name="reference-architecture"></a>Referentiearchitectuur

Zie hieronder een referentiearchitectuur voor het instellen van tokenverificatie op CDN werken met uw Web-App.

![Knop blade CDN-profiel beheren](./media/cdn-token-auth/cdn-token-auth-workflow2.png)

## <a name="token-validation-logic-on-cdn-endpoint"></a>Token validatielogica op CDN-eindpunt
    
Dit diagram wordt beschreven hoe Azure CDN clientaanvraag valideert wanneer tokenverificatie is geconfigureerd op de CDN-eindpunt.

![Knop blade CDN-profiel beheren](./media/cdn-token-auth/cdn-token-auth-validation-logic.png)

## <a name="setting-up-token-authentication"></a>Token verificatie instellen

1. Van de [Azure-portal](https://portal.azure.com), blader naar uw CDN-profiel en klik vervolgens op de **beheren** knop starten van de aanvullende portal.

    ![Knop blade CDN-profiel beheren](./media/cdn-rules-engine/cdn-manage-btn.png)

2. Beweeg de muisaanwijzer over **HTTP grote**, en klik vervolgens op **Token Auth** in het doel. Stelt u de versleutelingssleutel en versleuteling parameters op dit tabblad.

    1. Voer een unieke coderingssleutel voor **primaire sleutel**.  Voer een andere voor **back-up-sleutel**

        ![CDN-setup van token verificatiesleutel](./media/cdn-token-auth/cdn-token-auth-setupkey.png)
    
    2. Versleuteling parameters met versleuteling hulpprogramma instellen (toestaan of weigeren van aanvragen op basis van de verlooptijd, land, verwijzende, protocol, client-IP. U kunt elke combinatie.)

        ![Knop blade CDN-profiel beheren](./media/cdn-token-auth/cdn-token-auth-encrypttool.png)

        - EC-verloopt: wijst een Vervaltijd van een token na een opgegeven periode. Aanvragen verzonden nadat de vervaltijd worden geweigerd. Deze parameter gebruikt Unix tijdstempel (op basis van seconden sinds standaard epoche van 1/1/1970 00:00:00 GMT. U kunt online hulpprogramma's gebruiken voor conversie tussen stnd. tijd- en Unix-tijd.)  Bijvoorbeeld, als u wilt dat voor het instellen van het token verloopt op 31-12-2016 te 12:00:00 GMT, gebruikt u de Unix-tijd: 1483185600 zoals hieronder:
    
        ![Knop blade CDN-profiel beheren](./media/cdn-token-auth/cdn-token-auth-expire2.png)
    
        - EC url toestaan: u kunt aangeven van tokens, voor een bepaalde asset of het pad. Dit beperkt de toegang tot aanvragen waarvan de URL beginnen met een specifieke relatief pad. U kunt meerdere paden elk pad scheiden met komma's invoeren. URL's zijn hoofdlettergevoelig. Afhankelijk van het vereiste kunt u andere waarde instellen voor verschillende niveau van toegang. Hieronder vindt u een aantal scenario's:
        
            Als u een URL: http://www.mydomain.com/pictures/city/strasbourg.png. Zie invoerwaarde "" en de toegang dienovereenkomstig serviceniveau

            1. Invoerwaarde '/': alle aanvragen kunnen worden
            2. Invoerwaarde '/ afbeeldingen': alle volgende aanvragen worden toestaan
            
                - http://www.mydomain.com/Pictures.PNG
                - http://www.mydomain.com/Pictures/City/Strasbourg.PNG
                - http://www.mydomain.com/picturesnew/City/strasbourgh.PNG
            3. Waarde '/ afbeeldingen /' worden ingevoerd: alleen aanvragen voor /pictures/ wordt toegestaan
            4. Invoerwaarde ' / pictures/city/strasbourg.png ': kunt u alleen aanvragen voor dit activum
    
        ![Knop blade CDN-profiel beheren](./media/cdn-token-auth/cdn-token-auth-url-allow4.png)
    
        - EC land is toegestaan: kunt alleen aanvragen die afkomstig uit een of meer opgegeven landen zijn. Aanvragen die afkomstig van alle andere landen zijn worden geweigerd. Landcode gebruiken voor het instellen van de parameters en elke landcode scheiden met komma's. Als u wilt toegang toestaan via Verenigde Staten en Frankrijk, voer ons FR in de kolom als hieronder.  
        
        ![Knop blade CDN-profiel beheren](./media/cdn-token-auth/cdn-token-auth-country-allow.png)

        - EC land weigeren: aanvragen die afkomstig zijn uit een of meer opgegeven landen. Aanvragen die afkomstig van alle andere landen zijn kunnen worden. Landcode gebruiken voor het instellen van de parameters en elke landcode scheiden met komma's. Als u toegang weigeren van de Verenigde Staten en Frankrijk wilt, voer ons FR in de kolom.
    
        - EC ref toestaan: staat alleen aanvragen van de opgegeven verwijzende site. Een verwijzende identificeert de URL van de webpagina die is gekoppeld aan de aangevraagde resource. De parameterwaarde verwijzende site mag niet het protocol bevatten. U kunt een hostnaam en/of een bepaalde pad op dat hostnaam invoeren. U kunt ook meerdere verwijzende sites binnen een enkele parameter scheiden met komma's toevoegen. Als u een waarde van verwijzende site hebt opgegeven, maar de verwijzende site-informatie niet in de aanvraag als gevolg van een browserconfiguratie wordt verzonden, wordt standaard deze aanvragen worden geweigerd. U kunt toewijzen 'Ontbreekt' of een lege waarde in de parameter waarmee deze aanvragen met ontbrekende verwijzende site-informatie. U kunt ook gebruiken ' *. consoto.com ' om toe te staan alle subdomeinen van consoto.com.  Bijvoorbeeld, als u toegang toestaan voor aanvragen van www.consoto.com, alle subdomeinen onder consoto2.com en erquests met reffers leeg is of ontbreekt wilt, invoerwaarde hieronder:
        
        ![Knop blade CDN-profiel beheren](./media/cdn-token-auth/cdn-token-auth-referrer-allow2.png)
    
        - EC ref weigeren: Hiermee worden aanvragen van de opgegeven verwijzende geweigerd. Raadpleeg de details en voorbeeld in parameter 'ec-ref-toestaan'.
         
        - EC protocol toestaan: kunt alleen aanvragen van het opgegeven protocol. Bijvoorbeeld, http of https.
        
        ![Knop blade CDN-profiel beheren](./media/cdn-token-auth/cdn-token-auth-url-allow4.png)
            
        - EC protocol weigeren: Hiermee worden aanvragen van het opgegeven protocol geweigerd. Bijvoorbeeld, http of https.
    
        - EC-client-IP: de toegang beperkt tot opgegeven aanvrager IP-adres. Zowel IPV4 als IPV6 worden ondersteund. U kunt één aanvraag IP-adres of IP-subnet opgeven.
            
        ![Knop blade CDN-profiel beheren](./media/cdn-token-auth/cdn-token-auth-clientip.png)
        
    3. U kunt uw token met het hulpprogramma decodering testen.

    4. U kunt ook het type reactie die wordt geretourneerd naar de gebruiker wanneer de aanvraag is geweigerd. We gebruiken standaard 403.

3. Klik nu op **regelengine** tabblad onder **HTTP grote**. U kunt op dit tabblad paden voor het toepassen van de functie, de functie tokenverificatie inschakelen en inschakelen definiëren aanvullende tokenverificatie gerelateerde mogelijkheden.

    - Gebruik 'Als' kolom asset of het pad dat u wilt toepassen tokenverificatie definiëren. 
    - Hiermee voegt 'Token Auth' uit de vervolgkeuzelijst functie token-verificatie inschakelen.
        
    ![Knop blade CDN-profiel beheren](./media/cdn-token-auth/cdn-rules-engine-enable2.png)

4. In de **regelengine** tabblad, zijn er enkele aanvullende functies die u kunt inschakelen.
    
    - Autorisatiecode DOS-token: bepaalt het type van de reactie die wordt geretourneerd naar de gebruiker wanneer een aanvraag wordt geweigerd. Regels die u hier instelt, wordt de instelling DOS-code op het tabblad token auth overschreven.
    - Token auth negeren: Hiermee wordt bepaald of de URL die wordt gebruikt voor het valideren van token hoofdlettergevoelig wordt.
    - Token auth-parameter: Wijzig de naam van het token auth Querytekenreeksparameter met in de aangevraagde URL. 
        
    ![Knop blade CDN-profiel beheren](./media/cdn-token-auth/cdn-rules-engine2.png)

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
