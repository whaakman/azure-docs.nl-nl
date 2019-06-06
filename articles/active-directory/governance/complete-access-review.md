---
title: Een toegangscontrole van groepen of toepassingen - Azure Active Directory voltooien | Microsoft Docs
description: Leer hoe u een toegangscontrole van leden van beveiligingsgroep of toegang tot toepassingen in Azure Active Directory-toegangsbeoordelingen voltooien.
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
ms.date: 05/22/2019
ms.author: rolyon
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: ec3909ffbb624284f999360140b7454098643062
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2019
ms.locfileid: "66473364"
---
# <a name="complete-an-access-review-of-groups-or-applications-in-azure-ad-access-reviews"></a>Voltooien van een toegangscontrole van groepen of toepassingen in Azure AD-toegangsbeoordelingen

Als beheerder, u [maken van een toegangscontrole van groepen of toepassingen](create-access-review.md) en reviewers [uitvoeren van de toegangsbeoordeling](perform-access-review.md). In dit artikel wordt beschreven hoe u de resultaten ziet van de toegangsbeoordeling en de resultaten van toepassing.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

## <a name="prerequisites"></a>Vereisten

- Azure AD Premium P2
- Globale beheerder, beheerder, beveiligingsbeheerder of beveiligingslezer

Zie voor meer informatie, [welke gebruikers moeten beschikken over licenties?](access-reviews-overview.md#which-users-must-have-licenses).

## <a name="view-an-access-review"></a>Een toegangsbeoordeling weergeven

U kunt de voortgang volgen de revisoren hun beoordelingen zijn voltooid.

1. Aanmelden bij Azure portal en open de [Identiteitsbestuur pagina](https://portal.azure.com/#blade/Microsoft_AAD_ERM/DashboardBlade/).

1. Klik in het menu links op **Toegangsbeoordelingen**.

1. In de lijst, klikt u op een toegangscontrole.

    Beoordelingen, gaat u naar de toegangsbeoordeling om weer te geven van een reeks van toegang en vindt u toekomstige voorvallen in geplande beoordelingen.

    Op de **overzicht** pagina, kunt u de voortgang bekijken. Er is geen toegangsrechten worden gewijzigd in de map totdat de controle is voltooid.

    ![Toegangsbeoordelingen wordt uitgevoerd](./media/complete-access-review/overview-progress.png)

1. Als u wilt een toegangsbeoordeling stoppen voordat het de geplande einddatum is bereikt, klikt u op de **stoppen** knop.

    Wanneer een beoordeling stoppen, revisoren niet langer antwoorden meer geven. U kunt een beoordeling kan niet opnieuw starten nadat deze gestopt.

1. Als u niet meer geïnteresseerd in de toegangsbeoordeling bent, kunt u deze verwijderen door te klikken op de **verwijderen** knop.

## <a name="apply-the-changes"></a>De wijzigingen toepassen

Als **automatisch resultaten toepassen op resource** is ingeschakeld en op basis van uw selecties in **na voltooiing van de instellingen**, auto-toepassing zal worden uitgevoerd na de einddatum van de beoordeling of wanneer u de beoordeling handmatig stoppen.

Als **automatisch resultaten toepassen op resource** is niet ingeschakeld voor de evaluatie, klikt u op **toepassen** de wijzigingen handmatig toepassen. Als een gebruiker toegang is geweigerd in de evaluatie, wanneer u klikt op **toepassen**, Azure AD wordt de toewijzing van hun lidmaatschap of uw toepassing verwijderd.

![Toegang controleren wijzigingen toepassen](./media/complete-access-review/apply-changes.png)

De status van de beoordeling wordt gewijzigd van **voltooid** via tussenliggende Staten zoals **toepassen** en tot slot naar status **het resultaat is toegepast**. U kunt verwachten om te zien van geweigerde gebruikers, indien van toepassing, wordt verwijderd uit de groepstoewijzing voor lidmaatschap of uw toepassing in een paar minuten.

Een geconfigureerde automatisch toepassen van controleren of het selecteren van **toepassen** niet van invloed op een groep die afkomstig zijn van een on-premises directory of een dynamische groep hebt. Als u wijzigen van een groep die is afkomstig van wilt on-premises, de resultaten downloaden en deze wijzigingen van toepassing op de weergave van de groep in die map.

## <a name="retrieve-the-results"></a>De resultaten ophalen

De resultaten voor een eenmalige toegangsbeoordeling weergeven, klikt u op de **resultaten** pagina. Als u alleen de toegang van gebruikers, in het zoekvak, typt u de weergavenaam of UPN van een gebruiker die toegang is gecontroleerd.

![Resultaten voor een overzicht van access ophalen](./media/complete-access-review/retrieve-results.png)

Als u de voortgang van een actieve toegangsbeoordeling die wordt herhaald, klikt u op de **resultaten** pagina.

U kunt de resultaten van een voltooide exemplaar van een toegangscontrole die wordt herhaald, klikt u op **geschiedenis bekijken**en selecteer vervolgens het specifieke exemplaar in de lijst van voltooide exemplaren van toegangsbeoordeling, op basis van het exemplaar van de begin- en einddatum. De resultaten van dit exemplaar kunnen worden opgehaald uit de **resultaten** pagina.

Als u wilt ophalen van de resultaten van een toegangscontrole, klikt u op de **downloaden** knop. Het resulterende CSV-bestand kan worden weergegeven in Excel of in andere programma's die UTF-8-codering CSV-bestanden.

## <a name="remove-users-from-an-access-review"></a>Gebruikers verwijderen uit een toegangsbeoordeling

 Standaard geldt dat een verwijderde gebruiker gedurende dertig dagen in Azure AD aanwezig blijft met de status Verwijderd. In deze periode kan de gebruiker eventueel door een beheerder worden teruggezet.  Na dertig dagen wordt die gebruiker definitief verwijderd.  Daarnaast kan een hoofdbeheerder via de Azure Active Directory-portal expliciet [een recentelijk verwijderde gebruiker definitief verwijderen](../fundamentals/active-directory-users-restore.md) voordat de dertig dagen zijn verstreken.  Als een gebruiker definitief is verwijderd, worden gegevens over die gebruiker vervolgens uit actieve toegangsbeoordelingen verwijderd.  Controle-informatie over verwijderde gebruikers blijft in het auditlogboek aanwezig.

## <a name="next-steps"></a>Volgende stappen

- [Gebruikerstoegang beheren met Azure AD-toegangsbeoordelingen](manage-user-access-with-access-reviews.md)
- [Gasttoegang beheren met Azure AD-toegangsbeoordelingen](manage-guest-access-with-access-reviews.md)
- [Een toegangsbeoordeling van groepen of toepassingen maken](create-access-review.md)
- [Een toegangsbeoordeling maken van gebruikers met een Azure AD-beheerderrol](../privileged-identity-management/pim-how-to-start-security-review.md)
