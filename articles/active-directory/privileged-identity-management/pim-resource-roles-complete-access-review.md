---
title: Een toegangscontrole voor Azure-resources met behulp van Privileged Identity Management voltooien | Microsoft Docs
description: Beschrijft hoe u een toegangscontrole voor Azure-resources te voltooien.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: protection
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: e21d0240469a9c775e610c97f98c073b8f83ce8e
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37442188"
---
# <a name="complete-an-access-review-for-azure-resources-by-using-privileged-identity-management"></a>Een toegangscontrole voor Azure-resources met behulp van Privileged Identity Management voltooien
Beheerders met bevoegdheid kunnen bekijken bevoegde toegang na een [beveiligingsbeoordeling is gestart](pim-resource-roles-start-access-review.md). Privileged Identity Management (PIM) voor Azure-resources verzendt automatisch een e-mailbericht dat gebruikers om te controleren van hun toegang wordt gevraagd. Als een gebruiker een e-mailbericht ontvangen heeft, kunt u ze de instructies van verzenden [hoe u een beveiligingsbeoordeling uitvoeren](pim-resource-roles-perform-access-review.md).

Nadat de controleperiode voor de beveiliging is voltooid, of na alle gebruikers hebben hun eigen bekijken, volgt u de stappen in dit artikel voor het beheren van de beoordeling en bekijkt de resultaten.

## <a name="manage-security-reviews"></a>Beoordelingen voor beveiliging beheren
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



