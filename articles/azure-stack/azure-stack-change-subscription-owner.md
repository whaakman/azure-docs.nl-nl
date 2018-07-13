---
title: Bijwerken van de eigenaar van de Azure Stack-gebruikersabonnement | Microsoft Docs
description: De eigenaar van de facturering voor Azure STack-abonnementen voor gebruiker wijzigen.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: get-started-article
ms.date: 06/12/2018
ms.author: brenduns
ms.reviewer: shnatara
ms.openlocfilehash: de8610944e11d8cf106ac484a0c6634c8661b5a2
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2018
ms.locfileid: "39009488"
---
# <a name="change-the-owner-for-an-azure-stack-user-subscription"></a>Wijzig de eigenaar voor een Azure Stack-gebruikersabonnement

Azure Stack-operators kunnen PowerShell gebruiken om te wijzigen van de eigenaar van de facturering van het gebruikersabonnement van een. Reden voor het wijzigen van de eigenaar wordt vervangen door een gebruiker die uw organisatie verlaat.   

Er zijn twee soorten *eigenaren* die zijn toegewezen aan een abonnement:

- **De eigenaar van de facturering** – de eigenaar van de facturering is standaard het gebruikersaccount dat wordt opgehaald van het abonnement van een aanbieding en vervolgens is eigenaar van de facturering regelen voor dat abonnement. Dit account is ook een beheerder van het abonnement.  Slechts één gebruikersaccount kan deze aanwijzing van een abonnement hebben. De eigenaar van een facturering is vaak een lead organisatie of team. 

  U de PowerShell-cmdlet gebruikt de **Set AzsUserSubscription** te wijzigen van de eigenaar van de facturering.  

- **Eigenaren toevoegen via RBAC-rollen** – extra gebruikers kunnen worden verleend de eigenaar van rol met behulp van de [rollen gebaseerd toegangsbeheer](azure-stack-manage-permissions.md) system (RBAC).  Een willekeurig aantal aanvullende gebruikersaccounts kan worden toegevoegd als eigenaren aan een fraaie aanvulling in de eigenaar van de facturering. Aanvullende eigenaren zijn ook beheerders van het abonnement en hebben alle bevoegdheden voor het abonnement, met uitzondering van machtigingen voor het verwijderen van de eigenaar van de facturering. 

  U kunt PowerShell gebruiken voor aanvullende eigenaren beheren, Zie [Azure PowerShell voor het beheren van op rollen gebaseerd toegangsbeheer]( https://docs.microsoft.com/azure/role-based-access-control/role-assignments-powershell).


## <a name="change-the-billing-owner"></a>De eigenaar van de facturering wijzigen
Voer het volgende script als u wilt wijzigen van de eigenaar van de facturering van het gebruikersabonnement van een.  De computer die u gebruikt om uit te voeren van het script moet verbinding maken met Azure Stack en uitvoeren van de Azure Stack-PowerShell-module 1.3.0 of hoger. Zie voor meer informatie, [PowerShell voor Azure Stack installeren](azure-stack-powershell-install.md). 

> [!Note]  
>  In een multitenant Azure Stack, moet de nieuwe eigenaar zich in dezelfde map als de eigenaar van de bestaande. Voordat u de eigendom van het abonnement aan een gebruiker die zich in een andere map bieden kunt, moet u eerst [die gebruiker als gast uitnodigen in uw Directory](https://docs.microsoft.com/azure/active-directory/b2b/add-users-administrator). 


Vervang de volgende waarden in het script voordat deze wordt uitgevoerd: 
 
- **$ArmEndpoint** – Geef het Resource Manager-eindpunt voor uw omgeving.  
- **$TenantId** -opgeven van uw Tenant-ID. 
- **$SubscriptionId** – Geef uw abonnements-ID.
- **$OwnerUpn** -Geef een account op als *user@example.com* om toe te voegen als de nieuwe eigenaar voor facturering.  


```PowerSHell   
# Setup Azure Stack Admin Environment
Add-AzureRmEnvironment -ARMEndpoint $ArmEndpoint -Name AzureStack-admin
Add-AzureRmAccount -Environment AzureStack-admin -TenantId $TenantId

# Select Admin Subscriptionr
$providerSubscriptionId = (Get-AzureRmSubscription -SubscriptionName "Default Provider Subscription").Id
Write-Output "Setting context the Default Provider Subscription: $providerSubscriptionId" 
Set-AzureRmContext -Subscription $providerSubscriptionId

# Change User Subscription owner
$subscription = Get-AzsUserSubscription -SubscriptionId $SubscriptionId
$Subscription.Owner = $OwnerUpn
Set-AzsUserSubscription -InputObject $subscription
```


## <a name="next-steps"></a>Volgende stappen
[Toegangsbeheer op basis van rollen beheren](azure-stack-manage-permissions.md)
