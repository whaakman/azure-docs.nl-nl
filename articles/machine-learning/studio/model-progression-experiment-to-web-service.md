---
title: Hoe een Azure Machine Learning-model wordt een webservice | Microsoft Docs
description: Een overzicht van het mechanisme van hoe de voortgang van uw Azure Machine Learning-model van een ontwikkel met een operationalized webservice experimenteren.
services: machine-learning
documentationcenter: ''
author: YasinMSFT
ms.author: yahajiza
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
ms.openlocfilehash: 7b9aec2815f836b3b220de37fe6428c54d39c3e5
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/07/2018
ms.locfileid: "34835569"
---
# <a name="how-a-machine-learning-model-progresses-from-an-experiment-to-an-operationalized-web-service"></a>Hoe een Machine Learning-model van een experiment toegewezen aan een operationalized webservice
Azure Machine Learning Studio biedt een interactieve canvas waarmee u kunt ontwikkelen, worden uitgevoerd, testen en herhalen een ***experimenteren*** die vertegenwoordigt een predictive Analytics-model. Er zijn tal van modules die kunnen:

* Invoergegevens die in uw experiment
* De gegevens bewerken
* Een model met machine learning-algoritmen trainen
* Het model scoren
* De resultaten evalueren
* Laatste uitvoerwaarden

Als u tevreden met uw experiment bent, kunt u het implementeert als een ***klassieke Azure Machine Learning-webservice*** of een ***nieuwe Azure Machine Learning-webservice*** zodat gebruikers kunnen deze nieuwe gegevens verzenden en terug ontvangen resultaten.

We bieden een overzicht van het mechanisme van hoe de voortgang van uw Machine Learning-model van een ontwikkel met een operationalized webservice experimenteren in dit artikel.

