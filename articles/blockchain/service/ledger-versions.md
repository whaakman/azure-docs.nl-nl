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
ms.openlocfilehash: 63c9a8b9e266dacbb0fb6faba50fb44ac9a4b46e
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/02/2019
ms.locfileid: "65027883"
---
# <a name="supported-azure-blockchain-service-ledger-versions"></a>Ondersteunde versies van Azure Blockchain Service grootboek

Azure Blockchain-Service gebruikt de Ethereum-gebaseerde [Quorum](https://github.com/jpmorganchase/quorum/wiki) grootboek is ontworpen voor de verwerking van persoonlijke transacties binnen een groep bekende deelnemers, geïdentificeerd als een consortium in Azure Blockchain-Service.

Azure Blockchain-Service ondersteunt momenteel [Quorum versie 2.2.1](https://github.com/jpmorganchase/quorum/releases/tag/v2.2.1) en [Tessera transactiebeheer](https://github.com/jpmorganchase/tessera).

## <a name="managing-updates-and-upgrades"></a>Beheren van updates en upgrades

Versiebeheer in Quorum wordt gedaan door een grote release, belangrijke punt release en een secundaire point-release. Als het Quorum versie 2.0.1, 2 een grote release is, 0 is een belangrijke punt release en 1 kleine punt versie is. De service wordt automatisch patches kleine punt versies van het grootboek. Upgrade primaire en het primaire versies worden op dit moment niet ondersteund.

## <a name="availability-of-new-ledger-versions"></a>Beschikbaarheid van nieuwe versies van het grootboek

Azure Blockchain-Service biedt de nieuwste versies van het grootboek binnen 60 dagen beschikbaar worden gesteld van de fabrikant grootboek. Maximaal vier primaire punt versies zijn beschikbaar voor consortiums om de verkeersbelasting bij het inrichten van een nieuw lid en consortium.

## <a name="next-steps"></a>Volgende stappen

[Limieten in Azure Blockchain-Service](limits.md)
