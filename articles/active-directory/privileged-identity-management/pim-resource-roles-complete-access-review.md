---
title: Een toegangscontrole voor Azure-resource-rollen in PIM voltooien | Microsoft Docs
description: Leer hoe u een toegangscontrole voor de Azure-resource in Azure AD Privileged Identity Management (PIM) voltooien.
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
ms.openlocfilehash: 3d81dc65274600359c3d886203b067b3a90b60cf
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56160789"
---
# <a name="complete-an-access-review-for-azure-resource-roles-in-pim"></a>Een toegangscontrole voor Azure-resource-rollen in PIM voltooien
Beheerders met bevoegdheid kunnen bekijken bevoegde toegang na een [toegangsbeoordeling is gestart](pim-resource-roles-start-access-review.md). Privileged Identity Management (PIM) voor Azure-resources verzendt automatisch een e-mailbericht dat gebruikers om te controleren van hun toegang wordt gevraagd. Als een gebruiker een e-mailbericht ontvangen heeft, kunt u ze de instructies van verzenden [hoe u een toegangscontrole uitvoeren](pim-resource-roles-perform-access-review.md).

Nadat de periode van de toegangsbeoordeling is voltooid, of na alle gebruikers hebben hun eigen bekijken, volgt u de stappen in dit artikel voor het beheren van de beoordeling en bekijkt de resultaten.

## <a name="manage-access-reviews"></a>Toegangsbeoordelingen beheren
1. Ga naar de [Azure Portal](https://portal.azure.com/). Klik vervolgens op het dashboard, selecteert u de **Azure-resources** toepassing.

2. Selecteer uw resource.

3. Selecteer de **Toegangsbeoordelingen** gedeelte van het dashboard.
![Toegangsbeoordelingen](media/azure-pim-resource-rbac/rbac-access-review-home-list.png)

4. Selecteer de toegangsbeoordeling die u wilt beheren.

Op de blade details van de toegangsbeoordeling en zijn er een aantal opties voor het beheren van deze evaluatie. De opties zijn als volgt:

![Opties voor het beheren van een beoordeling](media/azure-pim-resource-rbac/rbac-access-review-menu.png)

### <a name="stop"></a>Stoppen
Alle beoordelingen voor toegang tot een einddatum hebben, maar u kunt de **stoppen** knop vroeg voltooid. Alle gebruikers die nog niet klaar met de beoordeling op dit moment niet mogelijk om deze te voltooien nadat u de beoordeling stoppen. U kunt een evaluatie kan niet opnieuw starten nadat deze gestopt.

### <a name="reset"></a>Opnieuw instellen
U kunt een toegangsbeoordeling als u wilt verwijderen van alle beslissingen die worden aangebracht op het opnieuw. Nadat u hebt een toegangsbeoordeling opnieuw ingesteld, alle gebruikers zijn gemarkeerd als niet-gecontroleerde opnieuw. 

### <a name="apply"></a>Toepassen
Wanneer een toegangsbeoordeling voltooid is, gebruikt u de **toepassen** knop voor het implementeren van de uitkomst van de beoordeling. Als een gebruiker de toegang is geweigerd in de evaluatie, wordt in deze stap de toewijzing van hun rol verwijdert.  

### <a name="delete"></a>Verwijderen
Als u worden niet meer geïnteresseerd in de beoordeling, het verwijderen. De **verwijderen** knop verwijdert u de controle van de PIM-toepassing.

## <a name="results"></a>Resultaten
Op de **resultaten** tabblad, weergeven en downloaden van een lijst van de resultaten bekijken. 
![Tabblad resultaten](media/azure-pim-resource-rbac/rbac-access-review-results.png)

## <a name="reviewers"></a>Revisor
Bekijk en revisoren toevoegen aan uw bestaande toegangsbeoordeling. Herinner revisoren hun beoordelingen te voltooien.
![Revisoren toevoegen](media/azure-pim-resource-rbac/rbac-access-review-reviewers.png)

## <a name="next-steps"></a>Volgende stappen

- [Een toegangscontrole voor Azure resource-rollen in PIM starten](pim-resource-roles-start-access-review.md)
- [Perform an access review of my Azure AD resource roles in PIM](pim-resource-roles-perform-access-review.md) (Een toegangsbeoordeling uitvoeren van mijn Azure AD-resourcerollen in PIM)
