---
title: Voorbeelden van Azure PowerShell - App Service | Microsoft Docs
description: Voorbeelden van Azure PowerShell - App Service
services: app-service
documentationcenter: app-service
author: syntaxc4
manager: erikre
editor: ggailey777
tags: azure-service-management
ms.assetid: b48d1137-8c04-46e0-b430-101e07d7e470
ms.service: app-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: app-service
ms.date: 03/08/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: 3254fdd57cfcd170f22374c1e3b058e6081d8e8e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="azure-powershell-samples"></a>Voorbeelden van Azure PowerShell

De volgende tabel bevat koppelingen naar bash-scripts die zijn gebouwd met behulp van Azure PowerShell.

| | |
|-|-|
|**App maken**||
| [Een web-app maken met de implementatie van GitHub](./scripts/app-service-powershell-deploy-github.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Hiermee maakt u een Azure-web-app dat code vanuit GitHub haalt. |
| [Een web-app maken met doorlopende implementatie vanuit GitHub](./scripts/app-service-powershell-continuous-deployment-github.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Hiermee maakt u een Azure-web-app dat continu code vanuit GitHub implementeert. |
| [Een web-app maken en implementeren van code met FTP](./scripts/app-service-powershell-deploy-ftp.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Maakt een Azure-web-app en het uploaden van bestanden van een lokale map met FTP. |
| [Een web-app maken en code implementeren vanuit een lokale Git-opslagplaats](./scripts/app-service-powershell-deploy-local-git.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Een Azure-web-app maakt en configureert u code push van een lokale Git-opslagplaats. |
| [Een web-app maken en code implementeren in een faseringsomgeving](./scripts/app-service-powershell-deploy-staging-environment.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Een Azure-web-app maakt met een implementatiesleuf voor het Faseren van codewijzigingen. |
|**App configureren**||
| [Een aangepast domein aan een web-app toewijzen](./scripts/app-service-powershell-configure-custom-domain.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Een Azure-web-app maakt en wijst een aangepaste domeinnaam aan. |
| [Een aangepaste SSL-certificaat binden aan een web-app](./scripts/app-service-powershell-configure-ssl-certificate.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Een Azure-web-app maakt en koppelt het SSL-certificaat van een aangepaste domeinnaam aan deze. |
|**Scale-app**||
| [Een web-app handmatig schalen](./scripts/app-service-powershell-scale-manual.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Een Azure-web-app maakt en over 2 exemplaren op schaal. |
| [Een web-app wereldwijd schalen met een architectuur voor hoge beschikbaarheid](./scripts/app-service-powershell-scale-high-availability.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Maakt twee Azure-web-apps in twee verschillende geografische regio's en maakt u ze beschikbaar zijn via één eindpunt met behulp van Azure Traffic Manager. |
|**App verbinden met resources**||
| [Een WebApp verbinden met een SQL-Database](./scripts/app-service-powershell-connect-to-sql.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Hiermee maakt u een Azure-web-app en een SQL-database en vervolgens wordt de databaseverbindingsreeks toegevoegd aan de app-instellingen. |
| [Een web-app verbinden met een opslagaccount](./scripts/app-service-powershell-connect-to-storage.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Hiermee maakt u een Azure-web-app en een opslagaccount en vervolgens voegt u de verbindingsreeks voor opslag toe aan de app-instellingen. |
|**App controleren**||
| [Een web-app bewaken met webserverlogboeken](./scripts/app-service-powershell-monitor.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Een Azure-web-app maakt, schakelt logboekregistratie voor het en downloadt de logboeken naar uw lokale computer. |
| | |
