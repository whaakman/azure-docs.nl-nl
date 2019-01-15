---
Titel: Application lifecycle management titleSuffix: Azure Machine Learning Studio description: Toepassing van aanbevolen procedures voor Application Lifecycle Management in Azure Machine Learning Studio-services: machine learning ms.service: machine learning ms.component: studio ms.topic: artikel

author: ericlicoding ms.author: amlstudiodocs ms.custom: previous-ms.author=haining, previous-author=hning86 ms.date: 10/27/2016
---
# <a name="application-lifecycle-management-in-azure-machine-learning-studio"></a>Application Lifecycle Management in Azure Machine Learning Studio
Azure Machine Learning Studio is een hulpprogramma voor het ontwikkelen van machine learning-experimenten die zijn geoperationaliseerd in het Azure-cloud-platform. Het is, zoals de Visual Studio IDE en schaalbare cloudservice samengevoegd in één platform. U kunt op verschillende activa implementatie en uitvoering van geautomatiseerde standaardprocedures Application Lifecycle Management (ALM) van versiebeheer opnemen in Azure Machine Learning Studio. In dit artikel komen enkele van de opties en benaderingen.

## <a name="versioning-experiment"></a>Versiebeheer experiment
Er zijn twee aanbevolen manieren naar versie van de experimenten nodig hebben. U kunt vertrouwen op de ingebouwde uitvoeringsgeschiedenis of exporteren van het experiment in een JSON-indeling zodat deze extern beheren. Elke benadering wordt geleverd met de voor- en nadelen.

### <a name="experiment-snapshots-using-run-history"></a>Experiment momentopnamen met behulp van de geschiedenis uitvoeren
In het uitvoeringsmodel van de Azure Machine Learning Studio learning-experiment, een onveranderbare een momentopname van het experiment naar de Taakplanner wordt verzonden wanneer u klikt op **uitvoeren** in de editor experiment. Deze lijst van momentopnamen wilt weergeven, klikt u op **Run History** op de opdrachtbalk in de lijstweergave met experimenten-editor.

![Knop Geschiedenis uitvoeren](./media/version-control/runhistory.png)

U kunt vervolgens open de momentopname in de modus vergrendeld door te klikken op de naam van het experiment op het moment dat het experiment is ingediend bij het uitvoeren en de momentopname werd gemaakt. Merk op dat alleen het eerste item in de lijst, die staat voor de huidige experiment, in een status worden bewerkt. Ook ziet u dat elke momentopname, kan zich in de Status van de verschillende statussen, met inbegrip van voltooide (gedeeltelijk uitvoeren), is mislukt, is mislukt (gedeeltelijk uitvoeren), of concept.

![Lijst met de geschiedenis uitvoeren](./media/version-control/runhistorylist.png)

Nadat deze geopend, kunt u het experiment momentopname opslaan als een nieuw experiment en vervolgens te wijzigen. Als de momentopname van uw experiment items, zoals getrainde modellen, transformaties en gegevenssets die versie hebt bijgewerkt bevat, behoudt de momentopname van de verwijzingen naar de oorspronkelijke versie wanneer de momentopname werd gemaakt. Als u de momentopname van het vergrendelde opslaan als een nieuw experiment, Azure Machine Learning Studio detecteert het bestaan van een nieuwere versie van deze middelen en automatisch bijgewerkt in de nieuwe experiment.

Als u het experiment verwijdert, worden alle momentopnamen van dit experiment verwijderd.

