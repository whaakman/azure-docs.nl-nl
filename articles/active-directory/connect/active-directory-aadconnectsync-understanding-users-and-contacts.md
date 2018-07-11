---
title: 'Azure AD Connect-synchronisatie: inzicht krijgen in gebruikers, groepen en contactpersonen | Microsoft Docs'
description: Gebruikers, groepen en contactpersonen in Azure AD Connect-synchronisatie uitgelegd.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
ms.assetid: 8d204647-213a-4519-bd62-49563c421602
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 740908a87752d4e3b94e0cd25fa7107c45b9f0d7
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2018
ms.locfileid: "37915320"
---
# <a name="azure-ad-connect-sync-understanding-users-groups-and-contacts"></a>Azure AD Connect-synchronisatie: inzicht krijgen in gebruikers, groepen en contactpersonen
Er zijn verschillende redenen waarom zijn er meerdere Active Directory-forests en er zijn diverse verschillende implementatietopologieën. Algemene modellen omvatten de implementatie van een account-resource en GAL sync'ed forests na een fusie & overname. Maar zelfs als er pure modellen, hybride modellen ook algemene zijn. De standaardconfiguratie in Azure AD Connect-synchronisatie niet wordt ervan uitgegaan een bepaald model, maar afhankelijk van hoe overeenkomende gebruiker is geselecteerd in de installatiehandleiding, verschillend gedrag kunnen worden waargenomen.

In dit onderwerp doorlopen we de werking van de standaardconfiguratie in bepaalde topologieën. We doorlopen de configuratie en de Synchronization Rules Editor kan worden gebruikt om te kijken naar de configuratie.

Er zijn enkele algemene regels voor de configuratie wordt ervan uitgegaan dat:
* Ongeacht welke volgorde we uit Active Directory's van de bron importeren, moet het eindresultaat altijd hetzelfde zijn.
* Een actief account altijd telt mee voor het aanmelden informatie, met inbegrip van **userPrincipalName** en **sourceAnchor**.
* Een uitgeschakeld account bijdragen userPrincipalName en sourceAnchor, tenzij het een gekoppeld postvak, als er geen actieve accounts worden gevonden.
* Een account met een gekoppeld postvak wordt nooit worden gebruikt voor de userPrincipalName en sourceAnchor. Ervan wordt uitgegaan dat een actief account later worden gevonden.
* Een contact-object kan worden ingericht met Azure AD als een contactpersoon of als een gebruiker. U weet niet zeker totdat alle bron Active Directory-forests is verwerkt.

## <a name="groups"></a>Groepen
Belangrijke punten om rekening mee bij het synchroniseren van groepen uit Active Directory met Azure AD:

* Azure AD Connect sluit ingebouwde beveiligingsgroepen van directory-synchronisatie.

