---
title: 'Quickstart: Detecteer afwijkingen in time series-gegevens met behulp van de detectie van afwijkingen SDK voor .NET'
titleSuffix: Azure Cognitive Services
description: Beginnen met het detecteren van afwijkingen in uw time series-gegevens met de detectie van afwijkingen-service.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: anomaly-detector
ms.topic: quickstart
ms.date: 07/01/2019
ms.author: aahi
ms.openlocfilehash: a75196e035585a7501cdd842fb5b80ceff424dcc
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2019
ms.locfileid: "67721574"
---
# <a name="quickstart-anomaly-detector-client-library-for-net"></a>Quickstart: Anomaliedetectie Detector-clientbibliotheek voor .NET

Aan de slag met de detectie van afwijkingen-clientbibliotheek voor .NET. Volg deze stappen voor het pakket installeren en uit te proberen de voorbeeldcode voor eenvoudige taken. De detectie van afwijkingen-service kunt u zoeken naar afwijkingen in uw time series-gegevens met behulp van de best passende modellen automatisch, ongeacht de branche, scenario of gegevensvolume.

De detectie van afwijkingen-clientbibliotheek voor .NET te gebruiken:

* Detecteer afwijkingen als batch
* De status van afwijkingen van de meest recente gegevenspunt detecteren

