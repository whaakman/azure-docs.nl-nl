---
title: 'Controlelijst: Azure Active Directory Premium P2-licenties functie | Microsoft Docs'
description: Controlelijst voor de implementatie voor de implementatie van Azure Active Directory Premium P2-functie op 30 dagen, 90 dagen, en meer.
services: active-directory
ms.service: active-directory
ms.component: ''
ms.topic: conceptual
ms.date: 09/17/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.custom: it-pro, seodec18
ms.openlocfilehash: 4fcb692d7189c84e32f55995538ffc692cb67dd6
ms.sourcegitcommit: 3ab534773c4decd755c1e433b89a15f7634e088a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/07/2019
ms.locfileid: "54064709"
---
# <a name="azure-active-directory-premium-p2-licensing-feature-checklist"></a>Azure Active Directory Premium P2 licentieverlening functie controlelijst

Het kan overweldigend zijn aan Azure Active Directory (Azure AD) implementeren voor uw organisatie en deze te beveiligen. In dit artikel geeft enkele algemene taken die klanten nuttig vindt. Klanten voeren deze taken doorgaans in de loop van 30 dagen na 90 dagen of buiten hun beveiligingspostuur verbeteren. Zelfs organisaties die Azure AD al hebben geïmplementeerd, kunnen deze controlelijst om ervoor te zorgen dat ze bij het ophalen van het meeste uit hun bestaande investering gebruiken.

Een goed geplande en uitgevoerde identiteitsinfrastructuur de weg wordt vrijgemaakt voor beter beveiligde toegang tot uw productiviteit workloads en gegevens alleen door een geverifieerde gebruikers en apparaten.

## <a name="prerequisites"></a>Vereisten

Deze handleiding wordt ervan uitgegaan dat u hebt Azure AD Premium P2-licenties, Enterprise Mobility + Security E5, Microsoft 365 E5 of een vergelijkbare licentie-bundel.

