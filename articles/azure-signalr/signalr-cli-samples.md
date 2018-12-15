---
title: Azure CLI-voorbeelden - Azure SignalR-service
description: Azure CLI-voorbeelden - Azure SignalR-service
author: sffamily
ms.service: signalr
ms.topic: sample
ms.date: 04/20/2018
ms.author: zhshang
ms.custom: mvc
ms.openlocfilehash: a39f0abf4f3a8cc30d6f8f83dcad7e9069f348e4
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2018
ms.locfileid: "53258777"
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