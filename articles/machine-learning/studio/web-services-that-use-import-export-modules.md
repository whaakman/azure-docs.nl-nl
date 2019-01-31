---
title: 'Import/Export-gegevens in de webservices: Azure Machine Learning Studio | Microsoft Docs'
description: Informatie over het gebruik van de modules importeren en exporteren van gegevens te verzenden en ontvangen van gegevens vanuit een webservice.
services: machine-learning
documentationcenter: ''
author: ericlicoding
ms.custom: seodec18
ms.author: amlstudiodocs
editor: cgronlun
ms.assetid: 3a7ac351-ebd3-43a1-8c5d-18223903d08e
ms.service: machine-learning
ms.subservice: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/28/2017
ms.openlocfilehash: ca4c8c2d16e2cc44768785c26ffa070b59d5f8d7
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55478012"
---
# <a name="deploy-azure-machine-learning-studio-web-services-that-use-data-import-and-data-export-modules"></a>Azure Machine Learning Studio-webservices die gebruikmaken van gegevens importeren en exporteren van gegevens modules implementeren

Wanneer u een Voorspellend experiment maakt, toevoegen u gewoonlijk een web-invoer en uitvoer. Wanneer u het experiment implementeert, kunnen gebruikers verzonden en ontvangen van gegevens van de webservice via de invoer en uitvoer. Voor sommige toepassingen van een gebruiker gegevens uit een gegevensfeed beschikbaar of al bevinden zich in een externe gegevensbron, zoals Azure Blob-opslag. In dergelijke gevallen hoeven ze niet lezen en schrijven van gegevens met behulp van web service invoer en uitvoer. Ze kunnen in plaats daarvan de Batch Execution Service (BES) gebruiken om gegevens te lezen uit de gegevensbron met behulp van een module gegevens importeren en de scoring resultaten schrijven naar een andere locatie met behulp van een module gegevens exporteren.

De gegevens importeren en exporteren van gegevens-modules, kunnen lezen en schrijven naar verschillende gegevens locaties, zoals een via HTTP, een Hive-Query, een Azure SQL-database, Azure Table storage, Azure Blob-opslag, een Gegevensfeed-URL opgeven of een on-premises SQL-database.

In dit onderwerp wordt gebruikgemaakt van de "voorbeeld 5: Trainen, testen, evalueren voor binaire classificatie: Volwassenen gegevensset'-voorbeeld en wordt ervan uitgegaan dat de gegevensset is al geladen in een Azure SQL-tabel met de naam censusdata.

## <a name="create-the-training-experiment"></a>De trainingsexperiment maken
Wanneer u opent de "voorbeeld 5: Trainen, testen, evalueren voor binaire classificatie: Volwassenen gegevensset' voorbeeld van de voorbeeldgegevensset voor volwassenen telling inkomsten binaire classificatie wordt gebruikt. En het experiment in het canvas ziet eruit als in de volgende afbeelding:

![Eerste configuratie van het experiment.](./media/web-services-that-use-import-export-modules/initial-look-of-experiment.png)

De gegevens lezen uit de Azure SQL-tabel:

1. De gegevensset-module verwijderen.
2. Typ in het zoekvak onderdelen importeren.
3. In de lijst met resultaten, Voeg een *importgegevens* module naar het experimentcanvas.
4. Koppel de uitvoer van de *importgegevens* module de invoer van de *Clean Missing Data* module.
5. Selecteer in het deelvenster met eigenschappen, **Azure SQL Database** in de **gegevensbron** vervolgkeuzelijst.
6. In de **databaseservernaam**, **databasenaam**, **gebruikersnaam**, en **wachtwoord** velden, voer de juiste gegevens voor uw de database.
7. Voer de volgende query in het queryveld Database.

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
8. Aan de onderkant van het experimentcanvas, klikt u op **uitvoeren**.

## <a name="create-the-predictive-experiment"></a>De Voorspellend experiment maken
De volgende instellen van de Voorspellend experiment van waaruit u uw webservice implementeert.

1. Aan de onderkant van het experimentcanvas, klikt u op **webservice ingesteld** en selecteer **voorspellende webservice (aanbevolen)**.
2. Verwijder de *Web Service invoer* en *Web Service uitvoer modules* uit de Voorspellend experiment.
3. Typ in het zoekvak onderdelen exporteren.
4. In de lijst met resultaten, Voeg een *gegevens exporteren* module naar het experimentcanvas.
5. Koppel de uitvoer van de *Score Model* module de invoer van de *gegevens exporteren* module.
6. Selecteer in het deelvenster met eigenschappen, **Azure SQL Database** in de vervolgkeuzelijst voor het doel van gegevens.
7. In de **databaseservernaam**, **databasenaam**, **accountnaam Server**, en **Server het wachtwoord voor gebruikersaccount** velden, voer de relevante informatie voor uw database.
8. In de **door komma's gescheiden lijst met kolommen op te slaan** veld, typt u Scored Labels.
9. In de **gegevensveld tabel naam**, typt u dbo. ScoredLabels. Als de tabel niet bestaat, wordt deze gemaakt wanneer het experiment wordt uitgevoerd of de webservice wordt aangeroepen.
10. In de **door komma's gescheiden lijst met kolommen voor datatable** veld, typt u ScoredLabels.

