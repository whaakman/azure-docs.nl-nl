---
title: Beheergroepen te organiseren Azure-resources maken | Microsoft Docs
description: Informatie over het maken van Azure beheergroepen voor het beheren van meerdere resources.
author: rthorn17
manager: rithorn
editor: 
ms.assetid: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/1/2018
ms.author: rithorn
ms.openlocfilehash: ae91ad29b867ad4ab00831ee40102bcec2fc890c
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/05/2018
---
# <a name="create-management-groups-for-resource-organization-and-management"></a>Beheergroepen voor bronorganisatie en -beheer maken
Beheergroepen zijn containers waarmee u toegang, beleid en naleving in meerdere abonnementen beheren. Maken van deze containers voor het bouwen van een effectiever en efficiënter-hiërarchie die kan worden gebruikt met [Azure beleid](../azure-policy/azure-policy-introduction.md) en [toegangsbeheer voor op basis van Azure-functie](../active-directory/role-based-access-control-what-is.md). Zie voor meer informatie over beheergroepen [ordenen van uw resources met Azure-beheergroepen ](management-groups-overview.md). 

De functie management group is beschikbaar in een openbare preview. Aan de slag met beheer van groepen, meld u aan bij de [Azure-portal](https://portal.azure.com) of kunt u [Azure PowerShell](https://www.powershellgallery.com/packages/AzureRM.ManagementGroups/0.0.1-preview), [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/extension?view=azure-cli-latest#az_extension_list_available), of de [REST-API](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/managementgroups/resource-manager/Microsoft.Management/preview/2018-01-01-preview) naar beheergroepen maken.   

De eerste beheergroep gemaakt in de map kan maximaal 15 minuten duren. Er zijn processen die worden uitgevoerd van de eerste keer instellen van de groepen service management voor uw map binnen Azure. U ontvangt een melding wanneer het proces voltooid is.  

## <a name="how-to-create-a-management-group"></a>Het maken van een beheergroep
Met behulp van de portal, PowerShell of Azure CLI kunt u de beheergroep.

### <a name="create-in-portal"></a>Maak in de portal

1. Meld u aan bij de [Azure-portal](http://portal.azure.com).
2. Selecteer **alle services** > **beheergroepen**.
3. Selecteer op de hoofdpagina **nieuwe beheergroep.** 

    ![Groep maken](media/management-groups/create_main.png) 
4.  Vul in het veld management groep-ID. 
    - De **beheergroep-ID** is de directory unieke id die wordt gebruikt voor het indienen van opdrachten in deze beheergroep. Deze id kan niet worden bewerkt nadat het maken, zoals het in het hele systeem Azure wordt gebruikt om deze groep te identificeren. 
    - Veld met de weergavenaam is de naam die wordt weergegeven in de Azure-portal. Een afzonderlijke weergavenaam is een optioneel veld bij het maken van de management groep en op elk gewenst moment kan worden gewijzigd.  

    ![Maken](media/management-groups/create_context_menu.png)  
5.  Selecteer **opslaan**


### <a name="create-in-powershell"></a>Maak in PowerShell
PowerShell, moet u de cmdlets Add AzureRmManagementGroups gebruiken.   

```azurepowershell-interactive
C:\> New-AzureRmManagementGroup -GroupName Contoso 
```
De **GroupName** is de unieke id die wordt gemaakt. Deze ID wordt gebruikt door andere opdrachten om te verwijzen naar deze groep en deze later niet meer wijzigen.

Als u wilt dat de beheergroep om weer te geven van een andere naam in de Azure-portal, voegt u de **DisplayName** parameter met de tekenreeks. Als u wilt maken van een beheergroep met de groepsnaam van Contoso en de weergavenaam van 'Contoso-groep', gebruikt u bijvoorbeeld de volgende cmdlet: 

```azurepowershell-interactive
C:\> New-AzureRmManagementGroup -GroupName Contoso -DisplayName "Contoso Group" -ParentId ContosoTenant
``` 
Gebruik de **ParentId** -parameter voor deze beheergroep hebt gemaakt onder een andere beheergroep.  

### <a name="create-in-azure-cli"></a>Maken in Azure CLI
Op de Azure CLI, gebruikt u de az account beheergroep opdracht maken. 

```azure-cli
C:\ az account management-group create --group-name <YourGroupName>
``` 

---

## <a name="next-steps"></a>Volgende stappen 
Zie voor meer informatie over beheergroepen: 
- [Uw resources te organiseren met Azure-beheergroepen ](management-groups-overview.md)
- [Wijzigen, verwijderen of beheren van uw beheergroepen](management-groups-manage.md)
- [Installeer de Azure Powershell-module](https://www.powershellgallery.com/packages/AzureRM.ManagementGroups/0.0.1-preview)
- [Bekijk de REST-API-specificaties](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/managementgroups/resource-manager/Microsoft.Management/preview/2018-01-01-preview)
- [De uitbreiding voor de Azure CLI installeren](https://docs.microsoft.com/en-us/cli/azure/extension?view=azure-cli-latest#az_extension_list_available)
