---
title: Tabel controle, TDS-omleiding en IP-eindpunten voor Azure SQL Database | Microsoft Docs
description: Meer informatie over controle, TDS-omleiding en IP-eindpunt wijzigingen bij het implementeren van de tabel in Azure SQL Database auditing.
services: sql-database
documentationcenter: 
author: giladm
manager: jhubbard
editor: 
ms.assetid: 4ef19ed1-e798-43a2-ad99-0e563f93ab53
ms.service: sql-database
ms.custom: security
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/31/2017
ms.author: giladm
ms.openlocfilehash: d4a7e6658ec65a70bd7e07859e2a69acee58b7b5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="sql-database----downlevel-clients-support-and-ip-endpoint-changes-for-table-auditing"></a>SQL Database - ondersteuning voor Downlevel-clients en IP-eindpunt voor de controle van de tabel wordt gewijzigd

> [!IMPORTANT]
> Dit document geldt alleen voor de tabel controle, namelijk **gedeprecieerd**.<br>
> Gebruik de nieuwe [Auditingfunctie voor blobs](sql-database-auditing.md) methode die **heeft geen** downlevel-client verbinding tekenreeks wijzigingen zijn vereist. Aanvullende informatie over de Blob-controle kunt u vinden in [aan de slag met SQL database auditing](sql-database-auditing.md).

[Controle van de database](sql-database-auditing.md) werkt automatisch met SQL-clients die ondersteuning bieden voor omleiden van TDS. Houd er rekening mee dat omleiding is niet van toepassing wanneer u de methode Auditingfunctie voor blobs.

## <a id="subheading-1"></a>Ondersteuning voor downlevel-clients
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

## <a id="subheading-2"></a>IP-eindpunt wordt gewijzigd bij het inschakelen van controle
Houd er rekening mee dat wanneer u tabel controleren activeert, het IP-eindpunt van de database worden gewijzigd. Als er strenge firewall-instellingen, werk deze firewall-instellingen dienovereenkomstig.

Het nieuwe IP-eindpunt voor database zijn afhankelijk van de regio:

| Databaseregio | Mogelijke IP-eindpunten |
| --- | --- |
| China - noord |139.217.29.176, 139.217.28.254 |
| China - oost |42.159.245.65, 42.159.246.245 |
| Australië - oost |104.210.91.32, 40.126.244.159, 191.239.64.60, 40.126.255.94 |
| Australië - zuidoost |191.239.184.223, 40.127.85.81, 191.239.161.83, 40.127.81.130 |
| Brazilië - zuid |104.41.44.161, 104.41.62.230, 23.97.99.54, 104.41.59.191 |
| VS - midden |104.43.255.70, 40.83.14.7, 23.99.128.244, 40.83.15.176 |
| VS-midden EUAP |52.180.178.16, 52.180.176.190 |
| Oost-Azië |23.99.125.133, 13.75.40.42, 23.97.71.138, 13.94.43.245 |
| VS - oost 2 |104.209.141.31, 104.208.238.177, 191.237.131.51, 104.208.235.50 |
| VS - oost |23.96.107.223, 104.41.150.122, 23.96.38.170, 104.41.146.44 |
| VS-Oost EUAP |52.225.190.86, 52.225.191.187 |
| Centraal-India |104.211.98.219, 104.211.103.71 |
| Zuid-India |104.211.227.102, 104.211.225.157 |
| West-India |104.211.161.152, 104.211.162.21 |
| Japan - oost |104.41.179.1, 40.115.253.81, 23.102.64.207, 40.115.250.196 |
| Japan - west |104.214.140.140, 104.214.146.31, 191.233.32.34, 104.214.146.198 |
| Noord-centraal VS |191.236.155.178, 23.96.192.130, 23.96.177.169, 23.96.193.231 |
| Noord-Europa |104.41.209.221, 40.85.139.245, 137.116.251.66, 40.85.142.176 |
| Zuid-centraal VS |191.238.184.128, 40.84.190.84, 23.102.160.153, 40.84.186.66 |
| Zuidoost-Azië |104.215.198.156, 13.76.252.200, 23.97.51.109, 13.76.252.113 |
| West-Europa |104.40.230.120, 13.80.23.64, 137.117.171.161, 13.80.8.37, 104.47.167.215, 40.118.56.193, 104.40.176.73, 40.118.56.20 |
| VS - west |191.236.123.146, 138.91.163.240, 168.62.194.148, 23.99.6.91 |
| VS - west 2 |13.66.224.156, 13.66.227.8 |
| West-centraal VS |52.161.29.186, 52.161.27.213 |
| Canada - midden |13.88.248.106, 13.88.248.110 |
| Canada - oost |40.86.227.82, 40.86.225.194 |
| VK, noord |13.87.101.18, 13.87.100.232 |
| VK, zuid 2 |13.87.32.202, 13.87.32.226 |
