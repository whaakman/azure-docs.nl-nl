---
title: Problemen met een klassieke Azure Machine Learning-webservice retraining | Microsoft Docs
description: Identificeren en te corrigeren van algemene problemen tegengekomen wanneer u het model zijn retraining voor een Azure Machine Learning-webservice.
services: machine-learning
documentationcenter: 
author: VDonGlover
manager: raymondl
editor: 
ms.assetid: 75cac53c-185c-437d-863a-5d66d871921e
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: v-donglo
ms.openlocfilehash: 85cf9175bb4a5f253c7b47b2edc3ac8b00616ba2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="troubleshooting-the-retraining-of-an-azure-machine-learning-classic-web-service"></a>Het oplossen van de retraining van een Azure Machine Learning klassieke webservice
## <a name="retraining-overview"></a>Overzicht retraining
Dit is een statische model wanneer u een Voorspellend experiment als scoreprofiel webservice implementeert. Zodra er nieuwe gegevens beschikbaar of wanneer de gebruiker van de API zijn eigen gegevens heeft, moet het model worden retrained. 

Zie voor een volledig overzicht van de retraining proces van een klassieke webservice [Retrain Machine Learning-modellen programmatisch](retrain-models-programmatically.md).

## <a name="retraining-process"></a>Proces retraining
Als u opnieuw trainen van de webservice wilt, moet u enkele aanvullende onderdelen toevoegen:

* Een webservice op basis van het Experiment Training geïmplementeerd. Het experiment moet hebben een **Web Service uitvoer** module die is gekoppeld aan de uitvoer van de **Train Model** module.  
  
    ![De uitvoer van de web service koppelen aan het train-model.][image1]
* Een nieuw toegevoegd aan uw scoreprofiel webservice-eindpunt.  U kunt het eindpunt programmatisch met behulp van de voorbeeldcode waarnaar wordt verwezen in de Retrain Machine Learning-modellen programmatisch toevoegen onderwerp of via de klassieke Azure portal.

Vervolgens kunt u in de voorbeeldcode van de Training Web Service API help-pagina C# opnieuw trainen model. Nadat u de resultaten hebt geëvalueerd en u tevreden mee bent, kunt u het getrainde model score berekenen voor webservice met behulp van het nieuwe eindpunt dat u hebt toegevoegd bijwerken.

Met alle benodigde onderdelen aanwezig zijn de belangrijkste stappen die u nemen moet om het model opnieuw trainen als volgt:

1. Roep de webservice voor Training: de aanroep naar de Batch uitvoering Service (BES), niet de Request Response Service (RR's) is. Kunt u het voorbeeld C#-code op de API help-pagina voor het gesprek. 
2. De waarden voor de *BaseLocation*, *RelativeLocation*, en *SasBlobToken*: deze waarden worden geretourneerd in de uitvoer van de aanroep van de webservice Training. 
   ![de uitvoer van de retraining voorbeeld en de BaseLocation RelativeLocation en SasBlobToken waarden weergegeven.][image6]
3. Het toegevoegde eindpunt van de score webservice bijwerken met het nieuwe getrainde model: het nieuwe eindpunt u toegevoegd aan het score model met het nieuwe getrainde model van de webservice Training met behulp van de voorbeeldcode in de Retrain Machine Learning-modellen programmatisch worden bijgewerkt.

## <a name="common-obstacles"></a>Algemene obstakels
### <a name="check-to-see-if-you-have-the-correct-patch-url"></a>Controleer of u de juiste PATCH-URL hebt
De PATCH URL u moet worden gekoppeld aan het nieuwe scoreprofiel eindpunt dat u hebt toegevoegd aan de score-webservice. Er zijn een aantal manieren verkrijgen van de PATCH-URL:

**Optie 1: via programmacode**

De juiste PATCH-URL ophalen:

1. Voer de [AddEndpoint](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs) voorbeeldcode.
2. Zoek in de uitvoer van AddEndpoint, de *HelpLocation* waarde en kopieer de URL.
   
   ![HelpLocation in de uitvoer van de voorbeeld-addEndpoint.][image2]
3. Plak de URL in een browser om te navigeren naar een pagina waarmee help-koppelingen voor de webservice.
4. Klik op de **Update Resource** koppeling te openen van de patch help-pagina.

**Optie 2: De klassieke Azure portal gebruiken**

1. Meld u aan bij de [klassieke Azure-portal](https://manage.windowsazure.com).
2. Open het tabblad Machine Learning. ![Machine bijvoorbeeld tabblad.][image4]
3. Klik op de naam van uw werkruimte **webservices**.
4. Klik op de score webservice die u met werkt. (Als u niet de standaardnaam van de webservice wijzigt hebt, wordt het beëindigd in [score berekenen Exp.].)
5. Klik op **eindpunt toevoegen**.
6. Nadat het eindpunt is toegevoegd, klik op de naam van het eindpunt. Klik vervolgens op **Update Resource** openen van de toepassing van patches help-pagina.

> [!NOTE]
> Als u het eindpunt hebt toegevoegd aan de Training Web Service in plaats van de voorspellende webservice, ontvangt u de volgende fout wanneer u klikt op de **Update Resource** koppeling: Sorry, maar deze functie wordt niet ondersteund of beschikbaar in deze context. Deze webservice heeft geen bronnen worden bijgewerkt. Wij bieden onze excuses aan voor het ongemak en werkt op het verbeteren van deze werkstroom.
> 
> 

![Nieuwe endpoint-dashboard.][image3]

De PATCH help-pagina bevat de PATCH-URL moet u en wordt een voorbeeldcode die u kunt deze aanroepen.

![URL van de patch.][image5]

### <a name="check-to-see-that-you-are-updating-the-correct-scoring-endpoint"></a>Controleer dat u het juiste scoreprofiel eindpunt wilt bijwerken
* De webservice Training niet doen patch: de patch-bewerking moet worden uitgevoerd op de score-webservice.
* Het standaardeindpunt van webservice niet doen patch: de patch-bewerking moet worden uitgevoerd op het nieuwe scoreprofiel Web service-eindpunt dat u hebt toegevoegd.

U kunt controleren welke webservice door het eindpunt is op via de klassieke Azure portal. 

> [!NOTE]
> Zorg ervoor dat u het eindpunt wilt toevoegen aan de voorspellende Web Service, niet de trainings-webservice. Als u correct zowel een trainings- en een Voorspellend webservice hebt geïmplementeerd, ziet u twee verschillende Web-services die worden vermeld. De voorspellende webservice moet eindigen met '[voorspellende exp.]'.
> 
> 

1. Meld u aan bij de [klassieke Azure-portal](https://manage.windowsazure.com).
2. Open het tabblad Machine Learning. ![Machine learning-werkruimte gebruikersinterface.][image4]
3. Selecteer uw werkruimte.
4. Klik op **webservices**.
5. Selecteer uw Predictive webservice.
6. Controleren of uw nieuwe eindpunt is toegevoegd aan de webservice.

### <a name="check-the-workspace-that-your-web-service-is-in-to-ensure-it-is-in-the-correct-region"></a>De werkruimte die uw web-service in om te controleren of dat deze zich in de juiste regio controleren
1. Meld u aan bij de [klassieke Azure-portal](https://manage.windowsazure.com).
2. Selecteer de Machine Learning in het menu.
   ![Machine learning regio gebruikersinterface.][image4]
3. Controleer of de locatie van uw werkruimte.

<!-- Image Links -->

[image1]: ./media/troubleshooting-retraining-a-model/ml-studio-tm-connnected-to-web-service-out.png
[image2]: ./media/troubleshooting-retraining-a-model/addEndpoint-output.png
[image3]: ./media/troubleshooting-retraining-a-model/azure-portal-update-resource.png
[image4]: ./media/troubleshooting-retraining-a-model/azure-portal-machine-learning-tab.png
[image5]: ./media/troubleshooting-retraining-a-model/ml-help-page-patch-url.png
[image6]: ./media/troubleshooting-retraining-a-model/retraining-output.png
[image7]: ./media/troubleshooting-retraining-a-model/web-services-tab.png
