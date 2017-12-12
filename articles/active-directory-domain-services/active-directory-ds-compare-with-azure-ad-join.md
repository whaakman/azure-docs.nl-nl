---
title: Koppelen aan Azure AD en Azure Active Directory Domain Services vergelijken | Microsoft Docs
description: Kiezen tussen Azure AD Join en Azure AD Domain Services
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: 31a71d36-58c1-4839-b958-80da0c6a77eb
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/26/2017
ms.author: maheshu
ms.openlocfilehash: fb28c7a66d26119444dd720c2e134f2818d46cfc
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="choose-between-azure-active-directory-join-and-azure-active-directory-domain-services"></a>Kiezen tussen Azure Active Directory join en Azure Active Directory Domain Services
Dit artikel worden de verschillen tussen koppelen aan Azure Active Directory (AD) en Azure AD Domain Services en zorgt die u ervoor kiest, op basis van uw use cases.

## <a name="azure-ad-registered-and-azure-ad-joined-devices"></a>Azure AD geregistreerd en Azure AD die lid zijn van apparaten
Azure AD kunt u de identiteit van apparaten die worden gebruikt door uw organisatie en beheren van toegang tot bedrijfsbronnen vanaf deze apparaten te beheren. Gebruikers kunnen hun privé (bring your own)-apparaat registreren met Azure AD, waardoor het apparaat met een identiteit. Azure AD kan het apparaat vervolgens verifiëren wanneer een gebruiker zich bij Azure AD aanmeldt en wordt het apparaat wordt gebruikt voor toegang tot beveiligde bronnen. Bovendien kunt u het apparaat met behulp van Mobile Device Management (MDM)-software, zoals Microsoft Intune beheren. Deze functie kunt u toegang te beperken tot gevoelige resources van beheerde apparaten die voldoen aan het beleid.

U kunt ook deelnemen aan de organisatie die eigendom zijn van apparaten naar Azure AD. Dit mechanisme biedt dezelfde voordelen van een persoonlijk apparaat registreren bij Azure AD. Bovendien kunnen gebruikers zich aanmelden op het apparaat met hun bedrijfsreferenties. Azure AD die lid zijn van apparaten bieden u de volgende voordelen:
* Eenmalige aanmelding (SSO) aan toepassingen die zijn beveiligd door Azure AD
* Enterprise voldoen aan het beleid zwervende gebruikersinstellingen verschillende apparaten.
* Toegang tot de Windows Store voor bedrijven met uw bedrijfsreferenties.
* Windows Hello voor bedrijven
* Beperkte toegang tot apps en resources van apparaten die voldoen aan het bedrijfsbeleid.

| **Type apparaat** | **Apparaatplatforms** | **Mechanisme** |
|:---| --- | --- |
| Persoonlijke apparaten | Windows 10-, iOS, Android-, Mac OS | Azure AD geregistreerd |
| Organisatie die geen lid zijn van apparaten in Bedrijfseigendom on-premises AD dat | Windows 10 | Azure AD die lid zijn van |
| Organisatie die eigendom zijn van het apparaat lid is van een on-premises AD | Windows 10 | Hybride die lid zijn van Azure AD |

Lid op een Azure AD of geregistreerd apparaat, gebruikersverificatie gebeurt met behulp van protocollen voor moderne OAuth/OpenID Connect gebaseerd. Deze protocollen zijn ontworpen om te werken via internet en zijn ideaal voor mobiele scenario's waar gebruikers toegang krijgen bedrijfsbronnen vanaf elke locatie tot.


