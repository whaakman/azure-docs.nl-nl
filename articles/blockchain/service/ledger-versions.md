---
title: De Azure Block Chain-service ondersteunt grootboek versies, patches en upgrades
description: Overzicht van de ondersteunde versies van de groot boeken in azure Block Chain Service, inclusief beleids regels voor systeem patches en door systeem beheer en door gebruikers beheerde upgrades.
services: azure-blockchain
keywords: Block Chain
author: PatAltimore
ms.author: patricka
ms.date: 05/02/2019
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: janders
manager: femila
ms.openlocfilehash: 6cded49f14ae90f84b18c0a37059677c6c1fed99
ms.sourcegitcommit: 39d95a11d5937364ca0b01d8ba099752c4128827
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/16/2019
ms.locfileid: "69562788"
---
# <a name="supported-azure-blockchain-service-ledger-versions"></a>Ondersteunde Azure Block Chain Service-grootboek versies

De Azure Block Chain-service maakt gebruik van het op Ethereum gebaseerde [quorum](https://www.goquorum.com/developers) grootboek dat is ontworpen voor de verwerking van persoonlijke trans acties binnen een groep bekende deel nemers, geïdentificeerd als een consortium in azure Block Chain service.

Op dit moment ondersteunt de Azure Block Chain-service [quorum versie 2.2.3](https://github.com/jpmorganchase/quorum/releases/tag/v2.2.3) en [Tessera-transactie beheer](https://github.com/jpmorganchase/tessera).

## <a name="managing-updates-and-upgrades"></a>Updates en upgrades beheren

Het maken van versies in quorum geschiedt via een grote, kleine en patch releases. Als de quorum versie bijvoorbeeld 2.0.1 is, wordt release type als volgt gecategoriseerd:

|Primaire | Secundaire  | Patch  |
| :--- | :----- | :----- |
| 2 | 0 | 1 | 

De Azure Block Chain-service werkt patch releases van quorum automatisch bij van bestaande actieve leden binnen 30 dagen na de beschik baarheid van quorum.

## <a name="availability-of-new-ledger-versions"></a>Beschik baarheid van nieuwe grootboek versies

De Azure Block Chain-service biedt de meest recente primaire en secundaire versies van het quorum grootboek binnen 60 dagen na de beschik baarheid van de quorum fabrikant. Er zijn Maxi maal vier secundaire releases beschikbaar voor consortia waaruit u kunt kiezen bij het inrichten van een nieuw lid en consortium. Een upgrade van naar een primaire of secundaire versie wordt momenteel niet ondersteund.

## <a name="next-steps"></a>Volgende stappen

[Limieten in azure Block Chain Service](limits.md)
