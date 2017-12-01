---
title: 'Stap 5: De Machine Learning-webservice implementeren | Microsoft Docs'
description: 'Stap 5 van de prognose een voorspellende oplossing overzicht: een Voorspellend experiment in Machine Learning Studio als een webservice implementeert.'
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 3fca74a3-c44b-4583-a218-c14c46ee5338
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2017
ms.author: garye
ms.openlocfilehash: ba8f1678d87159088c58cf0e05e0fe5a6579b358
ms.sourcegitcommit: 5a6e943718a8d2bc5babea3cd624c0557ab67bd5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2017
---
# <a name="walkthrough-step-5-deploy-the-azure-machine-learning-web-service"></a>Kennismaken, stap 5: De Azure Machine Learning-webservice implementeren
Dit is de vijfde stap van de procedure [predictive analytics-oplossing in Azure Machine Learning ontwikkelen](walkthrough-develop-predictive-solution.md)

1. [Een Machine Learning-werkruimte maken](walkthrough-1-create-ml-workspace.md)
2. [Bestaande gegevens uploaden](walkthrough-2-upload-data.md)
3. [Een nieuw experiment maken](walkthrough-3-create-new-experiment.md)
4. [De modellen trainen en evalueren](walkthrough-4-train-and-evaluate-models.md)
5. **De webservice implementeren**
6. [De webservice openen](walkthrough-6-access-web-service.md)

- - -
Om de kans te gebruiken het voorspellende model dat we hebben ontwikkeld in dit overzicht anderen, kunnen we het implementeren als een webservice in Azure.

Naar dit punt Experimenteer we met het model te trainen. Maar de geïmplementeerde service niet meer wilt doen training - gaat het genereren van nieuwe voorspellingen door score berekenen voor invoer van de gebruiker op basis van het model. Dus gaan we voorbereiding converteren dit experiment uit een ***training*** experimenteren naar een ***voorspellende*** experimenteren. 

Dit is een proces drie stappen:  

1. Verwijder een van de modellen
2. Converteer de *trainingsexperiment* we hebt gemaakt in een *Voorspellend experiment*
3. De Voorspellend experiment implementeren als een webservice

## <a name="remove-one-of-the-models"></a>Verwijder een van de modellen

Eerst moet dit experiment enigszins trim omlaag. Momenteel hebben we twee verschillende modellen in het experiment, maar we alleen wilt gebruiken één model wanneer we dit als een webservice implementeren.  

Stel, dat we hebt besloten dat de gestimuleerd structuur model beter presteert dan het SVM-model. Zodat het eerste wat te doen is verwijderen de [Two-Class ondersteuning Vectormachine] [ two-class-support-vector-machine] module en de modules die werden gebruikt voor het trainen van het. U kunt een kopie maken van het experiment eerst door te klikken op **OpslaanAls** aan de onderkant van het experimentcanvas.

We moeten de volgende modules te verwijderen:  

* [Vectormachine Two-Class-ondersteuning][two-class-support-vector-machine]
* [Model trainen] [ train-model] en [Score Model] [ score-model] modules die zijn verbonden
* [Gegevens normaliseren] [ normalize-data] (van beide)
* [Model evalueren] [ evaluate-model] (omdat we klaar bent met het evalueren van de modellen)

Selecteer elke module en druk op de toets Delete of met de rechtermuisknop op de module en selecteer **verwijderen**. 

![Het model SVM verwijderd][3a]

Het model moet nu als volgt uitzien:

![Het model SVM verwijderd][3]

Nu we klaar zijn voor het implementeren van dit model met behulp van de [Two-Class Boosted Decision Tree][two-class-boosted-decision-tree].

## <a name="convert-the-training-experiment-to-a-predictive-experiment"></a>Het trainingsexperiment converteren naar een Voorspellend experiment

Om dit model klaar voor implementatie, moet dit trainingsexperiment converteren naar een Voorspellend experiment. Dit omvat drie stappen:

1. Sla het model dat we hebben getraind en vervang onze trainingsmodules
2. Het experiment voor het verwijderen van modules die zijn alleen nodig voor training Trim
3. Definiëren waar de webservice invoer accepteert en waarin de uitvoer wordt gegenereerd

We kunnen dit handmatig doen, maar gelukkig alle drie stappen kunnen worden uitgevoerd door te klikken op **webservice ingesteld** aan de onderkant van het experimentcanvas (en het selecteren van de **voorspellende webservice** optie).

> [!TIP]
> Als u meer informatie wilt over wat er gebeurt wanneer u een trainingsexperiment converteren naar een Voorspellend experiment, Zie [uw model voorbereiden voor implementatie in Azure Machine Learning Studio](convert-training-experiment-to-scoring-experiment.md).

Wanneer u klikt op **webservice ingesteld**, op verschillende manieren gebeuren:

* Het getrainde model wordt geconverteerd naar één **getrainde Model** module en opgeslagen in het modulepalet links van het experimentcanvas (u vindt deze onder **Trained Models**)
* Modules die zijn gebruikt voor training zijn verwijderd; specifieke opdrachten:
  * [Two-Class gestimuleerd beslissingsstructuur][two-class-boosted-decision-tree]
  * [Train Model][train-model]
  * [Gegevens splitsen][split]
  * de tweede [R-Script uitvoeren] [ execute-r-script] module die is gebruikt voor testgegevens
* Het opgeslagen getrainde model weer toegevoegd aan het experiment
* **Web-invoer** en **Web service uitvoer** modules worden toegevoegd (deze vaststellen waar gegevens van de gebruiker voert het model en welke gegevens worden geretourneerd wanneer de web-service is geopend)

> [!NOTE]
> U kunt zien dat het experiment wordt opgeslagen in twee delen onder tabbladen die zijn toegevoegd aan de bovenkant van het experimentcanvas. De oorspronkelijke trainingsexperiment is op het tabblad **trainingsexperiment**, en de zojuist gemaakte Voorspellend experiment ligt onder de **Voorspellend experiment**. De Voorspellend experiment is de die we als een webservice implementeert.

Er moet een aanvullende stap met dit bepaalde experiment uitvoeren.
We twee toegevoegd [R-Script uitvoeren] [ execute-r-script] modules voor een functie weging tot de gegevens. Dat is slechts een slag die we nodig voor het trainen en te testen, zodat we uit deze modules in het laatste model kunt nemen.
Machine Learning Studio verwijderd een [R-Script uitvoeren] [ execute-r-script] module wanneer u deze verwijderen de [gesplitste] [ split] module. Nu kan het andere verwijderen en verbinding maken met [metagegevens Editor] [ metadata-editor] rechtstreeks naar [Score Model][score-model].    

Onze experiment ziet er nu als volgt:  

![Score berekenen voor het getrainde model][4]  

> [!NOTE]
> U vraagt zich misschien af waarom we de gegevensset UCI Duits creditcardgegevens achtergebleven in de Voorspellend experiment. De service wilt beoordelen van gegevens van de gebruiker, niet de oorspronkelijke gegevensset, dus waarom laat de oorspronkelijke gegevensset in het model?
> 
> Het is waar de service hoeft niet de oorspronkelijke creditcard-gegevens. Maar hoeft het schema voor die gegevens, waaronder gegevens zoals het aantal kolommen zijn en welke kolommen numeriek zijn. Dit schema-informatie is nodig om gegevens van de gebruiker te interpreteren. We laten deze onderdelen die zijn verbonden, zodat de scoremodule het schema van de dataset heeft wanneer de service wordt uitgevoerd. De gegevens wordt niet gebruikt, alleen het schema.  
> 
> 

Voer het experiment outproblemen (Klik op **uitvoeren**.) Als u controleren wilt of het model nog steeds werkt, klikt u op de uitvoer van de [Score Model] [ score-model] module en selecteer **resultaten weergeven**. U kunt zien dat de oorspronkelijke gegevens worden weergegeven, samen met de waarde van tegoed risico's ('Scored Labels') en de scoreprofiel waarschijnlijkheidswaarde ('berekend kansen'.) 

