---
title: Azure AD integreren met apps aan de slag | Microsoft Docs
description: Dit artikel is een introductiehandleiding voor het integreren van Azure Active Directory (AD) met on-premises toepassingen en cloud-toepassingen.
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: db6d210d-c970-49e9-bd20-36d984bcd1c3
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/31/2017
ms.author: markvi
ms.reviewer: asteen
ms.openlocfilehash: e273d27bacf6978c5056c0ab09846c26426dd12b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="integrating-azure-active-directory-with-applications-getting-started-guide"></a>Azure Active Directory integreren met toepassingen aan de slag
## <a name="overview"></a>Overzicht
In dit onderwerp is bedoeld om u te bieden een roadmap voor toepassingen integreren met Azure Active Directory (AD). Elk van de volgende secties bevatten een korte samenvatting van een meer gedetailleerde onderwerp zodat u kunt identificeren welke onderdelen van deze introductiehandleiding relevant zijn voor u.  Volg de koppelingen voor meer informatie over elk onderwerp.

## <a name="before-you-begin-take-inventory"></a>Voordat u begint, inventariseren
Voordat u naar toepassingen integreren met Azure AD in gaan, is het belangrijk te weten waar u zich bevindt en waar u naartoe wilt gaan.  De volgende vragen zijn bedoeld voor hulp bij het nadenken over uw Azure AD-integratie toepassingsproject.

### <a name="application-inventory"></a>Toepassingsinventaris
* Waar worden al uw toepassingen? Eigenaar van deze?
* Wat voor soort verificatie vereisen uw toepassingen?
* Wie heeft toegang tot welke toepassingen?
* Wilt u een nieuwe toepassing implementeren?
  * U bouwt het intern en deze implementeren op een exemplaar van Azure compute?
  * Wilt u gebruiken die beschikbaar is in de galerie van Azure-toepassing?

### <a name="user-and-group-inventory"></a>Inventarisatie van gebruikers en groepen
* Waar de gebruikersaccounts zich bevinden?
  * On-premises Active Directory
  * Azure AD
  * In de database van een afzonderlijke toepassing waarvan u eigenaar
  * In niet-toegestane toepassingen
  * Alle bovenstaande
* Welke machtigingen en roltoewijzingen individuele gebruikers hebt? Wilt u de toegang controleren of weet u zeker dat de toewijzingen van uw gebruikers toegang en de rol geschikt nu zijn?
* Groepen reeds bestaan in uw lokale Active Directory?
  * Hoe kan ik de groepen ingedeeld?
  * Wie zijn leden van de groep?
  * Welke machtigingen/roltoewijzingen de groepen momenteel hebt?
* Wilt u de databases van de gebruiker of groep opschonen vóór de integratie van?  (Dit is een vraag heel belangrijk. Garbagecollection in garbagecollection uit.)

### <a name="access-management-inventory"></a>Access management-inventarisatie
* Hoe beheert u momenteel gebruikerstoegang tot toepassingen? Die moet worden gewijzigd?  Hebt u andere manieren om toegang te beheren, zoals met beschouwd als [RBAC](role-based-access-control-configure.md) bijvoorbeeld?
* Wie heeft toegang tot wat?

Misschien niet hebt u al deze vragen worden beantwoord vooraf, maar dat is geen probleem.  Deze handleiding kunt u enkele van deze vragen te beantwoorden en sommige gefundeerde beslissingen te nemen.

## <a name="prerequisites"></a>Vereisten
* Een Azure-abonnement en een Azure Active Directory-directory.  Als u nog geen Azure-abonnement hebt, kunt u Azure voor 30 dagen gratis uitproberen. [Probeer het nu!](https://azure.microsoft.com/trial/get-started-active-directory/)

## <a name="application-integration-with-azure-ad"></a>Integratie van toepassingen met Azure AD
### <a name="finding-unsanctioned-cloud-applications-with-cloud-app-discovery"></a>Zoeken naar niet-toegestane cloud-toepassingen met Cloud App Discovery
Zoals eerder vermeld, worden toepassingen die nog niet is beheerd door uw organisatie tot op heden.  Als onderdeel van de procedure inventarisatie is het mogelijk om te zoeken naar niet-toegestane cloud-toepassingen. Zie [zoeken naar niet-toegestane cloud-toepassingen met Cloud App Discovery](active-directory-cloudappdiscovery-whatis.md).

### <a name="authentication-types"></a>Verificatietypen
Elk van uw toepassingen mogelijk andere verificatievereisten. Met Azure AD kan ondertekenen van certificaten worden gebruikt met toepassingen die gebruikmaken van SAML 2.0, WS-Federation, of OpenID Connect protocollen, evenals wachtwoord eenmalige aanmelding. Zie voor meer informatie over toepassing verificatietypen voor gebruik met Azure AD [certificaten beheren voor federatieve eenmalige aanmelding bij Azure Active Directory](active-directory-sso-certs.md) en [eenmalige aanmelding op op basis van wachtwoorden](active-directory-appssoaccess-whatis.md).

### <a name="enabling-sso-with-azure-ad-app-proxy"></a>Eenmalige aanmelding met Azure AD-toepassingsproxy inschakelen
Met Microsoft Azure AD-toepassingsproxy, kunt u toegang tot toepassingen die zich bevinden binnen uw particuliere netwerk veilig, overal en op elk apparaat voorzien. Nadat u een connector voor toepassingsproxy hebt geïnstalleerd in uw omgeving, kan het eenvoudig met Azure AD geconfigureerd.

### <a name="integrating-applications-with-azure-ad"></a>Toepassingen integreren met Azure AD
De volgende artikelen worden de verschillende manieren toepassingen integreren met Azure AD en sommige advies besproken.

* [Bepalen welke Active Directory gebruiken](active-directory-administer.md)
* [Gebruik van toepassingen in de galerie van Azure-toepassing](active-directory-appssoaccess-whatis.md)
* [Integratie van zelfstudies lijst met SaaS-toepassingen](active-directory-saas-tutorial-list.md)

## <a name="managing-access-to-applications"></a>Het beheren van toegang tot toepassingen
De volgende artikelen beschrijven manieren kunt u toegang tot toepassingen beheren wanneer ze zijn geïntegreerd met Azure AD dat gebruikmaakt van Azure AD-Connectors en Azure AD.

* [Het beheren van toegang tot apps die gebruikmaken van Azure AD](active-directory-managing-access-to-apps.md)
* [Automatiseren met Azure AD-Connectors](active-directory-saas-app-provisioning.md)
* [Gebruikers toewijzen aan een toepassing](active-directory-applications-guiding-developers-assigning-users.md)
* [Groepen toewijzen aan een toepassing](active-directory-applications-guiding-developers-assigning-groups.md)
* [Accounts delen](active-directory-sharing-accounts.md)

## <a name="integrating-custom-applications"></a>Integratie van aangepaste toepassingen
Als u een nieuwe toepassing en wilt ontwikkelaars helpen bij het gebruik van de mogelijkheden van Azure AD, Zie schrijft [Guiding ontwikkelaars](active-directory-applications-guiding-developers-for-lob-applications.md).

Als u wilt uw aangepaste toepassing toevoegen aan de galerie van Azure-toepassing, Zie ['Bring your own app' met Azure AD Self-Service SAML-configuratie](http://blogs.technet.com/b/ad/archive/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-gt-now-in-preview.aspx).

## <a name="see-also"></a>Zie ook
* [Article Index for Application Management in Azure Active Directory](active-directory-apps-index.md) (Artikelindex voor toepassingsbeheer in Azure Active Directory)

