---
title: 'Quickstart: Detecteer afwijkingen van gegevens met behulp van de bibliotheek voor detectie van afwijkingen en Python'
titleSuffix: Azure Cognitive Services
description: Gebruik de API van de detectie van afwijkingen voor het detecteren van afwijkingen in de reeks als een batch of voor streaming-gegevens.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: anomaly-detector
ms.topic: quickstart
ms.date: 07/01/2019
ms.author: aahi
ms.openlocfilehash: 9176ab84dd3f493604bd655e0498f5ad476776d0
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2019
ms.locfileid: "67721526"
---
# <a name="quickstart-anomaly-detector-client-library-for-python"></a>Quickstart: Anomaliedetectie Detector-clientbibliotheek voor Python

Aan de slag met de detectie van afwijkingen-clientbibliotheek voor .NET. Volg deze stappen voor het pakket installeren en uit te proberen de voorbeeldcode voor eenvoudige taken. De detectie van afwijkingen-service kunt u zoeken naar afwijkingen in uw time series-gegevens met behulp van de best passende modellen automatisch, ongeacht de branche, scenario of gegevensvolume.

## <a name="key-concepts"></a>Belangrijkste concepten

De detectie van afwijkingen-clientbibliotheek voor Python om te gebruiken:

* Detecteer afwijkingen in uw time series-gegevensset als een batchaanvraag voor
* De status van afwijkingen van de meest recente gegevenspunt in de tijdreeks detecteren

