---
title: Aan de slag met hybride Relay-verbindingen van Azure in Node | Microsoft Docs
description: Een knooppuntconsoletoepassing schrijven voor hybride verbindingen
services: service-bus-relay
documentationcenter: node
author: sethmanheim
manager: timlt
editor: 
ms.assetid: e44e4867-3cf3-46be-8f8a-7671e2013bc4
ms.service: service-bus-relay
ms.devlang: tbd
ms.topic: get-started-article
ms.tgt_pltfrm: node
ms.workload: na
ms.date: 05/22/2017
ms.author: sethm
ms.translationtype: Human Translation
ms.sourcegitcommit: 532ff423ff53567b6ce40c0ea7ec09a689cee1e7
ms.openlocfilehash: d8f3f6fbe256b34b812369dc1f7492ed4f15d3d3
ms.contentlocale: nl-nl
ms.lasthandoff: 06/05/2017


---
<a id="get-started-with-relay-hybrid-connections" class="xliff"></a>

# Aan de slag met hybride Relay-verbindingen

[!INCLUDE [relay-selector-hybrid-connections](../../includes/relay-selector-hybrid-connections.md)]

<a id="what-will-be-accomplished" class="xliff"></a>

## Wat wordt bereikt

Aangezien hybride verbindingen zowel een client- als een serveronderdeel vereisen, maken we in deze zelfstudie twee consoletoepassingen. Dit zijn de stappen:

1. Een Relay-naamruimte maken met behulp van Azure Portal.
2. Een hybride verbinding maken met behulp van Azure Portal.
3. Een serverconsoletoepassing schrijven om berichten te ontvangen.
4. Een clientconsoletoepassing schrijven om berichten te verzenden.

<a id="prerequisites" class="xliff"></a>

## Vereisten

1. [Node.js](https://nodejs.org/en/) (dit voorbeeld gebruikt Node 7.0).
2. Een Azure-abonnement.

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

<a id="1-create-a-namespace-using-the-azure-portal" class="xliff"></a>

## 1. Een naamruimte maken met de Azure-portal

Als u al een Relay-naamruimte hebt gemaakt, gaat u naar het gedeelte [Een hybride verbinding maken met behulp van Azure Portal](#2-create-a-hybrid-connection-using-the-azure-portal).

[!INCLUDE [relay-create-namespace-portal](../../includes/relay-create-namespace-portal.md)]

<a id="2-create-a-hybrid-connection-using-the-azure-portal" class="xliff"></a>

## 2. Een hybride verbinding maken met behulp van Azure Portal

Als u al een hybride verbinding hebt gemaakt, gaat u naar het gedeelte [Een servertoepassing maken](#3-create-a-server-application-listener).

[!INCLUDE [relay-create-hybrid-connection-portal](../../includes/relay-create-hybrid-connection-portal.md)]

<a id="3-create-a-server-application-listener" class="xliff"></a>

## 3. Een servertoepassing (listener) maken

We maken een Node.js-consoletoepassing om berichten van de Relay te beluisteren en te ontvangen.

[!INCLUDE [relay-hybrid-connections-dotnet-get-started-server](../../includes/relay-hybrid-connections-node-get-started-server.md)]

<a id="4-create-a-client-application-sender" class="xliff"></a>

## 4. Een clienttoepassing maken (afzender)

We maken een Node.js-consoletoepassing om berichten naar de Relay te verzenden.

[!INCLUDE [relay-hybrid-connections-dotnet-get-started-client](../../includes/relay-hybrid-connections-node-get-started-client.md)]

<a id="5-run-the-applications" class="xliff"></a>

## 5. De toepassingen uitvoeren

1. Voer de servertoepassing uit.
2. Voer de clienttoepassing uit en voer wat tekst in.
3. Zorg ervoor dat de servertoepassingsconsole de tekst uitvoert die in de clienttoepassing is ingevoerd.

![actieve-toepassingen](./media/relay-hybrid-connections-node-get-started/running-applications.png)

Gefeliciteerd, u hebt een end-to-end toepassing met hybride verbindingen gemaakt.

<a id="next-steps" class="xliff"></a>

## Volgende stappen:

* [Veelgestelde vragen over Relay](relay-faq.md)
* [Een naamruimte maken](relay-create-namespace-portal.md)
* [Aan de slag met .NET](relay-hybrid-connections-dotnet-get-started.md)
* [Aan de slag met knooppunten](relay-hybrid-connections-node-get-started.md)


