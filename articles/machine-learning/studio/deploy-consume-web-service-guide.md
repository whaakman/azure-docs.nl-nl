---
title: 'Azure Machine Learning-webservices: De implementatie en het verbruik | Microsoft Docs'
description: Bronnen voor het implementeren en gebruiken van web-services.
services: machine-learning
documentationcenter: 
author: garyericson
manager: raymondl
editor: 
ms.assetid: 47635376-d1f4-4ea4-a6af-bd1f99f69a69
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: v-donglo
ms.openlocfilehash: b0afbd54ddad4cc4be2b35c85d81abe90c717692
ms.sourcegitcommit: 42ee5ea09d9684ed7a71e7974ceb141d525361c9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/09/2017
---
# <a name="azure-machine-learning-web-services-deployment-and-consumption"></a>Azure Machine Learning-webservices: implementatie en verbruik
U kunt Azure Machine Learning gebruiken machine learning werkstromen en modellen als webservices te implementeren. Deze webservices kunnen vervolgens worden gebruikt voor het aanroepen van de machine learning-modellen van toepassingen via Internet te doen voorspellingen in realtime of in de batchmodus. Omdat de webservices RESTful, kunt u ze aanroepen uit verschillende programmeertalen en platforms, zoals .NET en Java, en toepassingen, zoals Excel.

De volgende secties vindt u koppelingen naar procedures, code en documentatie waarmee u op weg.

## <a name="deploy-a-web-service"></a>Een webservice implementeren

### <a name="with-azure-machine-learning-studio"></a>Met Azure Machine Learning Studio
Machine Learning Studio en de portal voor Microsoft Azure Machine Learning-webservices te implementeren en beheren van een webservice zonder code te schrijven.

De volgende koppelingen vindt algemene informatie over het implementeren van een nieuwe webservice:

* Zie voor een overzicht over het implementeren van een nieuwe webservice die gebaseerd op Azure Resource Manager [implementeren van een nieuwe webservice](publish-a-machine-learning-web-service.md).
* Zie voor een overzicht over het implementeren van een webservice [Azure Machine Learning-webservice implementeren](publish-a-machine-learning-web-service.md).
* Zie voor een volledig overzicht over het maken en implementeren van een webservice [scenario stap 1: een Machine Learning-werkruimte maken](walkthrough-1-create-ml-workspace.md).
* Zie voor specifieke voorbeelden die een webservice implementeert:

  * [Scenario-stap 5: De Azure Machine Learning-webservice implementeren](walkthrough-5-publish-web-service.md)
  * [Een webservice implementeren op meerdere regio 's](how-to-deploy-to-multiple-regions.md)

### <a name="with-web-services-resource-provider-apis-azure-resource-manager-apis"></a>Met web services resourceprovider API's (Azure Resource Manager-API's)
De Azure Machine Learning-resourceprovider voor webservices kunt implementatie en beheer van web-services met behulp van de REST API-aanroepen. Zie voor meer informatie de [Machine Learning Web Service (REST)](/rest/api/machinelearning/index) verwijzing.

<!-- [Machine Learning Web Service (REST)](https://msdn.microsoft.com/library/azure/mt767538.aspx) reference. -->


### <a name="with-powershell-cmdlets"></a>Met PowerShell-cmdlets
Azure Machine Learning-resourceprovider voor webservices kunt implementatie en beheer van web-services met behulp van PowerShell-cmdlets.

Voor het gebruik van de cmdlets, u moet eerst aanmelden bij uw Azure-account uit binnen de PowerShell-omgeving met behulp van de [Add-AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) cmdlet. Als u niet bekend bent met het aanroepen van PowerShell-opdrachten die zijn gebaseerd op Resource Manager, raadpleegt [Azure PowerShell gebruiken met Azure Resource Manager](../../azure-resource-manager/powershell-azure-resource-manager.md#log-in-to-your-azure-account).

Voor het exporteren van uw Voorspellend experiment [deze voorbeeldcode](https://github.com/ritwik20/AzureML-WebServices). Nadat u het .exe-bestand van de code hebt gemaakt, kunt u het volgende typen:

    C:\<folder>\GetWSD <experiment-url> <workspace-auth-token>

De toepassing wordt uitgevoerd, maakt een web service JSON-sjabloon. De sjabloon wilt gebruiken voor het implementeren van een webservice, moet u de volgende informatie toevoegen:

* Naam van het opslagaccount en de sleutel

    U kunt de opslagaccountnaam ophalen en sleutel uit de [Azure-portal](https://portal.azure.com/).
* Het streven plan-ID

    U krijgt de abonnement-ID van de [Azure Machine Learning-webservices](https://services.azureml.net) portal aanmelden en de naam van een abonnement op.

Toevoegen aan het JSON-sjabloon als onderliggende elementen van de *eigenschappen* knooppunt op hetzelfde niveau als het *MachineLearningWorkspace* knooppunt.

Hier volgt een voorbeeld:

    "StorageAccount": {
            "name": "YourStorageAccountName",
            "key": "YourStorageAccountKey"
    },
    "CommitmentPlan": {
        "id": "subscriptions/YouSubscriptionID/resourceGroups/YourResourceGroupID/providers/Microsoft.MachineLearning/commitmentPlans/YourPlanName"
    }

Zie de volgende artikelen en voorbeeldcode voor meer informatie:

* [Azure Machine Learning-Cmdlets](https://msdn.microsoft.com/library/azure/mt767952.aspx) -verwijzingen op MSDN
* Voorbeeld [scenario](https://github.com/raymondlaghaeian/azureml-webservices-arm-powershell/blob/master/sample-commands.txt) op GitHub

## <a name="consume-the-web-services"></a>De webservices
### <a name="from-the-azure-machine-learning-web-services-ui-testing"></a>Van de Azure Machine Learning-webservices UI (testen)
U kunt testen uw webservice via de portal voor Azure Machine Learning-webservices. Dit omvat het testen van de aanvraag en antwoord-service (RR's) en interfaces batchuitvoeringsservice (BES).

* [Een nieuwe webservice implementeren](publish-a-machine-learning-web-service.md)
* [Een Azure Machine Learning-webservice implementeren](publish-a-machine-learning-web-service.md)
* [Scenario-stap 5: De Azure Machine Learning-webservice implementeren](walkthrough-5-publish-web-service.md)

### <a name="from-excel"></a>Vanuit Excel
U kunt een Excel-sjabloon die de webservice verbruikt downloaden:

* [Een Azure Machine Learning-webservice vanuit Excel gebruiken](consuming-from-excel.md)
* [Excel-invoegtoepassing voor Azure Machine Learning-webservices](excel-add-in-for-web-services.md)

### <a name="from-a-rest-based-client"></a>Van een REST-client
Azure Machine Learning-webservices zijn RESTful-API's. U kunt deze API's van verschillende platformen, zoals .NET, Python, R, Java, enzovoort verbruiken. De **verbruiken** pagina voor uw webservice op de [portal voor Microsoft Azure Machine Learning-webservices](https://services.azureml.net) voorbeeldcode waarmee u aan de slag kunt heeft. Zie [How to consume an Azure Machine Learning Web service](consume-web-services.md) (Azure Machine Learning-webservice gebruiken) voor meer informatie.
