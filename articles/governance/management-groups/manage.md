---
title: Wijzigen, verwijderen of beheren van uw beheergroepen in Azure - Azure-beheer
description: Informatie over het weergeven, onderhouden, bijwerken en verwijderen van uw hiërarchie beheren.
author: rthorn17
ms.service: azure-resource-manager
ms.date: 02/20/2019
ms.author: rithorn
ms.topic: conceptual
ms.openlocfilehash: 51e9d44a95a3896767caf4b3f04d17c2933e8599
ms.sourcegitcommit: f7f4b83996640d6fa35aea889dbf9073ba4422f0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/28/2019
ms.locfileid: "56990529"
---
# <a name="manage-your-resources-with-management-groups"></a>Uw resources beheren met beheergroepen

Als uw organisatie veel abonnementen heeft, moet u de toegang, beleidsregels en naleving voor deze abonnementen op een efficiënte manier kunnen beheren. Azure-beheergroepen bieden een scopeniveau boven abonnementen. U ordent abonnementen in containers, zogenaamde 'beheergroepen', en past uw governancevoorwaarden hierop toe. Alle abonnementen in een beheergroep nemen automatisch de voorwaarden over die op de beheergroep zijn toegepast.

Beheergroepen bieden u beheer van bedrijfskwaliteit op grote schaal, ongeacht de typen abonnementen die u hebt.  Zie voor meer informatie over het van beheergroepen, [organiseren van uw resources met Azure-beheergroepen](overview.md).

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

[!INCLUDE [az-powershell-update](../../../includes/updated-for-az.md)]

## <a name="change-the-name-of-a-management-group"></a>Wijzig de naam van een beheergroep

U kunt de naam van de beheergroep wijzigen met behulp van de portal, PowerShell of Azure CLI.

### <a name="change-the-name-in-the-portal"></a>Wijzig de naam in de portal

