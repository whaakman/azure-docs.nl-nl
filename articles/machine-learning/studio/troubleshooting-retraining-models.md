---
title: Problemen oplossen opnieuw trainen van een Machine Learning Studio klassieke webservice - Azure | Microsoft Docs
description: Identificeren en te corrigeren van veelvoorkomende problemen tegengekomen wanneer u opnieuw het model voor een Azure Machine Learning Studio-webservice trainen bent.
services: machine-learning
documentationcenter: ''
author: ericlicoding
ms.custom: previous-ms.author=yahajiza, previous-author=YasinMSFT
ms.author: amlstudiodocs
editor: cgronlun
ms.assetid: 75cac53c-185c-437d-863a-5d66d871921e
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/01/2017
ms.openlocfilehash: 97116c4ad6efbaad28aec6451b02fc0dee1ac79f
ms.sourcegitcommit: d61faf71620a6a55dda014a665155f2a5dcd3fa2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54054857"
---
# <a name="troubleshooting-the-retraining-of-an-azure-machine-learning-studio-classic-web-service"></a>Het oplossen van het opnieuw trainen van een klassieke webservice Azure Machine Learning Studio
## <a name="retraining-overview"></a>Overzicht opnieuw trainen
Wanneer u een Voorspellend experiment als een scoringwebservice implementeren is een statisch model. Zodra er nieuwe gegevens beschikbaar is of wanneer de gebruiker van de API hun eigen gegevens is, moet het model opnieuw worden getraind. 

Zie voor een volledig overzicht van hoe u een klassieke webservice retraining [opnieuw trainen Machine Learning-modellen programmatisch](retrain-models-programmatically.md).

## <a name="retraining-process"></a>Opnieuw trainen van proces
Als u de webservice opnieuw trainen wilt, moet u enkele extra onderdelen toevoegen:

* Een van de Trainingsexperiment geïmplementeerde webservice. Het experiment moet hebben een **Web Service uitvoer** module die is gekoppeld aan de uitvoer van de **Train Model** module.  
  
    ![De uitvoer van web service koppelen aan het train-model.][image1]
* Een nieuw eindpunt toevoegen aan uw scoring webservice.  U kunt het eindpunt via een programma met de voorbeeldcode waarnaar wordt verwezen in de opnieuw trainen van Machine Learning-modellen programmatisch toevoegen onderwerp of via de portal voor Azure Machine Learning-webservices.

Vervolgens kunt u het voorbeeld C# code van de Training-webservice-API help-pagina model te trainen. Nadat u de resultaten hebt geëvalueerd en tevreden mee bent, kunt u het getrainde model scoren webservice met behulp van het nieuwe eindpunt dat u hebt toegevoegd bijwerken.

Alle benodigde onderdelen aanwezig is zijn de belangrijkste stappen die u ondernemen moet om het opnieuw trainen het model als volgt:

1. Roep de webservice Training:  De aanroep is naar de Batch Execution Service (BES), niet de Request Response Service (RRS). U kunt het voorbeeld C# code op de API help-pagina voor de aanroep. 
2. De waarden voor de *BaseLocation*, *RelativeLocation*, en *SasBlobToken*: Deze waarden worden geretourneerd in de uitvoer van de aanroep van de webservice-Training. 
   ![de uitvoer van de retraining-voorbeeld en de BaseLocation RelativeLocation en SasBlobToken waarden worden weergegeven.][image6]
3. De toegevoegde eindpunt uit de scoring-webservice met het nieuwe getrainde model bijwerken: Het nieuwe eindpunt voor dat u toegevoegd aan het score-model met het zojuist getrainde model van de webservice-Training met behulp van de voorbeeldcode die is opgegeven in de modellen voor Machine Learning opnieuw trainen via een programma, worden bijgewerkt.

## <a name="common-obstacles"></a>Algemene obstakels
### <a name="check-to-see-if-you-have-the-correct-patch-url"></a>Controleer of u hebt de juiste PATCH-URL
De PATCH URL u moet het account dat is gekoppeld aan het nieuwe scoring eindpunt dat u hebt toegevoegd aan de scoring-webservice. Er zijn een aantal manieren om op te halen van de PATCH-URL:

**Optie 1: Via programmacode**

