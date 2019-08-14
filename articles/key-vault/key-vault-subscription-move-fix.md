---
title: De Tenant-ID van de sleutel kluis wijzigen nadat een abonnement is verplaatst-Azure Key Vault | Microsoft Docs
description: Meer informatie over het overschakelen op een ander tenant-ID voor een Key Vault nadat een abonnement is verplaatst naar een andere tenant
services: key-vault
author: amitbapat
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.topic: tutorial
ms.date: 08/12/2019
ms.author: ambapat
ms.openlocfilehash: 2159b5b515e22458edf3ba0eb5b6f23f3f37ce95
ms.sourcegitcommit: 5b76581fa8b5eaebcb06d7604a40672e7b557348
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/13/2019
ms.locfileid: "68990110"
---
# <a name="change-a-key-vault-tenant-id-after-a-subscription-move"></a>De tenant-ID van de Key Vault wijzigen na een verplaatsing van een abonnement

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="q-my-subscription-was-moved-from-tenant-a-to-tenant-b-how-do-i-change-the-tenant-id-for-my-existing-key-vault-and-set-correct-acls-for-principals-in-tenant-b"></a>V: Mijn abonnement is van tenant A verplaatst naar tenant B. Hoe wijzig ik de tenant-id voor mijn bestaande sleutelkluis en stel ik de juiste ACL's in voor principals in tenant B?

Wanneer u een nieuwe Key Vault maakt in een abonnement, is het automatisch gekoppeld aan de tenant-ID van Azure Active Directory voor dit abonnement. Alle vermeldingen van het toegangsbeleid zijn ook gekoppeld aan deze tenant-ID. Wanneer u uw Azure-abonnement van tenant A naar tenant B verplaatst, hebben de principals (gebruikers en toepassingen) geen toegang tot de bestaande Key Vaults in tenant B. U kunt dit probleem oplossen door:

* De tenant-ID die is gekoppeld aan alle bestaande Key Vaults in dit abonnement te wijzigen in tenant B.
* Alle bestaande vermeldingen van het toegangsbeleid te verwijderen.
* Nieuwe vermeldingen van het toegangsbeleid toe te voegen die zijn gekoppeld aan tenant B.

Als u bijvoorbeeld Kay Vault 'mijnvault' hebt in een abonnement dat is verplaatst van tenant A naar B, kunt u de tenant-ID voor deze Key Vault als volgt wijzigen en het oude toegangsbeleid verwijderen.

<pre>
Select-AzSubscription -SubscriptionId YourSubscriptionID                   # Select your Azure Subscription
$vaultResourceId = (Get-AzKeyVault -VaultName myvault).ResourceId          # Get your Keyvault's Resource ID 
$vault = Get-AzResource –ResourceId $vaultResourceId -ExpandProperties     # Get the properties for your Keyvault
$vault.Properties.TenantId = (Get-AzContext).Tenant.TenantId               # Change the Tenant that your Keyvault resides in
$vault.Properties.AccessPolicies = @()                                     # Accesspolicies can be updated with real
                                                                           # applications/users/rights so that it does not need to be                                                                              # done after this whole activity. Here we are not setting 
                                                                           # any access policies. 
Set-AzResource -ResourceId $vaultResourceId -Properties $vault.Properties  # Modifies the kevault's properties.
</pre>

Omdat deze kluis zich in Tenant A bevond vóór de verplaatsing, is de oorspronkelijke waarde van **$Vault. Properties. TenantId** is Tenant A, terwijl **(Get-AzContext). Tenant. TenantId** is Tenant B.

Nu de kluis is gekoppeld aan de juiste Tenant-ID en de oude vermeldingen van het toegangs beleid worden verwijderd, stelt u nieuwe vermeldingen voor het toegangs beleid in met [set-AzKeyVaultAccessPolicy](https://docs.microsoft.com/powershell/module/az.keyvault/Set-azKeyVaultAccessPolicy).

## <a name="next-steps"></a>Volgende stappen

Ga naar de [forums van Azure Key Vault](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault) als u vragen hebt over Azure Key Vault.
