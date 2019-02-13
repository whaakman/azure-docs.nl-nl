---
title: De tenant-ID voor key vault wijzigen na de verplaatsing van een abonnement - Azure Key Vault | Microsoft Docs
description: Meer informatie over het overschakelen op een ander tenant-ID voor een Key Vault nadat een abonnement is verplaatst naar een andere tenant
services: key-vault
documentationcenter: ''
author: amitbapat
manager: barbkess
tags: azure-resource-manager
ms.assetid: 46d7bc21-fa79-49e4-8c84-032eef1d813e
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/07/2019
ms.author: ambapat
ms.openlocfilehash: a83bff5a494ce338f43b6e967df5fe67cacfab01
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56112186"
---
# <a name="change-a-key-vault-tenant-id-after-a-subscription-move"></a>De tenant-ID van de Key Vault wijzigen na een verplaatsing van een abonnement

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="q-my-subscription-was-moved-from-tenant-a-to-tenant-b-how-do-i-change-the-tenant-id-for-my-existing-key-vault-and-set-correct-acls-for-principals-in-tenant-b"></a>VRAAG: Mijn abonnement is van tenant A verplaatst naar tenant B. Hoe wijzig ik de tenant-id voor mijn bestaande sleutelkluis en stel ik de juiste ACL's in voor principals in tenant B?

Wanneer u een nieuwe Key Vault maakt in een abonnement, is het automatisch gekoppeld aan de tenant-ID van Azure Active Directory voor dit abonnement. Alle vermeldingen van het toegangsbeleid zijn ook gekoppeld aan deze tenant-ID. Wanneer u uw Azure-abonnement van tenant A naar tenant B verplaatst, hebben de principals (gebruikers en toepassingen) geen toegang tot de bestaande Key Vaults in tenant B. U kunt dit probleem oplossen door:

* De tenant-ID die is gekoppeld aan alle bestaande Key Vaults in dit abonnement te wijzigen in tenant B.
* Alle bestaande vermeldingen van het toegangsbeleid te verwijderen.
* Nieuwe vermeldingen van het toegangsbeleid toe te voegen die zijn gekoppeld aan tenant B.

Als u bijvoorbeeld Kay Vault 'mijnvault' hebt in een abonnement dat is verplaatst van tenant A naar B, kunt u de tenant-ID voor deze Key Vault als volgt wijzigen en het oude toegangsbeleid verwijderen.

<pre>
Select-AzSubscription -SubscriptionId YourSubscriptionID
$vaultResourceId = (Get-AzKeyVault -VaultName myvault).ResourceId
$vault = Get-AzResource –ResourceId $vaultResourceId -ExpandProperties
$vault.Properties.TenantId = (Get-AzContext).Tenant.TenantId
$vault.Properties.AccessPolicies = @()
Set-AzResource -ResourceId $vaultResourceId -Properties $vault.Properties
</pre>

Omdat deze vault zich in tenant A voordat u de verplaatsing, de oorspronkelijke waarde van **$vault. Properties.TenantId** tenant a en tijdens **(Get-AzContext). Tenant.TenantId** tenant B.

Nu uw vault gekoppeld aan de juiste tenant-ID is en de oude vermeldingen van het toegangsbeleid zijn verwijderd, instellen nieuwe vermeldingen van het toegangsbeleid met [Set AzKeyVaultAccessPolicy](https://docs.microsoft.com/powershell/module/az.keyvault/Set-azKeyVaultAccessPolicy).

## <a name="next-steps"></a>Volgende stappen

Ga naar de [forums van Azure Key Vault](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault) als u vragen hebt over Azure Key Vault.
