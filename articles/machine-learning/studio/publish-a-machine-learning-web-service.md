---
title: Een Machine Learning-webservice implementeren | Microsoft Docs
description: Hoe een trainingsexperiment converteren naar een Voorspellend experiment, voorbereiden voor implementatie en klik vervolgens als een Azure Machine Learning-webservice implementeren.
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 73a3e9c6-00d0-41d4-8cf1-2ec87713867e
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/06/2017
ms.author: garye
ms.openlocfilehash: 75577ad318f2ff23a7b7d10cf551f3bced56fb62
ms.sourcegitcommit: 5a6e943718a8d2bc5babea3cd624c0557ab67bd5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2017
---
# <a name="deploy-an-azure-machine-learning-web-service"></a>Een Azure Machine Learning-webservice implementeren
Azure Machine Learning kunt u bouwen, testen en implementeren van voorspellende analytische oplossingen.

Dit is van een punt-van-overzichtsweergave gedaan in drie stappen:

* **[Maken van een trainingsexperiment]**  -Azure Machine Learning Studio is een gezamenlijke visual ontwikkelomgeving die u gebruikt om te trainen en testen van een predictive analytics-model met trainingsgegevens die u opgeeft.
* **[Converteren naar een Voorspellend experiment]**  -nadat uw model is getraind met bestaande gegevens en u kunt gebruiken voor het beoordelen van nieuwe gegevens, u voorbereidt en uw experiment voor voorspellingen stroomlijnen.
* **[Als een webservice implementeren]**  -u kunt uw Voorspellend experiment als een [nieuwe] of [klassieke] Azure-web-service. Gebruikers kunnen gegevens verzenden naar het model en ontvangen van uw model voorspellingen.

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="create-a-training-experiment"></a>Een trainingsexperiment maken
Als u wilt een predictive analytics-model trainen, u Azure Machine Learning Studio maakt een trainingsexperiment waar u diverse modules trainingsgegevens laden, het voorbereiden van de gegevens zo nodig, het toepassen van machine learning-algoritmen en het evalueren van de resultaten opnemen. U kunt een experiment herhalen en probeer andere machine learning-algoritmen om te vergelijken en evalueren van de resultaten.

Het proces van het maken en beheren van training experimenten wordt uitgebreid elders besproken. Zie voor meer informatie in deze artikelen:

* [Een eenvoudige experiment maken in Azure Machine Learning Studio](create-experiment.md)
* [Een voorspellende oplossing met Azure Machine Learning ontwikkelen](walkthrough-develop-predictive-solution.md)
* [Uw trainingsgegevens importeren in Azure Machine Learning Studio](import-data.md)
* [Experimentherhalingen in Azure Machine Learning Studio beheren](manage-experiment-iterations.md)

## <a name="convert-the-training-experiment-to-a-predictive-experiment"></a>Het trainingsexperiment converteren naar een Voorspellend experiment
Zodra u uw model te trainen hebt, kunt u uw trainingsexperiment converteren naar een Voorspellend experiment voor de beoordeling van nieuwe gegevens.

Door te converteren naar een Voorspellend experiment, krijgt u het getrainde model gereed om te worden geïmplementeerd als een score-webservice. Gebruikers van de webservice kunnen invoergegevens verzenden naar uw model en het model wordt teruggestuurd de resultaten van de voorspelling. Als u een Voorspellend experiment omzetten, houd er rekening mee hoe u het model moet worden gebruikt door anderen verwacht.

Als u wilt uw trainingsexperiment omzetten in een Voorspellend experiment, klikt u op **uitvoeren** aan de onderkant van het experimentcanvas, klikt u op **webservice ingesteld**, selecteert u vervolgens **voorspellende webservice**.

![Converteren naar het experiment score berekenen](./media/publish-a-machine-learning-web-service/figure-1.png)

Zie voor meer informatie over het uitvoeren van deze conversie [uw model voorbereiden voor implementatie in Azure Machine Learning Studio](convert-training-experiment-to-scoring-experiment.md).

De volgende stappen beschrijven een Voorspellend experiment als een nieuwe webservice implementeren. U kunt ook het experiment implementeren als webservice klassiek.

## <a name="deploy-it-as-a-web-service"></a>Als een webservice implementeren

