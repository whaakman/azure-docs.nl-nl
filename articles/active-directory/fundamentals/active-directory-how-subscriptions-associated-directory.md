---
title: Een bestaande Azure-abonnement toevoegen aan uw tenant - Azure Active Directory | Microsoft Docs
description: Instructies over hoe u een bestaande Azure-abonnement toevoegen aan uw Azure Active Directory-tenant.
services: active-directory
author: msaburnley
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 03/13/2019
ms.author: ajburnle
ms.reviewer: jeffsta
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: fb4fa92d8b3c174cdf9b3695f8564cc11c1ad291
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2019
ms.locfileid: "68851752"
---
# <a name="associate-or-add-an-azure-subscription-to-your-azure-active-directory-tenant"></a>Koppelen of een Azure-abonnement toevoegen aan uw Azure Active Directory-tenant

Een Azure-abonnement heeft een vertrouwens relatie met Azure Active Directory (Azure AD). Dit betekent dat het abonnement Azure AD vertrouwt voor het verifiëren van gebruikers, services en apparaten. Meerdere abonnementen kunnen dezelfde Azure AD-directory vertrouwen, maar elk abonnement kan alleen een één directory vertrouwen.

Als uw abonnement is verlopen, verliest u toegang tot alle andere resources die zijn gekoppeld aan het abonnement. De Azure AD-directory blijft echter in Azure, zodat u kunt koppelen en beheren van de directory met behulp van een ander Azure-abonnement.

Al uw gebruikers hebben één *basis* Directory voor verificatie. Uw gebruikers kunnen echter ook gasten zijn in andere directory's zijn. U ziet de startpagina en Gast mappen voor elke gebruiker in Azure AD.

