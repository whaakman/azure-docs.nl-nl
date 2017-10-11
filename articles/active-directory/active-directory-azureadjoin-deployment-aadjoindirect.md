---
title: Gebruiksscenario's en overwegingen voor de implementatie voor deelname aan Azure AD | Microsoft Docs
description: Legt uit hoe beheerders kunnen Azure AD Join instellen voor hun eindgebruikers (werknemers, studenten en andere gebruikers). Ook de andere real-world scenario's voor het gebruik van Azure AD Join wordt besproken.
services: active-directory
documentationcenter: 
author: femila
manager: femila
editor: 
tags: azure-classic-portal
ms.assetid: 81d4461e-21c8-4fdd-9076-0e4991979f62
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: markvi
ms.openlocfilehash: fd0aab1a14bbd324e734e5efe8fe101e8a8dfefa
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="usage-scenarios-and-deployment-considerations-for-azure-ad-join"></a>Gebruiksscenario's en overwegingen voor de implementatie voor deelname aan Azure AD
## <a name="usage-scenarios-for-azure-ad-join"></a>Gebruiksscenario's voor Azure AD Join
### <a name="scenario-1-businesses-largely-in-the-cloud"></a>Scenario 1: Bedrijven grotendeels in de cloud
Azure Active Directory Join (Azure AD Join) profiteert u als u momenteel werkt en identiteiten voor uw bedrijf in de cloud beheren of naar de cloud snel verplaatst. U kunt een account dat u hebt gemaakt in Azure AD aan te melden bij Windows 10. Via [de eerste keer uitvoert (FRX) proces optreden](active-directory-azureadjoin-user-frx.md), of door deelname aan Azure AD van [het menu instellingen](active-directory-azureadjoin-user-upgrade.md), uw gebruikers kunnen toevoegen aan hun machines Azure AD.  Uw gebruikers kunnen ook profiteren van (eenmalige aanmelding SSO) toegang tot cloudbronnen zoals Office 365, in de browser of in de Office-toepassingen.

### <a name="scenario-2-educational-institutions"></a>Scenario 2: Onderwijsinstellingen
Onderwijsinstellingen hebben doorgaans twee gebruikerstypen: onderwijsmedewerkers en studenten. Onderwijsmedewerkers leden worden beschouwd als langere leden van de organisatie. Maken van lokale accounts voor ze is het wenselijk. Maar studenten korterlopende lid zijn van de organisatie en hun accounts kunnen worden beheerd in Azure AD. Dit betekent dat directory scale kan worden geactiveerd in de cloud in plaats van alleen lokaal opgeslagen. Het betekent ook dat studenten zich aanmelden bij Windows met hun Azure AD-accounts en toegang krijgen tot Office 365 resources in Office-toepassingen.

### <a name="scenario-3-retail-businesses"></a>Scenario 3: Retail bedrijven
Detailhandel bedrijven hebben seizoensgebonden werknemers en op lange termijn werknemers. U meestal on-premises-accounts maken en gebruiken van domein machines voor langere fulltime werknemers. Maar seizoensgebonden werknemers korterlopende lid zijn van de organisatie, en is het wenselijk om hun waar gebruikerslicenties kunnen eenvoudiger kan worden verplaatst om accounts te beheren. Als u hun gebruikersaccounts in de cloud met Office 365-licenties maakt, krijgt deze gebruikers de voordelen van aanmelding bij Windows en Office-toepassingen met een Azure AD-account, terwijl u meer flexibiliteit met de licenties worden onderhouden nadat ze verlaten.

### <a name="scenario-4-additional-scenarios"></a>Scenario 4: Aanvullende scenario 's
Samen met de voordelen die eerder is besproken, profiteert u dat uw gebruikers hun apparaten te koppelen aan Azure AD vanwege een vereenvoudigde gekoppelde ervaring, efficiënte Apparaatbeheer inschrijving voor beheer van automatische mobiele apparaten en eenmalige aanmelding bij Azure AD en on-premises resources.  

