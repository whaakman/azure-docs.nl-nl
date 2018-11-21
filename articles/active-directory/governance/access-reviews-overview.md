---
title: Wat zijn Azure AD-Toegangsbeoordelingen? | Microsoft Docs
description: Azure Active Directory-Toegangsbeoordelingen kunt u lidmaatschap en de toepassing toegang om te voldoen aan governance, risicobeheer en nalevingsinitiatieven in uw organisatie beheren.
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
ms.date: 11/19/2018
ms.author: rolyon
ms.reviewer: mwahl
ms.openlocfilehash: fe51419106f1164f9a9b5993261c61bad63333b5
ms.sourcegitcommit: fa758779501c8a11d98f8cacb15a3cc76e9d38ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/20/2018
ms.locfileid: "52262955"
---
# <a name="what-are-azure-ad-access-reviews"></a>Wat zijn Azure AD-Toegangsbeoordelingen?

Azure Active Directory (Azure AD)-Toegangsbeoordelingen kunnen organisaties op efficiënte wijze groepslidmaatschappen beheren, toegang tot bedrijfstoepassingen en roltoewijzingen. Van de gebruiker toegang kan worden gecontroleerd op regelmatige basis om ervoor te zorgen dat alleen de juiste personen blijven toegang houden.

Hier volgt een video waarin een kort overzicht van toegangsbeoordelingen geeft:

