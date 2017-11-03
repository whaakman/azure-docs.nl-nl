---
title: Een Machine Learning-webservice met de sjabloon voor een web-apps gebruiken | Microsoft Docs
description: Gebruik de sjabloon voor een web-apps in Azure Marketplace gebruiken voor een Voorspellend webservice in Azure Machine Learning.
keywords: Web-service, uitoefening, REST API, machine learning
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: e0d71683-61b9-4675-8df5-09ddc2f0d92d
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: garye;raymondl
ms.openlocfilehash: 1c182403409966923440f359cb2514af7b7df9f3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="consume-an-azure-machine-learning-web-service-with-a-web-app-template"></a>Een Azure Machine Learning-webservice gebruiken met een web-app-sjabloon

Eenmaal hebt u uw Voorspellend model ontwikkeld en geïmplementeerd als een Azure-web-service met behulp van Machine Learning Studio of hulpprogramma's zoals R- of Python gebruikt, u toegang tot het operationalized model met behulp van een REST-API.

Er zijn een aantal manieren om de REST-API gebruiken en toegang tot de webservice. U kunt bijvoorbeeld een toepassing schrijven in C#, R of Python met behulp van de voorbeeldcode voor u gegenereerd tijdens de implementatie van de webservice (beschikbaar in de [Machine Learning Web Services-Portal](https://services.azureml.net/quickstart) of in het dashboard van de web-service op computer Learning Studio). Of u kunt de voorbeeld Microsoft Excel-werkmap die voor u gemaakt op hetzelfde moment.

