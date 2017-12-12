---
title: Inleiding tot beheer van apparaten in Azure Active Directory | Microsoft Docs
description: Leer hoe Apparaatbeheer kunt u controle over de apparaten die toegang hebben tot bronnen in uw omgeving.
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/14/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: faea960d2b6207aab5a2f5df8dc65fddc9ba5b54
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="introduction-to-device-management-in-azure-active-directory"></a>Inleiding tot beheer van apparaten in Azure Active Directory

In een wereld mobiel eerste, cloud eerste kunnen Azure Active Directory (Azure AD) eenmalige aanmelding aan apparaten, apps en services vanaf elke locatie. IT-professionals worden met de komst van apparaten - Bring Your Own Device (BYOD), waaronder momenteel geconfronteerd met twee tegengestelde doelstellingen:

- Zorgen dat eindgebruikers om productief te zijn overal en wanneer
- Beveiligen van zakelijke activa op elk gewenst moment

Via apparaten, worden uw gebruikers toegang tot uw zakelijke activa ophalen. Ter bescherming van uw zakelijke activa als IT-beheerder, die u wilt hebben controle over deze apparaten. Hiermee kunt u ervoor zorgen dat uw gebruikers toegang hebben tot de bronnen vanaf apparaten die voldoen aan uw standaarden voor beveiliging en naleving. 

Apparaatbeheer is ook de basis voor [voorwaardelijke toegang op basis van apparaten](active-directory-conditional-access-policy-connected-applications.md). Met voorwaardelijke toegang op basis van apparaten, kunt u ervoor zorgen dat toegang tot bronnen in uw omgeving is alleen mogelijk met vertrouwde apparaten.   

In dit onderwerp wordt uitgelegd hoe beheer van apparaten in Azure Active Directory werkt.

## <a name="getting-devices-under-the-control-of-azure-ad"></a>Ophalen van apparaten onder het beheer van Azure AD

Als u een apparaat onder het beheer van Azure AD, hebt u twee opties:

- Registreren 
- Samenvoegen

**Registreren van** een apparaat met Azure AD kunt u een apparaat-id te beheren. Wanneer een apparaat is geregistreerd, biedt Azure AD-apparaatregistratie het apparaat met een identiteit die wordt gebruikt voor verificatie van het apparaat wanneer een gebruiker zich aanmeldt bij Azure AD. U kunt de identiteit van de in- of een apparaat uitschakelen.

In combinatie met een MDM-oplossing voor mobiele apparaten, zoals Microsoft Intune, worden de apparaatkenmerken in Azure AD bijgewerkt met aanvullende informatie over het apparaat. Hiermee kunt u extra regels voor voorwaardelijke toegang maken die toegang afdwingen van apparaten, zodat ze voldoen aan uw standaarden voor beveiliging en compliance Zie voor meer informatie over de inschrijving van apparaten in Microsoft Intune, apparaten inschrijven voor beheer in Intune.

**Lid worden van** een apparaat is een uitbreiding van een apparaat wordt geregistreerd. Dit houdt in dat het geeft u de voordelen van het registreren van een apparaat, en daarnaast ook verandert de status van de lokale van een apparaat. Wijzigen van de lokale status, kunnen uw gebruikers zich aanmelden bij een apparaat met een organisatie werk- of schoolaccount in plaats van een persoonlijk account.

## <a name="azure-ad-registered-devices"></a>Azure AD ingeschreven apparaten   

Het doel van Azure AD ingeschreven apparaten is om u te bieden ondersteuning voor de **Bring Your Own Device (BYOD)** scenario. In dit scenario wordt een gebruiker toegang krijgen tot bronnen van van uw organisatie Azure Active Directory die worden beheerd met behulp van een persoonlijk apparaat.  

![Azure AD ingeschreven apparaten](./media/device-management-introduction/03.png)

De toegang is gebaseerd op een account voor werk of school die is ingevoerd op het apparaat.  
Windows 10 kunnen bijvoorbeeld gebruikers een werk- of schoolaccount toevoegen aan een pc, tablet of telefoon.  
Wanneer een gebruiker een werk- of schoolaccount, het apparaat is toegevoegd, is geregistreerd in Azure AD en eventueel geregistreerd in het management (MDM)-systeem voor mobiele apparaten die uw organisatie is geconfigureerd. Gebruikers van uw organisatie kunnen toevoegen van een werk- of schoolaccount naar een persoonlijk apparaat gemakkelijk:

- Bij het openen van een werktoepassing voor de eerste keer
- Handmatig via de **instellingen** menu in het geval van Windows 10 

U kunt Azure AD geregistreerde apparaten configureren voor Windows 10-, iOS-, Android- en Mac OS.

## <a name="azure-ad-joined-devices"></a>Azure AD die lid zijn van apparaten

Het doel van Azure AD die lid zijn van apparaten is voor het vereenvoudigen van:

- Windows-implementaties van apparaten die eigendom zijn van werkitems 
- Toegang tot de organisatie-apps en resources van een Windows-apparaat

![Azure AD ingeschreven apparaten](./media/device-management-introduction/02.png)


Deze doelstellingen worden bereikt door het verstrekken van uw gebruikers met een selfservice-ervaring voor het ophalen van apparaten die eigendom zijn van werk onder het beheer van Azure AD.  
**Azure AD Join** is bedoeld voor organisaties die cloud eerste / alleen in de cloud. Dit zijn meestal kleine en middelgrote bedrijven die geen een on-premises Windows Server Active Directory-infrastructuur. 

Implementatie van Azure AD die lid zijn van apparaten biedt de volgende voordelen:

- **Eenmalige aanmelding (SSO)** naar uw Azure beheerd SaaS-apps en services. Uw gebruikers zien geen extra authenticatie prompts bij het openen van bronnen op het werk. De functionaliteit van eenmalige aanmelding is zelfs wanneer ze niet zijn verbonden met het domeinnetwerk beschikbaar.

- **Enterprise compatibele roaming** van gebruikersinstellingen op gekoppelde apparaten. Gebruikers hoeven niet te verbinding maken met een Microsoft-account (bijvoorbeeld Hotmail) om te zien van instellingen op apparaten.

- **Toegang tot Windows Store voor bedrijven** met behulp van AD-account. Uw gebruikers kunnen kiezen uit een inventarisatie van toepassingen die vooraf zijn geselecteerd door de organisatie.

- **Windows Hello** ondersteuning voor veilig en eenvoudig toegang tot bronnen op het werk.

- **Beperking van toegang** tot apps van alleen de apparaten die voldoen aan nalevingsbeleid.

Terwijl Azure AD join is voornamelijk bedoeld voor organisaties waarvoor geen een on-premises Windows Server Active Directory-infrastructuur, kunt u gewoon ook worden gebruikt in scenario's waarbij:

- U kunt een domein lokale bijvoorbeeld niet gebruiken als moet u mobiele apparaten, zoals tablets en telefoons onder controle.

- Uw gebruikers moeten voornamelijk voor toegang tot Office 365 of andere SaaS-apps die is geïntegreerd met Azure AD.

- Wilt u een groep gebruikers beheren in Azure AD in plaats van in Active Directory. Dit kunt toepassen, bijvoorbeeld op seizoensgebonden werknemers, contractanten of studenten.

- Wilt u gekoppelde mogelijkheden bieden aan werknemers in externe filialen met beperkte on-premises infrastructuur.

U kunt Azure AD die lid zijn van apparaten voor Windows 10-apparaten kunt configureren.


## <a name="hybrid-azure-ad-joined-devices"></a>Hybride Azure AD die lid zijn van apparaten

Voor meer dan een tien jaar hebben veel organisaties een lid van het domein met hun lokale Active Directory gebruikt om in te schakelen:

- IT-afdelingen eigendom van het werk om apparaten te beheren vanaf een centrale locatie.

- Gebruikers aan te melden bij hun apparaten bij hun Active Directory werk- of schoolaccounts. 

Normaal gesproken organisaties met een lokale footprint afhankelijk zijn van installatiekopieën van de methoden voor het inrichten van apparaten en kunnen ze vaak gebruiken **System Center Configuration Manager (SCCM)** of **Groepsbeleid (GP)** beheren ze.

Als uw omgeving een on-premises AD-footprint en ook voordeel van de mogelijkheden van Azure Active Directory, kunt u hybride Azure AD die lid zijn van apparaten implementeren. Dit zijn apparaten die beide zijn gekoppeld aan uw on-premises Active Directory en uw Azure Active Directory.

![Azure AD ingeschreven apparaten](./media/device-management-introduction/01.png)


Gebruik Azure AD hybride die lid zijn van apparaten als:

- U hebt geïmplementeerd op deze apparaten die gebruikmaken van NTLM Win32-apps / Kerberos.

- Gewenste GP of SCCM / DCM om apparaten te beheren.

- U wilt blijven gebruiken imaging-oplossingen voor het configureren van apparaten voor uw werknemers.

U kunt configureren hybride Azure AD die lid zijn van apparaten voor Windows 10 en downlevel-apparaten, zoals Windows 8 en Windows 7.

## <a name="summary"></a>Samenvatting

Met Apparaatbeheer in Azure AD, kunt u het volgende doen: 

- Het proces van apparaten meebrengen onder het beheer van Azure AD

- Uw gebruikers een eenvoudig te gebruiken toegang bieden tot cloud-gebaseerde bronnen van uw organisatie

Als een regel van een miniatuur, moet u het volgende gebruiken:

- Azure AD ingeschreven apparaten:

    - Voor persoonlijke apparaten 

    - Apparaten handmatig registreren met Azure AD

- Azure AD die lid zijn van apparaten: 

    - Voor apparaten die eigendom zijn van uw organisatie

    - Voor apparaten die zijn **niet** lid is van een on-premises AD

    - Apparaten handmatig registreren met Azure AD

    - De lokale status van een apparaat wijzigen

- Hybride Azure AD die lid zijn van de apparaten voor apparaten die lid zijn van een on-premises AD     

    - Voor apparaten die eigendom zijn van uw organisatie

    - Voor apparaten die lid zijn van een on-premises AD

    - Automatisch apparaten te registreren bij Azure AD

    - De lokale status van een apparaat wijzigen



## <a name="next-steps"></a>Volgende stappen

- Als u een overzicht van het apparaat in de Azure portal beheren, Zie [apparaten beheert met de Azure-portal](device-management-azure-portal.md)

- Zie voor meer informatie over voorwaardelijke toegang op basis van apparaten, [Azure Active Directory-beleid voor voorwaardelijke toegang op basis van apparaten configureren](active-directory-conditional-access-policy-connected-applications.md).

- Voor setup:
    - Azure Active Directory geregistreerd Windows 10-apparaten, Zie [het configureren van Azure Active Directory geregistreerd Windows 10-apparaten](device-management-azuread-registered-devices-windows10-setup.md)
    - Azure Active Directory die lid zijn van apparaten, Zie [het configureren van Azure Active Directory die lid zijn van apparaten](device-management-azuread-joined-devices-setup.md)
    - Hybride Azure AD die lid zijn van apparaten, Zie [hoe het configureren van hybride Azure Active Directory die lid zijn van apparaten](device-management-hybrid-azuread-joined-devices-setup.md).


