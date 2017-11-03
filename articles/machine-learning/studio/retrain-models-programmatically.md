---
title: Machine Learning-modellen programmatisch opnieuw trainen | Microsoft Docs
description: Informatie over het programmatisch opnieuw trainen van een model en het bijwerken van de webservice voor het gebruik van het zojuist getrainde model in Azure Machine Learning.
services: machine-learning
documentationcenter: 
author: raymondlaghaeian
manager: jhubbard
editor: cgronlun
ms.assetid: 7ae4f977-e6bf-4d04-9dde-28a66ce7b664
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: raymondl;garye;v-donglo
ms.openlocfilehash: c56ce659766536772d203d0366ef6b53e544a82b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="retrain-machine-learning-models-programmatically"></a>Machine Learning modellen programmatisch opnieuw trainen
In dit scenario leert u hoe u programmatisch opnieuw trainen een Azure Machine Learning-webservice met C# en de Batchuitvoering van Machine Learning-service.

Zodra u hebt het model retrained, geeft de volgende scenario's laten zien hoe het model in uw predictive webservice bijwerken:

* Als u een klassiek webservice in de portal voor Machine Learning-webservices hebt geïmplementeerd, Zie [opnieuw trainen van een webservice klassieke](retrain-a-classic-web-service.md). 
* Als u een nieuwe webservice hebt geïmplementeerd, Zie [opnieuw trainen van de Machine Learning-cmdlets met een nieuwe webservice](retrain-new-web-service-using-powershell.md).

Zie voor een overzicht van het proces retraining [opnieuw een Machine Learning-Model te trainen](retrain-machine-learning-model.md).

Als u beginnen met uw bestaande webservice voor de nieuwe Azure Resource Manager gebaseerde wilt, Zie [opnieuw trainen van een bestaande voorspellende webservice](retrain-existing-resource-manager-based-web-service.md).

## <a name="create-a-training-experiment"></a>Een trainingsexperiment maken
In dit voorbeeld gebruikt u ' voorbeeld 5: trein, Test evalueren voor binaire classificatie: volwassenen gegevensset ' van de Microsoft Azure Machine Learning-voorbeelden. 

Het experiment maken:

1. Meld u aan bij naar Microsoft Azure Machine Learning Studio. 
2. Klik op de rechterbenedenhoek van het dashboard, **nieuw**.
3. Selecteer in de Microsoft-Samples voorbeeld 5.
4. Wijzig de naam van het experiment, aan de bovenkant van het experimentcanvas, selecteert u de naam van het experiment "voorbeeld 5: trein, Test evalueren voor binaire classificatie: volwassenen gegevensset '.
5. Type telling Model.
6. Klik aan de onderkant van het experimentcanvas **uitvoeren**.
7. Klik op **instellen webservice** en selecteer **Retraining webservice**. 

Hieronder ziet u het eerste experiment.
   
   ![Eerste experiment.][2]


## <a name="create-a-predictive-experiment-and-publish-as-a-web-service"></a>Een Voorspellend experiment maken en publiceren als een webservice
Vervolgens maakt u een Experiment Predicative.

1. Aan de onderkant van het experimentcanvas, klikt u op **webservice ingesteld** en selecteer **voorspellende webservice**. Het model als een getraind Model opgeslagen en web service invoer en uitvoer modules worden toegevoegd. 
2. Klik op **Run**. 
3. Nadat het experiment is voltooid, klikt u op **webservice implementeren [klassieke]** of **Web Service implementeren [New]**.

> [!NOTE] 
> Voor het implementeren van een nieuwe webservice moet u voldoende machtigingen hebben in het abonnement waaraan u de webservice implementeren. Zie voor meer informatie, [beheren van een webservice via de portal voor Azure Machine Learning-webservices](manage-new-webservice.md). 

## <a name="deploy-the-training-experiment-as-a-training-web-service"></a>Het trainingsexperiment implementeren als een webservice Training
Als u wilt opnieuw trainen van het getrainde model, moet u het trainingsexperiment die u hebt gemaakt als een webservice Retraining implementeren. Deze webservice moet een *Web Service uitvoer* module die is verbonden met de  *[Train Model] [ train-model]*  -module, kunnen voor het produceren van nieuwe getraind modellen.