## <a name="deploy-the-web-service"></a>De webservice implementeren
U kunt het experiment implementeren als ofwel Classic webservice of als een nieuwe webservice die gebaseerd op Azure Resource Manager.

### <a name="deploy-as-a-classic-web-service"></a>Als een klassiek-webservice implementeren
Klik voor het implementeren van een klassiek-webservice die is afgeleid van onze experiment **webservice implementeren** onder het canvas en selecteer **webservice implementeren [klassieke]**. Machine Learning Studio implementeert het experiment als een webservice en gaat u naar het dashboard voor de webservice. Op deze pagina kunt u terugkeren naar het experimentcanvas (**weergave momentopname** of **geven het meest recente**) en een eenvoudige test van de webservice wordt uitgevoerd (Zie **testen van de webservice** hieronder). Er is ook hier informatie voor het maken van toepassingen die toegang heeft tot de webservice (meer informatie over die in de volgende stap van deze rondleiding).

![Web service-dashboard][6]

U kunt de service configureren door te klikken op de **configuratie** tabblad. Hier kunt u de naam van de service (dit is de naam van het experiment standaard toegekend) wijzigen en wijs hieraan een beschrijving. U kunt ook meer beschrijvende labels geven voor de invoer- en -gegevens.  

![Configureer de webservice][5]  

### <a name="deploy-as-a-new-web-service"></a>Als een nieuwe webservice implementeren

> [!NOTE] 
> Voor het implementeren van een nieuwe webservice moet u voldoende machtigingen hebben in het abonnement waaraan u de webservice implementeert. Zie voor meer informatie [beheren van een webservice via de portal voor Azure Machine Learning-webservices](manage-new-webservice.md). 

Voor het implementeren van een nieuwe webservice afgeleid van onze experiment:

1. Klik op **webservice implementeren** onder het canvas en selecteer **Web Service implementeren [New]**. Machine Learning Studio brengt u naar de Azure Machine Learning-webservices **implementeren Experiment** pagina.

2. Voer een naam voor de webservice. 

3. Voor **prijs Plan**, kan een bestaande prijscategorie selecteren of 'Nieuw maken' en geef een naam op voor het nieuwe plan en selecteer de optie voor maandelijks plannen. De standaardwaarde van de lagen abonnement op de plannen voor uw regio standaard en uw web-service wordt geïmplementeerd in deze regio.

4. Klik op **implementeren**.

Na een paar minuten de **Quick Start** pagina voor uw webservice wordt geopend.

U kunt de service configureren door te klikken op de **configureren** tabblad. Hier kunt u de service wijzigen title en wijs hieraan een beschrijving. 

Als u wilt testen van de webservice, klikt u op de **testen** tabblad (Zie **testen van de webservice** hieronder). Voor informatie over het maken van toepassingen die toegang heeft tot de webservice, klikt u op de **verbruiken** tabblad (de volgende stap in dit scenario gaat in meer detail).

> [!TIP]
> Nadat u deze hebt geïmplementeerd, kunt u de webservice bijwerken. Bijvoorbeeld, als u uw model wijzigen wilt en vervolgens kunt u het trainingsexperiment, aanpassen van de Modelparameters en klikt u op **webservice implementeren**, selecteren **webservice implementeren [klassieke]** of **[New]-webservice implementeren**. Wanneer u het experiment opnieuw implementeert, vervangt dan de web-service, nu met gebruik van uw bijgewerkte model.  
> 
> 

## <a name="test-the-web-service"></a>De webservice testen

Als de webservice wordt geopend, krijgt de gegevens van de gebruiker via de **Web service invoer** module waar wordt doorgegeven aan de [Score Model] [ score-model] module en gewaardeerd. De manier waarop die we de Voorspellend experiment hebt ingesteld, het model verwacht gegevens in dezelfde indeling als de oorspronkelijke tegoed risico gegevensset.
De resultaten worden geretourneerd naar de gebruiker van de webservice via de **Web service uitvoer** module.