1. Meld u aan bij de [Azure-portal](https://portal.azure.com).

1. Selecteer **alle services** > **beheergroepen**.

1. Selecteer de beheergroep die u wilt wijzigen.

1. Selecteer de **naam van groep** optie aan de bovenkant van de pagina.

   ![Naam van groep-optie](./media/detail_action_small.png)

1. Wanneer het menu wordt geopend, typt u de nieuwe naam die u wilt weergeven.

   ![Deelvenster van de naam van groep](./media/rename_context.png)

1. Selecteer **Opslaan**.

### <a name="change-the-name-in-powershell"></a>Wijzig de naam in PowerShell

Bijwerken van het gebruik van de naam weergegeven **Update AzManagementGroup**. Bijvoorbeeld, om te wijzigen van een management groepen weergavenaam van 'Contoso IT' naar 'Contoso groeperen', u de volgende opdracht uitvoeren:

```azurepowershell-interactive
Update-AzManagementGroup -GroupName 'ContosoIt' -DisplayName 'Contoso Group'
```

### <a name="change-the-name-in-azure-cli"></a>Wijzig de naam in de Azure CLI

Gebruik de opdracht update voor Azure CLI.

```azurecli-interactive
az account management-group update --name 'Contoso' --display-name 'Contoso Group'
```

## <a name="delete-a-management-group"></a>Een beheergroep verwijderen

Als u wilt verwijderen van een beheergroep, moeten de volgende vereisten worden voldaan:

1. Er zijn geen onderliggende beheergroepen of de abonnementen voor de beheergroep.

   - Zie voor het verplaatsen van een abonnement uit een beheergroep, [abonnement verplaatsen naar een andere beheergroep](#Move-subscriptions-in-the-hierarchy).

   - Zie voor het verplaatsen van een beheergroep met een andere beheergroep, [beheergroepen verplaatsen in de hiërarchie](#Move-management-groups-in-the-hierarchy).

1. U hebt schrijfmachtigingen voor de beheergroep ("Owner", "Bijdrager" of "Management groep bijdrager"). Om te zien welke machtigingen u hebt, selecteert u de beheergroep en selecteer vervolgens **IAM**. Zie voor meer informatie over RBAC-rollen, [beheren van toegang en machtigingen met RBAC](../../role-based-access-control/overview.md).  

### <a name="delete-in-the-portal"></a>In de portal verwijderen

1. Meld u aan bij de [Azure-portal](https://portal.azure.com).

1. Selecteer **alle services** > **beheergroepen**.

1. Selecteer de beheergroep die u wilt verwijderen.

1. Selecteer **verwijderen**

    > [!TIP]
    > Als het pictogram is uitgeschakeld, ziet de muis selector muiswijzer op het pictogram u de reden.

   ![De Groepsoptie verwijderen](./media/delete.png)

1. Er is een venster dat wordt geopend waaruit blijkt dat u wilt verwijderen van de beheergroep.

   ![Bevestigingsvenster groep verwijderen](./media/delete_confirm.png)

1. Selecteer **Ja**.

### <a name="delete-in-powershell"></a>Verwijderen in PowerShell

Gebruik de **Remove-AzManagementGroup** opdracht vanuit PowerShell beheergroepen verwijderen.

```azurepowershell-interactive
Remove-AzManagementGroup -GroupName 'Contoso'
```

### <a name="delete-in-azure-cli"></a>Verwijderen in de Azure CLI

Met Azure CLI, gebruikt u de opdracht az account management-groep verwijderen.

```azurecli-interactive
az account management-group delete --name 'Contoso'
```

## <a name="view-management-groups"></a>Beheergroepen weergeven

Hier vindt u een beheergroep die u hebt een directe of overgenomen RBAC-rol op.  

### <a name="view-in-the-portal"></a>Weergeven in de portal

1. Meld u aan bij de [Azure-portal](https://portal.azure.com).

1. Selecteer **alle services** > **beheergroepen**.

1. De beheerpagina voor de hiërarchie van groep wordt geladen. Deze pagina is waar u alle beheergroepen kunt verkennen en abonnementen dat u toegang hebt. De groepsnaam van de selecteren gaat u naar beneden een niveau in de hiërarchie. De navigatie werkt op dezelfde manier als een bestand in bestandenverkenner.

1. De details van de beheergroep, selecteer de **(details)** koppelen naast de titel van de beheergroep. Als deze koppeling niet beschikbaar is, hebt u machtigingen om deze beheergroep weer te geven.

   ![Algemeen](./media/main.png)

### <a name="view-in-powershell"></a>Weergeven in PowerShell

U kunt de opdracht Get-AzManagementGroup gebruiken om op te halen van alle groepen.  Zie [Az.Resources](/powershell/module/az.resources/Get-AzManagementGroup) modules voor een volledige lijst van management groep ophalen Powershell-opdrachten.  

```azurepowershell-interactive
Get-AzManagementGroup
```

Voor een enkele beheergroep informatie gebruikt u de parameter - groepsnaam

```azurepowershell-interactive
Get-AzManagementGroup -GroupName 'Contoso'
```

Ophalen van een bepaalde beheergroep en alle niveaus van de hiërarchie onder het **-Vouw** en **-Recurse** parameters.  

```azurepowershell-interactive
PS C:\> $response = Get-AzManagementGroup -GroupName TestGroupParent -Expand -Recurse
PS C:\> $response

Id                : /providers/Microsoft.Management/managementGroups/TestGroupParent
Type              : /providers/Microsoft.Management/managementGroups
Name              : TestGroupParent
TenantId          : 00000000-0000-0000-0000-000000000000
DisplayName       : TestGroupParent
UpdatedTime       : 2/1/2018 11:15:46 AM
UpdatedBy         : 00000000-0000-0000-0000-000000000000
ParentId          : /providers/Microsoft.Management/managementGroups/00000000-0000-0000-0000-000000000000
ParentName        : 00000000-0000-0000-0000-000000000000
ParentDisplayName : 00000000-0000-0000-0000-000000000000
Children          : {TestGroup1DisplayName, TestGroup2DisplayName}

PS C:\> $response.Children[0]

Type        : /managementGroup
Id          : /providers/Microsoft.Management/managementGroups/TestGroup1
Name        : TestGroup1
DisplayName : TestGroup1DisplayName
Children    : {TestRecurseChild}

PS C:\> $response.Children[0].Children[0]

Type        : /managementGroup
Id          : /providers/Microsoft.Management/managementGroups/TestRecurseChild
Name        : TestRecurseChild
DisplayName : TestRecurseChild
Children    :
```

### <a name="view-in-azure-cli"></a>Weergeven in de Azure CLI

U de opdracht lijst gebruiken om op te halen van alle groepen.  

```azurecli-interactive
az account management-group list
```

Voor een enkele beheergroep informatie gebruikt u de opdracht weergeven

```azurecli-interactive
az account management-group show --name 'Contoso'
```

Ophalen van een bepaalde beheergroep en alle niveaus van de hiërarchie onder het **-Vouw** en **-Recurse** parameters.

```azurecli-interactive
az account management-group show --name 'Contoso' -e -r
```

## <a name="move-subscriptions-in-the-hierarchy"></a>Verplaatsen van abonnementen in de hiërarchie

Eén reden om te maken van een beheergroep is abonnementen samen bundelen. Alleen de beheergroepen en abonnementen kunnen onderliggende elementen van een andere beheergroep worden gemaakt. Een abonnement dat wordt verplaatst naar een beheergroep neemt alle toegang van gebruikers en het beleid van de bovenliggende beheergroep.

Voor het verplaatsen van het abonnement, moet de volgende RBAC-machtigingen waar zijn:

- De rol van de 'Eigenaar' van het onderliggende-abonnement.
- 'Eigenaar', "Bijdrager" of 'Management groep Inzender'-rol op het doel bovenliggende management group.*
- 'Eigenaar', "Bijdrager" of 'Management groep Inzender'-rol op de bestaande bovenliggende management group.*

*: Als het doel of de bestaande bovenliggende beheergroep die de Root management-groep. Aangezien de Root management-groep de landingspagina voor alle nieuwe beheergroepen en abonnementen spot standaard is, hoeven gebruikers niet machtigingen voor het verplaatsen van een item.

Om te controleren welke machtigingen u hebt in de Azure portal, selecteer de management groep en selecteer vervolgens **IAM**. Zie voor meer informatie over RBAC-rollen, [beheren van toegang en machtigingen met RBAC](../../role-based-access-control/overview.md).

### <a name="move-subscriptions-in-the-portal"></a>Verplaatsen van abonnementen in de portal

#### <a name="add-an-existing-subscription-to-a-management-group"></a>Een bestaand abonnement toevoegen aan een beheergroep

1. Meld u aan bij de [Azure-portal](https://portal.azure.com).

1. Selecteer **alle services** > **beheergroepen**.

1. Selecteer de beheergroep die u van plan bent om te worden van het bovenliggende item.

1. Aan de bovenkant van de pagina, selecteer **abonnement toevoegen**.

1. Selecteer het abonnement in de lijst met de juiste ID.

   ![Onderliggende items](./media/add_context_sub.png)

1. Selecteer 'Opslaan'.

#### <a name="remove-a-subscription-from-a-management-group"></a>Een abonnement verwijdert uit een beheergroep

1. Meld u aan bij de [Azure-portal](https://portal.azure.com).

1. Selecteer **alle services** > **beheergroepen**.

1. Selecteer de beheergroep die u van plan bent dat wil zeggen de huidige bovenliggende.  

1. Selecteer de ellips aan het einde van de rij voor het abonnement in de lijst die u wilt verplaatsen.

   ![Optie verplaatsen](./media/move_small.png)

1. Selecteer **verplaatsen**.

1. Selecteer op het menu dat verschijnt, de **bovenliggende beheergroep**.

   ![Deelvenster verplaatsen](./media/move_small_context.png)

1. Selecteer **Opslaan**.

### <a name="move-subscriptions-in-powershell"></a>Verplaatsen van abonnementen in PowerShell

Voor het verplaatsen van een abonnement in PowerShell, gebruikt u de opdracht New-AzManagementGroupSubscription.  

```azurepowershell-interactive
New-AzManagementGroupSubscription -GroupName 'Contoso' -SubscriptionId '12345678-1234-1234-1234-123456789012'
```

De koppeling tussen verwijderen en het abonnement en de beheergroep die u gebruikt de opdracht Remove-AzManagementGroupSubscription.

```azurepowershell-interactive
Remove-AzManagementGroupSubscription -GroupName 'Contoso' -SubscriptionId '12345678-1234-1234-1234-123456789012'
```

### <a name="move-subscriptions-in-azure-cli"></a>Verplaatsen van abonnementen in Azure CLI

Voor het verplaatsen van een abonnement in de CLI, gebruikt u de opdracht toevoegen.

```azurecli-interactive
az account management-group subscription add --name 'Contoso' --subscription '12345678-1234-1234-1234-123456789012'
```

Als u wilt verwijderen van het abonnement uit de beheergroep, de abonnement-remove-opdracht te gebruiken.  

```azurecli-interactive
az account management-group subscription remove --name 'Contoso' --subscription '12345678-1234-1234-1234-123456789012'
```

## <a name="move-management-groups-in-the-hierarchy"></a>Beheergroepen verplaatsen in de hiërarchie  

Wanneer u een bovenliggende beheergroep hebt verplaatst, worden ook de mappenhiërarchie in die groep verplaatst.

### <a name="move-management-groups-in-the-portal"></a>Beheergroepen verplaatsen in de portal

1. Meld u aan bij de [Azure-portal](https://portal.azure.com).

1. Selecteer **alle services** > **beheergroepen**.

1. Selecteer de beheergroep die u van plan bent om te worden van het bovenliggende item.

1. Aan de bovenkant van de pagina, selecteer **beheergroep toevoegen**.

1. Selecteer in het menu dat verschijnt, als u wilt dat een nieuwe of een bestaande beheergroep.

   - Nieuw selecteren, wordt een nieuwe beheergroep maken.
   - Selecteren van een bestaande, geeft u met een vervolgkeuzelijst van alle beheergroepen die u naar deze beheergroep verplaatsen kunt.  

   ![Verplaatsen](./media/add_context_MG.png)

1. Selecteer **Opslaan**.

### <a name="move-management-groups-in-powershell"></a>Verplaatsen van beheergroepen in PowerShell

Gebruik de opdracht Update AzManagementGroup in PowerShell om te verplaatsen van een beheergroep onder een andere groep.

```azurepowershell-interactive
Update-AzManagementGroup -GroupName 'Contoso' -ParentId '/providers/Microsoft.Management/managementGroups/ContosoIT'
```  

### <a name="move-management-groups-in-azure-cli"></a>Beheergroepen verplaatsen in de Azure CLI

Gebruik de opdracht update voor het verplaatsen van een beheergroep met Azure CLI.

```azurecli-interactive
az account management-group update --name 'Contoso' --parent-id '/providers/Microsoft.Management/managementGroups/ContosoIT'
```

## <a name="audit-management-groups-using-activity-logs"></a>Beheergroepen controleren met behulp van activiteitenlogboeken

Beheergroepen worden ondersteund in [Azure Activity Log](../../azure-monitor/platform/activity-logs-overview.md). U kunt een query uitvoeren voor alle gebeurtenissen die in een beheergroep op de centrale locatie als andere Azure-resources optreden.  U kunt bijvoorbeeld zien dat alle roltoewijzingen of beleidstoewijzing wijzigingen aangebracht in een bepaalde beheergroep.

![Activiteitenlogboeken met beheergroepen](media/al-mg.png)

Bij het zoeken naar query op beheergroepen buiten de Azure-portal, het doelbereik voor beheergroepen ziet eruit als **"/ providers/Microsoft.Management/managementGroups/{yourMgID}"**.

## <a name="referencing-management-groups-from-other-resource-providers"></a>Verwijzende beheergroepen van andere leveranciers van Resource

Wanneer u verwijst naar beheergroepen van de andere Resource Provider acties, moet u het volgende pad gebruiken als het bereik. Dit pad wordt gebruikt bij het gebruik van PowerShell, Azure CLI en REST-API's.  

>"/providers/Microsoft.Management/managementGroups/{yourMgID}"

Een voorbeeld van het gebruik van dit pad is bij het toewijzen van een nieuwe functie toewijzen aan een beheergroep in PowerShell

```powershell-interactive
New-AzRoleAssignment -Scope "/providers/Microsoft.Management/managementGroups/Contoso"
```

Hetzelfde bereik pad wordt gebruikt bij het ophalen van de beleidsdefinitie van een op een beheergroep.

```http
GET https://management.azure.com/providers/Microsoft.Management/managementgroups/MyManagementGroup/providers/Microsoft.Authorization/policyDefinitions/ResourceNaming?api-version=2018-05-01
```

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over beheergroepen gaat u naar:

- [Beheergroepen maken om Azure-resources te ordenen](create.md)
- [Uw beheergroepen wijzigen, verwijderen of beheren](manage.md)
- [Beheergroepen bekijken via de module voor Azure PowerShell-resources](/powershell/module/az.resources#resources)
- [Beheergroepen bekijken via de REST-API](/rest/api/resources/managementgroups)
- [Beheergroepen bekijken via de Azure-CLI](/cli/azure/account/management-group)