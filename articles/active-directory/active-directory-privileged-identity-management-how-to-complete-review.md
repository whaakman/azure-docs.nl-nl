---
title: Een toegangscontrole voltooien | Microsoft Docs
description: Nadat u een toegangsbeoordeling in Azure AD Privileged Identity Management starten, informatie over het voltooien en de resultaten bekijken
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: protection
ms.date: 06/06/2017
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: fc6376a7bf1abd81e7a5ff17549a7b4a193edec4
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37950577"
---
# <a name="how-to-complete-an-access-review-in-azure-ad-privileged-identity-management"></a>Het uitvoeren van een toegangscontrole in Azure AD Privileged Identity Management
Beheerders met bevoegdheid kunnen eenmaal bevoegde toegang beoordelen een [beveiligingsbeoordeling is gestart](active-directory-privileged-identity-management-how-to-start-security-review.md). Azure AD Privileged Identity Management (PIM) ontvangt automatisch een e-mailbericht waarin wordt gevraagd de gebruikers kunnen hun toegang beoordelen. Als een gebruiker niet een e-mailbericht ontvangt is, kunt u ze de instructies in verzenden [hoe u een beveiligingsbeoordeling uitvoeren](active-directory-privileged-identity-management-how-to-perform-security-review.md).

Nadat de periode van beveiliging controleren of alle gebruikers hun zelf bekijken hebt, volgt u de stappen in dit artikel voor het beheren van de beoordeling en bekijkt de resultaten.

## <a name="manage-security-reviews"></a>Beoordelingen voor beveiliging beheren
1. Ga naar de [Azure-portal](https://portal.azure.com/) en selecteer de **Azure AD Privileged Identity Management** toepassingen op uw dashboard.
2. Selecteer de **Toegangsbeoordelingen** gedeelte van het dashboard.
3. Selecteer de toegangsbeoordeling die u wilt beheren.

Op de blade details van de toegangsbeoordeling zijn er een aantal opties voor het beheren van deze evaluatie.

![PIM-knoppen voor het controleren van toegang - schermafbeelding](./media/active-directory-privileged-identity-management-how-to-complete-review/PIM_review_buttons.png)

### <a name="remind"></a>Herinneren
Als een toegangsbeoordeling is ingesteld zodat de gebruikers zelf, Controleer de **herinnering sturen** knop verstuurt een melding. 

### <a name="stop"></a>Stoppen
Alle beoordelingen voor toegang tot een einddatum hebben, maar u kunt de **stoppen** knop vroeg voltooid. Als gebruikers nog niet hebt op dit moment zijn gecontroleerd, ze niet mogelijk om te na het stoppen van de beoordeling. U kunt een evaluatie kan niet opnieuw starten nadat deze gestopt.

### <a name="apply"></a>Toepassen
Wanneer een toegangsbeoordeling is voltooid, omdat u de einddatum is bereikt of handmatig gestopt omdat de **toepassen** knop implementeert de uitkomst van de beoordeling. Als een gebruiker de toegang is geweigerd in de evaluatie, is dit de stap die hun roltoewijzing wordt verwijderd.  

### <a name="export"></a>Exporteren
Als u de resultaten van de beveiligingsbeoordeling handmatig toepassen wilt, kunt u de evaluatie exporteren. De **exporteren** knop downloaden van een CSV-bestand wordt gestart. U kunt de resultaten in Excel of andere programma's die CSV-bestanden open beheren.

### <a name="delete"></a>Verwijderen
Als u niet geïnteresseerd in de beoordeling verdere bent, het verwijderen. De **verwijderen** knop verwijdert u de controle van de PIM-toepassing.

> [!IMPORTANT]
> U wordt niet ontvangt een waarschuwing voordat de verwijdering plaatsvindt, dus wees zeker dat u wilt verwijderen van deze evaluatie. 

## <a name="next-steps"></a>Volgende stappen
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]
