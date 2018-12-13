---
title: Machine Learning Studio modellen programmatisch opnieuw trainen - Azure | Microsoft Docs
description: Meer informatie over het programmatisch opnieuw trainen een model met C# en de service Machine Learning Batch Execution.
services: machine-learning
documentationcenter: ''
author: ericlicoding
ms.custom: seodec18
ms.author: amlstudiodocs
editor: cgronlun
ms.assetid: 7ae4f977-e6bf-4d04-9dde-28a66ce7b664
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.openlocfilehash: 7a1381ce43056607486a27710cd6ee6181b9a5c1
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2018
ms.locfileid: "53257390"
---
# <a name="retrain-azure-machine-learning-studio-models-programmatically"></a>Azure Machine Learning Studio-modellen programmatisch opnieuw trainen
In dit scenario leert u hoe u programmatisch opnieuw trainen een Azure Machine Learning Studio web service via C# en de service Machine Learning Batch Execution.

Nadat u het model hebt retrained, heeft de volgende scenario's laten zien hoe het model in uw voorspellende webservice bijwerken:

* Als u een klassieke webservice in Machine Learning-webservicesportal hebt geïmplementeerd, Zie [een klassieke webservice opnieuw trainen](retrain-a-classic-web-service.md). 
* Als u een nieuwe webservice hebt geïmplementeerd, Zie [opnieuw trainen van een nieuwe webservice met behulp van de Machine Learning Management-cmdlets](retrain-new-web-service-using-powershell.md).

Zie voor een overzicht van de retraining proces [opnieuw trainen van een Machine Learning-Model](retrain-machine-learning-model.md).

Als u beginnen met uw bestaande nieuwe Azure Resource Manager gebaseerde webservice wilt, Zie [een bestaande voorspellende webservice opnieuw trainen](retrain-existing-resource-manager-based-web-service.md).

## <a name="create-a-training-experiment"></a>Een opleidingsexperiment maken
In dit voorbeeld gaat u "voorbeeld 5: Trainen, testen, evalueren voor binaire classificatie: Volwassenen gegevensset' van de Microsoft Azure Machine Learning-voorbeelden. 

Het experiment maken:

1. Meld u aan bij Microsoft Azure Machine Learning Studio. 
2. Klik op de rechterbenedenhoek van het dashboard, **nieuw**.
3. Selecteer in de Microsoft-Samples voorbeeld 5.
4. Wijzig de naam van het experiment, aan de bovenkant van het experimentcanvas, selecteert u de naam van het experiment "voorbeeld 5: Trainen, testen, evalueren voor binaire classificatie: Volwassenen gegevensset'.
5. Type telling Model.
6. Aan de onderkant van het experimentcanvas, klikt u op **uitvoeren**.
7. Klik op **instellen webservice** en selecteer **webservice opnieuw trainen**. 

Hieronder ziet u het eerste experiment.
   
   ![Eerste experiment.][2]


## <a name="create-a-predictive-experiment-and-publish-as-a-web-service"></a>Een Voorspellend experiment maken en publiceren als een webservice
Vervolgens maakt u een Experiment Predicative.

1. Aan de onderkant van het experimentcanvas, klikt u op **webservice ingesteld** en selecteer **voorspellende webservice**. Dit Hiermee slaat u het model als een getraind Model en web service invoer en uitvoer-modules toegevoegd. 
2. Klik op **Run**. 
3. Nadat het experiment is voltooid, klikt u op **webservice implementeren [klassieke]** of **Web Service implementeren [Nieuw]**.

> [!NOTE] 
> Voor het implementeren van een nieuwe webservice moet u voldoende machtigingen hebben in het abonnement waarvoor u de webservice implementeert. Zie voor meer informatie, [beheren van een webservice met behulp van de Azure Machine Learning-webserviceportal](manage-new-webservice.md). 

