---
title: Aan de slag met HTTP-aanvragen voor hybride verbindingen van Azure Relay in Node | Microsoft Docs
description: Hier leert u hoe u een consoletoepassing schrijft in Node.js voor HTTP-aanvragen voor hybride verbindingen van Azure Relay.
services: service-bus-relay
documentationcenter: node
author: clemensv
manager: timlt
editor: ''
ms.assetid: e44e4867-3cf3-46be-8f8a-7671e2013bc4
ms.service: service-bus-relay
ms.devlang: tbd
ms.topic: get-started-article
ms.tgt_pltfrm: node
ms.workload: na
ms.date: 05/02/2018
ms.author: clemensv
ms.openlocfilehash: 2bc923650425c76562161dd6f44f3a5722b5cefe
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2018
ms.locfileid: "33896622"
---
# <a name="get-started-with-relay-hybrid-connections-http-requests-in-node"></a>Aan de slag met HTTP-aanvragen voor hybride verbindingen van Relay in Node

[!INCLUDE [relay-selector-hybrid-connections](../../includes/relay-selector-hybrid-connections.md)]

Deze zelfstudie biedt een inleiding tot HTTP-aanvragen voor [hybride verbindingen van Azure Relay](relay-what-is-it.md#hybrid-connections) en laat zien hoe u met behulp van Node.js een clienttoepassing kunt maken waarmee berichten worden verzonden naar een corresponderende listener-toepassing.

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

[!INCLUDE [relay-hybrid-connections-node-get-started-server](../../includes/relay-hybrid-connections-http-requests-node-get-started-server.md)]

## <a name="4-create-a-client-application-sender"></a>4. Een clienttoepassing maken (afzender)

Om berichten te versturen naar de Relay, kunt u een HTTP-client gebruiken of een consoletoepassing schrijven in Node.js.

[!INCLUDE [relay-hybrid-connections-node-get-started-client](../../includes/relay-hybrid-connections-http-requests-node-get-started-client.md)]

## <a name="5-run-the-applications"></a>5. De toepassingen uitvoeren

1. Voer de servertoepassing uit: via een Node.js-opdrachtprompt van het type `node listener.js`.
2. Voer de clienttoepassing uit: via een Node.js-opdrachtprompt van het type `node sender.js`, en voer tekst in.
3. Zorg ervoor dat de servertoepassingsconsole de tekst uitvoert die in de clienttoepassing is ingevoerd.

Gefeliciteerd, u hebt een end-to-endtoepassing met hybride verbindingen gemaakt met behulp van Node.js!

## <a name="next-steps"></a>Volgende stappen

* [Veelgestelde vragen over Relay](relay-faq.md)
* [Een naamruimte maken](relay-create-namespace-portal.md)
* [Aan de slag met .NET](relay-hybrid-connections-dotnet-get-started.md)
* [Aan de slag met knooppunten](relay-hybrid-connections-node-get-started.md)