1. Klik op het pictogram experimenten in het linkerdeelvenster om terug te keren naar het experimentcanvas training, en klik op het experiment telling Model met de naam.  
2. Typ in het zoekvak Experiment zoekitems webservice. 
3. Sleep een *Web Service invoer* module naar het experimentcanvas en koppel de uitvoer naar de *Clean Missing Data* module.  Dit zorgt ervoor dat uw retraining gegevens op dezelfde manier als uw oorspronkelijke trainingsgegevens is verwerkt.
4. Sleep twee *webservice uitvoer* modules naar het experimentcanvas. Verbinding maken met de uitvoer van de *Train Model* module die u wilt één en de uitvoer van de *Evaluate Model* module naar het andere. De uitvoer van de web service voor **Train Model** waardoor we het nieuwe getrainde model. De uitvoer die zijn gekoppeld aan **Evaluate Model** retourneert die module-uitvoer, die de prestatieresultaten is.
5. Klik op **Run**. 

Vervolgens moet u het trainingsexperiment implementeren als een webservice die een getraind model en de resultaten van evaluatie van model produceert. U doet dit door de volgende reeks acties afhankelijk zijn van of u werkt met een webservice klassieke of een nieuwe webservice.  

**Klassieke webservice**

Klik onderaan in het experimentcanvas **webservice ingesteld** en selecteer **webservice implementeren [klassieke]**. De webservice **Dashboard** weergegeven met de API-sleutel en de API help-pagina voor de uitvoering van de Batch. Alleen de Batchuitvoering-methode kan worden gebruikt voor het maken van de Trained Models.

**Nieuwe webservice**

Klik aan de onderkant van het experimentcanvas **webservice ingesteld** en selecteer **Web Service implementeren [New]**. De Web Service Azure Machine Learning-webservices wordt geopend aan de pagina service implementeren. Typ een naam voor uw web-service en kies een betaald abonnement en klik vervolgens op **implementeren**. Alleen de Batchuitvoering-methode kan worden gebruikt voor het maken van de Trained Models

In beide gevallen nadat het experiment is voltooid, ziet de resulterende werkstroom er als volgt:

![Resulterende workflow nadat uitgevoerd.][4]



## <a name="retrain-the-model-with-new-data-using-bes"></a>Het model met nieuwe gegevens met behulp van BES Retrain
Bijvoorbeeld, u gebruikmaakt van C# voor de retraining-toepassing maken. U kunt ook de voorbeeldcode Python of R gebruiken deze taak uit te voeren.

Om aan te roepen de Retraining API's:

1. Maak een C#-consoletoepassing in Visual Studio: **nieuw** > **Project** > **Visual C#** > **Classic Windows Desktop** > **Console-App (.NET Framework)**.
2. Aanmelden bij de portal van de Machine Learning-webservice.
3. Als u met een klassiek-webservice werkt, klikt u op **klassieke webservices**.
   1. Klik op de webservice die u met werkt.
   2. Klik op het standaardeindpunt.
   3. Klik op **verbruiken**.
   4. Aan de onderkant van de **verbruiken** pagina in de **voorbeeldcode** sectie, klikt u op **Batch**.
   5. Verder met stap 5 van deze procedure.
4. Als u met een nieuwe webservice werkt, klikt u op **webservices**.
   1. Klik op de webservice die u met werkt.
   2. Klik op **verbruiken**.
   3. Op de onderkant van de verbruiken pagina in de **voorbeeldcode** sectie, klikt u op **Batch**.
5. Kopieer de voorbeeldcode voor Batchuitvoering C# en plak deze in het bestand Program.cs om ervoor te zorgen dat de naamruimte blijft intact.

