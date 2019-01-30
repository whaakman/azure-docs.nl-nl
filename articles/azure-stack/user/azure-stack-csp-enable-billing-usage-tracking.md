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
ms.date: 01/19/2019
ms.author: sethm
ms.reviewer: alfredop
ms.lastreviewed: 01/19/2019
ms.openlocfilehash: 5d7398853e20702aef450e2532784f0dca7aac57
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55246599"
---
# <a name="enable-a-cloud-service-provider-to-manage-your-azure-stack-subscription"></a>Een Cloudserviceprovider voor het beheren van uw Azure Stack-abonnement inschakelen

*Van toepassing op: Azure Stack-geïntegreerde systemen*

Als u Azure Stack met een Cloud Service Provider (CSP), kunt u kiezen voor het beheren van uw eigen abonnement voor toegang tot resources in Azure en Azure Stack. U kunt ook de provider van uw abonnement voor u. In dit artikel ziet u hoe u naar:

* Geef uw service-provider toegang tot uw abonnement.
* Zorg ervoor dat uw service door de serviceprovider kunt beheren.

> [!NOTE]
> Als uw account wordt niet beheerd door de CSP en u de volgende stappen overslaan, kan uw Azure Stack-abonnement voor u niet de CSP beheren.

## <a name="manage-your-subscription-with-a-cloud-service-provider"></a>Beheren van uw abonnement met een Cloud Service Provider

Toevoegen van de CSP als **gebruiker** aan uw abonnement.

1. Uw CSP als gastgebruiker met de gebruikersrol toevoegen aan uw tenant-directory. Zie voor de stappen voor het toevoegen van een gebruiker, [nieuwe gebruikers toevoegen aan Azure Active Directory](../../active-directory/add-users-azure-active-directory.md)
2. De CSP wordt de lokale Azure Stack-abonnement voor u gemaakt. U bent klaar om te beginnen met behulp van Azure Stack.
3. Uw CSP moet een resource maken in uw abonnement om te controleren of ze kunnen ook uw bronnen beheren. Bijvoorbeeld, kunnen ze [een Windows-machine maken met de Azure Stack-portal](azure-stack-quick-windows-portal.md).

## <a name="enable-the-cloud-service-provider-to-manage-your-subscription-using-rbac-rights"></a>Inschakelen van de Cloudserviceprovider voor het beheren van uw abonnement met behulp van RBAC-rechten

Toevoegen van de CSP als **eigenaar** aan uw abonnement.

1. Uw CSP als gastgebruiker toevoegen aan uw tenant-directory. Zie voor de stappen voor het toevoegen van een gebruiker, [nieuwe gebruikers toevoegen aan Azure Active Directory](../../active-directory/add-users-azure-active-directory.md)
2. Voeg de **eigenaar** rol aan de gastgebruiker CSP. Zie voor de stappen voor de CSP-gebruiker toevoegen aan uw abonnement, [Use Role-Based toegangsbeheer voor het beheren van toegang tot de resources van uw Azure-abonnement](../../role-based-access-control/role-assignments-portal.md). De CSP wordt de lokale Azure Stack-abonnement voor u gemaakt. U bent klaar om te beginnen met behulp van Azure Stack.
3. Uw CSP moet een resource maken in uw abonnement om te controleren of dat ze uw resources kunnen beheren.

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over het ophalen van informatie over het gebruik van de resource van Azure Stack, [gebruik en facturering in Azure Stack](../azure-stack-billing-and-chargeback.md).
