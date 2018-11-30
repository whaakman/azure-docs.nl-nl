---
title: Gebruik van een Machine Learning webservice met een web-appsjabloon - Azure Machine Learning Studio | Microsoft Docs
description: Een web-appsjabloon gebruiken in Azure Marketplace een voorspellende webservice in Azure Machine Learning gebruiken.
keywords: webservice, uitoefening, REST-API voor machine learning
services: machine-learning
documentationcenter: ''
author: ericlicoding
ms.custom: (previous ms.author=yahajiza, author=YasinMSFT)
ms.author: amlstudiodocs
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
ms.openlocfilehash: 2c0bba4a442b73b82ebc271c3516f65954d6d8a4
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/26/2018
ms.locfileid: "52311565"
---
# <a name="consume-an-azure-machine-learning-web-service-by-using-a-web-app-template"></a>Een Azure Machine Learning-webservice met behulp van een web-appsjabloon gebruiken

U kunt een Voorspellend model te ontwikkelen en implementeren als een Azure-web-service met behulp van:
- Azure Machine Learning Studio.
- Hulpprogramma's, zoals R of Python. 

Hierna kunt u toegang tot de ingezette model met behulp van een REST-API.

Er zijn een aantal manieren om te gebruiken van de REST-API en toegang tot de webservice. U kunt bijvoorbeeld een toepassing schrijven C#, R of Python met behulp van de voorbeeldcode voor u gegenereerd tijdens de implementatie van de webservice. (De voorbeeldcode is beschikbaar in de [Machine Learning-webserviceportal](https://services.azureml.net/quickstart) of op het webservicedashboard in Machine Learning Studio.) Of u kunt de voorbeeld Microsoft Excel-werkmap die voor u gemaakt op hetzelfde moment.