* Azure AD Connect biedt geen ondersteuning voor het synchroniseren van [primaire groepslidmaatschappen](https://technet.microsoft.com/library/cc771489(v=ws.11).aspx) naar Azure AD.

* Azure AD Connect biedt geen ondersteuning voor het synchroniseren van [lidmaatschappen voor distributiegroepen dynamische](https://technet.microsoft.com/library/bb123722(v=exchg.160).aspx) naar Azure AD.

* Om te synchroniseren naar Azure AD als een e-mailgroep Active Directory-groep:

    * Als van de groep *proxyAddress* kenmerk leeg is, is de *e-mail* kenmerk moet een waarde hebben

    * Als van de groep *proxyAddress* kenmerk is niet leeg zijn, moet ten minste één SMTP-proxy-adreswaarde bevatten. Hier volgen enkele voorbeelden:
    
      * Een Active Directory-groep waarvan proxyAddress-kenmerk heeft de waarde *{"X500:/0=contoso.com/ou=users/cn=testgroup"}* niet meer worden e-mailadres in Azure AD. Er is geen een SMTP-adres.
      
      * Een Active Directory-groep waarvan proxyAddress-kenmerk waarden heeft *{"X500:/0=contoso.com/ou=users/cn=testgroup","SMTP:johndoe@contoso.com"}* worden e-mailadres in Azure AD.
      
      * Een Active Directory-groep waarvan proxyAddress-kenmerk waarden heeft *{"X500:/0=contoso.com/ou=users/cn=testgroup", "smtp:johndoe@contoso.com"}* worden ook het e-mailadres in Azure AD.

## <a name="contacts"></a>Contactpersonen
Contactpersonen die een gebruiker in een ander forest dat is gebruikelijk na een fusie & aanschaf waarbij een oplossing GALSync twee of meer Exchange-forests is bridging. Het contact-object is altijd van het connectorgebied toevoegen aan de metaverse met behulp van het e-mailkenmerk. Als er al een contact- of gebruikersobject met hetzelfde e-mailadres, de objecten aan elkaar worden gekoppeld. Dit is geconfigureerd in de regel **In uit Active Directory: Neem contact op met Join**. Er is ook een regel met de naam **In uit Active Directory: Neem contact op met algemene** met een kenmerkstroom met het metaverse-kenmerk **sourceObjectType** met de constante **Neem contact op met**. Deze regel bevat een zeer lage prioriteit, als een gebruikersobject is toegevoegd aan de dezelfde metaverse-object, wordt de regel **In uit Active Directory-gebruiker algemene** telt mee voor de waarde die gebruiker moet dit kenmerk. Dit kenmerk wordt met deze regel hebben de waarde Contact als er geen gebruiker is toegevoegd en de waarde gebruiker als ten minste één gebruiker is gevonden.

Voor het inrichten van een object met Azure AD, de regel voor uitgaande **Out naar AAD: Neem contact op met Join** maakt een contact-object als het metaverse-kenmerk **sourceObjectType** is ingesteld op **Neem contact op met**. Als dit kenmerk is ingesteld op **gebruiker**, klikt u vervolgens de regel **Out voor AAD-gebruiker toevoegen** maakt u een gebruikersobject in plaats daarvan.
Het is mogelijk dat een object wordt gepromoveerd van contact op met de gebruiker wanneer meer bron Active Directory's worden geïmporteerd en gesynchroniseerd.

Bijvoorbeeld in een topologie GALSync vindt we contact op met objecten voor iedereen in het tweede forest wanneer we de eerste forest importeert. Dit plannen nieuwe contact op met objecten in de AAD-Connector. Wanneer we later importeren en synchroniseren van het tweede forest, wordt de echte gebruikers zoeken en knooppunten toe aan de bestaande objecten in de metaverse. We zullen het contact-object in AAD verwijderen en in plaats daarvan een nieuwe gebruikersobject maken.

Als u een topologie waar gebruikers worden weergegeven als contactpersonen hebt, zorg ervoor dat u selecteert om gebruikers op het e-mailkenmerk in de installatiehandleiding te vergelijken. Als u een andere optie selecteert, wordt u een order-afhankelijke-configuratie hebben. Neem contact op met objecten altijd deel gaan uitmaken van het e-mailkenmerk, maar gebruikersobjecten wordt alleen deelnemen aan op het e-mailkenmerk als deze optie is geselecteerd in de installatiehandleiding. U kan zich vervolgens uiteindelijk met twee verschillende objecten in de metaverse met hetzelfde e-mailkenmerk als het contact-object is geïmporteerd voordat het gebruikersobject. Tijdens het exporteren naar Azure AD, een fout gegenereerd. Dit gedrag is zo ontworpen en zou wijzen op beschadigde gegevens of dat de topologie niet goed werd geïdentificeerd tijdens de installatie.

## <a name="disabled-accounts"></a>Uitgeschakelde accounts
Uitgeschakelde accounts worden ook gesynchroniseerd naar Azure AD. Uitgeschakelde accounts gelden voor resources in Exchange, bijvoorbeeld vergaderruimten vertegenwoordigen. De uitzondering hierop is de gebruikers met een gekoppeld postvak; zoals eerder vermeld, wordt deze nooit een Azure AD-account ingericht.

De veronderstelling is dat als een uitgeschakelde gebruikersaccount wordt gevonden, wordt er een andere actieve account geen later gevonden wordt en het object is ingericht met Azure AD met de userPrincipalName en sourceAnchor gevonden. In het geval een andere actieve account wordt toevoegen aan de dezelfde metaverse-object, wordt klikt u vervolgens de userPrincipalName en sourceAnchor gebruikt.

## <a name="changing-sourceanchor"></a>SourceAnchor wijzigen
Wanneer een object is geëxporteerd naar Azure AD en het is niet toegestaan de sourceAnchor niet meer wijzigen. Wanneer het object is geëxporteerd van het metaverse-kenmerk **cloudSourceAnchor** is ingesteld met de **sourceAnchor** waarde geaccepteerd door Azure AD. Als **sourceAnchor** wordt gewijzigd en niet overeen met **cloudSourceAnchor**, de regel **Out voor AAD-gebruiker toevoegen** de fout genereert **sourceAnchor-kenmerk heeft gewijzigd**. In dit geval wordt moeten de configuratie of de gegevens worden gecorrigeerd, zodat het dezelfde sourceAnchor aanwezig in de metaverse opnieuw is voordat het object opnieuw kan worden gesynchroniseerd.

## <a name="additional-resources"></a>Aanvullende resources
* [Azure AD Connect Sync: Synchronisatieopties aanpassen](active-directory-aadconnectsync-whatis.md)
* [Uw on-premises identiteiten integreren met Azure Active Directory](active-directory-aadconnect.md)

