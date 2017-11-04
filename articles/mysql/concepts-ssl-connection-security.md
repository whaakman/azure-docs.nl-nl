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
ms.date: 11/02/2017
ms.openlocfilehash: 37e2bbbe1ed4b6a9cca0e0b001e5e632b9b73be2
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/04/2017
---
# <a name="ssl-connectivity-in-azure-database-for-mysql"></a>SSL-verbindingen in Azure voor MySQL-Database
Azure MySQL-Database ondersteunt verbindingen van uw database-server met clienttoepassingen met Secure Sockets Layer (SSL). Afdwingen van SSL-verbindingen tussen uw database-server en client-toepassingen beschermt tegen 'man-in het midden'-aanvallen door het versleutelen van de gegevensstroom tussen de server en uw toepassing.

## <a name="default-settings"></a>Standaard-instellingen
Standaard wordt de database-service zo dat SSL-verbindingen vereisen bij het verbinden met MySQL.  Het is raadzaam om te voorkomen dat de SSL-optie indien mogelijk uitschakelen. 

Bij het inrichten van een nieuwe Azure-Database voor de MySQL-server via de Azure portal en CLI kan afdwinging van SSL-verbindingen is standaard ingeschakeld. 

Tekenreeksen voor databaseverbindingen voor verschillende programmeertalen worden weergegeven in de Azure-portal. De verbindingsreeksen zijn de vereiste parameters SSL verbinding maken met uw database. Selecteer uw server in de Azure-portal. Onder de **instellingen** kop, selecteer de **verbindingsreeksen**. De parameter SSL varieert op basis van de connector, bijvoorbeeld ' ssl = true ' of ' sslmode = vereisen ' of ' sslmode = vereist ' en andere variaties.

Raadpleeg voor informatie over het in- of uitschakelen van SSL-verbinding bij het ontwikkelen van toepassing, [SSL configureren](howto-configure-ssl.md). 

## <a name="next-steps"></a>Volgende stappen
[Verbindingsbibliotheken voor Azure-Database voor MySQL](concepts-connection-libraries.md)