Maar de snelste en eenvoudigste manier voor toegang tot uw webservice is via de web-app-sjablonen beschikbaar zijn in de [Azure Marketplace](https://azure.microsoft.com/marketplace/web-applications/all/).

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="azure-machine-learning-web-app-templates"></a>Azure Machine Learning web app-sjablonen
De web-appsjablonen die beschikbaar zijn in de Azure Marketplace kunnen maken van een aangepaste web-app die de invoergegevens van de webservice en de verwachte resultaten. U hoeft alleen is de web-app-toegang geven tot uw web-service en de gegevens en doet de rest van de sjabloon.

Twee sjablonen zijn beschikbaar:

* [Azure ML Request Response Service-Web-Appsjabloon](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlaspnettemplateforrrs/)
* [Azure ML-Batch tot uitvoering van Service-Web-Appsjabloon](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/)

Elke sjabloon maakt u een voorbeeld-ASP.NET-toepassing met behulp van de API-URI en sleutel van uw web-service. De sjabloon implementeert vervolgens de toepassing als een website naar Azure. 

De Request Response Service (RRS)-sjabloon maakt u een web-app die u gebruiken kunt voor het verzenden van één rij met gegevens met de web-service naar een enkelvoudig resultaat wordt verkregen. De Batch Execution Service (BES)-sjabloon maakt u een web-app die u gebruiken kunt voor het verzenden van veel rijen met gegevens om op te halen van meerdere resultaten.

Geen codering is vereist om deze sjablonen te gebruiken. U gewoon de API-sleutel en de URI en de sjabloon wordt de toepassing voor u gemaakt.

Voor het ophalen van de API-sleutel en de aanvraag-URI voor een webservice:

1. In de [Web Services-portal](https://services.azureml.net/quickstart), selecteer **webservices** aan de bovenkant. Of Selecteer voor een klassieke webservice **klassieke webservices**.
2. Selecteer de webservice die u wilt openen.
3. Voor een klassieke webservice, selecteert u het eindpunt dat u wilt openen.
4. Selecteer **verbruiken** aan de bovenkant.
5. Kopieer de primaire of secundaire sleutel en sla deze.
6. Als u een RRS-sjabloon maakt, kopieert u de **Request Response** URI en op te slaan. Als u een BES-sjabloon maakt, kopieert u de **batchaanvragen** URI en op te slaan.


## <a name="how-to-use-the-request-response-service-template"></a>Het gebruik van de Request Response-Service-sjabloon
Volg deze stappen voor het gebruik van de RRS-sjabloon voor web-app, zoals wordt weergegeven in het volgende diagram.

![Proces RRS websjabloon gebruiken][image1]


<!--    ![API Key][image3] -->

<!-- This value will look like this:
   
        https://ussouthcentral.services.azureml.net/workspaces/<workspace-id>/services/<service-id>/execute?api-version=2.0&details=true
   
    ![Request URI][image4] -->

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer **nieuw**, zoek en selecteer **Web-App voor Azure ML Request Response Service**, en selecteer vervolgens **maken**. 
3. In de **maken** deelvenster:
   
   * Geef uw web-app een unieke naam. De URL van de web-app wordt deze gevolgd door de naam dan **. azurewebsites.net**. Een voorbeeld is **http://carprediction.azurewebsites.net**.
   * Selecteer de Azure-abonnement en services waarmee u uw webservice wordt uitgevoerd.
   * Selecteer **Maken**.
     
   ![Een web-app maken][image5]

4. Wanneer Azure klaar is met de web-app implementeren, selecteert u de **URL** pagina op de instellingen van de web-app in Azure, of de URL opgeven in een webbrowser. Voer bijvoorbeeld **http://carprediction.azurewebsites.net**.
5. Wanneer de web-app eerst wordt uitgevoerd, wordt u gevraagd voor het **URL van de API-Post** en **API-sleutel**. Voer de waarden die u eerder hebt opgeslagen (aanvraag-URI en API-sleutel, respectievelijk). Selecteer **indienen**.
     
   ![Voer na URI en API-sleutel][image6]

6. De web-app wordt weergegeven de **Web App-configuratie** pagina met de huidige instellingen van de web-service. Hier kunt u wijzigingen aanbrengen in de instellingen die gebruikmaakt van de web-app.
   
   > [!NOTE]
   > Deze instellingen alleen wijzigt, worden ze voor deze web-app. Het verandert niet met de standaardinstellingen van uw web-service. Bijvoorbeeld, als u de tekst in **beschrijving** hier, wordt de beschrijving wordt weergegeven op het webservicedashboard in Machine Learning Studio niet wijzigen.
   > 
   > 
   
    Wanneer u klaar bent, selecteert u **wijzigingen opslaan**, en selecteer vervolgens **gaat u naar de startpagina**.

7. Vanaf de startpagina, kunt u waarden om te verzenden naar uw web-service. Selecteer **indienen** wanneer u klaar bent, en het resultaat wordt geretourneerd.

Als u terugkeren wilt naar de **configuratie** pagina, Ga naar de **setting.aspx** pagina van de web-app. Bijvoorbeeld, gaat u naar **http://carprediction.azurewebsites.net/setting.aspx**. U wordt gevraagd naar de API-sleutel opnieuw invoeren. U moet die op de pagina openen en bijwerken van de instellingen.

U kunt stoppen, starten of verwijderen van de web-app in Azure portal, zoals elke andere web-app. U kunt als deze wordt uitgevoerd, bladert u naar de startpagina webadres en voer nieuwe waarden.

## <a name="how-to-use-the-batch-execution-service-template"></a>Het gebruik van de Batchuitvoeringsservice-sjabloon
U kunt de BES-web-appsjabloon gebruiken op dezelfde manier als de RRS-sjabloon. Het verschil is dat u de gemaakte web-app gebruiken kunt voor meerdere rijen met gegevens verzenden en ontvangen van meerdere resultaten.

De ingevoerde waarden voor de webservice van een batch-uitvoering kunnen afkomstig zijn uit Azure Storage of een lokaal bestand. De resultaten worden opgeslagen in een Azure storage-container. Dus, moet u een Azure storage-container voor het opslaan van de resultaten die de web-app als resultaat geeft. U moet ook bereid u voor uw invoergegevens.

![Proces BES websjabloon gebruiken][image2]

1. Volg dezelfde procedure voor het maken van de BES-web-app als die voor de RRS-sjabloon. Maar in dit geval gaat u naar [Azure ML Batch uitvoering Service Web-Appsjabloon](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/) de BES-sjabloon geopend in de Azure Marketplace. Selecteer **Web-App maken**.

2. Om op te geven waar u de resultaten die zijn opgeslagen, voert u de bestemming container-gegevens in op de startpagina van de web-app. Ook opgeven waar de web-app in de invoerwaarden vind: in een lokaal bestand of in een Azure storage-container.
   Selecteer **indienen**.
   
   ![Opslaggegevens][image7]

Een pagina met de taakstatus wordt weergegeven in de web-app. Als de taak is voltooid, krijgt u de locatie van de resultaten in Azure Blob-opslag. U hebt ook de mogelijkheid van het downloaden van de resultaten naar een lokaal bestand.

## <a name="for-more-information"></a>Voor meer informatie
Voor meer informatie over:

* Het maken van een machine learning-experiment met Machine Learning Studio, Zie [uw eerste experiment maken in Azure Machine Learning Studio](create-experiment.md).
* Hoe het implementeren van uw machine learning-experiment als een webservice, raadpleegt u [implementeren van een Azure Machine Learning-webservice](publish-a-machine-learning-web-service.md).
* Andere manieren om uw webservice, Zie [hoe u een Azure Machine Learning-webservice gebruiken](consume-web-services.md).

[image1]: media/consume-web-service-with-web-app-template/rrs-web-template-flow.png
[image2]: media/consume-web-service-with-web-app-template/bes-web-template-flow.png
[image3]: media/consume-web-service-with-web-app-template/api-key.png
[image4]: media/consume-web-service-with-web-app-template/post-uri.png
[image5]: media/consume-web-service-with-web-app-template/create-web-app.png
[image6]: media/consume-web-service-with-web-app-template/web-service-info.png
[image7]: media/consume-web-service-with-web-app-template/storage.png
