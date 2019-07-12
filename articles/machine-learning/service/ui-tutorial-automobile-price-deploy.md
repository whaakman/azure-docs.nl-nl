---
title: 'Zelfstudie: Een machine learning-model met de visuele interface implementeren'
titleSuffix: Azure Machine Learning service
description: Meer informatie over het bouwen van een predictive analytics-oplossing in de visuele interface voor Azure Machine Learning-service. Trainen, beoordelen, en implementeren van een machine learning-model met behulp van slepen en neerzetten van modules. In deze zelfstudie is deel twee van een tweedelige reeks over lineaire regressie gebruiken prijzen van auto's voorspellen.
author: peterclu
ms.author: peterlu
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
ms.date: 07/11/2019
ms.openlocfilehash: dd28fb51a4fc3fbf3dfc893f2f5f159ccafdb4b3
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67839304"
---
# <a name="tutorial-deploy-a-machine-learning-model-with-the-visual-interface"></a>Zelfstudie: Een machine learning-model met de visuele interface implementeren

Zodat anderen de kans op het gebruik van het voorspellende model ontwikkeld in [deel één van de zelfstudie](ui-tutorial-automobile-price-train-score.md), kunt u deze implementeren als een Azure-web-service. U hebt tot nu toe is experimenteren met het trainen van uw model. Nu is het tijd voor het genereren van nieuwe voorspellingen op basis van de invoer van de gebruiker. In dit gedeelte van de zelfstudie hebt u:

> [!div class="checklist"]
> * Een model voor implementatie voorbereiden
> * Een webservice implementeren
> * Een webservice testen
> * Een webservice beheren
> * De webservice gebruiken

## <a name="prerequisites"></a>Vereisten

Volledige [deel één van de zelfstudie](ui-tutorial-automobile-price-train-score.md) voor informatie over het trainen en te beoordelen van een machine learning-model in de visuele interface.

## <a name="prepare-for-deployment"></a>Voorbereiden voor implementatie

Voordat u uw experiment als een webservice implementeert, hebt u eerst converteren uw *trainingsexperiment* in een *Voorspellend experiment*.

1. Selecteer **Voorspellend Experiment maken*** aan de onderkant van het experimentcanvas.

    ![De automatische conversie van een opleidingsexperiment naar een Voorspellend experiment met GIF-animatie](./media/ui-tutorial-automobile-price-deploy/deploy-web-service.gif)

    Wanneer u selecteert **Voorspellend Experiment maken**, op verschillende manieren gebeuren:
    
    * Het getrainde model wordt opgeslagen als een **getrainde Model** module in het modulepalet. U vindt deze onder **getrainde modellen**.
    * Modules die zijn gebruikt voor trainingen, worden verwijderd; met name:
      * Model trainen
      * Gegevens splitsen
      * Model evalueren
    * Het opgeslagen getrainde model wordt toegevoegd in het experiment.
    * **Web service invoer** en **Web service uitvoer** modules worden toegevoegd. Deze modules identificeren waar de gegevens van het model wordt ingevoerd, en waarin gegevens worden geretourneerd.

    De **trainingsexperiment** nog steeds wordt opgeslagen in de nieuwe tabbladen boven aan het experimentcanvas.

1. **Voer het experiment uit**.

