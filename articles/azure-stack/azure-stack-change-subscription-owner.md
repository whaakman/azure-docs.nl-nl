---
title: Bijwerken van de eigenaar van de Azure Stack-gebruikersabonnement | Microsoft Docs
description: De eigenaar van de facturering voor Azure Stack-abonnementen voor gebruiker wijzigen.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: conceptual
ms.date: 02/21/2019
ms.author: sethm
ms.reviewer: shnatara
ms.lastreviewed: 10/19/2018
ms.openlocfilehash: 1d9dd7d19c196679ead9b552bcf296b4acd4ca68
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57842886"
---
# <a name="change-the-owner-for-an-azure-stack-user-subscription"></a>Wijzig de eigenaar voor een Azure Stack-gebruikersabonnement

Azure Stack-operators kunnen PowerShell gebruiken om te wijzigen van de eigenaar van de facturering van het gebruikersabonnement van een. Reden voor het wijzigen van de eigenaar wordt bijvoorbeeld vervangen door een gebruiker die uw organisatie verlaat.

Er zijn twee soorten *eigenaren* die zijn toegewezen aan een abonnement:

- **De eigenaar van de facturering**: De eigenaar van de facturering is standaard, het gebruikersaccount dat wordt opgehaald van het abonnement van een aanbieding en vervolgens is eigenaar van de facturering regelen voor dat abonnement. Dit account is ook een beheerder van het abonnement. Slechts één gebruikersaccount kan deze aanwijzing van een abonnement hebben. De eigenaar van een facturering is vaak een lead organisatie of team.

  U kunt de PowerShell-cmdlet [Set AzsUserSubscription](/powershell/module/azs.subscriptions.admin/set-azsusersubscription) te wijzigen van de eigenaar van de facturering.  

- **Eigenaren toevoegen via RBAC-rollen** – extra gebruikers kunnen worden verleend de **eigenaar** rol met behulp van de [rollen gebaseerd toegangsbeheer](azure-stack-manage-permissions.md) system (RBAC). Een willekeurig aantal aanvullende gebruikersaccounts kan worden toegevoegd als eigenaren aan een fraaie aanvulling in de eigenaar van de facturering. Aanvullende eigenaren zijn ook beheerders van het abonnement en hebben alle bevoegdheden voor het abonnement, met uitzondering van de machtiging voor het verwijderen van de eigenaar van de facturering.

  U kunt PowerShell gebruiken voor het beheren van extra eigenaren. Raadpleeg [dit artikel](/azure/role-based-access-control/role-assignments-powershell) voor meer informatie.

## <a name="change-the-billing-owner"></a>De eigenaar van de facturering wijzigen

Voer het volgende script als u wilt wijzigen van de eigenaar van de facturering van het gebruikersabonnement van een. De computer die u gebruikt om uit te voeren van het script moet verbinding maken met Azure Stack en uitvoeren van de Azure Stack-PowerShell-module 1.3.0 of hoger. Zie voor meer informatie, [PowerShell voor Azure Stack installeren](azure-stack-powershell-install.md).

>[!NOTE]
>In een multitenant Azure Stack, moet de nieuwe eigenaar zich in dezelfde map als de eigenaar van de bestaande. Voordat u de eigendom van het abonnement aan een gebruiker die zich in een andere map bieden kunt, moet u eerst [die gebruiker als gast uitnodigen in uw directory](../active-directory/b2b/add-users-administrator.md).

Vervang de volgende waarden in het script voordat deze wordt uitgevoerd:

- **$ArmEndpoint**: Het Resource Manager-eindpunt voor uw omgeving.
- **$TenantId**: Uw Tenant-ID.
- **$SubscriptionId**: Uw abonnements-ID.
- **$OwnerUpn**: Een account, bijvoorbeeld **gebruiker\@voorbeeld.com**om toe te voegen als de eigenaar van de nieuwe facturering.

```powershell
# Set up Azure Stack admin environment
Add-AzureRmEnvironment -ARMEndpoint $ArmEndpoint -Name AzureStack-admin
Add-AzureRmAccount -Environment AzureStack-admin -TenantId $TenantId

# Select admin subscription
$providerSubscriptionId = (Get-AzureRmSubscription -SubscriptionName "Default Provider Subscription").Id
Write-Output "Setting context to the Default Provider Subscription: $providerSubscriptionId"
Set-AzureRmContext -Subscription $providerSubscriptionId

# Change user subscription owner
$subscription = Get-AzsUserSubscription -SubscriptionId $SubscriptionId
$Subscription.Owner = $OwnerUpn
Set-AzsUserSubscription -InputObject $subscription
```

## <a name="next-steps"></a>Volgende stappen

- [Toegangsbeheer op basis van rollen beheren](azure-stack-manage-permissions.md)
