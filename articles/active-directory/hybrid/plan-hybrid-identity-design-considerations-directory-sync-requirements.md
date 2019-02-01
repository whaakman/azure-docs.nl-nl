---
title: Hybride identiteit ontwerp - vereisten voor directory-synchronisatie Azure | Microsoft Docs
description: Identificeren welke vereisten nodig zijn voor het synchroniseren van alle gebruikers tussen = op locatie en cloud voor ondernemingen.
documentationcenter: ''
services: active-directory
author: billmath
manager: daveba
editor: ''
ms.assetid: 593eaa71-17eb-4c16-8c98-43cc62987e65
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.subservice: hybrid
ms.author: billmath
ms.custom: seohack1
ms.openlocfilehash: d3ded429e960679b6bb252bdcec67c5a1d833ac1
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55496003"
---
# <a name="determine-directory-synchronization-requirements"></a>Bepaal de vereisten voor directory-synchronisatie
Synchronisatie is alles over biedt gebruikers een identiteit in de cloud op basis van hun on-premises-identiteit. Ongeacht of ze gesynchroniseerde account wordt gebruikt voor verificatie of federatieve verificatie, of niet wordt nog steeds de gebruikers moeten beschikken over een identiteit in de cloud.  Deze identiteit moet worden onderhouden en regelmatig bijgewerkt.  De updates kunnen vele vormen aannemen, van wijzigingen voor de titel te wijzigen van wachtwoorden.  

Gestart door het evalueren van de organisaties on-premises oplossing en de gebruiker identiteitsvereisten. Deze evaluatie is belangrijk om te definiëren van de technische vereisten voor hoe gebruikers-id's worden gemaakt en onderhouden in de cloud.  De meeste organisaties, on-premises Active Directory is en de on-premises adreslijst dat gebruikers door gesynchroniseerd uit, maar in sommige gevallen dit niet het geval zal zijn.  

Zorg ervoor dat u de volgende vragen beantwoorden:

* Hebt u één AD-forest, meerdere of geen?
  
  * Het aantal Azure AD-directory's wordt u synchroniseren met?
    
    1. Gebruikt u filtering?
    2. Hebt u meerdere Azure AD Connect-servers gepland?
* Op dit moment hebt u een synchronisatie van on-premises hulpprogramma?
  
  * Zo ja, biedt uw gebruikers als gebruikers met een virtuele map/integratie van identiteiten?
* Hebt u een andere on-premises directory die u wilt synchroniseren (bijvoorbeeld LDAP-adreslijst, HR-database, enzovoort)?
  * Gaat u naar eventuele GALSync doen?
  * Wat is de huidige status van de UPN in uw organisatie? 
  * Hebt u een andere adreslijst die gebruikers verificatie aan de hand?
  * Uw bedrijf maakt gebruik van Microsoft Exchange?
    * Bent ze van plan een hybride implementatie voor exchange hebben?

Nu u een idee hebt over de vereisten voor adreslijstsynchronisatie, moet u bepalen welk hulpprogramma het juiste abonnement om te voldoen aan deze vereisten.  Microsoft biedt verschillende hulpmiddelen om uit te voeren van directory-integratie en synchronisatie.  Zie de [vergelijkingstabel voor hybride identiteit directory-integratie-hulpprogramma's](plan-hybrid-identity-design-considerations-tools-comparison.md) voor meer informatie. 

Nu dat u hebt uw vereisten voor adreslijstsynchronisatie en het hulpprogramma dat dit voor uw bedrijf doet, moet u evalueren van de toepassingen die gebruikmaken van deze directoryservices. Deze evaluatie is belangrijk om te definiëren van de technische vereisten voor het integreren van deze toepassingen naar de cloud. Zorg ervoor dat u de volgende vragen beantwoorden:

* Deze toepassingen worden verplaatst naar de cloud en de map gebruiken?
* Zijn er speciale kenmerken die worden gesynchroniseerd met de cloud moeten, zodat deze toepassingen ze met succes kunnen gebruiken?
* Deze toepassingen moet opnieuw worden geschreven om te profiteren van cloud-verificatie?
* Blijft deze toepassingen met on-premises live terwijl gebruikers toegang krijgen tot deze met behulp van de cloudidentiteit van de?

U moet ook de beveiliging vereisten en beperkingen van directory-synchronisatie bepalen. Deze evaluatie is het belangrijk om een lijst van de vereisten die nodig zijn om te kunnen maken en beheren van gebruikers-id's in de cloud. Zorg ervoor dat u de volgende vragen beantwoorden:

* Waar de synchronisatieserver komen te staan?
* Worden deze toegevoegd aan een domein?
* De server bevindt zich in een beperkte netwerk achter een firewall, zoals een DMZ?
  * Kan u worden de benodigde firewallpoorten ter ondersteuning van synchronisatie openen?
* Hebt u een herstelplan na noodgevallen voor de synchronisatieserver?
* Hebt u een account met de juiste machtigingen voor alle forests die u wilt synchroniseren met?
  * Als uw bedrijf niet bekend met het antwoord voor deze vraag bent, Raadpleeg de sectie 'Machtigingen voor Wachtwoordsynchronisatie' in het artikel [installeren van de Azure Active Directory Sync Service](https://msdn.microsoft.com/library/azure/dn757602.aspx#BKMK_CreateAnADAccountForTheSyncService) en bepalen of u al een account hebt Met deze machtigingen of als u moet een te maken.
* Hebt u Azure-forest synchronisatie is de synchronisatieserver voor elk forest krijgen?

> [!NOTE]
> Zorg ervoor dat u elk antwoord noteert en de logica achter het antwoord begrijpt. [Bepaal de vereisten voor respons op incidenten](plan-hybrid-identity-design-considerations-incident-response-requirements.md) gaat over de beschikbare opties. Moet door deze vragen die kunt u selecteren welke optie het beste past bij uw bedrijf te beantwoorden.
> 
> 

## <a name="next-steps"></a>Volgende stappen
[Bepaal de vereisten voor meervoudige verificatie](plan-hybrid-identity-design-considerations-multifactor-auth-requirements.md)

## <a name="see-also"></a>Zie ook
[Overzicht ontwerpoverwegingen](plan-hybrid-identity-design-considerations-overview.md)