U kunt de Voorspellend experiment implementeren als een nieuwe webservice of als een klassiek-webservice.

### <a name="deploy-the-predictive-experiment-as-a-new-web-service"></a>De Voorspellend experiment implementeren als een nieuwe webservice
Nu dat de Voorspellend experiment is voorbereid, kunt u deze kunt implementeren als een nieuwe Azure-web-service. Via de webservice, gebruikers kunnen gegevens verzenden naar uw model en het model wordt de voorspellingen geretourneerd.

Klik voor het implementeren van uw Voorspellend experiment **uitvoeren** aan de onderkant van het experimentcanvas. Wanneer het experiment is voltooid, klikt u op **webservice implementeren** en selecteer **webservice implementeren [nieuw]**.  De pagina implementatie van de Machine Learning-webservice-portal wordt geopend.

> [!NOTE] 
> Voor het implementeren van een nieuwe webservice moet u voldoende machtigingen hebben in het abonnement waaraan u de webservice implementeren. Zie voor meer informatie, [beheren van een webservice via de portal voor Azure Machine Learning-webservices](manage-new-webservice.md). 

#### <a name="machine-learning-web-service-portal-deploy-experiment-page"></a>Machine Learning-webservice portal implementeren Experiment pagina
Voer een naam voor de web-service op de pagina Experiment implementeren.
Een prijscategorie selecteren. Als u een bestaande prijsstelling dat kunt u dit hebt, moet anders u een nieuw plan prijs voor de service.

1. In de **prijs Plan** vervolgkeuzelijst, selecteer een bestaande planning of Selecteer de **Selecteer nieuw plan** optie.
2. In **naam**, typ een naam waarmee het plan op uw factuur wordt geïdentificeerd.
3. Selecteer een van de **maandelijks plannen lagen**. De standaardwaarde van de lagen abonnement op de plannen voor uw regio standaard en uw web-service wordt geïmplementeerd in deze regio.

Klik op **implementeren** en de **Quick Start** pagina voor uw webservice wordt geopend.

De pagina Quick Start webservice biedt u toegang en richtlijnen op de meest algemene taken die u uitvoeren wilt na het maken van een webservice. Hier kunt u eenvoudig toegang tot zowel de Test en verbruiken pagina.

<!-- ![Deploy the web service](./media/publish-a-machine-learning-web-service/figure-2.png)-->

