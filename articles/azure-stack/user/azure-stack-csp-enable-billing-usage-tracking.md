---
title: Een Cloud Service Provider voor het beheren van uw Azure-Stack-abonnement inschakelen | Microsoft Docs
description: Schakel de serviceprovider voor toegang tot een abonnement in Azure-Stack.
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/27/2018
ms.author: mabrigg
ms.reviewer: alfredop
ms.openlocfilehash: 4bc5644425aa11fb210d81095e4166baefc6432e
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2018
---
# <a name="enable-a-cloud-service-provider-to-manage-your-azure-stack-subscription"></a>Een Cloud Service Provider voor het beheren van uw Azure-Stack-abonnement inschakelen

*Van toepassing op: Azure Stack geïntegreerd systemen*

Als u Azure-Stack met een Cloud Service Provider (CSP), kan uw toegang tot bronnen in uw Azure-abonnement en Azure-Stack door de provider worden beheerd. Of u uw eigen abonnement kan beheren. In dit artikel wordt bekeken hoe u uw serviceprovider voor toegang tot uw abonnement namens u, of om te controleren of dat de serviceprovider kan uw service beheren kunt inschakelen.

> [!Note]  
>  Als de volgende stappen worden overgeslagen en de CSP wordt niet al het beheer van uw account, klikt u vervolgens pas de CSP weer voor het beheren van uw abonnement Azure Stack namens jou.

## <a name="manage-your-subscription-with-a-cloud-service-provider"></a>Beheren van uw abonnement met een Cloudserviceprovider

1. Uw CSP als gastgebruiker met de gebruikersrol toevoegen aan uw tenant-directory.  Zie voor instructies over het toevoegen van een gebruiker [nieuwe gebruikers toevoegen aan Azure Active Directory](https://docs.microsoft.com/azure/active-directory/add-users-azure-active-directory)
2. De CSP wordt dan het lokale Stack Azure-abonnement voor u gemaakt.
3. U bent klaar om te beginnen met behulp van Azure-Stack.
3. Uw CSP moet vervolgens met het maken van een resource in uw abonnement om te controleren of ze kunnen ook uw bronnen beheren. Bijvoorbeeld, kunnen ze [virtuele Windows-machine maken met de Stack van Azure-portal](azure-stack-quick-windows-portal.md).

## <a name="enable-the-cloud-service-provider-to-manage-your-subscription-using-rbac-rights"></a>De Cloudserviceprovider om uw abonnement met RBAC rechten inschakelen

De CSP als eigenaar aan uw abonnement toevoegt. 

1. Uw CSP als gastgebruiker toevoegen. met de rol van eigenaar naar de tenantmap van uw.  Zie voor instructies over het toevoegen van een gebruiker [nieuwe gebruikers toevoegen aan Azure Active Directory](https://docs.microsoft.com/azure/active-directory/add-users-azure-active-directory)
2. Rol van eigenaar toevoegen aan de gastgebruiker CSP. Zie voor stappen op de CSP-gebruiker toe te voegen aan uw abonnement [Use Role-Based toegangsbeheer voor het beheren van toegang tot de resources van uw Azure-abonnement](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure)
3. De CSP wordt dan het lokale Stack Azure-abonnement voor u gemaakt.
4. U bent klaar om te beginnen met behulp van Azure-Stack.
5. Uw CSP moet vervolgens met het maken van een resource in uw abonnement om te controleren of dat ze uw resources kunnen beheren. 

## <a name="next-steps"></a>Volgende stappen

  - Zie voor meer informatie over het ophalen van informatie over het gebruik van de bron van Azure-Stack, [gebruiks- en facturering in Azure Stack](../azure-stack-billing-and-chargeback.md).
