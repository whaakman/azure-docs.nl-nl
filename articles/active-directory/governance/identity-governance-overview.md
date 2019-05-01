---
title: Identity Governance - Azure Active Directory | Microsoft Docs
description: Azure Active Directory Identity Governance kunt u saldo van uw organisatie behoefte aan beveiligings- en werknemer productiviteit met de juiste processen en zichtbaarheid.
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
ms.subservice: compliance
ms.date: 04/29/2019
ms.author: rolyon
ms.reviewer: markwahl-msft
ms.collection: M365-identity-device-management
ms.openlocfilehash: d30bbddd044d1aea70e43825035c94b69a46f1f8
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/30/2019
ms.locfileid: "64935835"
---
# <a name="what-is-azure-ad-identity-governance"></a>Wat is Azure AD Identity Governance?

Azure Active Directory (Azure AD) Identity Governance kunt u saldo van uw organisatie behoefte aan beveiligings- en werknemer productiviteit met de juiste processen en zichtbaarheid. Het biedt u mogelijkheden om ervoor te zorgen dat de juiste gebruikers hebben de juiste toegang tot de juiste resources en deze kunt u beveiligen, beheer en controleer de toegang tot essentiële activa--terwijl ervoor te zorgen dat de productiviteit van werknemers.  

Identiteitsbeheer bieden organisaties de mogelijkheid te doen van de volgende taken voor werknemers, zakelijke partners en leveranciers, en services en toepassingen:

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

![Levenscyclus van identiteit](./media/identity-governance-overview/identity-lifecycle.png)

Levenscyclus van identiteit voor medewerkers is voor veel organisaties gekoppeld aan de weergave van die gebruiker in een HCM (personeelsadministratie)-systeem.  Azure AD Premium onderhoudt automatisch gebruikers-id's voor gebruikers weergegeven in Workday in Active Directory en Azure Active Directory, zoals beschreven in de [Workday inbound provisioning (preview) zelfstudie](../saas-apps/workday-inbound-tutorial.md).  Azure AD Premium omvat ook [Microsoft Identity Manager](/microsoft-identity-manager/), waarmee records kunt importeren uit on-premises HCM systemen, zoals SAP, Oracle eBusiness en Oracle PeopleSoft.

Samenwerking met personen buiten uw organisatie is steeds meer scenario's vereist. [Azure AD B2B](/azure/active-directory/b2b/) samenwerking kunt u de toepassingen en services van uw organisatie veilig delen met gastgebruikers en externe partners van elke organisatie, met behoud van controle over uw eigen zakelijke gegevens.

## <a name="access-lifecycle"></a>Levenscyclus van toegang

Organisaties moeten een proces voor het beheren van toegang tot meer dan wat in eerste instantie voor een gebruiker ingericht is wanneer de identiteit van die gebruiker is gemaakt.  Enterprise-organisaties moeten bovendien mogelijk op schaal efficiënt te kunnen ontwikkelen en af te dwingen toegangsbeleid en besturingselementen regelmatig.

![Levenscyclus van toegang](./media/identity-governance-overview/access-lifecycle.png)

Normaal gesproken IT delegeert toegang tot zakelijke beleidsmakers goedkeuring beslissingen nemen.  Bovendien IT kan betrekking hebben op de gebruikers zelf.  Gebruikers die toegang hebben tot vertrouwelijke klantgegevens in de handel brengen toepassing van een bedrijf in Europa moeten bijvoorbeeld weten beleidsregels van het bedrijf. Gastgebruikers ook kunnen mogelijk geen informatie over de vereisten voor de afhandeling van gegevens in een organisatie waaraan ze zijn uitgenodigd.

Organisaties kunnen het proces van de levenscyclus van toegang tot en met technologieën zoals automatiseren [dynamische groepen](../users-groups-roles/groups-dynamic-membership.md), verbonden met het inrichten van gebruikers naar [SaaS-apps](../saas-apps/tutorial-list.md) of [apps worden geïntegreerd met SCIM](../manage-apps/use-scim-to-provision-users-and-groups.md).  Organisaties kunnen ook bepalen welke [gastgebruikers hebben toegang tot on-premises toepassingen](../b2b/hybrid-cloud-to-on-premises.md).  Deze toegang krijgen tot rechten kan vervolgens worden regelmatig gecontroleerd met behulp van terugkerende [Azure AD-toegangsbeoordelingen](access-reviews-overview.md).

Wanneer een gebruiker probeert toegang tot toepassingen, Azure AD afgedwongen [voorwaardelijke toegang](/azure/active-directory/conditional-access/) beleid. Beleid voor voorwaardelijke toegang kunnen bijvoorbeeld weergeven van een [gebruiksvoorwaarden](../conditional-access/terms-of-use.md) en [ervoor te zorgen dat de gebruiker heeft ingestemd met deze voorwaarden](../conditional-access/require-tou.md) voordat hij er toegang tot een toepassing.

## <a name="privileged-access-lifecycle"></a>Levenscyclus van bevoorrechte toegang

In het verleden heeft uitgebreide toegang is beschreven door andere leveranciers als een afzonderlijke functie van identiteitsbeheer. Echter bij Microsoft, we denken dat van bestuur van bevoegde toegang is een belangrijk onderdeel van Identiteitsbestuur, met name de mogelijkheid gegeven voor misbruik die zijn gekoppeld aan de beheerder rechten aan een organisatie kunnen veroorzaken. De werknemers, leveranciers en contractanten die administratieve rechten overnemen moeten worden geregeld.

![Levenscyclus van bevoorrechte toegang](./media/identity-governance-overview/privileged-access-lifecycle.png)

Azure AD Privileged Identity Management (PIM) biedt extra besturingselementen die zijn afgestemd op het beveiligen van toegang rechten voor bronnen in Azure AD, Azure en andere Microsoft Online Services.  De just-in-time-toegang en het wijzigen van een rol waarschuwingsfuncties geleverd door Azure AD PIM, naast de meervoudige verificatie en voorwaardelijke toegang, bieden dat een uitgebreide set governance besturingselementen voor de beveiligde resources van uw bedrijf (directory Office 365 en Azure-resourcerollen). Organisaties kunnen toegangsbeoordelingen gebruiken voor het configureren van terugkerende toegang certificering voor alle gebruikers in beheerdersrollen net als bij andere vormen van toegang.

## <a name="getting-started"></a>Aan de slag

Hoewel er geen perfecte oplossing of de aanbeveling voor elke klant, bieden de volgende configuraties een handleiding voor het wat Microsoft u raadt basislijn-beleid volgen om te controleren of een veilige en productieve personeel.

- [Configuraties voor identiteit en Apparaattoegang](/microsoft-365/enterprise/microsoft-365-policies-configurations)
- [Uitgebreide toegang beveiligen](../users-groups-roles/directory-admin-roles-secure.md)

U kunt ook het tabblad aan de slag van uitchecken **Identiteitsbestuur** in de Azure-portal om te beginnen met beheer van waar u recht op toegang tot beoordelingen, Privileged Identity Management en gebruiksvoorwaarden.

![Identiteitsbestuur aan de slag](./media/identity-governance-overview/getting-started.png)

## <a name="next-steps"></a>Volgende stappen

- [Wat is Azure AD recht management? (Preview)](entitlement-management-overview.md)
- [Wat zijn Azure AD-toegang beoordeelt?](access-reviews-overview.md)
- [Wat is Azure AD Privileged Identity Management?](../privileged-identity-management/pim-configure.md)
- [Wat kan ik doen met Gebruiksvoorwaarden?](active-directory-tou.md)
