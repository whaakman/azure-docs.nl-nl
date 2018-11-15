---
title: Opnieuw trainen van een nieuwe Azure Machine Learning-webservice met PowerShell | Microsoft Docs
description: Leer hoe u programmatisch opnieuw trainen van een model en bijwerken van de webservice voor het gebruik van het zojuist getrainde model in Azure Machine Learning met behulp van de Machine Learning Management PowerShell-cmdlets.
services: machine-learning
documentationcenter: ''
author: YasinMSFT
ms.author: yahajiza
manager: hjerez
editor: cgronlun
ms.assetid: 3953a398-6174-4d2d-8bbd-e55cf1639415
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/28/2017
ms.openlocfilehash: abef73a477355d89236b1f62cffc1f3d05ba03d0
ms.sourcegitcommit: db2cb1c4add355074c384f403c8d9fcd03d12b0c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2018
ms.locfileid: "51687228"
---
# <a name="retrain-a-new-resource-manager-based-web-service-using-the-machine-learning-management-powershell-cmdlets"></a>Opnieuw trainen van een nieuwe Resource Manager gebaseerde webservice met behulp van de Machine Learning Management PowerShell-cmdlets
Wanneer u een nieuwe webservice opnieuw trainen, kunt u de definitie van de voorspellende webservice om te verwijzen naar het nieuwe getrainde model bijwerken.

## <a name="prerequisites"></a>Vereisten
U moet instellen van een opleidingsexperiment en een Voorspellend experiment, zoals wordt weergegeven in [opnieuw trainen Machine Learning-modellen programmatisch](retrain-models-programmatically.md).

> [!IMPORTANT]
> De Voorspellend experiment moet worden geïmplementeerd als een Azure Resource Manager (nieuw) op basis van machine learning-webservice.
> Voor het implementeren van een nieuwe webservice moet u voldoende machtigingen hebben in het abonnement waarvoor u de webservice implementeert. Zie voor meer informatie, [beheren van een webservice met behulp van de Azure Machine Learning-webserviceportal](manage-new-webservice.md).

Zie voor meer informatie over webservices implementeren, [een Azure Machine Learning-webservice implementeren](publish-a-machine-learning-web-service.md).

