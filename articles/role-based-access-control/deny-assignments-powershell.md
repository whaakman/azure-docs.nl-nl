---
title: Lijst weigeren toewijzingen voor Azure-resources met behulp van Azure PowerShell | Microsoft Docs
description: Leer hoe u de gebruikers, groepen, service-principals en beheerde identiteiten die toegang tot specifieke Azure-resource die aan bepaalde bereiken met behulp van Azure PowerShell is geweigerd.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/12/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: c1ea26fdb4d60262f89ea6ab0f87220a08c01e68
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67110483"
---
# <a name="list-deny-assignments-for-azure-resources-using-azure-powershell"></a>Lijst met toewijzingen voor Azure-resources met behulp van Azure PowerShell weigeren

[Toewijzingen weigeren](deny-assignments.md) blokkeren dat gebruikers kunnen uitvoeren van acties die specifieke Azure-resource, zelfs als een roltoewijzing deze toegang wordt verleend. Dit artikel wordt beschreven hoe u met behulp van Azure PowerShell toewijzingen weigeren.

> [!NOTE]
> U kunt geen rechtstreeks maken uw eigen toewijzingen weigeren. Zie voor informatie over hoe u weigeren toewijzingen worden gemaakt, [weigeren toewijzingen](deny-assignments.md).

## <a name="prerequisites"></a>Vereisten

Voor informatie over de toewijzing van een weigeren, moet u het volgende hebben:

- `Microsoft.Authorization/denyAssignments/read` machtiging die is opgenomen in de meeste [ingebouwde rollen voor Azure-resources](built-in-roles.md)
- [PowerShell in Azure Cloudshell](/azure/cloud-shell/overview) of [Azure PowerShell](/powershell/azure/install-az-ps)

## <a name="list-deny-assignments"></a>Lijst met toewijzingen weigeren

### <a name="list-all-deny-assignments"></a>Lijst met alle toewijzingen weigeren

Om weer te geven alle toewijzingen voor het huidige abonnement, gebruik weigeren [Get-AzDenyAssignment](/powershell/module/az.resources/get-azdenyassignment).

```azurepowershell
Get-AzDenyAssignment
```

```Example
PS C:\> Get-AzDenyAssignment

Id                      : 22222222-2222-2222-2222-222222222222
DenyAssignmentName      : Deny assignment '22222222-2222-2222-2222-222222222222' created by Blueprint Assignment
                          '/subscriptions/11111111-1111-1111-1111-111111111111/providers/Microsoft.Blueprint/blueprintAssignments/assignment-locked-storageaccount-TestingBPLocks'.
Description             : Created by Blueprint Assignment '/subscriptions/11111111-1111-1111-1111-111111111111/providers/Microsoft.Blueprint/blueprintAssignments/assignment-locked-storageaccount-TestingBPLocks'.
Actions                 : {*}
NotActions              : {*/read}
DataActions             : {}
NotDataActions          : {}
Scope                   : /subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/TestingBPLocks
DoNotApplyToChildScopes : True
Principals              : {
                          DisplayName:  All Principals
                          ObjectType:   SystemDefined
                          ObjectId:     00000000-0000-0000-0000-000000000000
                          }
ExcludePrincipals       : {
                          ObjectType:   ServicePrincipal
                          }
IsSystemProtected       : True

Id                      : 33333333-3333-3333-3333-333333333333
DenyAssignmentName      : Deny assignment '33333333-3333-3333-3333-333333333333' created by Blueprint Assignment
                          '/subscriptions/11111111-1111-1111-1111-111111111111/providers/Microsoft.Blueprint/blueprintAssignments/assignment-locked-storageaccount-TestingBPLocks'.
Description             : Created by Blueprint Assignment '/subscriptions/11111111-1111-1111-1111-111111111111/providers/Microsoft.Blueprint/blueprintAssignments/assignment-locked-storageaccount-TestingBPLocks'.
Actions                 : {*}
NotActions              : {*/read}
DataActions             : {}
NotDataActions          : {}
Scope                   : /subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/TestingBPLocks/providers/Microsoft.Storage/storageAccounts/storep6vkuxmu4m4pq
DoNotApplyToChildScopes : True
Principals              : {
                          DisplayName:  All Principals
                          ObjectType:   SystemDefined
                          ObjectId:     00000000-0000-0000-0000-000000000000
                          }
ExcludePrincipals       : {
                          DisplayName:  assignment-locked-storageaccount-TestingBPLocks
                          ObjectType:   ServicePrincipal
                          ObjectId:     2311a0b7-657a-4ca2-af6f-d1c33f6d2fff
                          }
IsSystemProtected       : True
```

### <a name="list-deny-assignments-at-a-resource-group-scope"></a>Lijst met toewijzingen met een bereik van de groep resource weigeren

Om weer te geven alle toewijzingen met een bereik van de groep resource, gebruik weigeren [Get-AzDenyAssignment](/powershell/module/az.resources/get-azdenyassignment).

```azurepowershell
Get-AzDenyAssignment -ResourceGroupName <resource_group_name>
```

```Example
PS C:\> Get-AzDenyAssignment -ResourceGroupName TestingBPLocks | FL DenyAssignmentName, Scope

DenyAssignmentName : Deny assignment '22222222-2222-2222-2222-222222222222' created by Blueprint Assignment
                     '/subscriptions/11111111-1111-1111-1111-111111111111/providers/Microsoft.Blueprint/blueprintAssignments/assignment-locked-storageaccount-TestingBPLocks'.
Scope              : /subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/TestingBPLocks
Principals         : {
                     DisplayName:       All Principals
                     ObjectType:        SystemDefined
                     ObjectId:  00000000-0000-0000-0000-000000000000
                     }
```

### <a name="list-deny-assignments-at-a-subscription-scope"></a>Lijst met toewijzingen met een abonnementsbereik weigeren

Om weer te geven alle toewijzingen in het abonnementsbereik van een, gebruik weigeren [Get-AzDenyAssignment](/powershell/module/az.resources/get-azdenyassignment). Als u de abonnements-ID, kunt u deze vinden op de **abonnementen** -blade in Azure portal of u kunt [Get-AzSubscription](/powershell/module/Az.Accounts/Get-AzSubscription).

```azurepowershell
Get-AzDenyAssignment -Scope /subscriptions/<subscription_id>
```

```Example
PS C:\> Get-AzDenyAssignment -Scope /subscriptions/11111111-1111-1111-1111-111111111111
```

## <a name="next-steps"></a>Volgende stappen

- [Informatie over toewijzingen voor Azure-resources te weigeren](deny-assignments.md)
- [Lijst weigeren toewijzingen voor Azure-resources met behulp van de Azure portal](deny-assignments-portal.md)
- [Lijst weigeren toewijzingen voor Azure-resources met behulp van de REST-API](deny-assignments-rest.md)