---
title: 'Stap 5: De Machine Learning Studio-webservice implementeren | Microsoft Docs'
description: 'Stap 5 van het ontwikkelen van een voorspellende oplossing-overzicht: een Voorspellend experiment in Machine Learning Studio als een webservice implementeren.'
services: machine-learning
documentationcenter: ''
author: ericlicoding
ms.custom: (previous ms.author=yahajiza, author=YasinMSFT)
ms.author: amlstudiodocs
manager: hjerez
editor: cgronlun
ms.assetid: 3fca74a3-c44b-4583-a218-c14c46ee5338
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2017
ms.openlocfilehash: 33965270c2be6f70614def79a49f1c4aa1a8fbbc
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/26/2018
ms.locfileid: "52309926"
---
# <a name="walkthrough-step-5-deploy-the-azure-machine-learning-studio-web-service"></a>Kennismaken, stap 5: De Azure Machine Learning Studio-webservice implementeren
Dit is de vijfde stap van de procedure [predictive analytics-oplossing in Azure Machine Learning ontwikkelen](walkthrough-develop-predictive-solution.md)

1. [Een Machine Learning-werkruimte maken](walkthrough-1-create-ml-workspace.md)
2. [Bestaande gegevens uploaden](walkthrough-2-upload-data.md)
3. [Een nieuw experiment maken](walkthrough-3-create-new-experiment.md)
4. [De modellen trainen en evalueren](walkthrough-4-train-and-evaluate-models.md)
5. **De webservice implementeren**
6. [De webservice openen](walkthrough-6-access-web-service.md)

- - -
Als u wilt een kans om te gebruiken de Voorspellend model dat we hebben ontwikkeld die in dit overzicht anderen, kunnen we het implementeren als een webservice op Azure.

Tot nu hebben we is experimenteren met het trainen van ons model. Maar de geïmplementeerde service is niet meer gaan doen training - gaat het genereren van nieuwe voorspellingen van de invoer van de gebruiker op basis van onze model scoren. We gaan doen voorbereiding om te converteren van dit experiment uit een ***training*** experimenteren naar een ***voorspellende*** experimenteren. 

Dit is een proces drie stappen:  

1. Verwijder een van de modellen
2. Converteer de *trainingsexperiment* we hebben gemaakt in een *Voorspellend experiment*
3. De Voorspellend experiment implementeren als een webservice

## <a name="remove-one-of-the-models"></a>Verwijder een van de modellen

Eerst moet dit experiment iets trim omlaag. Momenteel hebben we twee verschillende modellen in het experiment, maar we willen alleen één model gebruiken wanneer we dit als een webservice implementeert.  

Stel dat hebben we besloten dat de boosted structuur model beter presteert dan de SVM-model. Dus verwijder de Allereerst moet u de [Two-Class Support Vector Machine] [ two-class-support-vector-machine] -module en de modules die zijn gebruikt voor het trainen van het. U wilt mogelijk maakt u eerst een kopie van het experiment door te klikken op **OpslaanAls** aan de onderkant van het experimentcanvas.

We moeten de volgende modules verwijderen:  

* [Two-Class Support Vector Machine][two-class-support-vector-machine]
* [Model te trainen] [ train-model] en [Score Model] [ score-model] modules die zijn verbonden met het
* [Gegevens normaliseren] [ normalize-data] (van beide)
* [Model evalueren] [ evaluate-model] (omdat we klaar bent met het evalueren van de modellen)

Selecteer elke module en druk op Delete te drukken, of met de rechtermuisknop op de module en selecteer **verwijderen**. 

![Het model SVM verwijderd][3a]

Ons model ziet er ongeveer als volgt:

![Het model SVM verwijderd][3]

Nu kunt u implementeren in dit model met behulp van de [Two-Class Boosted beslissingsstructuur][two-class-boosted-decision-tree].

## <a name="convert-the-training-experiment-to-a-predictive-experiment"></a>Het trainingsexperiment converteren naar een Voorspellend experiment

Om dit model klaar is voor implementatie, moeten we deze trainingsexperiment converteren naar een Voorspellend experiment. Dit omvat drie stappen:

1. Sla het model dat we hebben getraind en vervangt u onze trainingsmodules
2. Verwijder het experiment als u wilt verwijderen van modules die zijn alleen nodig voor training
3. Bepalen waar de webservice invoer accepteert en waarin de uitvoer wordt gegenereerd

