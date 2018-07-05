---
title: Beheer van toegang tot apps met behulp van Azure AD | Microsoft Docs
description: Hierin wordt beschreven hoe Azure Active Directory kunnen organisaties om op te geven van de apps waartoe elke gebruiker toegang heeft.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: barbkess
ms.openlocfilehash: f03516bf22f46f1b5e4869409ad7e999dc9960f1
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37449845"
---
# <a name="managing-access-to-apps"></a>Toegang tot apps beheren
Beheer van continue toegang, gebruik evaluatie en rapportage blijven lastig zijn om nadat een app is geïntegreerd in identiteitssysteem van uw organisatie. In veel gevallen hebben IT-beheerders of de helpdesk te nemen van een doorlopende actieve rol bij het beheren van toegang tot uw apps. Toewijzing wordt soms uitgevoerd door een algemene of divisie IT-team. Vaak de beslissing van de toewijzing is bedoeld om te worden overgedragen aan de zakelijke besluitvormer, waarvoor ze worden goedgekeurd voordat IT heeft de toewijzing.  Andere organisaties investeren in integratie met een bestaande geautomatiseerde identiteits- en toegangsbeheer management-systeem, zoals Role-Based Access Control (RBAC) of op kenmerken gebaseerde Access Control (ABAC). De integratie en de ontwikkeling van de regel zijn meestal gespecialiseerde en dure. Controleren of rapportage over beide benaderingen management is een eigen afzonderlijke, dure en complexe investering.

