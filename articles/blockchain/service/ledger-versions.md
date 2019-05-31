---
title: Azure Blockchain-Service ondersteund grootboek versies, patchen en upgrade
description: Overzicht van de ondersteunde grootboeken versies in Azure Blockchain-Service, met inbegrip van beleid met betrekking tot de systemen toepassen van patches en systeem beheerde en beheerde gebruiker upgrades.
services: azure-blockchain
keywords: blockchain
author: PatAltimore
ms.author: patricka
ms.date: 05/02/2019
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: janders
manager: femila
ms.openlocfilehash: 53f65ec91a1e0f1e5a6322f0125bf83cd3e400b2
ms.sourcegitcommit: d89032fee8571a683d6584ea87997519f6b5abeb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/30/2019
ms.locfileid: "66399109"
---
# <a name="supported-azure-blockchain-service-ledger-versions"></a>Ondersteunde versies van Azure Blockchain Service grootboek

Azure Blockchain-Service gebruikt de Ethereum-gebaseerde [Quorum](https://www.goquorum.com/developers) grootboek is ontworpen voor de verwerking van persoonlijke transacties binnen een groep bekende deelnemers, geïdentificeerd als een consortium in Azure Blockchain-Service.

Azure Blockchain-Service ondersteunt momenteel [Quorum versie 2.2.1](https://github.com/jpmorganchase/quorum/releases/tag/v2.2.1) en [Tessera transactiebeheer](https://github.com/jpmorganchase/tessera).

## <a name="managing-updates-and-upgrades"></a>Beheren van updates en upgrades

Versiebeheer in Quorum wordt gedaan door een primaire, secundaire en versies van de patch. Bijvoorbeeld, als de versie van het Quorum 2.0.1, releasetype zou als volgt worden ingedeeld:

|Primaire | Secundaire  | Patch  |
| :--- | :----- | :----- |
| 2 | 0 | 1 | 

Patch-versies van Quorum Azure Blockchain-Service automatisch bijgewerkt naar bestaande actieve leden binnen 30 dagen na de lancering van Quorum.

## <a name="availability-of-new-ledger-versions"></a>Beschikbaarheid van nieuwe versies van het grootboek

Azure Blockchain-Service biedt de meest recente primaire en secundaire versies van het Quorum grootboek binnen 60 dagen beschikbaar worden gesteld van de Quorum-fabrikant. Maximaal vier secundaire versies zijn beschikbaar voor consortiums om de verkeersbelasting bij het inrichten van een nieuw lid en consortium. Upgrade uit naar een primaire of secundaire versie is momenteel niet ondersteund.

## <a name="next-steps"></a>Volgende stappen

[Limieten in Azure Blockchain-Service](limits.md)
