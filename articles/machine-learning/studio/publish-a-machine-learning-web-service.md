---
title: Een Machine Learning Studio-webservice - Azure implementeren | Microsoft Docs
description: Hoe u een opleidingsexperiment converteren naar een Voorspellend experiment, voorbereiding op deze implementatie vervolgens als een Azure Machine Learning Studio-webservice implementeren.
services: machine-learning
documentationcenter: ''
author: ericlicoding
ms.custom: previous-ms.author=yahajiza, previous-author=YasinMSFT
ms.author: amlstudiodocs
editor: cgronlun
ms.assetid: 73a3e9c6-00d0-41d4-8cf1-2ec87713867e
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/06/2017
ms.openlocfilehash: 71315b6c36a9e41e5805d5a15bde5b1d1d84f2b5
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53269920"
---
# <a name="deploy-an-azure-machine-learning-studio-web-service"></a>Een Azure Machine Learning Studio-webservice implementeren
Azure Machine Learning kunt u bouwen, testen en implementeren van voorspellende analytische oplossingen.

Dit is gedaan in op hoog niveau point-van-weergave, in drie stappen:

* **[Een opleidingsexperiment maken]**  -Azure Machine Learning Studio is een samenwerkingsverband visual ontwikkelomgeving die u gebruiken om te trainen en testen van een predictive analytics-model met behulp van de trainingsgegevens die u opgeeft.
* **[Converteren naar een Voorspellend experiment]**  -zodra uw model is getraind met bestaande gegevens en u bent klaar om te gebruiken voor het scoren van nieuwe gegevens, u voorbereidt en stroomlijnen van uw experiment voor voorspellingen.
* **[Als een webservice implementeren]**  -u kunt uw Voorspellend experiment implementeren als een [Nieuw] of [Klassiek] Azure-web-service. Gebruikers kunnen gegevens aan uw model verzenden en ontvangen van het model voorspellingen.



## <a name="create-a-training-experiment"></a>Een opleidingsexperiment maken
Als u wilt een Voorspellend model te trainen, kunt u Azure Machine Learning Studio maakt een opleidingsexperiment neemt u diverse modules trainingsgegevens laden, voorbereiden van de gegevens zo nodig, toepassen van machine learning-algoritmen en evalueren van de resultaten. U kunt een experiment herhalen en andere machine learning-algoritmen om te vergelijken en de resultaten evalueren proberen.

Het proces voor het maken en beheren van training experimenten wordt uitgebreid elders beschreven. Raadpleeg voor meer informatie de volgende artikelen:

* [Een eenvoudig experiment maken in Azure Machine Learning Studio](create-experiment.md)
* [Een voorspellende oplossing met Azure Machine Learning ontwikkelen](walkthrough-develop-predictive-solution.md)
* [Uw trainingsgegevens importeren in Azure Machine Learning Studio](import-data.md)
* [Experimentherhalingen in Azure Machine Learning Studio beheren](manage-experiment-iterations.md)

## <a name="convert-the-training-experiment-to-a-predictive-experiment"></a>Het trainingsexperiment converteren naar een Voorspellend experiment
Nadat u uw model hebt getraind, u kunt uw trainingsexperiment converteren naar een Voorspellend experiment te scoren van nieuwe gegevens.

Door te converteren naar een Voorspellend experiment, bent u het getrainde model kan worden geïmplementeerd als een scoringwebservice ophalen. Gebruikers van de webservice kunnen invoergegevens verzenden aan uw model en het model wordt terugsturen van de voorspellingsresultaten. Als u een Voorspellend experiment omzetten, houd er rekening mee hoe u uw model moet worden gebruikt door anderen verwacht.

Als u wilt uw trainingsexperiment converteren naar een Voorspellend experiment, klikt u op **uitvoeren** aan de onderkant van het experimentcanvas, klikt u op **webservice ingesteld**en selecteer vervolgens **voorspellende webservice**.

![Converteren naar score experiment](./media/publish-a-machine-learning-web-service/figure-1.png)

Zie voor meer informatie over het uitvoeren van deze conversie een hoeveelheid [over het voorbereiden van uw model voor implementatie in Azure Machine Learning Studio](convert-training-experiment-to-scoring-experiment.md).

De volgende stappen beschrijven een Voorspellend experiment als een nieuwe webservice implementeren. U kunt ook het experiment implementeren als een klassieke webservice.

