---
title: Een bestaande Azure-abonnement toevoegen aan uw tenant - Azure Active Directory | Microsoft Docs
description: Instructies over hoe u een bestaande Azure-abonnement toevoegen aan uw Azure Active Directory-tenant.
services: active-directory
author: eross-msft
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 03/13/2019
ms.author: lizross
ms.reviewer: jeffsta
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 262f46b3d0b61923352f3ff98b08cb5e4f6d5e75
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57993901"
---
# <a name="associate-or-add-an-azure-subscription-to-your-azure-active-directory-tenant"></a>Koppelen of een Azure-abonnement toevoegen aan uw Azure Active Directory-tenant

Een Azure-abonnement heeft een vertrouwensrelatie met Azure Active Directory (Azure AD), wat betekent dat het abonnement Azure AD om te verifiëren van gebruikers, services en apparaten vertrouwt. Meerdere abonnementen kunnen dezelfde Azure AD-directory vertrouwen, maar elk abonnement kan alleen een één directory vertrouwen.

Als uw abonnement is verlopen, verliest u toegang tot alle andere resources die zijn gekoppeld aan het abonnement. De Azure AD-directory blijft echter in Azure, zodat u kunt koppelen en beheren van de directory met behulp van een ander Azure-abonnement.

Alle gebruikers hebben één *home* Active directory voor verificatie. Uw gebruikers kunnen echter ook gasten zijn in andere directory's zijn. U ziet de startpagina en Gast mappen voor elke gebruiker in Azure AD.

> [!Important]
> Wanneer u een abonnement op een andere map, gebruikers die beschikken over de rol is toegewezen met behulp van koppelen [op rollen gebaseerd toegangsbeheer (RBAC)](../../role-based-access-control/role-assignments-portal.md) verliest de toegang. Klassieke abonnementsbeheerders (servicebeheerder en Medebeheerders) zal ook geen toegang meer.

## <a name="before-you-begin"></a>Voordat u begint

Voordat u kunt koppelen of uw abonnement toevoegt, moet u de volgende taken uitvoeren:

1. Bekijk de volgende lijst met wijzigingen en hoe u mogelijk worden beïnvloed:

    - Gebruikers die zijn toegewezen aan rollen met RBAC verliest de toegang
    - Servicebeheerder en Medebeheerders verliest toegang
    - Als u alle sleutelkluizen hebt, ze niet toegankelijk zijn en u hebt deze na de koppeling oplossen
    - Als u een geregistreerde Azure Stack hebt, hebt u opnieuw registreren na de koppeling

1. Meld u aan met een account dat:
    - Heeft een [eigenaar](../../role-based-access-control/built-in-roles.md#owner) roltoewijzing voor het abonnement. Zie voor meer informatie over het toewijzen van de rol van eigenaar [beheren van toegang tot Azure-resources met behulp van RBAC en de Azure-portal](../../role-based-access-control/role-assignments-portal.md).
    - Er bestaat in zowel de huidige map die is gekoppeld aan het abonnement en de nieuwe map die is waar u het abonnement voortaan koppelen. Zie voor meer informatie over het verkrijgen van toegang tot een andere map [hoe voeg door Azure Active Directory-beheerders de gebruikers van B2B-samenwerking toe?](../b2b/add-users-administrator.md).

1. Zorg ervoor dat u niet een Azure Cloud Service Providers (CSP)-abonnement (MS-AZR - 0145P, MS - AZR - 0146P, MS - AZR - 159P), een abonnement op Microsoft Internal (MS-AZR - 0015P) of een Microsoft Imagine-abonnement (MS-AZR - 0144P).
    
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

## <a name="post-association-steps"></a>Stappen na-koppeling

Nadat u een abonnement op een andere map hebt gekoppeld, is er mogelijk extra stappen die u uitvoeren moet om bewerkingen te hervatten.

1. Als u alle sleutelkluizen hebt, moet u een tenant-id in de vorm van de sleutelkluis. Zie voor meer informatie, [een tenant-ID van key vault wijzigen na een verplaatsing van een abonnement](../../key-vault/key-vault-subscription-move-fix.md).

1. Als u een Azure-Stack met behulp van dit abonnement hebt geregistreerd, moet u opnieuw registreren. Zie voor meer informatie, [Azure Stack registreren met Azure](../../azure-stack/azure-stack-registration.md).

## <a name="next-steps"></a>Volgende stappen

- Het maken van een nieuwe Azure AD-tenant, Zie [toegang tot Azure Active Directory om een nieuwe tenant maken](active-directory-access-create-new-tenant.md)

- Als u meer wilt weten over hoe de toegang tot resources wordt beheerd in Microsoft Azure, ziet u [Inzicht krijgen in toegang tot resources in Azure](../../role-based-access-control/rbac-and-directory-admin-roles.md)

- Zie voor meer informatie over het toewijzen van rollen in Azure AD, [directory-rollen toewijzen aan gebruikers met Azure Active Directory](active-directory-users-assign-role-azure-portal.md)
