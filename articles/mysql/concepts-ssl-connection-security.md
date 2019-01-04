---
title: SSL-connectiviteit voor Azure Database for MySQL
description: Informatie voor het configureren van Azure Database for MySQL en gekoppelde toepassingen gebruiken SSL-verbindingen
author: JasonMAnderson
ms.author: janders
ms.service: mysql
ms.topic: conceptual
ms.date: 02/28/2018
ms.openlocfilehash: 129f90d495627edb25dfafdeb1b274aa2c4c71cb
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/17/2018
ms.locfileid: "53539826"
---
# <a name="ssl-connectivity-in-azure-database-for-mysql"></a>SSL-verbindingen in Azure Database for MySQL
Azure Database for MySQL ondersteunt verbindingen van uw database-server voor clienttoepassingen met Secure Sockets Layer (SSL). Het afdwingen van SSL-verbindingen tussen uw databaseserver en clienttoepassingen zorgt dat u bent beschermt tegen 'man in the middle'-aanvallen omdat de gegevensstroom tussen de server en uw toepassing wordt versleuteld.

## <a name="default-settings"></a>Standaardinstellingen
Standaard moet de database-service worden geconfigureerd voor het SSL-verbindingen vereisen bij het verbinden met MySQL.  We raden aan om te voorkomen dat de SSL-optie zoveel mogelijk uit te schakelen. 

Bij het inrichten van een nieuwe Azure-Database voor MySQL-server via de Azure-portal en CLI kan afdwingen van SSL-verbindingen is standaard ingeschakeld. 

Verbindingsreeksen voor verschillende programmeertalen worden weergegeven in de Azure-portal. Deze tekenreeksen voor databaseverbindingen omvatten de vereiste parameters SSL verbinding maakt met uw database. Selecteer uw server in de Azure-portal. Onder de **instellingen** kop, selecteer de **verbindingsreeksen**. De SSL-parameter is afhankelijk van de connector, bijvoorbeeld ' ssl = true "of" sslmode = vereisen "of" sslmode = vereist ' en andere verschillen.

Raadpleeg voor informatie over het in- of uitschakelen van SSL-verbinding bij het ontwikkelen van toepassingen, [over het configureren van SSL](howto-configure-ssl.md). 

## <a name="next-steps"></a>Volgende stappen
[Verbindingsbibliotheken voor Azure Database for MySQL](concepts-connection-libraries.md)
