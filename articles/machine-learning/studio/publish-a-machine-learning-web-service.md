---
title: Een Machine Learning Studio-webservice implementeren
titleSuffix: Azure Machine Learning Studio
description: Hoe u een opleidingsexperiment converteren naar een Voorspellend experiment, voorbereiding op deze implementatie vervolgens als een Azure Machine Learning Studio-webservice implementeren.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: ericlicoding
ms.author: amlstudiodocs
ms.custom: previous-ms.author=yahajiza, previous-author=YasinMSFT
ms.date: 01/06/2017
ms.openlocfilehash: e56ab4d74b54e1da8b07de6081766c8669bfb68a
ms.sourcegitcommit: 30a0007f8e584692fe03c0023fe0337f842a7070
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57576630"
---
# <a name="deploy-an-azure-machine-learning-studio-web-service"></a>Een Azure Machine Learning Studio-webservice implementeren

Azure Machine Learning Studio kunt u bouwen en testen van een oplossing voor voorspellende analyse. U kunt vervolgens de oplossing implementeren als een webservice.

Machine Learning Studio-webservices bieden een interface tussen een toepassing en een scoremodel voor Machine Learning Studio-werkstroom. Een externe toepassing kan communiceren met een Machine Learning Studio-werkstroomscoremodel in realtime. Een aanroep van een Machine Learning Studio-webservice retourneert voorspellingsresultaten naar een externe toepassing. Tijdens een aanroep aan een webservice wordt een API-sleutel doorgegeven die is gemaakt tijdens de implementatie van de webservice. Een Machine Learning Studio-webservice is gebaseerd op REST, een populaire architectuur voor webprogrammering.

Azure Machine Learning Studio heeft twee soorten webservices:

* Request-Response Service (RRS): Een lage latentie en zeer schaalbare service die één gegevensrecord beoordeelt.
* Batch Execution Service (BES): Een asynchrone service die scores een batch gegevensrecords.

De invoer voor BES is net als de gegevensinvoer die door RRS wordt gebruikt. Het belangrijkste verschil is dat BES een blok records uit diverse bronnen leest, zoals Azure Blob Storage, Azure Table Storage, Azure SQL Database, HDInsight (hive query) en HTTP-bronnen.

Vanuit een op hoog niveau-van-oogpunt, moet u uw model in drie stappen implementeren:

* **[Een opleidingsexperiment maken]**  -In Studio die u kunt trainen en testen van een predictive analytics-model met behulp van de trainingsgegevens die u opgeeft, met behulp van een groot aantal ingebouwde machine learning-algoritmen.
* **[Converteren naar een Voorspellend experiment]**  -zodra uw model is getraind met bestaande gegevens en u bent klaar om te gebruiken voor het scoren van nieuwe gegevens, u voorbereidt en stroomlijnen van uw experiment voor voorspellingen.
* **Implementeer** als een **[nieuwe webservice]** of een **[klassieke webservice]** : wanneer u uw Voorspellend experiment als implementeren een Azure-web-service, gebruikers kunnen gegevens aan uw model verzenden en ontvangen van het model voorspellingen.

## <a name="create-a-training-experiment"></a>Een opleidingsexperiment maken

Als u wilt een Voorspellend model te trainen, kunt u Azure Machine Learning Studio maakt een opleidingsexperiment neemt u diverse modules trainingsgegevens laden, voorbereiden van de gegevens zo nodig, toepassen van machine learning-algoritmen en evalueren van de resultaten. U kunt een experiment herhalen en andere machine learning-algoritmen om te vergelijken en de resultaten evalueren proberen.

Het proces voor het maken en beheren van training experimenten wordt uitgebreid elders beschreven. Raadpleeg voor meer informatie de volgende artikelen:

* [Een eenvoudig experiment maken in Azure Machine Learning Studio](create-experiment.md)
* [Een voorspellende oplossing met Azure Machine Learning Studio ontwikkelen](tutorial-part1-credit-risk.md)
* [Uw trainingsgegevens importeren in Azure Machine Learning Studio](import-data.md)
* [Experimentherhalingen in Azure Machine Learning Studio beheren](manage-experiment-iterations.md)

## <a name="convert-the-training-experiment-to-a-predictive-experiment"></a>Het trainingsexperiment converteren naar een voorspellend experiment

Nadat u uw model hebt getraind, u kunt uw trainingsexperiment converteren naar een Voorspellend experiment te scoren van nieuwe gegevens.

Door te converteren naar een Voorspellend experiment, bent u het getrainde model kan worden geïmplementeerd als een scoringwebservice ophalen. Gebruikers van de webservice kunnen invoergegevens verzenden aan uw model en het model wordt terugsturen van de voorspellingsresultaten. Als u een Voorspellend experiment omzetten, houd er rekening mee hoe u uw model moet worden gebruikt door anderen verwacht.