We kunnen dit handmatig doen, maar gelukkig alle drie stappen kunnen worden bereikt door te klikken op **webservice ingesteld** aan de onderkant van het experimentcanvas (en selecteren van de **voorspellende webservice** optie).

> [!TIP]
> Als u wilt meer informatie over wat er gebeurt wanneer u een opleidingsexperiment converteren naar een Voorspellend experiment, Zie [over het voorbereiden van uw model voor implementatie in Azure Machine Learning Studio](convert-training-experiment-to-scoring-experiment.md).

Wanneer u klikt op **webservice ingesteld**, op verschillende manieren gebeuren:

* Het getrainde model wordt geconverteerd naar een enkele **getrainde Model** module en opgeslagen in het modulepalet links van het experimentcanvas (u vindt deze onder **getrainde modellen**)
* Modules die zijn gebruikt voor trainingen, worden verwijderd; met name:
  * [Two-Class Boosted-beslisboom][two-class-boosted-decision-tree]
  * [Train Model][train-model]
  * [Split Data][split]
  * de tweede [R-Script uitvoeren] [ execute-r-script] -module die is gebruikt voor testgegevens
* Het opgeslagen getrainde model wordt toegevoegd in het experiment
* **Web service invoer** en **Web service uitvoer** modules worden toegevoegd (worden geïdentificeerd waarbij gegevens van de gebruiker voert het model en welke gegevens worden geretourneerd wanneer de web-service wordt geopend)

> [!NOTE]
> U kunt zien dat het experiment wordt opgeslagen in twee onderdelen onder tabbladen die zijn toegevoegd aan de bovenkant van het experimentcanvas. De oorspronkelijke trainingsexperiment is op het tabblad **trainingsexperiment**, en wordt de zojuist gemaakte Voorspellend experiment **Voorspellend experiment**. De Voorspellend experiment is de die we als een webservice implementeert.

We moeten voor een aanvullende stap met dit bepaalde experiment.
We hebben twee toegevoegd [R-Script uitvoeren] [ execute-r-script] modules voor een gewicht van functie tot de gegevens. Dat is slechts een truc die we nodig hebben voor het trainen en te testen, dus we uit deze modules in het uiteindelijke model kiezen kunnen.
Machine Learning Studio verwijderd een [R-Script uitvoeren] [ execute-r-script] module wanneer deze verwijderd de [splitsen] [ split] module. Nu kunnen we de andere verwijderen en verbinding maken [metagegevens Editor] [ metadata-editor] rechtstreeks naar [Score Model][score-model].    

Onze experiment moet er nu als volgt uitzien:  

![Het getrainde model scoren][4]  

> [!NOTE]
> U vraagt zich misschien af waarom we de gegevensset UCI Duits creditcardgegevens links in de Voorspellend experiment. De service gaat van de gebruiker gegevens, niet de oorspronkelijke gegevensset te beoordelen, dus waarom laat u de oorspronkelijke gegevensset in het model?
> 
> Het is waar de service hoeft niet de oorspronkelijke gegevens van de creditcard. Maar hoeft het schema voor die gegevens, die uit informatie zoals het aantal kolommen in er zijn en welke kolommen zijn numerieke. Dit schema-informatie is nodig om gegevens van de gebruiker worden geïnterpreteerd. Laten we deze onderdelen die zijn gekoppeld, zodat de scoring-module bevat een schema van de gegevensset als de service wordt uitgevoerd. De gegevens wordt niet gebruikt, alleen het schema.  
> 
>Een van de belangrijke dingen om te weten is dat als de oorspronkelijke gegevensset deel uitmaakt van het label, klikt u vervolgens het verwachte schema van de web-invoer ook verwacht een kolom met het label dat. Er is een manier om dit om het label en alle andere gegevens die in de gegevensset training is, maar worden niet in de web-invoer, voordat u verbinding maakt de web-invoer- en training-gegevensset naar een algemene module te verwijderen. 
> 

