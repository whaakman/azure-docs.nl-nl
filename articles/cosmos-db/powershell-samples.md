---
title: Voorbeelden van Azure PowerShell voor Azure Cosmos DB | Microsoft Docs
description: Azure PowerShell-voorbeelden - Scripts voor hulp bij het maken en beheren van Azure DB die Cosmos-accounts.
services: cosmos-db
author: mimig1
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: cosmos-db
ms.custom: mvc
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: database
ms.date: 10/16/2017
ms.author: mimig
ms.openlocfilehash: f2687369b8e247f00e9de5f3f79d8e44be5b8300
ms.sourcegitcommit: bd0d3ae20773fc87b19dd7f9542f3960211495f9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/18/2017
---
# <a name="azure-powershell-samples-for-azure-cosmos-db"></a>Voorbeelden van Azure PowerShell voor Azure Cosmos-DB

De volgende tabel bevat koppelingen naar Azure PowerShell-voorbeeldscripts voor Azure Cosmos DB. Op dit moment kunt u alleen de accountlayer Azure Cosmos DB via PowerShell; beheren andere resources, zoals databases en verzamelingen kunnen niet worden beheerd via PowerShell.

| |  |
|---|---|
|**Een Azure DB die Cosmos-account maken**||
|[Een DocumentDB-API-account maken](scripts/create-database-account-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Maakt een enkele Azure DB die Cosmos-account gebruiken met de DocumentDB-API. |
|**Azure Cosmos DB schalen**||
|[Azure DB die Cosmos-account in meerdere regio's worden gerepliceerd en configureren van failover-prioriteiten](scripts/scale-multiregion-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)|Globaal repliceert accountgegevens naar meerdere regio's met een opgegeven failover-prioriteit.|
|**Azure Cosmos DB beveiligen**||
| [Sleutels van account ophalen](scripts/secure-get-account-key-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Hiermee haalt u de primaire en secundaire master schrijven en primaire en secundaire alleen-lezen sleutels voor het account.|
| [MongoDB-verbindingsreeks ophalen](scripts/secure-mongo-connection-string-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Hiermee haalt u de verbindingsreeks naar uw MongoDB-app verbinden met uw Azure DB die Cosmos-account.|
|[Opnieuw genereren van sleutels](scripts/secure-regenerate-key-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)|De sleutel van het model of de alleen-lezen voor het account genereert.|
|[Maken van een firewall](scripts/create-firewall-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Hiermee maakt u een binnenkomende IP-beleid voor toegangsbeheer om aan het account de toegang beperken tot een goedgekeurde aantal machines en/of cloudservices.|
|**Hoge beschikbaarheid, herstel na noodgevallen, back-up en herstel**||
|[Failover-beleid configureren](scripts/ha-failover-policy-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)|Hiermee stelt u de prioriteit van de failover van elke regio waarin het account wordt gerepliceerd.|
|||
