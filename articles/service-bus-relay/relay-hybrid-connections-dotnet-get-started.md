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
ms.date: 07/07/2017
ms.author: sethm
ms.openlocfilehash: 1af23bfd46dd7d3781505473f7c1d86e65ea9bc7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-with-relay-hybrid-connections"></a>Aan de slag met hybride Relay-verbindingen
[!INCLUDE [relay-selector-hybrid-connections](../../includes/relay-selector-hybrid-connections.md)]

Deze zelfstudie biedt een inleiding tot [hybride Relay-verbindingen van Azure](relay-what-is-it.md#hybrid-connections) en laat zien hoe u .NET gebruikt om een clienttoepassing te maken waarmee berichten worden verzonden naar een corresponderende listener-toepassing. 

## <a name="what-will-be-accomplished"></a>Wat wordt bereikt
Omdat hybride verbindingen zowel een client- als een serveronderdeel vereisen, worden in de zelfstudie twee consoletoepassingen gemaakt. Dit zijn de stappen:

1. Een Relay-naamruimte maken met behulp van Azure Portal.
2. Een hybride verbinding maken in die naamruimte, met behulp van Azure Portal.
3. Een serverconsoletoepassing (listener) schrijven om berichten te ontvangen.
4. Een clientconsoletoepassing (afzender) schrijven om berichten te verzenden.

## <a name="prerequisites"></a>Vereisten

Voor het voltooien van deze zelfstudie moet aan de volgende vereisten worden voldaan:

1. [Visual Studio 2015 of hoger](http://www.visualstudio.com). In de voorbeelden in deze zelfstudie wordt Visual Studio 2017 gebruikt.
2. Een Azure-abonnement.

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="1-create-a-namespace-using-the-azure-portal"></a>1. Een naamruimte maken met de Azure-portal
Als u al een Relay-naamruimte hebt gemaakt, gaat u naar de sectie [Een hybride verbinding maken met behulp van Azure Portal](#2-create-a-hybrid-connection-using-the-azure-portal).

[!INCLUDE [relay-create-namespace-portal](../../includes/relay-create-namespace-portal.md)]

## <a name="2-create-a-hybrid-connection-using-the-azure-portal"></a>2. Een hybride verbinding maken met behulp van Azure Portal
Als u al een hybride verbinding hebt gemaakt, gaat u naar de sectie [Een servertoepassing maken](#3-create-a-server-application-listener).

[!INCLUDE [relay-create-hybrid-connection-portal](../../includes/relay-create-hybrid-connection-portal.md)]

## <a name="3-create-a-server-application-listener"></a>3. Een servertoepassing (listener) maken
We maken een C#-consoletoepassing met Visual Studio om berichten van de Relay te beluisteren en te ontvangen.

[!INCLUDE [relay-hybrid-connections-dotnet-get-started-server](../../includes/relay-hybrid-connections-dotnet-get-started-server.md)]

## <a name="4-create-a-client-application-sender"></a>4. Een clienttoepassing maken (afzender)
We maken een C#-consoletoepassing met Visual Studio om berichten naar de Relay te verzenden.

[!INCLUDE [relay-hybrid-connections-dotnet-get-started-client](../../includes/relay-hybrid-connections-dotnet-get-started-client.md)]

## <a name="5-run-the-applications"></a>5. De toepassingen uitvoeren
1. Voer de servertoepassing uit.
2. Voer de clienttoepassing uit en voer wat tekst in.
3. Zorg ervoor dat de servertoepassingsconsole de tekst uitvoert die in de clienttoepassing is ingevoerd.

![actieve-toepassingen](./media/relay-hybrid-connections-dotnet-get-started/running-applications.png)

Gefeliciteerd, u hebt een end-to-end toepassing met hybride verbindingen gemaakt.

## <a name="next-steps"></a>Volgende stappen:
* [Veelgestelde vragen over Relay](relay-faq.md)
* [Een naamruimte maken](relay-create-namespace-portal.md)
* [Aan de slag met knooppunten](relay-hybrid-connections-node-get-started.md)