[Azure AD-licenties](https://azure.microsoft.com/pricing/details/active-directory/)

[Microsoft 365 Enterprise](https://www.microsoft.com/en-us/licensing/product-licensing/microsoft-365-enterprise)

[Enterprise Mobility + Security](https://www.microsoft.com/en-us/licensing/product-licensing/enterprise-mobility-security)

## <a name="plan-and-deploy-day-1-30"></a>Plannen en implementeren: 1 tot 30 dagen

- Opgeven van meer dan één globale beheerder (einde-glas account)
   - [Beheerdersaccounts voor EMS-toegang beheren in Azure AD](../users-groups-roles/directory-emergency-access.md)
- Inschakelen van Azure AD Privileged Identity Management (PIM) om rapporten weer te geven
   - [Beginnen met PIM](../privileged-identity-management/pim-getting-started.md)
- Niet-globale beheerdersrollen gebruik waar mogelijk.
   - [Beheerdersrollen toewijzen in Azure Active Directory](../users-groups-roles/directory-assign-admin-roles.md)
- Verificatie
   - [Selfservice voor wachtwoordherstel implementeren](../authentication/howto-sspr-deployment.md)
   - Azure AD-wachtwoord Protection (preview) te implementeren
      - [Onjuiste wachtwoorden in uw organisatie te elimineren](../authentication/concept-password-ban-bad.md)
      - [Afdwingen van de beveiliging van Azure AD-wachtwoord voor Windows Server Active Directory](../authentication/concept-password-ban-bad-on-premises.md)
   - Het beleid voor accountvergrendeling configureren
      - [Azure Active Directory slimme accountvergrendeling](../authentication/howto-password-smart-lockout.md)
      - [AD FS Extranet Lockout en slimme van het Extranet](/windows-server/identity/ad-fs/operations/configure-ad-fs-extranet-smart-lockout-protection)
   - [Azure AD multi-factor Authentication met behulp van beleid voor voorwaardelijke toegang implementeren](../authentication/howto-mfa-getstarted.md)
   - [Geconvergeerde registratie voor self-service voor wachtwoord opnieuw instellen en Azure AD multi-factor Authentication (preview) inschakelen](../authentication/concept-registration-mfa-sspr-converged.md)
   - [Azure Active Directory Identity Protection inschakelen](../identity-protection/enable.md)
      - [Risicogebeurtenissen trigger multi-factor Authentication en wachtwoordwijzigingen gebruiken](../authentication/tutorial-risk-based-sspr-mfa.md)
   - [Wachtwoord-richtlijnen](https://www.microsoft.com/research/publication/password-guidance/)
      - Een vereiste minimale lengte van acht tekens behouden, meer is niet noodzakelijkerwijs beter.
      - Elimineer teken samenstelling vereisten.
      - [Verplichte periodieke wachtwoord opnieuw instellen van wachtwoorden voor gebruikersaccounts te elimineren.](../authentication/concept-sspr-policy.md#set-a-password-to-never-expire)
- Gebruikers van on-premises Active Directory synchroniseren
   - [Azure AD Connect installeren](../connect/active-directory-aadconnect-select-installation.md)
   - [Synchronisatie van Wachtwoordhashes implementeren](../connect/active-directory-aadconnectsync-implement-password-hash-synchronization.md)
   - [Het terugschrijven van wachtwoorden implementeren](../authentication/howto-sspr-writeback.md)
   - [Implementeer Azure AD Connect Health](../connect-health/active-directory-aadconnect-health.md)
- [Licenties toewijzen aan gebruikers door groepslidmaatschappen in Azure Active Directory](../users-groups-roles/licensing-groups-assign.md)

## <a name="plan-and-deploy-day-31-90"></a>Plannen en implementeren: 31 tot 90 dagen

- [Plan voor toegang van gastgebruikers](../b2b/what-is-b2b.md)
   - [Gebruikers van Azure Active Directory B2B-samenwerking toevoegen in Azure portal](../b2b/add-users-administrator.md)
   - [Uitnodigingen voor B2B-gebruikers van bepaalde organisaties toestaan of blokkeren](../b2b/allow-deny-list.md)
   - [GRANT B2B-gebruikers in Azure AD-toegang tot uw on-premises toepassingen](../b2b/hybrid-cloud-to-on-premises.md)
- Neem beslissingen over de strategie voor het beheer van levenscyclus
- [Beslissen over de strategie voor het beheer van apparaat](../devices/overview.md)
   - [Gebruiksscenario's en overwegingen voor de implementatie voor Azure AD Join](../devices/azureadjoin-plan.md)
- [Beheren van Windows Hello voor bedrijven in uw organisatie](/windows/security/identity-protection/hello-for-business/hello-manage-in-organization)

## <a name="plan-and-deploy-day-90-and-beyond"></a>Plannen en implementeren: Dag 90 en hoger

- [Azure AD Privileged Identity Management](../privileged-identity-management/pim-configure.md)
   - [Azure AD directory rolinstellingen in PIM configureren](../privileged-identity-management/pim-how-to-change-default-settings.md)
   - [Azure AD-directory-rollen in PIM toewijzen](../privileged-identity-management/pim-how-to-add-role-to-user.md)
- [Een toegangscontrole voor Azure AD directory-rollen in PIM voltooien](../privileged-identity-management/pim-how-to-start-security-review.md)
- De levensduur van zuinigste beheren
   - Azure AD heeft een aanpak voor het beheer van levenscyclus van identiteit
   - Handmatige stappen van de levenscyclus van uw werknemer-account, om te voorkomen dat onbevoegde toegang verwijderen:
      - Identiteiten van de bron van de waarheid (HR-systeem) naar Azure AD synchroniseren.
      - [Dynamische groepen gebruiken om gebruikers automatisch toewijzen aan groepen op basis van hun kenmerken van HR (of de bron is), zoals afdeling, titel, regio, en andere kenmerken.](../users-groups-roles/groups-dynamic-membership.md)
      - [Gebruik toegangsbeheer op basis van een groep inrichting op automatisch gebruikers inrichten voor SaaS-toepassingen.](../manage-apps/what-is-access-management.md)

## <a name="next-steps"></a>Volgende stappen

[Configuraties voor identiteit en Apparaattoegang](https://docs.microsoft.com/microsoft-365/enterprise/microsoft-365-policies-configurations)

[Algemene beleidsregels voor identiteit en Apparaattoegang toegang aanbevolen](https://docs.microsoft.com/microsoft-365/enterprise/identity-access-policies)
