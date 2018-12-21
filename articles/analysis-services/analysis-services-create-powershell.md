---
title: 'Snelstart: Een Azure Analysis Services-server maken met behulp van PowerShell | Microsoft Docs'
description: Leer hoe u een Azure Analysis Services-server maakt met behulp van PowerShell
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: quickstart
ms.date: 10/18/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 3971311bff90d0b005acd516336e13766028cccf
ms.sourcegitcommit: 2469b30e00cbb25efd98e696b7dbf51253767a05
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "53001561"
---
# <a name="quickstart-create-a-server---powershell"></a>Snelstartgids: Een server maken - PowerShell

In deze snelstart wordt beschreven hoe u PowerShell vanaf de opdrachtregel kunt gebruiken om een Azure Analysis Services-server te maken in uw Azure-abonnement.

## <a name="prerequisites"></a>Vereisten

- **Azure-abonnement**: Ga naar [gratis proefversie van Azure](https://azure.microsoft.com/offers/ms-azr-0044p/) om een account te maken.
- **Azure Active Directory**: Uw abonnement moet zijn gekoppeld aan een Azure Active Directory-tenant en u moet een account hebben in de betreffende map. Raadpleeg voor meer informatie [Verificatie en gebruikersmachtigingen](analysis-services-manage-users.md).
- **Azure PowerShell-moduleversie 4.0 of later**. Voer ` Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Zie [Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps) om de module te installeren of te upgraden.

## <a name="import-azurermanalysisservices-module"></a>AzureRm.AnalysisServices-module importeren

U gebruikt de  [AzureRM.AnalysisServices](https://www.powershellgallery.com/packages/AzureRM.AnalysisServices)-module om een server in het abonnement te maken. Laad de AzureRm.AnalysisServices-module in de PowerShell-sessie.

```powershell
Import-Module AzureRM.AnalysisServices
```

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij uw Azure-abonnement met behulp van de opdracht [Connect-AzureRmAccount](/powershell/module/azurerm.profile/connect-azurermaccount). Volg de aanwijzingen op het scherm.

```powershell
Connect-AzureRmAccount
```

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Een [Azure-resourcegroep](../azure-resource-manager/resource-group-overview.md) is een logische container waarin Azure-resources worden geïmplementeerd en als groep beheerd. Wanneer u de server maakt, moet u een resourcegroep opgeven in uw abonnement. Als u nog geen resourcegroep hebt, maakt u een nieuwe met behulp van de opdracht [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). In het volgende voorbeeld wordt een resourcegroep met de naam `myResourceGroup` gemaakt in de regio US - west.

```powershell
New-AzureRmResourceGroup -Name "myResourceGroup" -Location "WestUS"
```

## <a name="create-a-server"></a>Een server maken

Maak een nieuwe server met behulp van de opdracht [New-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesserver). In het volgende voorbeeld wordt een server met de naam myServer gemaakt in myResourceGroup, in de regio VS - West, in de (gratis) D1-laag, en wordt philipc@adventureworks.com opgegeven als serverbeheerder.

```powershell
New-AzureRmAnalysisServicesServer -ResourceGroupName "myResourceGroup" -Name "myserver" -Location WestUS -Sku D1 -Administrator "philipc@adventure-works.com"
```

## <a name="clean-up-resources"></a>Resources opschonen

U kunt de server uit het abonnement verwijderen met behulp van de opdracht [Remove-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesserver). Verwijder de server niet als u met andere snelstartgidsen en zelfstudies in deze verzameling doorgaat. In het volgende voorbeeld wordt de server die u in de vorige stap hebt gemaakt, verwijderd.


```powershell
Remove-AzureRmAnalysisServicesServer -Name "myserver" -ResourceGroupName "myResourceGroup"
```

## <a name="next-steps"></a>Volgende stappen

In deze snelstart hebt u geleerd hoe u een server in uw Azure-abonnement maakt met behulp van PowerShell. Nu u een server hebt gemaakt, kunt u deze beveiligen door een serverfirewall te configureren. (Optioneel) U kunt ook rechtstreeks vanuit de portal een eenvoudig voorbeeldgegevensmodel toevoegen aan de server. Een voorbeeldmodel is handig als u meer wilt weten over het configureren van modeldatabaserollen en het testen van clientverbindingen. Als u meer wilt weten, gaat u verder met de zelfstudie waarin u leert een voorbeeldmodel toe te voegen.

> [!div class="nextstepaction"]
> [Snelstartgids: Een serverfirewall configureren - Portal](analysis-services-qs-firewall.md)      
> [!div class="nextstepaction"]
> [Zelfstudie: Een voorbeeldmodel toevoegen aan uw server](analysis-services-create-sample-model.md)