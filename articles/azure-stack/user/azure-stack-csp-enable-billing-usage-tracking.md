---
title: Een Cloud Service Provider voor het beheren van uw Azure-Stack-abonnement inschakelen | Microsoft Docs
description: Schakel de serviceprovider voor toegang tot een abonnement in Azure-Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/27/2018
ms.author: mabrigg
ms.reviewer: alfredop
ms.openlocfilehash: f0cff8f575b87872c0032854f1916b140d7fd62b
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
---
# <a name="enable-a-cloud-service-provider-to-manage-your-azure-stack-subscription"></a>Een Cloud Service Provider voor het beheren van uw Azure-Stack-abonnement inschakelen

*Van toepassing op: Azure Stack geïntegreerd systemen*

Als u Azure-Stack met een Cloud Service Provider (CSP), kunt u kiezen om het beheren van uw eigen abonnement voor toegang tot bronnen in Azure en Azure-Stack. U kunt ook de provider beheren van uw abonnement voor u. Dit artikel ziet u hoe aan:

 * Geef uw provider toegang tot uw abonnement.
 * Zorg ervoor dat de serviceprovider uw service kunt beheren.

> [!Note]
>  Als uw account is niet door de CSP beheren en u de volgende stappen overslaan, kan de CSP uw abonnement Azure Stack voor u niet beheren.

## <a name="manage-your-subscription-with-a-cloud-service-provider"></a>Beheren van uw abonnement met een Cloudserviceprovider

Toevoegen van de CSP als **gebruiker** aan uw abonnement.

1. Uw CSP als gastgebruiker met de gebruikersrol toevoegen aan uw tenant-directory.  Zie voor instructies over het toevoegen van een gebruiker [nieuwe gebruikers toevoegen aan Azure Active Directory](https://docs.microsoft.com/azure/active-directory/add-users-azure-active-directory)
2. De CSP wordt het lokale Stack Azure-abonnement voor u gemaakt.
3. U bent klaar om te beginnen met behulp van Azure-Stack.
4. De CSP moet een resource maken in uw abonnement om te controleren of ze kunnen ook uw bronnen beheren. Bijvoorbeeld, kunnen ze [virtuele Windows-machine maken met de Stack van Azure-portal](azure-stack-quick-windows-portal.md).

## <a name="enable-the-cloud-service-provider-to-manage-your-subscription-using-rbac-rights"></a>De Cloudserviceprovider om uw abonnement met RBAC rechten inschakelen

Toevoegen van de CSP als **eigenaar** aan uw abonnement.

1. Uw CSP als gastgebruiker toevoegen aan uw tenant-directory.  Zie voor instructies over het toevoegen van een gebruiker [nieuwe gebruikers toevoegen aan Azure Active Directory](https://docs.microsoft.com/azure/active-directory/add-users-azure-active-directory)
2. De rol van eigenaar toevoegen aan de gastgebruiker CSP. Zie voor stappen op de CSP-gebruiker toe te voegen aan uw abonnement [Use Role-Based toegangsbeheer voor het beheren van toegang tot de resources van uw Azure-abonnement](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal)
3. De CSP wordt het lokale Stack Azure-abonnement voor u gemaakt.
4. U bent klaar om te beginnen met behulp van Azure-Stack.
5. De CSP moet een resource maken in uw abonnement om te controleren of dat ze uw resources kunnen beheren.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het ophalen van informatie over het gebruik van de bron van Azure-Stack, [gebruiks- en facturering in Azure Stack](../azure-stack-billing-and-chargeback.md).
