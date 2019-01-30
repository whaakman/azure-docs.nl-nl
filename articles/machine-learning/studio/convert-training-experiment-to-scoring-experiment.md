---
Titel: Model voor implementatie titleSuffix voorbereiden: Azure Machine Learning Studio description: Klik hier voor meer informatie over het voorbereiden van het getrainde model voor implementatie als een webservice op uw Machine Learning Studio-trainingsexperiment converteren naar een Voorspellend experiment.
Services: machine learning ms.service: machine learning ms.subservice: studio ms.topic: artikel

Auteur: ericlicoding ms.author: amlstudiodocs ms.date: 03/28/2017
---
# <a name="how-to-prepare-your-model-for-deployment-in-azure-machine-learning-studio"></a>Over het voorbereiden van uw model voor implementatie in Azure Machine Learning Studio

Azure Machine Learning Studio biedt u de hulpmiddelen die u nodig hebt voor het ontwikkelen van een predictive analytics-model en dit vervolgens te implementeren door deze te implementeren als een Azure-web-service.

U doet dit door u Studio kunt gebruiken om u te maken van een experiment - met de naam een *trainingsexperiment* - waar u trainen, beoordelen en uw model bewerken. Wanneer u tevreden bent, krijgt uw model klaar om te implementeren door te converteren van uw trainingsexperiment op een *Voorspellend experiment* dat geconfigureerd voor de score gebruikersgegevens.

U ziet een voorbeeld van dit proces in [scenario: Predictive analytics-oplossing voor kredietrisicobeoordeling in Azure Machine Learning ontwikkelen](walkthrough-develop-predictive-solution.md).

In dit artikel wordt een dieper ingaan op de details van hoe een opleidingsexperiment wordt geconverteerd naar een Voorspellend experiment en hoe die Voorspellend experiment is geïmplementeerd. Door de volgende details, kunt u informatie over het configureren van uw geïmplementeerd model zodat het efficiëntere.



## <a name="overview"></a>Overzicht 

Het proces van het een opleidingsexperiment converteren naar een Voorspellend experiment bestaat uit drie stappen:

1. Vervang de machine learning-algoritme voor modules met het getrainde model.
2. Verwijder het experiment alleen de modules die nodig zijn voor het scoren. Een opleidingsexperiment omvat een aantal modules die nodig zijn voor de training, maar niet nodig zijn wanneer het model wordt getraind.
3. Definiëren hoe het model van uw gegevens van de gebruiker van de web-service worden geaccepteerd en welke gegevens worden geretourneerd.

> [!TIP]
> U hebt met training en scoren van uw model met behulp van uw eigen gegevens betrokken zijn in uw trainingsexperiment. Maar zodra geïmplementeerd, gebruikers nieuwe gegevens wordt verzonden aan uw model en voorspellingsresultaten wordt geretourneerd. Dus als u uw trainingsexperiment naar een Voorspellend experiment converteren zodat deze klaar is voor implementatie, houd er rekening mee hoe het model wordt gebruikt door anderen.
> 
> 

## <a name="set-up-web-service-button"></a>Knop Web Service instellen
Nadat u uw experiment uitvoeren (klikt u op **uitvoeren** aan de onderkant van het experimentcanvas), klikt u op de **webservice ingesteld** knop (Selecteer de **voorspellende webservice** optie). **Web-Service instellen** voert u de drie stappen uit uw trainingsexperiment converteren naar een Voorspellend experiment:

1. Slaat hij het getrainde model in de **getrainde modellen** gedeelte van het modulepalet (aan de linkerkant van het experimentcanvas). Deze wordt vervangen door de algoritme voor machine learning en [Train Model] [ train-model] modules met de opgeslagen getrainde model.
2. Deze analyseert uw experiment en verwijdert u modules die zijn duidelijk wordt alleen gebruikt voor training en niet langer nodig zijn.
3. Voegt _Web service invoer_ en _uitvoer_ modules in de standaardlocaties bevinden in uw experiment (deze modules accepteren en retourneren gebruikersgegevens).

Het volgende experiment traint bijvoorbeeld een tweeklasse boosted decision tree model met voorbeeldgegevens telling:

![Trainingsexperiment][figure1]

