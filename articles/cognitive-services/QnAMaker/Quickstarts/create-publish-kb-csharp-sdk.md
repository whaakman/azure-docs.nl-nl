---
title: 'Quickstart: QnA Maker-clientbibliotheek voor .NET'
titlesuffix: Azure Cognitive Services
description: Aan de slag met de QnA Maker-clientbibliotheek voor .NET. Volg deze stappen voor het pakket installeren en uit te proberen de voorbeeldcode voor eenvoudige taken.  Met QnA Maker kunt u een vraag- en antwoordservice maken op basis van uw semi-gestructureerde inhoud zoals FAQ-documenten, URL's en producthandleidingen.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: quickstart
ms.date: 07/10/2019
ms.author: diberry
ms.openlocfilehash: f38cbfa0efd3b9be9ca091942dca7ffcd91b6019
ms.sourcegitcommit: 441e59b8657a1eb1538c848b9b78c2e9e1b6cfd5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67828118"
---
# <a name="quickstart-qna-maker-client-library-for-net"></a>Quickstart: QnA Maker-clientbibliotheek voor .NET

Aan de slag met de QnA Maker-clientbibliotheek voor .NET. Volg deze stappen voor het pakket installeren en uit te proberen de voorbeeldcode voor eenvoudige taken.  Met QnA Maker kunt u een vraag- en antwoordservice maken op basis van uw semi-gestructureerde inhoud zoals FAQ-documenten, URL's en producthandleidingen. 

De QnA Maker-clientbibliotheek voor .NET te gebruiken:

* Een kennisdatabase maken 
* Een kennisdatabase beheren
* Een kennisdatabase publiceren

