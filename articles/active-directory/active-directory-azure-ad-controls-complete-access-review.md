---
title: Voltooien van een beoordeling van de toegang van de leden van een groep of gebruikers toegang tot een toepassing met Azure AD | Microsoft Docs
description: Informatie over het voltooien van een controle van toegang voor leden van een groep of gebruikers met toegang tot een toepassing in Azure Active Directory.
services: active-directory
documentationcenter: 
author: markwahl-msft
manager: mtillman
editor: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/19/2017
ms.author: billmath
ms.openlocfilehash: de853d633aa65c9f08f5e28088d5240c2e4d7fa6
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="complete-an-access-review-of-members-of-a-group-or-users-access-to-an-application-in-azure-ad"></a>Voltooien van een beoordeling van de toegang van de leden van een groep of gebruikers toegang tot een toepassing in Azure AD

Beheerders kunnen Azure Active Directory (Azure AD) gebruiken voor [maken een bekijken toegang](active-directory-azure-ad-controls-create-access-review.md) voor leden van de beveiligingsgroep of gebruikers die zijn toegewezen aan een toepassing. Azure AD verzendt revisoren automatisch een e-mail waarin wordt gevraagd om ze toegang te bekijken. Als een gebruiker een e-mailbericht niet hebt, kunt u ze de instructies in verzenden [Controleer uw toegang](active-directory-azure-ad-controls-perform-access-review.md). Nadat de controleperiode access voltooid is, of als een beheerder de controle van toegang stopt, de stappen in dit artikel om te bekijken en toepassen van de resultaten.

## <a name="view-an-access-review-in-the-azure-portal"></a>Een onderzoek toegang weergeven in de Azure portal

1. Ga naar de [toegang controleert pagina](https://portal.azure.com/#blade/Microsoft_AAD_ERM/DashboardBlade/), selecteer **programma's**, en selecteer het programma waarin het toegangsbeheer controleren.

2. Selecteer **beheren**, en selecteer het toegangsbeheer controleren. Als er veel besturingselementen in het programma, kunt u filteren op besturingselementen van een specifiek type en sorteren op hun status. U kunt ook zoeken door de naam van het toegangsbeheer controleren of de weergavenaam van de eigenaar op die deze is gemaakt. 

## <a name="stop-a-review-that-hasnt-finished"></a>Stoppen van een beoordeling die nog niet voltooid

Als de controle is niet de geplande einddatum bereikt, een beheerder kan selecteren **stoppen** naar de revisie voortijdig beëindigd. Na het stoppen van de evaluatie, kunnen gebruikers niet langer worden gecontroleerd. U kunt een beoordeling niet opnieuw starten nadat deze gestopt.

## <a name="apply-the-changes"></a>De wijzigingen toepassen 

Nadat een revisie access is voltooid, omdat de einddatum is bereikt of een beheerder handmatig gestopt Selecteer **toepassen**. Het resultaat van de controle wordt geïmplementeerd door het bijwerken van de groep of toepassing. Als een gebruiker toegang is geweigerd in de evaluatie, wanneer een beheerder deze optie selecteert, wordt de toewijzing van hun lidmaatschap of toepassing door Azure AD verwijderd. 

Selecteren **toepassen** heeft geen invloed op een groep die afkomstig zijn van een on-premises adreslijst of een dynamische groep. Als u wijzigen van een groep die afkomstig is van lokale wilt, de resultaten te downloaden en deze wijzigingen toepassen op de weergave van de groep in die map.

## <a name="download-the-results-of-the-review"></a>De resultaten van de evaluatie downloaden

Selecteer voor het ophalen van de resultaten van de evaluatie, **goedkeuringen** en selecteer vervolgens **downloaden**. Het resulterende CSV-bestand kan worden weergegeven in Excel of andere programma's die CSV-bestanden worden geopend.

## <a name="optional-delete-a-review"></a>Optioneel: Het verwijderen van een beoordeling
Als u niet meer geïnteresseerd in de evaluatie bent, kunt u het kunt verwijderen. Selecteer **verwijderen** verwijderen van de controle van Azure AD.

> [!IMPORTANT]
> Er is geen waarschuwing alvorens deze wordt verwijderd, dus wees zeker dat u wilt verwijderen van de controle.
> 
> 

## <a name="next-steps"></a>Volgende stappen

- [Gebruikerstoegang beheren met Azure AD-toegangsbeoordelingen](active-directory-azure-ad-controls-manage-user-access-with-access-reviews.md)
- [Gasttoegang beheren met Azure AD-toegangsbeoordelingen](active-directory-azure-ad-controls-manage-guest-access-with-access-reviews.md)
- [Programma's en controles voor Azure AD-toegangsbeoordelingen beheren](active-directory-azure-ad-controls-manage-programs-controls.md)
- [Een toegangsbeoordeling maken voor leden van een groep of toegang tot een toepassing](active-directory-azure-ad-controls-create-access-review.md)
- [Een toegangsbeoordeling maken van gebruikers met een Azure AD-beheerderrol](active-directory-privileged-identity-management-how-to-start-security-review.md)
