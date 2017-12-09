---
title: Met behulp van importeren/exporteren-gegevens in Azure Machine Learning-webservices | Microsoft Docs
description: Informatie over het gebruik van de modules die gegevens importeren en exporteren van gegevens te verzenden en ontvangen van gegevens van een webservice.
services: machine-learning
documentationcenter: 
author: garyericson
manager: raymondlaghaeian
editor: 
ms.assetid: 3a7ac351-ebd3-43a1-8c5d-18223903d08e
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/28/2017
ms.author: v-donglo
ms.openlocfilehash: 54b0434cb753484bdb8300f4b9c4f7f7ca75c7c8
ms.sourcegitcommit: 42ee5ea09d9684ed7a71e7974ceb141d525361c9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/09/2017
---
# <a name="deploying-azure-ml-web-services-that-use-data-import-and-data-export-modules"></a>Azure ML-webservices implementeren die gebruikmaken van gegevensimport- en gegevensexportmodules

Wanneer u een Voorspellend experiment maakt, voegt u gewoonlijk toe een web service invoer en uitvoer. Wanneer u het experiment implementeert, kunnen consumenten gegevens verzenden en ontvangen van de webservice via de invoer en uitvoer. Voor sommige toepassingen van de consumer gegevens uit een gegevensfeed beschikbaar of al bevinden zich in een externe gegevensbron zoals Azure Blob-opslag. In dergelijke gevallen hoeven ze niet lezen en schrijven van gegevens met behulp van web service in- en uitgangen. Ze kunnen in plaats daarvan Batch uitvoering Service (BES) gebruiken om te lezen van gegevens uit de gegevensbron met behulp van een module gegevens importeren en de resultaten van score berekenen schrijven naar een andere locatie met behulp van een module gegevens exporteren.

De gegevens importeren en exporteren modules die gegevens lezen uit en schrijven naar verschillende gegevens locaties zoals een via HTTP, een Hive-Query, een Azure SQL-database, Azure Table storage, Azure Blob storage, een Gegevensfeed-URL opgeeft, of een lokale SQL-database.

In dit onderwerp worden de "voorbeeld 5: trein, Test evalueren voor binaire classificatie: volwassenen gegevensset ' steekproef en wordt ervan uitgegaan dat de dataset al is geladen in een Azure SQL-tabel met de naam censusdata.

## <a name="create-the-training-experiment"></a>De trainingsexperiment maken
Wanneer u opent de "voorbeeld 5: trein, Test evalueren voor binaire classificatie: volwassenen gegevensset ' voorbeeld de voorbeeldgegevensset volwassenen inventarisering Income binaire classificatie wordt gebruikt. En het experiment in het canvas ziet er ongeveer als de volgende afbeelding:

![De aanvankelijke configuratie van het experiment.](./media/web-services-that-use-import-export-modules/initial-look-of-experiment.png)

De gegevens lezen uit de Azure SQL-tabel:

1. De dataset-module verwijderen.
2. Typ in het zoekvak onderdelen importeren.
3. Toevoegen van de lijst met resultaten een *importgegevens* module naar het experimentcanvas.
4. Verbinding maken met de uitvoer van de *importgegevens* module de invoer van de *Clean Missing Data* module.
5. Selecteer in het eigenschappendeelvenster **Azure SQL Database** in de **gegevensbron** vervolgkeuzelijst.
6. In de **databaseservernaam**, **databasenaam**, **gebruikersnaam**, en **wachtwoord** velden, voer de juiste informatie voor uw de database.
7. Voer de volgende query in het databaseveld query.
   
     Selecteer [leeftijd]
   
        [workclass],
        [fnlwgt],
        [education],
        [education-num],
        [marital-status],
        [occupation],
        [relationship],
        [race],
        [sex],
        [capital-gain],
        [capital-loss],
        [hours-per-week],
        [native-country],
        [income]
     van dbo.censusdata;
8. Klik aan de onderkant van het experimentcanvas **uitvoeren**.

## <a name="create-the-predictive-experiment"></a>De Voorspellend experiment maken
De volgende instellen van de Voorspellend experiment van waaruit u uw webservice implementeert.

1. Klik onderaan in het experimentcanvas **webservice ingesteld** en selecteer **voorspellende Web Service (aanbevolen)**.
2. Verwijder de *Web Service invoer* en *Web Service uitvoer modules* van Voorspellend experiment. 
3. Typ in het zoekvak onderdelen exporteren.
4. Toevoegen van de lijst met resultaten een *gegevens exporteren* module naar het experimentcanvas.
5. Verbinding maken met de uitvoer van de *Score Model* module de invoer van de *gegevens exporteren* module. 
6. Selecteer in het eigenschappendeelvenster **Azure SQL Database** in de vervolgkeuzelijst voor het doel van gegevens.
7. In de **databaseservernaam**, **databasenaam**, **Server gebruikersaccountnaam**, en **Server het wachtwoord voor gebruikersaccount** velden, voer de relevante informatie voor uw database.
8. In de **door komma's gescheiden lijst met kolommen op te slaan** veld, typt u Scored Labels.
9. In de **gegevensveld tabel naam**, typt u dbo. ScoredLabels. Als de tabel niet bestaat, wordt deze gemaakt wanneer het experiment is uitgevoerd of de webservice wordt aangeroepen.
10. In de **door komma's gescheiden lijst met kolommen datatable** veld ScoredLabels.

