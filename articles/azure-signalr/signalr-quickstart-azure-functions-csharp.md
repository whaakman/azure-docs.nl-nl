---
title: Snelstart voor serverloze Azure SignalR Service - C# | Microsoft Docs
description: Een snelstart waarin u leert hoe u Azure SignalR Service en Azure Functions gebruikt om een chatruimte te maken.
services: signalr
documentationcenter: ''
author: sffamily
manager: cfowler
editor: ''
ms.assetid: ''
ms.service: signalr
ms.devlang: dotnet
ms.topic: quickstart
ms.tgt_pltfrm: Azure Functions
ms.workload: tbd
ms.date: 09/23/2018
ms.author: zhshang
ms.openlocfilehash: 7c28385c9b29f98968bcdf758f4a9a5b08da3f9f
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46993101"
---
# <a name="quickstart-create-a-chat-room-with-azure-functions-and-signalr-service-using-c"></a>Snelstart: Een chatruimte maken met Azure Functions en SignalR Service met behulp van C#

Met Azure SignalR Service kunt u eenvoudig realtime functionaliteit toevoegen aan uw toepassing. Azure Functions is een serverloos platform waarmee u uw code kunt uitvoeren zonder een infrastructuur te moeten beheren. In deze snelstart leert u hoe u SignalR Service en Functions gebruikt om een serverloze, realtime chattoepassing te bouwen.


## <a name="prerequisites"></a>Vereisten

Als u Visual Studio 2017 nog niet hebt geïnstalleerd, kunt u het downloaden en de **gratis** [Community Edition van Visual Studio 2017](https://www.visualstudio.com/downloads/) gebruiken. Zorg ervoor dat u **Azure-ontwikkeling** inschakelt tijdens de installatie van Visual Studio.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]


## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Meld u met uw Azure-account aan bij Azure Portal op <https://portal.azure.com/>.


[!INCLUDE [Create instance](includes/signalr-quickstart-create-instance.md)]

[!INCLUDE [Clone application](includes/signalr-quickstart-clone-application.md)]


## <a name="configure-and-run-the-azure-function-app"></a>De Azure Functions-app uitvoeren

1. Start Visual Studio en open de oplossing in de map *chat\src\csharp* van de gekloonde opslagplaats.

1. Controleer in de browser waarin de Azure Portal is geopend of het SignalR Service-exemplaar dat u eerder hebt geïmplementeerd, is gemaakt. Daarvoor typt u de naam ervan in het zoekvak bovenaan de portal. Selecteer het exemplaar om het te openen.

    ![Het SignalR Service-exemplaar zoeken](media/signalr-quickstart-azure-functions-csharp/signalr-quickstart-search-instance.png)

1. Selecteer **Sleutels** om de verbindingsreeksen voor het SignalR Service-exemplaar weer te geven.

1. Selecteer en kopieer de primaire verbindingsreeks.

1. Terug in Visual Studio wijzigt u in Solution Explorer de naam van *local.settings.sample.json* in *local.settings.json*.

1. In **local.settings.json** plakt u de verbindingsreeks in de waarde van de instelling **AzureSignalRConnectionString**. Sla het bestand op.

1. Open **Functions.cs**. Deze functie-app bevat twee HTTP-geactiveerde functies:

    - **GetSignalRInfo**: gebruikt de invoergegevensbinding *SignalRConnectionInfo* om geldige verbindingsgegevens te genereren en te retourneren.
    - **SendMessage**: ontvangt een chatbericht in de aanvraagbody en gebruikt de uitvoergegevensbinding *SignalR* om het bericht uit te zenden naar alle verbonden clienttoepassingen.

1. Selecteer in het menu **Fouten opsporen** de optie **Foutopsporing starten** om de toepassing uit te voeren.

    ![Fouten in de toepassing opsporen](media/signalr-quickstart-azure-functions-csharp/signalr-quickstart-debug-vs.png)


[!INCLUDE [Run web application](includes/signalr-quickstart-run-web-application.md)]


[!INCLUDE [Cleanup](includes/signalr-quickstart-cleanup.md)]

## <a name="next-steps"></a>Volgende stappen

In deze snelstart hebt u een serverloze, realtime toepassing in VS Code gebouwd en uitgevoerd. Nu volgt meer informatie over het implementeren van Azure Functions vanuit VS Code.

> [!div class="nextstepaction"]
> [Azure Functions met VS Code implementeren](https://code.visualstudio.com/tutorials/functions-extension/getting-started)