---
title: Beheergroepen om in te delen van Azure-resources - Governance in Azure maken
description: Informatie over het maken van Azure-beheergroepen voor het beheren van meerdere resources met behulp van de portal, Azure PowerShell en Azure CLI.
author: rthorn17
manager: rithorn
ms.service: azure-resource-manager
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/05/2019
ms.author: rithorn
ms.topic: conceptual
ms.openlocfilehash: 2dd2a6e071533deef47a6482bfb9ed92953864ba
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/08/2019
ms.locfileid: "59259798"
---
# <a name="create-management-groups-for-resource-organization-and-management"></a>Beheergroepen voor resource organisatie en beheer maken

Beheergroepen zijn containers waarmee u toegang, beleid en naleving beheren voor meerdere abonnementen. Maken van deze containers voor het bouwen van een effectief en efficiënt-hiërarchie die kan worden gebruikt met [Azure Policy](../policy/overview.md) en [Azure rol-gebaseerd toegangsbeheer](../../role-based-access-control/overview.md). Zie voor meer informatie over beheergroepen [organiseren van uw resources met Azure-beheergroepen](overview.md).

De eerste beheergroep die u hebt gemaakt in de map kan tot 15 minuten duren. Er zijn processen die worden uitgevoerd van de eerste keer voor het instellen van de beheerservice van groepen in Azure voor uw directory. U ontvangt een melding wanneer het proces voltooid is.

[!INCLUDE [az-powershell-update](../../../includes/updated-for-az.md)]

## <a name="create-a-management-group"></a>Een beheergroep maken

U kunt de beheergroep maken met behulp van de portal, PowerShell of Azure CLI. Op dit moment niet u Resource Manager-sjablonen gebruiken voor het maken van beheergroepen.

### <a name="create-in-portal"></a>In de portal maken

1. Meld u aan bij de [Azure-portal](https://portal.azure.com).

1. Selecteer **alle services** > **beheergroepen**.

1. Selecteer op de hoofdpagina van **nieuwe beheergroep**.

   ![Pagina voor het werken met beheergroepen](./media/main.png)

1. Vul in het veld management groep-ID.

   - De **beheergroep-ID** is de unieke id van map die wordt gebruikt voor het verzenden van opdrachten in deze beheergroep. Deze id is niet bewerkbaar na het maken van zoals het in het hele systeem Azure wordt gebruikt voor het identificeren van deze groep. De [hoofdmap beheergroep](index.md#root-management-group-for-each-directory) wordt automatisch gemaakt met een ID die de Azure Active Directory-ID. Voor alle overige beheergroepen toewijzen een unieke ID.
   - Het weergavenaamveld is de naam die wordt weergegeven in Azure portal. Een afzonderlijke weergavenaam is een optioneel veld bij het maken van het beheer te groeperen en op elk gewenst moment kan worden gewijzigd.  

   ![Deelvenster Opties voor het maken van een nieuwe beheergroep](./media/create_context_menu.png)  

1. Selecteer **Opslaan**.

### <a name="create-in-powershell"></a>Maken in PowerShell

Gebruik voor PowerShell de [New-AzManagementGroup](/powershell/module/az.resources/new-azmanagementgroup) cmdlet voor het maken van een nieuwe beheergroep.

```azurepowershell-interactive
New-AzManagementGroup -GroupName 'Contoso'
```

De **GroupName** is een unieke id wordt gemaakt. Deze ID wordt gebruikt door andere opdrachten om te verwijzen naar deze groep en kan later worden gewijzigd.

Als u wilt dat de beheergroep die u wilt weergeven van een andere naam in Azure portal, voegt u toe de **DisplayName** parameter. Bijvoorbeeld, voor het maken van een beheergroep met de groepsnaam van Contoso en de weergavenaam van 'Contoso groeperen', gebruikt u de volgende cmdlet:

```azurepowershell-interactive
New-AzManagementGroup -GroupName 'Contoso' -DisplayName 'Contoso Group'
```

In de voorgaande voorbeelden worden de nieuwe beheergroep wordt gemaakt onder de root management-groep. Als u een andere beheergroep als de bovenliggende, gebruikt de **ParentId** parameter.

```azurepowershell-interactive
$parentGroup = Get-AzManagementGroup -GroupName Contoso
New-AzManagementGroup -GroupName 'ContosoSubGroup' -ParentId $parentGroup.id
```

### <a name="create-in-azure-cli"></a>Maken in Azure CLI

Voor Azure CLI, gebruikt u de [az account management-groep maken](/cli/azure/account/management-group?view=azure-cli-latest#az-account-management-group-create) opdracht voor het maken van een nieuwe beheergroep.

```azurecli-interactive
az account management-group create --name Contoso
```

De **naam** is een unieke id wordt gemaakt. Deze ID wordt gebruikt door andere opdrachten om te verwijzen naar deze groep en kan later worden gewijzigd.

Als u wilt dat de beheergroep die u wilt weergeven van een andere naam in Azure portal, voegt u toe de **weergavenaam** parameter. Bijvoorbeeld, voor het maken van een beheergroep met de groepsnaam van Contoso en de weergavenaam van 'Contoso groeperen', gebruikt u de volgende opdracht uit:

```azurecli-interactive
az account management-group create --name Contoso --display-name 'Contoso Group'
```

In de voorgaande voorbeelden worden de nieuwe beheergroep wordt gemaakt onder de root management-groep. Als u een andere beheergroep als de bovenliggende, gebruikt de **bovenliggende** parameter en geef de naam van de bovenliggende groep.

```azurecli-interactive
az account management-group create --name ContosoSubGroup --parent Contoso
```

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over beheergroepen gaat u naar:

- [Beheergroepen voor het ordenen van Azure-resources maken](create.md)
- [Wijzigen, verwijderen of uw beheergroepen beheren](manage.md)
- [Van beheergroepen in Azure PowerShell-Module voor Resources bekijken](/powershell/module/az.resources#resources)
- [Bekijk beheergroepen in REST-API](/rest/api/resources/managementgroups)
- [Van beheergroepen in Azure CLI bekijken](/cli/azure/account/management-group)