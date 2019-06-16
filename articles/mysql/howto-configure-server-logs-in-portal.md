---
title: Configureren en toegang tot van Logboeken voor langzame query's voor Azure Database voor MySQL in Azure portal
description: Dit artikel wordt beschreven hoe u configureert en toegang krijgen tot de trage Logboeken in Azure Database voor MySQL in Azure portal.
author: rachel-msft
ms.author: raagyema
ms.service: mysql
ms.topic: conceptual
ms.date: 05/29/2019
ms.openlocfilehash: b16ac525d41eb2423828a647fdb75fd3f4a80a31
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67052714"
---
# <a name="configure-and-access-slow-query-logs-in-the-azure-portal"></a>Configureren en toegang langzame query's beschikbaar zijn in Azure portal

U kunt configureren, weergeven en downloaden de [Azure Database for MySQL langzame-querylogboeken](concepts-server-logs.md) vanuit Azure portal.

## <a name="prerequisites"></a>Vereisten
Als u wilt in deze gebruiksaanwijzing kunt doorlopen, hebt u het volgende nodig:
- [Azure Database voor MySQL-server](quickstart-create-mysql-server-database-using-azure-portal.md)

## <a name="configure-logging"></a>Logboekregistratie configureren
Configureer de toegang tot het MySQL-logboek voor langzame query's. 

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).

2. Selecteer uw Azure Database voor MySQL-server.

3. Onder de **bewaking** sectie in de zijbalk, selecteer **serverlogboeken**. 
   ![Selecteer serverlogboeken, klik om te configureren](./media/howto-configure-server-logs-in-portal/1-select-server-logs-configure.png)

4. Selecteer de kop **Klik hier om Logboeken inschakelen en configureren van logboekparameters** om te zien van de parameters van de server.

5. Hiermee wijzigt u de parameters die u nodig hebt om aan te passen. Alle wijzigingen die u in deze sessie aanbrengt worden in paars gemarkeerd. 

   Nadat u de parameters hebt gewijzigd, kunt u klikken op **opslaan**. U kunt ook **negeren** uw wijzigingen.

   ![Klik op opslaan of negeren](./media/howto-configure-server-logs-in-portal/3-save-discard.png)

6. Ga terug naar de lijst met Logboeken door te klikken op de **knop Sluiten** (het pictogram X) op de **serverparameters** pagina.

## <a name="view-list-and-download-logs"></a>Weergeven en logboeken downloaden
Zodra logboekregistratie wordt gestart, kunt u een lijst met beschikbare langzame-querylogboeken weergeven en downloaden van afzonderlijke logboekbestanden op de Server Logs-deelvenster.

1. Open Azure Portal.

2. Selecteer uw Azure Database voor MySQL-server.

3. Onder de **bewaking** sectie in de zijbalk, selecteer **serverlogboeken**. De pagina bevat een overzicht van uw logboekbestanden, zoals wordt weergegeven:

   ![Lijst van Logboeken](./media/howto-configure-server-logs-in-portal/4-server-logs-list.png)

   > [!TIP]
   > De naamgevingsconventie van het logboek is **mysql - traag - < naam van uw server >-yyyymmddhh.log**. De datum en tijd die wordt gebruikt in de bestandsnaam is het tijd is wanneer het logboek is uitgegeven. Logboekbestanden worden gedraaid elke 24 uur of 7,5 GB, afhankelijk van wat eerst komt.

4. Als het nodig is, gebruikt u de **zoekvak** snel verfijnen naar een bepaald op basis van datum/tijd. De zoekopdracht is op de naam van het logboek.

5. Downloaden van afzonderlijke logboekbestanden met behulp van de **downloaden** knop (omlaag pijlpictogram) naast elke log-bestand in een rij in de tabel, zoals wordt weergegeven:

   ![Klik op het downloadpictogram](./media/howto-configure-server-logs-in-portal/5-download.png)

## <a name="next-steps"></a>Volgende stappen
- Zie [langzame query's voor toegang tot logboeken in CLI](howto-configure-server-logs-in-cli.md) voor meer informatie over het downloaden van Logboeken voor langzame query's via een programma.
- Meer informatie over [langzame-querylogboeken](concepts-server-logs.md) in Azure Database for MySQL.
- Zie voor meer informatie over de parameterdefinities en MySQL-logboekregistratie, de MySQL-documentatie op [logboeken](https://dev.mysql.com/doc/refman/5.7/en/slow-query-log.html).