## <a name="deploy-it-as-a-web-service"></a>Als een webservice implementeren

U kunt de Voorspellend experiment implementeren als een nieuwe webservice of als een klassieke webservice.

### <a name="deploy-the-predictive-experiment-as-a-new-web-service"></a>De Voorspellend experiment implementeren als een nieuwe webservice
Nu dat de Voorspellend experiment is voorbereid, kunt u deze kunt implementeren als een nieuwe Azure-web-service. Met behulp van de webservice, kunnen gebruikers gegevens verzenden naar uw model en het model de voorspellingen wordt geretourneerd.

Als u wilt uw Voorspellend experiment implementeren, klikt u op **uitvoeren** aan de onderkant van het experimentcanvas. Wanneer het experiment is voltooid, klikt u op **webservice implementeren** en selecteer **webservice implementeren [nieuw]**.  De implementatiepagina van de portal voor Machine Learning-webservice wordt geopend.

> [!NOTE] 
> Voor het implementeren van een nieuwe webservice moet u voldoende machtigingen hebben in het abonnement waarvoor u de webservice implementeert. Zie voor meer informatie, [beheren van een webservice met behulp van de Azure Machine Learning-webserviceportal](manage-new-webservice.md). 

#### <a name="machine-learning-web-service-portal-deploy-experiment-page"></a>Machine Learning-webservice portal implementeren Experiment pagina
Voer een naam voor de webservice op de pagina Experiment implementeren.
Een prijscategorie selecteren. Hebt u een bestaande prijsstelling dat kunt u dit selecteren, moet anders u een nieuwe prijs plan maken voor de service.

1. In de **Prijsplan** vervolgkeuzelijst, selecteert u een bestaand plan of de **Selecteer nieuw plan** optie.
2. In **Plannaam**, typ een naam waarmee het abonnement op uw factuur wordt geïdentificeerd.
3. Selecteer een van de **maandelijkse Plan lagen**. De standaardwaarde van de lagen plan voor de abonnementen voor uw standaardregio en wordt uw webservice wordt geïmplementeerd voor deze regio.

Klik op **implementeren** en de **snelstartgids** pagina voor uw webservice wordt geopend.

De webpagina van de service-Quick Start hebt u toegang en richtlijnen op de meest algemene taken die u uitvoeren wilt na het maken van een webservice. Hier kunt u eenvoudig de testpagina en de verbruikspagina openen.

<!-- ![Deploy the web service](./media/publish-a-machine-learning-web-service/figure-2.png)-->

#### <a name="test-your-new-web-service"></a>Uw nieuwe webservice testen
Als u wilt testen op uw nieuwe webservice, klikt u op **webservice testen** onder algemene taken. Op de pagina van testen, kunt u uw web-service als een Request Response Service (RRS) of een batchuitvoeringsservice (BES) testen.

De RRS-test-pagina wordt weergegeven voor de invoer, uitvoer en alle globale parameters die u hebt gedefinieerd voor het experiment. Als u wilt testen van de webservice, kunt u handmatig Geef de juiste waarden voor de invoer of een door komma's gescheiden waarden (CSV) opgemaakt bestand met de testwaarden opgeven.

Als u wilt testen met behulp van RRS, uit de lijst met weergave-modus, geef de juiste waarden voor de invoer en klikt u op **testen Aanvraagantwoord**. De voorspellingsresultaten op die worden weergegeven in de uitvoerkolom aan de linkerkant.

![De webservice implementeren](./media/publish-a-machine-learning-web-service/figure-5-test-request-response.png)

Als u wilt uw BES testen, klikt u op **Batch**. Klik op Bladeren onder uw invoer en selecteer een CSV-bestand met de juiste voorbeeldwaarden op de pagina van de test Batch. Als u een CSV-bestand hoeft, en u uw Voorspellend experiment met behulp van Machine Learning Studio hebt gemaakt, kunt u de gegevensset voor uw Voorspellend experiment downloaden en gebruiken.

Voor het downloaden van de gegevensset, opent u Machine Learning Studio. Open uw Voorspellend experiment en klikt u met de rechtermuisknop op de invoer voor uw experiment. Selecteer in het contextmenu **gegevensset** en selecteer vervolgens **downloaden**.

![De webservice implementeren](./media/publish-a-machine-learning-web-service/figure-7-mls-download.png)

Klik op **Test**. De status van de uitvoering van de Batch-taak wordt weergegeven aan de rechterkant onder **Test batchtaken**.