De modules in dit experiment uitvoeren in feite vier verschillende functies:

![Module-functies][figure2]

Wanneer u deze trainingsexperiment naar een Voorspellend experiment converteren, sommige van deze modules zijn niet meer nodig hebt of ze nu een ander doel dienen:

* **Gegevens** -de gegevens in de voorbeeldgegevensset wordt niet gebruikt tijdens het scoring - de gebruiker van de webservice levert de gegevens die moeten worden beoordeeld. Echter, de metagegevens van deze gegevensset, zoals gegevenstypen, wordt gebruikt door het getrainde model. Daarom moet u de gegevensset in de Voorspellend experiment houden, zodat deze metagegevens kan bieden.

* **PREP** - afhankelijk van de gebruikersgegevens die worden verzonden voor het scoren, deze modules kunnen of mogelijk niet nodig om de binnenkomende gegevens te verwerken. De **webservice ingesteld** knop niet deze touch - moet u besluiten hoe u wilt deze te verwerken.
  
    Bijvoorbeeld: in dit voorbeeld de voorbeeldgegevensset mogelijk ontbrekende waarden, dus een [Clean Missing Data] [ clean-missing-data] module is opgenomen om op te lossen met hen. Daarnaast bevat de voorbeeldgegevensset van kolommen die niet nodig zijn voor het model te trainen. Dus een [Select Columns in Dataset] [ select-columns] module opgenomen moeten worden uitgesloten van deze extra kolommen van de gegevensstroom is. Als u weet dat de gegevens die worden verzonden via de webservice scores voor de ontbrekende waarden niet hebben, en vervolgens kunt u de [Clean Missing Data] [ clean-missing-data] module. Echter, omdat de [Select Columns in Dataset] [ select-columns] module helpt u bij het definiëren van de kolommen met gegevens die het getrainde model verwacht, die module moet blijven.

* **Trainen** -deze modules worden gebruikt voor het model te trainen. Wanneer u klikt op **webservice ingesteld**, deze modules zijn vervangen door een één-module die u getraind model bevat. Deze nieuwe module wordt opgeslagen in de **getrainde modellen** gedeelte van het modulepalet.

* **Score** : In dit voorbeeld wordt de [Split Data] [ split] module wordt gebruikt om de gegevensstroom in testgegevens en trainingsgegevens te verdelen. In de Voorspellend experiment we niet traint meer nodig, dus [Split Data] [ split] kan worden verwijderd. Op dezelfde manier de tweede [Score Model] [ score-model] module en de [Evaluate Model] [ evaluate-model] module worden gebruikt voor het resultaten uit de testgegevens zo vergelijken Deze modules zijn niet nodig in de Voorspellend experiment. De resterende [Score Model] [ score-model] -module, is echter een score resultaat via de web-service vereist.

Hier volgt hoe ons voorbeeld eruitziet nadat u hebt geklikt **webservice instellen**:

![Geconverteerde Voorspellend experiment][figure3]

Het werk dat door **webservice ingesteld** kan worden volstaan met het voorbereiden van uw experiment te worden geïmplementeerd als een webservice. U wilt echter enkele extra werk die specifiek zijn voor uw experiment doet.

### <a name="adjust-input-and-output-modules"></a>Invoer- en -modules aanpassen
In uw trainingsexperiment gebruikt van een set trainingsgegevens en vervolgens heeft enkele verwerking om de gegevens in een formulier dat de machine learning-algoritme die nodig zijn. Als de gegevens die u verwacht te ontvangen via de webservice niet deze verwerking hoeft, kunt u dit negeren: koppel de uitvoer van de **Web service invoer module** naar een andere module in uw experiment. U ontvangt nu gegevens van de gebruiker in het model op deze locatie.

Bijvoorbeeld standaard **webservice ingesteld** plaatst de **Web service invoer** module aan de bovenkant van de gegevensstroom, zoals wordt weergegeven in de bovenstaande afbeelding. Maar we handmatig kunt plaatsen de **Web service invoer** voorbij de gegevensverwerking-modules:

![Het verplaatsen van de web service-invoer][figure4]

De ingevoerde gegevens opgegeven via de webservice geeft nu rechtstreeks in de module Score Model zonder eventuele voorverwerking.