1. Selecteer de uitvoer van de **Score Model** -module en selecteer **resultaten weergeven** om te controleren of het model is nog steeds werken. U ziet dat de oorspronkelijke gegevens wordt weergegeven, samen met de voorspelde prijs ("Scored Labels').

Uw experiment moet er nu als volgt uitzien:  

![Schermopname van de verwachte configuratie van het experiment nadat deze is voorbereid voor implementatie](./media/ui-tutorial-automobile-price-deploy/predictive-graph.png)

## <a name="deploy-the-web-service"></a>De webservice implementeren

1. Selecteer **webservice implementeren** onder het canvas.

1. Selecteer de **Compute-doel** dat u wilt uw webservice uit te voeren.

    De visuele interface ondersteunt momenteel alleen implementatie naar Azure Kubernetes Service (AKS) compute-doelen. U kunt kiezen uit de beschikbare AKS compute-doelen in uw machine learning-werkruimte-service of configureren van een nieuwe AKS-omgeving met behulp van de stappen in het dialoogvenster dat wordt weergegeven.

    ![Schermafbeelding van een mogelijke configuratie voor een nieuwe compute-doel](./media/ui-tutorial-automobile-price-deploy/deploy-compute.png)

1. Selecteer **-webservice implementeren**. U ziet het volgende bericht wanneer de implementatie is voltooid. Implementatie kan enkele minuten duren.

    ![Schermafbeelding van het bevestigingsbericht voor een geslaagde implementatie.](./media/ui-tutorial-automobile-price-deploy/deploy-succeed.png)

## <a name="test-the-web-service"></a>De webservice testen

U kunt testen en beheren van uw webservices visuele interface door te navigeren naar de **webservices** tabblad.

1. Ga naar de sectie van de web-service. Ziet u de webservice die u hebt geïmplementeerd met de naam van de **zelfstudie - auto-prijs voorspellen [voorspellende Exp]** .

     ![Schermopname van het tabblad web service met de onlangs gemaakte webservice gemarkeerd](./media/ui-tutorial-automobile-price-deploy/web-services.png)

1. Selecteer de naam van de webservice om extra details weer te geven.

     ![Schermopname van de aanvullende details beschikbaar in de webservice bekijken](./media/ui-tutorial-automobile-price-deploy/web-service-details.png)

1. Selecteer **Test**.

    ![Schermopname van de webservice-pagina testen](./media/ui-tutorial-automobile-price-deploy/web-service-test.png)

1. Voer gegevens testen of de voorbeeldgegevens autofilled gebruikt en selecteer **Test**.

    De testaanvraag wordt verzonden naar de webservice en de resultaten worden weergegeven op de pagina. Hoewel de waarde voor een prijs is gegenereerd voor de invoergegevens, wordt deze niet gebruikt voor het genereren van de waarde voor de voorspelling.

## <a name="consume-the-web-service"></a>De webservice gebruiken

Gebruikers kunnen nu API-aanvragen verzenden naar uw Azure-web-service en resultaten om te voorspellen de prijs van de nieuwe auto's ontvangen.

**Aanvraag/antwoord** -de gebruiker een of meer rijen van auto's gegevens verzendt naar de service met behulp van een HTTP-protocol. De service reageert met een of meer sets met resultaten.

U vindt een voorbeeld-REST-aanroepen in de **verbruiken** tabblad van de webpagina van de service-informatie.

   ![Schermopname van een voorbeeld-REST-aanroepen dat gebruikers op het tabblad verbruiken vinden kunnen](./media/ui-tutorial-automobile-price-deploy/web-service-consume.png)

Navigeer naar de **API-document** tabblad, voor meer informatie van de API.

  ![Schermafbeelding van de aanvullende details van de API die gebruikers op het tabblad API-document vinden kunnen](./media/ui-tutorial-automobile-price-deploy/web-service-api.png)

## <a name="manage-models-and-deployments"></a>Beheren van modellen en implementaties

De modellen en webservice-implementaties u in de visuele interface maakt kunnen ook worden beheerd vanuit de werkruimte van de Azure Machine Learning-service.

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

In deze zelfstudie hebt u geleerd stappen voor de sleutel in het maken, implementeren en gebruiken van een machine learning-model in de visuele interface. Zie voor meer informatie over hoe u de visuele interface gebruiken kunt om op te lossen van andere typen problemen van onze andere voorbeeldexperimenten.

> [!div class="nextstepaction"]
> [Tegoed risico classificatie voorbeeld](ui-sample-classification-predict-credit-risk-cost-sensitive.md)
