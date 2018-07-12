---
title: Aan de slag met websockets voor hybride verbindingen in Azure Relay in Node | Microsoft Docs
description: Een Node.js consoletoepassing schrijven voor websockets voor hybride verbindingen in Azure Relay
services: service-bus-relay
documentationcenter: node
author: sethmanheim
manager: timlt
editor: ''
ms.assetid: e44e4867-3cf3-46be-8f8a-7671e2013bc4
ms.service: service-bus-relay
ms.devlang: tbd
ms.topic: get-started-article
ms.tgt_pltfrm: node
ms.workload: na
ms.date: 05/02/2018
ms.author: sethm
ms.openlocfilehash: 1e0b76b96029e1a7ed84f1c8cd895090e8acbc6f
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38670999"
---
# <a name="get-started-with-relay-hybrid-connections-websockets-in-node"></a>Aan de slag met websockets voor hybride verbindingen in Azure Relay in Node

[!INCLUDE [relay-selector-hybrid-connections](../../includes/relay-selector-hybrid-connections.md)]

Deze zelfstudie biedt een inleiding tot de websockets-functie voor [hybride Azure Relay-verbindingen](relay-what-is-it.md#hybrid-connections) en laat zien hoe u met behulp van Node.js een clienttoepassing kunt maken waarmee Websockets-berichten worden verzonden naar een corresponderende listener-toepassing.

## <a name="what-will-be-accomplished"></a>Wat wordt bereikt

Omdat voor Hybride verbindingen zowel een client- als een serveronderdeel is vereist, maakt u in deze zelfstudie twee consoletoepassingen. Dit zijn de stappen:

1. Een Relay-naamruimte maken met behulp van Azure Portal.
2. Een hybride verbinding maken met behulp van Azure Portal.
3. Een serverconsoletoepassing schrijven om berichten te ontvangen.
4. Een clientconsoletoepassing schrijven om berichten te verzenden.

## <a name="prerequisites"></a>Vereisten

1. [Node.js](https://nodejs.org/en/).
2. Een Azure-abonnement.

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="1-create-a-namespace-using-the-azure-portal"></a>1. Een naamruimte maken met Azure Portal

Als u al een Relay-naamruimte hebt gemaakt, gaat u naar de sectie [Een hybride verbinding maken met behulp van Azure Portal](#2-create-a-hybrid-connection-using-the-azure-portal).

[!INCLUDE [relay-create-namespace-portal](../../includes/relay-create-namespace-portal.md)]

## <a name="2-create-a-hybrid-connection-using-the-azure-portal"></a>2. Een hybride verbinding maken met behulp van Azure Portal

Als u al een hybride verbinding hebt gemaakt, gaat u naar de sectie [Een servertoepassing maken](#3-create-a-server-application-listener).

[!INCLUDE [relay-create-hybrid-connection-portal](../../includes/relay-create-hybrid-connection-portal.md)]

## <a name="3-create-a-server-application-listener"></a>3. Een servertoepassing (listener) maken

Maak een Node.js-consoletoepassing om berichten van de Relay te beluisteren en te ontvangen.

[!INCLUDE [relay-hybrid-connections-node-get-started-server](../../includes/relay-hybrid-connections-node-get-started-server.md)]

## <a name="4-create-a-client-application-sender"></a>4. Een clienttoepassing maken (afzender)

Maak een Node.js-consoletoepassing om berichten naar de Relay te verzenden.

[!INCLUDE [relay-hybrid-connections-node-get-started-client](../../includes/relay-hybrid-connections-node-get-started-client.md)]

## <a name="5-run-the-applications"></a>5. De toepassingen uitvoeren

1. Voer de servertoepassing uit: via een Node.js-opdrachtprompt van het type `node listener.js`.
2. Voer de clienttoepassing uit: via een Node.js-opdrachtprompt van het type `node sender.js`, en voer tekst in.
3. Zorg ervoor dat de servertoepassingsconsole de tekst uitvoert die in de clienttoepassing is ingevoerd.

![actieve-toepassingen](./media/relay-hybrid-connections-node-get-started/running-applications.png)

Gefeliciteerd, u hebt een end-to-endtoepassing met hybride verbindingen gemaakt met behulp van Node.js!

## <a name="next-steps"></a>Volgende stappen

* [Veelgestelde vragen over Relay](relay-faq.md)
* [Een naamruimte maken](relay-create-namespace-portal.md)
* [Aan de slag met .NET](relay-hybrid-connections-dotnet-get-started.md)
* [Aan de slag met knooppunten](relay-hybrid-connections-node-get-started.md)