> [!NOTE]
> Er zijn andere manieren om te ontwikkelen en implementeren van machine learning-modellen, maar in dit artikel is gericht op hoe u Machine Learning Studio gebruiken. Bijvoorbeeld, om te lezen een beschrijving van het maken van een klassieke voorspellende webservice met R, Zie het blogbericht [Build & implementeren voorspellende Web-Apps met behulp van RStudio en Azure ML](http://blogs.technet.com/b/machinelearning/archive/2015/09/25/build-and-deploy-a-predictive-web-app-using-rstudio-and-azure-ml.aspx).
> 
> 

Hoewel Azure Machine Learning Studio is ontworpen voor hulp bij het ontwikkelen en implementeren van een *predictive Analytics-model*, kunt u met Studio een experiment waarin een predictive Analytics-model niet. Een experiment kan bijvoorbeeld alleen invoergegevens, bewerken en de resultaten. Net als een predictive Analytics-experiment, kunt u deze niet-Voorspellend experiment als een webservice implementeren, maar het is een eenvoudigere proces omdat het experiment niet training of score berekenen voor een machine learning-model. Hoewel dit niet de typische Studio op deze manier gebruiken, moet we het opnemen in de bespreking van de zodat we een volledige uitleg van de werking van Studio kunnen geven.

## <a name="developing-and-deploying-a-predictive-web-service"></a>Ontwikkeling en implementatie van een Voorspellend webservice
Hier worden de fasen die een gangbare oplossing volgt bij het ontwikkelen en implementeren met behulp van Machine Learning Studio:

![Implementatie-stroom](./media/model-progression-experiment-to-web-service/model-stages-from-experiment-to-web-service.png)

*Afbeelding 1: fasen van een typische predictive Analytics-model*

### <a name="the-training-experiment"></a>Het trainingsexperiment
De ***trainingsexperiment*** is de eerste fase van het ontwikkelen van uw Web-service in Machine Learning Studio. Het doel van het trainingsexperiment is om u te bieden een plaats om te ontwikkelen, testen, herhalen en uiteindelijk een machine learning-model te trainen. U kunt zelfs trainen meerdere modellen tegelijkertijd als u de beste oplossing zoeken, maar zodra u klaar bent u experimenteren één getraind kiest model en de rest van het experiment te elimineren. Zie voor een voorbeeld van het ontwikkelen van een predictive Analytics-experiment [predictive analytics-oplossing voor kredietrisicobeoordeling in Azure Machine Learning ontwikkelen](walkthrough-develop-predictive-solution.md).

### <a name="the-predictive-experiment"></a>De Voorspellend experiment
Wanneer u een getraind model in uw trainingsexperiment hebt, klikt u op **webservice ingesteld** en selecteer **voorspellende webservice** in Machine Learning Studio starten van het proces van het converteren van uw training om te experimenteren een ***Voorspellend experiment***. Het doel van de Voorspellend experiment wordt met het getrainde model beoordeling van nieuwe gegevens, met het doel van uiteindelijk steeds geoperationaliseerd als een Azure-webservice.

Deze conversie is gedaan om u de volgende stappen:

* De modules die worden gebruikt voor training in de module voor een enkele set converteren en deze opslaan als een getraind model
* Verwijderen van alle overbodige modules die niet gerelateerd aan score berekenen
* Toevoegen van de invoer- en poorten die door de uiteindelijke Web-service wordt gebruikt

Mogelijk zijn er meer wijzigingen die u maken wilt om uw Voorspellend experiment gereed om te implementeren als een webservice. Als u de webservice voor uitvoer van slechts een subset van de resultaten wilt, kunt u bijvoorbeeld een module filteren voordat de uitvoerpoort toevoegen.

In dit conversieproces het trainingsexperiment niet worden verwijderd. Wanneer het proces voltooid is, hebt u twee tabbladen in Studio: één voor het trainingsexperiment en één voor de Voorspellend experiment. Op deze manier kunt u wijzigingen naar het experimentcanvas training voordat u uw webservice implementeren en Voorspellend experiment opnieuw opbouwen. Of u een kopie van het trainingsexperiment starten van een andere line-of experimenteren kunt opslaan.

> [!NOTE]
> Wanneer u klikt op **voorspellende webservice** starten van een automatisch proces als u wilt uw trainingsexperiment omzetten in een Voorspellend experiment en dit werkt goed in de meeste gevallen. Als uw trainingsexperiment complex (bijvoorbeeld: u hebt meerdere paden voor training die u samenvoegen), misschien wilt u deze conversie handmatig doen. Zie voor meer informatie [uw model voorbereiden voor implementatie in Azure Machine Learning Studio](convert-training-experiment-to-scoring-experiment.md).
> 
> 

### <a name="the-web-service"></a>De webservice
Als u tevreden bent dat uw Voorspellend experiment gereed is, u uw service als een klassieke webservice implementeren kunt of een nieuwe webservice op basis van Azure Resource Manager. Voor uw model operationeel door te implementeren als een *klassieke Machine Learning-webservice*, klikt u op **webservice implementeren** en selecteer **webservice implementeren [klassieke]**. Om te implementeren als *nieuwe Machine Learning-webservice*, klikt u op **webservice implementeren** en selecteer **Web Service implementeren [New]**. Gebruikers kunnen nu gegevens verzenden naar het model met de webservice REST-API en ontvangen van de resultaten. Zie [How to consume an Azure Machine Learning Web service](consume-web-services.md) (Azure Machine Learning-webservice gebruiken) voor meer informatie.

## <a name="the-non-typical-case-creating-a-non-predictive-web-service"></a>Het niet-standaard geval: maken van een niet-predictive-webservice
Als uw experiment heeft geen training een predictive Analytics-model, dan hebt u hoeft niet te maken van een trainingsexperiment zowel een score experiment - er is slechts één experiment en kunt u dit implementeren als een webservice. Machine Learning Studio detecteert of uw experiment een Voorspellend model bevat door een analyse van de modules die u hebt gebruikt.

Nadat u hebt op uw experiment herhaald en u tevreden met het bent:

1. Klik op **webservice ingesteld** en selecteer **webservice Retraining** - invoer en uitvoer knooppunten automatisch worden toegevoegd
2. Klik op **uitvoeren**
3. Klik op **webservice implementeren** en selecteer **webservice implementeren [klassieke]** of **Web Service implementeren [New]** , afhankelijk van de omgeving waarnaar u wilt implementeren.

Uw webservice wordt nu geïmplementeerd, en u kunt toegang tot en beheer deze net als bij een webservice voorspeld.

## <a name="updating-your-web-service"></a>Bijwerken van uw Web-service
Nu dat u hebt uw experiment geïmplementeerd als een webservice, wat gebeurt er als u wilt bijwerken?

Dat is afhankelijk van wat u nodig hebt om bij te werken:

**U wilt wijzigen van de invoer of uitvoer of u wilt wijzigen hoe gegevens worden bewerkt door de webservice**

Als u het model niet wilt wijzigen, maar alleen hoe gegevens worden verwerkt door de webservice wilt wijzigen, kunt u de Voorspellend experiment bewerken en klik vervolgens op **webservice implementeren** en selecteer **webservice implementeren [klassieke]** of **Web Service implementeren [New]** opnieuw. De Web-service is gestopt, de bijgewerkte Voorspellend experiment wordt geïmplementeerd en de Web-service opnieuw wordt opgestart.

Hier volgt een voorbeeld: Stel dat uw Voorspellend experiment de hele rij invoer gegevens met het verwachte resultaat retourneert. U kunt bepalen dat de webservice alleen het resultaat. Zodat u kunt toevoegen een **Projectkolommen** -module in de Voorspellend experiment, aan vóór de uitvoerpoort kolommen behalve het resultaat uitsluiten. Wanneer u klikt op **webservice implementeren** en selecteer **webservice implementeren [klassieke]** of **Web Service implementeren [New]** opnieuw de webservice wordt bijgewerkt.

**U wilt opnieuw trainen van het model met nieuwe gegevens**

Als u wilt behouden van uw machine learning-model, maar u wilt deze retrain met nieuwe gegevens, hebt u twee opties:

1. **Het model opnieuw trainen terwijl de webservice wordt uitgevoerd** -als u uw model retrain wilt terwijl de voorspellende webservice wordt uitgevoerd, kunt u dit doen door het maken van een aantal wijzigingen in het trainingsexperiment zodat het een ***retraining Experimenteer***, en vervolgens kunt u het implementeert als een  ***retraining web* service**. Zie voor instructies over hoe u dit doet, [Retrain Machine Learning-modellen programmatisch](retrain-models-programmatically.md).
2. **Ga terug naar de oorspronkelijke trainingsexperiment en verschillende trainingsgegevens gebruiken voor het ontwikkelen van uw model** : uw Voorspellend experiment is gekoppeld aan de webservice, maar het trainingsexperiment niet rechtstreeks op deze manier is gekoppeld. Als u de oorspronkelijke trainingsexperiment wijzigen en klik op **webservice ingesteld**, wordt gemaakt een *nieuwe* voorspellende experimenteren die, wanneer geïmplementeerd, maakt u een *nieuwe* Web de service. Hiermee alleen de oorspronkelijke webservice niet bijgewerkt.
   
   Als u het trainingsexperiment wijzigen, opent u het en klik op **OpslaanAls** een kopie te maken. Dit wordt het oorspronkelijke trainingsexperiment Voorspellend experiment behouden en webservice. U kunt nu een nieuwe webservice maken met uw wijzigingen. Zodra u de nieuwe webservice die u kunt vervolgens beslissen of de vorige Web-service stoppen of te laten samen met de nieuwe knop hebt geïmplementeerd.

**U wilt een ander model trainen**

Als u aanbrengen in uw oorspronkelijke Voorspellend experiment, zoals het selecteren van een andere machine learning-algoritme wilt, probeert een ander training methode, enz., moet u de tweede procedure hierboven beschreven voor het model retraining volgen: open de training experiment, klikt u op **OpslaanAls** een kopie maken en start u het nieuwe pad van uw model te ontwikkelen, de Voorspellend experiment maken en implementeren van de webservice. Hiermee maakt u een nieuw Web service ongerelateerde naar de oorspronkelijke - kunt u bepalen welke, of beide, blijven uitvoeren.

## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen voor meer informatie over het proces van ontwikkeling en experiment:

* converteren van het experiment - [uw model voorbereiden voor implementatie in Azure Machine Learning Studio](convert-training-experiment-to-scoring-experiment.md)
* implementatie van de webservice - [een Azure Machine Learning-webservice implementeren](publish-a-machine-learning-web-service.md)
* de model - retraining [modellen programmatisch opnieuw trainen Machine Learning](retrain-models-programmatically.md)

Zie voor voorbeelden van het hele proces:

* [Machine learning-zelfstudie: uw eerste experiment maken in Azure Machine Learning Studio](create-experiment.md)
* [Overzicht: Een predictive analytics-oplossing voor kredietrisicobeoordeling in Azure Machine Learning ontwikkelen](walkthrough-develop-predictive-solution.md)

