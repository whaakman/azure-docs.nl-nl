---
title: Een toegangscontrole van leden van een groep of gebruikers toegang tot een toepassing met Azure AD voltooien | Microsoft Docs
description: Leer hoe u een toegangscontrole voltooien voor leden van een groep of gebruikers met toegang tot een toepassing in Azure Active Directory.
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
ms.date: 05/02/2018
ms.author: rolyon
ms.reviewer: mwahl
ms.openlocfilehash: ce791e2b230360a04a3051a8964e39c6d5a457ab
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55191360"
---
# <a name="complete-an-access-review-of-members-of-a-group-or-users-access-to-an-application-in-azure-ad"></a>Een toegangscontrole van leden van een groep of gebruikers toegang tot een toepassing in Azure AD voltooien

Beheerders kunnen Azure Active Directory (Azure AD) gebruiken om een [toegangsbeoordeling](create-access-review.md) te maken voor groepsleden of gebruikers die zijn toegewezen aan een toepassing. Azure AD stuurt revisoren automatisch een e-mailbericht waarin wordt gevraagd om ze toegang beoordelen. Als een gebruiker een e-mailbericht niet hebt, kunt u ze de instructies in verzenden [uw toegang controleren](perform-access-review.md). (Houd er rekening mee dat gasten die zijn toegewezen als beoordelaar, maar niet de uitnodiging hebt geaccepteerd niet een e-mailbericht wordt ontvangen van toegangsbeoordelingen, als ze moeten eerst akkoord gaan met een uitnodiging voordat bekijken.) Nadat de periode van de toegangsbeoordeling voltooid is, of als een beheerder de toegangsbeoordeling stopt, volg de stappen in dit artikel om te zien en de resultaten van toepassing.

## <a name="view-an-access-review-in-the-azure-portal"></a>Een toegangsbeoordeling weergeven in Azure portal

1. Ga naar de [pagina toegangsbeoordelingen](https://portal.azure.com/#blade/Microsoft_AAD_ERM/DashboardBlade/), selecteer **programma's**, en selecteer het programma dat het toegangsbeheer voor de beoordeling bevat.

2. Selecteer **beheren**, en selecteer het toegangsbeheer voor controle. Als het programma veel besturingselementen bevat, kunt u filteren op besturingselementen van een specifiek type en deze sorteren op status. U kunt ook zoeken op de naam van het besturingselement voor toegangsbeoordeling of op de weergavenaam van de eigenaar die het heeft gemaakt. 

## <a name="stop-a-review-that-hasnt-finished"></a>Stoppen van een beoordeling die nog niet voltooid

Als de beoordeling nog niet de geplande einddatum valt bereikt, een beheerder kan selecteren **stoppen** de beoordeling voortijdig beëindigen. Na het stoppen van de beoordeling, kunnen gebruikers niet meer worden gecontroleerd. U kunt een beoordeling kan niet opnieuw starten nadat deze gestopt.

## <a name="apply-the-changes"></a>De wijzigingen toepassen 

Nadat een toegangsbeoordeling is voltooid, ofwel omdat de einddatum is bereikt of een beheerder handmatig gestopt en automatisch toepassen is niet geconfigureerd voor de beoordeling, kunt u selecteren **toepassen** de wijzigingen handmatig toepassen. Het resultaat van de evaluatie wordt geïmplementeerd door het bijwerken van de groep of toepassing. Als een gebruiker de toegang is geweigerd in de beoordeling wanneer een beheerder deze optie selecteert, wordt de toewijzing van hun lidmaatschap of uw toepassing verwijderd in Azure AD. 

Nadat een toegangsbeoordeling is voltooid en automatisch toepassen is geconfigureerd, wordt de status van de beoordeling wordt gewijzigd van voltooide via tussenliggende statussen en ten slotte wordt gewijzigd in staat toegepast. U kunt verwachten geweigerde gebruikers wilt weergeven als een van de resource wordt verwijderd groepstoewijzing-lidmaatschap of uw app in een paar minuten.

Een geconfigureerde automatisch toepassen van controleren of het selecteren van **toepassen** niet van invloed op een groep die afkomstig zijn van een on-premises directory of een dynamische groep hebt. Als u wijzigen van een groep die is afkomstig van wilt on-premises, de resultaten downloaden en deze wijzigingen van toepassing op de weergave van de groep in die map.

## <a name="download-the-results-of-the-review"></a>De resultaten van de evaluatie downloaden

Als u wilt ophalen van de resultaten van de evaluatie, selecteer **goedkeuringen** en selecteer vervolgens **downloaden**. Het resulterende CSV-bestand kan worden weergegeven in Excel of in andere programma's die UTF-8-codering CSV-bestanden.

## <a name="optional-delete-a-review"></a>Optioneel: Verwijderen van een beoordeling
Als u niet meer geïnteresseerd in het onderzoek bent, kunt u deze kunt verwijderen. Selecteer **verwijderen** te verwijderen van de beoordeling van Azure AD.

> [!IMPORTANT]
> Er is geen waarschuwing voordat de verwijdering plaatsvindt, dus wees zeker dat u wilt verwijderen van de beoordeling.
> 
> 

## <a name="next-steps"></a>Volgende stappen

- [Gebruikerstoegang beheren met Azure AD-toegangsbeoordelingen](manage-user-access-with-access-reviews.md)
- [Gasttoegang beheren met Azure AD-toegangsbeoordelingen](manage-guest-access-with-access-reviews.md)
- [Programma's en controles voor Azure AD-toegangsbeoordelingen beheren](manage-programs-controls.md)
- [Een toegangsbeoordeling maken voor leden van een groep of toegang tot een toepassing](create-access-review.md)
- [Een toegangsbeoordeling maken van gebruikers met een Azure AD-beheerderrol](../privileged-identity-management/pim-how-to-start-security-review.md)