Voer het experiment een keer (Klik op **uitvoeren**.) Als u controleren wilt of het model nog steeds werkt, klikt u op de uitvoer van de [Score Model] [ score-model] -module en selecteer **resultaten weergeven**. U kunt zien dat de oorspronkelijke gegevens wordt weergegeven, samen met de waarde van het tegoed risico's ("Scored Labels') en de scoring kanswaarde ("score kansen'.) 

## <a name="deploy-the-web-service"></a>De webservice implementeren
U kunt het experiment implementeren als een van beide een klassieke webservice of als een nieuwe webservice die gebaseerd op Azure Resource Manager.

### <a name="deploy-as-a-classic-web-service"></a>Als een klassieke webservice implementeren
Voor het implementeren van een klassieke webservice die is afgeleid van onze experiment, klikt u op **webservice implementeren** onder het canvas en selecteer **webservice implementeren [klassieke]**. Machine Learning Studio implementeert het experiment als een webservice en gaat u naar het dashboard voor de webservice. Op deze pagina kunt u terugkeren naar het experimentcanvas (**weergave momentopname** of **weergeven het meest recente**) en voert u een eenvoudige test van de webservice (Zie **testen van de webservice** hieronder). Er is ook hier informatie voor het maken van toepassingen die toegang hebben tot de webservice (meer hierover in de volgende stap van deze procedure).

![Dashboard van de webservice][6]

U kunt de service configureren door te klikken op de **configuratie** tabblad. Hier kunt u de naam van de service (dit is de naam van het experiment standaard toegekend) wijzigen en wijs hieraan een beschrijving. U kunt ook meer beschrijvende labels bieden voor de invoer- en -gegevens.  

![Configureer de webservice][5]  

### <a name="deploy-as-a-new-web-service"></a>Als een nieuwe webservice implementeren

> [!NOTE] 
> Voor het implementeren van een nieuwe webservice moet u voldoende machtigingen hebben in het abonnement waarvan u de webservice implementeert. Zie voor meer informatie, [beheren van een webservice met behulp van de Azure Machine Learning-webserviceportal](manage-new-webservice.md). 

Een nieuwe webservice implementeren afgeleid van onze experiment:

1. Klik op **webservice implementeren** onder het canvas en selecteer **Web Service implementeren [Nieuw]**. Machine Learning Studio brengt u naar de Azure Machine Learning-webservices **Experiment implementeren** pagina.

2. Voer een naam voor de webservice. 

3. Voor **Prijsplan**, u kunt een bestaande prijsstelling selecteren of selecteert u 'Nieuw' en geef een naam op voor het nieuwe abonnement en selecteert u de optie voor maandelijks plannen. De standaardwaarde van de lagen plan voor de abonnementen voor uw standaardregio en wordt uw webservice wordt geïmplementeerd voor deze regio.

4. Klik op **implementeren**.

Na een paar minuten de **snelstartgids** pagina voor uw webservice wordt geopend.

U kunt de service configureren door te klikken op de **configureren** tabblad. Hier kunt u de service wijzigen van titel en wijs hieraan een beschrijving. 

Als u wilt testen van de webservice, klikt u op de **testen** tabblad (Zie **testen van de webservice** hieronder). Voor informatie over het maken van toepassingen die toegang hebben tot de webservice, klikt u op de **verbruiken** tabblad (is de volgende stap in dit scenario gaat meer informatie).

> [!TIP]
> Nadat u deze hebt geïmplementeerd, kunt u de webservice bijwerken. Bijvoorbeeld, als u wijzigen van het model wilt vervolgens u het trainingsexperiment bewerkt, aanpassen van de Modelparameters en klikt u op **webservice implementeren**, Klik daarvoor **webservice implementeren [klassieke]** of **[Nieuw]-webservice implementeren**. Wanneer u het experiment opnieuw implementeert, wordt deze vervangen door de webservice, nu met behulp van het model van uw bijgewerkte.  
> 
> 

## <a name="test-the-web-service"></a>De webservice testen

Als de webservice wordt geopend, krijgt de gegevens van de gebruiker via de **Web service invoer** module waar deze wordt doorgegeven aan de [Score Model] [ score-model] module en gewaardeerd. De manier waarop die we de Voorspellend experiment hebt ingesteld, het model wordt verwacht dat gegevens in dezelfde indeling als de oorspronkelijke credit risk gegevensset.
De resultaten worden geretourneerd aan de gebruiker van de webservice via de **Web service uitvoer** module.

> [!TIP]
> De manier waarop we de Voorspellend experiment geconfigureerd, zijn de volledige resultaat is van de [Score Model] [ score-model] module worden geretourneerd. Dit omvat de ingevoerde gegevens plus de waarde van de risico-tegoed en de kans op score. Maar u kunt iets anders als u wilt terugkeren: u kunt bijvoorbeeld alleen de tegoed risico waarde retourneren. Om dit te doen, Voeg een [Projectkolommen] [ project-columns] module tussen [Score Model] [ score-model] en de **Web service uitvoer**te elimineren kolommen u niet wilt dat de webservice om terug te keren. 
> 
> 

U kunt testen een klassieke web service in **Machine Learning Studio** of in de **Azure Machine Learning-webservices** portal.
U kunt een nieuw web test service alleen in de **Machine Learning-webservices** portal.

> [!TIP]
> Wanneer u test in de portal voor Azure Machine Learning-webservices, kunt u de voorbeeldgegevens die u gebruiken kunt voor het testen van de Request Response-service maken-portal hebt. Op de **configureren** pagina, selecteert u 'Ja' voor **Sample Data ingeschakeld?**. Wanneer u de Request Response-tabblad openen op de **Test** pagina, de portal ingevuld voorbeeldgegevens afkomstig uit de oorspronkelijke credit risk gegevensset.

### <a name="test-a-classic-web-service"></a>Een klassieke webservice testen

U kunt een klassieke webservice testen in Machine Learning Studio of in de portal voor Machine Learning-webservices. 

#### <a name="test-in-machine-learning-studio"></a>Testen in Machine Learning Studio

1. Op de **DASHBOARD** pagina voor de webservice, klikt u op de **Test** knop onder **standaard eindpunt**. Een dialoogvenster wordt weergegeven en u wordt gevraagd de invoergegevens voor de service. Dit zijn dezelfde kolommen die zijn opgetreden in de oorspronkelijke credit risk gegevensset.  

2. Voer een set gegevens en klik vervolgens op **OK**. 

#### <a name="test-in-the-machine-learning-web-services-portal"></a>Testen in de portal voor Machine Learning-webservices

1. Op de **DASHBOARD** pagina voor de webservice, klikt u op de **Test preview** koppeling onder **standaard eindpunt**. De testpagina in de portal voor Azure Machine Learning-webservices voor de webservice-eindpunt wordt geopend en u wordt gevraagd de invoergegevens voor de service. Dit zijn dezelfde kolommen die zijn opgetreden in de oorspronkelijke credit risk gegevensset.

2. Klik op **testen Request Response**. 

### <a name="test-a-new-web-service"></a>Een nieuwe webservice betreft testen

U kunt een nieuwe webservice betreft testen alleen in de portal voor Machine Learning-webservices.

1. In de [Azure Machine Learning-webservices](https://services.azureml.net/quickstart) en klik op **Test** aan de bovenkant van de pagina. De **Test** pagina wordt geopend en u kunt gegevens voor de service. De velden weergegeven komen overeen met de kolommen die zijn opgetreden in de oorspronkelijke credit risk gegevensset. 

2. Voer een set gegevens en klik vervolgens op **Test Request Response**.

De resultaten van de test worden weergegeven aan de rechterkant van de pagina in de uitvoerkolom. 


## <a name="manage-the-web-service"></a>De webservice beheren

Als u kunt uw web-service hebt geïmplementeerd, of het klassieke of nieuwe, kunt u het beheren van de [Microsoft Azure Machine Learning-webservices](https://services.azureml.net/quickstart) portal.

Voor het bewaken van de prestaties van uw webservice:

1. Aanmelden bij de [Microsoft Azure Machine Learning-webservices](https://services.azureml.net/quickstart) portal
2. Klik op **Web services**
3. Klik op de webservice
4. Klik op de **Dashboard**

- - -
**Volgende stap: [toegang tot de webservice](walkthrough-6-access-web-service.md)**

[3]: ./media/walkthrough-5-publish-web-service/publish3.png
[3a]: ./media/walkthrough-5-publish-web-service/publish3a.png
[4]: ./media/walkthrough-5-publish-web-service/publish4.png
[5]: ./media/walkthrough-5-publish-web-service/publish5.png
[6]: ./media/walkthrough-5-publish-web-service/publish6.png


<!-- Module References -->
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[metadata-editor]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[normalize-data]: https://msdn.microsoft.com/library/azure/986df333-6748-4b85-923d-871df70d6aaf/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[two-class-boosted-decision-tree]: https://msdn.microsoft.com/library/azure/e3c522f8-53d9-4829-8ea4-5c6a6b75330c/
[two-class-support-vector-machine]: https://msdn.microsoft.com/library/azure/12d8479b-74b4-4e67-b8de-d32867380e20/
[project-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
