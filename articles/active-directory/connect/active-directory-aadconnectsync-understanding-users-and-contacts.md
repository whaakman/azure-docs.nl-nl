---
title: 'Azure AD Connect-synchronisatie: inzicht krijgen in gebruikers, groepen en contactpersonen | Microsoft Docs'
description: Legt uit gebruikers, groepen en contactpersonen in Azure AD Connect-synchronisatie.
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
ms.assetid: 8d204647-213a-4519-bd62-49563c421602
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: markvi;andkjell
ms.openlocfilehash: 7f4bc51630653bfe341bfcb5c11699020053585a
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="azure-ad-connect-sync-understanding-users-groups-and-contacts"></a>Azure AD Connect-synchronisatie: inzicht krijgen in gebruikers, groepen en contactpersonen
Er zijn verschillende redenen waarom hebt u meerdere Active Directory-forests en er zijn diverse verschillende implementatietopologieën. Algemene modellen bevat de implementatie van een account-resource en GAL sync'ed forests na een fusie & overname. Maar zelfs als er pure modellen, hybride modellen, ook worden gebruikt. De standaardconfiguratie in Azure AD Connect-synchronisatie wordt niet wordt ervan uitgegaan dat een bepaald model maar, afhankelijk van hoe zoeken van overeenkomende gebruikers is geselecteerd in de installatiehandleiding, verschillende problemen kunnen worden waargenomen.

In dit onderwerp doorlopen we het gedrag van de standaardconfiguratie in bepaalde topologieën. We doorlopen dan de configuratie en de regeleditor synchronisatie kan worden gebruikt om te kijken naar de configuratie.

Er zijn enkele algemene regels voor de configuratie wordt ervan uitgegaan dat:
* Ongeacht de volgorde waarin we uit Active Directory's van de bron importeren moet het eindresultaat altijd hetzelfde zijn.
* Een actieve account altijd draagt bij aanmelden informatie, met inbegrip van **userPrincipalName** en **sourceAnchor**.
* Een uitgeschakeld account bijdragen userPrincipalName en sourceAnchor, tenzij het een gekoppeld postvak als er geen actieve rekening moet worden gezocht.
* Een account met een gekoppeld postvak wordt nooit worden gebruikt voor userPrincipalName en sourceAnchor. Ervan wordt uitgegaan dat er een actieve account later worden gevonden.
* Een contact-object kan worden ingericht met Azure AD als een contactpersoon of als een gebruiker. U weet niet zeker totdat alle bron Active Directory-forests zijn verwerkt.

## <a name="groups"></a>Groepen
Belangrijke punten moet denken bij het synchroniseren van groepen van Active Directory naar Azure AD:

* Azure AD Connect uitsluit ingebouwde beveiligingsgroepen van directory-synchronisatie.