#### <a name="test-your-new-web-service"></a>Uw nieuwe webservice testen
Als u wilt uw nieuwe webservice testen, klikt u op **webservice testen** onder algemene taken. U kunt uw webservice als een aanvraag en antwoord-Service (RR's) of een batchuitvoeringsservice (BES) testen op de pagina Test.

De testpagina RRS geeft de invoer, uitvoer en globale parameters die u hebt gedefinieerd voor het experiment. De webservice testen, kunt u handmatig Geef de juiste waarden voor de invoer of geef een door komma's gescheiden waarden (CSV) opgemaakt bestand met de testwaarden.

Als u wilt testen met RRS, uit de lijst met weergave-modus, geef de juiste waarden voor de invoer en klik op **testen aanvragen en antwoorden**. De voorspelling van de resultaten weergegeven in de uitvoerkolom naar links.

![De webservice implementeren](./media/publish-a-machine-learning-web-service/figure-5-test-request-response.png)

Als u wilt uw BES testen, klikt u op **Batch**. Klik op Bladeren onder uw invoer en selecteer een CSV-bestand met de juiste voorbeeldwaarden op de pagina van de test Batch. Als u een CSV-bestand niet hebt en u uw Voorspellend experiment met Machine Learning Studio hebt gemaakt, kunt u de gegevensset voor uw Voorspellend experiment downloaden en gebruiken.

Open de Machine Learning Studio voor het downloaden van de gegevensset. Open uw Voorspellend experiment en klikt u met de rechtermuisknop op de invoer voor uw experiment. Selecteer in het contextmenu **gegevensset** en selecteer vervolgens **downloaden**.

![De webservice implementeren](./media/publish-a-machine-learning-web-service/figure-7-mls-download.png)

Klik op **Test**. De status van uw taak Batchuitvoering weergegeven aan de rechterzijde onder **Test batchtaken**.

![De webservice implementeren](./media/publish-a-machine-learning-web-service/figure-6-test-batch-execution.png)

<!--![Test the web service](./media/publish-a-machine-learning-web-service/figure-3.png)-->

Op de **configuratie** pagina kunt u de beschrijving, de titel wijzigen, de opslagaccountsleutel bijwerken en voorbeeldgegevens inschakelen voor uw webservice.

![Configureer de webservice](./media/publish-a-machine-learning-web-service/figure-8-arm-configure.png)

Zodra u de web-service hebt geïmplementeerd, kunt u het volgende doen:

* **Toegang** deze via de API-webservice.
* **Beheren** deze via Azure Machine Learning web services-portal.
* **Update** deze als het model wordt gewijzigd.

#### <a name="access-your-new-web-service"></a>Toegang tot uw nieuwe webservice
Nadat u uw webservice van Machine Learning Studio implementeert, kunt u gegevens verzenden naar de service en antwoorden programmatisch ontvangen.

De **verbruiken** pagina vindt u alle informatie die u nodig hebt voor toegang tot de webservice. De API-sleutel wordt bijvoorbeeld geleverd aan geautoriseerde toegang tot de service.

Zie voor meer informatie over het openen van een Machine Learning-webservice [gebruiken van een Azure Machine Learning-webservice](consume-web-services.md).

#### <a name="manage-your-new-web-service"></a>Beheren van uw nieuwe webservice
U kunt uw nieuwe web services Machine Learning-webservices portal beheren. Van de [portal hoofdpagina](https://services.azureml-test.net/), klikt u op **webservices**. U kunt via de webpagina-services verwijderen of kopiëren van een service. Klik op de service voor het bewaken van een bepaalde service, en klik vervolgens op **Dashboard**. Voor het bewaken van batchtaken die zijn gekoppeld met de webservice, klikt u op **Batch aanvragen logboek**.

### <a name="deploy-the-predictive-experiment-as-a-classic-web-service"></a>De Voorspellend experiment implementeren als een klassiek-webservice

Nu dat de Voorspellend experiment is voldoende voorbereid, kunt u deze kunt implementeren als een klassieke Azure-webservice. Via de webservice, gebruikers kunnen gegevens verzenden naar uw model en het model wordt de voorspellingen geretourneerd.

Klik voor het implementeren van uw Voorspellend experiment **uitvoeren** aan de onderkant van het experiment canvas en klik vervolgens op **webservice implementeren**. De webservice is ingesteld en u in het dashboard van web service worden geplaatst.

![De webservice implementeren](./media/publish-a-machine-learning-web-service/figure-2.png)

#### <a name="test-your-classic-web-service"></a>Test uw webservice klassiek

U kunt de webservice testen in de portal van de Machine Learning-webservices of de Machine Learning Studio.

Als u wilt testen de Request Response-webservice, klikt u op de **testen** knop in het dashboard van web service. Een dialoogvenster weergegeven om te vragen u de invoergegevens van de service. Dit zijn de kolommen die door het scoreprofiel experiment wordt verwacht. Geef een set gegevens en klik op **OK**. De resultaten die zijn gegenereerd door de webservice worden weergegeven aan de onderkant van het dashboard.

U kunt klikken op de **testen** preview-koppeling voor het testen van uw service in de portal voor Azure Machine Learning-webservices, zoals eerder in de sectie nieuwe web-service.

Als u wilt testen van de Batch-Service kan worden uitgevoerd, klikt u op **testen** link bekijken. Klik op Bladeren onder uw invoer en selecteer een CSV-bestand met de juiste voorbeeldwaarden op de pagina van de test Batch. Als u een CSV-bestand niet hebt en u uw Voorspellend experiment met Machine Learning Studio hebt gemaakt, kunt u de gegevensset voor uw Voorspellend experiment downloaden en gebruiken.

![De webservice testen](./media/publish-a-machine-learning-web-service/figure-3.png)

Op de **configuratie** pagina kunt u de weergavenaam van de service wijzigen en een omschrijving. De naam en beschrijving wordt weergegeven in de [Azure-portal](https://portal.azure.com/) waar u uw webservices beheren.

U kunt een beschrijving opgeven voor uw invoergegevens, uitvoergegevens en web parameters van de service door een tekenreeks opgeven voor elke kolom onder **INVOERSCHEMA**, **UITVOERSCHEMA**, en **Web SERVICE PARAMETER**. Deze beschrijvingen worden gebruikt in de steekproef code-documentatie voor de webservice.

Logboekregistratie voor het vaststellen van fouten die u ziet, wanneer uw webservice wordt geopend, kunt u inschakelen. Zie voor meer informatie [logboekregistratie inschakelen voor Machine Learning-webservices](web-services-logging.md).

![Configureer de webservice](./media/publish-a-machine-learning-web-service/figure-4.png)

U kunt ook de eindpunten voor de webservice configureren in de portal voor Azure Machine Learning-webservices die vergelijkbaar is met de procedure eerder in de sectie nieuwe web-service. Zijn er andere opties, u kunt toevoegen of wijzigen van de beschrijving van de service, logboekregistratie inschakelen en de voorbeeldgegevens inschakelen voor het testen.

#### <a name="access-your-classic-web-service"></a>Toegang tot uw webservice klassiek
Nadat u uw webservice van Machine Learning Studio implementeert, kunt u gegevens verzenden naar de service en antwoorden programmatisch ontvangen.

Het dashboard bevat alle informatie die u nodig hebt voor toegang tot de webservice. Bijvoorbeeld: de API-sleutel voor geautoriseerde toegang tot de service wordt aangeboden en API help-pagina's zijn bedoeld om u aan de slag uw code te schrijven.

Zie voor meer informatie over het openen van een Machine Learning-webservice [gebruiken van een Azure Machine Learning-webservice](consume-web-services.md).

#### <a name="manage-your-classic-web-service"></a>Uw webservice klassieke beheren
Er zijn verschillende acties die u kunt uitvoeren voor het bewaken van een webservice. U kunt bijwerken en verwijderen. U kunt ook extra eindpunten toevoegen met een webservice klassieke naast het standaardeindpunt dat wordt gemaakt wanneer u deze implementeert.

Zie voor meer informatie [beheren van een Azure Machine Learning-werkruimte](manage-workspace.md) en [beheren van een webservice via de portal voor Azure Machine Learning-webservices](manage-new-webservice.md).

<!-- When this article gets published, fix the link and uncomment
For more information on how to manage Azure Machine Learning web service endpoints using the REST API, see **Azure machine learning web service endpoints**.
-->

## <a name="update-the-web-service"></a>De webservice bijwerken
U kunt wijzigingen aanbrengen in uw web-service, zoals het bijwerken van het model met extra trainingsgegevens, en implementeert u deze opnieuw, wordt de oorspronkelijke webservice overschreven.

Voor het bijwerken van de webservice, opent u de oorspronkelijke Voorspellend experiment dat u gebruikt voor het implementeren van de webservice en een bewerkbare kopie maken door te klikken op **SAVE AS**. Breng uw wijzigingen en klik vervolgens op **webservice implementeren**.

Omdat u dit experiment voordat hebt geïmplementeerd, wordt u gevraagd of u wilt overschrijven (klassieke webservice) of bij te werken (nieuwe webservice) van de bestaande service. Te klikken op **Ja** of **Update** de bestaande web-service gestopt en implementeert de nieuwe Voorspellend experiment in plaats daarvan wordt geïmplementeerd.

> [!NOTE]
> Als u wijzigingen in de configuratie hebt aangebracht in de oorspronkelijke webservice, bijvoorbeeld moet invoeren van een nieuwe weergavenaam of beschrijving, u deze waarden opnieuw invoeren.
> 
> 

Een optie voor het bijwerken van uw web-service is voor het model programmatisch opnieuw trainen. Zie [Machine Learning-modellen programmatisch opnieuw trainen](retrain-models-programmatically.md) voor meer informatie.

<!-- internal links -->
[Maken van een trainingsexperiment]: #create-a-training-experiment
[Converteren naar een Voorspellend experiment]: #convert-the-training-experiment-to-a-predictive-experiment
[Als een webservice implementeren]: #deploy-it-as-a-web-service
[nieuwe]: #deploy-the-predictive-experiment-as-a-new-Web-service
[klassieke]: #deploy-the-predictive-experiment-as-a-new-Web-service
[Access]: #access-the-Web-service
[Manage]: #manage-the-Web-service-in-the-azure-management-portal
[Update]: #update-the-Web-service
