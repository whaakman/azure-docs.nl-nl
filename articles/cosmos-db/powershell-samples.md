---
title: Voorbeelden van Azure PowerShell voor Azure Cosmos DB
description: 'Azure PowerShell-voorbeelden: scripts voor het maken en beheren van Azure Cosmos DB-accounts.'
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 05/08/2019
ms.author: mjbrown
ms.openlocfilehash: 68e845a05f4ebe2d1f25b55c00042c8925c8109e
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65069308"
---
# <a name="azure-powershell-samples-for-azure-cosmos-db"></a>Voorbeelden van Azure PowerShell voor Azure Cosmos DB

De volgende tabel bevat koppelingen naar Azure PowerShell-voorbeeldscripts voor Azure Cosmos DB voor Core (SQL) API.

| |  |
|---|---|
|**Azure Cosmos-accounts**||
|[Een account maken](scripts/powershell/sql/ps-account-create.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Hiermee maakt u een SQL API van Azure Cosmos-account. |
|[Maak een account](scripts/powershell/sql/ps-account-get.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Hiermee worden de eigenschappen van een Azure Cosmos-account. |
|[Een regio toevoegen](scripts/powershell/sql/ps-account-update.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Maak een Azure Cosmos-account en een regio toevoegt aan de lijst met locaties. |
|[De failover-prioriteit wijzigen](scripts/powershell/sql/ps-account-failover-priority-update.md?toc=%2fpowershell%2fmodule%2ftoc.json)| De failover-prioriteit van een Azure Cosmos-account met een handmatige failover-trigger wijzigen. |
|[Bijwerken van tags](scripts/powershell/sql/ps-account-tags-update.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Bijwerken van de tags voor een Azure Cosmos-account. |
|[Accountsleutels ophalen](scripts/powershell/sql/ps-account-key-get.md?toc=%2fpowershell%2fmodule%2ftoc.json)| De primaire en secundaire sleutels ophalen van een Azure Cosmos-account. |
|[Accountsleutels opnieuw genereren](scripts/powershell/sql/ps-account-key-regenerate.md?toc=%2fpowershell%2fmodule%2ftoc.json)| De primaire en secundaire sleutels van een Azure Cosmos-account opnieuw genereren. |
|[Lijst met verbindingsreeksen](scripts/powershell/sql/ps-account-connection-string-get.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Haal de primaire en secundaire verbindingsreeksen van een Azure Cosmos-account. |
|[IP-Firewall maken](scripts/powershell/sql/ps-account-firewall-create.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Een IP-Firewall voor een Azure Cosmos-account maken. |
|[Een Azure Cosmos-account verwijderen](scripts/powershell/sql/ps-account-delete.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Een Azure Cosmos-account verwijderen. |
|**Azure Cosmos-databases**||
| [Een database maken](scripts/powershell/sql/ps-database-create.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Maak een database binnen een Azure Cosmos-account.|
| [Een database maken met gedeelde/op databaseniveau doorvoer](scripts/powershell/sql/ps-database-create-shared.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Maak een Azure Cosmos-database met op databaseniveau doorvoer die wordt gedeeld met de containers.|
| [Lijst van alle databases](scripts/powershell/sql/ps-database-list.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Lijst met alle databases in een Azure Cosmos-account.|
| [Een database ophalen](scripts/powershell/sql/ps-database-get.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Hiermee worden de eigenschappen van een Azure Cosmos-database.|
|**Azure Cosmos-containers**||
| [Een container maken](scripts/powershell/sql/ps-container-create.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Een Azure Cosmos-container maken met speciaal toegewezen doorvoer.|
| [Een container maken met gedeelde doorvoer](scripts/powershell/sql/ps-container-create-shared.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Een Azure Cosmos-container maken met de doorvoer die worden gedeeld met andere containers in de database.|
| [Een container maken met index-beleid](scripts/powershell/sql/ps-container-create-index-custom.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Een Azure Cosmos-container maken met een beleid voor aangepaste index.|
| [Een container maken met geen index-beleid](scripts/powershell/sql/ps-container-create-index-none.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Een Azure Cosmos-container maken met index beleid is uitgeschakeld.|
| [Een container maken met unieke sleutels & TTL](scripts/powershell/sql/ps-container-create-unique-key-ttl.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Maak een Azure-Cosmos-container met een unieke key-beperking en time-to-live geconfigureerd.|
| [Een container maken met conflictoplossing](scripts/powershell/sql/ps-container-create-conflict-policy.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Een Azure Cosmos-container maken met de laatste schrijver wins resolutie beleid conflicteren.|
| [Lijst van alle containers](scripts/powershell/sql/ps-container-list.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Lijst van alle containers in een Azure Cosmos-database.|
| [Een container ophalen](scripts/powershell/sql/ps-container-get.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Hiermee worden de eigenschappen voor een container in een Azure Cosmos-database.|
| [Container verwijderen](scripts/powershell/sql/ps-container-delete.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Verwijderen van een container in een Azure Cosmos-database.|
|||