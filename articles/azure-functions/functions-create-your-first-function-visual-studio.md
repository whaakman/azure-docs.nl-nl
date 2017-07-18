---
title: Uw eerste functie maken in Azure met behulp van Visual Studio | Microsoft Docs
description: Maak een eenvoudige HTTP-geactiveerde functie en publiceer deze op Azure met behulp van Azure Functions-hulpprogramma's voor Visual Studio.
services: functions
documentationcenter: na
author: rachelappel
manager: erikre
editor: 
tags: 
keywords: azure-functies, functies, gebeurtenisverwerking, berekenen, architectuur zonder server
ms.assetid: 82db1177-2295-4e39-bd42-763f6082e796
ms.service: functions
ms.devlang: multiple
ms.topic: hero-article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 07/05/2017
ms.author: rachelap, glenga
ms.translationtype: Human Translation
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.openlocfilehash: 2d539dd51834e935f9c5f7a6be42011ffe9e0870
ms.contentlocale: nl-nl
ms.lasthandoff: 07/08/2017


---
# <a name="create-your-first-function-using-visual-studio"></a>Uw eerste functie maken met Visual Studio 

Met Azure Functions kunt u uw code in een serverloze omgeving uitvoeren zonder dat u eerst een virtuele machine moet maken of een webtoepassing moet publiceren. 

In dit onderwerp leert u hoe u met de Azure Function Tools voor Visual Studio 2017 lokaal een 'Hallo wereld'-functie kunt maken en testen. Vervolgens publiceert u de functiecode op Azure.

![Azure-functiecode in een Visual Studio-project](./media/functions-create-your-first-function-visual-studio/functions-vstools-intro.png)

## <a name="prerequisites"></a>Vereisten

Voor deze zelfstudie installeert u het volgende:

* [Visual Studio 2017 Preview versie 15.3](https://www.visualstudio.com/vs/preview/), waaronder de **Azure-ontwikkel**workload.
    
    ![Visual Studio 2017 installeren met de Azure-ontwikkelworkload](./media/functions-create-your-first-function-visual-studio/functions-vs-workloads.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="install-azure-functions-tools-for-visual-studio-2017"></a>Azure Functions Tools voor Visual Studio 2017 installeren

Voordat u begint, moet u de Azure Function Tools voor Visual Studio 2017 downloaden en installeren. Deze hulpprogramma's kunnen alleen worden gebruikt met Visual Studio 2017 Preview-versie 15.3 of een latere versie. Als u de hulpprogramma's van Azure Functions al hebt geïnstalleerd, kunt u deze sectie overslaan.

[!INCLUDE [Install the Azure Functions Tools for Visual Studio](../../includes/functions-install-vstools.md)]   

## <a name="create-an-azure-functions-project-in-visual-studio"></a>Een Azure Functions-project in Visual Studio maken

[!INCLUDE [Create a project using the Azure Functions template](../../includes/functions-vstools-create.md)]

Nu u het project hebt gemaakt, kunt u uw eerste functie maken.

## <a name="create-the-function"></a>De functie maken

Klik in **Solution Explorer** met de rechtermuisknop op het projectknooppunt en selecteer  > **Nieuw item****Toevoegen**. Selecteer **Azure-functie** en klik op **Toevoegen**.

Selecteer **HttpTrigger**, typ een **Functienaam**, selecteer **Anoniem** bij **Toegangsrechten** en klik op **Maken**. De gemaakte functie wordt geopend door een HTTP-aanvraag vanaf een client. 

![Een nieuwe Azure-functie maken](./media/functions-create-your-first-function-visual-studio/functions-vstools-add-new-function-2.png)

Nu u een HTTP-geactiveerde-functie hebt gemaakt, kunt u deze testen op uw lokale computer.

## <a name="test-the-function-locally"></a>De functie lokaal testen

[!INCLUDE [Test the function locally](../../includes/functions-vstools-test.md)]

Kopieer de URL van uw functie vanuit de uitvoer van de Azure Functions-runtime.  

![Lokale Azure-runtime](./media/functions-create-your-first-function-visual-studio/functions-vstools-f5.png)

 Plak de URL van de HTTP-aanvraag in de adresbalk van uw browser. Voeg de queryreeks `&name=<yourname>` toe aan de URL en voer de aanvraag uit. Hieronder ziet u de reactie op de lokale GET-aanvraag die door de functie wordt geretourneerd, weergegeven in de browser: 

![De reactie van de lokale host van de functie in de browser](./media/functions-create-your-first-function-visual-studio/functions-test-local-browser.png)

Als u de foutopsporing wilt stoppen, klikt u op de knop **Stop** op de werkbalk van Visual Studio.

Nadat u hebt gecontroleerd of de functie correct wordt uitgevoerd op uw lokale computer, is het tijd om het project te publiceren naar Azure.

## <a name="publish-the-project-to-azure"></a>Het project naar Azure publiceren

Voordat u uw project kunt publiceren, moet u een functie-app in uw Azure-abonnement hebben. U kunt rechtstreeks vanuit Visual Studio een functie-app maken.

[!INCLUDE [Publish the project to Azure](../../includes/functions-vstools-publish.md)]

## <a name="test-your-function-in-azure"></a>Uw functie testen in Azure

Kopieer de basis-URL van de functie-app van de pagina Profiel publiceren. Vervang het `localhost:port`-deel van de URL dat u hebt gebruikt bij het lokaal testen van de functie door de nieuwe basis-URL. Zorg ervoor dat u net als eerder de queryreeks `&name=<yourname>` toevoegt aan de URL en de aanvraag uitvoert.

De URL die uw HTTP-geactiveerde functie aanroept, ziet er als volgt uit:

    http://<functionappname>.azurewebsites.net/api/<functionname>?name=<yourname> 

Plak deze nieuwe URL van de HTTP-aanvraag in de adresbalk van uw browser. Hieronder ziet u het antwoord op de externe GET-aanvraag dat door de functie wordt geretourneerd, weergegeven in de browser: 

![Het antwoord van de functie in de browser](./media/functions-create-your-first-function-visual-studio/functions-test-remote-browser.png)
 
## <a name="next-steps"></a>Volgende stappen

U hebt een C#-functie-app met een eenvoudige HTTP-geactiveerde functie gemaakt in Visual Studio. 

[!INCLUDE [functions-quickstart-next-steps](../../includes/functions-quickstart-next-steps.md)]

Zie voor meer informatie over het lokale testen en foutopsporing met behulp van de Azure Functions Core Tools, [Code and test Azure Functions locally](functions-run-local.md). Zie [Using .NET class libraries with Azure Functions](functions-dotnet-class-library.md) (.NET-klassebibliotheken gebruiken met Azure Functions) voor meer informatie over het ontwikkelen van functies als .NET-klassebibliotheken. 