[API-referentiedocumentatie](https://docs.microsoft.com/dotnet/api/Microsoft.Azure.CognitiveServices.AnomalyDetector?view=azure-dotnet-preview) | [broncode clientbibliotheek](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/AnomalyDetector) | [pakket (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.AnomalyDetector/) | [voorbeelden](https://github.com/Azure-Samples/anomalydetector)

## <a name="prerequisites"></a>Vereisten

* Azure-abonnement: [maakt u er een gratis](https://azure.microsoft.com/free/)
* De huidige versie van [.NET Core](https://dotnet.microsoft.com/download/dotnet-core)

## <a name="setting-up"></a>Instellen

### <a name="create-an-anomaly-detector-resource"></a>Maak een resource Anomaliedetectie Detector

[!INCLUDE [anomaly-detector-resource-creation](../../../../includes/cognitive-services-anomaly-detector-resource-cli.md)]

### <a name="create-a-new-c-app"></a>Maak een nieuwe C# app

Maak een nieuwe .NET Core-toepassing in de gewenste editor of IDE. 

In een consolevenster (zoals cmd, PowerShell of Bash), gebruikt u de dotnet `new` opdracht voor het maken van een nieuwe console-app met de naam van de `anomaly-detector-quickstart`. Deze opdracht maakt u een eenvoudige 'Hallo wereld" C# project met een één-bronbestand: **Program.cs**. 

```console
dotnet new console -n anomaly-detector-quickstart
```

Wijzig de directory naar de zojuist gemaakte app-map. U kunt de toepassing met het bouwen:

```console
dotnet build
```

Uitvoer van de build mag geen fouten of waarschuwingen worden gegenereerd. 

```console
...
Build succeeded.
 0 Warning(s)
 0 Error(s)
...
```

### <a name="install-the-client-library"></a>Installeren van de clientbibliotheek

In de toepassingsmap de detectie van afwijkingen-clientbibliotheek voor .NET te installeren met de volgende opdracht:

```console
dotnet add package Microsoft.Azure.CognitiveServices.AnomalyDetector --version 0.8.0-preview
```

Als u de Visual Studio IDE, is de clientbibliotheek beschikbaar als een NuGet-pakket. 

## <a name="object-model"></a>Objectmodel

De detectie van afwijkingen-client is een [AnomalyDetectorClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.anomalydetectorclient) -object dat wordt geverifieerd bij Azure met [ApiKeyServiceClientCredentials](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.apikeyserviceclientcredentials), die uw sleutel bevat. De client biedt twee methoden voor het detecteren van afwijkingen: Over het gebruik van een volledige gegevensset [EntireDetectAsync()](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.anomalydetectorclientextensions.entiredetectasync), en op de meest recente gegevens verwijzen met behulp van [LastDetectAsync()](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.anomalydetectorclientextensions.lastdetectasync). 

Time series-gegevens wordt verzonden als een reeks [punten](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.models.request.series?view=azure-dotnet-preview#Microsoft_Azure_CognitiveServices_AnomalyDetector_Models_Request_Series) in een [aanvragen](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.models.request) object. De `Request` -object bevat eigenschappen om te beschrijven van de gegevens ([granulariteit](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.models.request.granularity) bijvoorbeeld), en parameters voor de detectie van afwijkingen. 

Het antwoord van de detectie van afwijkingen is ofwel een [EntireDetectResponse](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.models.entiredetectresponse) of [LastDetectResponse](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.models.lastdetectresponse) object, afhankelijk van de methode die wordt gebruikt. 

## <a name="code-examples"></a>Voorbeelden van code

Deze codefragmenten laten zien hoe u het volgende doen met de detectie van afwijkingen-clientbibliotheek voor .NET:

* [Verifiëren van de client](#authenticate-the-client)
* [Een set time series gegevens uit een bestand laden](#load-time-series-data-from-a-file)
* [Detecteer afwijkingen in de hele gegevensset](#detect-anomalies-in-the-entire-data-set) 
* [De status van afwijkingen van de meest recente gegevenspunt detecteren](#detect-the-anomaly-status-of-the-latest-data-point)

### <a name="add-the-main-method"></a>De belangrijkste methode toevoegen

In de projectmap:

1. Open het bestand Program.cs in de gewenste editor of IDE
2. Voeg de volgende `using` richtlijnen

[!code-csharp[using statements](~/samples-anomaly-detector/quickstarts/sdk/csharp-sdk-sample.cs?name=usingStatements)]

> [!NOTE]
> In deze snelstartgids wordt ervan uitgegaan dat u hebt [gemaakt van een omgevingsvariabele](../../cognitive-services-apis-create-account.md#configure-an-environment-variable-for-authentication) voor uw sleutel Anomaliedetectie Detector met de naam `ANOMALY_DETECTOR_KEY`.

In de toepassingsmap `main()` methode variabelen voor de Azure-locatie van uw resource en de sleutel als een omgevingsvariabele maken. Als u de omgevingsvariabele gemaakt nadat de toepassing wordt gestart, moet de editor, IDE of shell die wordt uitgevoerd om te worden gesloten en opnieuw geladen voor toegang tot de variabele.

[!code-csharp[Main method](~/samples-anomaly-detector/quickstarts/sdk/csharp-sdk-sample.cs?name=mainMethod)]

### <a name="authenticate-the-client"></a>Verifiëren van de client

In een nieuwe methode exemplaar maken van een client met het eindpunt en de sleutel. Maak een [ApiKeyServiceClientCredentials](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.apikeyserviceclientcredentials?view=azure-dotnet-preview) object met de sleutel, en deze gebruiken met uw eindpunt te maken van een [AnomalyDetectorClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.anomalydetectorclient?view=azure-dotnet-preview) object. 

[!code-csharp[Client authentication function](~/samples-anomaly-detector/quickstarts/sdk/csharp-sdk-sample.cs?name=createClient)]
    
### <a name="load-time-series-data-from-a-file"></a>Time series-gegevens laden uit een bestand

Download de voorbeeldgegevens voor deze Quick Start uit [GitHub](https://github.com/Azure-Samples/AnomalyDetector/blob/master/example-data/request-data.csv):
1. In uw browser, met de rechtermuisknop op **Raw**
2. Klik op **koppeling opslaan als**
3. Sla het bestand naar de toepassingsmap als een CSV-bestand.

Deze time series-gegevens zijn opgemaakt als een CSV-bestand en worden verzonden naar de detectie van afwijkingen API.

Maak een nieuwe methode om te lezen in de time series-gegevens en toe te voegen aan een [aanvragen](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.models.request?view=azure-dotnet-preview) object. Bel `File.ReadAllLines()` met het bestandspad en maak een lijst van [punt](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.models.point?view=azure-dotnet-preview) objecten en alle nieuwe strook / lijn tekens. De waarden ophalen en de datumstempel scheiden van de numerieke waarde en toe te voegen aan een nieuwe `Point` object. 

Controleer een `Request` object met de reeks van punten en `Granularity.Daily` voor de [granulariteit](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.models.granularity?view=azure-dotnet-preview) (of periodiciteit) van de gegevenspunten.

[!code-csharp[load the time series data file](~/samples-anomaly-detector/quickstarts/sdk/csharp-sdk-sample.cs?name=GetSeriesFromFile)]

### <a name="detect-anomalies-in-the-entire-data-set"></a>Detecteer afwijkingen in de hele gegevensset 

Maken van een methode voor het aanroepen van de client [EntireDetectAsync()](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.anomalydetectorclientextensions.entiredetectasync?view=azure-dotnet-preview#Microsoft_Azure_CognitiveServices_AnomalyDetector_AnomalyDetectorClientExtensions_EntireDetectAsync_Microsoft_Azure_CognitiveServices_AnomalyDetector_IAnomalyDetectorClient_Microsoft_Azure_CognitiveServices_AnomalyDetector_Models_Request_System_Threading_CancellationToken_) methode met de `Request` object en wacht tot de reactie als een [EntireDetectResponse](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.models.entiredetectresponse?view=azure-dotnet-preview) object. Als de tijdreeks eventuele afwijkingen bevat, doorlopen van het antwoord [IsAnomaly](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.models.entiredetectresponse.isanomaly?view=azure-dotnet-preview) waarden en printers worden `true`. Deze waarden overeenkomen met de index van afwijkende gegevenspunten, als deze zijn gevonden.

[!code-csharp[EntireDetectSampleAsync() function](~/samples-anomaly-detector/quickstarts/sdk/csharp-sdk-sample.cs?name=entireDatasetExample)]

### <a name="detect-the-anomaly-status-of-the-latest-data-point"></a>De status van afwijkingen van de meest recente gegevenspunt detecteren

Maken van een methode voor het aanroepen van de client [LastDetectAsync()](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.anomalydetectorclientextensions.lastdetectasync?view=azure-dotnet-preview#Microsoft_Azure_CognitiveServices_AnomalyDetector_AnomalyDetectorClientExtensions_LastDetectAsync_Microsoft_Azure_CognitiveServices_AnomalyDetector_IAnomalyDetectorClient_Microsoft_Azure_CognitiveServices_AnomalyDetector_Models_Request_System_Threading_CancellationToken_) methode met de `Request` object en wacht tot de reactie als een [LastDetectResponse](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.models.lastdetectresponse?view=azure-dotnet-preview) object. Controleren van het antwoord [IsAnomaly](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.models.lastdetectresponse.isanomaly?view=azure-dotnet-preview) kenmerk om te bepalen of de meest recente gegevenspunt verzonden een anomalie of niet is. 

[!code-csharp[LastDetectSampleAsync() function](~/samples-anomaly-detector/quickstarts/sdk/csharp-sdk-sample.cs?name=latestPointExample)]

## <a name="run-the-application"></a>De toepassing uitvoeren

Voer de toepassing uit met de dotnet `run` opdracht uit vanaf de toepassingsmap.

```dotnet
dotnet run
```

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