Op deze manier standaard **webservice ingesteld** plaatst de module Web service uitvoer onder aan de gegevensstroom. In dit voorbeeld wordt de webservice wordt geretourneerd naar de gebruiker de uitvoer van de [Score Model] [ score-model] module, deze de volledige invoergegevens vector plus de scoring resultaten bevat.
Echter, als u liever een ander resultaat, vervolgens kunt u toevoegen aanvullende modules voordat de **Web service uitvoer** module. 

Voeg bijvoorbeeld het volgende om terug te keren alleen de resultaten van de scoring- en niet de gehele vector van invoergegevens, een [Select Columns in Dataset] [ select-columns] module wilt uitsluiten van alle kolommen behalve de scoring resultaten. Verplaats de **Web service uitvoer** module aan de uitvoer van de [Select Columns in Dataset] [ select-columns] module. Het experiment ziet er als volgt:

![Het verplaatsen van de web service-uitvoer][figure5]

### <a name="add-or-remove-additional-data-processing-modules"></a>Toevoegen of verwijderen van extra gegevensverwerkende modules
Als er meer modules in uw experiment waarvan u weet niet meer nodig dat tijdens de score, kunnen deze worden verwijderd. Bijvoorbeeld, omdat we verplaatst de **Web service invoer** -module op een punt na de verwerking van data-modules, kunnen we verwijderen de [Clean Missing Data] [ clean-missing-data] module op basis van de Voorspellend experiment.

Onze Voorspellend experiment ziet er nu als volgt uit:

![Aanvullende module verwijderen][figure6]


### <a name="add-optional-web-service-parameters"></a>Toevoegen van optionele Parameters van de Web Service
In sommige gevallen kunt u toestaan dat de gebruiker van uw web-service het gedrag van modules wijzigen wanneer de service wordt geopend. *Web-Parameters van de Service* kunt u om dit te doen.

Een veelvoorkomend voorbeeld is het instellen van een [importgegevens] [ import-data] module, zodat de gebruiker van de geïmplementeerde webservice een andere gegevensbron opgeven kan wanneer de web-service wordt geopend. Of het configureren van een [gegevens exporteren] [ export-data] module zodat een andere bestemming kan worden opgegeven.

U kunt Web Service Parameters definiëren en deze koppelen aan een of meer moduleparameters en kunt u opgeven of ze verplicht of optioneel zijn. De gebruiker van de webservice bevat waarden voor deze parameters wanneer de service kan worden geopend en de acties van de module worden gewijzigd.

Zie voor meer informatie over wat Webserviceparameters zijn en het gebruik ervan [met behulp van Azure Machine Learning Web Service Parameters][webserviceparameters].

[webserviceparameters]: web-service-parameters.md


## <a name="deploy-the-predictive-experiment-as-a-web-service"></a>De Voorspellend experiment implementeren als een webservice
Nu dat de Voorspellend experiment voldoende is voorbereid, kunt u deze kunt implementeren als een Azure-web-service. Met behulp van de webservice, kunnen gebruikers gegevens verzenden naar uw model en het model de voorspellingen wordt geretourneerd.

Zie voor meer informatie over het proces van de volledige [een Azure Machine Learning-webservice implementeren][deploy]

[deploy]: publish-a-machine-learning-web-service.md


<!-- Images -->
[figure1]:./media/convert-training-experiment-to-scoring-experiment/figure1.png
[figure2]:./media/convert-training-experiment-to-scoring-experiment/figure2.png
[figure3]:./media/convert-training-experiment-to-scoring-experiment/figure3.png
[figure4]:./media/convert-training-experiment-to-scoring-experiment/figure4.png
[figure5]:./media/convert-training-experiment-to-scoring-experiment/figure5.png
[figure6]:./media/convert-training-experiment-to-scoring-experiment/figure6.png


<!-- Module References -->
[clean-missing-data]: https://msdn.microsoft.com/library/azure/d2c5ca2f-7323-41a3-9b7e-da917c99f0c4/
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[export-data]: https://msdn.microsoft.com/library/azure/7a391181-b6a7-4ad4-b82d-e419c0d6522c/
