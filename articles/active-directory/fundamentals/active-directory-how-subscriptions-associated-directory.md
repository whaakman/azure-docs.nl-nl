---
title: Een bestaande Azure-abonnement toevoegen aan uw tenant - Azure Active Directory | Microsoft Docs
description: Instructies over hoe u een bestaande Azure-abonnement toevoegen aan uw Azure Active Directory-tenant.
services: active-directory
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: lizross
ms.reviewer: jeffsta
ms.custom: it-pro, seodec18
ms.openlocfilehash: 4e4d4d9f34c8220ba70c64f073557f0293881c3e
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2018
ms.locfileid: "53083866"
---
# <a name="associate-or-add-an-azure-subscription-to-your-azure-active-directory-tenant"></a>Koppelen of een Azure-abonnement toevoegen aan uw Azure Active Directory-tenant
Het Azure-abonnement heeft een vertrouwensrelatie met Azure Active Directory (Azure AD), wat betekent dat het abonnement Azure AD om te verifiëren van gebruikers, services en apparaten vertrouwt. Meerdere abonnementen kunnen dezelfde Azure AD-directory vertrouwen, maar elk abonnement kan alleen een één directory vertrouwen.

Als uw abonnement is verlopen, verliest u toegang tot alle andere resources die zijn gekoppeld aan het abonnement. De Azure AD-directory blijft echter in Azure, zodat u kunt koppelen en beheren van de directory met behulp van een ander Azure-abonnement.

Alle gebruikers hebben één "" basisdirectory voor verificatie. Uw gebruikers kunnen echter ook gasten zijn in andere directory's zijn. U ziet de startpagina en Gast mappen voor elke gebruiker in Azure AD.

>[!Important]
>Alle [Role-Based Access Control (RBAC)](../../role-based-access-control/role-assignments-portal.md) gebruikers met toegewezen toegang, samen met alle abonnementsbeheerders toegang nadat het abonnement gaan directorywijzigingen verloren. Bovendien hebt u een sleutelkluizen, moeten ze ook worden beïnvloed door het verplaatsen van het abonnement. Om op te lossen die, moet u [wijzigen van de tenant-ID voor key vault](../../key-vault/key-vault-subscription-move-fix.md) voordat bewerkingen wordt hervat.


## <a name="before-you-begin"></a>Voordat u begint
Voordat u kunt koppelen of uw abonnement toevoegt, moet u de volgende taken uitvoeren:

- Meld u aan met een account dat:
    - Heeft **RBAC-eigenaar** toegang tot het abonnement.

    - Er bestaat in zowel de huidige map die is gekoppeld aan het abonnement en de nieuwe map die is waar u het abonnement voortaan koppelen. Zie voor meer informatie over het verkrijgen van toegang tot een andere map [hoe voeg door Azure Active Directory-beheerders de gebruikers van B2B-samenwerking toe?](../b2b/add-users-administrator.md).

- Zorg ervoor dat u niet een Azure Cloud Service Providers (CSP)-abonnement (MS-AZR - 0145P, MS - AZR - 0146P, MS - AZR - 159P), een abonnement op Microsoft Internal (MS-AZR - 0015P) of een Microsoft Imagine-abonnement (MS-AZR - 0144P).
    
## <a name="to-associate-an-existing-subscription-to-your-azure-ad-directory"></a>Een bestaand abonnement koppelen aan uw Azure AD Directory
1. Meld u aan en selecteer het abonnement dat u gebruiken wilt in de [pagina met abonnementen in Azure portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).

2. Selecteer **Wijzig de map**.

    ![De pagina abonnementen, met de wijziging directory optie gemarkeerd](media/active-directory-how-subscriptions-associated-directory/change-directory-button.png)

3. Bekijk eventuele waarschuwingen die worden weergegeven, en selecteer vervolgens **wijziging**.

    ![Wijzig de directory-pagina met de map wijzigen](media/active-directory-how-subscriptions-associated-directory/edit-directory-ui.png)

    De map wordt gewijzigd voor het abonnement en u krijgt een bericht weergegeven.

    ![Geslaagd](media/active-directory-how-subscriptions-associated-directory/edit-directory-success.png)    

4. Gebruik de directorywisselaar om naar de nieuwe adreslijst te gaan. Het kan maximaal 10 minuten duren voordat alles goed wordt weergegeven.

    ![Overschakelen naar directorypagina](media/active-directory-how-subscriptions-associated-directory/directory-switcher.png)

Wijzigen van de abonnementsdirectory is een bewerking op serviceniveau, zodat deze niet van invloed op abonnement eigendom. De accountbeheerder kan nog steeds wijzigen voor de servicebeheerder van de [Accountcentrum](https://account.azure.com/subscriptions). Als de oorspronkelijke map verwijderen, moet u overbrengen van de abonnementsfacturering over aan een nieuwe accountbeheerder. Zie [Eigendom van een Azure-abonnement naar een ander account overdragen](../../billing/billing-subscription-transfer.md) voor meer informatie over het overdragen van het eigendom. 

## <a name="next-steps"></a>Volgende stappen

- Het maken van een nieuwe Azure AD-tenant, Zie [toegang tot Azure Active Directory om een nieuwe tenant maken](active-directory-access-create-new-tenant.md)

- Als u meer wilt weten over hoe de toegang tot resources wordt beheerd in Microsoft Azure, ziet u [Inzicht krijgen in toegang tot resources in Azure](../../role-based-access-control/rbac-and-directory-admin-roles.md)

- Zie voor meer informatie over het toewijzen van rollen in Azure AD, [directory-rollen toewijzen aan gebruikers met Azure Active Directory](active-directory-users-assign-role-azure-portal.md)
