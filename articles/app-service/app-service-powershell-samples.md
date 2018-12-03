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
ms.openlocfilehash: 211284f4eff112b9aebec7fa1a031f292a4a92f4
ms.sourcegitcommit: beb4fa5b36e1529408829603f3844e433bea46fe
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/22/2018
ms.locfileid: "52290736"
---
# <a name="azure-powershell-samples"></a>Voorbeelden van Azure PowerShell

De volgende tabel bevat koppelingen naar PowerShell-scripts die zijn gemaakt met behulp van Azure PowerShell.

| | |
|-|-|
|**App maken**||
| [Een app maken met implementatie vanuit GitHub](./scripts/app-service-powershell-deploy-github.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Hiermee maakt u een Azure-web-app die code ophaalt uit GitHub. |
| [Een app maken met continue implementatie vanuit GitHub](./scripts/app-service-powershell-continuous-deployment-github.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Hiermee maakt u een Azure-web-app die code implementeert vanuit GitHub. |
| [Een app maken en code implementeren via FTP](./scripts/app-service-powershell-deploy-ftp.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Hiermee maakt u een Azure-web-app en uploadt u bestanden uit een lokale map met behulp van FTP. |
| [Een app maken en code implementeren vanuit een lokale Git-opslagplaats](./scripts/app-service-powershell-deploy-local-git.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Hiermee maakt u een Azure-web-app en configureert u het pushen van code vanuit een lokale Git-opslagplaats. |
| [Een app maken en code implementeren in een faseringsomgeving](./scripts/app-service-powershell-deploy-staging-environment.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Hiermee maakt u een Azure-web-app met een implementatiesite voor het faseren van codewijzigingen. |
|**App configureren**||
| [Een aangepast domein toewijzen aan een app](./scripts/app-service-powershell-configure-custom-domain.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Hiermee maakt u een Azure-web-app en wijst u er een aangepaste domeinnaam aan toe. |
| [Een aangepast SSL-certificaat verbinden met een app](./scripts/app-service-powershell-configure-ssl-certificate.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Hiermee maakt u een Azure-web-app en verbindt u het SSL-certificaat van een aangepaste domeinnaam met de app. |
|**App schalen**||
| [Een app handmatig schalen](./scripts/app-service-powershell-scale-manual.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Hiermee maakt u een Azure-web-app en schaalt u deze over 2 exemplaren. |
| [Een app wereldwijd schalen met een architectuur voor hoge beschikbaarheid](./scripts/app-service-powershell-scale-high-availability.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Hiermee maakt u twee Azure-web-apps in twee verschillende geografische regio's en maakt u ze met behulp van Azure Traffic Manager beschikbaar via één eindpunt. |
|**App verbinden met resources**||
| [Een app verbinden met een SQL-database](./scripts/app-service-powershell-connect-to-sql.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Hiermee maakt u een Azure-web-app en een SQL-database, waarna u de verbindingsreeks van de database toevoegt aan de app-instellingen. |
| [Een app verbinden met een opslagaccount](./scripts/app-service-powershell-connect-to-storage.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Hiermee maakt u een Azure-web-app en een opslagaccount, waarna u de verbindingsreeks van de opslag toevoegt aan de app-instellingen. |
|**Back-up van app maken en terugzetten**||
| [Een back-up maken van een app](./scripts/app-service-powershell-backup-onetime.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Hiermee maakt u een Azure-web-app en maakt u er een eenmalige back-up van. |
| [Een geplande back-up maken voor een app](./scripts/app-service-powershell-backup-scheduled.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Hiermee maakt u een Azure-web-app en maakt u er een geplande back-up van. |
| [Een back-up van een app verwijderen](./scripts/app-service-powershell-backup-delete.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Hiermee verwijdert u een bestaande back-up voor een app. |
| [Een app terugzetten vanuit de back-up](./scripts/app-service-powershell-backup-restore.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Hiermee zet u een app terug vanuit een eerder voltooide back-up. |
| [Een back-up terugzetten binnen abonnementen](./scripts/app-service-powershell-backup-restore-diff-sub.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Hiermee zet u een web-app terug vanuit een back-up in een ander abonnement. |
|**App controleren**||
| [Een app bewaken met webserverlogboeken](./scripts/app-service-powershell-monitor.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Hiermee maakt u een Azure-web-app, schakelt u logboekregistratie voor de app in en downloadt u de logboeken naar uw lokale computer. |
| | |
