---
title: Azure AD integreren met apps aan de slag | Microsoft Docs
description: Dit artikel is een aan de slag-handleiding voor het integreren van Azure Active Directory (AD) met on-premises toepassingen en cloudtoepassingen.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: app-mgmt
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/16/2018
ms.author: barbkess
ms.reviewer: asteen
ms.openlocfilehash: 97404421a48d15339fdecf23a951a64d975da577
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/17/2018
ms.locfileid: "39090335"
---
# <a name="integrating-azure-active-directory-with-applications-getting-started-guide"></a>Azure Active Directory integreren in toepassingen aan de slag
## <a name="overview"></a>Overzicht
In dit onderwerp is bedoeld om u te bieden een schema voor het integreren van toepassingen met Azure Active Directory (AD). Elk van de volgende secties bevatten een korte samenvatting van een meer gedetailleerde onderwerp, zodat u kunt identificeren welke onderdelen van deze introductiehandleiding relevant zijn voor u.  Volg de koppelingen voor meer informatie over op elk onderwerp.

## <a name="before-you-begin-take-inventory"></a>Voordat u begint, inventariseren
Voordat u gaan met toepassingen integreren met Azure AD, is het belangrijk te weten waar u zich bevindt en waar u naartoe wilt gaan.  De volgende vragen zijn bedoeld om te denken over uw Azure AD application integration-project.

### <a name="application-inventory"></a>Toepassingsinventaris
* Waar bevinden zich al uw toepassingen? Wie is eigenaar van deze?
* Wat voor soort verificatie moet uw toepassingen?
* Wie heeft toegang nodig tot welke toepassingen?
* Wilt u om een nieuwe toepassing te implementeren?
  * Wordt u interne bouwen en implementeren op een Azure compute-instantie?
  * Wilt u gebruiken die beschikbaar is in de galerie met Azure?

### <a name="user-and-group-inventory"></a>Inventarisatie van gebruikers en groepen
* Waar zich de gebruikersaccounts?
  * On-premises Active Directory
  * Azure AD
  * In de database van een afzonderlijke toepassing waarvan u eigenaar bent
  * In niet-goedgekeurde toepassingen
  * Al het bovenstaande
* Welke machtigingen en roltoewijzingen hebt afzonderlijke gebruikers dan momenteel? Heb ik nodig voor hun toegang beoordelen of weet u zeker dat uw gebruikers toegang en de rol toewijzingen nu geschikt zijn?
* Groepen reeds bestaan in uw on-premises Active Directory?
  * Hoe zijn de groepen ingedeeld?
  * Wie zijn leden van de groep?
  * Welke machtigingen/roltoewijzingen de groepen die op dit moment hebben?
* Moet u voor het opschonen van de gebruiker of groep databases voordat de integratie van?  (Dit is een heel belangrijk vraag. Garbagecollection in garbagecollection uit.)

### <a name="access-management-inventory"></a>Toegang beheren-inventaris
* Hoe kan u toegang tot toepassingen op dit moment beheren? Die moet worden gewijzigd?  Hebt u andere manieren om toegang te beheren, zoals met beschouwd als [RBAC](../../role-based-access-control/role-assignments-portal.md) bijvoorbeeld?
* Wie heeft toegang tot wat nodig?

Misschien niet hebt u de antwoorden voor al deze vragen een maar dat is geen probleem.  Deze handleiding kunt u sommige van deze vragen te beantwoorden en sommige gefundeerde beslissingen te nemen.

## <a name="prerequisites"></a>Vereisten
* Een Azure-abonnement en een Azure Active Directory-map.  Als u nog een Azure-abonnement hebt, kunt u Azure voor 30 dagen gratis uitproberen. [Probeer het nu!](https://azure.microsoft.com/trial/get-started-active-directory/)

## <a name="application-integration-with-azure-ad"></a>Integratie van toepassingen met Azure AD
### <a name="finding-unsanctioned-cloud-applications-with-cloud-discovery"></a>Cloud-Apps zoeken die niet goedgekeurd met Cloud Discovery
Zoals eerder vermeld, is er mogelijk toepassingen die nog niet is beheerd door uw organisatie tot op heden.  Als onderdeel van het voorraadproces is het mogelijk om niet-goedgekeurde cloud-apps te vinden. Zie [Cloud Discovery instellen](/cloud-app-security/set-up-cloud-discovery).

### <a name="authentication-types"></a>Verificatietypen
Elk van uw toepassingen mogelijk verschillende vereisten. Met Azure AD kunt ondertekenen van certificaten worden gebruikt met toepassingen die gebruikmaken van SAML 2.0, WS-Federation, of OpenID Connect-protocollen, evenals wachtwoord Single Sign On. Zie voor meer informatie over de toepassing verificatietypen voor gebruik met Azure AD [beheren van certificaten voor federatieve eenmalige aanmelding in Azure Active Directory](manage-certificates-for-federated-single-sign-on.md) en [wachtwoord eenmalige aanmelding op basis van](what-is-single-sign-on.md).

### <a name="enabling-sso-with-azure-ad-app-proxy"></a>Eenmalige aanmelding met Azure AD-toepassingsproxy inschakelen
Met Microsoft Azure AD-toepassingsproxy, kunt u toegang tot toepassingen die zich bevinden binnen uw particuliere netwerk veilig vanaf elke locatie en op elk apparaat opgeven. Nadat u een connector voor toepassingsproxy hebt geïnstalleerd in uw omgeving, kan deze eenvoudig worden geconfigureerd met Azure AD.

### <a name="integrating-applications-with-azure-ad"></a>Toepassingen integreren met Azure AD
De volgende artikelen worden de verschillende manieren toepassingen integreren met Azure AD, en enige houvast bieden besproken.

* [Bepalen welke Active Directory gebruiken](../fundamentals/active-directory-administer.md)
* [Met behulp van toepassingen in de Azure AD-toepassingsgalerie](what-is-single-sign-on.md)
* [Lijst met zelfstudies van SaaS-toepassingen integreren](../saas-apps/tutorial-list.md)

## <a name="managing-access-to-applications"></a>Beheer van toegang tot toepassingen
De volgende artikelen beschreven manieren kunt u toegang tot toepassingen beheren nadat ze zijn geïntegreerd met Azure AD met behulp van Azure AD-Connectors en Azure AD.

* [Beheer van toegang tot apps met behulp van Azure AD](what-is-access-management.md)
* [Automatiseren met Azure AD-Connectors](../active-directory-saas-app-provisioning.md)
* [Gebruikers toewijzen aan een toepassing](../active-directory-applications-guiding-developers-assigning-users.md)
* [Groepen toewijzen aan een toepassing](../active-directory-applications-guiding-developers-assigning-groups.md)
* [Accounts delen](../active-directory-sharing-accounts.md)

## <a name="integrating-custom-applications"></a>Aangepaste toepassingen integreren
Als u een nieuwe toepassing en wilt ontwikkelaars helpen bij het gebruik te maken van de kracht van Azure AD, Zie [Guiding ontwikkelaars](../active-directory-applications-guiding-developers-for-lob-applications.md).

Als u uw aangepaste toepassing toevoegen aan de galerie met Azure wilt, Zie ['Bring your own app' met Azure AD Self-Service SAML-configuratie](https://cloudblogs.microsoft.com/enterprisemobility/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-now-in-preview/).

## <a name="see-also"></a>Zie ook
* [Article Index for Application Management in Azure Active Directory](../active-directory-apps-index.md) (Artikelindex voor toepassingsbeheer in Azure Active Directory)

