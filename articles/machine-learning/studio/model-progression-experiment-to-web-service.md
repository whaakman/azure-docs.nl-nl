---
title: Hoe een Azure Machine Learning-model wordt een webservice | Microsoft Docs
description: Een overzicht van het mechanisme van hoe de voortgang van uw Azure Machine Learning-model van een ontwikkeling met een ingezette webservice experimenteren.
services: machine-learning
documentationcenter: ''
author: ericlicoding
ms.custom: (previous ms.author=yahajiza, author=YasinMSFT)
ms.author: amlstudiodocs
manager: hjerez
editor: cgronlun
ms.assetid: 25e0c025-f8b0-44ab-beaf-d0f2d485eb91
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.openlocfilehash: 4045b677b99eaa47e80f0a04ebf7f478eb6229d6
ms.sourcegitcommit: fa758779501c8a11d98f8cacb15a3cc76e9d38ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/20/2018
ms.locfileid: "52265182"
---
# <a name="how-a-machine-learning-model-progresses-from-an-experiment-to-an-operationalized-web-service"></a>Hoe een Machine Learning-model wordt uitgevoerd van een experiment naar een ingezette-webservice
Azure Machine Learning Studio biedt een interactief canvas waarmee u kunt ontwikkelen, te worden uitgevoerd, te testen en te herhalen een ***experimenteren*** vertegenwoordigt een predictive Analytics-model. Er zijn tal van modules die kunnen:

* Ingevoerde gegevens in uw experiment
* De gegevens bewerken
* Een model met machine learning-algoritmes te trainen
* Het model scoren
* De resultaten evalueren
* Laatste uitvoerwaarden

Als u tevreden met uw experiment bent, kunt u het implementeren als een ***klassieke Azure Machine Learning Web service*** of een ***nieuwe Azure Machine Learning Web service*** zodat gebruikers kunnen deze nieuwe gegevens verzenden en terug ontvangen resultaten.

In dit artikel geven we een overzicht van het mechanisme van hoe de voortgang van uw Machine Learning-model van een ontwikkeling met een ingezette webservice experimenteren.

