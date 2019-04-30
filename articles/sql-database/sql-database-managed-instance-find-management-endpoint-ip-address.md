---
title: Ontdek Azure SQL Database Managed Instance-beheereindpunt | Microsoft Docs
description: Meer informatie over het krijgen van Azure SQL Database Managed Instance management eindpunt openbare IP-adres te controleren of de ingebouwde firewall-beveiliging
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: sstein, carlrab
manager: craigg
ms.date: 12/04/2018
ms.openlocfilehash: b7eb9ecd6b94aad263346ad6b5c45b694e0bd46f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60699979"
---
# <a name="determine-the-management-endpoint-ip-address"></a>Bepalen van het IP-adres van de management-eindpunt

De virtuele Azure SQL Database Managed Instance-cluster bevat een beheereindpunt die gebruikmaakt van Microsoft voor beheerbewerkingen. Het eindpunt is beveiligd met een ingebouwde firewall op het netwerk niveau en wederzijdse certificaatverificatie op het toepassingsniveau van de. U kunt het IP-adres van het beheereindpunt bepalen, maar u geen toegang tot dit eindpunt.

## <a name="determine-ip-address"></a>IP-adres bepalen

We gaan ervan uit dat Managed Instance host `mi-demo.xxxxxx.database.windows.net`. Voer `nslookup` met behulp van de hostnaam.

![Het omzetten van interne host-naam](./media/sql-database-managed-instance-management-endpoint/01_find_internal_host.png)

Voer een andere `nslookup` voor het verwijderen van de naam van de gemarkeerde de `.vnet.` segment. U krijgt het openbare IP-adres wanneer u deze opdracht wordt uitgevoerd.

![Het omzetten van openbare IP-adres](./media/sql-database-managed-instance-management-endpoint/02_find_public_ip.png)

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over beheerde exemplaren en -connectiviteit, [Azure SQL-Database beheerd exemplaar Connectiviteitsarchitectuur](sql-database-managed-instance-connectivity-architecture.md).