## <a name="how-does-azure-active-directory-help"></a>Hoe helpt Azure Active Directory?
 Azure AD biedt ondersteuning voor uitgebreide toegang management voor geconfigureerde toepassingen waarmee organisaties het juiste toegangsbeleid, variërend van automatische, op basis van een kenmerk toewijzing (ABAC of RBAC scenario's) door middel van overdracht en inclusief eenvoudig wordt om beheerdersbeheer. Met Azure AD, u kunt eenvoudig complexe beleid bereiken combineren van meerdere modellen voor één toepassing en kan zelfs opnieuw te gebruiken regels binnen toepassingen met de dezelfde doelgroepen.

* [Toevoegen van nieuwe of bestaande toepassingen](configure-single-sign-on-portal.md)

 Toewijzing van de Azure AD-toepassing is gericht op twee modi van de primaire toewijzing:

* **Individuele toewijzing** een IT-beheerder met de globale beheerdersmachtigingen directory kunt selecteren van afzonderlijke gebruikersaccounts en hun toegang tot de toepassing wordt verleend.
* **Toewijzing op basis van een groep (Azure AD alleen betaald)** een IT-beheerder met de globale beheerdersmachtigingen directory een groep kunt toewijzen aan de toepassing. Specifieke gebruikers toegang wordt bepaald door of ze leden van de groep op het moment dat ze proberen zijn te krijgen tot de toepassing. Een beheerder kan met andere woorden, een regel voor de toewijzing met vermelding van "alle huidige leden van de toegewezen groep heeft toegang tot de toepassing" effectief maken. Met deze optie toewijzing, beheerders kunnen profiteren van een van de Azure AD-groep beheeropties, met inbegrip van [dynamische groepen op basis van een kenmerk](../fundamentals/active-directory-groups-create-azure-portal.md), extern systeemgroepen (bijvoorbeeld on-premises Active Directory of Workday) of de beheerder of de zelf-Self-service beheerd groepen. Een groep kan worden eenvoudig toegewezen aan meerdere apps ervoor te zorgen dat toepassingen met toewijzing affiniteit toewijzingsregels, kunnen delen de complexiteit van het totale beheer verminderen. Houd er rekening mee dat genest groepslidmaatschap worden niet ondersteund voor toewijzing op basis van een groep aan een toepassing op dit moment.

Met behulp van deze toewijzing van de twee modi, beheerders kunnen maar liefst de methode voor elk gewenst toewijzing.

Met Azure AD, is gebruik en de toewijzing reporting volledig geïntegreerd, waardoor beheerders gemakkelijk rapporteren over de status van sitetoewijzing, fouten bij licentietoewijzing en zelfs gebruik.

## <a name="complex-application-assignment-with-azure-ad"></a>Toewijzing van complexe toepassingen met Azure AD
Houd rekening met een toepassing zoals Salesforce. In veel organisaties wordt Salesforce voornamelijk gebruikt met de marketing- en organisaties. Vaak, uitgebreide leden van het marketing-team zeer toegang tot Salesforce, terwijl de leden van het verkoopteam beperkte toegang hebben. In veel gevallen is een brede populatie van IT-medewerkers toegang beperkt tot de toepassing. Uitzonderingen op deze regels bemoeilijken belangrijk is. Het is vaak het recht van de teams van de leiderschap marketing- of een gebruikerstoegang verlenen of hun rollen onafhankelijk van deze algemene regels wijzigen.

Met Azure AD is-toepassingen, zoals Salesforce vooraf geconfigureerd voor eenmalige aanmelding (SSO) en geautomatiseerde inrichting. Nadat de toepassing is geconfigureerd, kan een beheerder de eenmalige actie maken en toewijzen van de juiste groepen duren. In dit voorbeeld kan een beheerder de volgende toewijzingen uitvoeren:

* [Dynamische groepen](../fundamentals/active-directory-groups-create-azure-portal.md) staat voor alle leden van het marketing- en teams met behulp van kenmerken, zoals afdeling of rol automatisch kunnen worden gedefinieerd:
  
  * Alle leden van groepen van marketing wordt toegewezen aan de rol 'marketing' in Salesforce
  * Alle leden van het verkoopteam groepen wordt toegewezen aan de rol 'sales' in Salesforce. Een verdere verfijning kan meerdere groepen die staan voor regionale verkoopteams toegewezen aan verschillende rollen voor Salesforce gebruiken.
* Als u wilt het uitzonderingsmechanisme voor inschakelen, kan een groepsbeheer met Self-service voor elke rol worden gemaakt. De groep 'Salesforce marketing uitzondering' kan bijvoorbeeld worden gemaakt als een groep zelf. De groep kan worden toegewezen aan de rol van de Salesforce-marketing en het marketingteam van leidinggevenden eigenaren kan worden gemaakt. Leden van het marketingteam van leidinggevenden kunnen toevoegen of verwijderen van gebruikers, instellen van een join-beleid, of zelfs goedkeuren of afwijzen afzonderlijke gebruikers aanvragen voor deelname aan. Dit mechanisme wordt ondersteund door een information worker juiste ervaring die geen speciale training voor eigenaren of leden vereist.

In dit geval zouden alle toegewezen gebruikers worden automatisch ingericht met Salesforce, wanneer ze worden toegevoegd aan verschillende groepen die hun roltoewijzing zou worden bijgewerkt in Salesforce. Gebruikers zou kunnen detecteren en Salesforce via het toegangsvenster Microsoft-toepassing en Office web-clients, of zelfs door te navigeren naar de aanmeldingspagina van hun organisatie Salesforce. Beheerders zou kunnen eenvoudig gebruik en de toewijzing om status te bekijken met behulp van Azure AD-rapportage zijn.

Beheerders kunnen gebruikmaken van [voorwaardelijke toegang voor Azure AD](../active-directory-conditional-access-azure-portal.md) toegangsbeleid voor specifieke rollen instellen. Deze beleidsregels kunnen opnemen of toegang is toegestaan buiten de bedrijfsomgeving en zelfs multi-factor Authentication of het apparaat vereisten voor toegang in verschillende gevallen bereiken.

## <a name="next-steps"></a>Volgende stappen

* [Article Index for Application Management in Azure Active Directory](../active-directory-apps-index.md) (Artikelindex voor toepassingsbeheer in Azure Active Directory)
* [Beveiligen van apps met voorwaardelijke toegang](../active-directory-conditional-access-azure-portal.md)
* [Groepsbeheer met Self-Service management/SSAA](../users-groups-roles/groups-self-service-management.md)
