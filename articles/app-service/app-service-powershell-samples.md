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
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: app-service
ms.date: 03/08/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: ba2bd2b185c395e54f2f085317a424a2aa1b4421
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2018
---
# <a name="azure-powershell-samples"></a>Voorbeelden van Azure PowerShell

De volgende tabel bevat koppelingen naar de PowerShell-scripts die zijn gebouwd met behulp van Azure PowerShell.

| | |
|-|-|
|**App maken**||
| [Een web-app maken met de implementatie van GitHub](./scripts/app-service-powershell-deploy-github.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Hiermee maakt u een Azure-web-app die code vanuit GitHub haalt. |
| [Een web-app maken met doorlopende implementatie vanuit GitHub](./scripts/app-service-powershell-continuous-deployment-github.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Hiermee maakt u een Azure-web-app die continu code vanuit GitHub implementeert. |
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
|**Back-up en herstellen van app**||
| [Back-up van een web-app](./scripts/app-service-powershell-backup-onetime.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Een Azure-web-app maakt en maakt een eenmalige back-up voor. |
| [Een geplande back-up voor een web-app maken](./scripts/app-service-powershell-backup-scheduled.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Een Azure-web-app maakt en maakt een geplande back-up voor. |
| [Een back-up voor een web-app verwijderen](./scripts/app-service-powershell-backup-delete.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Hiermee verwijdert u een bestaande back-up voor een web-app. |
|**App controleren**||
| [Een web-app bewaken met webserverlogboeken](./scripts/app-service-powershell-monitor.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Een Azure-web-app maakt, schakelt logboekregistratie voor het en downloadt de logboeken naar uw lokale computer. |
| | |
