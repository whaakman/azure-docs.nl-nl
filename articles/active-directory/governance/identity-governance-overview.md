---
title: Azure AD identity governance | Microsoft Docs
description: Azure AD identity governance kunt u saldo van uw organisatie behoefte aan beveiligings- en werknemer productiviteit met de juiste processen en zichtbaarheid.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.component: compliance
ms.date: 09/25/2018
ms.author: rolyon
ms.reviewer: markwahl-msft
ms.openlocfilehash: 20b1c8673bfdb3b2207ed63749f79539c396642c
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47167908"
---
# <a name="what-is-azure-ad-identity-governance"></a>Wat is Azure AD identity governance?

Azure Active Directory (Azure AD) identity governance kunt u saldo van uw organisatie behoefte aan beveiligings- en werknemer productiviteit met de juiste processen en zichtbaarheid. Het biedt u mogelijkheden om ervoor te zorgen dat de juiste gebruikers hebben de juiste toegang tot de juiste resources en deze kunt u beveiligen, beheer en controleer de toegang tot essentiële activa--terwijl ervoor te zorgen dat de productiviteit van werknemers.  

Identiteitsbeheer bieden organisaties de mogelijkheid voor het volgende voor werknemers, zakelijke partners en leveranciers, en services en toepassingen:

- Bepalen van de levenscyclus van identiteit
- Levenscyclus van de toegang beheren
- Veilig beheer

Specifiek, is bedoeld voor organisaties die verhelpen van deze vier belangrijke vragen:

- Welke gebruikers moeten toegang tot resources hebben?
- Wat zijn de gebruikers met die toegang doen?
- Zijn er effectieve organisatie besturingselementen voor het beheren van toegang?
- Kunnen auditors controleren of de besturingselementen zijn werkt?

## <a name="identity-lifecycle"></a>Levenscyclus van identiteit

Identiteitsbeheer helpt organisaties bij een evenwicht tussen *productiviteit* -hoe snel een persoon die toegang hebben tot de bronnen ze nodig hebben, zoals wanneer ze lid van mijn organisatie? En *security* -hoe moet de toegang wijzigen gedurende een periode, zoals vanwege wijzigingen naar de werkstatus van die persoon?  Beheer van identiteitslevenscycli vormt de basis voor identiteitsbeheer en doeltreffend beheer op schaal vereist de infrastructuur voor identiteiten lifecycle management voor toepassingen moderniseren.

Levenscyclus van identiteit voor medewerkers is voor veel organisaties gekoppeld aan de weergave van die gebruiker in een HCM (personeelsadministratie)-systeem.  Azure AD Premium onderhoudt automatisch gebruikers-id's voor gebruikers weergegeven in Workday in Active Directory en Azure Active Directory, zoals beschreven in de [Workday inbound provisioning (preview) zelfstudie](../saas-apps/workday-inbound-tutorial.md).  Azure AD Premium omvat ook [Microsoft Identity Manager](/microsoft-identity-manager/), waarmee records kunt importeren uit on-premises HCM systemen, zoals SAP, Oracle eBusiness en Oracle PeopleSoft.

Samenwerking met personen buiten uw organisatie is steeds meer scenario's vereist. [Azure AD B2B](/azure/active-directory/b2b/) samenwerking kunt u de toepassingen en services van uw organisatie veilig delen met gastgebruikers en externe partners van elke organisatie, met behoud van controle over uw eigen zakelijke gegevens.

## <a name="access-lifecycle"></a>Levenscyclus van toegang

Organisaties moeten een proces voor het beheren van toegang tot meer dan wat in eerste instantie voor een gebruiker ingericht is wanneer de identiteit van die gebruiker is gemaakt.  Enterprise-organisaties moeten bovendien mogelijk op schaal efficiënt te kunnen ontwikkelen en af te dwingen toegangsbeleid en besturingselementen regelmatig.

Normaal gesproken IT delegeert toegang tot zakelijke beleidsmakers goedkeuring beslissingen nemen.  Bovendien IT kan betrekking hebben op de gebruikers zelf.  Gebruikers die toegang hebben tot vertrouwelijke klantgegevens in de handel brengen toepassing van een bedrijf in Europa moeten bijvoorbeeld weten beleidsregels van het bedrijf. Gastgebruikers ook kunnen mogelijk geen informatie over de vereisten voor de afhandeling van gegevens in een organisatie waaraan ze zijn uitgenodigd.

