---
title: Wat zijn toegangsbeoordelingen? -Azure Active Directory | Microsoft Docs
description: Azure Active Directory-toegangsbeoordelingen kunt u lidmaatschap en de toepassing toegang om te voldoen aan governance, risicobeheer en nalevingsinitiatieven in uw organisatie beheren.
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
ms.date: 06/05/2019
ms.author: rolyon
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7fcc804db66430598e72e9ebf31a8837eda1cca6
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67204596"
---
# <a name="what-are-azure-ad-access-reviews"></a>Wat zijn Azure AD-toegang beoordeelt?

Azure Active Directory (Azure AD)-toegangsbeoordelingen kunnen organisaties op efficiënte wijze groepslidmaatschappen beheren, toegang tot bedrijfstoepassingen en roltoewijzingen. Van de gebruiker toegang kan worden gecontroleerd op regelmatige basis om ervoor te zorgen dat alleen de juiste personen blijven toegang houden.

Hier volgt een video waarin een kort overzicht van toegangsbeoordelingen geeft:

>[!VIDEO https://www.youtube.com/embed/kDRjQQ22Wkk]

## <a name="why-are-access-reviews-important"></a>Waarom zijn de toegang beoordeelt belangrijk?

Azure AD kunt u intern samenwerken binnen uw organisatie en gebruikers van externe organisaties, zoals partners. Gebruikers kunnen deelnemen aan groepen, gasten uitnodigen, verbinding maken met cloud-apps en op afstand werken vanaf hun werk- of persoonlijke apparaten. Het gemak van het gebruik van de kracht van self-service heeft geleid tot betere mogelijkheden voor toegangsbeheer nodig.

- Wanneer nieuwe werknemers aan worden toegevoegd, hoe u ervoor zorgen dat ze de juiste toegang om productief te zijn?
- Wanneer mensen teams verplaatsen of het bedrijf verlaten, hoe u ervoor zorgen dat de oude toegang wordt verwijderd, met name wanneer dan ook de gasten?
- Overmatige toegangsrechten kunnen leiden tot het controleren van de bevindingen en compromissen aangezien ze duiden op een gebrek aan beheer van toegang.
- U moet proactief betrekken met eigenaren van resources om te controleren of dat ze regelmatig te controleren wie toegang heeft tot hun resources.

## <a name="when-to-use-access-reviews"></a>Wanneer u de toegang beoordeelt?

- **Te veel gebruikers in bevoorrechte rollen:** Er is een goed idee om te controleren hoeveel gebruikers met beheerdersrechten toegang hebben, hoeveel hiervan zijn globale beheerders, en als er een uitgenodigd gasten of partners die zijn niet verwijderd nadat ze zijn toegewezen aan het doen van een beheertaak. U kunt opnieuw certificeren voor de rol van toewijzing van gebruikers in [Azure AD-rollen](../privileged-identity-management/pim-how-to-perform-security-review.md?toc=%2fazure%2factive-directory%2fgovernance%2ftoc.json) zoals globale beheerders of [Azure-resources-rollen](../privileged-identity-management/pim-resource-roles-perform-access-review.md?toc=%2fazure%2factive-directory%2fgovernance%2ftoc.json) zoals Administrator voor gebruikerstoegang in de [Azure AD Privileged Identity Management (PIM)](../privileged-identity-management/pim-configure.md) optreden.
- **Automation is wanneer onbruikbare:** U kunt regels voor dynamisch lidmaatschap voor beveiligingsgroepen of Office 365-groepen, maar wat gebeurt er als de HR-gegevens zijn niet in Azure AD of als gebruikers nog steeds toegang nodig na het verlaten van de groep hun vervanging van de trein maken? Vervolgens kunt u een evaluatie maken voor de groep om ervoor te zorgen die nog steeds toegang nodig blijvende toegang moeten hebben.
- **Wanneer een groep wordt gebruikt voor een nieuw doel:** Als u een groep die u wilt worden gesynchroniseerd met Azure AD hebt, of als u van plan bent om in te schakelen van de Salesforce-toepassing voor iedereen in de groep Sales-team, is het handig om te vragen van de eigenaar van de groep om te controleren van het groepslidmaatschap voorafgaand aan de groep wordt gebruikt in een co verschillende risico 's inhoud.
- **Toegang tot kritieke gegevens van zakelijke:** voor bepaalde resources, kan het worden vereist om aan te vragen mensen buiten IT regelmatig afmelden en geef een reden op waarom ze toegang nodig hebben voor controledoeleinden.
- **Lijst met uitzonderingen van een beleid onderhouden:** In een ideaal volgen alle gebruikers het toegangsbeleid voor beveiligde toegang tot resources van uw organisatie. Soms zijn er echter bedrijfsscenario's waarvoor u uitzonderingen maken. Als de IT-beheerder, kunt u deze taak beheren, te voorkomen dat toezicht van uitzonderingen en auditors voorzien van bewijs dat deze uitzonderingen regelmatig worden gecontroleerd.
- **Eigenaren van groepen om te bevestigen moeten nog steeds gasten in hun groepen vragen:** Toegang van werknemers kan worden geautomatiseerd met bepaalde on-premises IAM, maar niet uitgenodigd. Als een groep Gasten toegang geeft tot zakelijke gevoelige inhoud wordt beschreven, worden de verantwoordelijkheid van de Groepseigenaar om te bevestigen van de gasten hebben nog steeds een legitieme zakelijke behoeften om toegang te krijgen.
- **Beoordelingen regelmatig terugkerende hebben:** U kunt terugkerende toegangsbeoordelingen van gebruikers bij set-frequenties, zoals instellen wekelijks, maandelijks, per kwartaal of jaar en de revisoren ontvangt een melding aan het begin van elke beoordeling. Revisoren kunnen goedkeuren of weigeren van toegang met een gebruiksvriendelijke interface en met behulp van intelligente aanbevelingen.

## <a name="where-do-you-create-reviews"></a>Waar ik u beoordelingen maken?

Afhankelijk van wat u bekijken wilt, maakt u de toegangsbeoordeling in Azure AD toegang tot beoordelingen, Azure AD enterprise-apps (in Preview-versie) of Azure AD PIM.

| Rechten van gebruikers | Revisoren kunnen zijn | Revisie is gemaakt in | Revisor-ervaring |
| --- | --- | --- | --- |
| Leden van beveiliging</br>Leden van Office | Opgegeven revisoren</br>Eigenaren van groepen</br>Zelf controleren | Azure AD-toegangsbeoordelingen</br>Azure AD-groepen | Toegangsvenster |
| Toegewezen aan een verbonden app | Opgegeven revisoren</br>Zelf controleren | Azure AD-toegangsbeoordelingen</br>Azure AD-enterprise-apps (in Preview-versie) | Toegangsvenster |
| Azure AD-rol | Opgegeven revisoren</br>Zelf controleren | [Azure AD PIM](../privileged-identity-management/pim-how-to-start-security-review.md?toc=%2fazure%2factive-directory%2fgovernance%2ftoc.json) | Azure Portal |
| De rol van de Azure-resource | Opgegeven revisoren</br>Zelf controleren | [Azure AD PIM](../privileged-identity-management/pim-resource-roles-start-access-review.md?toc=%2fazure%2factive-directory%2fgovernance%2ftoc.json) | Azure Portal |

## <a name="which-users-must-have-licenses"></a>Welke gebruikers moeten licenties hebben?

Elke gebruiker die met toegangsbeoordelingen communiceert moet een betaalde Azure AD Premium P2-licentie hebben. Voorbeelden zijn:

- Beheerders die een toegangsbeoordeling maken
- Eigenaren van groepen die toegang controleren
- Gebruikers die zijn toegewezen als revisoren
- Gebruikers die uitvoeren op een zelf controleren

U kunt ook vragen gastgebruikers ook kunnen op hun eigen toegang beoordelen. Voor elke betaalde Azure AD Premium P2-licentie die u aan een van de gebruikers van uw eigen organisatie toewijst, kunt u Azure AD business-to-business (B2B) op maximaal vijf gastgebruikers onder de aftrek van de externe gebruiker uitnodigen. Deze gastgebruikers kunnen functies van Azure AD Premium P2 ook gebruiken. Zie voor meer informatie, [Azure AD B2B-samenwerking licentierichtlijnen](../b2b/licensing-guidance.md).

Hier volgen enkele scenario's met voorbeelden om te bepalen het aantal licenties hebt.

| Scenario | Berekening | Vereiste aantal licenties |
| --- | --- | --- |
| Een beheerder maakt een toegangsbeoordeling van groep A met 500 gebruikers.<br/>3 groepseigenaren als beoordelaar toegewezen. | beheerder van 1 + 3 groepseigenaren | 4 |
| Een beheerder maakt een toegangsbeoordeling van groep A met 500 gebruikers.<br/>Maakt het een zelf controleren. | 1 beheerder + 500 gebruikers als zelfrevisors | 501 |
| Een beheerder maakt een toegangsbeoordeling van groep A met 5 gebruikers en 25 gastgebruikers.<br/>Maakt het een zelf controleren. | 1 beheerder + 5 gebruikers als zelfrevisors<br/>(gastgebruikers ook kunnen worden behandeld in de verhouding van de vereiste 1:5) | 6 |
| Een beheerder maakt een toegangsbeoordeling van groep A met 5 gebruikers en 28 gastgebruikers.<br/>Maakt het een zelf controleren. | 1 beheerder + 5 gebruikers als zelfrevisors + 1 voor de gebruiker om gastgebruikers ook kunnen in de verhouding tussen de vereiste 1:5 | 7 |

Zie voor meer informatie over het toewijzen van licenties aan uw gebruikt [toewijzen of verwijderen met behulp van de Azure Active Directory-portal licenties](../fundamentals/license-users-groups.md).

## <a name="learn-about-access-reviews"></a>Meer informatie over toegangsbeoordelingen

Bekijk deze korte demo voor meer informatie over het maken en uitvoeren van beoordelingen:

>[!VIDEO https://www.youtube.com/embed/6KB3TZ8Wi40]

Als u klaar bent voor toegangsbeoordelingen in uw organisatie te implementeren, als volgt te werk in de video om te introduceren, trainen uw beheerders en uw eerste toegangsbeoordeling maken!

>[!VIDEO https://www.youtube.com/embed/X1SL2uubx9M]

## <a name="license-requirements"></a>Licentievereisten

[!INCLUDE [Azure AD Premium P2 license](../../../includes/active-directory-p2-license.md)]

## <a name="next-steps"></a>Volgende stappen

- [Een toegangsbeoordeling van groepen of toepassingen maken](create-access-review.md)
- [Een toegangsbeoordeling maken van gebruikers met een Azure AD-beheerderrol](../privileged-identity-management/pim-how-to-start-security-review.md?toc=%2fazure%2factive-directory%2fgovernance%2ftoc.json)
- [Toegang tot groepen of toepassingen controleren](perform-access-review.md)
- [Een toegangscontrole van groepen of toepassingen voltooien](complete-access-review.md)
