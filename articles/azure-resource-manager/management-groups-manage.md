---
title: Wijzigen, verwijderen of beheren van uw beheergroepen - Azure | Microsoft Docs
description: Informatie over het onderhouden en bijwerken van uw hiërarchie beheren.
author: rthorn17
manager: rithorn
editor: ''
ms.assetid: ''
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 5/15/2018
ms.author: rithorn
ms.openlocfilehash: 822a2df113b848f07e616f155881f345028cee1d
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
---
# <a name="manage-your-resources-with-management-groups"></a>Uw resources beheren met beheergroepen 
Beheergroepen zijn containers waarmee u toegang, beleid en naleving in meerdere abonnementen beheren. U kunt wijzigen, verwijderen en beheren van deze containers om hiërarchieën die kunnen worden gebruikt met [Azure beleid](../azure-policy/azure-policy-introduction.md) en [Azure Access besturingselementen RBAC (Role Based)](../role-based-access-control/overview.md). Zie voor meer informatie over beheergroepen, [ordenen van uw resources met Azure-beheergroepen ](management-groups-overview.md).

De functie management group is beschikbaar in een openbare preview. Aan de slag met beheer van groepen, meld u aan bij de [Azure-portal](https://portal.azure.com) of kunt u [Azure PowerShell](https://www.powershellgallery.com/packages/AzureRM.ManagementGroups/0.0.1-preview), [Azure CLI](https://docs.microsoft.com/cli/azure/extension?view=azure-cli-latest#az_extension_list_available), of de [REST-API](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/managementgroups/resource-manager/Microsoft.Management/preview/2018-01-01-preview) naar uw beheergroepen beheren.

Als u wijzigingen aan een beheergroep, moet u een eigenaar of bijdrager rol hebben op de beheergroep. Om te zien welke machtigingen u hebt, selecteert u de beheergroep en selecteer vervolgens **IAM**. Zie voor meer informatie over RBAC-rollen, [beheren van toegang en machtigingen met RBAC](../role-based-access-control/overview.md).

[!INCLUDE [Handle personal data](../../includes/gdpr-intro-sentence.md)]

## <a name="change-the-name-of-a-management-group"></a>Wijzig de naam van een beheergroep 
U kunt de naam van de beheergroep wijzigen met behulp van de portal, PowerShell of Azure CLI.

### <a name="change-the-name-in-the-portal"></a>Wijzig de naam in de portal

1. Meld u aan bij de [Azure-portal](https://portal.azure.com)
2. Selecteer **alle services** > **beheergroepen**  
3. Selecteer de beheergroep die u wilt wijzigen. 
4. Selecteer de **naam van groep** optie boven aan de pagina. 

    ![Naam van groep wijzigen](media/management-groups/detail_action_small.png)
5. Wanneer het menu wordt geopend, typt u de nieuwe naam die u wilt weergeven.

    ![Naam van groep wijzigen](media/management-groups/rename_context.png) 
4. Selecteer **Opslaan**. 

### <a name="change-the-name-in-powershell"></a>Wijzig de naam in PowerShell

Bijwerken van het gebruik van de naam weer **Update AzureRmManagementGroup**. Bijvoorbeeld, als wilt wijzigen een naam op groepen van 'Contoso IT' aan 'Contoso-groep', voert u de volgende opdracht: 

```azurepowershell-inetractive
C:\> Update-AzureRmManagementGroup -GroupName ContosoIt -DisplayName "Contoso Group"  
``` 

### <a name="change-the-name-in-azure-cli"></a>Wijzig de naam in de Azure CLI

Gebruik de opdracht update voor Azure CLI. 

```azure-cli
C:\> az account management-group update --group-name Contoso --display-name "Contoso Group" 
```

---
 
## <a name="delete-a-management-group"></a>Een beheergroep verwijderen
Als u wilt verwijderen van een beheergroep, moeten de volgende vereisten worden voldaan:
1. Er zijn geen onderliggende beheergroepen of abonnementen onder de beheergroep. 
    - Zie voor het verplaatsen van een abonnement uit een beheergroep [abonnement verplaatsen naar een andere managemnt groep](#Move-subscriptions-in-the-hierarchy). 
    - Zie voor het verplaatsen van een beheergroep met een andere beheergroep, [beheergroepen verplaatsen in de hiërarchie](#Move-management-groups-in-the-hierarchy). 
2. U hebt schrijfmachtigingen voor de groep eigenaar of bijdrager rol op de beheergroep. Om te zien welke machtigingen u hebt, selecteert u de beheergroep en selecteer vervolgens **IAM**. Zie voor meer informatie over RBAC-rollen, [beheren van toegang en machtigingen met RBAC](../role-based-access-control/overview.md).  

### <a name="delete-in-the-portal"></a>In de portal verwijderen

1. Meld u aan bij de [Azure-portal](https://portal.azure.com)
2. Selecteer **alle services** > **beheergroepen**  
3. Selecteer de beheergroep die u wilt verwijderen. 
    
    ![Groep verwijderen](media/management-groups/delete.png)
4. Selecteer **Verwijderen**. 
    - Als het pictogram is uitgeschakeld, ziet de muis selector muiswijzer op het pictogram u de reden. 
5. Er is een venster dat wordt geopend waarin u wilt verwijderen van de beheergroep wordt bevestigd. 

    ![Groep verwijderen](media/management-groups/delete_confirm.png) 
6. Selecteer **Ja** 


### <a name="delete-in-powershell"></a>In PowerShell verwijderen

Gebruik de **verwijderen AzureRmManagementGroup** opdracht PowerShell beheergroepen verwijderen. 

```azurepowershell-interactive
Remove-AzureRmManagementGroup -GroupName Contoso
```

### <a name="delete-in-azure-cli"></a>Verwijderen in de Azure CLI
Met Azure CLI, gebruikt u de opdracht az account management-groep verwijderen. 

```azure-cli
C:\> az account management-group delete --group-name Contoso
```
---

## <a name="view-management-groups"></a>Beheergroepen weergeven
U kunt een beheergroep die u hebt een directe of overgenomen RBAC-rol op weergeven.  

### <a name="view-in-the-portal"></a>Weergeven in de portal
1. Meld u aan bij de [Azure-portal](https://portal.azure.com)
2. Selecteer **alle services** > **beheergroepen** 
3. De hiërarchie beheergroep pagina laadt waar u de beheergroepen en abonnementen die u toegang tot hebt kunt verkennen. Naam van de groep selecteren gaat u een lager in de hiërarchie. De navigatie werkt op dezelfde manier als een Windows Verkenner. 
    ![Main](media/management-groups/main.png)
4. Voor de details van de beheergroep, selecteert u de **(details)** koppeling naast de titel van de beheergroep. Als u deze koppeling niet beschikbaar is, er geen machtigingen voor het weergeven van de beheergroep.  

### <a name="view-in-powershell"></a>Weergeven in PowerShell
U kunt de opdracht Get-AzureRmManagementGroup gebruiken voor het ophalen van alle groepen.  

```azurepowershell-interactive
Get-AzureRmManagementGroup
```
Voor een enkele beheergroep informatie gebruikt u de parameter - GroupName

```azurepowershell-interactive
Get-AzureRmManagementGroup -GroupName Contoso
```

### <a name="view-in-azure-cli"></a>Weergave in de Azure CLI
De opdracht lijst kunt u alle groepen ophalen.  

```azure-cli
az account management-group list
```
Voor een enkele beheergroep informatie gebruikt u de opdracht weergeven

```azurepowershell-interactive
az account management-group show --group-name Contoso
```
---

## <a name="move-subscriptions-in-the-hierarchy"></a>Verplaatsen van abonnementen in de hiërarchie
Een reden voor het maken van een beheergroep is abonnementen samen. Alleen beheergroepen en abonnementen kunnen onderliggende elementen van een andere beheergroep worden gemaakt. Een abonnement dat wordt verplaatst naar een beheergroep neemt over van alle gebruikerstoegang en het beleid van de bovenliggende groep. 

Voor het verplaatsen van het abonnement, zijn er enkele machtigingen die u moet hebben: 
- 'Eigenaar'-rol op het onderliggende-abonnement.
- De rol 'Eigenaar' of 'Inzender' op de nieuwe bovenliggende beheergroep. 
- De rol 'Eigenaar' of 'Inzender' op de oude bovenliggende beheergroep.
Om te zien welke machtigingen u hebt, selecteert u de beheergroep en selecteer vervolgens **IAM**. Zie voor meer informatie over RBAC-rollen, [beheren van toegang en machtigingen met RBAC](../role-based-access-control/overview.md). 

### <a name="move-subscriptions-in-the-portal"></a>Verplaatsen van abonnementen in de portal

**Een bestaand abonnement toevoegen aan een beheergroep**
1. Meld u aan bij de [Azure-portal](https://portal.azure.com)
2. Selecteer **alle services** > **beheergroepen** 
3. Selecteer de beheergroep die u van plan bent als bovenliggende.      
5. Selecteer boven aan de pagina **bestaande toevoegen**. 
6. Selecteer in het menu dat geopend, de **brontype** van het item dat u probeert te verplaatsen dat **abonnement**.
7. Selecteer het abonnement in de lijst met de juiste ID. 

    ![Onderliggende items](media/management-groups/add_context_2.png)
8. 'Opslaan' selecteren

**Een abonnement verwijderen uit een beheergroep**
1. Meld u aan bij de [Azure-portal](https://portal.azure.com)
2. Selecteer **alle services** > **beheergroepen** 
3. Selecteer de beheergroep die u van plan bent dat de huidige bovenliggende.  
4. Selecteer de knop met weglatingstekens aan het einde van de rij voor het abonnement in de lijst die u wilt verplaatsen.

    ![Verplaatsen](media/management-groups/move_small.png)
5. Selecteer **verplaatsen**
6. Selecteer op het menu dat verschijnt, de **beheergroep bovenliggende**.

    ![Verplaatsen](media/management-groups/move_small_context.png)
7. Selecteer **opslaan**

### <a name="move-subscriptions-in-powershell"></a>Verplaatsen van abonnementen in PowerShell
Voor het verplaatsen van een abonnement in PowerShell, moet u de opdracht Add-AzureRmManagementGroupSubscription gebruiken.  

```azurepowershell-interactive
New-AzureRmManagementGroupSubscription -GroupName Contoso -SubscriptionId 12345678-1234-1234-1234-123456789012
```

De koppeling tussen verwijderen en het abonnement en de beheergroep gebruikt u de opdracht Remove-AzureRmManagementGroupSubscription.

```azurepowershell-interactive
Remove-AzureRmManagementGroupSubscription -GroupName Contoso -SubscriptionId 12345678-1234-1234-1234-123456789012
```

### <a name="move-subscriptions-in-azure-cli"></a>Verplaatsen van abonnementen in de Azure CLI
Voor het verplaatsen van een abonnement in de CLI, gebruikt u de opdracht toevoegen. 

```azure-cli
C:\> az account management-group add --group-name Contoso --subscription 12345678-1234-1234-1234-123456789012
```

Als het abonnement uit de beheergroep wilt verwijderen, gebruik het abonnement opdracht verwijderen.  

```azure-cli
C:\> az account management-group remove --group-name Contoso --subscription 12345678-1234-1234-1234-123456789012
```

---

## <a name="move-management-groups-in-the-hierarchy"></a>Beheergroepen verplaatsen in de hiërarchie  
Wanneer u een bovenliggende groep alle onderliggende resources met beheergroepen, abonnementen, resourcegroepen en resources verplaatsen met de bovenliggende verplaatst.   

### <a name="move-management-groups-in-the-portal"></a>Beheergroepen verplaatsen in de portal

1. Meld u aan bij de [Azure-portal](https://portal.azure.com)
2. Selecteer **alle services** > **beheergroepen** 
3. Selecteer de beheergroep die u van plan bent als bovenliggende.      
5. Selecteer boven aan de pagina **bestaande toevoegen**.
6. Selecteer in het menu dat geopend, de **brontype** van het item dat u probeert te verplaatsen dat **beheergroep**.
7. Selecteer de beheergroep met de juiste ID en naam.

    ![verplaatsen](media/management-groups/add_context.png)
8. Selecteer **opslaan**

### <a name="move-management-groups-in-powershell"></a>Beheergroepen in PowerShell verplaatsen
Gebruik de opdracht Update AzureRmManagementGroup in PowerShell om te verplaatsen van een beheergroep onder een andere groep.  

```powershell
C:\> Update-AzureRmManagementGroup -GroupName Contoso  -ParentName ContosoIT
```  
### <a name="move-management-groups-in-azure-cli"></a>Beheergroepen verplaatsen in de Azure CLI
Gebruik de opdracht update voor het verplaatsen van een beheergroep met Azure CLI. 

```azure-cli
C:/> az account management-group udpate --group-name Contoso --parent-id "Contoso Tenant" 
``` 

---

## <a name="next-steps"></a>Volgende stappen 
Zie voor meer informatie over beheergroepen: 
- [Uw resources te organiseren met Azure-beheergroepen ](management-groups-overview.md)
- [Beheergroepen te organiseren Azure-resources maken](management-groups-create.md)
- [Installeer de Azure Powershell-module](https://www.powershellgallery.com/packages/AzureRM.ManagementGroups/0.0.1-preview)
- [Bekijk de REST-API-specificaties](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/managementgroups/resource-manager/Microsoft.Management/preview/2018-01-01-preview)
- [De uitbreiding voor de Azure CLI installeren](https://docs.microsoft.com/cli/azure/extension?view=azure-cli-latest#az_extension_list_available)
