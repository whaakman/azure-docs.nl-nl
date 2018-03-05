---
title: Configureren en toegang van server-logboeken voor Azure-Database voor MySQL in Azure Portal
description: Dit artikel wordt beschreven hoe u kunt configureren en toegang tot de server-Logboeken in Azure-Database voor MySQL in Azure Portal.
services: mysql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: b68d92688b18e69f74c0e0a9b2e572795a288579
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/28/2018
---
# <a name="configure-and-access-server-logs-in-the-azure-portal"></a>Configureren en de logboeken van de toegangsserver in de Azure portal

U kunt configureren, weergeven en downloaden de [Azure Database voor de serverlogboeken MySQL](concepts-server-logs.md) vanuit de Azure-portal.

## <a name="prerequisites"></a>Vereisten
Stap in deze handleiding instructies, wilt u het volgende nodig:
- [Azure-Database voor de MySQL-server](quickstart-create-mysql-server-database-using-azure-portal.md)

## <a name="configure-logging"></a>Logboekregistratie configureren
Toegang tot de trage querylogboek MySQL configureren. 

1. Meld u aan bij [Azure Portal](http://portal.azure.com/).

2. Selecteer uw Azure-Database voor de MySQL-server.

3. Onder de **bewaking** sectie in de zijbalk, selecteer **serverlogboeken**. 
   ![Selecteer serverlogboeken, klikt u op configureren](./media/howto-configure-server-logs-in-portal/1-select-server-logs-configure.png)

4. Selecteer de kop **Klik hier om Logboeken inschakelen en configureren van parameters voor het vastleggen** om te zien van de parameters van de server.

5. Wijzig de parameters die u nodig hebt om aan te passen. Alle wijzigingen die u in deze sessie aanbrengt worden in paars gemarkeerd. 

   Zodra u de parameters hebt gewijzigd, klikt u op **opslaan**. U kunt **negeren** uw wijzigingen.

   ![Klik op opslaan of negeren](./media/howto-configure-server-logs-in-portal/3-save-discard.png)

6. Terug naar de lijst met Logboeken door te klikken op de **knop Sluiten** (pictogram X) op de **serverparameters** pagina.

## <a name="view-list-and-download-logs"></a>Lijst weergeven en logboeken downloaden
Zodra logboekregistratie wordt gestart, kunt u een lijst met beschikbare logboeken weergeven en downloaden in het deelvenster logboeken van de Server een afzonderlijke logboekbestanden. 

1. Open Azure Portal.

2. Selecteer uw Azure-Database voor de MySQL-server.

3. Onder de **bewaking** sectie in de zijbalk, selecteer **serverlogboeken**. De pagina bevat een overzicht van uw logboekbestanden, zoals wordt weergegeven:

   ![Lijst met Logboeken](./media/howto-configure-server-logs-in-portal/4-server-logs-list.png)

   > [!TIP]
   > De naamgevingsconventie van het logboek is **mysql - traag - < naam van uw server >-yyyymmddhh.log**. De datum en tijd gebruikt in de bestandsnaam is de tijd wanneer het logboek is uitgegeven. Logboekbestanden worden gedraaid elke 24 uur of 7.5 GB, afhankelijk van wat het eerst wordt bereikt.

4. Indien nodig, gebruiken de **zoekvak** snel beperken aan een specifieke logboek op basis van datum/tijd. De zoekopdracht is op de naam van het logboek.

5. Downloaden van afzonderlijke logboekbestanden met behulp van de **downloaden** knop (omlaag pijlpictogram) naast elk logboekbestand in de tabelrij, zoals wordt weergegeven:

   ![Klik op het downloadpictogram](./media/howto-configure-server-logs-in-portal/5-download.png)


## <a name="next-steps"></a>Volgende stappen
- Zie [Access Server-logboeken in CLI](howto-configure-server-logs-in-cli.md) voor informatie over het om Logboeken te downloaden via een programma.
- Meer informatie over [serverlogboeken](concepts-server-logs.md) in Azure voor MySQL-Database. 
- Zie de MySQL-documentatie voor meer informatie over de parameterdefinities en MySQL logboekregistratie op [logboeken](https://dev.mysql.com/doc/refman/5.7/en/slow-query-log.html).