Wanneer u een toepassing die de laatste webservice roept schrijft, is het raadzaam om op te geven van een andere invoer voor de query of een doeltabel tijdens runtime. Wilt configureren en deze uitgangen, gebruikt u de functie Webserviceparameters om in te stellen de *importgegevens* module *gegevensbron* eigenschap en de *gegevens exporteren* modus gegevens doel-eigenschap.  Zie voor meer informatie over Webserviceparameters de [Webserviceparameters AzureML-vermelding](https://blogs.technet.microsoft.com/machinelearning/2014/11/25/azureml-web-service-parameters/) op de Cortana Intelligence en Machine Learning-Blog.

Parameters van de webservice voor de query importeren en de doeltabel configureren:

1. In het deelvenster met eigenschappen voor de *gegevens importeren* -module, klikt u op het pictogram aan de bovenkant rechts van de **databasequery** veld en selecteert u **instellen als web Serviceparameter**.
2. In het deelvenster met eigenschappen voor de *gegevens exporteren* -module, klikt u op het pictogram aan de bovenkant rechts van de **gegevens tabelnaam** veld en selecteert u **instellen als web Serviceparameter**.
3. Aan de onderkant van de *gegevens exporteren* eigenschappendeelvenster module in de **Webserviceparameters** sectie, klikt u op databasequery en wijzig deze met een Query.
4. Klik op **gegevens tabelnaam** en wijzig de naam **tabel**.

Wanneer u klaar bent, is uw experiment moet er ongeveer als de volgende afbeelding:

![Laatste uiterlijk van experiment.](./media/web-services-that-use-import-export-modules/experiment-with-import-data-added.png)

U kunt nu het experiment implementeren als een webservice.

## <a name="deploy-the-web-service"></a>De webservice implementeren
U kunt implementeren op een klassiek of een nieuwe webservice.

### <a name="deploy-a-classic-web-service"></a>Een klassieke webservice implementeren
Als een klassieke webservice implementeert en een toepassing gebruiken voor het maken:

1. Klik op uitvoeren aan de onderkant van het experimentcanvas.
2. Wanneer de uitvoering is voltooid, klikt u op **webservice implementeren** en selecteer **webservice implementeren [klassieke]**.
3. Zoek uw API-sleutel op het web service-dashboard. Kopiëren en opslaan voor later gebruik.
4. In de **standaard eindpunt** tabel, klikt u op de **Batchuitvoering** koppeling naar de API-pagina te openen.
5. Maak een C#-consoletoepassing in Visual Studio: **nieuw** > **Project** > **Visual C#** > **Windows Klassieke bureaublad** > **Console-App (.NET Framework)**.
6. Op de pagina API Help vinden de **voorbeeldcode** sectie aan de onderkant van de pagina.
7. Kopieer en plak de C#-voorbeeldcode in uw Program.cs-bestand en verwijder alle verwijzingen naar de blobopslag.
8. Werk de waarde van de *apiKey* variabele met de API-sleutel eerder hebt opgeslagen.
9. Zoek de declaratie van de aanvraag en werk de waarden van de Webserviceparameters die worden doorgegeven aan de *importgegevens* en *gegevens exporteren* modules. In dit geval u gebruikt u de oorspronkelijke query, maar de naam van een nieuwe tabel te definiëren.
   
        var request = new BatchExecutionRequest() 
        {           
            GlobalParameters = new Dictionary<string, string>() {
                { "Query", @"select [age], [workclass], [fnlwgt], [education], [education-num], [marital-status], [occupation], [relationship], [race], [sex], [capital-gain], [capital-loss], [hours-per-week], [native-country], [income] from dbo.censusdata" },
                { "Table", "dbo.ScoredTable2" },
            }
        };
10. Voer de toepassing uit. 

Na voltooiing van de cyclus wordt een nieuwe tabel toegevoegd aan de database met de resultaten van score berekenen.

### <a name="deploy-a-new-web-service"></a>Een nieuwe webservice implementeren

> [!NOTE] 
> Voor het implementeren van een nieuwe webservice moet u voldoende machtigingen hebben in het abonnement waaraan u de webservice implementeren. Zie voor meer informatie [beheren van een webservice via de portal voor Azure Machine Learning-webservices](manage-new-webservice.md). 

Als een nieuwe webservice implementeert en een toepassing gebruiken voor het maken:

1. Klik aan de onderkant van het experimentcanvas **uitvoeren**.
2. Wanneer de uitvoering is voltooid, klikt u op **webservice implementeren** en selecteer **Web Service implementeren [New]**.
3. Voer een naam voor uw webservice op de pagina Experiment implementeren en een prijscategorie selecteren en klik vervolgens op **implementeren**.
4. Op de **Quick Start** pagina, klikt u op **verbruiken**.
5. In de **voorbeeldcode** sectie, klikt u op **Batch**.
6. Maak een C#-consoletoepassing in Visual Studio: **nieuw** > **Project** > **Visual C#** > **Windows Klassieke bureaublad** > **Console-App (.NET Framework)**.
7. Kopieer en plak de C#-voorbeeldcode in uw Program.cs-bestand.
8. Werk de waarde van de *apiKey* variabele met de **primaire sleutel** zich in de **Basic verbruik info** sectie.
9. Zoek de *scoreRequest* declaratie en werk de waarden van de Webserviceparameters die worden doorgegeven aan de *importgegevens* en *gegevens exporteren* modules. In dit geval u gebruikt u de oorspronkelijke query, maar de naam van een nieuwe tabel te definiëren.
   
        var scoreRequest = new
        {       
            Inputs = new Dictionary<string, StringTable>()
            {
            },
            GlobalParameters = new Dictionary<string, string>() {
                { "Query", @"select [age], [workclass], [fnlwgt], [education], [education-num], [marital-status], [occupation], [relationship], [race], [sex], [capital-gain], [capital-loss], [hours-per-week], [native-country], [income] from dbo.censusdata" },
                { "Table", "dbo.ScoredTable3" },
            }
        };
10. Voer de toepassing uit. 

