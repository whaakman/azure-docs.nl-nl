---
title: Een Cloudserviceprovider voor het beheren van uw Azure Stack-abonnement inschakelen | Microsoft Docs
description: Schakel de serviceprovider voor toegang tot een abonnement in Azure Stack.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2018
ms.author: sethm
ms.reviewer: alfredop
ms.openlocfilehash: fca08ef1d803c3bd47b0ae925c4dd12255175f2c
ms.sourcegitcommit: ab9514485569ce511f2a93260ef71c56d7633343
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/15/2018
ms.locfileid: "45630712"
---
# <a name="enable-a-cloud-service-provider-to-manage-your-azure-stack-subscription"></a>Een Cloudserviceprovider voor het beheren van uw Azure Stack-abonnement inschakelen

*Is van toepassing op: Azure Stack-geïntegreerde systemen*

Als u Azure Stack met een Cloud Service Provider (CSP), kunt u kiezen voor het beheren van uw eigen abonnement voor toegang tot resources in Azure en Azure Stack. U kunt ook de provider van uw abonnement voor u. In dit artikel ziet u hoe u naar:

 * Geef uw service-provider toegang tot uw abonnement.
 * Zorg ervoor dat uw service door de serviceprovider kunt beheren.

> [!Note]
>  Als de CSP is niet van uw account beheren en u de volgende stappen overslaan, kan uw Azure Stack-abonnement voor u niet de CSP beheren.

## <a name="manage-your-subscription-with-a-cloud-service-provider"></a>Beheren van uw abonnement met een Cloud Service Provider

Toevoegen van de CSP als **gebruiker** aan uw abonnement.

1. Uw CSP als gastgebruiker met de gebruikersrol toevoegen aan uw tenant-directory.  Zie voor instructies over het toevoegen van een gebruiker [nieuwe gebruikers toevoegen aan Azure Active Directory](https://docs.microsoft.com/azure/active-directory/add-users-azure-active-directory)
2. De CSP wordt de lokale Azure Stack-abonnement voor u gemaakt.
3. U bent klaar om te beginnen met behulp van Azure Stack.
4. Uw CSP moet een resource maken in uw abonnement om te controleren of ze kunnen ook uw bronnen beheren. Bijvoorbeeld, kunnen ze [een Windows-machine maken met de Azure Stack-portal](azure-stack-quick-windows-portal.md).

## <a name="enable-the-cloud-service-provider-to-manage-your-subscription-using-rbac-rights"></a>Inschakelen van de Cloudserviceprovider voor het beheren van uw abonnement met behulp van RBAC-rechten

Toevoegen van de CSP als **eigenaar** aan uw abonnement.

1. Uw CSP als gastgebruiker toevoegen aan uw tenant-directory.  Zie voor instructies over het toevoegen van een gebruiker [nieuwe gebruikers toevoegen aan Azure Active Directory](https://docs.microsoft.com/azure/active-directory/add-users-azure-active-directory)
2. De rol van eigenaar toevoegen aan de gastgebruiker CSP. Zie voor stappen van de CSP-gebruiker toevoegen aan uw abonnement [Use Role-Based toegangsbeheer voor het beheren van toegang tot de resources van uw Azure-abonnement](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal)
3. De CSP wordt de lokale Azure Stack-abonnement voor u gemaakt.
4. U bent klaar om te beginnen met behulp van Azure Stack.
5. Uw CSP moet een resource maken in uw abonnement om te controleren of dat ze uw resources kunnen beheren.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het ophalen van informatie over het gebruik van de resource van Azure Stack, [gebruik en facturering in Azure Stack](../azure-stack-billing-and-chargeback.md).