## <a name="deploy-the-training-experiment-as-a-training-web-service"></a>De trainingsexperiment implementeren als een webservice Training
Als u wilt opnieuw trainen van het getrainde model, moet u het trainingsexperiment die u hebt gemaakt als een webservice Retraining implementeren. Deze webservice moet een *Web Service uitvoer* module die zijn verbonden met de *[Train Model] [ train-model]* -module, kunnen voor het produceren van nieuwe getraind modellen.

1. Als u wilt terugkeren naar het trainingsexperiment, klik op het pictogram experimenten in het linkerdeelvenster, en klik vervolgens op het experiment met de naam telling Model.  
2. Typ in het zoekvak Experiment zoekitems webservice. 
3. Sleep een *Web Service invoer* module naar het experimentcanvas en koppel de uitvoer naar de *Clean Missing Data* module.  Dit zorgt ervoor dat uw retraining gegevens op dezelfde manier als uw oorspronkelijke trainingsgegevens worden verwerkt.
4. Sleep twee *webservice uitvoer* modules naar het experimentcanvas. Koppel de uitvoer van de *Train Model* module één en de uitvoer van de *Evaluate Model* module naar het andere. De uitvoer van de web service voor **Train Model** geeft ons voor het nieuwe getrainde model. De uitvoer die is gekoppeld aan **Evaluate Model** retourneert die module-uitvoer, wordt de prestatieresultaten.
5. Klik op **Run**. 

Vervolgens moet u het trainingsexperiment implementeren als een webservice die een getraind model en de resultaten van evaluatie van model produceert. U doet dit door de volgende set acties afhankelijk zijn van of u werkt met een klassieke webservice of een nieuwe webservice betreft.  

**Klassieke webservice**

Aan de onderkant van het experimentcanvas, klikt u op **webservice ingesteld** en selecteer **webservice implementeren [klassieke]**. De webservice **Dashboard** wordt weergegeven met de API-sleutel en de API help-pagina voor de Batch niet uitvoeren. Alleen de Batch Execution-methode kan worden gebruikt voor het maken van getrainde modellen.

**Nieuwe webservice**

Aan de onderkant van het experimentcanvas, klikt u op **webservice ingesteld** en selecteer **Web Service implementeren [Nieuw]**. De Web Service Azure Machine Learning-webservices wordt geopend aan de pagina service implementeren. Typ een naam voor uw web-service en kies een abonnement aan en klik vervolgens op **implementeren**. Alleen de Batch Execution-methode kan worden gebruikt voor het maken van getrainde modellen

In beide gevallen nadat het experiment is voltooid, ziet de resulterende werkstroom er als volgt:

![Resulterende werkstroom na het uitvoeren van.][4]



## <a name="retrain-the-model-with-new-data-using-bes"></a>Opnieuw trainen van het model met nieuwe gegevens met behulp van BES
In dit voorbeeld gebruikt u C# de retraining toepassing maken. U kunt ook de voorbeeldcode voor Python of R gebruiken om deze taak te volbrengen.

Om aan te roepen de Retraining API's:

1. Maak een C# in Visual Studio-consoletoepassing: **Nieuwe** > **Project** > **Visual C#**   >  **Windows Classic Desktop**  >   **Console-App (.NET Framework)**.
2. Aanmelden bij de portal van de Machine Learning-webservice.
3. Als u met een klassieke webservice werkt, klikt u op **klassieke webservices**.
   1. Klik op de webservice die u met werkt.
   2. Klik op de standaardeindpunt.
   3. Klik op **gebruiken**.
   4. Aan de onderkant van de **verbruiken** pagina, in de **voorbeeldcode** sectie, klikt u op **Batch**.
   5. Ga verder met stap 5 van deze procedure.
4. Als u met een nieuwe webservice betreft werkt, klikt u op **webservices**.
   1. Klik op de webservice die u met werkt.
   2. Klik op **gebruiken**.
   3. Op de onderkant van het verbruiken pagina, in de **voorbeeldcode** sectie, klikt u op **Batch**.
5. Kopieer het voorbeeld C# voor de Batchuitvoering van de-code en plak deze in het bestand Program.cs te zorgen dat de naamruimte blijft intact.