>[!VIDEO https://www.youtube.com/embed/kDRjQQ22Wkk]

## <a name="why-are-access-reviews-important"></a>Waarom zijn de toegang beoordeelt belangrijk?

Azure AD kunt u intern samenwerken binnen uw organisatie en gebruikers van externe organisaties, zoals partners. Gebruikers kunnen deelnemen aan groepen, gasten uitnodigen, verbinding maken met cloud-apps en op afstand werken vanaf hun werk- of persoonlijke apparaten. Het gemak van het gebruik van de kracht van self-service heeft geleid tot betere mogelijkheden voor toegangsbeheer nodig.

- Wanneer nieuwe werknemers aan worden toegevoegd, hoe u ervoor zorgen dat ze de juiste toegang om productief te zijn?
- Wanneer mensen teams verplaatsen of het bedrijf verlaten, hoe u ervoor zorgen dat de oude toegang wordt verwijderd, met name wanneer dan ook de gasten?
- Overmatige toegangsrechten kunnen leiden tot het controleren van de bevindingen en compromissen aangezien ze duiden op een gebrek aan beheer van toegang.
- U moet proactief betrekken met eigenaren van resources om te controleren of dat ze regelmatig te controleren wie toegang heeft tot hun resources.

## <a name="when-to-use-access-reviews"></a>Wanneer u de toegang beoordeelt?

- **Te veel gebruikers in bevoorrechte rollen:** het is een goed idee om te controleren hoeveel gebruikers met beheerdersrechten toegang hebben, hoeveel hiervan zijn algemene Admininistrators, en als er een uitgenodigd gasten of partners die zijn niet verwijderd nadat ze zijn toegewezen om te doen een administratieve taken. U kunt opnieuw certificeren voor de rol van toewijzing van gebruikers in [Azure AD-maprollen](../privileged-identity-management/pim-how-to-perform-security-review.md?toc=%2fazure%2factive-directory%2fgovernance%2ftoc.json) zoals globale beheerders of [Azure-resources-rollen](../privileged-identity-management/pim-resource-roles-perform-access-review.md?toc=%2fazure%2factive-directory%2fgovernance%2ftoc.json) zoals Administrator voor gebruikerstoegang in de [Azure AD Privileged Identity Management (PIM)](../privileged-identity-management/pim-configure.md) optreden.
- **Wanneer de automatisering is onbruikbare:** kunt u regels voor dynamisch lidmaatschap voor beveiligingsgroepen of Office 365-groepen maken, maar wat gebeurt er als de HR-gegevens zijn niet in Azure AD of als gebruikers nog steeds toegang nodig nadat de groep hun vervanging van de trein verlaten? Vervolgens kunt u een evaluatie maken voor de groep om ervoor te zorgen die nog steeds toegang nodig blijvende toegang moeten hebben.
- **Wanneer een groep wordt gebruikt voor een nieuw doel:** als u een groep die u wilt worden gesynchroniseerd met Azure AD hebt, of als u van plan bent om in te schakelen van de Salesforce-toepassing voor iedereen in de groep Sales-team, het is handig om te vragen van de eigenaar van de groep om te controleren van de groep membershi p voorafgaand aan de groep wordt gebruikt in de inhoud van een verschillende risico's.
- **Toegang tot kritieke gegevens van zakelijke:** voor bepaalde resources, kan het worden vereist om aan te vragen mensen buiten IT regelmatig afmelden en geef een reden op waarom ze toegang nodig hebben voor controledoeleinden.
- **Lijst met uitzonderingen van een beleid onderhouden:** ideaal, volgt alle gebruikers de toegang tot de beleidsregels voor beveiligde toegang tot resources van uw organisatie. Soms zijn er echter bedrijfsscenario's waarvoor u uitzonderingen maken. Als de IT-beheerder, kunt u deze taak beheren, te voorkomen dat toezicht van uitzonderingen en auditors voorzien van bewijs dat deze uitzonderingen regelmatig worden gecontroleerd.
- **Eigenaren van groepen om te bevestigen moeten nog steeds gasten in hun groepen vragen:** toegang van werknemers kan worden geautomatiseerd met sommige IAM on-premises, maar niet uitgenodigd. Als een groep Gasten toegang geeft tot zakelijke gevoelige inhoud wordt beschreven, worden de verantwoordelijkheid van de Groepseigenaar om te bevestigen van de gasten hebben nog steeds een legitieme zakelijke behoeften om toegang te krijgen.
- **Beoordelingen regelmatig terugkerende hebben:** kunt u een terugkerende toegangsbeoordelingen van gebruikers bij set-frequenties, zoals wekelijks, maandelijks, per kwartaal of per jaar instellen en de revisoren ontvangt een melding aan het begin van elke revisie. Revisoren kunnen goedkeuren of weigeren van toegang met een gebruiksvriendelijke interface en met behulp van intelligente aanbevelingen.

## <a name="where-do-you-create-reviews"></a>Waar ik u beoordelingen maken?

Afhankelijk van wat u bekijken wilt, maakt u de toegangsbeoordeling in Azure AD-Toegangsbeoordelingen, Azure AD enterprise-apps (in Preview-versie) of Azure AD PIM.

| Rechten van gebruikers | Revisoren kunnen zijn | Revisie is gemaakt in | Revisor-ervaring |
| --- | --- | --- | --- |
| Leden van beveiliging</br>Leden van Office | Opgegeven revisoren</br>Groepseigenaren</br>Self bekijken | Azure AD-Toegangsbeoordelingen</br>Azure AD-groepen | Toegangsvenster |
| Toegewezen aan een verbonden app | Opgegeven revisoren</br>Self bekijken | Azure AD-Toegangsbeoordelingen</br>Azure AD-enterprise-apps (in Preview-versie) | Toegangsvenster |
| Azure AD-directory-rol | Opgegeven revisoren</br>Self bekijken | Azure AD PIM | Azure Portal |
| De rol van de Azure-resource | Opgegeven revisoren</br>Self bekijken | Azure AD PIM | Azure Portal |

## <a name="prerequisites"></a>Vereisten

Voor het gebruik van toegangsbeoordelingen, moet u een van de volgende licenties hebben:

- Azure AD Premium P2
- Enterprise Mobility + Security (EMS) E5-licentie

Zie voor meer informatie, [hoe: aanmelden voor Azure Active Directory Premium](../fundamentals/active-directory-get-started-premium.md) of [Enterprise Mobility + Security E5-proefversie](http://aka.ms/emse5trial).

## <a name="get-started-with-access-reviews"></a>Aan de slag met toegangsbeoordelingen

Bekijk deze korte demo voor meer informatie over het maken en uitvoeren van beoordelingen:

>[!VIDEO https://www.youtube.com/embed/6KB3TZ8Wi40]

Als u klaar bent voor toegangsbeoordelingen in uw organisatie te implementeren, als volgt te werk in de video om te introduceren, trainen uw beheerders en uw eerste toegangsbeoordeling maken!

>[!VIDEO https://www.youtube.com/embed/X1SL2uubx9M]

## <a name="enable-access-reviews"></a>Schakel toegangsbeoordelingen in

Volg deze stappen zodat toegangsbeoordelingen.

1. Als een globale beheerder of beheerder van gebruikersaccounts, moet u zich aanmelden bij de [Azure-portal](https://portal.azure.com) beoordelingen van waar u om toegang te gebruiken.

1. Klik op **alle services** en zoek de toegang tot de service beoordeelt.

    ![Alle services - Toegangsbeoordelingen](./media/access-reviews-overview/all-services-access-reviews.png)

1. Klik op **Toegangsbeoordelingen**.

    ![Toegangsbeoordelingen onboarden](./media/access-reviews-overview/onboard-button.png)

1. Klik in de navigatielijst **Onboard** openen de **toegangsbeoordelingen Onboarden** pagina.

    ![Toegangsbeoordelingen onboarden](./media/access-reviews-overview/onboard-access-reviews.png)

1. Klik op **maken** controleert om toegang te krijgen in de huidige map. De volgende keer start van toegangsbeoordelingen, worden de opties ingeschakeld.

    ![Toegangsbeoordelingen ingeschakeld](./media/access-reviews-overview/access-reviews-enabled.png)

## <a name="next-steps"></a>Volgende stappen

- [Een toegangsbeoordeling maken voor leden van een groep of toegang tot een toepassing](create-access-review.md)
- [Een toegangsbeoordeling maken van gebruikers met een Azure AD-beheerderrol](../privileged-identity-management/pim-how-to-start-security-review.md?toc=%2fazure%2factive-directory%2fgovernance%2ftoc.json)
- [Een toegangscontrole met Azure AD-Toegangsbeoordelingen uitvoeren](perform-access-review.md)
- [Een toegangscontrole van leden van een groep of gebruikers toegang tot een toepassing in Azure AD voltooien](complete-access-review.md)