Maar de snelste en gemakkelijkste manier toegang krijgen tot uw webservice is via het Web-App sjablonen die beschikbaar zijn in de [Azure Web App Marketplace](https://azure.microsoft.com/marketplace/web-applications/all/).

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="the-azure-machine-learning-web-app-templates"></a>De Azure Machine Learning-sjablonen voor Web-App
De web-app beschikbare sjablonen in Azure Marketplace kunnen maken van een aangepaste web-app dat de invoergegevens van uw webservice en de verwachte resultaten kent. U hoeft te doen is de web-app-toegang geven tot uw web-service en -gegevens en doet de rest van de sjabloon.

Twee sjablonen zijn beschikbaar:

* [Azure ML-aanvragen en antwoorden Service Web-App-sjabloon](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlaspnettemplateforrrs/)
* [Azure ML-Batch uitvoering Web App servicesjabloon](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/)

Elke sjabloon een voorbeeldtoepassing ASP.NET, met behulp van de API-URI en sleutel van uw web-service maakt en implementeert dit als een website naar Azure. De aanvraag en antwoord-Service (RR's)-sjabloon maakt u een web-app waarmee u één rij met gegevens verzenden naar de webservice moet een enkelvoudig resultaat wordt verkregen. De Service Batch-uitvoering (BES)-sjabloon maakt u een web-app waarmee u veel rijen gegevens ophalen van meerdere resultaten te verzenden.

Er is geen codering is vereist om deze sjablonen te gebruiken. U gewoon de API-sleutel en de URI en de sjabloon wordt de toepassing voor u gemaakt.

Ophalen van de API-sleutel en de aanvraag-URI voor een webservice:

1. In de [Web Services-Portal](https://services.azureml.net/quickstart), voor een nieuwe webservice, klikt u op **webservices** aan de bovenkant. Of voor een klassieke web service Klik **klassieke webservices**.
2. Klik op de webservice die u wilt openen.
3. Een klassieke-webservice, klikt u op het eindpunt dat u wilt openen.
4. Klik op **verbruiken** aan de bovenkant.
5. Kopieer de **primaire** of **secundaire sleutel** en op te slaan.
6. Als u een aanvraag en antwoord-Service (RR's)-sjabloon maakt, kopieert u de **aanvragen en antwoorden** URI en op te slaan. Als u een Batch uitvoering Service (BES)-sjabloon maakt, kopieert u de **batchaanvragen** URI en op te slaan.


## <a name="how-to-use-the-request-response-service-rrs-template"></a>Het gebruik van de sjabloon voor de aanvraag en antwoord-Service (RR's)
Volg deze stappen voor het gebruik van de sjabloon RRS web app, zoals wordt weergegeven in het volgende diagram.

![Proces RRS websjabloon gebruiken][image1]


<!--    ![API Key][image3] -->

<!-- This value will look like this:
   
        https://ussouthcentral.services.azureml.net/workspaces/<workspace-id>/services/<service-id>/execute?api-version=2.0&details=true
   
    ![Request URI][image4] -->

1. Ga naar de [Azure-portal](https://portal.azure.com), **aanmelding**, klikt u op **nieuw**, zoekt en selecteert u **Web-App voor Azure ML aanvragen en antwoorden Service**, klikt u vervolgens op  **Maak**. 
   
   * Een unieke naam voor uw web-app geven. De URL van de web-app wordt deze gevolgd door naam worden `.azurewebsites.net.` bijvoorbeeld:`http://carprediction.azurewebsites.net.`
   * Selecteer de Azure-abonnement en services, waaronder uw webservice wordt uitgevoerd.
   * Klik op **Create**.
     
     ![Een web-app maken][image5]

4. Wanneer de web-app implementeren, Azure is voltooid, klikt u op de **URL** pagina op de instellingen voor web-app in Azure of Voer de URL in een webbrowser. Bijvoorbeeld: `http://carprediction.azurewebsites.net.`
5. Wanneer de web-app voor het eerst wordt uitgevoerd, wordt u gevraagt voor de **API Post URL** en **API-sleutel**.
   Voer de waarden die u eerder hebt opgeslagen (**aanvraag-URI** en **API-sleutel**respectievelijk).
     
     Klik op **indienen**.
     
     ![Voer Post URI en API-sleutel][image6]

6. De web-app wordt weergegeven de **Web-App-configuratie** pagina met de huidige instellingen voor web-service. Hier kunt u wijzigingen aanbrengen in de instellingen die worden gebruikt door de web-app.
   
   > [!NOTE]
   > Deze instellingen alleen wijzigt, worden ze voor deze web-app. Deze wijzigen de standaardinstellingen van uw web-service niet. Als u bijvoorbeeld de **beschrijving** Hier wordt de beschrijving weergegeven op het web service-dashboard in Machine Learning Studio niet gewijzigd.
   > 
   > 
   
    Wanneer u bent klaar, klikt u op **wijzigingen opslaan**, en klik vervolgens op **gaat u naar de startpagina**.

7. U kunt waarden om te verzenden naar uw webservice invoeren vanaf de startpagina. Klik op **indienen** wanneer u klaar bent, en het resultaat wordt geretourneerd.

Als u terugkeren wilt naar de **configuratie** pagina, gaat u naar de `setting.aspx` pagina van de web-app. Bijvoorbeeld: `http://carprediction.azurewebsites.net/setting.aspx.` wordt u gevraagd de API-sleutel opnieuw invoeren: u hebt nodig die toegang tot de pagina en de instellingen bijwerken.

U kunt stoppen, starten of te verwijderen van de web-app in de Azure portal als elke andere web-app. U kunt bladeren naar het oorspronkelijke webadres en voer nieuwe waarden, zolang deze wordt uitgevoerd.

## <a name="how-to-use-the-batch-execution-service-bes-template"></a>Het gebruik van de sjabloon Batch uitvoering Service (BES)
Behalve dat de web-app die gemaakt, kunt u meerdere rijen met gegevens te verzenden en ontvangen van meerdere resultaten, kunt u de sjabloon BES web-app op dezelfde manier als de RSS-sjabloon.

De ingevoerde waarden voor een webservice van de batch-uitvoering kunnen afkomstig zijn van Azure-opslag of een lokaal bestand; de resultaten worden opgeslagen in een Azure storage-container.
Daarom moet u een Azure storage-container voor het opslaan van de resultaten van de web-app en moet u bereid u voor uw invoergegevens.

![Proces BES websjabloon gebruiken][image2]

1. Volg dezelfde procedure voor het maken van de web-app voor BES als voor de sjabloon RRS behalve gaat u naar [Azure ML Batch uitvoering Web App servicesjabloon](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/) de sjabloon BES op Azure Marketplace openen en klik op **Web-App maken**.

2. Waar u de resultaten die zijn opgeslagen, typ de doelgegevens container op de startpagina van de web-app. Ook kunt u opgeven waar de web-app de invoerwaarden in een lokaal bestand of een Azure storage-container kan krijgen.
   Klik op **indienen**.
   
    ![Storage-gegevens][image7]

De web-app wordt een pagina met de taakstatus weergegeven.
Wanneer de taak is voltooid krijgt u de locatie van de resultaten in Azure blob-opslag. U hebt ook de optie van de resultaten naar een lokaal bestand downloaden.

## <a name="for-more-information"></a>Voor meer informatie
Voor meer informatie over...

* het maken van een machine learning-experiment met Machine Learning Studio, Zie [uw eerste experiment maken in Azure Machine Learning Studio](create-experiment.md)
* het implementeren van uw machine learning-experiment als een webservice, Zie [een Azure Machine Learning-webservice implementeren](publish-a-machine-learning-web-service.md)
* Zie andere manieren om uw webservice [gebruiken van een Azure Machine Learning-webservice](consume-web-services.md)

[image1]: media/consume-web-service-with-web-app-template/rrs-web-template-flow.png
[image2]: media/consume-web-service-with-web-app-template/bes-web-template-flow.png
[image3]: media/consume-web-service-with-web-app-template/api-key.png
[image4]: media/consume-web-service-with-web-app-template/post-uri.png
[image5]: media/consume-web-service-with-web-app-template/create-web-app.png
[image6]: media/consume-web-service-with-web-app-template/web-service-info.png
[image7]: media/consume-web-service-with-web-app-template/storage.png
