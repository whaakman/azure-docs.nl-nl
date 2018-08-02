---
title: Gebruiksscenario's en overwegingen voor de implementatie voor Azure AD Join | Microsoft Docs
description: Wordt uitgelegd hoe beheerders van Azure AD Join kunnen instellen voor hun eindgebruikers (werknemers, studenten, andere gebruikers). Hierin worden ook de verschillende echte scenario's voor het gebruik van Azure AD Join.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
tags: azure-classic-portal
ms.component: devices
ms.assetid: 81d4461e-21c8-4fdd-9076-0e4991979f62
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2018
ms.author: markvi
ms.openlocfilehash: a145b4184fbebd9c4f447face1c47bec20c0ec32
ms.sourcegitcommit: 96f498de91984321614f09d796ca88887c4bd2fb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39414887"
---
# <a name="usage-scenarios-and-deployment-considerations-for-azure-ad-join"></a>Gebruiksscenario's en overwegingen voor de implementatie voor Azure AD Join
## <a name="usage-scenarios-for-azure-ad-join"></a>Gebruiksscenario's voor Azure AD Join
### <a name="scenario-1-businesses-largely-in-the-cloud"></a>Scenario 1: Bedrijven grotendeels in de cloud
Azure Active Directory Join (Azure AD Join) kunt u profiteren van als u momenteel werkt en identiteiten voor uw bedrijf in de cloud beheren of snel worden verplaatst naar de cloud. U kunt een account dat u hebt gemaakt in Azure AD om aan te melden bij Windows 10. Via [de eerste keer uitvoert (FRX) proces optreden](azuread-joined-devices-frx.md), of lid wordt van Azure AD van [het instellingenmenu](../user-help/device-management-azuread-joined-devices-setup.md), uw gebruikers kunnen hun computers toevoegen aan Azure AD.  Uw gebruikers kunnen ook profiteren van eenmalige aanmelding (SSO) toegang tot cloudbronnen zoals Office 365, in hun browser of in Office-toepassingen.

### <a name="scenario-2-educational-institutions"></a>Scenario 2: Onderwijsinstellingen
Onderwijsinstellingen hebben doorgaans twee gebruikerstypen: faculteit en studenten. Faculteitsleden worden beschouwd als op de langere termijn leden van de organisatie. Het maken van on-premises-accounts voor hen is wenselijk. Maar studenten korterlopende lid zijn van de organisatie en hun accounts kunnen worden beheerd in Azure AD. Dit betekent dat directory schaal kan worden gepusht naar de cloud in plaats van lokaal opgeslagen. Het betekent ook dat studenten zich aanmelden bij Windows met hun Azure AD-accounts en toegang krijgen tot Office 365 bronnen in Office-toepassingen.

### <a name="scenario-3-retail-businesses"></a>Scenario 3: Retail bedrijven
Detailhandel bedrijven hebben seizoenswerkers en op lange termijn werknemers. U doorgaans on-premises-accounts maken en gebruiken van machines domein voor langere fulltime werknemers. Maar seizoenswerkers korterlopende lid zijn van de organisatie en is het wenselijk om hun waar gebruikerslicenties kunnen eenvoudiger kan worden verplaatst om accounts te beheren. Wanneer u de gebruikersaccounts in de cloud met Office 365-licenties maakt, krijgen deze gebruikers de voordelen van het aan te melden bij Windows en Office-toepassingen met een Azure AD-account terwijl u meer flexibiliteit in combinatie met de licenties worden onderhouden nadat ze verlaten.

### <a name="scenario-4-additional-scenarios"></a>Scenario 4: Extra scenario 's
Samen met de voordelen die eerder zijn besproken, profiteert u met uw gebruikers hun apparaten toevoegen aan Azure AD vanwege een vereenvoudigde lid te worden ervaring, efficiënte Apparaatbeheer inschrijving van automatische mobiele apparaten beheren en eenmalige aanmelding met Azure AD en on-premises bronnen.  

## <a name="deployment-considerations-for-azure-ad-join"></a>Overwegingen voor de implementatie voor Azure AD Join
### <a name="enable-your-users-to-join-a-company-owned-device-directly-to-azure-ad"></a>Uw gebruikers lid worden van een apparaat Bedrijfseigendom rechtstreeks naar Azure AD inschakelen
Ondernemingen kunnen alleen in de cloud-accounts bieden aan partnerbedrijven en organisaties. Deze partners hebben vervolgens eenvoudig toegang tot bedrijfs-apps en resources met eenmalige aanmelding. In dit scenario is van toepassing op gebruikers die toegang krijgen tot bronnen voornamelijk in de cloud, zoals Office 365 of SaaS-apps die afhankelijk van Azure AD voor verificatie zijn.

### <a name="prerequisites"></a>Vereisten
**Op de enterprise-niveau (beheerder)**

* Azure-abonnement met Azure Active Directory  

**Op het gebruikersniveau van de**

* Windows 10 (Professional en Enterprise-edities)

### <a name="administrator-tasks"></a>Beheerderstaken
* [Apparaatregistratie instellen](device-management-azure-portal.md)

### <a name="user-tasks"></a>Taken van de gebruiker
* [Instellen van een nieuwe Windows 10-apparaat met Azure AD tijdens de installatie](azuread-joined-devices-frx.md)
* [Een Windows 10-apparaat met Azure AD instellen in het instellingenmenu](../user-help/device-management-azuread-registered-devices-windows10-setup.md)
* [Een persoonlijke Windows 10-apparaat toevoegen aan uw organisatie](../user-help/device-management-azuread-joined-devices-setup.md)

## <a name="enable-byod-in-your-organization-for-windows-10"></a>Inschakelen van BYOD in uw organisatie voor Windows 10
U kunt uw gebruikers en medewerkers instellen op hun persoonlijke Windows-apparaten (BYOD) voor toegang tot bedrijfs-apps en resources gebruiken. Uw gebruikers kunnen hun Azure AD-accounts (werk of school-accounts) toevoegen aan een persoonlijke Windows-apparaat voor toegang tot resources op een manier beveiligd en compatibel zijn.

### <a name="prerequisites"></a>Vereisten
**Op de enterprise-niveau (beheerder)**

* Azure AD-abonnement

**Op het gebruikersniveau van de**

* Windows 10 (Professional en Enterprise-edities)

### <a name="administrator-tasks"></a>Beheerderstaken
* [Apparaatregistratie instellen](device-management-azure-portal.md)

### <a name="user-tasks"></a>Taken van de gebruiker
* [Een persoonlijke Windows 10-apparaat toevoegen aan uw organisatie](../user-help/device-management-azuread-joined-devices-setup.md)

## <a name="next-steps"></a>Volgende stappen

- [Apparaatbeheer](overview.md)