Organisaties kunnen het proces van de levenscyclus van toegang tot en met technologieën zoals automatiseren [dynamische groepen](../users-groups-roles/groups-dynamic-membership.md), verbonden met het inrichten van gebruikers naar [SaaS-apps](../saas-apps/tutorial-list.md) of [apps worden geïntegreerd met SCIM](../manage-apps/use-scim-to-provision-users-and-groups.md).  Organisaties kunnen ook bepalen welke [gastgebruikers hebben toegang tot on-premises toepassingen](../b2b/hybrid-cloud-to-on-premises.md).  Deze toegang krijgen tot rechten kan vervolgens worden regelmatig gecontroleerd met behulp van terugkerende [Azure AD-toegangsbeoordelingen](access-reviews-overview.md).

Wanneer een gebruiker probeert toegang tot toepassingen, Azure AD afgedwongen [voorwaardelijke toegang](/azure/active-directory/conditional-access/) beleid. Beleid voor voorwaardelijke toegang kunnen bijvoorbeeld weergeven van een [gebruiksvoorwaarden](active-directory-tou.md) en [ervoor te zorgen dat de gebruiker heeft ingestemd met deze voorwaarden](../conditional-access/require-tou.md) voordat hij er toegang tot een toepassing.

## <a name="privileged-access-lifecycle"></a>Levenscyclus van bevoorrechte toegang

In het verleden heeft uitgebreide toegang is beschreven door andere leveranciers als een afzonderlijke functie van identiteitsbeheer. Echter bij Microsoft, we denken dat van bestuur van bevoegde toegang is een belangrijk onderdeel van identiteitsbestuur, met name de mogelijkheid gegeven voor misbruik die zijn gekoppeld aan de beheerder rechten aan een organisatie kunnen veroorzaken. De werknemers, leveranciers en contractanten die administratieve rechten overnemen moeten worden geregeld.

Azure AD Privileged Identity Management (PIM) biedt extra besturingselementen die zijn afgestemd op het beveiligen van toegang rechten voor bronnen in Azure AD, Azure en andere Microsoft Online Services.  De just-in-time-toegang en het wijzigen van een rol waarschuwingsfuncties geleverd door Azure AD PIM, naast de meervoudige verificatie en voorwaardelijke toegang, bieden dat een uitgebreide set governance besturingselementen voor de beveiligde resources van uw bedrijf (directory Office 365 en Azure-resourcerollen). Organisaties kunnen toegangsbeoordelingen gebruiken voor het configureren van terugkerende toegang certificering voor alle gebruikers in beheerdersrollen net als bij andere vormen van toegang.

## <a name="getting-started"></a>Aan de slag

Hoewel er geen perfecte oplossing of de aanbeveling voor elke klant, bieden de volgende configuraties een handleiding voor welk Microsoft raadt aan om te controleren of een veilige en productieve personeelsbestand basislijn-beleid.

- [Configuraties voor identiteit en Apparaattoegang](/microsoft-365/enterprise/microsoft-365-policies-configurations)
- [Uitgebreide toegang beveiligen](../users-groups-roles/directory-admin-roles-secure.md)


### <a name="access-reviews"></a>Toegangsbeoordelingen

- [Wat is een toegangsbeoordeling?](access-reviews-overview.md)
- [Gebruikerstoegang beheren met toegangsbeoordelingen](manage-user-access-with-access-reviews.md)
- [Toegang voor gasten met toegangsbeoordelingen beheren](manage-guest-access-with-access-reviews.md)
- [Een toegangscontrole van een directory-rol starten](../privileged-identity-management/pim-how-to-start-security-review.md)

### <a name="terms-of-use"></a>Gebruiksvoorwaarden

- [Wat kan ik doen met de gebruiksvoorwaarden](active-directory-tou.md)

### <a name="privileged-identity-management"></a>Privileged identity management

- [Wat is Azure AD PIM?](../privileged-identity-management/pim-configure.md)
