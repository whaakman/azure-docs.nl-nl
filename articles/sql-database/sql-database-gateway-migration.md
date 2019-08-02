---
title: Kennisgeving van Gateway migratie voor Azure SQL Database van Gen2 naar Gen3 | Microsoft Docs
description: Artikel geeft gebruikers informatie over de migratie van IP-adressen van Azure SQL Database gateways
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.topic: conceptual
author: rohitnayakmsft
ms.author: rohitna
ms.reviewer: vanto
ms.date: 07/01/2019
ms.openlocfilehash: 85691464684ff327c01a85bf357514f447564dd7
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68568120"
---
# <a name="azure-sql-database-traffic-migration-to-newer-gateways"></a>Azure SQL Database verkeer migratie naar nieuwere gateways

Naarmate de Azure-infra structuur wordt verbeterd, zal micro soft hardware periodiek vernieuwen om ervoor te zorgen dat we de best mogelijke klant ervaring bieden. In de komende maanden kunnen we gateways toevoegen die zijn gebouwd op nieuwere hardware en gateways uit bedrijf nemen die op oudere hardware in sommige regio's zijn gebouwd.  

Klanten worden op de hoogte gesteld via e-mail en in het Azure Portal goed van elke wijziging aan gateways die beschikbaar zijn in elke regio. De meest recente informatie wordt bewaard in de tabel met [IP-adressen van de Azure SQL database gateway](sql-database-connectivity-architecture.md#azure-sql-database-gateway-ip-addresses) .

## <a name="impact-of-this-change"></a>Impact van deze wijziging

De eerste ronde van gateway buiten gebruik stellen is gepland voor 1 september 2019 in de volgende regio's:

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

Het buiten gebruik gestelde IP-adres stopt met het accepteren van verkeer en eventuele nieuwe verbindings pogingen worden doorgestuurd naar een van de gateways in de regio.

De impact van deze wijziging wordt niet weer geven:

- Klanten die gebruikmaken van omleiding als verbindings beleid zien geen gevolgen.
- Verbindingen met SQL Database van binnen Azure en het gebruik van service tags worden niet beïnvloed.
- Verbindingen die zijn gemaakt met ondersteunde versies van het JDBC-stuur programma voor SQL Server, zien geen invloed. Zie [micro soft JDBC-stuur programma voor SQL Server downloaden](/sql/connect/jdbc/download-microsoft-jdbc-driver-for-sql-server)voor een ondersteunde versie van JDBC.

## <a name="what-to-do-you-do-if-youre-affected"></a>Wat u moet doen als dit van invloed is

We raden u aan om uitgaand verkeer naar IP-adressen toe te staan voor alle [IP-adressen van de Azure SQL database gateway](sql-database-connectivity-architecture.md#azure-sql-database-gateway-ip-addresses) in de regio op TCP-poort 1433 en poort bereik 11000-11999 op uw firewall-apparaat. Zie [verbindings beleid](sql-database-connectivity-architecture.md#connection-policy)voor meer informatie over poortbereiken.

Voor verbindingen die zijn gemaakt via toepassingen met het micro soft JDBC-stuur programma onder versie 4,0, kan het certificaat niet worden gevalideerd. Lagere versies van micro soft JDBC zijn afhankelijk van de algemene naam (CN) in het veld onderwerp van het certificaat. De beperking is om ervoor te zorgen dat de eigenschap hostNameInCertificate is ingesteld op *. database.windows.net. Zie [verbinding maken met SSL-versleuteling](/sql/connect/jdbc/connecting-with-ssl-encryption)voor meer informatie over het instellen van de eigenschap hostNameInCertificate.

Als de bovenstaande oplossing niet werkt, kunt u een ondersteunings aanvraag voor SQL Database indienen met behulp van de volgende URL: https://aka.ms/getazuresupport

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [Azure SQL-connectiviteits architectuur](sql-database-connectivity-architecture.md)