* Azure AD Connect biedt geen ondersteuning voor het synchroniseren van [primaire groepslidmaatschappen](https://technet.microsoft.com/library/cc771489(v=ws.11).aspx) naar Azure AD.

* Azure AD Connect biedt geen ondersteuning voor het synchroniseren van [groepslidmaatschappen voor dynamische verdeling](https://technet.microsoft.com/library/bb123722(v=exchg.160).aspx) naar Azure AD.

* Om te synchroniseren naar Azure AD als een e-mailgroep Active Directory-groep:

    * Als de groep *proxyAddress* kenmerk leeg is, is de *mail* kenmerk moet een waarde hebben

    * Als de groep *proxyAddress* kenmerk is niet leeg zijn, moet ten minste één SMTP-proxy-adreswaarde bevatten. Hier volgen enkele voorbeelden:
    
      * Een Active Directory-groep waarvan proxyAddress-kenmerk heeft de waarde *{"X500:/0=contoso.com/ou=users/cn=testgroup"}* niet meer worden e-mailfunctionaliteit in Azure AD. Dit beschikt niet over een SMTP-adres.
      
      * Een Active Directory-groep waarvan proxyAddress-kenmerk waarden heeft *{"X500:/0=contoso.com/ou=users/cn=testgroup","SMTP:johndoe@contoso.com"}* worden e-mailfunctionaliteit in Azure AD.
      
      * Een Active Directory-groep waarvan proxyAddress-kenmerk waarden heeft *{'X500:/0=contoso.com/ou=users/cn=testgroup', 'smtp:johndoe@contoso.com"}* worden ook het e-mailfunctionaliteit in Azure AD.

## <a name="contacts"></a>Contactpersonen
Contactpersonen die een gebruiker in een ander forest dat is gebruikelijk dat na een fusie & overname waarbij een oplossing GALSync twee of meer Exchange-forests is bridging. Het contact-object lid altijd wordt van het connectorgebied overgebracht naar de metaverse met behulp van het e-mailkenmerk. Als er al een contactpersoon of gebruikersobject met hetzelfde e-mailadres, worden de objecten samengevoegd. Dit is geconfigureerd in de regel **In uit Active Directory: Neem contact op met Join**. Er is ook een regel met naam **In uit Active Directory: Neem contact op met algemene** met een kenmerkstroom met het metaverse-kenmerk **sourceObjectType** met de constante **Contact**. Deze regel zeer lage voorrang heeft dus als een gebruikersobject wordt gekoppeld aan hetzelfde metaverse-object, wordt de regel **In uit Active Directory-gebruiker algemene** 's dragen bij de waarde van gebruiker aan dit kenmerk. Met deze regel heeft dit kenmerk de waarde Contact als er geen gebruiker is toegevoegd en de gebruiker waarde als er ten minste één gebruiker zijn gevonden.

Voor het inrichten van een object naar Azure AD, de uitgaande regel **Out voor AAD: Neem contact op met Join** maakt contact-object als het metaverse-kenmerk **sourceObjectType** is ingesteld op **Neem contact op met**. Als dit kenmerk is ingesteld op **gebruiker**, klikt u vervolgens de regel **buiten het AAD-gebruiker toevoegen** in plaats daarvan maakt een gebruikersobject.
Het is mogelijk dat een object wordt gepromoveerd van contactpersoon aan gebruiker wanneer meer bron Active Directory's worden geïmporteerd en gesynchroniseerd.

Bijvoorbeeld in een topologie GALSync vindt we contact op met objecten voor iedereen in het tweede forest wanneer wij het eerste forest importeert. Dit wordt nieuwe contact op met de objecten in de AAD-Connector voorbereiden. Wanneer we later importeren en synchroniseren van het tweede forest, wordt de echte gebruikers zoeken en voeg ze toe aan de bestaande objecten in de metaverse. We vervolgens verwijdert u het contact-object in AAD en maakt een nieuw gebruikersobject in plaats daarvan.

Als u een topologie waarbij gebruikers worden weergegeven als contactpersonen hebt, zorg ervoor dat u selecteert om gebruikers op het e-mailkenmerk in de installatiehandleiding te vergelijken. Als u een andere optie selecteert, wordt u een order-afhankelijke configuratie hebben. Neem contact op met objecten altijd op de e-mailkenmerk wordt toegevoegd, maar gebruikersobjecten wordt alleen op het e-mailkenmerk toegevoegd als deze optie is geselecteerd in de installatiehandleiding. U kan vervolgens eindigen met twee verschillende objecten in de metaverse met hetzelfde kenmerk voor e-mail als het contact-object is geïmporteerd voordat het gebruikersobject. Tijdens het exporteren naar Azure AD, een fout gegenereerd. Dit gedrag is inherent en zou wijzen op beschadigde gegevens of de topologie is niet juist geïdentificeerd tijdens de installatie.

## <a name="disabled-accounts"></a>Uitgeschakelde accounts
Uitgeschakelde accounts worden ook gesynchroniseerd naar Azure AD. Uitgeschakelde accounts gelden voor bronnen in Exchange, bijvoorbeeld vergaderruimten vertegenwoordigen. De uitzondering hierop is de gebruikers met een gekoppeld postvak; zoals eerder vermeld, wordt deze nooit inrichten van een account met Azure AD.

De veronderstelling is dat als een uitgeschakelde gebruikersaccount wordt gevonden, wordt er een andere actieve account later geen wordt gevonden en het object is ingericht met Azure AD met de userPrincipalName en sourceAnchor gevonden. Als een andere actieve account wordt toegevoegd met hetzelfde metaverse-object, worden de userPrincipalName en sourceAnchor gebruikt.

## <a name="changing-sourceanchor"></a>SourceAnchor wijzigen
Wanneer een object is geëxporteerd naar Azure AD en het is niet toegestaan de sourceAnchor niet meer wijzigen. Wanneer het object is het metaverse-kenmerk geëxporteerd **cloudSourceAnchor** is ingesteld met de **sourceAnchor** waarde geaccepteerd door Azure AD. Als **sourceAnchor** wordt gewijzigd en niet overeen met **cloudSourceAnchor**, de regel **buiten het AAD-gebruiker toevoegen** de fout genereert **kenmerk sourceAnchor is gewijzigd**. In dit geval wordt moeten de configuratie of de gegevens worden gecorrigeerd zodat de dezelfde sourceAnchor aanwezig in de metaverse opnieuw is voordat het object opnieuw kan worden gesynchroniseerd.

## <a name="additional-resources"></a>Aanvullende resources
* [Azure AD Connect-synchronisatie: Synchronisatie-opties voor aanpassen](active-directory-aadconnectsync-whatis.md)
* [Uw on-premises identiteiten integreren met Azure Active Directory](active-directory-aadconnect.md)