Nuget-pakket Microsoft.AspNet.WebApi.Client zoals opgegeven in de opmerkingen toevoegen. De verwijzing naar Microsoft.WindowsAzure.Storage.dll wilt toevoegen, moet u mogelijk eerst de clientbibliotheek voor Microsoft Azure storage-services te installeren. Zie voor meer informatie, [Windows Storage-Services](https://www.nuget.org/packages/WindowsAzure.Storage).

### <a name="update-the-apikey-declaration"></a>De declaratie apikey bijwerken
Zoek de **apikey** declaratie.

    const string apiKey = "abc123"; // Replace this with the API key for the web service

In de **Basic verbruik info** sectie van de **verbruiken** pagina, Ga naar de primaire sleutel en kopieert u het naar de **apikey** declaratie.

### <a name="update-the-azure-storage-information"></a>De Azure Storage-gegevens bijwerken
De BES-voorbeeldcode wordt een bestand vanaf een lokaal station (bijvoorbeeld 'C:\temp\CensusIpnput.csv') naar Azure Storage geüpload, verwerkt en schrijft de resultaten terug naar Azure Storage.  

Als u wilt deze taak wordt uitgevoerd, moet u de opslag account naam, sleutel en container-informatie ophalen voor uw opslagaccount in de klassieke Azure-portal en de bijbehorende waarden van de update in de code. 

1. Meld u aan bij de klassieke Azure portal.
2. Klik in de linkernavigatiekolom op **opslag**.
3. Selecteer in de lijst met opslagaccounts, een voor het opslaan van het retrained model.
4. Aan de onderkant van de pagina, klikt u op **toegangssleutels beheren**.
5. Kopieer en bewaar de **primaire toegangssleutel** en sluit het dialoogvenster. 
6. Aan de bovenkant van de pagina, klikt u op **Containers**.
7. Selecteer een bestaande container of een nieuwe maken en opslaan van de naam.

Zoek de *StorageAccountName*, *StorageAccountKey*, en *StorageContainerName* declaraties en werk de waarden die u hebt opgeslagen in Azure portal.

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
> De namen van de uitvoerlocaties kunnen afwijken van de namen in dit scenario op basis van de volgorde waarin u de web service uitvoer-modules toegevoegd. Omdat u deze trainingsexperiment met twee uitvoer hebt ingesteld, bevatten de resultaten locatiegegevens opslag voor beide.  
> 
> 

![Uitvoer opnieuw trainen][6]

Diagram 4: Opnieuw trainen uitvoer.

## <a name="evaluate-the-retraining-results"></a>De Retraining resultaten evalueren
Wanneer u de toepassing uitvoert, wordt in de uitvoer de URL en SAS-token nodig voor toegang tot de resultaten van evaluatie van bevat.

U kunt de prestatieresultaten van het model retrained zien door combineren de *BaseLocation*, *RelativeLocation*, en *SasBlobToken* vanuit de resultaten van de uitvoer voor *output2* (zoals weergegeven in de voorgaande retraining uitvoer afbeelding) en de volledige URL plakken in de adresbalk van de browser.  

Bekijk de resultaten om te bepalen of het zojuist getrainde model goed genoeg wordt uitgevoerd op het bestaande bestand vervangen.

Kopieer de *BaseLocation*, *RelativeLocation*, en *SasBlobToken* vanuit de resultaten van de uitvoer wordt u deze gebruiken tijdens de retraining.

## <a name="next-steps"></a>Volgende stappen
Als u de voorspellende webservice geïmplementeerd door te klikken op **webservice implementeren [klassieke]**, Zie [een klassieke webservice opnieuw trainen](retrain-a-classic-web-service.md).

Als u de voorspellende webservice geïmplementeerd door te klikken op **Web Service implementeren [Nieuw]**, Zie [opnieuw trainen van een nieuwe webservice met behulp van de Machine Learning Management-cmdlets](retrain-new-web-service-using-powershell.md).

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
