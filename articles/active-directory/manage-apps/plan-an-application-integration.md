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
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/16/2018
ms.author: barbkess
ms.reviewer: asteen
ms.openlocfilehash: d52ec316f9f5540d4d0d0fe0bc4e4bf778e1daf7
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/11/2018
ms.locfileid: "44345438"
---
# <a name="integrating-azure-active-directory-with-applications-getting-started-guide"></a>Azure Active Directory integreren in toepassingen aan de slag

In dit onderwerp bevat een overzicht van het proces voor het integreren van toepassingen met Azure Active Directory (AD). Elk van de volgende secties bevatten een korte samenvatting van een meer gedetailleerde onderwerp, zodat u kunt identificeren welke onderdelen van deze introductiehandleiding relevant zijn voor u.

Zie voor het downloaden van de uitgebreide implementatieplannen [Vervolgstappen](#next-steps).

## <a name="take-inventory"></a>Inventarisatie
Voordat u toepassingen integreren met Azure AD, is het belangrijk te weten waar u zich bevindt en waar u naartoe wilt gaan.  De volgende vragen zijn bedoeld om te denken over uw Azure AD application integration-project.

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

### <a name="find-unsanctioned-cloud-applications-with-cloud-discovery"></a>Zoeken naar niet-goedgekeurde cloud-toepassingen met Cloud Discovery

Zoals eerder vermeld, is er mogelijk toepassingen die nog niet is beheerd door uw organisatie tot op heden.  Als onderdeel van het voorraadproces is het mogelijk om niet-goedgekeurde cloud-apps te vinden. Zie [Cloud Discovery instellen](/cloud-app-security/set-up-cloud-discovery).

## <a name="integrating-applications-with-azure-ad"></a>Toepassingen integreren met Azure AD
De volgende artikelen worden de verschillende manieren toepassingen integreren met Azure AD, en enige houvast bieden besproken.

* [Bepalen welke Active Directory gebruiken](../fundamentals/active-directory-administer.md)
* [Met behulp van toepassingen in de Azure AD-toepassingsgalerie](what-is-single-sign-on.md)
* [Lijst met zelfstudies van SaaS-toepassingen integreren](../active-directory-saas-tutorial-list.md)

### <a name="authentication-types"></a>Verificatietypen
Elk van uw toepassingen mogelijk verschillende vereisten. Met Azure AD kunt ondertekenen van certificaten worden gebruikt met toepassingen die gebruikmaken van SAML 2.0, WS-Federation, of OpenID Connect-protocollen, evenals wachtwoord Single Sign On. Zie voor meer informatie over de toepassing verificatietypen voor gebruik met Azure AD [beheren van certificaten voor federatieve eenmalige aanmelding in Azure Active Directory](manage-certificates-for-federated-single-sign-on.md) en [wachtwoord eenmalige aanmelding op basis van](what-is-single-sign-on.md).

### <a name="enabling-sso-with-azure-ad-app-proxy"></a>Eenmalige aanmelding met Azure AD-toepassingsproxy inschakelen
Met Microsoft Azure AD-toepassingsproxy, kunt u toegang tot toepassingen die zich bevinden binnen uw particuliere netwerk veilig vanaf elke locatie en op elk apparaat opgeven. Nadat u een connector voor toepassingsproxy hebt geïnstalleerd in uw omgeving, kan deze eenvoudig worden geconfigureerd met Azure AD.

### <a name="integrating-custom-applications"></a>Aangepaste toepassingen integreren
Als u een nieuwe toepassing en wilt ontwikkelaars helpen bij het gebruik te maken van de kracht van Azure AD, Zie [Guiding ontwikkelaars](../active-directory-applications-guiding-developers-for-lob-applications.md).

Als u uw aangepaste toepassing toevoegen aan de galerie met Azure wilt, Zie ['Bring your own app' met Azure AD Self-Service SAML-configuratie](https://cloudblogs.microsoft.com/enterprisemobility/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-now-in-preview/).

## <a name="managing-access-to-applications"></a>Beheer van toegang tot toepassingen
De volgende artikelen beschreven manieren kunt u toegang tot toepassingen beheren nadat ze zijn geïntegreerd met Azure AD met behulp van Azure AD-Connectors en Azure AD.

* [Beheer van toegang tot apps met behulp van Azure AD](what-is-access-management.md)
* [Automatiseren met Azure AD-Connectors](user-provisioning.md)
* [Gebruikers toewijzen aan een toepassing](../active-directory-applications-guiding-developers-assigning-users.md)
* [Groepen toewijzen aan een toepassing](../active-directory-applications-guiding-developers-assigning-groups.md)
* [Accounts delen](../active-directory-sharing-accounts.md)

## <a name="next-steps"></a>Volgende stappen
Voor gedetailleerde informatie, kunt u Azure Active Directory-implementatieplannen van downloaden [GitHub](https://aka.ms/deploymentplans). Voor galerietoepassingen, kunt u downloaden van implementatieplannen voor eenmalige aanmelding voor voorwaardelijke toegang en gebruikers inrichten via de [Azure-portal](https://portal.azure.com). 

Een implementatieplan downloaden vanuit de Azure-portal:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer **bedrijfstoepassingen** | **kiezen van een App** | **implementatieplan**.

Geef feedback over implementatieplannen voor door de [implementatie plan enquête](https://aka.ms/DeploymentPlanFeedback).