Als u wilt uw trainingsexperiment converteren naar een Voorspellend experiment, klikt u op **uitvoeren** aan de onderkant van het experimentcanvas, klikt u op **webservice ingesteld**en selecteer vervolgens **voorspellende webservice**.

![Converteren naar score experiment](./media/publish-a-machine-learning-web-service/figure-1.png)

Zie voor meer informatie over het uitvoeren van deze conversie een hoeveelheid [over het voorbereiden van uw model voor implementatie in Azure Machine Learning Studio](convert-training-experiment-to-scoring-experiment.md).

De volgende stappen beschrijven een Voorspellend experiment als een nieuwe webservice implementeren. U kunt ook het experiment implementeren als een klassieke webservice.

## <a name="deploy-it-as-a-new-web-service"></a>Als een nieuwe webservice implementeren

Nu dat de Voorspellend experiment is voorbereid, kunt u deze kunt implementeren als een nieuwe (op een Resource Manager gebaseerde) Azure-web-service. Met behulp van de webservice, kunnen gebruikers gegevens verzenden naar uw model en het model de voorspellingen wordt geretourneerd.

Als u wilt uw Voorspellend experiment implementeren, klikt u op **uitvoeren** aan de onderkant van het experimentcanvas. Wanneer het experiment is voltooid, klikt u op **webservice implementeren** en selecteer **webservice implementeren [nieuw]**.  De implementatiepagina van de portal voor Machine Learning Studio-webservice wordt geopend.

> [!NOTE] 
> Voor het implementeren van een nieuwe webservice moet u voldoende machtigingen hebben in het abonnement waarvoor u de webservice implementeert. Zie voor meer informatie, [beheren van een webservice met behulp van de Azure Machine Learning-webserviceportal](manage-new-webservice.md). 

### <a name="machine-learning-studio-web-service-portal-deploy-experiment-page"></a>Machine Learning Studio-webservice portal implementeren Experiment pagina

Voer een naam voor de webservice op de pagina Experiment implementeren.
Een prijscategorie selecteren. Hebt u een bestaande prijsstelling dat kunt u dit selecteren, moet anders u een nieuwe prijs plan maken voor de service.

1. In de **Prijsplan** vervolgkeuzelijst, selecteert u een bestaand plan of de **Selecteer nieuw plan** optie.
2. In **Plannaam**, typ een naam waarmee het abonnement op uw factuur wordt geïdentificeerd.
3. Selecteer een van de **maandelijkse Plan lagen**. Standaard worden de prijscategorieën voor uw standaardregio gebruikt en de webservice wordt ook in die regio geïmplementeerd.

Klik op **implementeren** en de **snelstartgids** pagina voor uw webservice wordt geopend.

De webpagina van de service-Quick Start hebt u toegang en richtlijnen op de meest algemene taken die u uitvoeren wilt na het maken van een webservice. Hier kunt u eenvoudig de testpagina en de verbruikspagina openen.

<!-- ![Deploy the web service](./media/publish-a-machine-learning-web-service/figure-2.png)-->

### <a name="test-your-new-web-service"></a>Uw nieuwe webservice testen

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

![De webservice configureren](./media/publish-a-machine-learning-web-service/figure-8-arm-configure.png)

### <a name="access-your-new-web-service"></a>Toegang tot uw nieuwe webservice

Nadat u uw webservice van Machine Learning Studio implementeert, kunt u gegevens verzenden naar de service en antwoorden ontvangen via een programma.

De **verbruiken** pagina vindt u alle informatie die u nodig hebt voor toegang tot uw webservice. Bijvoorbeeld, is de API-sleutel opgegeven waarmee geautoriseerde toegang tot de service.

Zie voor meer informatie over het openen van een Machine Learning Studio-webservice [hoe u een webservice Azure Machine Learning Studio gebruiken](consume-web-services.md).

### <a name="manage-your-new-web-service"></a>Uw nieuwe webservice beheren