[Bibliotheek referentiedocumentatie voor](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector?view=azure-python) | [broncode clientbibliotheek](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cognitiveservices/azure-cognitiveservices-anomalydetector) | [pakket (PyPi)](https://pypi.org/project/azure-cognitiveservices-anomalydetector/) | [voorbeelden](https://github.com/Azure-Samples/anomalydetector)

## <a name="prerequisites"></a>Vereisten

* Azure-abonnement: [maakt u er een gratis](https://azure.microsoft.com/free/)
* [Python 3.x](https://www.python.org/)
* De [Pandas gegevens analysebibliotheek](https://pandas.pydata.org/)
 
## <a name="setting-up"></a>Instellen

### <a name="create-an-anomaly-detector-resource"></a>Maak een resource Anomaliedetectie Detector

[!INCLUDE [anomaly-detector-resource-creation](../../../../includes/cognitive-services-anomaly-detector-resource-cli.md)]

### <a name="install-the-client-library"></a>Installeren van de clientbibliotheek

Na de installatie van Python, kunt u de clientbibliotheek met installeren:

```console
pip install --upgrade azure-cognitiveservices-anomalydetector
```

## <a name="object-model"></a>Objectmodel

De detectie van afwijkingen-client is een [AnomalyDetectorClient](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.anomalydetectorclient?view=azure-python) -object dat wordt geverifieerd bij Azure met behulp van de sleutel. De client biedt twee methoden voor het detecteren van afwijkingen: Over het gebruik van een volledige gegevensset [entire_detect()](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.anomalydetectorclient?view=azure-python#entire-detect-body--custom-headers-none--raw-false----operation-config-), en op de meest recente gegevens verwijzen met behulp van [Last_detect()](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.anomalydetectorclient?view=azure-python#last-detect-body--custom-headers-none--raw-false----operation-config-). 

Time series-gegevens wordt verzonden als een reeks [punten](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.point(class)?view=azure-python) in een [aanvragen](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.request(class)?view=azure-python) object. De `Request` -object bevat eigenschappen om te beschrijven van de gegevens ([granulariteit](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.granularity?view=azure-python) bijvoorbeeld), en parameters voor de detectie van afwijkingen. 

Het antwoord van de detectie van afwijkingen is een [LastDetectResponse](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.lastdetectresponse?view=azure-python) of [EntireDetectResponse](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.entiredetectresponse?view=azure-python) afhankelijk van de methode die wordt gebruikt. 

## <a name="getting-started"></a>Aan de slag

Maak een nieuwe Python-toepassing in de gewenste editor of IDE. Voeg de volgende import declaraties toe aan het bestand. 

[!code-python[import declarations](~/samples-anomaly-detector/quickstarts/sdk/python-sdk-sample.py?name=imports)]

> [!NOTE]
> In deze snelstartgids wordt ervan uitgegaan dat u hebt [gemaakt van een omgevingsvariabele](../../cognitive-services-apis-create-account.md#configure-an-environment-variable-for-authentication) voor uw sleutel Anomaliedetectie Detector met de naam `ANOMALY_DETECTOR_KEY`.

Variabelen voor de sleutel als een omgevingsvariabele, het pad naar een bestand met time series en de azure-locatie van uw abonnement maken. Bijvoorbeeld `westus2`. 

[!code-python[Vars for the key, path location and data path](~/samples-anomaly-detector/quickstarts/sdk/python-sdk-sample.py?name=initVars)]

## <a name="code-examples"></a>Voorbeelden van code 

Deze codefragmenten laten zien hoe u het volgende doen met de detectie van afwijkingen-clientbibliotheek voor .NET:

* [Verifiëren van de client](#authenticate-the-client)
* [Een set time series gegevens uit een bestand laden](#load-time-series-data-from-a-file)
* [Detecteer afwijkingen in de hele gegevensset](#detect-anomalies-in-the-entire-data-set) 
* [De status van afwijkingen van de meest recente gegevenspunt detecteren](#detect-the-anomaly-status-of-the-latest-data-point)

### <a name="authenticate-the-client"></a>Verifiëren van de client

De variabele van uw azure-locatie toevoegen aan het eindpunt en de client te verifiëren met uw sleutel.

[!code-python[Client authentication](~/samples-anomaly-detector/quickstarts/sdk/python-sdk-sample.py?name=client)]

### <a name="load-time-series-data-from-a-file"></a>Time series-gegevens laden uit een bestand

Download de voorbeeldgegevens voor deze Quick Start uit [GitHub](https://github.com/Azure-Samples/AnomalyDetector/blob/master/example-data/request-data.csv):
1. In uw browser, met de rechtermuisknop op **Raw**
2. Klik op **koppeling opslaan als**
3. Sla het bestand naar de toepassingsmap als een CSV-bestand.

Deze time series-gegevens zijn opgemaakt als een CSV-bestand en worden verzonden naar de detectie van afwijkingen API.

Laden van het gegevensbestand met de Pandas-bibliotheek `read_csv()` methode en maak een lege lijst variabele voor het opslaan van de gegevensreeks. Doorlopen van het bestand en toevoegen van de gegevens op als een [punt](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.point%28class%29?view=azure-python) object. Dit object bevat de tijdstempel en een numerieke waarde van de rijen van uw CSV-gegevensbestand. 

[!code-python[Load the data file](~/samples-anomaly-detector/quickstarts/sdk/python-sdk-sample.py?name=loadDataFile)]

Maakt een [aanvragen](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.request%28class%29?view=azure-python) object met de tijdreeksen en de [granulariteit](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.granularity?view=azure-python) (of periodiciteit) van de gegevenspunten. Bijvoorbeeld `Granularity.daily`.

[!code-python[Create the request object](~/samples-anomaly-detector/quickstarts/sdk/python-sdk-sample.py?name=request)]

### <a name="detect-anomalies-in-the-entire-data-set"></a>Detecteer afwijkingen in de hele gegevensset 

De API voor het detecteren van afwijkingen in de hele time series-gegevens met behulp van de client aanroepen [entire_detect()](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.anomalydetectorclient?view=azure-python#entire-detect-body--custom-headers-none--raw-false----operation-config-) methode. De geretourneerde Store [EntireDetectResponse](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.entiredetectresponse?view=azure-python) object. Doorlopen van het antwoord `is_anomaly` weergeven en afdrukken van de index van een `true` waarden. Deze waarden overeenkomen met de index van afwijkende gegevenspunten, als deze zijn gevonden.

[!code-python[Batch anomaly detection sample](~/samples-anomaly-detector/quickstarts/sdk/python-sdk-sample.py?name=detectAnomaliesBatch)]

### <a name="detect-the-anomaly-status-of-the-latest-data-point"></a>De status van afwijkingen van de meest recente gegevenspunt detecteren

De Anomaliedetectie Detector API om te bepalen of uw laatste gegevenspunt een anomalie met behulp van de client aanroepen [last_detect()](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.anomalydetectorclient?view=azure-python#last-detect-body--custom-headers-none--raw-false----operation-config-) methode en de geretourneerde store [LastDetectResponse](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.lastdetectresponse?view=azure-python) object. Van het antwoord `is_anomaly` waarde is een Booleaanse waarde waarmee wordt aangegeven dat punt van afwijkingen status.  

[!code-python[Batch anomaly detection sample](~/samples-anomaly-detector/quickstarts/sdk/python-sdk-sample.py?name=latestPointDetection)]

## <a name="run-the-application"></a>De toepassing uitvoeren

De toepassing uitvoert in uw IDE of in de opdrachtregel met de `python` opdracht en de bestandsnaam van uw.
 
## <a name="clean-up-resources"></a>Resources opschonen

Als u wilt wissen en verwijderen van een Cognitive Services-abonnement, kunt u de resource of resourcegroep verwijderen. Als u de resourcegroep verwijdert, verwijdert u ook alle andere resources die zijn gekoppeld aan de resourcegroep.

* [Portal](../../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure-CLI](../../cognitive-services-apis-create-account-cli.md#clean-up-resources)

U kunt ook de volgende cloud shell-opdracht om de resourcegroep en alle bijbehorende resources te verwijderen uitvoeren. Dit kan enkele minuten duren. 

```azurecli-interactive
az group delete --name example-anomaly-detector-resource-group
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
>[Streaming afwijkingsdetectie met Azure Databricks](../tutorials/anomaly-detection-streaming-databricks.md)

* Wat is de [Anomaliedetectie Detector API?](../overview.md)
* [Aanbevolen procedures](../concepts/anomaly-detection-best-practices.md) wanneer u de API van de detectie van afwijkingen.
* De broncode voor dit voorbeeld is te vinden op [GitHub](https://github.com/Azure-Samples/AnomalyDetector/blob/master/quickstarts/sdk/csharp-sdk-sample.cs).