[Referentiedocumentatie](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker?view=azure-dotnet) | [broncode clientbibliotheek](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/Knowledge.QnAMaker) | [pakket (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Knowledge.QnAMaker/)  |  [ C# voorbeelden](https://github.com/Azure-Samples/cognitive-services-qnamaker-csharp)

## <a name="prerequisites"></a>Vereisten

* Azure-abonnement: [maakt u er een gratis](https://azure.microsoft.com/free/)
* De huidige versie van [.NET Core](https://dotnet.microsoft.com/download/dotnet-core).

## <a name="setting-up"></a>Instellen

### <a name="create-a-qna-maker-azure-resource"></a>Een QnA Maker Azure-resource maken

Azure Cognitive Services worden vertegenwoordigd door de Azure-resources die u zich abonneert op. Een resource maken voor QnA Maker met behulp van de [Azure-portal](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) of [Azure CLI](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account-cli) op uw lokale computer. 

Nadat u een sleutel uit uw resource [maken van een omgevingsvariabele](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) voor de sleutel met de naam `QNAMAKER_SUBSCRIPTION_KEY`.

### <a name="create-a-new-c-application"></a>Maak een nieuwe C# toepassing

Maak een nieuwe .NET Core-toepassing in de gewenste editor of IDE. 

In een consolevenster (zoals cmd, PowerShell of Bash), gebruikt u de dotnet `new` opdracht voor het maken van een nieuwe console-app met de naam van de `qna-maker-quickstart`. Deze opdracht maakt u een eenvoudige 'Hallo wereld" C# project met een één-bronbestand: `Program.cs`. 

```console
dotnet new console -n qna-maker-quickstart
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


### <a name="install-the-sdk"></a>De SDK installeren

In de toepassingsmap de QnA Maker-clientbibliotheek voor .NET te installeren met de volgende opdracht:

```console
dotnet add package Microsoft.Azure.CognitiveServices.Knowledge.QnAMaker --version 1.0.0
```

Als u de Visual Studio IDE, is de clientbibliotheek beschikbaar als een downloadbare NuGet-pakket.


## <a name="object-model"></a>Objectmodel

De QnA Maker-client is een [QnAMakerClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.qnamakerclient?view=azure-dotnet) -object dat wordt geverifieerd bij Azure met behulp van Microsoft.Rest.ServiceClientCredentials, dat de sleutel bevat.

Nadat de client is gemaakt, gebruikt u de [Knowledge base](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.qnamakerclient.knowledgebase?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Knowledge_QnAMaker_QnAMakerClient_Knowledgebase) eigenschap maken, beheren en publiceren van uw knowledge base. 

Uw knowledge base beheren met het verzenden van een JSON-object. Voor directe bewerkingen retourneert een methode doorgaans een JSON-object die status aangeeft. Voor langlopende bewerkingen is het antwoord de bewerking-ID. Roep de [client. Operations.GetDetailsAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.operationsextensions.getdetailsasync?view=azure-dotnet) methode met de bewerkings-ID om te bepalen de [status van de aanvraag](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.operationstatetype?view=azure-dotnet). 

 
## <a name="code-examples"></a>Voorbeelden van code

Deze codefragmenten laten zien hoe u het volgende doen met de QnA Maker-clientbibliotheek voor .NET:

* [Een kennisdatabase maken](#create-a-knowledge-base)
* [Een kennisdatabase bijwerken](#update-a-knowledge-base)
* [Een kennisdatabase downloaden](#download-a-knowledge-base)
* [Een kennisdatabase publiceren](#publish-a-knowledge-base)
* [Een kennisdatabase verwijderen](#delete-a-knowledge-base)
* [Status van een bewerking ophalen](#get-status-of-an-operation)

## <a name="add-the-dependencies"></a>De afhankelijkheden toevoegen

Open in de projectmap, de **Program.cs** -bestand in de gewenste editor of IDE. Vervang de bestaande `using` code met de volgende `using` richtlijnen:

[!code-csharp[Using statements](~/samples-qnamaker-csharp/documentation-samples/quickstarts/Knowledgebase_Quickstart/Program.cs?name=Dependencies)]

## <a name="authenticate-the-client"></a>Verifiëren van de client

In de **belangrijkste** methode, maak een variabele voor de Azure sleutel van uw resource opgehaald uit een omgevingsvariabele met de naam `QNAMAKER_SUBSCRIPTION_KEY`. Als u de omgevingsvariabele gemaakt nadat de toepassing wordt gestart, moet de editor, IDE of shell die wordt uitgevoerd om te worden gesloten en opnieuw geladen voor toegang tot de variabele. De methoden wordt later opnieuw worden gemaakt.

Maak vervolgens een [ApiKeyServiceClientCredentials](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.apikeyserviceclientcredentials?view=azure-dotnet) object met de sleutel, en deze gebruiken met uw eindpunt te maken van een [QnAMakerClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.qnamakerclient?view=azure-dotnet) object.

Als de sleutel niet in de `westus` regio, als deze voorbeeldcode bevat, wijzigt u de locatie voor de **eindpunt** variabele. Deze locatie is gevonden op de **overzicht** -pagina voor uw QnA Maker-resource in Azure portal.

[!code-csharp[Authorization to resource key](~/samples-qnamaker-csharp/documentation-samples/quickstarts/Knowledgebase_Quickstart/Program.cs?name=Authorization)]

## <a name="create-a-knowledge-base"></a>Een kennisdatabase maken

Een kennisdatabase slaat paren van vragen en antwoorden voor de [CreateKbDTO](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.createkbdto?view=azure-dotnet) object op basis van drie bronnen:

* Voor **redactionele inhoud**, gebruikt u de [QnADTO](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.qnadto?view=azure-dotnet) object.
* Voor **bestanden**, gebruikt u de [FileDTO](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.filedto?view=azure-dotnet) object. 
* Voor **URL's**, gebruikt u een lijst met tekenreeksen.

Bel de [CreateAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.knowledgebaseextensions.createasync?view=azure-dotnet) methode geeft u de geretourneerde bewerkings-ID aan de [MonitorOperation](#get-status-of-an-operation) methode om te pollen voor status. 

De laatste regel van de volgende code retourneert de knowledge base-ID van het antwoord van MonitorOoperation. 

[!code-csharp[Create a knowledge base](~/samples-qnamaker-csharp/documentation-samples/quickstarts/Knowledgebase_Quickstart/Program.cs?name=CreateKB&highlight=29,30)]

## <a name="update-a-knowledge-base"></a>Een kennisdatabase bijwerken

U kunt een kennisdatabase bijwerken door te geven in het knowledge base-ID en een [UpdatekbOperationDTO](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.updatekboperationdto?view=azure-dotnet) met [toevoegen](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.updatekboperationdtoadd?view=azure-dotnet), [bijwerken](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.updatekboperationdtoupdate?view=azure-dotnet), en [verwijderen](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.updatekboperationdtodelete?view=azure-dotnet)DTO objecten naar de [UpdateAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.knowledgebaseextensions.updateasync?view=azure-dotnet) methode. Gebruik de [MonitorOperation](#get-status-of-an-operation) methode om te bepalen als de update is voltooid.

[!code-csharp[Update a knowledge base](~/samples-qnamaker-csharp/documentation-samples/quickstarts/Knowledgebase_Quickstart/Program.cs?name=UpdateKB&highlight=4,13)]

## <a name="download-a-knowledge-base"></a>Een kennisdatabase downloaden

Gebruik de [DownloadAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.knowledgebaseextensions.downloadasync?view=azure-dotnet) methode voor het downloaden van de database als een lijst met [QnADocumentsDTO](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.qnadocumentsdto?view=azure-dotnet). Dit is _niet_ gelijk is aan de QnA Maker-portal-export uit de **instellingen** pagina omdat het resultaat van deze methode niet een TSV-bestand is.

[!code-csharp[Download a knowledge base](~/samples-qnamaker-csharp/documentation-samples/quickstarts/Knowledgebase_Quickstart/Program.cs?name=DownloadKB&highlight=2)]

## <a name="publish-a-knowledge-base"></a>Een kennisdatabase publiceren

Publiceer de knowledge base met behulp van de [PublishAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.knowledgebaseextensions.publishasync?view=azure-dotnet) methode. Dit duurt het huidige opgeslagen en het getrainde model, waarnaar wordt verwezen door de knowledge base-ID, en die op een eindpunt publiceert. 

[!code-csharp[Publish a knowledge base](~/samples-qnamaker-csharp/documentation-samples/quickstarts/Knowledgebase_Quickstart/Program.cs?name=PublishKB&highlight=2)]

## <a name="delete-a-knowledge-base"></a>Een knowledge base verwijderen

Verwijderen van de knowledge base met behulp van de [DeleteAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.knowledgebaseextensions.deleteasync?view=azure-dotnet) methode met een parameter van de knowledge base-ID. 

[!code-csharp[Delete a knowledge base](~/samples-qnamaker-csharp/documentation-samples/quickstarts/Knowledgebase_Quickstart/Program.cs?name=DeleteKB&highlight=2)]

## <a name="get-status-of-an-operation"></a>Status van een bewerking ophalen

Sommige methoden, zoals maken en bijwerken, kan duren voordat voldoende tijd die niet afhankelijk van het proces is voltooid, een [bewerking](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.operation?view=azure-dotnet) wordt geretourneerd. Gebruik de [bewerkings-ID](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.operation.operationid?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Knowledge_QnAMaker_Models_Operation_OperationId) van de bewerking voor het pollen (met de logica voor opnieuw proberen) om de status van de oorspronkelijke methode te bepalen. 

De _lus_ en _Task.Delay_ in het volgende codeblok worden gebruikt voor het simuleren van logica voor opnieuw proberen. Deze moeten worden vervangen door uw eigen logica voor opnieuw proberen. 

[!code-csharp[Monitor an operation](~/samples-qnamaker-csharp/documentation-samples/quickstarts/Knowledgebase_Quickstart/Program.cs?name=MonitorOperation&highlight=10)]

## <a name="run-the-application"></a>De toepassing uitvoeren

Voer de toepassing uit met de dotnet `run` opdracht uit vanaf de toepassingsmap.

```dotnet
dotnet run
```

## <a name="clean-up-resources"></a>Resources opschonen

Als u wilt wissen en verwijderen van een Cognitive Services-abonnement, kunt u de resource of resourcegroep verwijderen. Als u de resourcegroep, verwijdert u ook alle andere resources die ermee verbonden zijn.

* [Portal](../../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure-CLI](../../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
>[Zelfstudie: Maken en een KB beantwoorden](../tutorials/create-publish-query-in-portal.md)

* [Wat is de QnA Maker-API?](../Overview/overview.md)
* [Een kennisdatabase bewerken](../how-to/edit-knowledge-base.md)
* [Gebruiksanalyse ophalen](../how-to/get-analytics-knowledge-base.md)
* De broncode voor dit voorbeeld is te vinden op [GitHub](https://github.com/Azure-Samples/cognitive-services-qnamaker-csharp/blob/master/documentation-samples/quickstarts/Knowledgebase_Quickstart/Program.cs).