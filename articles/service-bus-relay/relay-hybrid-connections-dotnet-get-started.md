---
title: Aan de slag met hybride Relay-verbindingen van Azure in .NET | Microsoft Docs
description: Een consoletoepassing in C# schrijven voor hybride Relay-verbindingen van Azure.
services: service-bus-relay
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: d1386900-b942-4abf-acfc-38d2ef826253
ms.service: service-bus-relay
ms.devlang: tbd
ms.topic: get-started-article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 12/15/2017
ms.author: sethm
ms.openlocfilehash: 3667d7f4a74ac7c43f8f443aed2beacafd909dbd
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2018
---
# <a name="get-started-with-relay-hybrid-connections"></a>Aan de slag met hybride Relay-verbindingen
[!INCLUDE [relay-selector-hybrid-connections](../../includes/relay-selector-hybrid-connections.md)]

Deze zelfstudie bevat een inleiding tot [Hybride verbindingen van Azure Relay](relay-what-is-it.md#hybrid-connections). Hier vindt u informatie over hoe u met Microsoft .NET een clienttoepassing kunt maken die berichten naar een bijbehorende listener-toepassing verzendt. 

## <a name="what-will-be-accomplished"></a>Wat wordt bereikt
Voor Hybride verbindingen is zowel een client- als een servercomponent vereist. In deze zelfstudie voert u de volgende stappen uit om twee consoletoepassingen te maken:

1. Een Relay-naamruimte maken met behulp van Azure Portal.
2. Een hybride verbinding in die naamruimte maken met behulp van Azure Portal.
3. Een serverconsoletoepassing (listener) schrijven om berichten te ontvangen.
4. Een clientconsoletoepassing (afzender) schrijven om berichten te verzenden.

## <a name="prerequisites"></a>Vereisten

Voor het voltooien van deze zelfstudie moet aan de volgende vereisten worden voldaan:

* [Visual Studio 2015 of hoger](http://www.visualstudio.com). In de voorbeelden in deze zelfstudie wordt Visual Studio 2017 gebruikt.
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

[!INCLUDE [relay-hybrid-connections-dotnet-get-started-server](../../includes/relay-hybrid-connections-dotnet-get-started-server.md)]

## <a name="4-create-a-client-application-sender"></a>4. Een clienttoepassing maken (afzender)
Maak in Visual Studio een C#-consoletoepassing om berichten naar de Relay te sturen.

[!INCLUDE [relay-hybrid-connections-dotnet-get-started-client](../../includes/relay-hybrid-connections-dotnet-get-started-client.md)]

## <a name="5-run-the-applications"></a>5. De toepassingen uitvoeren
1. Voer de servertoepassing uit.
2. Voer de clienttoepassing uit en voer wat tekst in.
3. Zorg ervoor dat de servertoepassingsconsole de tekst weergeeft die in de clienttoepassing is ingevoerd.

![actieve-toepassingen](./media/relay-hybrid-connections-dotnet-get-started/running-applications.png)

Gefeliciteerd, u hebt een end-to-end toepassing met hybride verbindingen gemaakt.

## <a name="next-steps"></a>Volgende stappen

* [Veelgestelde vragen over Relay](relay-faq.md)
* [Een naamruimte maken](relay-create-namespace-portal.md)
* [Aan de slag met knooppunten](relay-hybrid-connections-node-get-started.md)