### <a name="exportimport-experiment-in-json-format"></a>Exporteren/importeren experiment in JSON-indeling
De uitvoeringsgeschiedenis momentopnamen Houd een onveranderbare versie van het experiment in Azure Machine Learning Studio telkens wanneer deze wordt verzonden om uit te voeren. U kunt ook een lokale kopie van het experiment opslaan en inchecken in uw favoriete bronbeheersysteem, zoals Team Foundation Server, en een experiment dat lokaal bestand later opnieuw maken. U kunt de [Azure Machine Learning PowerShell](https://aka.ms/amlps) commandlets [ *Export-AmlExperimentGraph* ](https://github.com/hning86/azuremlps#export-amlexperimentgraph) en [  *Import-AmlExperimentGraph* ](https://github.com/hning86/azuremlps#import-amlexperimentgraph) Agent installeert.

Het JSON-bestand is een tekstuele weergave genereren van de experimentgrafiek, waaronder een verwijzing naar assets in de werkruimte, zoals een gegevensset of een getraind model mogelijk. Deze bevat geen een geserialiseerde versie van de asset. Als u probeert te importeren van het JSON-document terug in de werkruimte, wordt de waarnaar wordt verwezen, assets moeten al bestaan met de dezelfde asset-id's waarnaar wordt verwezen in het experiment. U geen anders toegang tot het geïmporteerde experiment.

## <a name="versioning-trained-model"></a>Het getrainde model versiebeheer
Een getraind model in Azure Machine Learning is geserialiseerd in een indeling die bekend staat als een iLearner-bestand (`.iLearner`), en wordt opgeslagen in de Azure Blob storage-account dat is gekoppeld aan de werkruimte. Er is één manier om een kopie van het iLearner-bestand via de retraining API. [In dit artikel](retrain-models-programmatically.md) wordt uitgelegd hoe de retraining API gebruikt. De stappen op hoog niveau:

1. Instellen van uw trainingsexperiment.
2. De uitvoerpoort van een web-service toevoegen aan de module Train Model of de module die het getrainde model, zoals Model Hyperparameter af te stemmen of R-Model maken produceert.
3. Uw trainingsexperiment uitvoeren en vervolgens implementeren als een webservice van de model-training.
4. De BES-eindpunt van de training-webservice aanroepen en geef de naam van de gewenste iLearner-bestand en Blob storage-accountlocatie waar deze worden opgeslagen.
5. Verzamelt de geproduceerde iLearner-bestand nadat de BES-aanroep is voltooid.

Een andere manier om op te halen van het iLearner-bestand is via de PowerShell-commandlet [ *downloaden AmlExperimentNodeOutput*](https://github.com/hning86/azuremlps#download-amlexperimentnodeoutput). Dit is mogelijk beter als u een kopie van het iLearner-bestand zonder de noodzaak wilt voor het model programmatisch opnieuw trainen.

Nadat u het iLearner-bestand met het getrainde model hebt, kunt u uw eigen strategie voor versiebeheer vervolgens gebruiken. De strategie kan net zo eenvoudig als een vooraf/postfix als een naamgevingsconventie toepassen en laat het iLearner-bestand in Blob-opslag, of kopiëren van/importeren in uw versiebeheersysteem zijn.

Het opgeslagen iLearner-bestand kan vervolgens worden gebruikt voor het scoren via geïmplementeerde webservices.

## <a name="versioning-web-service"></a>Versiebeheer-webservice
U kunt twee soorten webservices uit een Azure Machine Learning-experiment implementeren. De klassieke webservice is nauw gekoppeld met het experiment, evenals de werkruimte. De nieuwe webservice gebruikmaakt van het Azure Resource Manager-framework, en deze niet meer is gekoppeld aan het oorspronkelijke experiment of de werkruimte.

### <a name="classic-web-service"></a>Klassieke webservice
Naar een klassieke webservice-versie, kunt u profiteren van web service-eindpunt om voor te bereiden. Hier volgt een typische stroom:

1. U implementeert een nieuwe klassieke webservice, waarin een eindpunt uit uw Voorspellend experiment.
2. U maakt een nieuw eindpunt met de naam ep2, waarmee wordt aangegeven dat de huidige versie van het experiment/getraind model.
3. U gaat u terug en bijwerken van uw Voorspellend experiment en het getrainde model.
4. U opnieuw de Voorspellend experiment, klikt u vervolgens het standaardeindpunt update implementeren. Maar dit heeft geen invloed op ep2.
5. U maakt een extra eindpunt met de naam ep3, waarmee wordt aangegeven dat de nieuwe versie van het experiment en het getrainde model.
6. Ga terug naar stap 3 indien nodig.

Na verloop van tijd mogelijk veel gemaakt in dezelfde webservice-eindpunten. Elk eindpunt vertegenwoordigt een point-in-time-kopie van het experiment met de punt-in-time-versie van het getrainde model. Vervolgens kunt u externe logica om te bepalen welk eindpunt aan te roepen, waardoor effectief een versie van het getrainde model voor de score selecteren.

U kunt ook veel identieke web service-eindpunten te maken en vervolgens vullen van de verschillende versies van het iLearner-bestand naar het eindpunt om vergelijkbare effect te bereiken. [In dit artikel](create-models-and-endpoints-with-powershell.md) wordt in meer detail wordt uitgelegd hoe u Agent installeert.

### <a name="new-web-service"></a>Nieuwe webservice
Als u een nieuwe Azure Resource Manager gebaseerde webservice maakt, is om voor te bereiden eindpunt niet meer beschikbaar. In plaats daarvan kunt u web service definition (WSD)-bestanden, in JSON-indeling, genereren van uw Voorspellend experiment met behulp van de [Export-AmlWebServiceDefinitionFromExperiment](https://github.com/hning86/azuremlps#export-amlwebservicedefinitionfromexperiment) PowerShell-commandlet of met behulp van de [ *Export-AzureRmMlWebservice* ](https://docs.microsoft.com/powershell/module/azurerm.machinelearning/export-azurermmlwebservice?view=azurermps-6.6.0) PowerShell-commandlet uit een geïmplementeerde Resource Manager gebaseerde webservice.

Nadat u het geëxporteerde WSD-bestand en besturing van het versie hebt, kunt u ook de WSD als een nieuwe webservice implementeren in een andere webservice-abonnement in een andere Azure-regio. Zorg ervoor dat u opgeeft de configuratie van het juiste storage-account, evenals de nieuwe web service-plan-ID. Om aan te vullen in verschillende iLearner-bestanden, kunt u het WSD-bestand te wijzigen en bijwerken van de referentie van de locatie van het getrainde model en als een nieuwe webservice implementeren.

## <a name="automate-experiment-execution-and-deployment"></a>De uitvoering van het experiment en implementatie automatiseren
Een belangrijk aspect van ALM is het mogelijk om de uitvoering en het implementatieproces van de toepassing te automatiseren. In Azure Machine Learning, u kunt dit doen met behulp van de [PowerShell-module](https://aka.ms/amlps). Hier volgt een voorbeeld van end-to-end-stappen die relevant voor een standaard ALM zijn uitvoering/implementatie geautomatiseerd met behulp van de [Azure Machine Learning Studio PowerShell-module](https://aka.ms/amlps). Elke stap is gekoppeld aan een of meer PowerShell-commandlets die u gebruiken kunt om uit te voeren van deze stap.

1. [Uploaden van een gegevensset](https://github.com/hning86/azuremlps#upload-amldataset).
2. Een opleidingsexperiment kopiëren naar de werkruimte van een [werkruimte](https://github.com/hning86/azuremlps#copy-amlexperiment) of [galerie](https://github.com/hning86/azuremlps#copy-amlexperimentfromgallery), of [importeren](https://github.com/hning86/azuremlps#import-amlexperimentgraph) een [geëxporteerd](https://github.com/hning86/azuremlps#export-amlexperimentgraph) experiment vanuit lokale opslag schijf.
3. [Bijwerken van de gegevensset](https://github.com/hning86/azuremlps#update-amlexperimentuserasset) in het trainingsexperiment.
4. [Het trainingsexperiment uitvoeren](https://github.com/hning86/azuremlps#start-amlexperiment).
5. [Niveau verhogen van het getrainde model](https://github.com/hning86/azuremlps#promote-amltrainedmodel).
6. [Kopiëren van een Voorspellend experiment](https://github.com/hning86/azuremlps#copy-amlexperiment) in de werkruimte.
7. [Bijwerken van het getrainde model](https://github.com/hning86/azuremlps#update-amlexperimentuserasset) in de Voorspellend experiment.
8. [De voorspellende experiment uit te voeren](https://github.com/hning86/azuremlps#start-amlexperiment).
9. [Een webservice implementeren](https://github.com/hning86/azuremlps#new-amlwebservice) uit de Voorspellend experiment.
10. Test de webservice [RRS](https://github.com/hning86/azuremlps#invoke-amlwebservicerrsendpoint) of [BES](https://github.com/hning86/azuremlps#invoke-amlwebservicebesendpoint) eindpunt.

## <a name="next-steps"></a>Volgende stappen
* Download de [Azure Machine Learning Studio PowerShell](https://aka.ms/amlps) -module en begin om uw ALM-taken te automatiseren.
* Meer informatie over het [maken en beheren van groot aantal ML-modellen met behulp van slechts één experiment](create-models-and-endpoints-with-powershell.md) via PowerShell en de retraining API.
* Meer informatie over [implementeren van Azure Machine Learning-webservices](publish-a-machine-learning-web-service.md).
