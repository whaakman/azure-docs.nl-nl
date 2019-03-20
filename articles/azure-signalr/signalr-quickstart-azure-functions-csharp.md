---
title: Azure SignalR Service-serverloze snelstart:C#
description: Een quickstart waarin u leert hoe u de service Azure SignalR en Azure Functions gebruikt om een chatruimte te maken.
author: sffamily
ms.service: signalr
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 03/04/2019
ms.author: zhshang
ms.openlocfilehash: e3a437d44c557d2ec53182d4f6178cbea3e54eba
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57544969"
---
# <a name="quickstart-create-a-chat-room-with-azure-functions-and-signalr-service-using-c"></a>Snelstart: Een chatruimte maken met Azure Functions en SignalR Service met behulp van C\#

Met de service Azure SignalR kunt u eenvoudig realtimefunctionaliteit toevoegen aan een toepassing. Azure Functions is een serverloos platform waarmee u code kunt uitvoeren zonder een infrastructuur te beheren. In deze quickstart leert u hoe u de service SignalR en Functions gebruikt om een serverloze, realtimechattoepassing te bouwen.

## <a name="prerequisites"></a>Vereisten

Als u Visual Studio 2017 nog niet hebt geïnstalleerd, kunt u het downloaden en de **gratis** [Community Edition van Visual Studio 2017](https://www.visualstudio.com/downloads/) gebruiken. Zorg ervoor dat u **Azure-ontwikkeling** inschakelt tijdens de installatie van Visual Studio.

U kunt deze zelfstudie ook uitvoeren op de opdrachtregel (Mac OS, Windows of Linux) met behulp van de [Azure Functions Core Tools (v2)](https://github.com/Azure/azure-functions-core-tools#installing), wordt de [.NET Core SDK](https://dotnet.microsoft.com/download), en uw favoriete code-editor.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Meld u met uw Azure-account aan bij Azure Portal op <https://portal.azure.com/>.

[!INCLUDE [Create instance](includes/signalr-quickstart-create-instance.md)]

[!INCLUDE [Clone application](includes/signalr-quickstart-clone-application.md)]

## <a name="configure-and-run-the-azure-function-app"></a>De Azure-functie-app configureren en uitvoeren

1. Start Visual Studio (of een andere code-editor) en open de oplossing in de *chat\src\csharp* map van de gekloonde opslagplaats.

1. Controleer in de browser waarin de Azure-portal is geopend, of het service-exemplaar van SignalR dat u eerder hebt geïmplementeerd, is gemaakt. Hiervoor typt u de naam van het exemplaar in het zoekvak boven in de portal. Selecteer het exemplaar om het te openen.

    ![Het service-exemplaar van SignalR zoeken](media/signalr-quickstart-azure-functions-csharp/signalr-quickstart-search-instance.png)

1. Selecteer **Sleutels** om de verbindingsreeksen voor het service-exemplaar van SignalR weer te geven.

1. Selecteer en kopieer de primaire verbindingsreeks.

1. Terug in Visual Studio wijzigt u in Solution Explorer de naam van *local.settings.sample.json* in *local.settings.json*.

1. Plak in **local.settings.json** de verbindingsreeks in de waarde van de instelling **AzureSignalRConnectionString**. Sla het bestand op.

1. Open **Functions.cs**. Deze functie-app bevat twee HTTP-geactiveerde functies:

    - **GetSignalRInfo**: gebruikt de invoergegevensbinding *SignalRConnectionInfo* om geldige verbindingsgegevens te genereren en te retourneren.
    - **SendMessage**: ontvangt een chatbericht in de aanvraagbody en gebruikt de uitvoergegevensbinding *SignalR* om het bericht uit te zenden naar alle verbonden clienttoepassingen.

1. Gebruik een van de volgende opties om de Azure-functie-app lokaal te starten.

    - **Visual Studio**: Selecteer in het menu *Fouten opsporen* de optie *Foutopsporing starten* om de toepassing uit te voeren.

        ![Fouten in de toepassing opsporen](media/signalr-quickstart-azure-functions-csharp/signalr-quickstart-debug-vs.png)

    - **Vanaf de opdrachtregel**: Voer de volgende opdracht om de functie host start.

        ```bash
        func start
        ```

[!INCLUDE [Run web application](includes/signalr-quickstart-run-web-application.md)]

[!INCLUDE [Cleanup](includes/signalr-quickstart-cleanup.md)]

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids hebt gemaakt en is een realtime toepassing zonder server wordt uitgevoerd in Visual Studio. Nu volgt meer informatie over het ontwikkelen en implementeren van Azure Functions met behulp van Visual Studio Code.

> [!div class="nextstepaction"]
> [Azure Functions ontwikkelen met Visual Studio](../azure-functions/functions-develop-vs.md)