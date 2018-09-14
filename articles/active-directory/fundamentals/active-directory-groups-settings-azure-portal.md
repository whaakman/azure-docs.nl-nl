---
title: Het bewerken van de groepsgegevens van uw met behulp van Azure Active Directory | Microsoft Docs
description: Klik hier voor meer informatie over het bewerken van een groep gegevens met behulp van Azure Active Directory.
services: active-directory
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: conceptual
ms.date: 08/27/2018
ms.author: lizross
ms.reviewer: krbain
ms.custom: it-pro
ms.openlocfilehash: 0bac740a5da9c1e57ea1755c58579463da4a883a
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/14/2018
ms.locfileid: "45580432"
---
# <a name="how-to-edit-your-group-information-using-azure-active-directory"></a>Hoe: de groepsgegevens van uw met behulp van Azure Active Directory bewerken

Met Azure Active Directory, kunt u van een groep instellingen, waaronder het bijwerken van de naam, beschrijving of lidmaatschapstype bewerken.

## <a name="to-edit-your-group-settings"></a>Instellingen van uw groep bewerken
1. Aanmelden bij de [Azure-portal](https://portal.azure.com) met behulp van een globale beheerdersaccount voor de map.

2. Selecteer **Azure Active Directory**, en selecteer vervolgens **groepen**.

    De **groepen - alle groepen** pagina wordt weergegeven, waarin al uw actieve groepen.

3. Uit de **groepen - alle groepen** pagina, typt u zo veel mogelijk van de naam van de zoals u kunt in de **zoeken** vak. Voor de doeleinden van dit artikel, we zoeken voor de **MDM-beleid - West** groep.

    De lijst met zoekresultaten worden weergegeven onder de **zoeken** vak bijwerken terwijl u meer tekens typt.

    ![Pagina met alle groepen, met de tekst in het zoekvak zoeken](media/active-directory-groups-settings-azure-portal/search-for-specific-group.png)

4. Selecteer de groep **MDM-beleid - West**, en selecteer vervolgens **eigenschappen** uit de **beheren** gebied.

    ![Pagina overzicht met aantal en de leden en de lid-optie is gemarkeerd](media/active-directory-groups-settings-azure-portal/group-overview-blade.png)

5. Update de **algemene instellingen** gegevens naar behoefte, waaronder:

    ![Eigenschappen voor instellingen voor een groep](media/active-directory-groups-settings-azure-portal/group-properties-settings.png)

    - **Naam van de groep.** Bewerk de bestaande groepsnaam.
    
    - **De beschrijving van de groep.** De groepsbeschrijving van de bestaande bewerken.

    - **Het type groep.** U kunt het type van de groep niet wijzigen nadat deze is gemaakt. Wijzigen van de **groepstype**, moet u de groep verwijderen en een nieuwe maken.
    
    - **Lidmaatschapstype.** Het lidmaatschapstype wijzigen. Zie voor meer informatie over de verschillende beschikbare lidmaatschap typen [hoe: een eenvoudige groep maken en toevoegen van leden met behulp van de Azure Active Directory-portal](active-directory-groups-create-azure-portal.md)
    
    - **Object-ID.** U kunt de Object-ID niet wijzigen, maar u kunt deze gebruiken in uw PowerShell-opdrachten voor de groep kopiëren. Zie voor meer informatie over het gebruik van PowerShell-cmdlets [Azure Active Directory-cmdlets voor het configureren van groepsinstellingen](../users-groups-roles/groups-settings-v2-cmdlets.md).

## <a name="next-steps"></a>Volgende stappen
Deze artikelen bevatten aanvullende informatie over Azure Active Directory.

- [Uw groepen en leden weergeven](active-directory-groups-view-azure-portal.md)

- [Een basisgroep maken en leden toevoegen](active-directory-groups-create-azure-portal.md)

- [Toevoegen of verwijderen van leden uit een groep](active-directory-groups-members-azure-portal.md)

- [Dynamische regels voor gebruikers in een groep beheren](../users-groups-roles/groups-create-rule.md)

- [Lidmaatschappen van een groep beheren](active-directory-groups-membership-azure-portal.md)

- [Toegang tot resources met behulp van groepen beheren](active-directory-manage-groups.md)

- [Koppelen of een Azure-abonnement toevoegen aan Azure Active Directory](active-directory-how-subscriptions-associated-directory.md)
