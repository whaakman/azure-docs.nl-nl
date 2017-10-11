---
title: Voorbeelden van Azure CLI - App Service | Microsoft Docs
description: Voorbeelden van Azure CLI - App Service
services: app-service
documentationcenter: app-service
author: syntaxc4
manager: erikre
editor: ggailey777
tags: azure-service-management
ms.assetid: 53e6a15a-370a-48df-8618-c6737e26acec
ms.service: app-service
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: app-service
ms.date: 03/08/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: 6718694af487929d193dae54ecb2d85ece64887a
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="azure-cli-samples"></a>Voorbeelden van Azure CLI

De volgende tabel bevat koppelingen naar bash-scripts die zijn gebouwd met behulp van de Azure CLI.

| | |
|-|-|
|**App maken**||
| [Een web-app maken en code implementeren vanuit GitHub](./scripts/app-service-cli-deploy-github.md?toc=%2fcli%2fazure%2ftoc.json)| Een Azure-web-app maakt en implementeert de code van een openbare GitHub-opslagplaats. |
| [Een web-app maken met doorlopende implementatie vanuit GitHub](./scripts/app-service-cli-continuous-deployment-github.md?toc=%2fcli%2fazure%2ftoc.json)| Hiermee maakt een Azure-web-app met continue publiceren vanaf een GitHub-opslagplaats die u bezit. |
| [Een web-app maken en code implementeren vanuit een lokale Git-opslagplaats](./scripts/app-service-cli-deploy-local-git.md?toc=%2fcli%2fazure%2ftoc.json) | Een Azure-web-app maakt en configureert u code push van een lokale Git-opslagplaats. |
| [Een web-app maken en code implementeren in een faseringsomgeving](./scripts/app-service-cli-deploy-staging-environment.md?toc=%2fcli%2fazure%2ftoc.json) | Een Azure-web-app maakt met een implementatiesleuf voor het Faseren van codewijzigingen. |
| [Een ASP.NET Core-web-app maken in een Docker-container](./scripts/app-service-cli-linux-docker-aspnetcore.md?toc=%2fcli%2fazure%2ftoc.json)| Een Azure-web-app maakt op Linux en laadt een Docker-afbeelding van Docker-Hub. |
|**App configureren**||
| [Een aangepast domein aan een web-app toewijzen](./scripts/app-service-cli-configure-custom-domain.md?toc=%2fcli%2fazure%2ftoc.json)| Een Azure-web-app maakt en wijst een aangepaste domeinnaam aan. |
| [Een aangepaste SSL-certificaat binden aan een web-app](./scripts/app-service-cli-configure-ssl-certificate.md?toc=%2fcli%2fazure%2ftoc.json)| Een Azure-web-app maakt en koppelt het SSL-certificaat van een aangepaste domeinnaam aan deze. |
|**Scale-app**||
| [Een web-app handmatig schalen](./scripts/app-service-cli-scale-manual.md?toc=%2fcli%2fazure%2ftoc.json) | Een Azure-web-app maakt en over 2 exemplaren op schaal. |
| [Een web-app wereldwijd schalen met een architectuur voor hoge beschikbaarheid](./scripts/app-service-cli-scale-high-availability.md?toc=%2fcli%2fazure%2ftoc.json) | Maakt twee Azure-web-apps in twee verschillende geografische regio's en maakt u ze beschikbaar zijn via één eindpunt met behulp van Azure Traffic Manager. |
|**App verbinden met resources**||
| [Een WebApp verbinden met een SQL-Database](./scripts/app-service-cli-app-service-sql.md?toc=%2fcli%2fazure%2ftoc.json)| Hiermee maakt u een Azure-web-app en een SQL-database en vervolgens wordt de databaseverbindingsreeks toegevoegd aan de app-instellingen. |
| [Een web-app verbinden met een opslagaccount](./scripts/app-service-cli-app-service-storage.md?toc=%2fcli%2fazure%2ftoc.json)| Hiermee maakt u een Azure-web-app en een opslagaccount en vervolgens voegt u de verbindingsreeks voor opslag toe aan de app-instellingen. |
| [Een WebApp verbinden met een redis-cache](./scripts/app-service-cli-app-service-redis.md?toc=%2fcli%2fazure%2ftoc.json) | Hiermee maakt u een Azure-web-app en een redis-cache en vervolgens voegt u de details van de redis-verbinding toe aan de app-instellingen.) |
| [Een WebApp verbinden met Cosmos-DB](./scripts/app-service-cli-app-service-documentdb.md?toc=%2fcli%2fazure%2ftoc.json) | Hiermee maakt u een Azure-web-app en een Cosmos-database en vervolgens voegt u de details van de Cosmos-DB-verbinding toe aan de app-instellingen. |
|**App controleren**||
| [Een web-app bewaken met webserverlogboeken](./scripts/app-service-cli-monitor.md?toc=%2fcli%2fazure%2ftoc.json) | Een Azure-web-app maakt, schakelt logboekregistratie voor het en downloadt de logboeken naar uw lokale computer. |
| | |