## <a name="domain-join-to-azure-ad-domain-services-managed-domains"></a>Aan domein toevoegen aan de beheerde domeinen Azure AD Domain Services
Azure AD Domain Services biedt een beheerde AD-domein in een Azure-netwerk. U kunt computers toevoegen aan deze beheerd domein met traditionele domein mechanismen. Windows-client (Windows 7, Windows 10) en Windows Server-machines kunnen worden gekoppeld aan het beheerde domein. U kunt daarnaast ook machines met Linux en Mac OS toevoegen aan het beheerde domein. Wanneer u een computer aan een AD-domein toevoegen, kunnen gebruikers zich kunnen aanmelden bij de computer met hun bedrijfsreferenties. Deze machines kunnen worden beheerd met behulp van Groepsbeleid, waardoor het afdwingen van compatibiliteit met het beveiligingsbeleid van uw organisatie.

Op een machine domein gebruikersverificatie gebeurt met behulp van NTLM of Kerberos-verificatieprotocollen. De domein-machine moet onbelemmerd zicht naar de domeincontrollers van het beheerde domein in de volgorde voor gebruikersverificatie om te werken. Daarom kan het domein machine moet zich in hetzelfde virtuele netwerk als het beheerde domein. U kunt ook domein machine moet worden verbonden met het beheerde domein via een peered virtueel netwerk of via een site-naar-site VPN-en ExpressRoute-verbinding. Dit mechanisme is dus niet een geweldige passend maken voor apparaten die zijn Mobiel of verbinding maken met bronnen van buiten het bedrijfsnetwerk.

> [!NOTE]
> Het is technisch gezien is mogelijk een lokale clientwerkstation toevoegen aan het beheerde domein via een site-naar-site VPN- of ExpressRoute-verbinding. Echter voor apparaten van eindgebruikers die wordt aangeraden u ofwel Registreer het apparaat met Azure AD (persoonlijke apparaten) of het apparaat toevoegen aan Azure AD (zakelijke apparaten). Dit mechanisme beter werkt via het internet en kan eindgebruikers werken vanaf elke locatie. Azure AD Domain Services is ideaal voor Windows of Linux-Server virtuele machines die zijn geïmplementeerd in uw virtuele netwerken in Azure, waarop uw toepassingen zijn geïmplementeerd.


## <a name="summary---key-differences"></a>Samenvatting - belangrijke verschillen
| **Aspect** | **Azure AD Join** | **Azure AD Domain Services** |
|:---| --- | --- |
| Apparaat beheerd door | Azure AD | Azure AD Domain Services beheerd domein |
| Weergave in de map | Apparaatobjecten in de Azure AD-directory. | Computerobjecten in het AAD-DS beheerde domein. |
| Authentication | OAuth/OpenID Connect gebaseerde protocollen | Kerberos, NTLM-protocollen |
| Beheer | Mobile Device Management (MDM)-software, zoals Intune | Groepsbeleid |
| Netwerken | Werkt via het internet | Vereist machines zich in hetzelfde virtuele netwerk als het beheerde domein.|
| Ideaal voor... | Eindgebruikers mobiele of bureaubladtoepassingen apparaten | Server virtuele machines zijn geïmplementeerd in Azure |


## <a name="next-steps"></a>Volgende stappen
### <a name="learn-more-about-azure-ad-domain-services"></a>Meer informatie over Azure AD Domain Services
* [Overzicht van Azure AD Domain Services](active-directory-ds-overview.md)
* [Functies](active-directory-ds-features.md)
* [Implementatiescenario 's](active-directory-ds-scenarios.md)
* [Als Azure AD Domain Services aansluit op uw use cases weten](active-directory-ds-comparison.md)
* [Begrijpen hoe Azure AD Domain Services wordt gesynchroniseerd met uw Azure AD-directory](active-directory-ds-synchronization.md)

### <a name="learn-more-about-azure-ad-join"></a>Meer informatie over Azure AD Join
* [Inleiding tot beheer van apparaten in Azure Active Directory](../active-directory/device-management-introduction.md)

### <a name="get-started-with-azure-ad-domain-services"></a>Aan de slag met Azure AD Domain Services
* [Azure AD Domain Services met Azure portal inschakelen](active-directory-ds-getting-started.md)
