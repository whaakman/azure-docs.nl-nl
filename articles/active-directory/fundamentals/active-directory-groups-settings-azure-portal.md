---
title: Bewerk de groepsgegevens van uw - Azure Active Directory | Microsoft Docs
description: Instructies over het bewerken van uw groep informatie met behulp van Azure Active Directory.
services: active-directory
author: eross-msft
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 08/27/2018
ms.author: lizross
ms.reviewer: krbain
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 691f705574050b15869a0ac8b7d128507e5aae10
ms.sourcegitcommit: 8a59b051b283a72765e7d9ac9dd0586f37018d30
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58285629"
---
# <a name="edit-your-group-information-using-azure-active-directory"></a>Bewerk de groepsgegevens van uw met behulp van Azure Active Directory

Met behulp van Azure Active Directory (Azure AD), kunt u de instellingen van een groep, met inbegrip van het type naam, beschrijving of lidmaatschap bijwerken bewerken.

## <a name="to-edit-your-group-settings"></a>Instellingen van uw groep bewerken
1. Meld u aan bij de [Azure Portal](https://portal.azure.com) met het account van een globale administrator voor de map.

2. Selecteer **Azure Active Directory**, en selecteer vervolgens **groepen**.

    De **groepen - alle groepen** pagina wordt weergegeven, waarin al uw actieve groepen.

3. Uit de **groepen - alle groepen** pagina, typt u zo veel mogelijk van de naam van de zoals u kunt in de **zoeken** vak. Voor de doeleinden van dit artikel, we zoeken voor de **MDM-beleid - West** groep.

    De lijst met zoekresultaten worden weergegeven onder de **zoeken** vak bijwerken terwijl u meer tekens typt.

    ![Pagina met alle groepen, met de tekst in het zoekvak zoeken](media/active-directory-groups-settings-azure-portal/search-for-specific-group.png)

4. Selecteer de groep **MDM-beleid - West**, en selecteer vervolgens **eigenschappen** uit de **beheren** gebied.

    ![Overzichtspagina van de groep met de optie lid en gemarkeerde gegevens](media/active-directory-groups-settings-azure-portal/group-overview-blade.png)

5. Update de **algemene instellingen** gegevens naar behoefte, waaronder:

    ![Eigenschappen voor instellingen voor een groep](media/active-directory-groups-settings-azure-portal/group-properties-settings.png)

    - **Naam van de groep.** Bewerk de bestaande groepsnaam.
    
    - **Groepsbeschrijving.** De groepsbeschrijving van de bestaande bewerken.

    - **Het type groep.** U kunt het type van de groep niet wijzigen nadat deze is gemaakt. Wijzigen van de **groepstype**, moet u de groep verwijderen en een nieuwe maken.
    
    - **Lidmaatschapstype.** Het lidmaatschapstype wijzigen. Zie voor meer informatie over de verschillende beschikbare lidmaatschap typen [het: Een eenvoudige groep maken en toevoegen van leden met behulp van de Azure Active Directory-portal](active-directory-groups-create-azure-portal.md).
    
    - **Object-ID.** U kunt de Object-ID niet wijzigen, maar u kunt deze gebruiken in uw PowerShell-opdrachten voor de groep kopiëren. Zie voor meer informatie over het gebruik van PowerShell-cmdlets [Azure Active Directory-cmdlets voor het configureren van groepsinstellingen](../users-groups-roles/groups-settings-v2-cmdlets.md).

## <a name="next-steps"></a>Volgende stappen
Deze artikelen bevatten aanvullende informatie over Azure Active Directory.

- [Groepen en leden weergeven](active-directory-groups-view-azure-portal.md)

- [Een basisgroep maken en leden toevoegen](active-directory-groups-create-azure-portal.md)

- [Toevoegen of verwijderen van leden uit een groep](active-directory-groups-members-azure-portal.md)

- [Dynamische regels voor gebruikers in een groep beheren](../users-groups-roles/groups-create-rule.md)

- [Lidmaatschappen van een groep beheren](active-directory-groups-membership-azure-portal.md)

- [Toegang tot resources met behulp van groepen beheren](active-directory-manage-groups.md)

- [Een Azure-abonnement aan Azure Active Directory koppelen of toevoegen](active-directory-how-subscriptions-associated-directory.md)