Dit proces vereist dat u de Cmdlets van Azure Machine Learning hebt geïnstalleerd. Zie voor informatie over het installeren van de Machine Learning-cmdlets de [Azure Machine Learning-Cmdlets](https://docs.microsoft.com/powershell/module/azurerm.machinelearning/) verwijzingen op MSDN.

De volgende informatie opgehaald uit de retraining uitvoer:

* BaseLocation
* RelativeLocation

De stappen zijn:

1. Aanmelden bij uw Azure Resource Manager-account.
2. De definitie van de web service ophalen
3. De definitie van de Web Service exporteren als JSON
4. De verwijzing naar de ilearner-blob in de JSON worden bijgewerkt.
5. Het importeren van de JSON in de definitie van een Web-Service
6. De webservice bijwerken met nieuwe Web-servicedefinitie

## <a name="sign-in-to-your-azure-resource-manager-account"></a>Meld u aan bij uw Azure Resource Manager-account
U moet zich eerst aanmelden bij uw Azure-account uit in de PowerShell-omgeving met de [Connect-AzureRmAccount](/powershell/module/azurerm.profile/connect-azurermaccount) cmdlet.

## <a name="get-the-web-service-definition"></a>De definitie van de Web Service ophalen
De Web-Service vervolgens ophalen door het aanroepen van de [Get-AzureRmMlWebService](https://docs.microsoft.com/powershell/module/azurerm.machinelearning/get-azurermmlwebservice) cmdlet. De definitie van de Web-Service is een interne weergave van het getrainde model van de webservice en kan niet rechtstreeks worden gewijzigd. Zorg ervoor dat u de definitie van de Web-Service voor uw Voorspellend experiment en niet uw trainingsexperiment ophalen.

    $wsd = Get-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'

Om te bepalen van de naam van de resourcegroep van een bestaande webservice, voer de cmdlet Get-AzureRmMlWebService zonder parameters om de webservices in uw abonnement weer te geven. Ga naar de webservice en zoek vervolgens naar de web service-ID. De naam van de resourcegroep is het vierde element-id, direct na de *resourceGroups* element. In het volgende voorbeeld is de naam van de resourcegroep standaard-MachineLearning-SouthCentralUS.

    Properties : Microsoft.Azure.Management.MachineLearning.WebServices.Models.WebServicePropertiesForGraph
    Id : /subscriptions/<subscription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237
    Name : RetrainSamplePre.2016.8.17.0.3.51.237
    Location : South Central US
    Type : Microsoft.MachineLearning/webServices
    Tags : {}

U kunt ook om te bepalen van de naam van de resourcegroep van een bestaande webservice, meld u aan bij de portal voor Microsoft Azure Machine Learning-webservices. Selecteer de web-service. Naam van de resourcegroep is het vijfde element van de URL van de webservice direct na de *resourceGroups* element. In het volgende voorbeeld is de naam van de resourcegroep standaard-MachineLearning-SouthCentralUS.

    https://services.azureml.net/subscriptions/<subscription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237


## <a name="export-the-web-service-definition-as-json"></a>De definitie van de Web Service exporteren als JSON
Wijzigen van de definitie van het getrainde model gebruik van de zojuist getrainde Model, moet u eerst gebruiken de [Export-AzureRmMlWebService](https://docs.microsoft.com/powershell/module/azurerm.machinelearning/export-azurermmlwebservice) cmdlet om te exporteren naar een bestand met JSON-indeling.

    Export-AzureRmMlWebService -WebService $wsd -OutputFile "C:\temp\mlservice_export.json"

## <a name="update-the-reference-to-the-ilearner-blob-in-the-json"></a>De verwijzing naar de ilearner-blob in de JSON worden bijgewerkt.
Zoek in de assets, het [getrainde model], bij te werken de *uri* waarde in de *locationInfo* knooppunt met de URI van de ilearner-blob. De URI wordt gegenereerd door combineren de *BaseLocation* en de *RelativeLocation* uit de uitvoer van de aanroep opnieuw trainen BES. Hiermee wordt het pad om te verwijzen naar het nieuwe getrainde model.

     "asset3": {
        "name": "Retrain Samp.le [trained model]",
        "type": "Resource",
        "locationInfo": {
          "uri": "https://mltestaccount.blob.core.windows.net/azuremlassetscontainer/baca7bca650f46218633552c0bcbba0e.ilearner"
        },
        "outputPorts": {
          "Results dataset": {
            "type": "Dataset"
          }
        }
      },

## <a name="import-the-json-into-a-web-service-definition"></a>Het importeren van de JSON in de definitie van een Web-Service
Moet u de [importeren AzureRmMlWebService](https://docs.microsoft.com/powershell/module/azurerm.machinelearning/import-azurermmlwebservice) cmdlet voor het converteren van het gewijzigde JSON-bestand naar de definitie van een Web-Service die u gebruiken kunt om bij te werken van de definitie van de Web Service.

    $wsd = Import-AzureRmMlWebService -InputFile "C:\temp\mlservice_export.json"


## <a name="update-the-web-service-with-new-web-service-definition"></a>De webservice bijwerken met nieuwe Web-servicedefinitie
Gebruik tot slot [Update AzureRmMlWebService](https://docs.microsoft.com/powershell/module/azurerm.machinelearning/update-azurermmlwebservice) cmdlet voor het bijwerken van de definitie van de Web-Service.

    Update-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'  -ServiceUpdates $wsd

## <a name="summary"></a>Samenvatting
Met behulp van de Machine Learning PowerShell-cmdlets voor beheer, kunt u het getrainde model van een voorspellende webservice inschakelen van scenario's zoals bijwerken:

* Periodieke model opnieuw trainen met nieuwe gegevens.
* Distributie van een model voor klanten met het doel om ze opnieuw trainen het model met behulp van hun eigen gegevens.