Wanneer u een toepassing die de laatste webservice roept schrijft, kunt u een andere invoer voor de query of doeltabel opgeven tijdens de uitvoering. Als u wilt deze invoer en uitvoer configureren, gebruikt u de functie Webserviceparameters om in te stellen de *importgegevens* module *gegevensbron* eigenschap en de *gegevens exporteren* modus gegevens doel-eigenschap.  Zie voor meer informatie over Webserviceparameters de [Azure Machine Learning studio Webserviceparameters vermelding](https://blogs.technet.microsoft.com/machinelearning/2014/11/25/azureml-web-service-parameters/) op de Cortana Intelligence en Machine Learning Blog.

Parameters van de webservice voor de import-query en de doeltabel configureren:

1. In het deelvenster met eigenschappen voor de *gegevens importeren* -module, klikt u op het pictogram aan de bovenkant van de **databasequery** veld en selecteert u **instellen als web Serviceparameter**.
2. In het deelvenster met eigenschappen voor de *gegevens exporteren* -module, klikt u op het pictogram aan de bovenkant van de **gegevens tabelnaam** veld en selecteert u **instellen als web Serviceparameter**.
3. Aan de onderkant van de *gegevens exporteren* module het deelvenster met eigenschappen in de **Webserviceparameters** sectie, klikt u op Database-query en wijzig de naam Query.
4. Klik op **gegevens tabelnaam** en wijzig de naam **tabel**.

Wanneer u klaar bent, is uw experiment ziet die vergelijkbaar is met de volgende afbeelding:

![Laatste uiterlijk van het experiment.](./media/web-services-that-use-import-export-modules/experiment-with-import-data-added.png)

U kunt nu het experiment implementeren als een webservice.

## <a name="deploy-the-web-service"></a>De webservice implementeren
U kunt implementeren op een klassieke of een nieuwe webservice betreft.

### <a name="deploy-a-classic-web-service"></a>Een klassieke webservice implementeren
Als een klassieke webservice implementeren en maken van een toepassing gebruikt:

1. Klik op uitvoeren aan de onderkant van het experimentcanvas.
2. Wanneer de uitvoering is voltooid, klikt u op **webservice implementeren** en selecteer **webservice implementeren [klassieke]**.
3. Zoek op het dashboard van de webservice, uw API-sleutel. Kopiëren en opslaan voor later gebruik.
4. In de **standaard eindpunt** tabel, klikt u op de **Batchuitvoering** koppeling naar de API Help-pagina te openen.
5. Maak in Visual Studio een C# consoletoepassing: **Nieuwe** > **Project** > **Visual C#**   >  **Windows Classic Desktop**  >   **Console-App (.NET Framework)**.
6. Zoek op de API Help-pagina, de **voorbeeldcode** sectie aan de onderkant van de pagina.
7. Kopieer en plak de C# voorbeeldcode in het bestand Program.cs en verwijder alle verwijzingen naar de blob-opslag.
8. Werk de waarde van de *apiKey* variabele met de API-sleutel eerder hebt opgeslagen.
9. Zoek de declaratie van de aanvraag en de waarden voor de Webserviceparameters die worden doorgegeven aan de *importgegevens* en *gegevens exporteren* modules. In dit geval u gebruikt u de oorspronkelijke query, maar de naam van een nieuwe tabel te definiëren.

        var request = new BatchExecutionRequest()
        {
            GlobalParameters = new Dictionary<string, string>() {
                { "Query", @"select [age], [workclass], [fnlwgt], [education], [education-num], [marital-status], [occupation], [relationship], [race], [sex], [capital-gain], [capital-loss], [hours-per-week], [native-country], [income] from dbo.censusdata" },
                { "Table", "dbo.ScoredTable2" },
            }
        };
10. Voer de toepassing uit.

Na voltooiing van de uitvoering wordt een nieuwe tabel toegevoegd aan de database met de scoring resultaten.

### <a name="deploy-a-new-web-service"></a>Een nieuwe webservice implementeren

> [!NOTE]
> Voor het implementeren van een nieuwe webservice moet u voldoende machtigingen hebben in het abonnement waarvoor u de webservice implementeert. Zie voor meer informatie, [beheren van een webservice met behulp van de Azure Machine Learning-webserviceportal](manage-new-webservice.md).

Als een nieuwe webservice implementeren en maken van een toepassing gebruikt:

1. Aan de onderkant van het experimentcanvas, klikt u op **uitvoeren**.
2. Wanneer de uitvoering is voltooid, klikt u op **webservice implementeren** en selecteer **Web Service implementeren [Nieuw]**.
3. Voer een naam voor uw webservice op de pagina Experiment implementeren en een prijscategorie selecteren en klik vervolgens op **implementeren**.
4. Op de **snelstartgids** pagina, klikt u op **verbruiken**.
5. In de **voorbeeldcode** sectie, klikt u op **Batch**.
6. Maak in Visual Studio een C# consoletoepassing: **Nieuwe** > **Project** > **Visual C#**   >  **Windows Classic Desktop**  >   **Console-App (.NET Framework)**.
7. Kopieer en plak de C# voorbeeldcode in het bestand Program.cs.
8. Werk de waarde van de *apiKey* variabele met de **primaire sleutel** zich in de **Basic verbruik info** sectie.
9. Zoek de *scoreRequest* declaratie en de waarden voor de Webserviceparameters die worden doorgegeven aan de *importgegevens* en *gegevens exporteren* modules. In dit geval u gebruikt u de oorspronkelijke query, maar de naam van een nieuwe tabel te definiëren.

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

