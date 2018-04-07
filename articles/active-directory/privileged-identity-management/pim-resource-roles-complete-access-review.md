---
title: Privileged Identity Management voor Azure-Resources - volledige toegang revisie voor Azure-Resources | Microsoft Docs
description: Beschrijft hoe een beoordeling toegang voltooien voor Azure-Resources.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/02/2018
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: 5ce02c2d27ec3de87fe44e9c904f19409600e5c5
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/06/2018
---
# <a name="privileged-identity-management---resource-role---finish-access-review"></a>Privileged Identity Management - Resource role - voltooien toegang controleren
Bevoorrechte rol beheerders kunnen bekijken voor bevoorrechte toegang eenmaal een [beveiligingsbeoordeling is gestart](pim-resource-roles-start-access-review.md). Privileged Identity Management (PIM) voor Azure-Resources worden automatisch een e-mailbericht aan gebruikers om te controleren van de toegang gevraagd verzonden. Als een gebruiker niet een e-mailbericht ontvangt is, kunt u ze de instructies in verzenden [het uitvoeren van een beveiligingsbeoordeling](pim-resource-roles-perform-access-review.md).

Nadat de periode van beveiliging controleren of alle gebruikers klaar bent met hun eigen bekijken, de stappen in dit artikel voor het beheren van de controle en de resultaten te zien.

## <a name="manage-security-reviews"></a>Beoordelingen van beveiliging beheren
1. Ga naar de [Azure-portal](https://portal.azure.com/) en selecteer de **Azure-resources** toepassing op uw dashboard.
2. Selecteer de bron.
3. Selecteer de **toegang tot beoordelingen** sectie van het dashboard.
![](media/azure-pim-resource-rbac/rbac-access-review-home-list.png)
4. Selecteer de controle van toegang dat u wilt beheren.

Op de toegang-revisie detail blade zijn er een aantal opties voor het beheren van deze evaluatie.
![](media/azure-pim-resource-rbac/rbac-access-review-menu.png)

### <a name="stop"></a>Stoppen
Alle beoordelingen van toegang tot een einddatum hebben, maar u kunt de **stoppen** knop vroeg voltooid. Als gebruikers zich nog niet op dit tijdstip zijn gecontroleerd, ze niet mogelijk om te na het stoppen van de controle. U kunt een beoordeling niet opnieuw starten nadat deze gestopt.

### <a name="reset"></a>Opnieuw instellen
U kunt een onderzoek toegang als u wilt verwijderen van alle beslissingen over het opnieuw instellen. Zodra u een onderzoek toegang opnieuw instelt hebt, alle gebruikers zijn gemarkeerd als niet opnieuw verwerkt. 

### <a name="apply"></a>Toepassen
Nadat een revisie access is voltooid, omdat u de einddatum is bereikt of handmatig gestopt omdat de **toepassen** knop implementeert de uitkomst van de evaluatie. Als een gebruiker toegang is geweigerd in de evaluatie, is dit de stap die hun roltoewijzing wordt verwijderd.  

### <a name="delete"></a>Verwijderen
Als u niet geïnteresseerd in de revisie verdere bent, verwijderen. De **verwijderen** knop verwijdert u de controle van de PIM-toepassing.

## <a name="results"></a>Resultaten
Weergeven en downloaden van een lijst van de resultaten bekijken op het tabblad resultaten. ![](media/azure-pim-resource-rbac/rbac-access-review-results.png)

## <a name="reviewers"></a>Revisors
Weergeven en revisoren toevoegen aan uw bestaande toegang revisie. Herinneren revisoren hun beoordelingen voltooid.
![](media/azure-pim-resource-rbac/rbac-access-review-reviewers.png)