> [!Important]
> Wanneer u een abonnement aan een andere map koppelt, verliezen gebruikers die rollen hebben die zijn toegewezen via [op rollen gebaseerd toegangs beheer (RBAC)](../../role-based-access-control/role-assignments-portal.md) hun toegang. Klassieke abonnements beheerders (Service beheerder en mede beheerders) hebben ook geen toegang meer.
> 
> Als u uw Azure Kubernetes service-cluster (AKS) verplaatst naar een ander abonnement of het cluster eigenaar-abonnement verplaatst naar een nieuwe Tenant, zorgt het cluster er daarom voor dat de functionaliteit verloren gaat als gevolg van kwijt raken van roltoewijzingen en machtigingen voor service-principals. Zie [Azure Kubernetes service (AKS) (Engelstalig)](https://docs.microsoft.com/azure/aks/)voor meer informatie over AKS.

## <a name="before-you-begin"></a>Voordat u begint

Voordat u kunt koppelen of uw abonnement toevoegt, moet u de volgende taken uitvoeren:

1. Bekijk de volgende lijst met wijzigingen en de manier waarop u dit kunt beïnvloeden:

    - Gebruikers aan wie rollen zijn toegewezen via RBAC, raken hun toegang kwijt
    - Service beheerder en mede beheerders hebben geen toegang meer
    - Als u sleutel kluizen hebt, zijn ze niet toegankelijk en moet u deze na de koppeling oplossen
    - Als u beheerde identiteiten hebt voor resources zoals Virtual Machines of Logic Apps, moet u deze opnieuw inschakelen of opnieuw maken na de koppeling
    - Als u een geregistreerde Azure Stack hebt, moet u deze na de koppeling opnieuw registreren

1. Meld u aan met een account dat:
    - Beschikt over [](../../role-based-access-control/built-in-roles.md#owner) een roltoewijzing voor het abonnement. Zie [toegang tot Azure-resources beheren met RBAC en de Azure Portal](../../role-based-access-control/role-assignments-portal.md)voor meer informatie over het toewijzen van de rol van eigenaar.
    - Er bestaat in zowel de huidige map die is gekoppeld aan het abonnement en de nieuwe map die is waar u het abonnement voortaan koppelen. Zie voor meer informatie over het verkrijgen van toegang tot een andere map [hoe voeg door Azure Active Directory-beheerders de gebruikers van B2B-samenwerking toe?](../b2b/add-users-administrator.md).

1. Zorg ervoor dat u niet een Azure Cloud Service Providers (CSP)-abonnement (MS-AZR - 0145P, MS - AZR - 0146P, MS - AZR - 159P), een abonnement op Microsoft Internal (MS-AZR - 0015P) of een Microsoft Imagine-abonnement (MS-AZR - 0144P).
    
## <a name="to-associate-an-existing-subscription-to-your-azure-ad-directory"></a>Een bestaand abonnement koppelen aan uw Azure AD Directory

1. Meld u aan en selecteer het abonnement dat u gebruiken wilt in de [pagina met abonnementen in Azure portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).

2. Selecteer **Wijzig de map**.

    ![De pagina abonnementen, met de wijziging directory optie gemarkeerd](media/active-directory-how-subscriptions-associated-directory/change-directory-button.png)

3. Bekijk eventuele waarschuwingen die worden weergegeven, en selecteer vervolgens **wijziging**.

    ![Wijzig de directory-pagina met de map wijzigen](media/active-directory-how-subscriptions-associated-directory/edit-directory-ui.png)

    De map wordt gewijzigd voor het abonnement en u krijgt een bericht weergegeven.

    ![Geslaagd bericht over Directory wijziging](media/active-directory-how-subscriptions-associated-directory/edit-directory-success.png)
4. Gebruik de **Directory-schakelaar** om naar uw nieuwe map te gaan. Het kan enkele uren duren voordat alles goed wordt weer gegeven. Als het te lang lijkt te duren, controleert u het **algemene abonnements filter** voor het verplaatste abonnement om te controleren of het niet is verborgen. Mogelijk moet u zich afmelden bij de Azure Portal en u vervolgens weer aanmelden om de nieuwe map te kunnen zien. 

    ![Pagina Directory-wisselaar, met voorbeeld gegevens](media/active-directory-how-subscriptions-associated-directory/directory-switcher.png)

Wijzigen van de abonnementsdirectory is een bewerking op serviceniveau, zodat deze niet van invloed op abonnement eigendom. De accountbeheerder kan nog steeds wijzigen voor de servicebeheerder van de [Accountcentrum](https://account.azure.com/subscriptions). Als de oorspronkelijke map verwijderen, moet u overbrengen van de abonnementsfacturering over aan een nieuwe accountbeheerder. Zie [Eigendom van een Azure-abonnement naar een ander account overdragen](../../billing/billing-subscription-transfer.md) voor meer informatie over het overdragen van het eigendom.

## <a name="post-association-steps"></a>Stappen na de koppeling
Nadat u een abonnement aan een andere map hebt gekoppeld, zijn er mogelijk extra stappen die u moet uitvoeren om de bewerkingen te hervatten.

1. Als u sleutel kluizen hebt, moet u de Tenant-ID van de sleutel kluis wijzigen. Zie voor meer informatie [een sleutel kluis Tenant-id wijzigen nadat een abonnement is verplaatst](../../key-vault/key-vault-subscription-move-fix.md).

2. Als u door het systeem toegewezen beheerde identiteiten voor resources gebruikt, moet u deze opnieuw inschakelen. Als u door de gebruiker toegewezen beheerde identiteiten gebruikt, moet u deze opnieuw maken. Nadat u de beheerde identiteiten opnieuw hebt ingeschakeld of opnieuw hebt gemaakt, moet u de machtigingen die zijn toegewezen aan deze identiteiten opnieuw instellen. Zie [Wat is beheerde identiteiten voor Azure-resources?](../managed-identities-azure-resources/overview.md)voor meer informatie.

3. Als u een Azure Stack met dit abonnement hebt geregistreerd, moet u zich opnieuw registreren. Zie [Azure stack registreren bij Azure](/azure-stack/operator/azure-stack-registration)voor meer informatie.



## <a name="next-steps"></a>Volgende stappen

- Het maken van een nieuwe Azure AD-tenant, Zie [toegang tot Azure Active Directory om een nieuwe tenant maken](active-directory-access-create-new-tenant.md)

- Als u meer wilt weten over hoe de toegang tot resources wordt beheerd in Microsoft Azure, ziet u [Inzicht krijgen in toegang tot resources in Azure](../../role-based-access-control/rbac-and-directory-admin-roles.md)

- Zie voor meer informatie over het toewijzen van rollen in Azure AD, [directory-rollen toewijzen aan gebruikers met Azure Active Directory](active-directory-users-assign-role-azure-portal.md)
