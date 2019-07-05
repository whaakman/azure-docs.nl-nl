---
title: Kennisgeving voor gateway-migratie voor Azure SQL Database van Gen2 om Gen3 | Microsoft Docs
description: Artikel vindt u ziet dat aan de gebruikers over de migratie van Azure SQL Database-Gateways IP-adressen
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.topic: conceptual
author: rohitnayakmsft
ms.author: rohitna
ms.reviewer: vanto
manager: craigg
ms.date: 07/01/2019
ms.openlocfilehash: 5894579c62c524394c7fea044b96885f7c8e8204
ms.sourcegitcommit: 084630bb22ae4cf037794923a1ef602d84831c57
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2019
ms.locfileid: "67538378"
---
# <a name="azure-sql-database-traffic-migration-to-newer-gateways"></a>Azure SQL Database-verkeer-migratie naar nieuwere Gateways

Azure-infrastructuur verbetert, wordt Microsoft hardware om ervoor te zorgen dat we de best mogelijke klantervaring bieden regelmatig vernieuwd. In de komende maanden wij van plan bent om toe te voegen Gateways die is gebouwd op nieuwere hardwaregeneraties en Gateways die is gebouwd op oudere hardware in sommige regio's buiten gebruik stellen.  

Klanten ontvangt een melding via e-mail en in de Azure-portal ook voorafgaand aan elke wijziging in Gateways die beschikbaar zijn in elke regio. De meest actuele informatie wordt onderhouden de [IP-adressen van Azure SQL Database-gateway](sql-database-connectivity-architecture.md#azure-sql-database-gateway-ip-addresses) tabel.

## <a name="impact-of-this-change"></a>Gevolgen van deze wijziging

De eerste ronde van de Gateway uit bedrijf nemen is gepland voor 1 September 2019 in de volgende regio's:

- US - west
- Europa -west
- East US
- US - centraal
- Azië - zuidoost
- US - zuid-centraal
- Europa - noord
- US - noord-centraal
- Japan - west
- Japan - oost
- US - oost 2
- Azië - oost

Het IP-adres van buiten gebruik gesteld stopt verkeer accepteren en een nieuwe verbindingspogingen worden doorgestuurd naar een van de Gateways in de regio.

U niet waar deze wijziging weergegeven:

- Klanten die gebruik van omleiding als hun verbindingsbeleid dit van invloed is niet weergegeven.
- Verbindingen met SQL Database uit in Azure en het gebruik van servicetags geen invloed op.
- Verbindingen gemaakt met behulp van ondersteunde versies van JDBC-stuurprogramma voor SQL Server ziet niet van invloed zijn. Zie voor ondersteunde versies van JDBC, [Microsoft JDBC-stuurprogramma downloaden voor SQL Server](/sql/connect/jdbc/download-microsoft-jdbc-driver-for-sql-server).

## <a name="what-to-do-you-do-if-youre-affected"></a>Wat wilt u doen als u bent beïnvloed

Het is raadzaam dat u uitgaand verkeer naar IP-adressen toestaan voor alle de [IP-adressen van Azure SQL Database-gateway](sql-database-connectivity-architecture.md#azure-sql-database-gateway-ip-addresses) in de regio op TCP-poort 1433 en poort bereik 11000-11999 in uw firewall-apparaat. Zie voor meer informatie over poortbereiken [verbindingsbeleid](sql-database-connectivity-architecture.md#connection-policy).

Verbindingen van toepassingen die gebruikmaken van Microsoft JDBC-stuurprogramma lagere versie dan versie 4.0 mislukken validatie van het servercertificaat. Lagere versies van Microsoft JDBC vertrouwen op algemene naam (CN) in het onderwerpveld van het certificaat. De oplossing is om ervoor te zorgen dat de eigenschap hostNameInCertificate is ingesteld op *. database.windows.net. Zie voor meer informatie over het instellen van de eigenschap hostNameInCertificate [verbinding te maken met SSL-versleuteling](/sql/connect/jdbc/connecting-with-ssl-encryption).

Als de bovenstaande oplossing niet werkt, het bestand een verzoek om ondersteuning voor SQL-Database met behulp van de volgende URL: https://aka.ms/getazuresupport

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [Azure SQL-Connectiviteitsarchitectuur](sql-database-connectivity-architecture.md)