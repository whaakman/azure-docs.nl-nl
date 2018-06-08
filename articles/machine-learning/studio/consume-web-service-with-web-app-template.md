---
title: Een Machine Learning-webservice gebruiken met een web-app-sjabloon | Microsoft Docs
description: Gebruik de sjabloon voor een web-apps in Azure Marketplace gebruiken voor een Voorspellend webservice in Azure Machine Learning.
keywords: Web-service, uitoefening, REST API, machine learning
services: machine-learning
documentationcenter: ''
author: YasinMSFT
ms.author: yahajiza
manager: hjerez
editor: cgronlun
ms.assetid: e0d71683-61b9-4675-8df5-09ddc2f0d92d
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.openlocfilehash: 03729a5b94b355869367e7f356e299f9afe38f75
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/07/2018
ms.locfileid: "34834995"
---
# <a name="consume-an-azure-machine-learning-web-service-by-using-a-web-app-template"></a>Een Azure Machine Learning-webservice met behulp van een web-app-sjabloon gebruiken

U kunt een Voorspellend model ontwikkelen en als een Azure-web-service implementeren met behulp van:
- Azure Machine Learning Studio.
- Hulpprogramma's zoals R- of Python. 

Daarna kunt u het operationalized model openen met behulp van een REST-API.

Er zijn een aantal manieren om de REST-API gebruiken en toegang tot de webservice. U kunt bijvoorbeeld een toepassing in C#, R of Python schrijven met behulp van de voorbeeldcode voor u gegenereerd tijdens de implementatie van de webservice. (De voorbeeldcode is beschikbaar in de [Machine Learning-webservices portal](https://services.azureml.net/quickstart) of in het dashboard van de web service in Machine Learning Studio.) Of u kunt de voorbeeld Microsoft Excel-werkmap die voor u gemaakt op hetzelfde moment.

