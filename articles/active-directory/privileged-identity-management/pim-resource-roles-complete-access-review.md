---
title: Een toegangscontrole van Azure-resource-rollen in PIM - Azure Active Directory voltooien | Microsoft Docs
description: Leer hoe u een toegangscontrole van Azure-resource-rollen in Azure AD Privileged Identity Management (PIM) voltooien.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: pim
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9903bb82a82291febf571829fb9874ba66d2eab2
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2019
ms.locfileid: "67476365"
---
# <a name="complete-an-access-review-of-azure-resource-roles-in-pim"></a>Een toegangscontrole van Azure-resource-rollen in PIM voltooien
Beheerders met bevoegdheid kunnen bekijken bevoegde toegang na een [toegangsbeoordeling is gestart](pim-resource-roles-start-access-review.md). Azure Active Directory (Azure AD) Privileged Identity Management (PIM) verzendt automatisch een e-mailbericht dat gebruikers om te controleren van hun toegang wordt gevraagd. Als een gebruiker een e-mailbericht ontvangen heeft, kunt u ze de instructies van verzenden [hoe u een toegangscontrole uitvoeren](pim-resource-roles-perform-access-review.md).

Nadat de periode van de toegangsbeoordeling is voltooid, of na alle gebruikers hebben hun eigen bekijken, volgt u de stappen in dit artikel voor het beheren van de beoordeling en bekijkt de resultaten.

## <a name="manage-access-reviews"></a>Toegangsbeoordelingen beheren
1. Ga naar de [Azure Portal](https://portal.azure.com/). Klik vervolgens op het dashboard, selecteert u de **Azure-resources** toepassing.

2. Selecteer uw resource.

3. Selecteer de **Toegangsbeoordelingen** gedeelte van het dashboard.

    ![Azure-resources - toegangsbeoordelingen lijst tonen rol, eigenaar, begindatum, einddatum en -status](media/pim-resource-roles-complete-access-review/rbac-access-review-home-list.png)

4. Selecteer de toegangsbeoordeling die u wilt beheren.

Op de blade details van de toegangsbeoordeling en zijn er een aantal opties voor het beheren van deze evaluatie. De opties zijn als volgt:

![Opties voor het beheren van een evaluatie - stoppen, opnieuw instellen, toepassen, verwijderen](media/pim-resource-roles-complete-access-review/rbac-access-review-menu.png)

### <a name="stop"></a>Stoppen
Alle beoordelingen voor toegang tot een einddatum hebben, maar u kunt de **stoppen** knop vroeg voltooid. Alle gebruikers die nog niet klaar met de beoordeling op dit moment niet mogelijk om deze te voltooien nadat u de beoordeling stoppen. U kunt een evaluatie kan niet opnieuw starten nadat deze gestopt.

### <a name="reset"></a>Opnieuw instellen
U kunt een toegangsbeoordeling als u wilt verwijderen van alle beslissingen die worden aangebracht op het opnieuw. Nadat u hebt een toegangsbeoordeling opnieuw ingesteld, alle gebruikers zijn gemarkeerd als niet-gecontroleerde opnieuw. 

### <a name="apply"></a>Toepassen
Wanneer een toegangsbeoordeling voltooid is, gebruikt u de **toepassen** knop voor het implementeren van de uitkomst van de beoordeling. Als een gebruiker de toegang is geweigerd in de evaluatie, wordt in deze stap de toewijzing van hun rol verwijdert.  

### <a name="delete"></a>Verwijderen
Als u worden niet meer geïnteresseerd in de beoordeling, het verwijderen. De **verwijderen** knop verwijdert u de controle van de PIM-toepassing.

## <a name="results"></a>Resultaten
Op de **resultaten** pagina, weergeven en downloaden van een lijst van de resultaten bekijken. 

![Pagina met resultaten met gebruikers, resultaat, om die reden worden bekeken door, toegepast door en resultaat toepassen](media/pim-resource-roles-complete-access-review/rbac-access-review-results.png)

## <a name="reviewers"></a>Revisoren
Bekijk en revisoren toevoegen aan uw bestaande toegangsbeoordeling. Herinner revisoren hun beoordelingen te voltooien.

![Revisoren pagina weergegeven met een naam en het UPN-naam](media/pim-resource-roles-complete-access-review/rbac-access-review-reviewers.png)

## <a name="next-steps"></a>Volgende stappen

- [Een toegangsbeoordeling starten voor mijn Azure AD-resourcerollen in PIM](pim-resource-roles-start-access-review.md)
- [Perform an access review of my Azure AD resource roles in PIM](pim-resource-roles-perform-access-review.md) (Een toegangsbeoordeling uitvoeren van mijn Azure AD-resourcerollen in PIM)
