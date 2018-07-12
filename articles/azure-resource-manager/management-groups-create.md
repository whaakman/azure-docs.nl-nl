---
title: Beheergroepen voor het ordenen van Azure-resources maken | Microsoft Docs
description: Informatie over het maken van Azure-beheergroepen voor het beheren van meerdere resources.
author: rthorn17
manager: rithorn
editor: ''
ms.assetid: ''
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/09/2018
ms.author: rithorn
ms.openlocfilehash: f2b596b34aa18d20fa888ad40e82eccb90d5fd8c
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38465768"
---
# <a name="create-management-groups-for-resource-organization-and-management"></a>Beheergroepen voor resource organisatie en beheer maken
Beheergroepen zijn containers waarmee u toegang, beleid en naleving beheren voor meerdere abonnementen. Maken van deze containers voor het bouwen van een effectief en efficiënt-hiërarchie die kan worden gebruikt met [Azure Policy](../azure-policy/azure-policy-introduction.md) en [Azure rol-gebaseerd toegangsbeheer](../role-based-access-control/overview.md). Zie voor meer informatie over beheergroepen [organiseren van uw resources met Azure-beheergroepen ](management-groups-overview.md). 

De functie management group is beschikbaar in een openbare preview. Om te beginnen met beheer van groepen, meld u aan bij de [Azure-portal](https://portal.azure.com) of kunt u [Azure PowerShell](https://www.powershellgallery.com/packages/AzureRM.ManagementGroups/0.0.1-preview), [Azure CLI](https://docs.microsoft.com/cli/azure/extension?view=azure-cli-latest#az_extension_list_available), of de [REST-API](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/managementgroups/resource-manager/Microsoft.Management/preview/2018-01-01-preview) naar beheergroepen maken.   

De eerste beheergroep die u hebt gemaakt in de map kan tot 15 minuten duren. Er zijn processen die worden uitgevoerd van de eerste keer voor het instellen van de beheerservice van groepen in Azure voor uw directory. U ontvangt een melding wanneer het proces voltooid is.  

## <a name="how-to-create-a-management-group"></a>Over het maken van een beheergroep
U kunt de beheergroep maken met behulp van de portal, PowerShell of Azure CLI.

### <a name="create-in-portal"></a>In de portal maken

1. Meld u aan bij de [Azure-portal](http://portal.azure.com).
2. Selecteer **alle services** > **beheergroepen**.
3. Selecteer op de hoofdpagina van **nieuwe beheergroep.** 

    ![Main-groep](media/management-groups/main.png) 
4.  Vul in het veld management groep-ID. 
    - De **beheergroep-ID** is de unieke id van map die wordt gebruikt voor het verzenden van opdrachten in deze beheergroep. Deze id kan niet worden bewerkt nadat is gemaakt, omdat deze in het hele systeem Azure gebruikt om te identificeren van deze groep. 
    - Het weergavenaamveld is de naam die wordt weergegeven in Azure portal. Een afzonderlijke weergavenaam is een optioneel veld bij het maken van het beheer te groeperen en op elk gewenst moment kan worden gewijzigd.  

    ![Maken](media/management-groups/create_context_menu.png)  
5.  Selecteer **opslaan**


### <a name="create-in-powershell"></a>Maken in PowerShell
In PowerShell, moet u de cmdlets Add-AzureRmManagementGroups gebruiken.   

```azurepowershell-interactive
C:\> New-AzureRmManagementGroup -GroupName Contoso 
```
De **GroupName** is een unieke id wordt gemaakt. Deze ID wordt gebruikt door andere opdrachten om te verwijzen naar deze groep en kan later worden gewijzigd.

Als u wilt dat de beheergroep die u wilt weergeven van een andere naam in Azure portal, voegt u de **DisplayName** parameter met de tekenreeks. Als u wilt maken van een beheergroep met de groepsnaam van Contoso en de weergavenaam van 'Contoso groeperen', gebruikt u bijvoorbeeld de volgende cmdlet: 

```azurepowershell-interactive
C:\> New-AzureRmManagementGroup -GroupName Contoso -DisplayName "Contoso Group" -ParentId ContosoTenant
``` 
Gebruik de **ParentId** parameter voor deze beheergroep hebt gemaakt onder een ander management.  

### <a name="create-in-azure-cli"></a>Maken in Azure CLI
Op de Azure CLI, gebruikt u de az-opdracht account management-groep maken. 

```azure-cli
C:\ az account management-group create --group-name <YourGroupName>
``` 

---

## <a name="next-steps"></a>Volgende stappen 
Zie voor meer informatie over de beheergroepen: 
- [Resources organiseren met beheergroepen voor Azure ](management-groups-overview.md)
- [Wijzigen, verwijderen of uw beheergroepen beheren](management-groups-manage.md)
- [De Azure Powershell-module installeren](https://www.powershellgallery.com/packages/AzureRM.ManagementGroups/0.0.1-preview)
- [Controleer de REST API-specificatie](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/managementgroups/resource-manager/Microsoft.Management/preview)
- [De Azure CLI-extensie installeren](https://docs.microsoft.com/cli/azure/extension?view=azure-cli-latest#az_extension_list_available)
