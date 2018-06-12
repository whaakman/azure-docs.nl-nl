---
title: SSL-verbindingen voor de Azure-Database voor MySQL
description: Informatie voor het configureren van Azure-Database voor MySQL en de bijbehorende toepassingen juist gebruik van SSL-verbindingen
services: mysql
author: JasonMAnderson
ms.author: janders
editor: jasonwhowell
manager: kfile
ms.service: mysql
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: ee7e0ec8524d66ee89cf7b2c4d44b70efa784f8f
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35265058"
---
# <a name="ssl-connectivity-in-azure-database-for-mysql"></a>SSL-verbindingen in Azure voor MySQL-Database
Azure MySQL-Database ondersteunt verbindingen van uw database-server met clienttoepassingen met Secure Sockets Layer (SSL). Het afdwingen van SSL-verbindingen tussen uw databaseserver en clienttoepassingen zorgt dat u bent beschermt tegen 'man in the middle'-aanvallen omdat de gegevensstroom tussen de server en uw toepassing wordt versleuteld.

## <a name="default-settings"></a>Standaardinstellingen
Standaard wordt de database-service zo dat SSL-verbindingen vereisen bij het verbinden met MySQL.  Het is raadzaam om te voorkomen dat de SSL-optie indien mogelijk uitschakelen. 

Bij het inrichten van een nieuwe Azure-Database voor de MySQL-server via de Azure portal en CLI kan afdwinging van SSL-verbindingen is standaard ingeschakeld. 

Tekenreeksen voor databaseverbindingen voor verschillende programmeertalen worden weergegeven in de Azure-portal. De verbindingsreeksen zijn de vereiste parameters SSL verbinding maken met uw database. Selecteer uw server in de Azure-portal. Onder de **instellingen** kop, selecteer de **verbindingsreeksen**. De parameter SSL varieert op basis van de connector, bijvoorbeeld ' ssl = true ' of ' sslmode = vereisen ' of ' sslmode = vereist ' en andere variaties.

Raadpleeg voor informatie over het in- of uitschakelen van SSL-verbinding bij het ontwikkelen van toepassing, [SSL configureren](howto-configure-ssl.md). 

## <a name="next-steps"></a>Volgende stappen
[Verbindingsbibliotheken voor Azure-Database voor MySQL](concepts-connection-libraries.md)
