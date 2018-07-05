---
title: Resultaten van Azure AD-toegangsbeoordeling ophalen | Microsoft Docs
description: Hoe u de resultaten van Azure Active Directory-toegangsbeoordelingen ophaalt.
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
ms.component: compliance-reports
ms.date: 06/21/2018
ms.author: rolyon
ms.reviewer: mwahl
ms.openlocfilehash: 1c1488b2744c25e9b7a191d208cb3f81d336bd3a
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37446687"
---
# <a name="retrieve-access-review-results"></a>Resultaten van toegangsbeoordeling ophalen

Beheerders kunnen Azure Active Directory (Azure AD) gebruiken om een [toegangsbeoordeling](active-directory-azure-ad-controls-create-access-review.md) te maken voor groepsleden of gebruikers die zijn toegewezen aan een toepassing.  Een gebruiker met de rol **Globale beheerder**, **Gebruikersaccountbeheerder**, **Beveiligingsbeheerder** of **Beveiligingslezer** kan de resultaten van een toegangsbeoordeling ook lezen.  Om gebruikers toe te wijzen aan een van deze rollen, kan een Beheerder met bevoorrechte rol Azure AD PIM gebruiken om een ​​gebruiker in aanmerking te laten komen voor het activeren van de rol, of kan een Globale beheerder [een gebruiker permanent toewijzen aan de rol](fundamentals/active-directory-users-assign-role-azure-portal.md).

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="locating-an-access-review"></a>Een toegangsbeoordeling vinden

Als u weet welk programma de toegangsbeoordeling bevat, gaat u naar de pagina met toegangsbeoordelingen, selecteert u **Programma's**en selecteert u het programma dat het besturingselement voor toegangsbeoordeling bevat.  Selecteer vervolgens **Besturingselementen**, en selecteer het besturingselement voor toegangsbeoordeling. Als het programma veel besturingselementen bevat, kunt u filteren op besturingselementen van een specifiek type en deze sorteren op status. U kunt ook zoeken op de naam van het besturingselement voor toegangsbeoordeling of op de weergavenaam van de eigenaar die het heeft gemaakt. 

Als u niet weet welk programma de toegangsbeoordeling bevat, gaat u naar de pagina met toegangsbeoordelingen en selecteert u **Besturingselementen** .  U kunt filteren op besturingselementen van een specifiek type en deze sorteren op status, en u kunt ook zoeken op basis van de naam van het besturingselement voor toegangsbeoordeling of op de weergavenaam van de eigenaar die het heeft gemaakt. 

## <a name="retrieving-the-results-for-a-one-time-access-review"></a>De resultaten voor een eenmalige toegangsbeoordeling ophalen

Als het herhalingstype van beoordeling Eenmalig is, kunt u de voortgang van een actieve toegangsbeoordeling en de resultaten van een voltooide toegangsbeoordeling ophalen uit de sectie **Resultaten**.  U kunt de weergavenaam of user principal name van een gebruiker wiens toegang wordt beoordeeld typen om alleen de toegang van die gebruiker te bekijken.  Als u alle resultaten van een voltooide toegangsbeoordeling wilt ophalen, klikt u op de knop **Downloaden**.

## <a name="retrieving-the-results-for-multiple-instances-of-a-recurring-access-review"></a>De resultaten voor meerdere exemplaren van een terugkerende toegangsbeoordeling ophalen

Als u de voortgang van een terugkerende actieve toegangsbeoordeling wilt bekijken, klikt u op de sectie **Resultaten**.  U kunt de weergavenaam of user principal name typen van een gebruiker wiens toegang wordt beoordeeld.

Als u de resultaten van een voltooid exemplaar van een terugkerende toegangsbeoordeling wilt bekijken, selecteert u **Beoordelingsgeschiedenis** en selecteert u vervolgens het specifieke exemplaar in de lijst met voltooide toegangsbeoordelingen, op basis van de begin- en einddatum van het exemplaar.   De resultaten van dit exemplaar kunnen worden opgehaald uit de sectie **Resultaten**.  U kunt de weergavenaam of user principal name van een gebruiker wiens toegang wordt beoordeeld typen om alleen de toegang van die gebruiker te bekijken.  Als u alle resultaten van een voltooid exemplaar van een terugkerende toegangsbeoordeling wilt ophalen, klikt u op de knop **Downloaden**.


## <a name="removing-users-from-an-access-review"></a>Gebruikers verwijderen uit een toegangsbeoordeling

Standaard geldt dat een verwijderde gebruiker gedurende dertig dagen in Azure AD aanwezig blijft met de status Verwijderd. In deze periode kan de gebruiker eventueel door een beheerder worden teruggezet.  Na dertig dagen wordt die gebruiker definitief verwijderd.  Daarnaast kan een hoofdbeheerder via de Azure Active Directory-portal expliciet [een recentelijk verwijderde gebruiker definitief verwijderen](fundamentals/active-directory-users-restore.md) voordat de dertig dagen zijn verstreken.  Als een gebruiker definitief is verwijderd, worden gegevens over die gebruiker vervolgens uit actieve toegangsbeoordelingen verwijderd.  Controle-informatie over verwijderde gebruikers blijft in het auditlogboek aanwezig.

## <a name="next-steps"></a>Volgende stappen

- [Gebruikerstoegang beheren met Azure AD-toegangsbeoordelingen](active-directory-azure-ad-controls-manage-user-access-with-access-reviews.md)
- [Gasttoegang beheren met Azure AD-toegangsbeoordelingen](active-directory-azure-ad-controls-manage-guest-access-with-access-reviews.md)
- [Programma's en controles voor Azure AD-toegangsbeoordelingen beheren](active-directory-azure-ad-controls-manage-programs-controls.md)
- [Een toegangsbeoordeling maken voor leden van een groep of toegang tot een toepassing](active-directory-azure-ad-controls-create-access-review.md)
- [Een toegangsbeoordeling maken van gebruikers met een Azure AD-beheerderrol](active-directory-privileged-identity-management-how-to-start-security-review.md)


