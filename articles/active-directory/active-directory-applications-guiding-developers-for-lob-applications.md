---
title: Apps ontwikkelen voor Azure AD | Microsoft-Docs
description: In dit artikel bevat voor IT-professionals wordt geschreven, richtlijnen voor het Azure-toepassingen integreren met Active Directory.
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
editor: 
ms.assetid: dd69f2bc-37c5-457c-857d-27acb84267fb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2017
ms.author: kgremban
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 6b119be9c06d8c1ccc8e747168429e6c2d2e7a8f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="develop-line-of-business-apps-for-azure-active-directory"></a>Line-of-business-apps ontwikkelen voor Azure Active Directory
Deze handleiding biedt een overzicht van het ontwikkelen van line-of-business (LoB)-toepassingen voor Azure Active Directory (AD). De doelgroep is globale beheerders Active Directory/Office 365.

## <a name="overview"></a>Overzicht
Maken van toepassingen die zijn geïntegreerd met Azure AD geeft gebruikers in uw organisatie eenmalige aanmelding met Office 365. De toepassing is in Azure AD-hebt die u meer controle over het verificatiebeleid voor de toepassing. Voor meer informatie over voorwaardelijke toegang en het beveiligen van apps met multi-factor authentication (MFA) Zie [toegangsregels configureren](active-directory-conditional-access-azuread-connected-apps.md).

Uw toepassing te gebruiken van Azure Active Directory registreren. Registreren van de toepassing, betekent dat ontwikkelaars van uw Azure AD gebruiken kunnen voor het verifiëren van gebruikers en toegang vragen tot Gebruikersresources zoals e-mail, agenda en documenten.

Elk lid van uw directory (niet gasten) kunt u registreert een toepassing, ook bekend als *maken van een toepassingsobject*.

Registreren van een toepassing kan elke gebruiker het volgende doen:

* Een identiteit voor de toepassing die Azure AD herkent ophalen
* Een of meer geheimen/sleutels ophalen die de toepassing gebruiken kunt om zichzelf te verifiëren naar AD
* Merk de toepassing in de Azure-portal met een aangepaste naam, het logo, enzovoort.
* Azure AD-functies voor autorisatie van toepassing op hun app, met inbegrip van:

  * RBAC (op rollen gebaseerd toegangsbeheer)
  * Azure Active Directory als oAuth-autorisatie-server (een API die worden weergegeven door de toepassing beveiligen)
* Declareren vereiste machtigingen voor de toepassing naar de functie nodig, zoals verwacht, waaronder:

      - App-machtigingen (alleen globale beheerders). Bijvoorbeeld: lidmaatschap van de rol in een andere Azure AD-toepassing of functie lidmaatschap ten opzichte van een Azure Resource, resourcegroep, of -abonnement
      - Gedelegeerde machtigingen (een willekeurige gebruiker). Bijvoorbeeld: Azure AD, aanmelden en profiel lezen

> [!NOTE]
> Standaard kan een lid registreert een toepassing. Zie voor informatie over het beperken van machtigingen voor het registreren van toepassingen naar specifieke leden, [hoe toepassingen worden toegevoegd aan Azure AD](develop/active-directory-how-applications-are-added.md#who-has-permission-to-add-applications-to-my-azure-ad-instance).
>
>

Dit is wat u als globale beheerder moet doen om ervoor dat de toepassing gereed voor productie ontwikkelaars helpen bij het:

* Toegangsregels (toegang beleid/MFA) configureren
* De app configureren om te vereisen Gebruikerstoewijzing en gebruikers toewijzen
* De standaard toestemming gebruikerservaring onderdrukken

## <a name="configure-access-rules"></a>Toegangsregels configureren
Configureer regels voor toegang per toepassing naar uw SaaS-apps. U kunt bijvoorbeeld MFA vereisen of dat alleen toegang tot gebruikers in vertrouwde netwerken. De details voor deze zijn beschikbaar in het document [toegangsregels configureren](active-directory-conditional-access-azuread-connected-apps.md).

## <a name="configure-the-app-to-require-user-assignment-and-assign-users"></a>De app configureren om te vereisen Gebruikerstoewijzing en gebruikers toewijzen
Standaard kunnen gebruikers toepassingen openen zonder dat wordt toegewezen. Als de toepassing ook functies of als u wilt dat de toepassing worden weergegeven op het toegangsvenster van een gebruiker, moet u de Gebruikerstoewijzing van de instellen.

[Gebruikerstoewijzing vereisen](active-directory-applications-guiding-developers-requiring-user-assignment.md)

Als u een Azure AD Premium of Enterprise Mobility Suite (EMS)-abonnee bent, wordt aangeraden met behulp van groepen. Groepen toewijzen aan de toepassing, kunt u actieve toegangsbeheer naar de eigenaar van de groep te delegeren. U kunt de groep te maken of vraag de verantwoordelijke partij in uw organisatie te maken van de groep met behulp van de groep management-functie.

[Gebruikers toewijzen aan een toepassing](active-directory-applications-guiding-developers-assigning-users.md)  
[Groepen toewijzen aan een toepassing](active-directory-applications-guiding-developers-assigning-groups.md)

## <a name="suppress-user-consent"></a>Onderdrukken van toestemming van de gebruiker
Standaard is elke gebruiker doorloopt van een ervaring toestemming aan te melden. De ervaring van de toestemming, gebruikers vragen om te machtigen om een toepassing kan worden toegevoegd voor gebruikers die niet bekend bent met deze beslissingen zijn.

Voor toepassingen die u vertrouwt, kunt u de gebruikerservaring met ermee akkoord dat de toepassing namens uw organisatie te vereenvoudigen.

Zie voor meer informatie over de toestemming van de gebruiker en de toestemming in Azure optreden, [toepassingen integreren met Azure Active Directory](active-directory-integrating-applications.md).

## <a name="related-articles"></a>Verwante artikelen
* [Veilige externe toegang tot on-premises toepassingen met Azure AD-toepassingsproxy inschakelen](active-directory-application-proxy-get-started.md)
* [Voorwaardelijke toegang tot Azure Preview voor SaaS-Apps](active-directory-conditional-access-azuread-connected-apps.md)
* [Het beheren van toegang tot apps met Azure AD](active-directory-managing-access-to-apps.md)
* [Article Index for Application Management in Azure Active Directory](active-directory-apps-index.md) (Artikelindex voor toepassingsbeheer in Azure Active Directory)