Maar de snelste en gemakkelijkste manier toegang krijgen tot uw webservice is via het web-app sjablonen die beschikbaar zijn in de [Azure Marketplace](https://azure.microsoft.com/marketplace/web-applications/all/).

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="azure-machine-learning-web-app-templates"></a>Azure Machine Learning web-app-sjablonen
De web-app beschikbare sjablonen in Azure Marketplace kunnen maken van een aangepaste web-app dat de invoergegevens van uw webservice en de verwachte resultaten kent. U hoeft te doen is de web-app-toegang geven tot uw web-service en -gegevens en doet de rest van de sjabloon.

Twee sjablonen zijn beschikbaar:

* [Azure ML-aanvragen en antwoorden Service Web-App-sjabloon](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlaspnettemplateforrrs/)
* [Azure ML-Batch uitvoering Web App servicesjabloon](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/)

Elke sjabloon maakt u een voorbeeld van een ASP.NET-toepassing met behulp van de API-URI en sleutel van uw web-service. De sjabloon implementeert vervolgens de toepassing als een website op Azure. 

De aanvraag en antwoord-Service (RR's)-sjabloon maakt u een web-app die u gebruiken kunt één rij met gegevens verzenden naar de webservice moet een enkelvoudig resultaat wordt verkregen. De Service Batch-uitvoering (BES)-sjabloon maakt u een web-app die u gebruiken kunt voor het verzenden van veel rijen van gegevens naar meerdere resultaten krijgt.

Er is geen codering is vereist om deze sjablonen te gebruiken. U gewoon de API-sleutel en de URI en de sjabloon wordt de toepassing voor u gemaakt.

Ophalen van de API-sleutel en de URI van de aanvraag voor een webservice:

1. In de [Web Services-portal](https://services.azureml.net/quickstart), selecteer **webservices** aan de bovenkant. Of Selecteer voor een klassieke webservice **klassieke webservices**.
2. Selecteer de webservice die u wilt openen.
3. Voor een klassieke webservice, selecteert u het eindpunt dat u wilt openen.
4. Selecteer **verbruiken** aan de bovenkant.
5. Kopieer de primaire of secundaire sleutel en opslaan.
6. Als u een RSS-sjabloon maakt, kopieert u de **aanvragen en antwoorden** URI en op te slaan. Als u een BES-sjabloon maakt, kopieert u de **batchaanvragen** URI en op te slaan.


## <a name="how-to-use-the-request-response-service-template"></a>Het gebruik van de aanvraag en antwoord-servicesjabloon
Volg deze stappen voor het gebruik van de sjabloon RRS web app, zoals wordt weergegeven in het volgende diagram.

![Proces RRS websjabloon gebruiken][image1]


<!--    ![API Key][image3] -->

<!-- This value will look like this:
   
        https://ussouthcentral.services.azureml.net/workspaces/<workspace-id>/services/<service-id>/execute?api-version=2.0&details=true
   
    ![Request URI][image4] -->

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer **nieuw**, zoekt en selecteert u **Web-App voor Azure ML aanvragen en antwoorden Service**, en selecteer vervolgens **maken**. 
3. In de **maken** deelvenster:
   
   * Een unieke naam voor uw web-app geven. De URL van de web-app wordt deze gevolgd door naam worden **. azurewebsites.net**. Een voorbeeld is **http://carprediction.azurewebsites.net**.
   * Selecteer de Azure-abonnement en services, waaronder uw webservice wordt uitgevoerd.
   * Selecteer **Maken**.
     
   ![Een web-app maken][image5]

4. Wanneer Azure klaar is met de web-app implementeren, selecteert u de **URL** pagina op de instellingen voor web-app in Azure of Voer de URL in een webbrowser. Voer bijvoorbeeld **http://carprediction.azurewebsites.net**.
5. Wanneer de web-app voor het eerst uitvoert, wordt u gevraagd een voor de **API Post URL** en **API-sleutel**. Voer de waarden die u eerder hebt opgeslagen (aanvraag-URI en API-sleutel, respectievelijk). Selecteer **indienen**.
     
   ![Voer na URI en API-sleutel][image6]

6. De web-app wordt weergegeven de **Web-App-configuratie** pagina met de huidige instellingen voor web-service. Hier kunt u wijzigingen aanbrengen in de instellingen die gebruikmaakt van de web-app.
   
   > [!NOTE]
   > Deze instellingen alleen wijzigt, worden ze voor deze web-app. Deze wijzigen de standaardinstellingen van uw web-service niet. Bijvoorbeeld, als u de tekst in wijzigen **beschrijving** Hier wordt de beschrijving weergegeven op het web service-dashboard in Machine Learning Studio niet gewijzigd.
   > 
   > 
   
    Wanneer u bent klaar, selecteert u **wijzigingen opslaan**, en selecteer vervolgens **gaat u naar de startpagina**.

7. U kunt waarden om te verzenden naar uw webservice invoeren vanaf de startpagina. Selecteer **indienen** wanneer u klaar bent, en het resultaat wordt geretourneerd.

Als u terugkeren wilt naar de **configuratie** pagina, gaat u naar de **setting.aspx** pagina van de web-app. Bijvoorbeeld, gaat u naar **http://carprediction.azurewebsites.net/setting.aspx**. U wordt gevraagd de API-sleutel opnieuw invoeren. U moet die toegang tot de pagina en de instellingen bijwerken.

U kunt stoppen, starten of te verwijderen van de web-app in de Azure portal als elke andere web-app. Als deze wordt uitgevoerd, kunt u bladeren naar het webadres thuis en voer nieuwe waarden.

## <a name="how-to-use-the-batch-execution-service-template"></a>Het gebruik van de Batch-Service voor uitvoering van sjabloon
U kunt de sjabloon BES web-app op dezelfde manier als de RSS-sjabloon gebruiken. Het verschil is dat u de gemaakte web-app gebruiken kunt voor meerdere rijen met gegevens te verzenden en ontvangen van meerdere resultaten.

De ingevoerde waarden voor een webservice van de batch-uitvoering kunnen afkomstig zijn uit Azure Storage of een lokaal bestand. De resultaten worden opgeslagen in een Azure storage-container. U moet dus een Azure storage-container voor het opslaan van de resultaten die de web-app retourneert. U moet ook bereid u voor uw invoergegevens.

![Proces BES websjabloon gebruiken][image2]

1. Volg de procedure voor het maken van de web-app voor BES als de RSS-sjabloon voor. Maar in dit geval gaat u naar [Azure ML Batch uitvoering Web App servicesjabloon](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/) voor de sjabloon BES in Azure Marketplace. Selecteer **Web-App maken**.

2. Waar u de resultaten die zijn opgeslagen, typ de doelgegevens container op de startpagina van de web-app. Ook opgeven hoe de web-app in de invoerwaarden kom: in een lokaal bestand of in een Azure storage-container.
   Selecteer **indienen**.
   
   ![Storage-gegevens][image7]

Een pagina met taakstatus wordt weergegeven in de web-app. Als de taak is voltooid, krijgt u de locatie van de resultaten in Azure Blob-opslag. U hebt ook de optie van de resultaten naar een lokaal bestand downloaden.

## <a name="for-more-information"></a>Voor meer informatie
Voor meer informatie over:

* Het maken van een machine learning-experiment met Machine Learning Studio, Zie [uw eerste experiment maken in Azure Machine Learning Studio](create-experiment.md).
* Het implementeren van uw machine learning-experiment als een webservice, Zie [een Azure Machine Learning-webservice implementeren](publish-a-machine-learning-web-service.md).
* Zie andere manieren om uw webservice [gebruiken van een Azure Machine Learning-webservice](consume-web-services.md).

[image1]: media/consume-web-service-with-web-app-template/rrs-web-template-flow.png
[image2]: media/consume-web-service-with-web-app-template/bes-web-template-flow.png
[image3]: media/consume-web-service-with-web-app-template/api-key.png
[image4]: media/consume-web-service-with-web-app-template/post-uri.png
[image5]: media/consume-web-service-with-web-app-template/create-web-app.png
[image6]: media/consume-web-service-with-web-app-template/web-service-info.png
[image7]: media/consume-web-service-with-web-app-template/storage.png
