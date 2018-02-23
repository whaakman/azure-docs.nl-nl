---
title: Uw eerste functie maken in Azure met behulp van Visual Studio | Microsoft Docs
description: Maak een eenvoudige HTTP-geactiveerde functie en publiceer deze op Azure met behulp van Azure Functions-hulpprogramma's voor Visual Studio.
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
keywords: azure-functies, functies, gebeurtenisverwerking, berekenen, architectuur zonder server
ms.assetid: 82db1177-2295-4e39-bd42-763f6082e796
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 01/17/2018
ms.author: glenga
ms.custom: mvc, devcenter
ms.openlocfilehash: f908b874a8205c2b8738deeccb3d7dd38b9833fe
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2018
---
# <a name="create-your-first-function-using-visual-studio"></a>Uw eerste functie maken met Visual Studio

Met Azure Functions kunt u uw code in een [serverloze](https://azure.microsoft.com/overview/serverless-computing/) omgeving uitvoeren zonder dat u eerst een virtuele machine moet maken of een webtoepassing publiceren.

In dit artikel leert u hoe u met de Visual Studio 2017-hulpprogramma’s voor Azure Functions lokaal een ‘Hallo wereld’-functie kunt maken en testen. Vervolgens publiceert u de functiecode op Azure. Deze hulpprogramma's zijn beschikbaar als onderdeel van de Azure-ontwikkelworkload in Visual Studio 2017.

![Azure-functiecode in een Visual Studio-project](./media/functions-create-your-first-function-visual-studio/functions-vstools-intro.png)

Dit onderwerp omvat een [video](#watch-the-video) die dezelfde basisstappen laat zien.

## <a name="prerequisites"></a>Vereisten

Vereisten voor het voltooien van deze zelfstudie:

* Installeer [Visual Studio 2017 versie 15.4](https://www.visualstudio.com/vs/) of een latere versie, met inbegrip van de **Azure-ontwikkelworkload**.

    ![Visual Studio 2017 installeren met de Azure-ontwikkelworkload](./media/functions-create-your-first-function-visual-studio/functions-vs-workloads.png)

* Zorg ervoor dat u over de meest recente versie van Azure Functions en WebJobs Tools beschikt. Dat kunt u controleren onder **Updates** > **Visual Studio Marketplace** in **Extensions and Updates**.
    
[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)] 

## <a name="create-a-function-app-project"></a>Een functie-appproject maken

[!INCLUDE [Create a project using the Azure Functions template](../../includes/functions-vstools-create.md)]

Visual Studio maakt een project met daarin een klasse die standaardcode voor het gekozen functietype bevat. Met het kenmerk **FunctionName** in de methode wordt de naam van de functie ingesteld. Met het kenmerk **HttpTrigger** wordt aangegeven dat de functie wordt geactiveerd door een HTTP-aanvraag. De standaardcode verzendt een HTTP-reactie met een waarde uit de hoofdtekst van de aanvraag of uit de query-tekenreeks. U kunt in- en uitvoerbindingen toevoegen aan een functie door de juiste kenmerken op de methode toe te passen. Zie de sectie [Triggers en bindingen](functions-dotnet-class-library.md#triggers-and-bindings) van de [Azure Functions C#-referentie voor ontwikkelaars](functions-dotnet-class-library.md) voor meer informatie.

![Functiecodebestand](./media/functions-create-your-first-function-visual-studio/functions-code-page.png)

Nu u uw functieproject en een HTTP-geactiveerde functie hebt gemaakt, kunt u deze testen op uw lokale computer.

## <a name="test-the-function-locally"></a>De functie lokaal testen

Met Azure Functions Core-hulpprogramma's kunt u een Azure Functions-project uitvoeren op uw lokale ontwikkelcomputer. De eerste keer dat u een functie vanuit Visual Studio start, wordt u gevraagd deze hulpprogramma's te installeren.  

1. Druk op F5 om de functie testen. Accepteer desgevraagd de aanvraag van Visual Studio om Azure Functions Core (CLI)-hulpprogramma's te downloaden en installeren. Mogelijk moet u ook een firewall-uitzondering inschakelen, zodat de hulpprogramma's HTTP-aanvragen kunnen afhandelen.

2. Kopieer de URL van uw functie vanuit de uitvoer van de Azure Functions-runtime.  

    ![Lokale Azure-runtime](./media/functions-create-your-first-function-visual-studio/functions-vstools-f5.png)

3. Plak de URL van de HTTP-aanvraag in de adresbalk van uw browser. Voeg de queryreeks `?name=<yourname>` toe aan de URL en voer de aanvraag uit. Hieronder ziet u de reactie op de lokale GET-aanvraag die door de functie wordt geretourneerd, weergegeven in de browser: 

    ![De reactie van de lokale host van de functie in de browser](./media/functions-create-your-first-function-visual-studio/functions-test-local-browser.png)

4. Als u wilt stoppen met fouten opsporen, drukt u op Shift + F5.

Nadat u hebt gecontroleerd of de functie correct wordt uitgevoerd op uw lokale computer, is het tijd om het project te publiceren naar Azure.

## <a name="publish-the-project-to-azure"></a>Het project naar Azure publiceren

Voordat u uw project kunt publiceren, moet u een functie-app in uw Azure-abonnement hebben. U kunt rechtstreeks vanuit Visual Studio een functie-app maken.

[!INCLUDE [Publish the project to Azure](../../includes/functions-vstools-publish.md)]

## <a name="test-your-function-in-azure"></a>Uw functie testen in Azure

1. Kopieer de basis-URL van de functie-app van de pagina Profiel publiceren. Vervang het `localhost:port`-deel van de URL dat u hebt gebruikt bij het lokaal testen van de functie door de nieuwe basis-URL. Zorg ervoor dat u net als eerder de queryreeks `?name=<yourname>` toevoegt aan de URL en de aanvraag uitvoert.

    De URL die uw HTTP-geactiveerde functie aanroept, moet de volgende indeling hebben:

        http://<functionappname>.azurewebsites.net/api/<functionname>?name=<yourname> 

2. Plak deze nieuwe URL van de HTTP-aanvraag in de adresbalk van uw browser. Hieronder ziet u het antwoord op de externe GET-aanvraag dat door de functie wordt geretourneerd, weergegeven in de browser: 

    ![Het antwoord van de functie in de browser](./media/functions-create-your-first-function-visual-studio/functions-test-remote-browser.png)

## <a name="watch-the-video"></a>Video bekijken

> [!VIDEO https://www.youtube-nocookie.com/embed/DrhG-Rdm80k]

## <a name="next-steps"></a>Volgende stappen

U hebt een C#-functie-app met een eenvoudige HTTP-geactiveerde functie gemaakt in Visual Studio. 

+ Zie [Het project configureren voor lokale ontwikkeling](functions-develop-vs.md#configure-the-project-for-local-development) in de sectie [Azure Functions-tools voor Visual Studio](functions-develop-vs.md) voor meer informatie over het configureren van uw project ter ondersteuning van andere soorten triggers en bindingen.
+ Zie voor meer informatie over het lokale testen en foutopsporing met behulp van de Azure Functions Core Tools, [Code and test Azure Functions locally](functions-run-local.md). 
+ Zie [Using .NET class libraries with Azure Functions](functions-dotnet-class-library.md) (.NET-klassebibliotheken gebruiken met Azure Functions) voor meer informatie over het ontwikkelen van functies als .NET-klassebibliotheken. 