![De webservice implementeren](./media/publish-a-machine-learning-web-service/figure-6-test-batch-execution.png)

<!--![Test the web service](./media/publish-a-machine-learning-web-service/figure-3.png)-->

Op de **configuratie** pagina, kunt u de beschrijving, de titel wijzigen, de opslagaccountsleutel bijwerken en voorbeeldgegevens inschakelen voor uw webservice.

![Configureer de webservice](./media/publish-a-machine-learning-web-service/figure-8-arm-configure.png)

Nadat u de web-service hebt geïmplementeerd, kunt u het volgende doen:

* **Toegang tot** deze via de webservice-API.
* **Beheren** deze via Azure Machine Learning-services-webportal.
* **Update** het als uw model wordt gewijzigd.

#### <a name="access-your-new-web-service"></a>Toegang tot uw nieuwe webservice
Nadat u uw webservice van Machine Learning Studio implementeert, kunt u gegevens verzenden naar de service en antwoorden ontvangen via een programma.

De **verbruiken** pagina vindt u alle informatie die u nodig hebt voor toegang tot uw webservice. Bijvoorbeeld, is de API-sleutel opgegeven waarmee geautoriseerde toegang tot de service.

Zie voor meer informatie over het openen van een Machine Learning-webservice [hoe u een Azure Machine Learning-webservice gebruiken](consume-web-services.md).

