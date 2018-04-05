---
title: Voorbeelden van Azure PowerShell voor Azure Cosmos DB | Microsoft Docs
description: 'Azure PowerShell-voorbeelden: scripts voor het maken en beheren van Azure Cosmos DB-accounts.'
services: cosmos-db
author: mimig1
manager: jhubbard
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: cosmos-db
ms.custom: mvc
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: database
ms.date: 10/16/2017
ms.author: mimig
ms.openlocfilehash: f651a88f71e62518d0531a2d5cee5c1cd2bc5ce4
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/28/2018
---
# <a name="azure-powershell-samples-for-azure-cosmos-db"></a>Voorbeelden van Azure PowerShell voor Azure Cosmos DB

De volgende tabel bevat links naar Azure PowerShell-voorbeeldscripts voor Azure Cosmos DB. Op dit moment kunt u alleen de accountlaag van Azure Cosmos DB beheren via PowerShell. Andere resources, zoals databases en verzamelingen, kunnen niet via PowerShell worden beheerd.

| |  |
|---|---|
|**Azure Cosmos DB-account maken**||
|[SQL API-account maken](scripts/create-database-account-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Hiermee wordt één Azure Cosmos DB-account gemaakt voor gebruik met de SQL-API. |
|**Azure Cosmos DB schalen**||
|[Azure Cosmos DB-account in meerdere regio's repliceren en failover-prioriteiten configureren](scripts/scale-multiregion-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)|Hiermee worden accountgegevens in meerdere regio's met een bepaalde failover-prioriteit globaal gerepliceerd.|
|**Azure Cosmos DB beveiligen**||
| [Accountsleutels ophalen](scripts/secure-get-account-key-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Hiermee worden de primaire en secundaire hoofdsleutels voor schrijven en de primaire en secundaire sleutels voor alleen-lezen voor het account opgehaald.|
| [MongoDB-verbindingsreeks ophalen](scripts/secure-mongo-connection-string-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Hiermee wordt de verbindingsreeks opgehaald om uw MongoDB-app te verbinden met uw Azure Cosmos DB-account.|
|[Accountsleutels opnieuw genereren](scripts/secure-regenerate-key-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)|Hiermee wordt de hoofdsleutel of alleen-lezensleutel voor het account opnieuw gegenereerd.|
|[Firewall maken](scripts/create-firewall-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Hiermee wordt toegangsbeheerbeleid voor inkomende IP's gemaakt om de toegang tot het account vanaf een goedgekeurde set computers en/of cloudservices te beperken.|
|**Hoge beschikbaarheid, herstel na noodgevallen, back-up en herstel**||
|[Failoverbeleid configureren](scripts/ha-failover-policy-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)|Hiermee wordt de prioriteit van failover ingesteld voor elke regio waarin het account wordt gerepliceerd.|
|||
