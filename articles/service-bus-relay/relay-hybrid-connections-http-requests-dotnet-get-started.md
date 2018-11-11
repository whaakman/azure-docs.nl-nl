---
title: Aan de slag met HTTP-aanvragen voor hybride verbindingen van Azure Relay in .NET | Microsoft Docs
description: Hier leert u hoe u een C#-consoletoepassing schrijft voor HTTP-aanvragen voor hybride verbindingen van Azure Relay in .NET.
services: service-bus-relay
documentationcenter: .net
author: spelluru
manager: timlt
editor: ''
ms.assetid: d1386900-b942-4abf-acfc-38d2ef826253
ms.service: service-bus-relay
ms.devlang: tbd
ms.topic: get-started-article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 08/16/2018
ms.author: spelluru
ms.openlocfilehash: 55e97ff95245ce222ccbc2a99f6ae2882cef3715
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51248772"
---
# <a name="get-started-with-relay-hybrid-connections-http-requests-in-net"></a>Aan de slag met HTTP-aanvragen voor hybride verbindingen van Relay in .NET
[!INCLUDE [relay-selector-hybrid-connections](../../includes/relay-selector-hybrid-connections.md)]

Deze zelfstudie bevat een inleiding tot [Hybride verbindingen van Azure Relay](relay-what-is-it.md#hybrid-connections). Hier vindt u informatie over hoe u met Microsoft .NET een clienttoepassing kunt maken die aanvragen naar een bijbehorende listener-toepassing verzendt. 

## <a name="what-will-be-accomplished"></a>Wat wordt bereikt
Voor Hybride verbindingen is zowel een client- als een servercomponent vereist. In deze zelfstudie voert u de volgende stappen uit om twee consoletoepassingen te maken:

1. Een Relay-naamruimte maken met behulp van Azure Portal.
2. Een hybride verbinding in die naamruimte maken met behulp van Azure Portal.
3. Een serverconsoletoepassing (listener) schrijven om aanvragen te ontvangen.
4. Een clientconsoletoepassing (afzender) schrijven om aanvragen te verzenden.

## <a name="prerequisites"></a>Vereisten

Voor het voltooien van deze zelfstudie moet aan de volgende vereisten worden voldaan:

* [Visual Studio 2015 of hoger](https://www.visualstudio.com). In de voorbeelden in deze zelfstudie wordt Visual Studio 2017 gebruikt.
* Een Azure-abonnement.

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="1-create-a-namespace-by-using-the-azure-portal"></a>1. Een naamruimte maken met behulp van Azure Portal
Als u al een Relay-naamruimte hebt gemaakt, gaat u naar [Een hybride verbinding maken met behulp van Azure Portal](#2-create-a-hybrid-connection-using-the-azure-portal).

[!INCLUDE [relay-create-namespace-portal](../../includes/relay-create-namespace-portal.md)]

## <a name="2-create-a-hybrid-connection-by-using-the-azure-portal"></a>2. Een hybride verbinding maken met behulp van Azure Portal
Als u al een hybride verbinding hebt gemaakt, gaat u naar [Een servertoepassing maken](#3-create-a-server-application-listener).

[!INCLUDE [relay-create-hybrid-connection-portal](../../includes/relay-create-hybrid-connection-portal.md)]

## <a name="3-create-a-server-application-listener"></a>3. Een servertoepassing (listener) maken
Maak in Visual Studio een C#-consoletoepassing om berichten van de Relay te beluisteren en te ontvangen.

[!INCLUDE [relay-hybrid-connections-http-requests-dotnet-get-started-server](../../includes/relay-hybrid-connections-http-requests-dotnet-get-started-server.md)]

## <a name="4-create-a-client-application-sender"></a>4. Een clienttoepassing maken (afzender)
Maak in Visual Studio een C#-consoletoepassing om berichten naar de Relay te sturen.

[!INCLUDE [relay-hybrid-connections-http-requests-dotnet-get-started-client](../../includes/relay-hybrid-connections-http-requests-dotnet-get-started-client.md)]

## <a name="5-run-the-applications"></a>5. De toepassingen uitvoeren
1. Voer de servertoepassing uit. U ziet de volgende tekst in het consolevenster:

    ```
    Online
    Server listening
    ```
1. Voer de clienttoepassing uit. U ziet `hello!` in het clientvenster. De client heeft een HTTP-aanvraag verzonden naar de server en de server heeft gereageerd met een `hello!`. 
3. Nu kunt u de consolevensters sluiten door in beide consolevenster op **Enter** te drukken. 

Gefeliciteerd, u hebt een end-to-end toepassing met hybride verbindingen gemaakt.

## <a name="next-steps"></a>Volgende stappen

* [Veelgestelde vragen over Relay](relay-faq.md)
* [Een naamruimte maken](relay-create-namespace-portal.md)
* [Aan de slag met knooppunten](relay-hybrid-connections-node-get-started.md)