Aan de juiste PATCH-URL:

1. Voer de [AddEndpoint](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs) voorbeeldcode.
2. Uit de uitvoer van AddEndpoint, vinden de *HelpLocation* waarde en kopieer de URL.
   
   ![HelpLocation in de uitvoer van het voorbeeld addEndpoint.][image2]
3. Plak de URL in een browser om te navigeren naar een pagina met help-koppelingen voor de webservice.
4. Klik op de **resources bijwerken** koppeling om de patch help-pagina te openen.

**Optie 2: Gebruik de portal voor Azure Machine Learning-webservices**

1. Aanmelden bij de [Azure Machine Learning-webservices](https://services.azureml.net/) portal.
2. Klik op **webservices** of **klassieke webservices** aan de bovenkant.
4. Klik op de scoringwebservice u met werkt (als u de standaardnaam van de webservice niet wijzigt, deze wordt beëindigd in "[score Exp.]").
5. Klik op **+ nieuw**.
6. Nadat het eindpunt is toegevoegd, klik op de naam van het eindpunt.
7. Onder de **Patch** URL, klikt u op **API Help** de patch Helppagina te openen.

> [!NOTE]
> Als u het eindpunt aan de webservice voor de Training in plaats van de voorspellende webservice hebt toegevoegd, ontvangt u de volgende fout wanneer u klikt op de **resources bijwerken** koppeling: "Onze excuses, maar deze functie wordt niet ondersteund of beschikbaar in deze context. Deze webservice heeft geen resources bij te werken. We excuses voor het ongemak en worden gewerkt aan het verbeteren van deze werkstroom."
> 
> 

De PATCH help-pagina bevat de vullen van de URL die u moet gebruiken en voorbeeldcode die u gebruiken kunt om aan te roepen van het biedt.

![URL van de patch.][image5]

### <a name="check-to-see-that-you-are-updating-the-correct-scoring-endpoint"></a>Controleer dat u het juiste eindpunt scoring bijwerkt
* Patch de training-webservice niet uitvoeren: De patchbewerking moet worden uitgevoerd op de scoring-webservice.
* Patch het standaardeindpunt voor de webservice niet uitvoeren: De patchbewerking moet worden uitgevoerd op het nieuwe scoring web service-eindpunt dat u hebt toegevoegd.

U kunt controleren of het eindpunt is op door naar de Web Services-portal te gaan welke-webservice. 

> [!NOTE]
> Zorg ervoor dat u het eindpunt wilt toevoegen aan de voorspellende webservice, niet de Training-webservice. Als u goed zowel een Training en een voorspellende webservice hebt geïmplementeerd, ziet u twee afzonderlijke services die worden vermeld. De voorspellende webservice moet eindigen met '[voorspellende exp.]'.
> 
> 

1. Aanmelden bij de [Azure Machine Learning-webservices](https://services.azureml.net/) portal.
2. Klik op **webservices** of **klassieke webservices**.
3. Selecteer uw voorspellende webservice.
4. Controleren of het nieuwe eindpunt is toegevoegd aan de webservice.

### <a name="check-that-your-workspace-is-in-the-same-region-as-the-web-service"></a>Controleer of uw werkruimte zich in dezelfde regio als de webservice
1. Aanmelden bij [Machine Learning Studio](https://studio.azureml.net/).
2. Klik op de vervolgkeuzelijst met uw werkruimten aan de bovenkant.

   ![Machine learning regio gebruikersinterface.][image4]

3. Controleer of de regio waarin uw werkruimte zich bevindt.

<!-- Image Links -->

[image1]: ./media/troubleshooting-retraining-a-model/ml-studio-tm-connected-to-web-service-out.png
[image2]: ./media/troubleshooting-retraining-a-model/addEndpoint-output.png
[image3]: ./media/troubleshooting-retraining-a-model/azure-portal-update-resource.png
[image4]: ./media/troubleshooting-retraining-a-model/check-workspace-region.png
[image5]: ./media/troubleshooting-retraining-a-model/ml-help-page-patch-url.png
[image6]: ./media/troubleshooting-retraining-a-model/retraining-output.png
[image7]: ./media/troubleshooting-retraining-a-model/web-services-tab.png
