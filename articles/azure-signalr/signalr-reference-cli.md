---
title: Azure CLI-voorbeelden - Azure SignalR-service
description: Azure CLI-voorbeelden - Azure SignalR-service
author: sffamily
ms.service: signalr
ms.topic: reference
ms.date: 03/01/2019
ms.author: zhshang
ms.openlocfilehash: dedd9d14bafb342b0b34e3685ef3b7a46a54f9c8
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57555012"
---
# <a name="azure-cli-reference"></a>Naslaginformatie over Azure CLI

De volgende tabel bevat links naar bash-scripts voor het maken van een Azure SignalR-service met behulp van Azure CLI.

| | |
|-|-|
|**Maken**||
| [Een nieuwe SignalR-service en resourcegroep maken](scripts/signalr-cli-create-service.md) | Hiermee maakt u een nieuwe resource voor een Azure SignalR-service in een nieuwe resourcegroep met een willekeurige naam.  |
|**Integreren**||
| [Een nieuwe SignalR-service maken en een web-app die is geconfigureerd voor het gebruik van SignalR](scripts/signalr-cli-create-with-app-service.md) | Hiermee maakt u een nieuwe resource voor een Azure SignalR-service in een nieuwe resourcegroep met een willekeurige naam. Hiermee worden ook een nieuwe web-app en een App Service-abonnement toegevoegd voor het hosten van een web-app van ASP.NET Core die de SignalR-service gebruikt. De web-app wordt geconfigureerd met een app-instelling om verbinding te maken met de nieuwe resource voor de SignalR-service. |
| [Een nieuwe SignalR-service maken en een web-app die is geconfigureerd voor het gebruik van SignalR en GitHub OAuth](scripts/signalr-cli-create-with-app-service-github-oauth.md) | Hiermee maakt u een nieuwe resource voor een Azure SignalR-service in een nieuwe resourcegroep met een willekeurige naam. Hiermee worden ook een nieuwe web-app in Azure en een hosting-abonnement toegevoegd voor het hosten van een web-app van ASP.NET Core die de SignalR-service gebruikt. De web-app wordt geconfigureerd met app-instellingen voor de verbindingsreeks voor de nieuwe SignalR-serviceresource, en met clientgeheimen ter ondersteuning van [GitHub-verificatie](https://developer.github.com/v3/guides/basics-of-authentication/), zoals wordt beschreven in de [zelfstudie over verificatie](signalr-concept-authenticate-oauth.md). De web-app wordt ook geconfigureerd voor het gebruik van een implementatiebron van een lokale Git-opslagplaats. |
| | |