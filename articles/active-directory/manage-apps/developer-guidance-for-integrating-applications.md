---
title: Registreren van uw toepassing voor het gebruik van Azure Active Directory | Microsoft Docs
description: In dit artikel bevat richtlijnen voor het Azure-toepassingen integreren met Active Directory die zijn geschreven voor IT-professionals.
services: active-directory
documentationcenter: ''
author: barbkess
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: article
ms.date: 10/30/2018
ms.author: barbkess
ms.custom: seohack1
ms.openlocfilehash: d307b0f5f855b48d13b3c302a62f154b9df7aa48
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55155082"
---
# <a name="develop-line-of-business-apps-for-azure-active-directory"></a>Line-of-business-apps ontwikkelen voor Azure Active Directory
Deze handleiding bevat een overzicht van het ontwikkelen van line-of-business (LoB)-toepassingen voor Azure Active Directory (AD). De doelgroep is globale beheerders van Active Directory/Office 365.

## <a name="overview"></a>Overzicht
Het bouwen van toepassingen die zijn geïntegreerd met Azure AD biedt gebruikers in uw organisatie eenmalige aanmelding met Office 365. De toepassing die in Azure AD biedt die u meer controle over het verificatiebeleid voor de toepassing. Voor meer informatie over voorwaardelijke toegang en hoe u apps beveiligen met multi-factor authentication (MFA) Zie [toegangsregels configureren](../conditional-access/app-based-mfa.md).

Registreer uw toepassing in Azure Active Directory gebruiken. Registreren van de toepassing, betekent dat uw ontwikkelaars Azure AD gebruiken kunnen voor verificatie van gebruikers en de toegang tot resources van de gebruiker, zoals e-mail, agenda en documenten.

Elk lid van de map (geen gasten) kan een toepassing registreert, ook wel genoemd *het maken van een toepassingsobject*.

Registreren van een toepassing kan elke gebruiker het volgende doen:

* Een identiteit voor de toepassing die door Azure AD worden herkend ophalen
* Een of meer geheimen/sleutels ophalen die de toepassing gebruiken kunt om te authenticeren bij AD
* Merk de toepassing in Azure portal met een aangepaste naam, het logo, enzovoort.
* Azure AD-autorisatie functies toepassen op hun app, met inbegrip van:

  * RBAC (op rollen gebaseerd toegangsbeheer)
  * Azure Active Directory als oAuth-autorisatie-server (Beveilig een API die worden weergegeven door de toepassing)
* Declareer vereiste machtigingen die nodig zijn voor de toepassing van functie, zoals verwacht, met inbegrip van:

     - App-machtigingen (alleen globale beheerders). Bijvoorbeeld: Lidmaatschap van de rol in een andere Azure AD-toepassing of rol lidmaatschap ten opzichte van een Azure-Resource, resourcegroep, of een abonnement
     - Gedelegeerde machtigingen (een gebruiker). Bijvoorbeeld: Azure AD, aanmelden, profiel- en lezen

> [!NOTE]
> Standaard kan alle leden registreert een toepassing. Zie voor informatie over het beperken van machtigingen voor het registreren van toepassingen naar specifieke leden, [hoe toepassingen worden toegevoegd aan Azure AD](../develop/active-directory-how-applications-are-added.md#who-has-permission-to-add-applications-to-my-azure-ad-instance).
>
>

Dit is wat u als globale beheerder, moet doen zodat ontwikkelaars hun toepassing gereed is voor productie maken:

* Access-regels (access policy/MFA) configureren
* De app voor het toewijzen van gebruikers vereisen en toewijzen van gebruikers configureren
* De standaard-gebruikerservaring toestemming onderdrukken

## <a name="configure-access-rules"></a>Toegangsregels configureren
Regels voor toegang per toepassing naar uw SaaS-apps configureren. U kunt bijvoorbeeld MFA vereisen of toegang tot gebruikers alleen toestaan in vertrouwde netwerken. De details voor deze zijn beschikbaar in het document [toegangsregels configureren](../conditional-access/app-based-mfa.md).

## <a name="configure-the-app-to-require-user-assignment-and-assign-users"></a>De app voor het toewijzen van gebruikers vereisen en toewijzen van gebruikers configureren
Standaard hebben gebruikers toegang tot van toepassingen zonder te worden toegewezen. Als de toepassing wordt aangegeven dat functies of als u wilt dat de toepassing wordt weergegeven in het toegangsvenster van een gebruiker, moet u echter Gebruikerstoewijzing vereisen.

Als u een Azure AD Premium of Enterprise Mobility Suite (EMS)-abonnee bent, wordt aangeraden met behulp van groepen. Groepen toewijzen aan de toepassing, kunt u om te delegeren van beheer van de continue toegang naar de eigenaar van de groep. U kunt maken van de groep of de verantwoordelijke partij vragen in uw organisatie om de groep met behulp van de groep management-functie te maken.

[Gebruikers en groepen toewijzen aan een toepassing](methods-for-assigning-users-and-groups.md)  


## <a name="suppress-user-consent"></a>Toestemming van de gebruiker onderdrukken
Elke gebruiker gaat u standaard via een toestemming aan te melden. De ervaring van toestemming, gebruikers vragen om toegang te verlenen tot een toepassing kan worden toegevoegd voor gebruikers die niet bekend bent met deze beslissingen.

Voor toepassingen die u vertrouwt, kunt u de gebruikerservaring vereenvoudigen door stemt ermee in dat de toepassing namens uw organisatie.

Zie voor meer informatie over de toestemming van de gebruiker en de toestemming-ervaring in Azure, [toepassingen integreren met Azure Active Directory](../develop/quickstart-v1-integrate-apps-with-azure-ad.md).

## <a name="related-articles"></a>Gerelateerde artikelen
* [Veilige externe toegang tot on-premises toepassingen met Azure AD-toepassingsproxy inschakelen](application-proxy.md)
* [Beheer van toegang tot apps met Azure AD](what-is-access-management.md)

