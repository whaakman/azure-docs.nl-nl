---
title: Voltooi een revisie toegang voor Azure-resources met behulp van Privileged Identity Management | Microsoft Docs
description: Beschrijft hoe een revisie toegang voor Azure-resources te voltooien.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 1dd5eb4a2a0a4edd15e5299754028570806db09b
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34699451"
---
# <a name="complete-an-access-review-for-azure-resources-by-using-privileged-identity-management"></a>Voltooi een revisie toegang voor Azure-resources met behulp van Privileged Identity Management
Bevoorrechte rol beheerders kunnen bekijken voor bevoorrechte toegang na een [beveiligingsbeoordeling is gestart](pim-resource-roles-start-access-review.md). Privileged Identity Management (PIM) voor Azure-resources verzendt automatisch een e-mailbericht dat gebruikers om te controleren van de toegang wordt gevraagd. Als een gebruiker een e-mailbericht ontvangen heeft, kunt u ze verzenden de instructies voor [het uitvoeren van een beveiligingsbeoordeling](pim-resource-roles-perform-access-review.md).

Nadat de controleperiode beveiliging is voltooid, of als alle gebruikers hun eigen controleren hebt voltooid, de stappen in dit artikel voor het beheren van de controle en de resultaten te zien.

## <a name="manage-security-reviews"></a>Beoordelingen van beveiliging beheren
1. Ga naar de [Azure Portal](https://portal.azure.com/). Op het dashboard, selecteer de **Azure-resources** toepassing.

2. Selecteer de bron.

3. Selecteer de **toegang tot beoordelingen** sectie van het dashboard.
![Toegangsbeoordelingen](media/azure-pim-resource-rbac/rbac-access-review-home-list.png)

4. Selecteer de controle van toegang dat u wilt beheren.

Op de blade details van de controle van toegang en zijn er een aantal opties voor het beheren van deze evaluatie. De opties zijn als volgt:

![Opties voor het beheren van een beoordeling](media/azure-pim-resource-rbac/rbac-access-review-menu.png)

### <a name="stop"></a>Stoppen
Alle beoordelingen van toegang tot een einddatum hebben, maar u kunt de **stoppen** knop vroeg voltooid. Alle gebruikers die nog niet klaar met hun controleren op dit tijdstip niet mogelijk na het stoppen van de controle wilt voltooien. U kunt een beoordeling niet opnieuw starten nadat deze gestopt.

### <a name="reset"></a>Opnieuw instellen
U kunt een onderzoek toegang als u wilt verwijderen van alle beslissingen die zijn gemaakt herstellen. Nadat u een onderzoek toegang opnieuw instelt hebt, alle gebruikers zijn gemarkeerd als niet opnieuw verwerkt. 

### <a name="apply"></a>Toepassen
Nadat een revisie access voltooid is, gebruikt u de **toepassen** knop voor het implementeren van de uitkomst van de evaluatie. Als een gebruiker toegang is geweigerd in de evaluatie, verwijdert deze stap hun roltoewijzing.  

### <a name="delete"></a>Verwijderen
Als u worden niet meer geïnteresseerd in de evaluatie, verwijderen. De **verwijderen** knop verwijdert u de controle van de PIM-toepassing.

## <a name="results"></a>Resultaten
Op de **resultaten** tabblad, weergeven en downloaden van een lijst van de resultaten bekijken. 
![Tabblad resultaten](media/azure-pim-resource-rbac/rbac-access-review-results.png)

## <a name="reviewers"></a>Revisor
Weergeven en revisoren toevoegen aan uw bestaande toegang revisie. Herinneren revisoren hun beoordelingen voltooid.
![Revisoren toevoegen](media/azure-pim-resource-rbac/rbac-access-review-reviewers.png)