#### <a name="manage-your-new-web-service"></a>Uw nieuwe webservice beheren
U kunt uw nieuwe web services Machine Learning-webservices-portal beheren. Uit de [portal hoofdpagina](https://services.azureml-test.net/), klikt u op **webservices**. U kunt verwijderen of kopiëren van een service van de webpagina van services. Klik op de service voor het controleren van een bepaalde service, en klik vervolgens op **Dashboard**. Voor het controleren van batch-taken die zijn gekoppeld aan de webservice, klikt u op **logboek voor Batch aanvraag**.

### <a name="deploy-the-predictive-experiment-as-a-classic-web-service"></a>De Voorspellend experiment implementeren als een klassieke webservice

Nu dat de Voorspellend experiment voldoende is voorbereid, kunt u deze kunt implementeren als een klassieke Azure-webservice. Met behulp van de webservice, kunnen gebruikers gegevens verzenden naar uw model en het model de voorspellingen wordt geretourneerd.

Als u wilt uw Voorspellend experiment implementeren, klikt u op **uitvoeren** canvas aan de onderkant van het experiment en klik vervolgens op **webservice implementeren**. De webservice is ingesteld en u in het dashboard van de webservice worden geplaatst.

![De webservice implementeren](./media/publish-a-machine-learning-web-service/figure-2.png)

#### <a name="test-your-classic-web-service"></a>Uw klassieke webservice testen

U kunt de webservice testen in de portal van Machine Learning-webservices of Machine Learning Studio.

Als u wilt testen de Request Response-webservice, klikt u op de **testen** knop in het dashboard van de webservice. Een dialoogvenster wordt weergegeven gebruiken om u te vragen voor de invoergegevens voor de service. Dit zijn de kolommen die door het scoring experiment wordt verwacht. Voer een set gegevens en klik vervolgens op **OK**. De resultaten die worden gegenereerd door de webservice worden weergegeven aan de onderkant van het dashboard.

U kunt klikken op de **testen** preview koppeling naar uw service testen in de portal voor Azure Machine Learning-webservices, zoals eerder in de sectie nieuwe web-service.

Als u wilt testen van de Batch-Service kan worden uitgevoerd, klikt u op **testen** link bekijken. Klik op Bladeren onder uw invoer en selecteer een CSV-bestand met de juiste voorbeeldwaarden op de pagina van de test Batch. Als u een CSV-bestand hoeft, en u uw Voorspellend experiment met behulp van Machine Learning Studio hebt gemaakt, kunt u de gegevensset voor uw Voorspellend experiment downloaden en gebruiken.

![De webservice testen](./media/publish-a-machine-learning-web-service/figure-3.png)

Op de **configuratie** pagina, kunt u de weergavenaam van de service wijzigen en wijs hieraan een beschrijving. De naam en beschrijving wordt weergegeven in de [Azure-portal](https://portal.azure.com/) waar u uw webservices beheren.

U kunt een beschrijving voor uw invoergegevens, de uitvoergegevens en het web serviceparameters opgeven door te voeren van een tekenreeks voor elke kolom onder **INVOERSCHEMA**, **UITVOERSCHEMA**, en **-WEBSERVICE PARAMETER**. Deze beschrijvingen worden gebruikt in het voorbeeld van code-documentatie voor de webservice.

U kunt logboekregistratie inschakelen om opsporen van fouten die u ziet wanneer uw webservice wordt geopend. Zie voor meer informatie, [logboekregistratie inschakelen voor Machine Learning-webservices](web-services-logging.md).

![Configureer de webservice](./media/publish-a-machine-learning-web-service/figure-4.png)

U kunt ook de eindpunten voor de webservice configureren in de portal voor Azure Machine Learning-webservices die vergelijkbaar is met de procedure eerder weergegeven in de sectie nieuwe web-service. Zijn er andere opties, kunt u toevoegen of wijzigen van de beschrijving van de service, logboekregistratie inschakelen en de voorbeeldgegevens inschakelen voor het testen.

#### <a name="access-your-classic-web-service"></a>Toegang tot uw klassieke webservice
Nadat u uw webservice van Machine Learning Studio implementeert, kunt u gegevens verzenden naar de service en antwoorden ontvangen via een programma.

Het dashboard bevat alle informatie die u nodig hebt voor toegang tot uw webservice. Bijvoorbeeld, de API-sleutel waarmee geautoriseerde toegang tot de service wordt geleverd, en API help-pagina's zijn bedoeld om u aan de slag uw code te schrijven.

Zie voor meer informatie over het openen van een Machine Learning-webservice [hoe u een Azure Machine Learning-webservice gebruiken](consume-web-services.md).

#### <a name="manage-your-classic-web-service"></a>Uw klassieke webservice beheren
Er zijn verschillende acties die u kunt uitvoeren voor het bewaken van een webservice. U kunt bijwerken en verwijderen. U kunt ook extra eindpunten toevoegen aan een klassieke webservice naast de standaardeindpunt dat wordt gemaakt wanneer u deze implementeren.

Zie voor meer informatie, [een Azure Machine Learning-werkruimte beheren](manage-workspace.md) en [beheren van een webservice met behulp van de Azure Machine Learning-webserviceportal](manage-new-webservice.md).

<!-- When this article gets published, fix the link and uncomment
For more information on how to manage Azure Machine Learning web service endpoints using the REST API, see **Azure machine learning web service endpoints**.
-->

## <a name="update-the-web-service"></a>Bijwerken van de webservice
U kunt wijzigingen aanbrengen in uw webservice, zoals het bijwerken van het model met aanvullende gegevens, en implementeert, overschrijven de oorspronkelijke web-service.

Voor het bijwerken van de webservice, opent u de oorspronkelijke Voorspellend experiment dat u gebruikt voor het implementeren van de webservice en een bewerkbare kopie maken door te klikken op **SAVE AS**. Breng uw wijzigingen en klik vervolgens op **webservice implementeren**.

Omdat u dit experiment voordat hebt geïmplementeerd, kunt u wordt gevraagd of u wilt overschrijven (klassieke webservice) of de bestaande service (nieuwe webservice) bijwerken. Te klikken op **Ja** of **Update** Hiermee stopt u de bestaande webservice en implementeert u de nieuwe Voorspellend experiment is geïmplementeerd in plaats daarvan.

> [!NOTE]
> Als u configuratiewijzigingen in de oorspronkelijke webservice aangebracht, bijvoorbeeld moet invoeren van een nieuwe weergavenaam of beschrijving, u deze waarden opnieuw invoeren.
> 
> 

Een optie voor het bijwerken van de webservice is aan het model programmatisch opnieuw trainen. Zie [Machine Learning-modellen programmatisch opnieuw trainen](retrain-models-programmatically.md) voor meer informatie.

<!-- internal links -->
[Een opleidingsexperiment maken]: #create-a-training-experiment
[Converteren naar een Voorspellend experiment]: #convert-the-training-experiment-to-a-predictive-experiment
[Als een webservice implementeren]: #deploy-it-as-a-web-service
[Nieuw]: #deploy-the-predictive-experiment-as-a-new-web-service
[Klassiek]: #deploy-the-predictive-experiment-as-a-classic-web-service
[Access]: #access-the-Web-service
[Manage]: #manage-the-Web-service-in-the-azure-management-portal
[Update]: #update-the-Web-service