Het Nuget-pakket Microsoft.AspNet.WebApi.Client zoals opgegeven in de opmerkingen toevoegen. De verwijzing naar Microsoft.WindowsAzure.Storage.dll wilt toevoegen, moet u mogelijk eerst de clientbibliotheek voor Microsoft Azure storage-services te installeren. Zie voor meer informatie [Windows Storage-Services](https://www.nuget.org/packages/WindowsAzure.Storage).

### <a name="update-the-apikey-declaration"></a>De declaratie apikey bijwerken
Zoek de **apikey** declaratie.

    const string apiKey = "abc123"; // Replace this with the API key for the web service

In de **Basic verbruik info** sectie van de **verbruiken** pagina, Ga naar de primaire sleutel en kopieert u deze naar de **apikey** declaratie.

### <a name="update-the-azure-storage-information"></a>De informatie van de Azure Storage bijwerken
De voorbeeldcode BES uploadt een bestand vanaf een lokaal station (bijvoorbeeld 'C:\temp\CensusIpnput.csv') naar Azure Storage, verwerkt en schrijft de resultaten terug naar Azure Storage.  

Als u wilt uitvoeren van deze taak moet u de Storage-account, de sleutel en het container-gegevens ophalen voor uw opslagaccount vanuit de klassieke Azure portal en de bijbehorende waarden van de update in de code. 

1. Aanmelden bij de klassieke Azure-portal.
2. Klik in de kolom linkernavigatievenster **opslag**.
3. Selecteer in de lijst met opslagaccounts, een voor het opslaan van het retrained model.
4. Klik onder aan de pagina op **toegangssleutels beheren**.
5. Kopieer en sla de **primaire toegangssleutel** en sluit het dialoogvenster. 
6. Klik boven aan de pagina op **Containers**.
7. Selecteer een bestaande container of maak een nieuwe en sla het op.

Zoek de *StorageAccountName*, *StorageAccountKey*, en *StorageContainerName* declaraties en werk de waarden die u hebt genoteerd in de Azure-portal.

    const string StorageAccountName = "mystorageacct"; // Replace this with your Azure Storage Account name
    const string StorageAccountKey = "a_storage_account_key"; // Replace this with your Azure Storage Key
    const string StorageContainerName = "mycontainer"; // Replace this with your Azure Storage Container name

Ook moet dat het invoerbestand is beschikbaar op de locatie die u in de code opgeeft. 

### <a name="specify-the-output-location"></a>Geef de uitvoerlocatie
Wanneer u de uitvoerlocatie opgeeft in de nettolading van de aanvraag, de uitbreiding van het bestand die is opgegeven *RelativeLocation* moet worden opgegeven als ilearner. 

Zie het volgende voorbeeld:

    Outputs = new Dictionary<string, AzureBlobDataReference>() {
        {
            "output1",
            new AzureBlobDataReference()
            {
                ConnectionString = storageConnectionString,
                RelativeLocation = string.Format("{0}/output1results.ilearner", StorageContainerName) /*Replace this with the location you would like to use for your output file, and valid file extension (usually .csv for scoring results, or .ilearner for trained models)*/
            }
        },

> [!NOTE]
> De namen van de uitvoerlocaties kunnen afwijken van de referenties die in dit overzicht op basis van de volgorde waarin u de web service uitvoer modules toegevoegd. Nadat u dit trainingsexperiment met twee uitvoer hebt ingesteld, omvatten de resultaten locatiegegevens van opslag voor beide parameters.  
> 
> 

![Uitvoer retraining][6]

Diagram 4: Retraining uitvoer.

## <a name="evaluate-the-retraining-results"></a>De Retraining resultaten evalueren
Wanneer u de toepassing uitvoert, wordt de uitvoer de URL en SAS-token nodig voor toegang tot de evaluatieresultaten bevat.

U kunt de prestatieresultaten van het model retrained zien door combineren de *BaseLocation*, *RelativeLocation*, en *SasBlobToken* uit de resultaten van de uitvoer voor *output2* (zoals weergegeven in de voorgaande retraining uitvoer afbeelding) en de volledige URL in de adresbalk van de browser te plakken.  

Bekijk de resultaten om te bepalen of het zojuist getrainde model goed genoeg ter vervanging van de bestaande uitvoert.

Kopieer de *BaseLocation*, *RelativeLocation*, en *SasBlobToken* uit de resultaten uitvoer wordt u ze gebruiken tijdens de retraining.

## <a name="next-steps"></a>Volgende stappen
Als u de voorspellende webservice geïmplementeerd door te klikken op **webservice implementeren [klassieke]**, Zie [opnieuw trainen van een webservice klassieke](retrain-a-classic-web-service.md).

Als u de voorspellende webservice geïmplementeerd door te klikken op **Web Service implementeren [New]**, Zie [opnieuw trainen van de Machine Learning-cmdlets met een nieuwe webservice](retrain-new-web-service-using-powershell.md).

<!-- Retrain a New web service using the Machine Learning Management REST API -->


[1]: ./media/retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE01.png
[2]: ./media/retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE02.png
[3]: ./media/retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE03.png
[4]: ./media/retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE04.png
[5]: ./media/retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE05.png
[6]: ./media/retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE06.png
[7]: ./media/retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE07.png


<!-- Module References -->
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