## <a name="deployment-considerations-for-azure-ad-join"></a>Overwegingen voor de implementatie voor deelname aan Azure AD
### <a name="enable-your-users-to-join-a-company-owned-device-directly-to-azure-ad"></a>Uw gebruikers lid worden van een apparaat in Bedrijfseigendom rechtstreeks naar Azure AD inschakelen
Ondernemingen kunnen alleen in de cloud-accounts bieden aan partnerbedrijven en organisaties. Deze partners kunnen vervolgens gemakkelijk toegang tot bedrijfs-apps en resources met eenmalige aanmelding. Dit scenario is van toepassing op gebruikers die toegang tot bronnen voornamelijk in de cloud, zoals Office 365 of SaaS-apps die afhankelijk van Azure AD voor verificatie zijn.

### <a name="prerequisites"></a>Vereisten
**Op ondernemingsniveau (beheerder)**

* Azure-abonnement met Azure Active Directory  

**Op het gebruikersniveau van de**

* Windows 10 (Professional en Enterprise-edities)

### <a name="administrator-tasks"></a>Beheerderstaken
* [Apparaatregistratie instellen](active-directory-azureadjoin-setup.md)

### <a name="user-tasks"></a>Gebruikerstaken
* [Instellen van een nieuwe Windows 10-apparaat met Azure AD tijdens de installatie](active-directory-azureadjoin-user-frx.md)
* [Instellen van een Windows 10-apparaat met Azure AD in het menu instellingen](active-directory-azureadjoin-user-upgrade.md)
* [Een persoonlijke Windows 10-apparaat koppelt aan uw organisatie](active-directory-azureadjoin-personal-device.md)

## <a name="enable-byod-in-your-organization-for-windows-10"></a>BYOD inschakelen in uw organisatie voor Windows 10
U kunt uw gebruikers en werknemers instellen om te gebruiken hun persoonlijke Windows-apparaten (BYOD) voor toegang tot bedrijfs-apps en resources. Uw gebruikers kunnen hun Azure AD-accounts (werk of school-accounts) toevoegen aan een persoonlijke Windows-apparaat voor toegang tot bronnen op een wijze beveiligd en compatibel zijn.

### <a name="prerequisites"></a>Vereisten
**Op ondernemingsniveau (beheerder)**

* Azure AD-abonnement

**Op het gebruikersniveau van de**

* Windows 10 (Professional en Enterprise-edities)

### <a name="administrator-tasks"></a>Beheerderstaken
* [Apparaatregistratie instellen](active-directory-azureadjoin-setup.md)

### <a name="user-tasks"></a>Gebruikerstaken
* [Een persoonlijke Windows 10-apparaat koppelt aan uw organisatie](active-directory-azureadjoin-personal-device.md)

## <a name="additional-information"></a>Aanvullende informatie
* [Windows 10 for the enterprise: Ways to use devices for work](active-directory-azureadjoin-windows10-devices-overview.md) (Windows 10 voor de onderneming: manieren om apparaten voor werk te gebruiken)
* [Extending cloud capabilities to Windows 10 devices through Azure Active Directory Join](active-directory-azureadjoin-user-upgrade.md) (Cloudmogelijkheden uitbreiden naar Windows 10-apparaten met behulp van Azure Active Directory Join)
* [Identiteiten zonder wachtwoorden via Microsoft Passport verifiëren](active-directory-azureadjoin-passport.md)
* [Learn about usage scenarios for Azure AD Join](active-directory-azureadjoin-deployment-aadjoindirect.md) (Gebruiksscenario’s voor Azure AD Join)
* [Connect domain-joined devices to Azure AD for Windows 10 experiences](active-directory-azureadjoin-devices-group-policy.md) (Apparaten die lid zijn van een domein verbinden met Azure AD voor Windows 10-ervaringen)
* [Azure AD Join instellen](active-directory-azureadjoin-setup.md)

