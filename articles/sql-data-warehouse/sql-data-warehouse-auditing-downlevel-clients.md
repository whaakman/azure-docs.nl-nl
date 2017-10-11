---
title: Ondersteuning voor SQL Data Warehouse downlevel-clients voor het controleren van de gegevens | Microsoft Docs
description: Meer informatie over SQL Data Warehouse-ondersteuning voor downlevel-clients voor het controleren van gegevens
services: sql-data-warehouse
documentationcenter: 
author: ronortloff
manager: jhubbard
editor: 
ms.assetid: dfe29ff3-dfeb-4309-83c0-c1a300f4f44e
ms.service: sql-database
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: security
ms.date: 10/31/2016
ms.author: rortloff;barbkess
ms.openlocfilehash: a7ea6141285a0098339f1e071af2592dd4535c12
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="sql-data-warehouse----downlevel-clients-support-for-auditing-and-dynamic-data-masking"></a>Ondersteuning voor SQL datawarehouse - Downlevel-clients voor controle en dynamische Gegevensmaskering
[Controle](sql-data-warehouse-auditing-overview.md) werkt met SQL-clients die ondersteuning bieden voor omleiden van TDS.

Omleiding moet ook ondersteuning voor elke client waarmee de TDS-7.4 wordt geïmplementeerd. Uitzonderingen op deze omvatten JDBC 4.0 waarin de functie voor omleiding wordt niet volledig ondersteund en Tedious voor Node.JS in welke omleiding is niet geïmplementeerd.

Voor 'Downlevel-clients', moet dat wil zeggen welke ondersteuning TDS 7.3 en hieronder - de FQDN-naam van de server in de verbinding versietekenreeks worden gewijzigd:

Oorspronkelijke server FQDN-naam in de verbindingsreeks: <*servernaam*>. database.windows.net

Gewijzigde FQDN van de server in de verbindingsreeks: <*servernaam*> .database. **beveiligde**. windows.net

Een gedeeltelijke lijst met 'Downlevel-clients' omvat:

* .NET 4.0 en lager,
* ODBC-10.0 en lager.
* JDBC (Hoewel JDBC TDS 7.4 ondersteunt, de TDS-omleiding-functie is niet volledig ondersteund)
* Omslachtig (voor Node.JS)

**Opmerking:** de bovenstaande server FDQN wijziging mogelijk handig ook voor het toepassen van een beleid voor controle van SQL Server op zonder behoefte aan een configuratie stap in elke database (tijdelijke risicobeperking).     

