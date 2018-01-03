---
title: Gebruikersmachtigingen toewijzen aan specifieke labbeleidsregels | Microsoft Docs
description: Informatie over het verlenen van de gebruikersmachtigingen voor specifieke labbeleidsregels in DevTest Labs op basis van de behoeften van elke gebruiker
services: devtest-lab,virtual-machines,visual-studio-online
documentationcenter: na
author: craigcaseyMSFT
manager: douge
editor: 
ms.assetid: 5ca829f0-eb69-40a1-ae26-03a629db1d7e
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/25/2016
ms.author: v-craic
ms.openlocfilehash: f92ad5e991bdb066bb9680b4865501076d43f450
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2018
---
# <a name="grant-user-permissions-to-specific-lab-policies"></a>Gebruikersmachtigingen toewijzen aan specifieke labbeleidsregels
## <a name="overview"></a>Overzicht
In dit artikel laat zien hoe u PowerShell gebruikt om een bepaalde lab beleid gebruikersmachtigingen te verlenen. Op die manier kunnen machtigingen worden toegepast op basis van de behoeften van elke gebruiker. U wilt wijzigen van de beleidsinstellingen van de VM, maar niet de beleidsregels van de kosten voor een bepaalde gebruiker verlenen.

## <a name="policies-as-resources"></a>Beleid als resources
Zoals beschreven in de [toegangsbeheer op basis van rollen in Azure](../active-directory/role-based-access-control-configure.md) artikel RBAC kunt Geavanceerd toegangsbeheer van resources voor Azure. Met RBAC kunt u taken scheiden binnen uw team DevOps en de hoeveelheid toegang verlenen aan gebruikers die ze nodig hebben voor het uitvoeren van hun taken.

In DevTest Labs is een beleid een resourcetype waarmee de actie RBAC **Microsoft.DevTestLab/labs/policySets/policies/**. Elk beleid lab is een resource in het beleid brontype en kan worden toegewezen als een bereik aan een RBAC-rol.

Bijvoorbeeld, als u wilt machtigen gebruikers lezen/schrijven naar de **VM-grootten toegestaan** beleid, maakt u een aangepaste rol die geschikt is voor de **Microsoft.DevTestLab/labs/policySets/policies/*** actie, en vervolgens de juiste gebruikers toewijzen aan deze aangepaste rol in het bereik van **Microsoft.DevTestLab/labs/policySets/policies/AllowedVmSizesInLab**.

Zie voor meer informatie over aangepaste rollen in RBAC, de [aangepaste rollen toegangsbeheer](../active-directory/role-based-access-control-custom-roles.md).

## <a name="creating-a-lab-custom-role-using-powershell"></a>Maken van een aangepaste lab-functie met behulp van PowerShell
Om te beginnen, moet u het volgende artikel, waarin wordt uitgelegd hoe u wilt installeren en configureren van de Azure PowerShell-cmdlets lezen: [https://azure.microsoft.com/blog/azps-1-0-pre](https://azure.microsoft.com/blog/azps-1-0-pre).

Zodra u de Azure PowerShell-cmdlets hebt ingesteld, kunt u de volgende taken uitvoeren:

* Lijst met alle bewerkingen/acties voor een resourceprovider
* Lijst met acties in een bepaalde rol:
* Een aangepaste beveiligingsrol maken

De volgende PowerShell-script ziet u voorbeelden van hoe u deze taken uitvoert:

    ‘List all the operations/actions for a resource provider.
    Get-AzureRmProviderOperation -OperationSearchString "Microsoft.DevTestLab/*"

    ‘List actions in a particular role.
    (Get-AzureRmRoleDefinition "DevTest Labs User").Actions

    ‘Create custom role.
    $policyRoleDef = (Get-AzureRmRoleDefinition "DevTest Labs User")
    $policyRoleDef.Id = $null
    $policyRoleDef.Name = "Policy Contributor"
    $policyRoleDef.IsCustom = $true
    $policyRoleDef.AssignableScopes.Clear()
    $policyRoleDef.AssignableScopes.Add("/subscriptions/<SubscriptionID> ")
    $policyRoleDef.Actions.Add("Microsoft.DevTestLab/labs/policySets/policies/*")
    $policyRoleDef = (New-AzureRmRoleDefinition -Role $policyRoleDef)

## <a name="assigning-permissions-to-a-user-for-a-specific-policy-using-custom-roles"></a>Machtigingen toewijzen aan een gebruiker voor een specifiek beleid met behulp van aangepaste rollen
Als u uw aangepaste rollen hebt gedefinieerd, kunt u deze toewijzen aan gebruikers. Om een aangepaste rol toewijzen aan een gebruiker, moet u eerst aanvragen de **ObjectId** voor die gebruiker. Gebruik hiervoor de **Get-AzureRmADUser** cmdlet.

In het volgende voorbeeld wordt de **ObjectId** van de *SomeUser* gebruiker 05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3 is.

    PS C:\>Get-AzureRmADUser -SearchString "SomeUser"

    DisplayName                    Type                           ObjectId
    -----------                    ----                           --------
    someuser@hotmail.com                                          05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3

Zodra u hebt de **ObjectId** voor de gebruiker en de naam van een aangepaste rol, kunt u die rol toewijzen aan de gebruiker met de **New AzureRmRoleAssignment** cmdlet:

    PS C:\>New-AzureRmRoleAssignment -ObjectId 05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3 -RoleDefinitionName "Policy Contributor" -Scope /subscriptions/<SubscriptionID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.DevTestLab/labs/<LabName>/policySets/default/policies/AllowedVmSizesInLab

In het vorige voorbeeld de **AllowedVmSizesInLab** beleid wordt gebruikt. U kunt een van de volgende beleid:

* MaxVmsAllowedPerUser
* MaxVmsAllowedPerLab
* AllowedVmSizesInLab
* LabVmsShutdown

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Volgende stappen
U hebt verleende gebruikersmachtigingen voor specifieke labbeleidsregels, hier zijn enkele Vervolgstappen rekening houden met:

* [Toegang tot een lab beveiligen](devtest-lab-add-devtest-user.md)
* [Labbeleidsregels instellen](devtest-lab-set-lab-policy.md)
* [Een labsjabloon maken](devtest-lab-create-template.md)
* [Aangepaste artefacten maken voor uw virtuele machines](devtest-lab-artifact-author.md)
* [Een virtuele machine aan een lab toevoegen](devtest-lab-add-vm.md)

