---
title: Firewallregels op serverniveau
description: Firewallregels op serverniveau
keywords: SQL-verbinding, verbindingsreeks
services: sql-database
author: dalechen
manager: craigg
ms.service: sql-database
ms.custom: develop apps
ms.topic: include
ms.date: 07/13/2018
ms.author: ninarn
ms.openlocfilehash: 8d0f9899dbb7599340b8d15ca010a0157011fb9e
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/23/2019
ms.locfileid: "66164270"
---
1. Meld u aan bij [Azure Portal](https://portal.azure.com/).

2. Selecteer in de lijst aan de linkerkant, **alle services**.

3. Schuif omlaag en selecteer **SQL-servers**.

    ![Uw Azure SQL Database-server niet vinden in de portal][b21-FindServerInPortal]
5. Begin met het typen van de naam van uw server in het filtertekstvak. De rij wordt weergegeven.

6. Selecteer de rij voor uw server. Een blade voor uw server wordt weergegeven.

7. Selecteer op de serverblade **instellingen**.

8. Selecteer **Firewall**.

    ![Selecteer instellingen > Firewall][b31-SettingsFirewallNavig]
9. Selecteer **-Client toevoegen IP**. Typ een naam voor de nieuwe regel in het eerste tekstvak in.

10. Typ in de hoge en lage IP-adreswaarden voor het bereik dat u wilt inschakelen.

    * Kan het zijn handig om te hebben van het end lage waarde met **.0** en de hoge waarde eindigen met **.255**.

11. Selecteer **Opslaan**.

<!-- Image references. -->

[b21-FindServerInPortal]: ./media/sql-database-include-ip-address-22-v12portal/firewall-ip-b21-v12portal-findsvr.png

[b31-SettingsFirewallNavig]: ./media/sql-database-include-ip-address-22-v12portal/firewall-ip-b31-v12portal-settingsfirewall.png

[b41-AddRange]: ./media/sql-database-include-ip-address-22-v12portal/firewall-ip-b41-v12portal-addrange.png



<!--
These includes/ files are a sequenced set, but you can pick and choose:

includes/sql-database-include-ip-address-22-v12portal.md
? includes/sql-database-include-ip-address-*.md
-->
