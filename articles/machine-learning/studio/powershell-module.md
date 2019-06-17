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
ms.date: 04/25/2019
ms.openlocfilehash: bee42f8a9582908963c0eef95a2fd04742cd425e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65205664"
---
# <a name="powershell-modules-for-azure-machine-learning-studio"></a>PowerShell-modules voor Azure Machine Learning Studio

Met behulp van PowerShell-modules, kunt u programmatisch beheren uw Studio resources en -assets zoals werkruimten, gegevenssets en -webservices.

U kunt werken met Studio resources met behulp van drie Powershell-modules:

* [Azure PowerShell-Az](#az-rm) uitgebracht in 2018, bevat alle functionaliteit van AzureRM, hoewel u met de namen van de verschillende cmdlets
* [AzureRM](#az-rm) uitgebracht 2016, vervangen door PowerShell Az
* [Azure Machine Learning PowerShell-klassiek](#classic) in 2016 uitgebracht

Hoewel deze PowerShell-modules hebben een aantal overeenkomsten, is elk ontworpen voor bepaalde scenario's. Dit artikel beschrijft de verschillen tussen de PowerShell-modules en helpt die u bepalen welke u moet kiezen.  

Controleer de [ondersteuning tabel](#support-table) hieronder om te zien welke bronnen worden ondersteund door elke module. 

## <a name="az-rm"></a> Azure PowerShell-Az en AzureRM

AZ is nu de beoogde PowerShell-module voor interactie met Azure en bevat alle vorige functionaliteit van de AzureRM. AzureRM blijven ontvangen van oplossingen voor problemen, maar ontvangt geen nieuwe-cmdlets of functies.  AZ en AzureRM beide beheren van oplossingen die zijn geïmplementeerd met behulp van de **Azure Resource Manager** implementatiemodel. Deze bronnen omvatten Studio-werkruimten en "New" Studio-webservices. 

PowerShell klassiek kan worden geïnstalleerd naast Az of AzureRM voor beide typen resource 'Nieuw' en 'klassieke'. Het verdient echter niet beschikken over Az- en AzureRM geïnstalleerd op hetzelfde moment. Microsoft raadt Az om te beslissen tussen Az en AzureRM, aan voor alle toekomstige implementaties.  Meer informatie over Az ten opzichte van de azurerm-module en het migratiepad in [Inleiding tot de Azure PowerShell Az](https://docs.microsoft.com/powershell/azure/new-azureps-module-az).

Als u wilt beginnen met Az, volgt u de [installatie-instructies voor Azure Az](https://docs.microsoft.com/powershell/azure/install-az-ps).

## <a name="classic"></a> PowerShell klassiek

De Studio [klassieke PowerShell-module](https://aka.ms/amlps) kunt u voor het beheren van resources die zijn geïmplementeerd met behulp van de **klassieke implementatiemodel**. Deze bronnen omvatten Studio gebruiker activa, webservices 'klassieke' en 'klassieke' web service-eindpunten.

Microsoft raadt echter aan dat u het Resource Manager-implementatiemodel voor alle toekomstige resources gebruiken voor het vereenvoudigen van de implementatie en het beheer van resources. Als u meer informatie over de implementatiemodellen wilt, raadpleegt u de [Azure Resource Manager en klassieke implementatie](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-model) artikel.

Om te beginnen met PowerShell klassiek, downloadt de [releasepakket](https://github.com/hning86/azuremlps/releases) van GitHub en volgt de [instructies voor de installatie](https://github.com/hning86/azuremlps/blob/master/README.md). De instructies wordt uitgelegd hoe u de gedownloade/uitgepakte DLL deblokkeren en vervolgens importeren in uw PowerShell-omgeving.

PowerShell klassiek kan worden geïnstalleerd naast Az of AzureRM voor beide typen resource 'Nieuw' en 'klassieke'.

## <a name="support-table"></a> PowerShell-ondersteuning-tabel


| | **Az** |  **PowerShell klassiek** |
| --- | --- | --- |
| Werkruimten maken/verwijderen | [Resource Manager-sjablonen](https://docs.microsoft.com/azure/machine-learning/studio/deploy-with-resource-manager-template) |  |
| Werkruimte toegezegde abonnementen beheren | [New-AzMlCommitmentPlan](https://docs.microsoft.com/powershell/module/az.machinelearning/new-azmlcommitmentplan) | |
| Gebruikers van deze werkruimte beheren |  | [Add-AmlWorkspaceUsers](https://github.com/hning86/azuremlps#add-amlworkspaceusers)|
| Webservices beheren | [New-AzMlWebService](https://docs.microsoft.com/powershell/module/az.machinelearning/new-azmlwebservice) <br>('Nieuw' web services)|| [New-AmlWebService](https://github.com/hning86/azuremlps#manage-classic-web-service) <br>('klassieke' web services) |
| Web service-eindpunten/sleutels beheren |  [Get-AzMlWebServiceKey](https://docs.microsoft.com/powershell/module/az.machinelearning/get-azmlwebservicekey)|  [Add-AmlWebServiceEndpoint](https://github.com/hning86/azuremlps#manage-classic-web-servcie-endpoint)|
| Gebruiker gegevenssets/getrainde modellen beheren| | [Get-AmlDataset](https://github.com/hning86/azuremlps#manage-user-assets-dataset-trained-model-transform) |
| Gebruiker experimenten beheren |  | [Start-AmlExperiment](https://github.com/hning86/azuremlps#manage-experiment) |
| Aangepaste modules beheren | | [New-AmlCustomModule](https://github.com/hning86/azuremlps#manage-custom-module) |


## <a name="next-steps"></a>Volgende stappen
Raadpleeg de volledige documentatie deze PowerShell-module:
* [PowerShell klassiek](https://aka.ms/amlps)
* [Azure PowerShell Az](https://docs.microsoft.com/powershell/module/az.machinelearning/#machine_learning)