> [!NOTE]
> Er zijn andere manieren om te ontwikkelen en implementeren van machine learning-modellen, maar in dit artikel is gericht op hoe u Machine Learning Studio gebruiken. Bijvoorbeeld, een beschrijving van hoe u een klassieke voorspellende webservice maken met R, Zie het blogbericht [Build & implementeren Predictive Web Apps met behulp van RStudio en Azure ML](http://blogs.technet.com/b/machinelearning/archive/2015/09/25/build-and-deploy-a-predictive-web-app-using-rstudio-and-azure-ml.aspx).
> 
> 

Azure Machine Learning Studio is ontworpen om te ontwikkelen en implementeren van een *predictive Analytics-model*, is het mogelijk Studio gebruiken voor het ontwikkelen van een experiment dat een predictive Analytics-model niet bevat. Een experiment kan bijvoorbeeld alleen de invoer, bewerken en de resultaten. Net als bij een voorspellende analyse-experiment, u kunt deze niet-Voorspellend experiment implementeren als een webservice, maar het is een eenvoudiger proces omdat het experiment niet training of scoren van een machine learning-model. Het is niet de typische Studio op deze manier gebruiken, moet we het opnemen in de discussie zodat we een volledige uitleg van de werking van Studio kunt geven.

## <a name="developing-and-deploying-a-predictive-web-service"></a>Ontwikkelen en implementeren van een voorspellende webservice
Hier volgen de stappen die een typische oplossing volgt bij het ontwikkelen en implementeren met behulp van Machine Learning Studio:

![Implementatie-stroom](./media/model-progression-experiment-to-web-service/model-stages-from-experiment-to-web-service.png)

*Afbeelding 1: fasen van een typische predictive Analytics-model*

### <a name="the-training-experiment"></a>Het trainingsexperiment
De ***trainingsexperiment*** is de eerste fase van het ontwikkelen van uw webservice in Machine Learning Studio. Het doel van het trainingsexperiment is waarin u een plaats om te ontwikkelen, testen, herhalen en uiteindelijk een machine learning-model te trainen. U kunt zelfs trainen van meerdere modellen tegelijkertijd als u de beste oplossing zoekt, maar wanneer u klaar bent u experimenteren één getraind selecteert model en de rest van het experiment te elimineren. Zie voor een voorbeeld van het ontwikkelen van een predictive Analytics-experiment [predictive analytics-oplossing voor kredietrisicobeoordeling in Azure Machine Learning ontwikkelen](walkthrough-develop-predictive-solution.md).

### <a name="the-predictive-experiment"></a>De Voorspellend experiment
Nadat u een getraind model in uw trainingsexperiment hebt, klikt u op **webservice ingesteld** en selecteer **voorspellende webservice** in Machine Learning Studio om het proces van het converteren van uw training initiëren om te experimenteren een ***Voorspellend experiment***. Het doel van de Voorspellend experiment is het gebruik van het getrainde model te scoren van nieuwe gegevens, met het doel van het uiteindelijk wordt geoperationaliseerd als een Azure-webservice.

Deze conversie vindt plaats voor u via de volgende stappen uit:

* De set met modules die worden gebruikt voor de training in een enkele module converteren en opslaan als een getraind model
* Alle overbodige modules die niet gerelateerd aan het scoring-verwijderen
* Toevoegen van de invoer- en -poorten die de uiteindelijke webservice worden gebruikt

Mogelijk zijn er meer wijzigingen die u maken wilt om uw Voorspellend experiment gereed om te implementeren als een webservice. Als u wilt dat de webservice voor de uitvoer van slechts een subset van resultaten, kunt u bijvoorbeeld een filter module voordat de uitvoerpoort toevoegen.

In dit conversieproces het trainingsexperiment niet worden verwijderd. Wanneer het proces voltooid is, hebt u twee tabbladen in Studio: één voor het trainingsexperiment en één voor de Voorspellend experiment. Deze manier die u wijzigingen aan het trainingsexperiment aanbrengen kunt voordat u uw webservice implementeert en de Voorspellend experiment opnieuw. Of u een kopie van het trainingsexperiment om te starten van een andere line-of-experimenten kunt opslaan.

> [!NOTE]
> Wanneer u klikt op **voorspellende webservice** u automatische starten als u wilt uw trainingsexperiment converteren naar een Voorspellend experiment en dit werkt ook in de meeste gevallen. Als uw trainingsexperiment complex is (bijvoorbeeld, u hebt meerdere paden voor training die u samenvoegen), misschien wilt u deze conversie een hoeveelheid handmatig doen. Zie voor meer informatie, [over het voorbereiden van uw model voor implementatie in Azure Machine Learning Studio](convert-training-experiment-to-scoring-experiment.md).
> 
> 

### <a name="the-web-service"></a>De webservice
Als u tevreden bent dat uw Voorspellend experiment klaar is, u uw service als een klassieke webservice implementeren kunt of een nieuwe webservice gebaseerd op Azure Resource Manager. Aan uw model operationeel maken door te implementeren als een *klassieke Machine Learning-webservice*, klikt u op **webservice implementeren** en selecteer **webservice implementeren [klassieke]**. Om te implementeren als *nieuwe Machine Learning-webservice*, klikt u op **webservice implementeren** en selecteer **Web Service implementeren [Nieuw]**. Gebruikers kunnen nu gegevens aan uw model met behulp van de REST-API-webservice verzenden en ontvangen van de resultaten. Zie [How to consume an Azure Machine Learning Web service](consume-web-services.md) (Azure Machine Learning-webservice gebruiken) voor meer informatie.

## <a name="the-non-typical-case-creating-a-non-predictive-web-service"></a>Niet-standaard: het maken van een niet-voorspellende webservice
Als uw experiment heeft niet de trein een predictive Analytics-model, dan hebt u niet nodig hebt om zowel een opleidingsexperiment en een score experiment te maken: Er is slechts één experiment en kunt u deze als een webservice implementeren. Machine Learning Studio wordt gedetecteerd of uw experiment een Voorspellend model bevat door het analyseren van de modules die u hebt gebruikt.

Nadat u hebt uw experiment herhaald en tevreden met het bent:

1. Klik op **webservice ingesteld** en selecteer **webservice opnieuw trainen** - invoer en uitvoer knooppunten automatisch worden toegevoegd
2. Klik op **uitvoeren**
3. Klik op **webservice implementeren** en selecteer **webservice implementeren [klassieke]** of **Web Service implementeren [Nieuw]** afhankelijk van de omgeving waarnaar u wilt implementeren.

Uw webservice is nu geïmplementeerd en u kunt toegang tot en beheer deze net als bij een voorspellende webservice.

## <a name="updating-your-web-service"></a>Bijwerken van uw Web-service
Nu dat u hebt uw experiment geïmplementeerd als een webservice, wat gebeurt er als u wilt bijwerken?

Dat is afhankelijk van wat u nodig hebt om bij te werken:

**U wilt wijzigen van de invoer of uitvoer, of u wilt wijzigen hoe gegevens worden bewerkt door de webservice**

Als u niet van het model wijzigen bent, maar zijn alleen wijzigen hoe gegevens worden verwerkt door de webservice, kunt u de Voorspellend experiment bewerken en klik vervolgens op **webservice implementeren** en selecteer **webservice implementeren [klassieke]** of **Web Service implementeren [Nieuw]** opnieuw. De Web-service is gestopt, de bijgewerkte Voorspellend experiment is geïmplementeerd en de Web-service opnieuw wordt opgestart.

Hier volgt een voorbeeld: Stel uw voorspellende experiment de hele rij van ingevoerde gegevens met het verwachte resultaat retourneert. U besluiten dat u wilt dat de webservice alleen het resultaat. Zodat u kunt toevoegen een **Projectkolommen** de Voorspellend experiment, vlak voor de uitvoerpoort wilt uitsluiten van kolommen behalve het resultaat van de module. Wanneer u klikt op **webservice implementeren** en selecteer **webservice implementeren [klassieke]** of **Web Service implementeren [Nieuw]** nogmaals, de webservice wordt bijgewerkt.

**U wilt opnieuw trainen het model met nieuwe gegevens**

Als u wilt behouden van uw machine learning-model, maar u wilt het opnieuw trainen met nieuwe gegevens, hebt u twee opties:

1. **Het model opnieuw trainen terwijl de webservice wordt uitgevoerd** -als u wilt dat uw model trainen terwijl de voorspellende webservice wordt uitgevoerd, kunt u dit doen door enkele wijzigingen aan te brengen de trainingsexperiment u gemakkelijk een ***opnieuw trainen experimenteren***, en vervolgens kunt u het implementeert als een  ***retraining web* service**. Zie voor instructies over hoe u dit doet, [opnieuw trainen Machine Learning-modellen programmatisch](retrain-models-programmatically.md).
2. **Ga terug naar de oorspronkelijke trainingsexperiment en verschillende trainingsgegevens gebruiken voor het ontwikkelen van uw model** : uw Voorspellend experiment is gekoppeld aan de webservice, maar het trainingsexperiment niet rechtstreeks is gekoppeld op deze manier. Als u de oorspronkelijke trainingsexperiment wijzigen en klik op **webservice instellen**, wordt er een *nieuwe* voorspellende experimenteren die wanneer geïmplementeerd, maakt een *nieuwe* Web de service. Het niet alleen de oorspronkelijke webservice bijgewerkt.
   
   Als u wijzigen van het trainingsexperiment wilt, open het en klik op **OpslaanAls** om een kopie te maken. Dit wordt het oorspronkelijke trainingsexperiment, Voorspellend experiment behouden en webservice. U kunt nu een nieuwe webservice maken met uw wijzigingen. Nadat u de nieuwe webservice die u kunt vervolgens beslissen of u zorgen dat naast de nieuwe resourcegroep of stoppen van de vorige webservice hebt geïmplementeerd.

**U wilt een ander model trainen**

Als u wijzigingen aanbrengen in uw oorspronkelijke Voorspellend experiment, zoals het selecteren van een andere machine learning-algoritme wilt, probeert een andere training methode, enzovoort, moet u de tweede procedure zoals hierboven beschreven voor het opnieuw trainen van uw model te volgen: open de training experiment, klikt u op **OpslaanAls** maakt u een kopie en start u het nieuwe pad van het ontwikkelen van uw model, de Voorspellend experiment maken en implementeren van de webservice. Hiermee maakt u een nieuwe Web service dat geen verband houdt met de oorspronkelijke - kunt u bepalen welke een of beide worden uitgevoerd.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het proces van het ontwikkelen en te experimenteren, de volgende artikelen:

* converteren van het experiment - [over het voorbereiden van uw model voor implementatie in Azure Machine Learning Studio](convert-training-experiment-to-scoring-experiment.md)
* implementatie van de webservice - [een Azure Machine Learning-webservice implementeren](publish-a-machine-learning-web-service.md)
* opnieuw trainen het model - [-modellen programmatisch opnieuw trainen van Machine Learning](retrain-models-programmatically.md)

Zie voor voorbeelden van het hele proces:

* [Machine learning-zelfstudie: uw eerste experiment maken in Azure Machine Learning Studio](create-experiment.md)
* [Rondleiding: Een predictive analytics-oplossing voor kredietrisicobeoordeling in Azure Machine Learning ontwikkelen](walkthrough-develop-predictive-solution.md)

