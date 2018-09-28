---
title: Snelstartgids - Azure SignalR-service REST-API | Microsoft Docs
description: Een snelstartgids voor het gebruik van de Azure SignalR Service REST-API.
services: signalr
documentationcenter: ''
author: sffamily
manager: cfowler
editor: ''
ms.assetid: ''
ms.service: signalr
ms.devlang: dotnet
ms.topic: quickstart
ms.tgt_pltfrm: ASP.NET
ms.workload: tbd
ms.date: 06/13/2018
ms.author: zhshang
ms.openlocfilehash: 93c1198ecfba6db809228ed6dcd99c705f53926c
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46972756"
---
# <a name="quickstart-broadcast-real-time-messages-from-console-app"></a>Snelstartgids: Realtimeberichten verzenden via de console-app

Azure SignalR-service biedt [REST-API](https://github.com/Azure/azure-signalr/blob/dev/docs/rest-api.md) ter ondersteuning van server-naar-client-communicatie, zoals broadcasting. U kunt elke programmeertaal kiezen die een REST-API kan aanroepen. U kunt berichten verzenden naar alle verbonden clients, een specifieke client op basis van naam of een groep clients.

In deze snelstartgids leert u hoe u in C# berichten via een opdrachtregel-app kunt verzenden naar verbonden client-apps.

## <a name="prerequisites"></a>Vereisten

Deze snelstartgids kan worden uitgevoerd op macOS, Windows of Linux.
* [.NET Core-SDK](https://www.microsoft.com/net/download/core)
* Een teksteditor of code-editor naar keuze.


[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]


## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u met uw Azure-account aan bij Azure Portal op <https://portal.azure.com/>.


[!INCLUDE [Create instance](includes/signalr-quickstart-create-instance.md)]

## <a name="clone-the-sample-application"></a>De voorbeeldtoepassing klonen

Laten wij de code klaarmaken terwijl de service wordt geïmplementeerd. Kloon de [voorbeeld-app vanuit GitHub](https://github.com/aspnet/AzureSignalR-samples.git), stel de SignalR Service-verbindingsreeks in en voer de toepassing lokaal uit.

1. Open een nieuw git-terminalvenster. Ga naar een map waarin u het voorbeeldproject wilt klonen.

1. Voer de volgende opdracht uit om de voorbeeldopslagplaats te klonen. Deze opdracht maakt een kopie van de voorbeeld-app op uw computer.

    ```bash
    git clone https://github.com/aspnet/AzureSignalR-samples.git
    ```

## <a name="build-and-run-the-sample"></a>Het voorbeeldproject compileren en uitvoeren

Dit voorbeeld is een console-app die het gebruik van de Azure SignalR-service weergeeft. Deze biedt twee modi:

- Servermodus: gebruik eenvoudige opdrachten om de REST-API voor Azure SignalR-service aan te roepen.
- Clientmodus: maak verbinding met de Azure SignalR-service en ontvang berichten van de server.

U kunt ook zien hoe u een toegangstoken kunt genereren voor verificatie met Azure SignalR-service.

### <a name="build-the-executable-file"></a>Het uitvoerbare bestand maken

We gebruiken macOS osx.10.13-x64 als voorbeeld. U kunt [naslaginformatie](https://docs.microsoft.com/dotnet/core/rid-catalog) vinden over het maken op andere platforms.
```bash
cd AzureSignalR-samples/samples/Serverless/

dotnet publish -c Release -r osx.10.13-x64
```

### <a name="start-a-client"></a>Een client starten

```bash
cd bin/Release/netcoreapp2.1/osx.10.13-x64/

Serverless client <ClientName> -c "<ConnectionString>" -h <HubName>
```

### <a name="start-a-server"></a>Een server starten

```bash
cd bin/Release/netcoreapp2.1/osx.10.13-x64/

Serverless server -c "<ConnectionString>" -h <HubName>
```

## <a name="run-the-sample-without-publishing"></a>Het voorbeeld uitvoeren zonder te publiceren

U kunt ook de onderstaande opdracht uitvoeren om een server of client te starten

```bash
# Start a server
dotnet run -- server -c "<ConnectionString>" -h <HubName>

# Start a client
dotnet run -- client <ClientName> -c "<ConnectionString>" -h <HubName>
```

### <a name="use-user-secrets-to-specify-connection-string"></a>Gebruikersgeheimen gebruiken om een verbindingsreeks te specificeren

U kunt `dotnet user-secrets set Azure:SignalR:ConnectionString "<ConnectionString>"` uitvoeren in de hoofdmap van het voorbeeld. Daarna hebt u de optie `-c "<ConnectionString>"` niet meer nodig.

## <a name="usage"></a>Gebruik

Gebruik de opdracht om het bericht te verzenden nadat de server is gestart

```
send user <User Id>
send users <User List>
send group <Group Name>
send groups <Group List>
broadcast
```

U kunt meerdere clients met verschillende clientnamen starten.


[!INCLUDE [Cleanup](includes/signalr-quickstart-cleanup.md)]
