---
title: Toevoegen of een groep verwijderen uit een andere groep in Azure Active Directory | Microsoft Docs
description: Informatie over het toevoegen of verwijderen van een groep uit een andere groep met behulp van Azure Active Directory.
services: active-directory
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: conceptual
ms.date: 08/28/2018
ms.author: lizross
ms.custom: it-pro
ms.reviewer: krbain
ms.openlocfilehash: c28fe5ef226fac993fde221b16bfa875ba4845ca
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/14/2018
ms.locfileid: "45579765"
---
# <a name="how-to-add-or-remove-a-group-from-another-group-using-azure-active-directory"></a>Hoe: toevoegen of verwijderen van een groep uit een andere groep met behulp van Azure Active Directory
In dit artikel helpt u bij het toevoegen en verwijderen van een groep van een andere groep met behulp van Azure Active Directory.

>[!Note]
>Als u probeert te verwijderen van de bovenliggende groep, Zie [bijwerken of verwijderen van een groep en de bijbehorende leden](active-directory-groups-delete-group.md).

## <a name="add-a-group-as-a-member-to-another-group"></a>Een groep als een lid toevoegen aan een andere groep
U kunt een bestaande groep toevoegen aan een andere bestaande groep, het maken van een lid van groep (subgroep) en een bovenliggende groep. De lid-groep neemt de kenmerken en eigenschappen van de bovenliggende groep, zodat u de configuratie.

### <a name="to-add-a-group-as-a-member-to-another-group"></a>Een groep als een lid toevoegen aan een andere groep

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) met behulp van een globale beheerdersaccount voor de map.

2. Selecteer **Azure Active Directory**, en selecteer vervolgens **groepen**.

3. Op de **groepen - alle groepen** pagina, zoek en selecteer de groep die is lid van een andere groep. Voor deze oefening, gebruiken we de **MDM-beleid - West** groep.

    >[!Note]
    >U kunt uw groep als een lid toevoegen aan slechts één groep tegelijk. Bovendien de **groep selecteren** vak filteren op basis van die overeenkomen met uw inzending op een deel van de naam van een gebruiker of het apparaat weergegeven. Echter worden niet jokertekens ondersteund.

    ![Groepen - alle groepen pagina met beleid voor MDM - West groep geselecteerd](media/active-directory-groups-membership-azure-portal/group-all-groups-screen.png)

4. Op de **MDM - West - beleid groepslidmaatschappen** weergeeft, schakelt **groepslidmaatschappen**, selecteer **toevoegen**, zoek de groep die u wilt dat uw groep lid zijn van en kies vervolgens  **Selecteer**. Voor deze oefening, gebruiken we de **MDM-beleid - alle org** groep.

    De **MDM-beleid - West** groep is nu lid van de **MDM-beleid - alle org** groep, nemen alle eigenschappen en de configuratie van het beleid van de MDM - groep met alle org.

    ![Lidmaatschap van een door de groep toe te voegen aan een andere groep maken](media/active-directory-groups-membership-azure-portal/add-group-membership.png)

5. Controleer de **groepslidmaatschappen van MDM - West - beleid** pagina om te bekijken van de groep en lid relatie.

    ![MDM - West - groepslidmaatschappen beleidspagina met de bovenliggende groep](media/active-directory-groups-membership-azure-portal/group-membership-blade.png)

6. Voor een gedetailleerdere weergave van de groep en lid relatie, selecteer de naam van de (**MDM-beleid - alle org**) en bekijk de **MDM-beleid - West** pagina details.

    ![Pagina van het lidmaatschap van zowel het lid en de Groepsdetails](media/active-directory-groups-membership-azure-portal/group-membership-review.png)

## <a name="remove-a-member-group-from-another-group"></a>Een groep lid verwijderen uit een andere groep
U kunt een bestaande groep voor lid verwijderen uit een andere groep. Echter, het lidmaatschap ook verwijdert alle overgenomen kenmerken en de eigenschappen voor uw gebruikers.

### <a name="to-remove-a-member-group-from-another-group"></a>Een lid van groep te verwijderen uit een andere groep
1. Op de **groepen - alle groepen** pagina, zoek en selecteer de groep die moet worden verwijderd als een lid van een andere groep. Voor deze oefening opnieuw gebruiken we de **MDM-beleid - West** groep.

2. Op de **MDM-beleid - West-overzicht** weergeeft, schakelt **groepslidmaatschappen**.

    ![Beleid voor MDM - West-overzichtspagina](media/active-directory-groups-membership-azure-portal/group-membership-overview.png)

3. Selecteer de **MDM-beleid - alle org** groep uit de **groepslidmaatschappen van MDM - West - beleid** pagina en selecteer vervolgens **verwijderen** van de **MDM-beleid - West** pagina details.

    ![Pagina van het lidmaatschap van zowel het lid en de Groepsdetails](media/active-directory-groups-membership-azure-portal/group-membership-remove.png)


## <a name="additional-information"></a>Aanvullende informatie
Deze artikelen bevatten aanvullende informatie over Azure Active Directory.

- [Uw groepen en leden weergeven](active-directory-groups-view-azure-portal.md)

- [Een basisgroep maken en leden toevoegen](active-directory-groups-create-azure-portal.md)

- [Leden toevoegen of verwijderen uit een groep](active-directory-groups-members-azure-portal.md)

- [Instellingen van uw groep bewerken](active-directory-groups-settings-azure-portal.md)

- [Licenties toewijzen aan gebruikers per groep](../users-groups-roles/licensing-groups-assign.md)
