---
title: Een nieuwe Azure Machine Learning-webservice met PowerShell Retrain | Microsoft Docs
description: Informatie over het programmatisch opnieuw trainen van een model en het bijwerken van de webservice voor het gebruik van het zojuist getrainde model in Azure Machine Learning met Machine Learning Management PowerShell-cmdlets.
services: machine-learning
documentationcenter: 
author: garyericson
manager: raymondlaghaeian
editor: 
ms.assetid: 3953a398-6174-4d2d-8bbd-e55cf1639415
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/28/2017
ms.author: v-donglo
ms.openlocfilehash: 630e9958d5083300fdf7910c5fdd47989b0376ad
ms.sourcegitcommit: 42ee5ea09d9684ed7a71e7974ceb141d525361c9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/09/2017
---
# <a name="retrain-a-new-resource-manager-based-web-service-using-the-machine-learning-management-powershell-cmdlets"></a>Een nieuwe Resource Manager gebaseerde webservice met Machine Learning Management PowerShell-cmdlets opnieuw trainen
Wanneer u een nieuwe webservice opnieuw trainen, kunt u de voorspellende webservicedefinitie om te verwijzen naar het nieuwe getrainde model bijwerken.  

## <a name="prerequisites"></a>Vereisten
U moet instellen van een trainingsexperiment en een Voorspellend experiment zoals weergegeven in [Retrain Machine Learning-modellen programmatisch](retrain-models-programmatically.md). 

> [!IMPORTANT]
> De Voorspellend experiment moet worden geïmplementeerd als een Azure Resource Manager (nieuw) op basis van machine learning-webservice. Voor het implementeren van een nieuwe webservice moet u voldoende machtigingen hebben in het abonnement waaraan u de webservice implementeren. Zie voor meer informatie [beheren van een webservice via de portal voor Azure Machine Learning-webservices](manage-new-webservice.md). 

Zie voor meer informatie over webservices implementeren [Azure Machine Learning-webservice implementeren](publish-a-machine-learning-web-service.md).

Dit proces vereist dat u de Cmdlets van Azure Machine Learning hebt geïnstalleerd. Zie voor informatie over het installeren van de Machine Learning-cmdlets de [Azure Machine Learning-Cmdlets](https://msdn.microsoft.com/library/azure/mt767952.aspx) -verwijzingen op MSDN.

De volgende informatie uit de uitvoer van de retraining gekopieerd:

* BaseLocation
* RelativeLocation

De stappen zijn:

1. Aanmelden bij uw Azure Resource Manager-account.
2. Definitie van de webservice ophalen
3. Exporteren van de webservicedefinitie als JSON
4. De verwijzing naar de blob ilearner in de JSON bijwerken.
5. De JSON importeren in de definitie van een Web-Service
6. De webservice bijwerken met definitie van een nieuwe Web-Service

## <a name="sign-in-to-your-azure-resource-manager-account"></a>Aanmelden bij uw account voor Azure Resource Manager
U moet zich eerst aanmelden bij uw Azure-account uit binnen de PowerShell-omgeving met de [Add-AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) cmdlet.

## <a name="get-the-web-service-definition"></a>Definitie van de webservice ophalen
De Web-Service vervolgens ophalen door het aanroepen van de [Get-AzureRmMlWebService](https://msdn.microsoft.com/library/mt619267.aspx) cmdlet. Definitie van de webservice is een interne representatie van het getrainde model van de webservice en kan niet rechtstreeks worden gewijzigd. Zorg ervoor dat u voor uw Voorspellend experiment en niet uw trainingsexperiment definitie van de webservice ophaalt.

    $wsd = Get-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'

Om te bepalen van de naam van de resourcegroep van een bestaande webservice, voert u de cmdlet Get-AzureRmMlWebService zonder parameters om weer te geven van de webservices in uw abonnement. Ga naar de webservice en zoek vervolgens naar de web service-ID. De naam van de resourcegroep is het vierde element in de ID, direct na de *resourceGroups* element. In het volgende voorbeeld is de naam van de resourcegroep standaard-MachineLearning-SouthCentralUS.

    Properties : Microsoft.Azure.Management.MachineLearning.WebServices.Models.WebServicePropertiesForGraph
    Id : /subscriptions/<subscription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237
    Name : RetrainSamplePre.2016.8.17.0.3.51.237
    Location : South Central US
    Type : Microsoft.MachineLearning/webServices
    Tags : {}

U kunt ook om te bepalen van de naam van de resourcegroep van een bestaande webservice, meld u aan bij de portal voor Microsoft Azure Machine Learning-webservices. Selecteer de webservice. Naam van de resourcegroep is het vijfde element van de URL van de webservice direct na de *resourceGroups* element. In het volgende voorbeeld is de naam van de resourcegroep standaard-MachineLearning-SouthCentralUS.

    https://services.azureml.net/subscriptions/<subcription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237


## <a name="export-the-web-service-definition-as-json"></a>Exporteren van de webservicedefinitie als JSON
Wijzig de definitie van het getrainde model gebruiken de zojuist getraind Model, moet u de [Export AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767935.aspx) cmdlet exporteren naar bestand met een JSON-indeling.

    Export-AzureRmMlWebService -WebService $wsd -OutputFile "C:\temp\mlservice_export.json"

## <a name="update-the-reference-to-the-ilearner-blob-in-the-json"></a>De verwijzing naar de blob ilearner in de JSON bijwerken.
Zoek in de activa [getraind model], werkt de *uri* waarde in de *locationInfo* knooppunt met de URI van de ilearner-blob. De URI wordt gegenereerd door combineren de *BaseLocation* en de *RelativeLocation* uit de uitvoer van de retraining aanroep BES. Hiermee wordt het pad om te verwijzen naar het nieuwe getrainde model bijgewerkt.

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

## <a name="import-the-json-into-a-web-service-definition"></a>De JSON importeren in de definitie van een Web-Service
Moet u de [importeren AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767925.aspx) cmdlet om te converteren van het gewijzigde JSON-bestand naar de definitie van een Web-Service die u gebruiken kunt voor het bijwerken van definitie van de webservice.

    $wsd = Import-AzureRmMlWebService -InputFile "C:\temp\mlservice_export.json"


## <a name="update-the-web-service-with-new-web-service-definition"></a>De webservice bijwerken met definitie van een nieuwe Web-Service
Gebruik tot slot [Update AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767922.aspx) cmdlet bij te werken van definitie van de webservice.

    Update-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'  -ServiceUpdates $wsd

## <a name="summary"></a>Samenvatting
De Machine Learning PowerShell-cmdlets kunt u het getrainde model van een Voorspellend webservice inschakelen van scenario's zoals bijwerken:

* Periodieke model retraining met nieuwe gegevens.
* Distributie van een model voor klanten met het doel van zodat ze opnieuw trainen van het model met hun eigen gegevens.

