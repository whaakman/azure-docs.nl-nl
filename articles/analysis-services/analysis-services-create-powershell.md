---
title: Een Azure Analysis Services-server maken met behulp van PowerShell | Microsoft Docs
description: Leer hoe u een Azure Analysis Services-server maakt met behulp van PowerShell
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
ms.assetid: 
ms.service: analysis-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 08/01/2017
ms.author: owend
ms.custom: mvc
ms.openlocfilehash: cb42fd3ed51364cf478848cc51ebbb2f175e96d2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-an-azure-analysis-services-server-by-using-powershell"></a>Een Azure Analysis Services-server maken met behulp van PowerShell

In deze snelstartgids wordt beschreven hoe u PowerShell in de opdrachtregel kunt gebruiken om een Azure Analysis Services-server te maken in een [Azure-resourcegroep](../azure-resource-manager/resource-group-overview.md) in uw Azure-abonnement.

Voor deze taak is Azure PowerShell-moduleversie 4.0 of hoger vereist. Voer ` Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Zie [Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps) om de module te installeren of te upgraden. 

> [!NOTE]
> Het maken van een server kan zorgen voor een nieuwe factureerbare service. Zie [Prijzen van Analysis Services](https://azure.microsoft.com/pricing/details/analysis-services/) voor meer informatie.

## <a name="prerequisites"></a>Vereisten
U hebt het volgende nodig om deze snelstartgids te voltooien:

* **Azure-abonnement**: ga naar [gratis proefversie van Azure](https://azure.microsoft.com/offers/ms-azr-0044p/) om een account te maken.
* **Azure Active Directory**: uw abonnement moet worden gekoppeld aan een Azure Active Directory-tenant en u moet een account hebben in de betreffende map. Raadpleeg voor meer informatie [Verificatie en gebruikersmachtigingen](analysis-services-manage-users.md).

## <a name="import-azurermanalysisservices-module"></a>AzureRm.AnalysisServices-module importeren
U gebruikt de  [AzureRM.AnalysisServices](https://www.powershellgallery.com/packages/AzureRM.AnalysisServices)-module om een server in het abonnement te maken. Laad de AzureRm.AnalysisServices-module in de PowerShell-sessie.

```powershell
Import-Module AzureRM.AnalysisServices
```

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij uw Azure-abonnement met behulp van de opdracht [Add-AzureRmAccount](/powershell/module/azurerm.profile/add-azurermaccount). Volg de aanwijzingen op het scherm.

```powershell
Add-AzureRmAccount
```

## <a name="create-a-resource-group"></a>Een resourcegroep maken
 
Een [Azure-resourcegroep](../azure-resource-manager/resource-group-overview.md) is een logische container waarin Azure-resources worden geïmplementeerd en als groep beheerd. Wanneer u de server maakt, moet u een resourcegroep opgeven in uw abonnement. Als u nog geen resourcegroep hebt, maakt u een nieuwe met behulp van de opdracht [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). In het volgende voorbeeld wordt een resourcegroep met de naam `myResourceGroup` gemaakt in de regio VS - West.

```powershell
New-AzureRmResourceGroup -Name "myResourceGroup" -Location "West US"
```

## <a name="create-a-server"></a>Een server maken

Maak een nieuwe server met behulp van de opdracht [New-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesserver). In het volgende voorbeeld wordt een server met de naam myServer gemaakt in myResourceGroup, in de regio VS - West, in de laag D1, en wordt philipc@adventureworks.com opgegeven als serverbeheerder.

```powershell
New-AzureRmAnalysisServicesServer -ResourceGroupName "myResourceGroup" -Name "myServer" -Location West US -Sku D1 -Administrator "philipc@adventure-works.com"
```

## <a name="clean-up-resources"></a>Resources opschonen

U kunt de server uit het abonnement verwijderen met behulp van de opdracht [Remove-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesserver). Verwijder de server niet als u met andere snelstartgidsen en zelfstudies in deze verzameling doorgaat. In het volgende voorbeeld wordt de server die u in de vorige stap hebt gemaakt, verwijderd.


```powershell
Remove-AzureRmAnalysisServicesServer -Name "myServer" -ResourceGroupName "myResourceGroup"
```

## <a name="next-steps"></a>Volgende stappen
[Azure Analysis Services beheren met PowerShell](analysis-services-powershell.md)   
[Een model implementeren vanuit SSDT](analysis-services-deploy.md)   
[Een model maken in Azure Portal](analysis-services-create-model-portal.md)