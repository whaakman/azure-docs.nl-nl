---
title: Gebruikersmachtigingen toewijzen aan specifieke lab-beleid | Microsoft Docs
description: Meer informatie over het verlenen van gebruikersmachtigingen voor het beleid voor specifieke lab maken in DevTest Labs op basis van de behoeften van elke gebruiker
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: 5ca829f0-eb69-40a1-ae26-03a629db1d7e
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: spelluru
ms.openlocfilehash: 70469a9e8737a9df18628951a061c97081c74080
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2019
ms.locfileid: "56735098"
---
# <a name="grant-user-permissions-to-specific-lab-policies"></a>Gebruikersmachtigingen verlenen aan specifieke lab-beleid
## <a name="overview"></a>Overzicht
In dit artikel ziet u hoe u PowerShell gebruikt om gebruikersmachtigingen aan een bepaalde lab-beleid te verlenen. Op die manier kunnen kunnen machtigingen worden toegepast op basis van de behoeften van elke gebruiker. U wilt bijvoorbeeld de mogelijkheid om te wijzigen van de instellingen voor virtuele machine, maar niet de beleidsregels van de kosten voor een bepaalde gebruiker verlenen.

## <a name="policies-as-resources"></a>Beleid als resources
Zoals beschreven in de [Azure Role-based Access Control](../role-based-access-control/role-assignments-portal.md) RBAC-artikel kunt u over Geavanceerd toegangsbeheer van resources voor Azure. Met RBAC kunt u taken scheiden binnen uw DevOps-team en alleen de mate van toegang verlenen aan gebruikers die ze nodig hebben om hun werk te kunnen.

In DevTest Labs, een beleid is een resourcetype waarmee de RBAC-actie **Microsoft.DevTestLab/labs/policySets/policies/**. Elk lab-beleid is een resource in het resourcetype en kan worden toegewezen als een bereik aan een RBAC-rol.

Bijvoorbeeld, om te kunnen gebruikers lezen/schrijven toestemming te geven de **toegestaan VM-grootten** beleid, maakt u een aangepaste rol die geschikt is voor de **Microsoft.DevTestLab/labs/policySets/policies/** actie , en vervolgens de juiste gebruikers toewijzen aan deze aangepaste rol binnen het bereik van **Microsoft.DevTestLab/labs/policySets/policies/AllowedVmSizesInLab**.

Zie voor meer informatie over aangepaste rollen in RBAC, de [toegangsbeheer voor aangepaste rollen](../role-based-access-control/custom-roles.md).

## <a name="creating-a-lab-custom-role-using-powershell"></a>Het maken van een aangepaste lab-rol met behulp van PowerShell
Als u wilt beginnen, moet u [Installeer Azure PowerShell](/powershell/azure/install-az-ps). 

Nadat u de Azure PowerShell-cmdlets hebt ingesteld, kunt u de volgende taken uitvoeren:

* Lijst met alle bewerkingen/acties voor een resourceprovider
* Lijst met acties in een bepaalde rol:
* Een aangepaste rol maken

De volgende PowerShell-script ziet u voorbeelden van hoe u deze taken uitvoert:

    ‘List all the operations/actions for a resource provider.
    Get-AzProviderOperation -OperationSearchString "Microsoft.DevTestLab/*"

    ‘List actions in a particular role.
    (Get-AzRoleDefinition "DevTest Labs User").Actions

    ‘Create custom role.
    $policyRoleDef = (Get-AzRoleDefinition "DevTest Labs User")
    $policyRoleDef.Id = $null
    $policyRoleDef.Name = "Policy Contributor"
    $policyRoleDef.IsCustom = $true
    $policyRoleDef.AssignableScopes.Clear()
    $policyRoleDef.AssignableScopes.Add("/subscriptions/<SubscriptionID> ")
    $policyRoleDef.Actions.Add("Microsoft.DevTestLab/labs/policySets/policies/*")
    $policyRoleDef = (New-AzRoleDefinition -Role $policyRoleDef)

## <a name="assigning-permissions-to-a-user-for-a-specific-policy-using-custom-roles"></a>Machtigingen toewijzen aan een gebruiker voor een specifiek beleid met behulp van aangepaste rollen
Als u uw aangepaste rollen hebt gedefinieerd, kunt u ze toewijzen aan gebruikers. Als u wilt een aangepaste rol toewijzen aan een gebruiker, moet u eerst aanvragen de **ObjectId** die aangeeft dat de gebruiker. Om dit te doen, gebruikt u de **Get-AzADUser** cmdlet.

In het volgende voorbeeld wordt de **ObjectId** van de *SomeUser* gebruiker 05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3 is.

    PS C:\>Get-AzADUser -SearchString "SomeUser"

    DisplayName                    Type                           ObjectId
    -----------                    ----                           --------
    someuser@hotmail.com                                          05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3

Zodra u hebt de **ObjectId** voor de gebruiker en de naam van een aangepaste rol, kunt u die rol toewijzen aan de gebruiker met de **New-AzRoleAssignment** cmdlet:

    PS C:\>New-AzRoleAssignment -ObjectId 05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3 -RoleDefinitionName "Policy Contributor" -Scope /subscriptions/<SubscriptionID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.DevTestLab/labs/<LabName>/policySets/default/policies/AllowedVmSizesInLab

In het vorige voorbeeld de **AllowedVmSizesInLab** beleid wordt gebruikt. U kunt een van de volgende beleid:

* MaxVmsAllowedPerUser
* MaxVmsAllowedPerLab
* AllowedVmSizesInLab
* LabVmsShutdown

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Volgende stappen
Nadat u hebt opgegeven gebruikersmachtigingen voor het specifieke lab-beleid, Hier vindt u enkele Vervolgstappen om te overwegen:

* [Toegang tot een lab beveiligen](devtest-lab-add-devtest-user.md)
* [Labbeleidsregels instellen](devtest-lab-set-lab-policy.md)
* [Een labsjabloon maken](devtest-lab-create-template.md)
* [Aangepaste artefacten maken voor uw virtuele machines](devtest-lab-artifact-author.md)
* [Een virtuele machine aan een lab toevoegen](devtest-lab-add-vm.md)

