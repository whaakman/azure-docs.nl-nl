---
title: Wat is hybride identiteit met Azure Active Directory?
description: Hybride identiteit heeft een gemeenschappelijke gebruikersidentiteit in voor verificatie en autorisatie zowel on-premises en in de cloud.
keywords: inleiding tot Azure AD Connect, overzicht Azure AD Connect, wat is Azure AD Connect, Active Directory installeren
services: active-directory
author: billmath
manager: daveba
ms.assetid: 59bd209e-30d7-4a89-ae7a-e415969825ea
ms.service: active-directory
ms.workload: identity
ms.topic: overview
ms.date: 05/17/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9c43238d44b2309d105ef14e696a5a16848d0b58
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2019
ms.locfileid: "65896830"
---
# <a name="what-is-hybrid-identity-with-azure-active-directory"></a>Wat is hybride identiteit met Azure Active Directory?

Vandaag de dag maken organisaties en bedrijven steeds meer gebruik van een mix van on-premises toepassingen en cloudtoepassingen.  Gebruikers hebben zowel on-premises als in de cloud toegang tot deze toepassingen nodig. Beheren van gebruikers zowel on-premises en in de cloud poses lastig scenario's. 

De identiteitsoplossingen van Microsoft kunnen zowel on-premises als in de cloud worden gebruikt.  Met deze oplossingen wordt een algemene gebruikersidentiteit gemaakt voor verificatie en autorisatie voor alle resources, ongeacht de locatie. We noemen dit **hybride identiteit**.

Met hybride identiteiten met Azure AD en hybride identity management deze scenario's worden mogelijk.

Als u wilt bereiken voor hybride identiteit met Azure AD, kan een van drie verificatiemethoden worden gebruikt, afhankelijk van uw scenario's.   Deze methoden zijn: 

- **[Synchronisatie van wachtwoord-hashes (PHS)](whatis-phs.md)**  
- **[Pass-through-verificatie (PTA)](how-to-connect-pta.md)**  
- **[Federation (AD FS)](whatis-fed.md)** 

Met deze verificatiemethoden is tevens [eenmalige aanmelding](how-to-connect-sso.md) mogelijk.  Met eenmalige aanmelding worden gebruikers aangemeld als hun bedrijfsapparaten zijn verbonden met het bedrijfsnetwerk.

Zie [Selecteer de juiste verificatiemethode voor uw Azure Active Directory-oplossing voor hybride identiteit](https://docs.microsoft.com/azure/security/azure-ad-choose-authn) voor meer informatie. 

## <a name="common-scenarios-and-recommendations"></a>Veelvoorkomende scenario's en aanbevelingen 

Hieronder volgen enkele veelvoorkomende hybride identiteits- en beheerscenario's met aanbevelingen voor de meest geschikte hybride identiteitsoptie(s) voor elk scenario. 

|Ik wil graag:|PHS en eenmalige aanmelding<sup>1</sup>| PTA en eenmalige aanmelding<sup>2</sup> | AD FS<sup>3</sup>| 
|-----|-----|-----|-----| 
|nieuwe accounts van gebruikers, contactpersonen en groepen die in mijn on-premises Active Directory zijn gemaakt, automatisch met de cloud synchroniseren.|![Aanbevolen](./media/whatis-hybrid-identity/ic195031.png)| ![Aanbevolen](./media/whatis-hybrid-identity/ic195031.png) |![Aanbevolen](./media/whatis-hybrid-identity/ic195031.png)| 
|mijn tenant instellen voor hybride Office 365-scenario's.|![Aanbevolen](./media/whatis-hybrid-identity/ic195031.png)| ![Aanbevolen](./media/whatis-hybrid-identity/ic195031.png) |![Aanbevolen](./media/whatis-hybrid-identity/ic195031.png)| 
|mijn gebruikers in staat stellen zich aan te melden bij cloudservices met hun on-premises wachtwoord.|![Aanbevolen](./media/whatis-hybrid-identity/ic195031.png)| ![Aanbevolen](./media/whatis-hybrid-identity/ic195031.png) |![Aanbevolen](./media/whatis-hybrid-identity/ic195031.png)| 
|eenmalige aanmelding implementeren met bedrijfsreferenties.|![Aanbevolen](./media/whatis-hybrid-identity/ic195031.png)| ![Aanbevolen](./media/whatis-hybrid-identity/ic195031.png) |![Aanbevolen](./media/whatis-hybrid-identity/ic195031.png)|  
|ervoor zorgen dat er geen wachtwoord-hashes in de cloud worden opgeslagen.| |![Aanbevolen](./media/whatis-hybrid-identity/ic195031.png)|![Aanbevolen](./media/whatis-hybrid-identity/ic195031.png)| 
|Oplossingen voor meervoudige verificatie in de cloud mogelijk maken.|![Aanbevolen](./media/whatis-hybrid-identity/ic195031.png)|![Aanbevolen](./media/whatis-hybrid-identity/ic195031.png)|![Aanbevolen](./media/whatis-hybrid-identity/ic195031.png)| 
|oplossingen voor meervoudige verificatie on-premises mogelijk maken.| | |![Aanbevolen](./media/whatis-hybrid-identity/ic195031.png)| 
|verificatie van smartcards voor mijn gebruikers mogelijk maken.<sup>4</sup>| | |![Aanbevolen](./media/whatis-hybrid-identity/ic195031.png)| 
|meldingen voor verlopen wachtwoorden weergeven in de Office-portal en op het Windows 10-bureaublad.| | |![Aanbevolen](./media/whatis-hybrid-identity/ic195031.png)| 

> <sup>1</sup> Wachtwoordhashsynchronisatie met eenmalige aanmelding. 
> 
> <sup>2</sup> Pass-through-verificatie en eenmalige aanmelding.  
> 
> <sup>3</sup> Federatieve eenmalige aanmelding met AD FS.  
>  
> <sup>4</sup> AD FS kan worden geïntegreerd met uw Enterprise PKI om aanmelding met certificaten toe te staan. Deze certificaten kunnen voorlopige certificaten zijn die worden geïmplementeerd via vertrouwde inrichtingskanalen, zoals MDM-, GPO- of smartcardcertificaten (waaronder PIV/CAC-kaarten) of Hello voor Bedrijven (cert-trust). Zie [dit blog](https://blogs.msdn.microsoft.com/samueld/2016/07/19/adfs-certauth-aad-o365/) (Engelstalig) voor meer informatie over ondersteuning voor smartcardverificatie. 
> 

## <a name="license-requirements-for-using-azure-ad-connect"></a>Licentievereisten voor het gebruik van Azure AD Connect

[!INCLUDE [active-directory-free-license.md](../../../includes/active-directory-free-license.md)]

## <a name="next-steps"></a>Volgende stappen 

- [Wat zijn Azure AD Connect en Connect Health?](whatis-azure-ad-connect.md) 
- [Wat is synchronisatie van wachtwoord-hashes (PHS)?](whatis-phs.md) 
- [Wat is pass-through-verificatie (PTA)?](how-to-connect-pta.md) 
- [Wat is federatie?](whatis-fed.md) 
- [Wat is eenmalige aanmelding?](how-to-connect-sso.md) 

