---
title: 'Zelfstudie: Een machine learning-model met de visuele interface implementeren'
titleSuffix: Azure Machine Learning service
description: Meer informatie over het bouwen van een predictive analytics-oplossing in de visuele interface voor Azure Machine Learning-service. Trainen, beoordelen, en implementeren van een machine learning-model met behulp van slepen en neerzetten van modules. In deze zelfstudie is deel twee van een tweedelige reeks over lineaire regressie gebruiken prijzen van auto's voorspellen.
author: peterlu
ms.author: peterlu
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
ms.date: 04/06/2019
ms.openlocfilehash: e9ece81b1f663910fb3c051bd94c13c54ffa8470
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/02/2019
ms.locfileid: "65026863"
---
# <a name="tutorial-deploy-a-machine-learning-model-with-the-visual-interface"></a>Zelfstudie: Een machine learning-model met de visuele interface implementeren

In deze zelfstudie maakt uitvoeren u een uitgebreide blik op het ontwikkelen van een voorspellende oplossing in de visuele interface voor Azure Machine Learning-service. Deze zelfstudie is **deel twee van een tweedelige reeks**. In [deel één van de zelfstudie](ui-tutorial-automobile-price-train-score.md), getraind, beoordeeld en een model om de prijzen van auto's voorspellen geëvalueerd. In dit gedeelte van de zelfstudie hebt u:

> [!div class="checklist"]
> * Een model voor implementatie voorbereiden
> * Een webservice implementeren
> * Een webservice testen
> * Een webservice beheren
> * De webservice gebruiken

## <a name="prerequisites"></a>Vereisten

Voltooi [deel één van de zelfstudie](ui-tutorial-automobile-price-train-score.md).

## <a name="prepare-for-deployment"></a>Implementatie voorbereiden

Als u wilt geven anderen kans op het gebruik van het voorspellende model ontwikkeld in deze zelfstudie, kunt u deze implementeren als een Azure-web-service.

U hebt tot nu toe is experimenteren met het trainen van uw model. Nu is het tijd voor het genereren van nieuwe voorspellingen op basis van de invoer van de gebruiker.

Voorbereiding voor de implementatie is een proces in twee stappen:  

1. Converteer de *trainingsexperiment* die u hebt gemaakt in een *Voorspellend experiment*
1. Het voorspellende experiment implementeren als webservice

U wilt mogelijk maakt u eerst een kopie van het experiment hiervoor **OpslaanAls** aan de onderkant van het experimentcanvas.

### <a name="convert-the-training-experiment-to-a-predictive-experiment"></a>Het trainingsexperiment converteren naar een voorspellend experiment

Om dit model klaar is voor implementatie, deze trainingsexperiment te converteren naar een Voorspellend experiment. Dit betekent meestal dat drie stappen:

1. Sla het model dat u hebt getraind en uw trainingsmodules vervangen
1. Werk het experiment bij door modules te verwijderen die alleen voor de training nodig waren
1. Bepalen waar de webservice invoergegevens accepteert en waarin de uitvoer wordt gegenereerd

U kunt deze stappen handmatig uitvoeren of u kunt selecteren **webservice ingesteld** onder aan het experimentcanvas om deze automatisch uitgevoerd.

![De automatische conversie van een opleidingsexperiment naar een Voorspellend experiment met GIF-animatie](./media/ui-tutorial-automobile-price-deploy/deploy-web-service.gif)

Wanneer u selecteert **webservice ingesteld**, op verschillende manieren gebeuren:

* Het getrainde model wordt geconverteerd naar een enkele **getrainde Model** module. Deze wordt opgeslagen in het modulepalet links van het experimentcanvas. U vindt deze onder **getrainde modellen**.
* Modules die zijn gebruikt voor de training, worden verwijderd. Specifieker gezegd:
  * Train Model
  * Split Data
  * Model evalueren
* Het opgeslagen getrainde model wordt weer toegevoegd aan het experiment
* **Web service invoer** en **Web service uitvoer** modules worden toegevoegd. Deze modules identificeren waarin gegevens van de gebruiker het model wordt invoeren en waarin gegevens worden geretourneerd.

U kunt zien dat het experiment wordt opgeslagen in twee onderdelen op de nieuwe tabbladen boven aan het experimentcanvas. Het oorspronkelijke trainingsexperiment staat op het tabblad **Trainingsexperiment** en het zojuist gemaakte voorspellende experiment staat bij **Voorspellend experiment**. U gaat het voorspellende experiment implementeren als webservice.

Uw experiment moet er nu als volgt uitzien:  

![Schermopname van de verwachte configuratie van het experiment nadat deze is voorbereid voor implementatie](./media/ui-tutorial-automobile-price-deploy/predictive-graph.png)