> [!TIP]
> De manier waarop we de Voorspellend experiment geconfigureerd, hebben de gehele resultaat is van de [Score Model] [ score-model] module worden geretourneerd. Dit omvat alle ingevoerde gegevens plus de tegoed risico waarde en de kans op scoreprofiel. Maar u kunt een iets andere betekenis als u wilt terugkeren - u kan bijvoorbeeld alleen de tegoed risico waarde retourneren. U doet dit door invoegen een [Projectkolommen] [ project-columns] module tussen [Score Model] [ score-model] en de **Web service uitvoer**elimineren kolommen u niet wilt dat de webservice om terug te keren. 
> 
> 

U kunt een klassiek web testen in service **Machine Learning Studio** of in de **Azure Machine Learning-webservices** portal.
U kunt een nieuw web testen service alleen in de **Machine Learning-webservices** portal.

> [!TIP]
> Wanneer u in de portal voor Azure Machine Learning-webservices test, kunt u de portal voorbeeldgegevens die u gebruiken kunt voor het testen van de service aanvragen en antwoorden maken kunt hebben. Op de **configureren** pagina, selecteert u 'Ja' voor **voorbeeld gegevens ingeschakeld?**. Wanneer u opent het tabblad aanvragen en antwoorden op de **Test** pagina de portal ingevuld voorbeeldgegevens die afkomstig zijn uit de oorspronkelijke tegoed risico gegevensset.

### <a name="test-a-classic-web-service"></a>Een klassieke webservice testen

U kunt een webservice klassieke testen in Machine Learning Studio of in de portal van de Machine Learning-webservices. 

#### <a name="test-in-machine-learning-studio"></a>Testen in Machine Learning Studio

1. Op de **DASHBOARD** pagina voor de webservice, klikt u op de **Test** knop onder **eindpunt standaard**. Een dialoogvenster weergegeven en u wordt gevraagd de invoergegevens van de service. Dit zijn dezelfde kolommen die zijn opgetreden in de oorspronkelijke tegoed risico gegevensset.  

2. Geef een set gegevens en klik op **OK**. 

#### <a name="test-in-the-machine-learning-web-services-portal"></a>Testen in de portal voor Machine Learning-webservices

1. Op de **DASHBOARD** pagina voor de webservice, klikt u op de **Test preview** koppeling onder **eindpunt standaard**. De testpagina in de portal voor Azure Machine Learning-webservices voor de webservice-eindpunt wordt geopend en u wordt gevraagd de invoergegevens van de service. Dit zijn dezelfde kolommen die zijn opgetreden in de oorspronkelijke tegoed risico gegevensset.

2. Klik op **testen aanvragen en antwoorden**. 

### <a name="test-a-new-web-service"></a>Een nieuwe webservice testen

U kunt een nieuwe webservice testen alleen in de portal voor Machine Learning-webservices.

1. In de [Azure Machine Learning-webservices](https://services.azureml.net/quickstart) en klik op **Test** boven aan de pagina. De **Test** pagina wordt geopend en u kunt gegevens voor de service. De invoervelden weergegeven overeenkomen met de kolommen die zijn opgetreden in de oorspronkelijke tegoed risico gegevensset. 

2. Geef een set gegevens en klik op **Test aanvragen en antwoorden**.

De resultaten van de test worden weergegeven aan de rechterkant van de pagina in de uitvoerkolom. 


## <a name="manage-the-web-service"></a>De webservice beheren

Als u hebt uw webservice geïmplementeerd of klassieke of nieuwe, kunt u het beheren van de [Microsoft Azure Machine Learning-webservices](https://services.azureml.net/quickstart) portal.

Voor het bewaken van de prestaties van uw web-service:

1. Aanmelden bij de [Microsoft Azure Machine Learning-webservices](https://services.azureml.net/quickstart) portal
2. Klik op **webservices**
3. Klik op uw web-service
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
[project-columns]: https://msdn.microsoft.com/en-us/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
