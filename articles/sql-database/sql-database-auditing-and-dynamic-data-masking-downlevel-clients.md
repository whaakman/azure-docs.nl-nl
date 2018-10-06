---
title: Controle, TDS-omleiding, tabel en IP-eindpunten voor Azure SQL Database | Microsoft Docs
description: Meer informatie over controle, TDS-omleiding en IP-eindpunt wijzigingen bij het implementeren van tabelcontrole in Azure SQL Database.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: ronitr
ms.author: ronitr
ms.reviewer: vanto
manager: craigg
ms.date: 04/01/2018
ms.openlocfilehash: 41509c1f35d42a1e8558299b4244ad515c36ec17
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/05/2018
ms.locfileid: "48816052"
---
# <a name="sql-database----downlevel-clients-support-and-ip-endpoint-changes-for-table-auditing"></a>SQL Database - ondersteuning voor Downlevel-clients en IP-eindpunt voor Tabelcontrole wordt gewijzigd

> [!IMPORTANT]
> Dit document geldt alleen voor Tabelcontrole, namelijk **gedeprecieerd**.<br>
> Gebruik de nieuwe [controlefunctie voor blobs](sql-database-auditing.md) methode die **niet** downlevel client connection string wijzigingen zijn vereist. Aanvullende informatie over de controlefunctie voor blobs te vinden in [aan de slag met SQL database auditing](sql-database-auditing.md).

[Controle van de database](sql-database-auditing.md) werkt automatisch met SQL-clients die ondersteuning bieden voor TDS-omleiding. Houd er rekening mee dat omleiding is niet van toepassing wanneer u de methode controlefunctie voor blobs.

## <a id="subheading-1"></a>Ondersteuning voor downlevel-clients
Omleiding moet ook ondersteuning voor elke client waarmee de TDS-7.4 wordt geïmplementeerd. Uitzonderingen op deze omvatten JDBC 4.0 waarin de functie voor omleiding wordt niet volledig ondersteund en Tedious voor Node.JS in waarvoor omleiding is niet geïmplementeerd.

Voor 'Downlevel-clients', moet dat wil zeggen welke ondersteuning TDS-versie 7.3 hieronder wordt weergegeven: de FQDN-naam van de server in de verbinding tekenreeks en worden gewijzigd:

Oorspronkelijke server FQDN-naam in de connection string: <*servernaam*>. database.windows.net

Gewijzigde server FQDN-naam in de connection string: <*servernaam*> .database. **beveiligde**. windows.net

Een gedeeltelijke lijst van 'Downlevel-clients' bevat:

* .NET 4.0 en lager,
* ODBC-10.0 en lager.
* JDBC (Hoewel JDBC TDS 7.4 ondersteunt, de functie voor TDS-omleiding wordt niet volledig ondersteund)
* Tedious (voor Node.JS)

**Opmerking:** de bovenstaande server FQDN wijzigingen zijn mogelijk nuttig zijn ook voor het toepassen van een beleid voor controle van SQL Server op zonder nodig voor de configuratie van een stap in elke database (tijdelijke risicobeperking).

## <a id="subheading-2"></a>IP-eindpunt wijzigingen bij het inschakelen van controle
Houd er rekening mee dat wanneer u de Tabelcontrole inschakelt, het IP-eindpunt van uw database worden gewijzigd. Als u strikte firewall-instellingen hebt, werkt u de firewall-instellingen dienovereenkomstig.

De nieuwe database IP-eindpunt is afhankelijk van de databaseregio:

| Databaseregio | Mogelijke IP-eindpunten |
| --- | --- |
| Noord-China |139.217.29.176, 139.217.28.254 |
| China Oost |42.159.245.65, 42.159.246.245 |
| Australië - oost |104.210.91.32, 40.126.244.159, 191.239.64.60, 40.126.255.94 |
| Australië - zuidoost |191.239.184.223, 40.127.85.81, 191.239.161.83, 40.127.81.130 |
| Brazilië - zuid |104.41.44.161, 104.41.62.230, 23.97.99.54, 104.41.59.191 |
| US - centraal |104.43.255.70, 40.83.14.7, 23.99.128.244, 40.83.15.176 |
| US - centraal EUAP |52.180.178.16, 52.180.176.190 |
| Azië - oost |23.99.125.133, 13.75.40.42, 23.97.71.138, 13.94.43.245 |
| US - oost 2 |104.209.141.31, 104.208.238.177, 191.237.131.51, 104.208.235.50 |
| US - oost |23.96.107.223, 104.41.150.122, 23.96.38.170, 104.41.146.44 |
| US - oost EUAP |52.225.190.86, 52.225.191.187 |
| India - centraal |104.211.98.219, 104.211.103.71 |
| India - zuid |104.211.227.102, 104.211.225.157 |
| India - west |104.211.161.152, 104.211.162.21 |
| Japan - oost |104.41.179.1, 40.115.253.81, 23.102.64.207, 40.115.250.196 |
| Japan - west |104.214.140.140, 104.214.146.31, 191.233.32.34, 104.214.146.198 |
| US - noord-centraal |191.236.155.178, 23.96.192.130, 23.96.177.169, 23.96.193.231 |
| Europa - noord |104.41.209.221, 40.85.139.245, 137.116.251.66, 40.85.142.176 |
| US - zuid-centraal |191.238.184.128, 40.84.190.84, 23.102.160.153, 40.84.186.66 |
| Azië - zuidoost |104.215.198.156, 13.76.252.200, 23.97.51.109, 13.76.252.113 |
| Europa - west |104.40.230.120, 13.80.23.64, 137.117.171.161, 13.80.8.37, 104.47.167.215, 40.118.56.193, 104.40.176.73, 40.118.56.20 |
| US - west |191.236.123.146, 138.91.163.240, 168.62.194.148, 23.99.6.91 |
| US - west 2 |13.66.224.156, 13.66.227.8 |
| US - west-centraal |52.161.29.186, 52.161.27.213 |
| Canada Centraal |13.88.248.106, 13.88.248.110 |
| Canada Oost |40.86.227.82, 40.86.225.194 |
| VK Noord |13.87.101.18, 13.87.100.232 |
| VK Zuid 2 |13.87.32.202, 13.87.32.226 |