Voer het experiment een keer (Selecteer **uitvoeren**). Kies de compute-doel dat u wilt dat het experiment dat moet worden uitgevoerd op in het dialoogvenster pop-upvenster. Om te controleren of het model is nog bezig is, selecteert u de uitvoer van de module Score Model en selecteer **resultaten weergeven**. U ziet dat de oorspronkelijke gegevens wordt weergegeven, samen met de voorspelde prijs ("Scored Labels').

## <a name="deploy-the-web-service"></a>De webservice implementeren

Een nieuwe webservice implementeren afgeleid van uw experiment:

1. Selecteer **webservice implementeren** onder het canvas.
1. Selecteer de **Compute-doel** dat u wilt uw webservice uit te voeren.

    De visuele interface ondersteunt momenteel alleen implementatie naar Azure Kubernetes Service (AKS) compute-doelen. U kunt kiezen uit de beschikbare AKS compute-doelen in uw machine learning-werkruimte-service of configureren van een nieuwe AKS-omgeving met behulp van de stappen in het dialoogvenster dat wordt weergegeven.

    ![Schermafbeelding van een mogelijke configuratie voor een nieuwe compute-doel](./media/ui-tutorial-automobile-price-deploy/deploy-compute.png)

1. Selecteer **-webservice implementeren**. U ziet het volgende bericht wanneer de implementatie is voltooid. Implementatie kan enkele minuten duren.

    ![Schermafbeelding van het bevestigingsbericht voor een geslaagde implementatie.](./media/ui-tutorial-automobile-price-deploy/deploy-succeed.png)

## <a name="test-the-web-service"></a>De webservice testen

Invoergegevens van de gebruiker krijgt uw geïmplementeerd model via de **Web service invoer** module. De invoer wordt vervolgens bepaald in de **Score Model** module. De manier waarop die u de Voorspellend experiment hebt ingesteld, het model wordt verwacht dat gegevens in dezelfde indeling als de oorspronkelijke gegevensset voor auto's prijs. Ten slotte de resultaten worden geretourneerd aan de gebruiker via de **Web service uitvoer** module.

U kunt een webservice testen op het tabblad web service in de visuele interface.

1. Ga naar de sectie van de web-service. Ziet u de webservice die u hebt geïmplementeerd met de naam van de **zelfstudie - auto-prijs voorspellen [voorspellende Exp]**.

     ![Schermopname van het tabblad web service met de onlangs gemaakte webservice gemarkeerd](./media/ui-tutorial-automobile-price-deploy/web-services.png)

1. Selecteer de naam van de webservice om extra details weer te geven.

     ![Schermopname van de aanvullende details beschikbaar in de webservice bekijken](./media/ui-tutorial-automobile-price-deploy/web-service-details.png)

1. Selecteer **Test**.

    ![Schermopname van de webservice-pagina testen](./media/ui-tutorial-automobile-price-deploy/web-service-test.png)

1. Voer gegevens testen of de voorbeeldgegevens autofilled gebruikt en selecteer **Test** aan de onderkant. De testaanvraag wordt verzonden naar de webservice en de resultaten worden weergegeven op de pagina.

## <a name="manage-the-web-service"></a>De webservice beheren

Als u uw web-service hebt geïmplementeerd, kunt u het beheren van de **webservices** tabblad in de visuele interface.

U kunt een webservice verwijderen door het selecteren van **verwijderen** in de detailpagina van het web-service.

   ![Schermopname van de locatie van de knop verwijderen web service aan de onderkant van het venster](./media/ui-tutorial-automobile-price-deploy/web-service-delete.png)

## <a name="consume-the-web-service"></a>De webservice gebruiken

In de vorige stappen van deze zelfstudie, kunt u een voorspellingsmodel auto's geïmplementeerd als een Azure-web-service. Gebruikers kunnen nu gegevens naar verzenden en ontvangen resultaten via REST-API.

**Aanvraag/antwoord** -de gebruiker een of meer rijen van auto's gegevens verzendt naar de service met behulp van een HTTP-protocol. De service reageert met een of meer sets met resultaten.

U vindt een voorbeeld-REST-aanroepen in de **verbruiken** tabblad van de webpagina van de service-informatie.

   ![Schermopname van een voorbeeld-REST-aanroepen dat gebruikers op het tabblad verbruiken vinden kunnen](./media/ui-tutorial-automobile-price-deploy/web-service-consume.png)

Navigeer naar de **API-document** tabblad, voor meer informatie van de API.

  ![Schermafbeelding van de aanvullende details van de API die gebruikers op het tabblad API-document vinden kunnen](./media/ui-tutorial-automobile-price-deploy/web-service-api.png)

## <a name="manage-models-and-deployments-in-azure-machine-learning-service-workspace"></a>Beheren van modellen en implementaties in de werkruimte van Azure Machine Learning-service

De modellen en webservice-implementaties u in de visuele interface maakt kunnen worden beheerd vanuit de werkruimte van de Azure Machine Learning-service.

1. Open de werkruimte in de [Azure-portal](https://portal.azure.com/).  

1. Selecteer in uw werkruimte **modellen**. Selecteer vervolgens het experiment dat u hebt gemaakt.

    ![Schermopname die laat zien hoe u om te navigeren naar experimenten in de Azure-portal](./media/ui-tutorial-automobile-price-deploy/portal-models.png)

    Op deze pagina ziet u aanvullende informatie over het model.

    ![Schermafbeelding die laat zien overzicht van de statistieken experiment in Azure portal](./media/ui-tutorial-automobile-price-deploy/model-details.png)

1. Selecteer **implementaties**, er wordt een lijst een webservices die gebruikmaken van het model. Selecteer de naam van de webservice, gaat u naar de detailpagina van de web-service. Op deze pagina krijgt u meer gedetailleerde informatie van de webservice.

    ![Schermafbeelding van gedetailleerde rapport](./media/ui-tutorial-automobile-price-deploy/deployment-details.png)

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [aml-ui-cleanup](../../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd stappen voor de sleutel in het maken, implementeren en gebruiken van een machine learning-model in de visuele interface. Bekijk de voorbeeldexperimenten voor meer informatie over hoe u de visuele interface gebruiken kunt om op te lossen van andere typen problemen.

> [!div class="nextstepaction"]
> [Tegoed risico classificatie voorbeeld](ui-sample-classification-predict-credit-risk-basic.md)
