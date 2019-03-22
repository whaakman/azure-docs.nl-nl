---
title: Azure SignalR Service zonder server Quick Start - Java
description: Een quickstart waarin u leert hoe u de service Azure SignalR en Azure Functions gebruikt om een chatruimte te maken.
author: sffamily
ms.service: signalr
ms.devlang: java
ms.topic: quickstart
ms.date: 03/04/2019
ms.author: zhshang
ms.openlocfilehash: b2f89d3fbb427cc3e14be231d4ffae879e641f2c
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57554851"
---
# <a name="quickstart-create-a-chat-room-with-azure-functions-and-signalr-service-using-java"></a>Quickstart: Een chatruimte maken met Azure Functions en SignalR-Service met behulp van Java

Met de service Azure SignalR kunt u eenvoudig realtimefunctionaliteit toevoegen aan een toepassing. Azure Functions is een serverloos platform waarmee u code kunt uitvoeren zonder een infrastructuur te beheren. In deze quickstart leert u hoe u de service SignalR en Functions gebruikt om een serverloze, realtimechattoepassing te bouwen.

## <a name="prerequisites"></a>Vereisten

Deze quickstart kan worden uitgevoerd op macOS, Windows of Linux.

Zorg ervoor dat u een code-editor hebt geïnstalleerd, bijvoorbeeld [Visual Studio Code](https://code.visualstudio.com/).

Installeer [Azure Functions Core Tools (v2)](https://github.com/Azure/azure-functions-core-tools#installing) om de Azure Function-apps lokaal uit te voeren.

> [!NOTE]
> Gebruik de bindingen SignalR-Service in Java, zorg ervoor dat u versie 2.4.419 of hoger van Azure Functions Core Tools (hostversie 2.0.12332).

Momenteel moet [.NET Core SDK](https://www.microsoft.com/net/download) zijn geïnstalleerd voor Azure Functions Core Tools om de extensies te installeren. Er is echter geen kennis van .NET vereist om JavaScript Azure-functie-apps te bouwen.

Als u functie-apps wilt ontwikkelen met behulp van Java, moet het volgende zijn geïnstalleerd:

* [Java Developer Kit](https://www.azul.com/downloads/zulu/), versie 8.
* [Apache Maven](https://maven.apache.org), versie 3.0 of hoger.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Meld u met uw Azure-account aan bij Azure Portal op <https://portal.azure.com/>.

[!INCLUDE [Create instance](includes/signalr-quickstart-create-instance.md)]

[!INCLUDE [Clone application](includes/signalr-quickstart-clone-application.md)]

## <a name="configure-and-run-the-azure-function-app"></a>De Azure-functie-app configureren en uitvoeren

1. Controleer in de browser waarin de Azure-portal is geopend, of het service-exemplaar van SignalR dat u eerder hebt geïmplementeerd, is gemaakt. Hiervoor typt u de naam van het exemplaar in het zoekvak boven in de portal. Selecteer het exemplaar om het te openen.

    ![Het service-exemplaar van SignalR zoeken](media/signalr-quickstart-azure-functions-csharp/signalr-quickstart-search-instance.png)

1. Selecteer **Sleutels** om de verbindingsreeksen voor het service-exemplaar van SignalR weer te geven.

1. Selecteer en kopieer de primaire verbindingsreeks.

    ![De service SignalR maken](media/signalr-quickstart-azure-functions-javascript/signalr-quickstart-keys.png)

1. Open in uw code-editor de *chat/src/java* map in de gekloonde opslagplaats.

1. Wijzig de naam *local.settings.sample.json* in *local.settings.json*.

1. Plak in **local.settings.json** de verbindingsreeks in de waarde van de instelling **AzureSignalRConnectionString**. Sla het bestand op.

1. Het belangrijkste bestand met de functies zijn *src/main/java/com/function/Functions.java*:

    - **negotiate**: gebruikt de invoerbinding *SignalRConnectionInfo* om geldige verbindingsgegevens te genereren en te retourneren.
    - **SendMessage** : een chatbericht in de hoofdtekst van de aanvraag ontvangt en gebruikt de *SignalR* uitvoer binding voor het uitzenden van het bericht voor alle verbonden clienttoepassingen.

1. In de terminal, zorg ervoor dat u de *chat/src/java* map. De functie-app bouwen.

    ```bash
    mvn clean package
    ```

1. De functie-app lokaal uitvoeren.

    ```bash
    mvn azure-functions:run
    ```

[!INCLUDE [Run web application](includes/signalr-quickstart-run-web-application.md)]

[!INCLUDE [Cleanup](includes/signalr-quickstart-cleanup.md)]

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids hebt gemaakt en een realtime serverloze toepassingen met behulp van Maven is uitgevoerd. Vervolgens kunt u meer informatie over het maken van Java-Azure-functies vanaf het begin.

> [!div class="nextstepaction"]
> [Uw eerste functie maken met Java en Maven](../azure-functions/functions-create-first-java-maven.md)