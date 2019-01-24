---
title: Azure AD Join en Azure Active Directory Domain Services vergelijken | Microsoft Docs
description: Kiezen tussen Azure AD Join en Azure AD Domain Services
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: daveba
editor: curtand
ms.assetid: 31a71d36-58c1-4839-b958-80da0c6a77eb
ms.service: active-directory
ms.component: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/26/2017
ms.author: ergreenl
ms.openlocfilehash: 8db95f0bcc715967bf942827c140312572b03095
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54853966"
---
# <a name="choose-between-azure-active-directory-join-and-azure-active-directory-domain-services"></a>Kiezen tussen Azure Active Directory join en Azure Active Directory Domain Services
Dit artikel wordt beschreven verschillen tussen koppelen aan Azure Active Directory (AD) en Azure AD Domain Services en zorgt die u ervoor kiest, op basis van uw use cases.

## <a name="azure-ad-registered-and-azure-ad-joined-devices"></a>Azure AD geregistreerd en Azure AD gekoppelde apparaten
Azure AD kunt u de identiteit van apparaten die worden gebruikt door uw organisatie en beheren van toegang tot resources van deze apparaten beheren. Gebruikers kunnen hun privé (bring-your-own)-apparaat registreren bij Azure AD, waarmee het apparaat een identiteit. Azure AD kan vervolgens het apparaat verifiëren als een gebruiker zich aanmeldt bij Azure AD en wordt het apparaat wordt gebruikt voor toegang tot beveiligde bronnen. Bovendien kunt u het apparaat met behulp van Mobile Device Management (MDM)-software, zoals Microsoft Intune beheren. Deze functie kunt u beperken van toegang tot gevoelige resources van beheerde en beleid-compatibele apparaten.

U kunt ook lid van apparaten naar Azure AD-organisatie. Dit mechanisme biedt de voordelen van een persoonlijk apparaat registreren bij Azure AD. Bovendien kunnen gebruikers zich aanmelden op het apparaat met hun bedrijfsreferenties. Azure AD gekoppelde apparaten bieden u de volgende voordelen:
* Eenmalige aanmelding (SSO) voor toepassingen die zijn beveiligd door Azure AD
* Zakelijke voldoen aan het beleid roaming van instellingen voor gebruikers op apparaten.
* Toegang tot de Windows Store voor bedrijven met uw bedrijfsreferenties.
* Windows Hello voor Bedrijven
* Beperkte toegang tot apps en resources van apparaten die compatibel zijn met het bedrijfsbeleid.

| **Type apparaat** | **Apparaatplatformen** | **Mechanisme** |
|:---| --- | --- |
| Persoonlijke apparaten | Windows 10, iOS, Android, Mac OS | Geregistreerd bij Azure AD |
| Organisatie niet is toegevoegd aan apparaten in Bedrijfseigendom on-premises AD | Windows 10 | Azure AD-domein |
| Organisatie eigendom van apparaten die zijn gekoppeld aan een on-premises AD | Windows 10 | Hybrid Azure AD join |

Op een Azure AD gekoppeld of geregistreerd apparaat, gebruikersverificatie gebeurt met behulp van moderne OAuth/OpenID verbinding maken op basis van protocollen. Deze protocollen zijn ontworpen om te werken via internet en is ideaal voor mobiele scenario's waar gebruikers toegang bedrijfsbronnen vanaf elke locatie tot.


## <a name="domain-join-to-azure-ad-domain-services-managed-domains"></a>Aan domein toevoegen aan Azure AD Domain Services beheerde domeinen
Azure AD Domain Services biedt een beheerde AD-domein in een Azure-netwerk. U kunt computers toevoegen aan dit beheerde domein met behulp van traditionele domain-join-mechanismen. Windows-client (Windows 7, Windows 10) en Windows Server-machines kunnen worden toegevoegd aan het beheerde domein. Bovendien kunt u ook Mac OS en Linux-machines toevoegen aan het beheerde domein. Wanneer u een virtuele machine aan een AD-domein, kunnen gebruikers zich kunnen aanmelden bij de virtuele machine met hun bedrijfsreferenties. Deze machines kunnen worden beheerd met behulp van Groepsbeleid, dus het afdwingen van compatibiliteit met het beveiligingsbeleid van uw organisatie.

Op een machine domein gebruikersverificatie gebeurt met behulp van NTLM of Kerberos-verificatie-protocollen. De domein-machine moet peeren naar de domeincontrollers van het beheerde domein in de volgorde voor verificatie van de gebruiker om te werken. Daarom kan het domein machine moet zich in hetzelfde virtuele netwerk bevinden als het beheerde domein. U kunt ook domein machine moet worden verbonden met het beheerde domein via een gekoppeld virtueel netwerk of via een site-naar-site VPN/ExpressRoute-verbinding. Dit mechanisme wordt dus niet uitstek geschikt voor apparaten die zijn Mobiel of verbinding maken met bronnen van buiten het bedrijfsnetwerk.

> [!NOTE]
> Het is technisch gezien mogelijk op een werkstation met on-premises client toevoegen aan het beheerde domein via een site-naar-site VPN of ExpressRoute-verbinding. Echter, voor apparaten van eindgebruikers die wordt aangeraden u het apparaat registreren bij Azure AD (persoonlijke apparaten) of het apparaat toevoegen aan Azure AD (apparaten). Dit mechanisme beter werkt via internet en kan eindgebruikers werken vanaf elke locatie. Azure AD Domain Services is erg handig voor Windows of Linux-Server virtuele machines die zijn geïmplementeerd in uw Azure virtual networks, waarop uw toepassingen zijn geïmplementeerd.


## <a name="summary---key-differences"></a>Overzicht - belangrijke verschillen
| **Aspect** | **Azure AD Join** | **Azure AD Domain Services** |
|:---| --- | --- |
| Apparaat wordt beheerd door | Azure AD | Azure AD Domain Services beheerde domein |
| Weergave in de map | Apparaatobjecten in de Azure AD-directory. | Computerobjecten in de beheerde AAD-DS-domein. |
| Verificatie | Protocollen op basis van OAuth/OpenID Connect | Kerberos, NTLM protocols |
| Beheer | Mobile Device Management (MDM)-software, zoals Intune | Groepsbeleid |
| Netwerken | Werkt via internet | Machines zich in hetzelfde virtuele netwerk bevinden als het beheerde domein vereist.|
| Ideaal voor... | Mobiel of desktop-apparaten van eindgebruikers | Server virtuele machines geïmplementeerd in Azure |


## <a name="next-steps"></a>Volgende stappen
### <a name="learn-more-about-azure-ad-domain-services"></a>Meer informatie over Azure AD Domain Services
* [Overzicht van Azure AD Domain Services](active-directory-ds-overview.md)
* [Functies](active-directory-ds-features.md)
* [Implementatiescenario 's](active-directory-ds-scenarios.md)
* [Ontdek als Azure AD Domain Services aansluit bij uw use cases](active-directory-ds-comparison.md)
* [Begrijpen hoe Azure AD Domain Services worden gesynchroniseerd met uw Azure AD-directory](active-directory-ds-synchronization.md)

### <a name="learn-more-about-azure-ad-join"></a>Meer informatie over Azure AD Join
* [Inleiding tot apparaatbeheer in Azure Active Directory](../active-directory/device-management-introduction.md)

### <a name="get-started-with-azure-ad-domain-services"></a>Aan de slag met Azure AD Domain Services
* [Azure AD Domain Services met behulp van de Azure-portal inschakelen](active-directory-ds-getting-started.md)