U kunt uw nieuwe web services Machine Learning Studio-webservices-portal beheren. Uit de [portal hoofdpagina](https://services.azureml-test.net/), klikt u op **webservices**. U kunt verwijderen of kopiëren van een service van de webpagina van services. Klik op de service voor het controleren van een bepaalde service, en klik vervolgens op **Dashboard**. Voor het controleren van batch-taken die zijn gekoppeld aan de webservice, klikt u op **logboek voor Batch aanvraag**.

### <a id="multi-region"></a> Uw nieuwe webservice implementeren in meerdere regio 's

U kunt eenvoudig een nieuwe webservice implementeren in meerdere regio's zonder meerdere abonnementen of werkruimten.

Prijzen is regiospecifiek, dus moet u voor het definiëren van een abonnement voor elke regio waarin u de webservice implementeren.

#### <a name="create-a-plan-in-another-region"></a>Een plan maken in een andere regio

1. Aanmelden bij [Microsoft Azure Machine Learning-webservices](https://services.azureml.net/).
2. Klik op de **plannen** menu-optie.
3. Klik op de abonnementen via de weergavepagina **nieuw**.
4. Uit de **abonnement** vervolgkeuzelijst, selecteer het abonnement waarin het nieuwe abonnement worden geplaatst.
5. Uit de **regio** vervolgkeuzelijst, selecteer een regio voor het nieuwe abonnement. De opties plannen voor de geselecteerde regio wordt weergegeven in de **opties plannen** sectie van de pagina.
6. Uit de **resourcegroep** vervolgkeuzelijst, selecteer een resourcegroep voor het abonnement. Zie voor meer informatie over resourcegroepen [overzicht van Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).
7. In **Plannaam** typt u de naam van het plan.
8. Onder **abonnementsopties**, klikt u op het niveau van de facturering voor het nieuwe abonnement.
9. Klik op **Create**.

#### <a name="deploy-the-web-service-to-another-region"></a>De webservice implementeert in een andere regio

1. Klik op de pagina Microsoft Azure Machine Learning-webservices op de **webservices** menu-optie.
2. Selecteer de Web-Service die u naar een nieuwe regio implementeert.
3. Klik op **kopie**.
4. In **naam van de webservice**, typt u een nieuwe naam voor de webservice.
5. In **Web servicebeschrijving**, typ een beschrijving voor de webservice.
6. Uit de **abonnement** vervolgkeuzelijst, selecteer het abonnement waarin de nieuwe webservice zich bevindt.
7. Uit de **resourcegroep** vervolgkeuzelijst, selecteer een resourcegroep voor de webservice. Zie voor meer informatie over resourcegroepen [overzicht van Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).
8. Uit de **regio** vervolgkeuzelijst, selecteer de regio waarin u kunt de webservice implementeren.
9. Uit de **opslagaccount** vervolgkeuzelijst, selecteer een storage-account waarin u voor het opslaan van de webservice.
10. Uit de **Prijsplan** vervolgkeuzelijst, selecteer een abonnement in de regio die u hebt geselecteerd in stap 8.
11. Klik op **kopie**.

## <a name="deploy-it-as-a-classic-web-service"></a>Als een klassieke webservice implementeren

Nu dat de Voorspellend experiment voldoende is voorbereid, kunt u deze kunt implementeren als een klassieke Azure-webservice. Met behulp van de webservice, kunnen gebruikers gegevens verzenden naar uw model en het model de voorspellingen wordt geretourneerd.

Als u wilt uw Voorspellend experiment implementeren, klikt u op **uitvoeren** canvas aan de onderkant van het experiment en klik vervolgens op **webservice implementeren**. De webservice is ingesteld en u in het dashboard van de webservice worden geplaatst.

![De webservice implementeren](./media/publish-a-machine-learning-web-service/figure-2.png)

### <a name="test-your-classic-web-service"></a>Uw klassieke webservice testen

U kunt de webservice testen in de portal van de Machine Learning Studio-webservices of Machine Learning Studio.

Als u wilt testen de Request Response-webservice, klikt u op de **testen** knop in het dashboard van de webservice. Een dialoogvenster wordt weergegeven gebruiken om u te vragen voor de invoergegevens voor de service. Dit zijn de kolommen die door het scoring experiment wordt verwacht. Voer een set gegevens in en klik vervolgens op **OK**. De resultaten die worden gegenereerd door de webservice worden weergegeven aan de onderkant van het dashboard.

U kunt klikken op de **testen** preview koppeling naar uw service testen in de portal voor Azure Machine Learning Studio-webservices, zoals eerder in de sectie nieuwe web-service.

Als u wilt testen van de Batch-Service kan worden uitgevoerd, klikt u op **testen** link bekijken. Klik op Bladeren onder uw invoer en selecteer een CSV-bestand met de juiste voorbeeldwaarden op de pagina van de test Batch. Als u een CSV-bestand hoeft, en u uw Voorspellend experiment met behulp van Machine Learning Studio hebt gemaakt, kunt u de gegevensset voor uw Voorspellend experiment downloaden en gebruiken.

![De webservice testen](./media/publish-a-machine-learning-web-service/figure-3.png)

Op de **configuratie** pagina, kunt u de weergavenaam van de service wijzigen en wijs hieraan een beschrijving. De naam en beschrijving wordt weergegeven in de [Azure-portal](https://portal.azure.com/) waar u uw webservices beheren.

U kunt een beschrijving voor uw invoergegevens, de uitvoergegevens en het web serviceparameters opgeven door te voeren van een tekenreeks voor elke kolom onder **INVOERSCHEMA**, **UITVOERSCHEMA**, en **-WEBSERVICE PARAMETER**. Deze beschrijvingen worden gebruikt in het voorbeeld van code-documentatie voor de webservice.

U kunt logboekregistratie inschakelen om opsporen van fouten die u ziet wanneer uw webservice wordt geopend. Zie voor meer informatie, [logboekregistratie inschakelen voor Machine Learning Studio-webservices](web-services-logging.md).

![De webservice configureren](./media/publish-a-machine-learning-web-service/figure-4.png)

U kunt ook de eindpunten voor de webservice configureren in de portal voor Azure Machine Learning-webservices die vergelijkbaar is met de procedure eerder weergegeven in de sectie nieuwe web-service. Zijn er andere opties, kunt u toevoegen of wijzigen van de beschrijving van de service, logboekregistratie inschakelen en de voorbeeldgegevens inschakelen voor het testen.

### <a name="access-your-classic-web-service"></a>Toegang tot uw klassieke webservice

Nadat u uw webservice van Machine Learning Studio implementeert, kunt u gegevens verzenden naar de service en antwoorden ontvangen via een programma.

Het dashboard bevat alle informatie die u nodig hebt voor toegang tot uw webservice. Bijvoorbeeld, de API-sleutel waarmee geautoriseerde toegang tot de service wordt geleverd, en API help-pagina's zijn bedoeld om u aan de slag uw code te schrijven.

Zie voor meer informatie over het openen van een Machine Learning Studio-webservice [hoe u een webservice Azure Machine Learning Studio gebruiken](consume-web-services.md).

### <a name="manage-your-classic-web-service"></a>Uw klassieke webservice beheren

Er zijn verschillende acties die u kunt uitvoeren voor het bewaken van een webservice. U kunt bijwerken en verwijderen. U kunt ook extra eindpunten toevoegen aan een klassieke webservice naast de standaardeindpunt dat wordt gemaakt wanneer u deze implementeren.

Zie voor meer informatie, [een Azure Machine Learning Studio-werkruimte beheren](manage-workspace.md) en [een webservice met behulp van de portal van Azure Machine Learning Studio-webservices beheren](manage-new-webservice.md).

## <a name="update-the-web-service"></a>Bijwerken van de webservice
U kunt wijzigingen aanbrengen in uw webservice, zoals het bijwerken van het model met aanvullende gegevens, en implementeert, overschrijven de oorspronkelijke web-service.

Voor het bijwerken van de webservice, opent u de oorspronkelijke Voorspellend experiment dat u gebruikt voor het implementeren van de webservice en een bewerkbare kopie maken door te klikken op **SAVE AS**. Breng uw wijzigingen en klik vervolgens op **webservice implementeren**.

Omdat u dit experiment voordat hebt geïmplementeerd, kunt u wordt gevraagd of u wilt overschrijven (klassieke webservice) of de bestaande service (nieuwe webservice) bijwerken. Te klikken op **Ja** of **Update** Hiermee stopt u de bestaande webservice en implementeert u de nieuwe Voorspellend experiment is geïmplementeerd in plaats daarvan.

> [!NOTE]
> Als u configuratiewijzigingen in de oorspronkelijke webservice aangebracht, bijvoorbeeld moet invoeren van een nieuwe weergavenaam of beschrijving, u deze waarden opnieuw invoeren.

Een optie voor het bijwerken van de webservice is aan het model programmatisch opnieuw trainen. Zie voor meer informatie, [-modellen programmatisch opnieuw trainen van Machine Learning Studio](/azure/machine-learning/studio/retrain-machine-learning-model).

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer technische informatie over de werking van implementatie [hoe een Machine Learning Studio-model verder uit een experiment naar een ingezette webservice](model-progression-experiment-to-web-service.md).

* Zie voor meer informatie over hoe u aan uw model klaar om te implementeren, [over het voorbereiden van uw model voor implementatie in Azure Machine Learning Studio](convert-training-experiment-to-scoring-experiment.md).

* Er zijn verschillende manieren om de REST-API te gebruiken en toegang te krijgen tot de webservice. Zie [hoe u een Azure Machine Learning Studio-webservice gebruiken](consume-web-services.md).


<!-- internal links -->
[Een opleidingsexperiment maken]: #create-a-training-experiment
[Converteren naar een Voorspellend experiment]: #convert-the-training-experiment-to-a-predictive-experiment
[Nieuwe webservice]: #deploy-it-as-a-new-web-service
[Klassieke webservice]: #deploy-it-as-a-classic-web-service
[Nieuw]: #deploy-it-as-a-new-web-service
[classic]: #deploy-the-predictive-experiment-as-a-classic-web-service
[Access]: #access-the-Web-service
[Manage]: #manage-the-Web-service-in-the-azure-management-portal
[Update]: #update-the-Web-service
