---
title: PowerShell-modules voor Machine Learning Studio
titleSuffix: Azure Machine Learning Studio
description: PowerShell gebruiken om te maken en beheren van Azure Machine Learning Studio-werkruimten, experimenten, webservices en meer.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: xiaoharper
ms.author: amlstudiodocs
ms.date: 01/25/2019
ms.openlocfilehash: bd3a82f326cdf7f51e8842e45333ff2bd647c260
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58092749"
---
# <a name="powershell-modules-for-azure-machine-learning-studio"></a>PowerShell-modules voor Azure Machine Learning Studio

Met behulp van PowerShell-modules, kunt u programmatisch beheren uw Studio resources en -assets zoals werkruimten, gegevenssets en -webservices.

U kunt werken met Studio resources met behulp van drie Powershell-modules:

* [Azure PowerShell-Az](#az-rm) uitgebracht in 2018, bevat alle functionaliteit van AzureRM, hoewel u met de namen van de verschillende cmdlets
* [AzureRM](#az-rm) in 2016 uitgebracht
* [Azure Machine Learning PowerShell-klassiek](#classic) in 2016 uitgebracht

Hoewel deze modules hebben een aantal overeenkomsten, is elk ontworpen voor bepaalde scenario's. Dit artikel beschrijft de verschillen tussen de PowerShell-modules en helpt die u bepalen welke u moet kiezen.

## <a name="choosing-modules"></a> Modules kiezen

Kiezen tussen de beschikbare PowerShell-modules, is afhankelijk van het type van de resources die u beheert.

Controleer de [ondersteuning tabel](#support-table) hieronder om te zien welke bronnen worden ondersteund door elke module. Omdat PowerShell klassiek kan in combinatie met Az of AzureRM worden geïnstalleerd, kunt u twee modules installeren en betrekking hebben op alle resourcetypen (klassieke met Az of klassieke met AzureRM)

Het verdient echter niet beschikken over Az- en AzureRM geïnstalleerd op hetzelfde moment. Microsoft raadt Az om te beslissen tussen Az en AzureRM, aan voor alle toekomstige implementaties. Gebruik AzureRm alleen als er speciale omstandigheden in uw omgeving waarvoor dit vereist is.

Zie voor meer informatie over de verschillen tussen Az en AzureRM, evenals in onze opgegeven migratiepad, onze [Inleiding tot de Azure PowerShell Az.](https://docs.microsoft.com/powershell/azure/new-azureps-module-az)

## <a name="az-rm"></a> Azure PowerShell-Az en AzureRM

AZ en AzureRM beide beheren van oplossingen die zijn geïmplementeerd met behulp van de **Azure Resource Manager** implementatiemodel. Deze bronnen omvatten Studio-werkruimten en nieuwe Studio-webservices. Voor het beheren van resources die zijn geïmplementeerd met behulp van het klassieke implementatiemodel, moet u de klassieke PowerShell-module. Als u meer informatie over de implementatiemodellen wilt, raadpleegt u de [Azure Resource Manager en klassieke implementatie](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-model) artikel.

AZ is nu de beoogde PowerShell-module voor interactie met Azure en bevat alle vorige functionaliteit van de AzureRM. AzureRM blijven ontvangen van oplossingen voor problemen, maar ontvangt geen nieuwe-cmdlets of functies. Hoewel er een upgradepad van AzureRM, als u problemen met Az ondervindt bij het werken met Studio, het probleem rapporteren en neerkomt dat u de AzureRM.

Als u wilt beginnen met Az, volgt u de [installatie-instructies voor Azure Az](https://docs.microsoft.com/powershell/azure/install-az-ps).

## <a name="classic"></a> PowerShell klassiek

De Studio [klassieke PowerShell-module](https://aka.ms/amlps) kunt u voor het beheren van resources die zijn geïmplementeerd met behulp van de **klassieke implementatiemodel**. Deze bronnen omvatten Studio gebruiker activa, klassieke webservices en klassieke web service-eindpunten.

Microsoft raadt echter aan dat u het Resource Manager-implementatiemodel voor alle nieuwe resources gebruiken voor het vereenvoudigen van de implementatie en het beheer van resources. Als u meer informatie over de implementatiemodellen wilt, raadpleegt u de [Azure Resource Manager en klassieke implementatie](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-model) artikel.

Om te beginnen met PowerShell klassiek, downloadt de [releasepakket](https://github.com/hning86/azuremlps/releases) van GitHub en volgt de [instructies voor de installatie](https://github.com/hning86/azuremlps/blob/master/README.md). De instructies wordt uitgelegd hoe u de gedownloade/uitgepakte DLL deblokkeren en vervolgens importeren in uw PowerShell-omgeving.

## <a name="support-table"></a> PowerShell-ondersteuning-tabel

 **Studio-werkruimten** | **Az** |  **AzureRM** | **PowerShell klassiek** |
| --- | --- | --- | --- |
| Werkruimten maken/verwijderen | [Resource Manager-sjablonen](https://docs.microsoft.com/azure/machine-learning/studio/deploy-with-resource-manager-template) | [Resource Manager-sjablonen](https://docs.microsoft.com/azure/machine-learning/studio/deploy-with-resource-manager-template) |  |
| Gebruikers van deze werkruimte beheren |  |  | [Add-AmlWorkspaceUsers](https://github.com/hning86/azuremlps#add-amlworkspaceusers)|
| Toegezegde abonnementen beheren | [New-AzMlCommitmentPlan](https://docs.microsoft.com/powershell/module/az.machinelearning/new-azmlcommitmentplan) | [New-AzureRmMlCommitmentPlan](https://docs.microsoft.com/powershell/module/azurerm.machinelearning/new-azurermmlcommitmentplan) |
|||
| **Webservices** | **Az** | **AzureRM** | **PowerShell klassiek** |
| Webservices beheren | [New-AzMlWebService](https://docs.microsoft.com/powershell/module/az.machinelearning/new-azmlwebservice) <br> (Nieuwe webservices) | [New-AzureRmMlWebService](https://docs.microsoft.com/powershell/module/azurerm.machinelearning/new-azurermmlwebservice) <br> (Nieuwe webservices) |[New-AmlWebService](https://github.com/hning86/azuremlps#manage-classic-web-service) <br> (klassieke webservices) |
| Eindpunten/sleutels beheren |  [Get-AzMlWebServiceKeys](https://docs.microsoft.com/powershell/module/az.machinelearning/get-azmlwebservicekeys) <br> (Nieuwe webservices) | [Get-AzureRmMlWebServiceKeys](https://docs.microsoft.com/powershell/module/azurerm.machinelearning/get-azurermmlwebservicekeys) <br> (Nieuwe webservices) | [Add-AmlWebServiceEndpoint](https://github.com/hning86/azuremlps#manage-classic-web-servcie-endpoint) <br> (klassieke webservices) |
|||
| **Gebruiker activa** | **Az** | **AzureRM** | **PowerShell klassiek** |
| Gegevenssets/getrainde modellen beheren |  |  | [Get-AmlDataset](https://github.com/hning86/azuremlps#manage-user-assets-dataset-trained-model-transform) |
| Experimenten beheren |  |  | [Start-AmlExperiment](https://github.com/hning86/azuremlps#manage-experiment) |
| Aangepaste modules beheren |  |  | [New-AmlCustomModule](https://github.com/hning86/azuremlps#manage-custom-module) |


## <a name="next-steps"></a>Volgende stappen
Volg deze koppelingen voor volledige documentatie over de PowerShell-modules:
* [AzureRM](https://docs.microsoft.com/powershell/module/azurerm.machinelearning/#machine_learning)
* [PowerShell klassiek](https://aka.ms/amlps)
* [Azure PowerShell Az](https://docs.microsoft.com/powershell/module/az.machinelearning/#machine_learning)
