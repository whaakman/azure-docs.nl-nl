---
title: SSL-verbindingen voor de Azure-Database voor MySQL | Microsoft Docs
description: Informatie voor het configureren van Azure-Database voor MySQL en de bijbehorende toepassingen juist gebruik van SSL-verbindingen
services: mysql
author: JasonMAnderson
ms.author: janders
editor: jasonwhowell
manager: jhubbard
ms.service: mysql-database
ms.topic: article
ms.date: 05/10/2017
ms.openlocfilehash: 4b03b3a2dbfad92cc0cfa84777b38ddff90452cf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="ssl-connectivity-in-azure-database-for-mysql"></a>SSL-verbindingen in Azure voor MySQL-Database
Azure MySQL-Database ondersteunt verbindingen van uw database-server met clienttoepassingen met Secure Sockets Layer (SSL). Afdwingen van SSL-verbindingen tussen uw database-server en client-toepassingen beschermt tegen 'man-in het midden'-aanvallen door het versleutelen van de gegevensstroom tussen de server en uw toepassing.

## <a name="default-settings"></a>Standaard-instellingen
Standaard wordt de database-service zo dat SSL-verbindingen vereisen bij het verbinden met MySQL.  Het verdient aanbeveling te voorkomen dat de SSL-optie indien mogelijk uitschakelen. 

Bij het inrichten van een nieuwe Azure-Database voor de MySQL-server via de Azure portal en CLI kan afdwinging van SSL-verbindingen is standaard ingeschakeld. 

Verbindingsreeksen die vooraf zijn gedefinieerd in de instellingen 'Verbindingsreeksen' onder uw server in de Azure portal omvatten ook de vereiste parameters voor algemene talen verbinding maken met uw database-server met behulp van SSL. De parameter SSL varieert op basis van de connector, bijvoorbeeld ' ssl = true ' of ' sslmode = vereisen ' of ' sslmode = vereist ' en andere variaties.

Voor informatie over het in- of uitschakelen van SSL-verbinding bij het ontwikkelen van toepassing, raadpleegt u [SSL configureren](howto-configure-ssl.md).

## <a name="next-steps"></a>Volgende stappen
[Verbindingsbibliotheken voor Azure-Database voor MySQL](concepts-connection-libraries.md)
