---
title: Azure Active Directory hybride identiteit ontwerpoverwegingen - directory-synchronisatievereisten bepalen | Microsoft Docs
description: Identificeren welke vereisten nodig zijn voor het synchroniseren van alle gebruikers tussen on = eigen locatie en cloud voor de onderneming.
documentationcenter: 
services: active-directory
author: billmath
manager: femila
editor: 
ms.assetid: 593eaa71-17eb-4c16-8c98-43cc62987e65
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 5ef87e606f055359ca325befd6048353ce57ca2b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="determine-directory-synchronization-requirements"></a>Vereisten voor directory-synchronisatie bepalen
Synchronisatie is alles over een identiteit in de cloud op basis van hun on-premises identiteits zodat gebruikers beschikken. Of ze gesynchroniseerde account wordt gebruikt voor verificatie of federatieve verificatie, of niet wordt nog steeds de gebruikers moeten een identiteit hebben in de cloud.  Deze identiteit moet worden behouden en regelmatig bijgewerkt.  De updates kunnen vele vormen aannemen, wijzigingen van de titel aan wachtwoord te wijzigen.  

Begin met het evalueren van de organisaties on-premises identity-oplossing en de gebruiker vereisten. Deze evaluatie is belangrijk om te definiëren van de technische vereisten voor hoe gebruikers-id's worden gemaakt en onderhouden in de cloud.  De meeste organisaties, Active Directory is lokale en de on-premises adreslijst dat gebruikers worden door gesynchroniseerd uit, maar in sommige gevallen dit niet het geval zal zijn.  

Zorg ervoor dat de volgende vragen beantwoorden:

* Hebt u één AD-forest, meerdere of geen?
  
  * Hoeveel Azure AD-mappen wordt u synchroniseren met?
    
    1. U gebruikt voor het filteren?
    2. Hebt u meerdere Azure AD Connect-servers gepland?
* Op dit moment hebt u een synchronisatie lokale hulpprogramma?
  
  * Zo ja, biedt uw gebruikers als gebruikers beschikken over een virtuele map/integratie van identiteiten?
* Hebt u een andere directory lokale die u wilt synchroniseren (bijvoorbeeld LDAP-adreslijst, HR-database, enzovoort)?
  * Gaat u een GALSync doen?
  * Wat is de huidige status van de UPN's in uw organisatie? 
  * Hebt u een andere map die het authenticeren van gebruikers?
  * Maakt uw bedrijf gebruik van Microsoft Exchange?
    * Wel ze van plan bent een hybride implementatie voor exchange hebben?

Nu u een idee hebt over de synchronisatievereisten van uw, moet u bepalen welke hulpprogramma is het juiste project aan deze vereisten voldoet.  Microsoft biedt verschillende hulpmiddelen voor het uitvoeren van Active directory-integratie en synchronisatie.  Zie de [extra vergelijkingstabel voor hybride identiteit directory-integratie](active-directory-hybrid-identity-design-considerations-tools-comparison.md) voor meer informatie. 

Nu u uw synchronisatievereisten en het hulpprogramma dat u dit voor uw bedrijf doen hebt, moet u evalueren van de toepassingen die gebruikmaken van deze directoryservices. Deze evaluatie is belangrijk om te definiëren van de technische vereisten voor het integreren van deze toepassingen naar de cloud. Zorg ervoor dat de volgende vragen beantwoorden:

* Deze toepassingen worden verplaatst naar de cloud en gebruikt de directory?
* Zijn er speciale kenmerken die worden gesynchroniseerd met de cloud moeten, zodat deze toepassingen kunnen worden gebruikt met succes?
* Deze toepassingen moet opnieuw worden geschreven om te profiteren van cloud-verificatie?
* Blijven deze toepassingen lokale live terwijl gebruikers toegang krijgen tot deze met de cloudidentiteit van de?

U moet ook de beveiliging vereisten en beperkingen adreslijstsynchronisatie bepalen. Deze evaluatie is belangrijk om een lijst met de vereisten die nodig zijn om te kunnen maken en beheren van gebruikers-id's in de cloud. Zorg ervoor dat de volgende vragen beantwoorden:

* Waar de synchronisatieserver komen te staan?
* Kan het zijn verbonden met het domein?
* De server bevindt zich in een beperkte netwerk achter een firewall, zoals een Perimeternetwerk?
  * U mag de vereiste firewallpoorten ter ondersteuning van synchronisatie openen?
* Hebt u een noodherstelplan voor de synchronisatieserver?
* Hebt u een account met de juiste machtigingen voor alle forests die u wilt synchroniseren met?
  * Als uw bedrijf kan het antwoord voor deze vraag niet weet, Raadpleeg de sectie 'Machtigingen voor Wachtwoordsynchronisatie' in het artikel [installeren van de Azure Active Directory-synchronisatieservice](https://msdn.microsoft.com/library/azure/dn757602.aspx#BKMK_CreateAnADAccountForTheSyncService) en bepalen of u al een account met deze machtigingen hebt of als u wilt maken.
* Als u synchronisatie zijn forest kan de synchronisatieserver ophalen voor elk forest?

> [!NOTE]
> Zorg ervoor dat elk antwoord noteert de logica achter het antwoord begrijpt. [Vereisten voor respons op incidenten bepalen](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md) gaat over de beschikbare opties. Moet door deze vragen die u selecteert welke optie het beste past bij uw bedrijf te beantwoorden.
> 
> 

## <a name="next-steps"></a>Volgende stappen
[Vereisten multi-factor authentication bepalen](active-directory-hybrid-identity-design-considerations-multifactor-auth-requirements.md)

## <a name="see-also"></a>Zie ook
[Overzicht ontwerpoverwegingen](active-directory-hybrid-identity-design-considerations-overview.md)

