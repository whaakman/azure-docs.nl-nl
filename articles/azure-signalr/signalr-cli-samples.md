---
title: Azure CLI-voorbeelden - Azure SignalR-service | Microsoft Docs
description: Azure CLI-voorbeelden - Azure SignalR-service
services: signalr
documentationcenter: signalr
author: wesmc7777
manager: cfowler
editor: ''
tags: azure-service-management
ms.service: signalr
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: signalr
ms.date: 04/20/2018
ms.author: wesmc
ms.custom: mvc
ms.openlocfilehash: 6e76ee89160c84ff0f1033590d14c288f023f201
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
ms.locfileid: "33779851"
---
# <a name="azure-cli-samples"></a>Azure CLI-voorbeelden

De volgende tabel bevat links naar bash-scripts voor het maken van een Azure SignalR-service met behulp van Azure CLI.

| | |
|-|-|
|**Maken**||
| [Een nieuwe SignalR-service en resourcegroep maken](scripts/signalr-cli-create-service.md) | Hiermee maakt u een nieuwe resource voor een Azure SignalR-service in een nieuwe resourcegroep met een willekeurige naam.  |
|**Integreren**||
| [Een nieuwe SignalR-service maken en een web-app die is geconfigureerd voor het gebruik van SignalR](scripts/signalr-cli-create-with-app-service.md) | Hiermee maakt u een nieuwe resource voor een Azure SignalR-service in een nieuwe resourcegroep met een willekeurige naam. Hiermee worden ook een nieuwe web-app en een App Service-abonnement toegevoegd voor het hosten van een web-app van ASP.NET Core die de SignalR-service gebruikt. De web-app wordt geconfigureerd met een app-instelling om verbinding te maken met de nieuwe resource voor de SignalR-service. |
| [Een nieuwe SignalR-service maken en een web-app die is geconfigureerd voor het gebruik van SignalR en GitHub OAuth](scripts/signalr-cli-create-with-app-service-github-oauth.md) | Hiermee maakt u een nieuwe resource voor een Azure SignalR-service in een nieuwe resourcegroep met een willekeurige naam. Hiermee worden ook een nieuwe web-app in Azure en een hosting-abonnement toegevoegd voor het hosten van een web-app van ASP.NET Core die de SignalR-service gebruikt. De web-app wordt geconfigureerd met app-instellingen voor de verbindingsreeks voor de nieuwe SignalR-serviceresource, en met clientgeheimen ter ondersteuning van [GitHub-verificatie](https://developer.github.com/v3/guides/basics-of-authentication/), zoals wordt beschreven in de [zelfstudie over verificatie](signalr-authenticate-oauth.md). De web-app wordt ook geconfigureerd voor het gebruik van een implementatiebron van een lokale Git-opslagplaats. |
| | |
