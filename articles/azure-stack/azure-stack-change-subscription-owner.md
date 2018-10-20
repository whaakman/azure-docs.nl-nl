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
ms.topic: get-started-article
ms.date: 10/19/2018
ms.author: sethm
ms.reviewer: shnatara
ms.openlocfilehash: e5ce479940faaaae95467fe6d426e999b4c6569f
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/19/2018
ms.locfileid: "49468667"
---
# <a name="change-the-owner-for-an-azure-stack-user-subscription"></a>Wijzig de eigenaar voor een Azure Stack-gebruikersabonnement

Azure Stack-operators kunnen PowerShell gebruiken om te wijzigen van de eigenaar van de facturering van het gebruikersabonnement van een. Reden voor het wijzigen van de eigenaar wordt bijvoorbeeld vervangen door een gebruiker die uw organisatie verlaat.   

Er zijn twee soorten *eigenaren* die zijn toegewezen aan een abonnement:

- **De eigenaar van de facturering**: de eigenaar van de facturering is standaard het gebruikersaccount dat wordt opgehaald van het abonnement van een aanbieding en vervolgens is eigenaar van de facturering regelen voor dat abonnement. Dit account is ook een beheerder van het abonnement. Slechts één gebruikersaccount kan deze aanwijzing van een abonnement hebben. De eigenaar van een facturering is vaak een lead organisatie of team. 

  U de PowerShell-cmdlet gebruikt de [Set AzsUserSubscription](/powershell/module/azs.subscriptions.admin/set-azsusersubscription) te wijzigen van de eigenaar van de facturering.  

- **Eigenaren toevoegen via RBAC-rollen** – extra gebruikers kunnen worden verleend de **eigenaar** rol met behulp van de [rollen gebaseerd toegangsbeheer](azure-stack-manage-permissions.md) system (RBAC). Een willekeurig aantal aanvullende gebruikersaccounts kan worden toegevoegd als eigenaren aan een fraaie aanvulling in de eigenaar van de facturering. Aanvullende eigenaren zijn ook beheerders van het abonnement en hebben alle bevoegdheden voor het abonnement, met uitzondering van de machtiging voor het verwijderen van de eigenaar van de facturering. 

  U kunt PowerShell gebruiken voor aanvullende eigenaren beheren, Zie [Azure PowerShell voor het beheren van op rollen gebaseerd toegangsbeheer](/azure/role-based-access-control/role-assignments-powershell).

## <a name="change-the-billing-owner"></a>De eigenaar van de facturering wijzigen

Voer het volgende script als u wilt wijzigen van de eigenaar van de facturering van het gebruikersabonnement van een. De computer die u gebruikt om uit te voeren van het script moet verbinding maken met Azure Stack en uitvoeren van de Azure Stack-PowerShell-module 1.3.0 of hoger. Zie voor meer informatie, [PowerShell voor Azure Stack installeren](azure-stack-powershell-install.md). 

> [!Note]  
>  In een multitenant Azure Stack, moet de nieuwe eigenaar zich in dezelfde map als de eigenaar van de bestaande. Voordat u de eigendom van het abonnement aan een gebruiker die zich in een andere map bieden kunt, moet u eerst [die gebruiker als gast uitnodigen in uw directory](../active-directory/b2b/add-users-administrator.md). 

Vervang de volgende waarden in het script voordat deze wordt uitgevoerd: 
 
- **$ArmEndpoint**: Geef het Resource Manager-eindpunt voor uw omgeving.  
- **$TenantId**: Geef uw Tenant-ID. 
- **$SubscriptionId**: Geef uw abonnements-ID.
- **$OwnerUpn**: Geef een account op als **user@example.com** om toe te voegen als de eigenaar van de nieuwe facturering.  

```PowerShell   
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

[Toegangsbeheer op basis van rollen beheren](azure-stack-manage-permissions.md)
