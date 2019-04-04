---
title: Controlelijst voor implementatie van Azure AD
description: Azure Active Directory-functie controlelijst voor implementatie
services: active-directory
ms.service: active-directory
ms.subservice: ''
ms.topic: conceptual
ms.date: 01/08/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: ''
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6e9ee0d6fab96c84eee8a520d01d97faddab49f2
ms.sourcegitcommit: 9f4eb5a3758f8a1a6a58c33c2806fa2986f702cb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58904182"
---
# <a name="azure-active-directory-feature-deployment-guide"></a>Azure Active Directory-functie-Implementatiehandleiding

Het kan afschrikken aan Azure Active Directory (Azure AD) implementeren voor uw organisatie en deze te beveiligen. In dit artikel bevat algemene taken dat klanten nuttig om te voltooien in fasen, in de loop van 30, 60, 90 dagen of meer voor het verbeteren van de beveiliging verbeteren. Zelfs organisaties die Azure AD al hebben geïmplementeerd, kunnen deze handleiding gebruiken om te controleren of dat ze zijn het meeste uit hun bestaande investering ophalen.

Een goed geplande en uitgevoerde identiteitsinfrastructuur de weg wordt vrijgemaakt voor beveiligde toegang tot uw productiviteit workloads en gegevens door een bekende gebruikers en apparaten alleen.

Daarnaast klanten kunnen controleren op hun [identiteit secure score](identity-secure-score.md) om te zien hoe uitgelijnde ze worden aanbevolen procedures van Microsoft. Controleer uw beveiligde score vóór en na de implementatie van deze aanbevelingen om te zien hoe goed u doet in vergelijking met anderen in uw branche en andere organisaties van de grootte van uw.

## <a name="prerequisites"></a>Vereisten

Veel van de aanbevelingen in deze handleiding kunnen worden geïmplementeerd met Azure AD Free, Basic of er geen licentie helemaal. Waarvoor licenties vereist zijn we aangeven welke licentie is vereist ten minste de taak te voltooien.

Meer informatie over licentieverlening vindt u op de volgende pagina's:

* [Azure AD-licenties](https://azure.microsoft.com/pricing/details/active-directory/)
* [Microsoft 365 Enterprise](https://www.microsoft.com/en-us/licensing/product-licensing/microsoft-365-enterprise)
* [Enterprise Mobility + Security](https://www.microsoft.com/en-us/licensing/product-licensing/enterprise-mobility-security)
* [Azure AD B2B-licentieverlening richtlijnen](../b2b/licensing-guidance.md)

## <a name="phase-1-build-a-foundation-of-security"></a>Fase 1: Bouw een basis van beveiliging

In deze fase inschakelen beheerders basislijn beveiligingsfuncties die u het maken van een basis veiliger en gemakkelijk te gebruiken in Azure AD voordat we importeer of maak gewone gebruikersaccounts. Deze fundamentele fase zorgt ervoor dat u een beter beveiligde status vanaf het begin en dat uw eindgebruikers alleen moeten worden opgenomen in de nieuwe concepten één keer.

| Taak | Detail | Vereiste licentie |
| ---- | ------ | ---------------- |
| [Meer dan één globale beheerder aanwijzen](../users-groups-roles/directory-emergency-access.md) | Ten minste twee alleen in de cloud permanente global administrator-accounts voor gebruik toewijzen als zich een noodsituatie. Deze accounts zijn niet dagelijks worden gebruikt en lange en complexe wachtwoorden moeten hebben. | Azure AD Free |
| [Niet-globale beheerdersrollen gebruik waar mogelijk](../users-groups-roles/directory-assign-admin-roles.md) | Geef uw beheerders alleen de benodigde toegang tot alleen de gebieden die ze nodig hebben toegang tot. Niet alle beheerders moeten globale beheerders. | Azure AD Free |
| [Privileged Identity Management inschakelen voor het bijhouden van gebruik van beheerder-rol](../privileged-identity-management/pim-getting-started.md) | Privileged Identity Management om te beginnen met het bijhouden van het gebruik van administratieve rollen inschakelen. | Azure AD Premium P2 |
| [Implementatie van selfservice voor wachtwoord opnieuw instellen](../authentication/howto-sspr-deployment.md) | Aanroepen van de helpdesk verminderen van wachtwoorden door medewerkers opnieuw in te stellen hun eigen wachtwoorden met behulp van beleid toe te laten u als een beheerder kan bepalen. | Azure AD Basic |
| [Maak een lijst met organisatie-specifieke aangepaste uitgesloten wachtwoorden](../authentication/howto-password-ban-bad-configure.md) | Voorkomen dat gebruikers wachtwoorden die gewone woorden of zinsdelen van uw organisatie of maken. | Azure AD Basic |
| [On-premises integratie met Azure AD wachtwoordbeveiliging inschakelen](../authentication/concept-password-ban-bad-on-premises.md) | De lijst met uitgesloten wachtwoorden naar uw on-premises adreslijst, om ervoor te zorgen wachtwoorden set on-premises zijn ook in overeenstemming met de globale uitbreiden en tenant-specifieke verboden lijsten met wachtwoorden. | Azure AD Premium P1 |
| [Richtlijnen voor het wachtwoord van Microsofts inschakelen](https://www.microsoft.com/research/publication/password-guidance/) | Stop waarvoor gebruikers hun wachtwoord volgens een vast schema wijzigen, uitschakelen complexiteitsvereisten voldoen en uw gebruikers zijn meer apt hun wachtwoord onthouden en houd ze iets dat is beveiligd. | Azure AD Free |
| [Periodieke wachtwoord opnieuw instellen van wachtwoorden voor gebruikersaccounts met cloud-gebaseerde uitschakelen](../authentication/concept-sspr-policy.md#set-a-password-to-never-expire) | Periodieke wachtwoorden Vertel uw gebruikers dat moet worden verhoogd van hun bestaande wachtwoorden. Gebruik de richtlijnen in Microsoft wachtwoord richtlijnen doc-bestand en mirror van uw on-premises beleid voor alleen-cloud-gebruikers. | Azure AD Free |
| [Slimme vergrendeling van het Azure Active Directory aanpassen](../authentication/howto-password-smart-lockout.md) | Vergrendelingen van cloud-gebaseerde gebruikers kunnen worden gerepliceerd naar de on-premises Active Directory-gebruikers stoppen | Azure AD Basic |
| [Slimme van het Extranet inschakelen voor AD FS](/windows-server/identity/ad-fs/operations/configure-ad-fs-extranet-smart-lockout-protection) | AD FS-extranetvergrendeling beschermt tegen beveiligingsaanvallen wachtwoord raden aanvallen, terwijl u geldige AD FS-gebruikers echter ook doorgaan met hun accounts. | |
| [Azure AD multi-factor Authentication met behulp van beleid voor voorwaardelijke toegang implementeren](../authentication/howto-mfa-getstarted.md) | Vereisen dat gebruikers verificatie in twee stappen uitvoeren bij het openen van gevoelige toepassingen met behulp van beleid voor voorwaardelijke toegang. | Azure AD Premium P1 |
| [Azure Active Directory Identity Protection inschakelen](../identity-protection/enable.md) | Schakel tracering van riskante aanmeldingen en gestolen referenties voor gebruikers in uw organisatie. | Azure AD Premium P2 |
| [Risicogebeurtenissen die worden gebruikt voor het activeren van multi-factor authentication en het wachtwoord te wijzigen](../authentication/tutorial-risk-based-sspr-mfa.md) | Die gebeurtenissen, zoals meervoudige verificatie, wachtwoord opnieuw instellen en het blokkeren van aanmeldingen op basis van risico's kunt activeren-automatisering inschakelen. | Azure AD Premium P2 |
| [Geconvergeerde registratie voor self-service voor wachtwoord opnieuw instellen en Azure AD multi-factor Authentication (preview) inschakelen](../authentication/concept-registration-mfa-sspr-converged.md) | Toestaan dat uw gebruikers zich registreren vanuit een gemeenschappelijke ervaring voor zowel de Azure multi-factor Authentication en de self-service voor wachtwoord opnieuw instellen. | Azure AD Premium P1 |

## <a name="phase-2-import-users-enable-synchronization-and-manage-devices"></a>Fase 2: Gebruikers importeren, schakelt u de synchronisatie en apparaten beheren

Voeg vervolgens we toe aan de foundation in fase 1 grond van onze gebruikers importeren en het inschakelen van synchronisatie plannen voor toegang voor gasten en voorbereiden om aanvullende functionaliteit te ondersteunen.

| Taak | Detail | Vereiste licentie |
| ---- | ------ | ---------------- |
| [Azure AD Connect installeren](../connect/active-directory-aadconnect-select-installation.md) | Voorbereiden om te synchroniseren van gebruikers van uw bestaande on-premises directory naar de cloud. | Azure AD Free |
| [Synchronisatie van Wachtwoordhashes implementeren](../connect/active-directory-aadconnectsync-implement-password-hash-synchronization.md) | Wachtwoord-hashes zodat het wachtwoord te wijzigen om te worden gerepliceerd, onjuist wachtwoord detectie en herstel en de referentie is gelekt rapportage worden gesynchroniseerd. | Azure AD Premium P1 |
| [Het terugschrijven van wachtwoorden implementeren](../authentication/howto-sspr-writeback.md) | Toestaan dat wachtwoord te wijzigen in de cloud worden teruggeschreven naar een on-premises Windows Server Active Directory-omgeving. | Azure AD Premium P1 |
| [Implementeer Azure AD Connect Health](../connect-health/active-directory-aadconnect-health.md) | Schakel de bewaking van de van belangrijke gezondheidsstatistieken voor uw Azure AD Connect-servers, AD FS-servers en domeincontrollers. | Azure AD Premium P1 |
| [Licenties toewijzen aan gebruikers door groepslidmaatschappen in Azure Active Directory](../users-groups-roles/licensing-groups-assign.md) | Bespaar tijd en moeite door het maken van licentieverlening groepen die functies per groep in plaats van de instelling per gebruiker- of uitschakelen. | |
| [Maak een plan voor toegang van gastgebruikers](../b2b/what-is-b2b.md) | Werk samen met gastgebruikers doordat ze aanmelden bij uw apps en services met hun eigen werk, school of sociale identiteiten. | [Azure AD B2B-licentieverlening richtlijnen](../b2b/licensing-guidance.md) |
| [Beslissen over de strategie voor het beheer van apparaat](../devices/overview.md) | Bepaal uw organisatie kan met betrekking tot de apparaten. Registreren van vs lid wordt, Bring Your Own Device vs opgegeven bedrijf. | |
| [Windows Hello voor bedrijven in uw organisatie implementeren](/windows/security/identity-protection/hello-for-business/hello-manage-in-organization) | Voorbereiden op het zonder wachtwoord verificatie met behulp van Windows Hello | |

## <a name="phase-3-manage-applications"></a>Fase 3: Toepassingen beheren

Als we doorgaan met het bouwen van op de vorige fasen, we bepalen welke toepassingen kandidaat voor de migratie en integratie met Azure AD en voltooit de installatie van deze toepassingen.

| Taak | Detail | Vereiste licentie |
| ---- | ------ | ---------------- |
| Uw toepassingen identificeren | Bepalen welke toepassingen wordt gebruikt in uw organisatie: on-premises, SaaS-toepassingen in de cloud en andere line-of-business-toepassingen. Als deze toepassingen kunnen en moeten worden beheerd met Azure AD bepalen. | Er is geen licentie vereist |
| [Ondersteunde SaaS-toepassingen in de galerie integreren](../manage-apps/add-application-portal.md) | Azure AD heeft een galerie met duizenden vooraf geïntegreerde toepassingen. Enkele van de toepassingen die uw organisatie gebruikt, waarschijnlijk in de galerie die toegankelijk is rechtstreeks vanuit de Azure portal. | Azure AD Free |
| [Application Proxy gebruiken om te integreren van on-premises toepassingen](../manage-apps/application-proxy-add-on-premises-application.md) | Application Proxy kan gebruikers toegang tot on-premises toepassingen aanmelden met hun Azure AD-account. | Azure AD Basic |

## <a name="phase-4-audit-privileged-identities-complete-an-access-review-and-manage-user-lifecycle"></a>Fase 4: Bevoorrechte identiteiten controleren en beheren van de levenscyclus van de gebruiker een toegangscontrole voltooien

Fase 4 ziet beheerders afdwingen van minimale bevoegdheden beginselen voor het beheer, hun eerste toegangsbeoordelingen voltooien en het inschakelen van automatisering van algemene taken van de levenscyclus van gebruiker.

| Taak | Detail | Vereiste licentie |
| ---- | ------ | ---------------- |
| [Het gebruik van Privileged Identity Management afdwingen](../privileged-identity-management/pim-security-wizard.md) | Verwijder beheerdersrollen van normale dagelijkse gebruikersaccounts. Controleer gebruikers met beheerdersrechten in aanmerking voor het gebruik van hun rol na een controle van de multi-factor authentication slaagt, een zakelijke rechtvaardiging te geven of aanvragen van goedkeuring van fiatteurs. | Azure AD Premium P2 |
| [Een toegangscontrole voor Azure AD directory-rollen in PIM voltooien](../privileged-identity-management/pim-how-to-start-security-review.md) | Werken met uw beveiligings- en leiderschap teams om het maken van een toegangsbeleid voor de beoordeling om te controleren met beheerdersrechten toegang op basis van beleid van uw organisatie. | Azure AD Premium P2 |
| [Een dynamische groepslidmaatschap beleid implementeren](../users-groups-roles/groups-dynamic-membership.md) | Dynamische groepen gebruiken om gebruikers automatisch toewijzen aan groepen op basis van hun kenmerken van HR (of de bron is), zoals afdeling, titel, regio, en andere kenmerken. |  |
| [Inrichten van de groep op basis van toepassingen implementeren](../manage-apps/what-is-access-management.md) | Gebruik toegangsbeheer op basis van een groep inrichting op automatisch gebruikers inrichten voor SaaS-toepassingen. |  |
| [Inrichten van gebruikers en het opheffen van inrichting automatiseren](../manage-apps/user-provisioning.md) | Handmatige stappen van de levenscyclus van de werknemer-account om te voorkomen dat onbevoegde toegang verwijderen. Identiteiten van de bron van de waarheid (HR-systeem) naar Azure AD synchroniseren. |  |

## <a name="next-steps"></a>Volgende stappen

[Azure AD-licenties en prijzen](https://azure.microsoft.com/pricing/details/active-directory/)

[Configuraties voor identiteit en Apparaattoegang](https://docs.microsoft.com/microsoft-365/enterprise/microsoft-365-policies-configurations)

[Algemene beleidsregels voor identiteit en Apparaattoegang toegang aanbevolen](https://docs.microsoft.com/microsoft-365/enterprise/identity-access-policies)
