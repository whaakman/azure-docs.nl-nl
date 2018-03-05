---
title: Hoe gebruik sys_schema voor het afstemmen van prestaties en onderhoud in Azure-Database voor de MySQL-Database
description: In dit artikel wordt beschreven hoe sys_schema gebruiken om te vinden van prestatieproblemen en -database in Azure-Database voor MySQL onderhouden.
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 2e5b6b859df06d686a97fc1b134da8d66df6783e
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/28/2018
---
# <a name="how-to-use-sysschema-for-performance-tuning-and-database-maintenance-in-azure-database-for-mysql"></a>Het gebruik van sys_schema voor prestaties afstemmen en het Databaseonderhoud in Azure-Database voor MySQL

MySQL-performance_schema, eerst beschikbaar is in MySQL 5.5, biedt instrumentation voor veel essentieel serverbronnen zoals geheugentoewijzing, opgeslagen programma's, metagegevens vergrendelen, enzovoort. Echter, de performance_schema bevat meer dan 80 tabellen en ophalen van de benodigde informatie vaak moet lid te worden van tabellen in de performance_schema, evenals de tabellen uit de information_schema. Voortbouwend op zowel performance_schema en information_schema, de sys_schema biedt een krachtige verzameling [gebruiksvriendelijke weergaven](https://dev.mysql.com/doc/refman/5.7/en/sys-schema-views.html) in een database alleen-lezen en volledig in Azure-Database is ingeschakeld voor MySQL versie 5.7.

![weergaven van sys_schema](./media/howto-troubleshoot-sys-schema/sys-schema-views.png)

Er zijn 52 weergaven in de sys_schema en elke weergave bevat een van de volgende voorvoegsels:

- Host_summary of i/o: i/o-gerelateerde latenties.
- InnoDB: Status van de buffer InnoDB en vergrendelingen.
- Geheugen: Geheugengebruik door de host en gebruikers.
- Schema: Schema-gerelateerde informatie, zoals Automatische toename, indexen, enzovoort.
- Overzicht: Informatie over SQL-instructies; instructie dat heeft geresulteerd in een van volledige tabelscan of lang Querytijd kan zijn.
- Gebruiker: Resources verbruikt en gegroepeerd per gebruiker. Voorbeelden zijn bestand i/o's, verbindingen en geheugen.
- Wacht: Wacht gebeurtenissen gegroepeerd op de host of de gebruiker.

Nu we een aantal algemene gebruikspatronen van de sys_schema bekijken. Allereerst we groepeert de gebruikspatronen in twee categorieën: **prestatieafstemming** en **Database onderhoud**.

## <a name="performance-tuning"></a>Prestaties afstemmen

### <a name="sysusersummarybyfileio"></a>*sys.user_summary_by_file_io*

I/o is de meest dure bewerking in de database. Er vindt hier de gemiddelde latentie voor i/o door het uitvoeren van query's de *sys.user_summary_by_file_io* weergeven. Met het standaard dat 125 GB ingerichte opslag, Mijn i/o-latentie ongeveer 15 seconden is.

![i/o-latentie: 125 GB](./media/howto-troubleshoot-sys-schema/io-latency-125GB.png)

Omdat Azure Database voor MySQL IO met betrekking tot opslag, schaalt na een stijging van mijn ingerichte opslag 1 TB, beperkt mijn i/o-latentie voor 571 ms, die een 26 verhoging van de prestaties X!

![i/o-latentie: 1TB](./media/howto-troubleshoot-sys-schema/io-latency-1TB.png)

### <a name="sysschematableswithfulltablescans"></a>*sys.schema_tables_with_full_table_scans*

Ondanks een zorgvuldige planning veel query's kunnen nog steeds leiden tot volledige tabelscans. Voor meer informatie over de typen indexen en ze te optimaliseren, u kunt verwijzen naar dit artikel: [queryprestaties oplossen](./howto-troubleshoot-query-performance.md). Volledige tabelscans resource-intensief en nemen de databaseprestaties van uw. Is de snelste manier om erachter tabellen met volledige tabelscan query de *sys.schema_tables_with_full_table_scans* weergeven.

![volledige tabelscans](./media/howto-troubleshoot-sys-schema/full-table-scans.png)

### <a name="sysusersummarybystatementtype"></a>*sys.user_summary_by_statement_type*

Voor het oplossen van prestatieproblemen met de database, kan het nuttig voor het identificeren van de gebeurtenissen plaatsvinden binnen uw database en het gebruik van zijn de *sys.user_summary_by_statement_type* weergave kan alleen de slag uitvoeren.

![Samenvatting van verklaring](./media/howto-troubleshoot-sys-schema/summary-by-statement.png)

In dit voorbeeld besteed aan het Azure-Database voor MySQL 53 minuten leegmaken van het querylogboek slog 44579 tijden. Dat is lang duren en veel IOs. U kunt deze activiteit verminderen door ofwel uit te schakelen uw trage querylogboek of verkleinen van de frequentie van de query wordt vertraagd aanmelding Azure-portal.

## <a name="database-maintenance"></a>Databaseonderhoud

### <a name="sysinnodbbufferstatsbytable"></a>*sys.innodb_buffer_stats_by_table*

De buffergroep InnoDB bevindt zich in het geheugen en is het mechanisme hoofdcache tussen de DBMS en de opslag. De grootte van de buffergroep InnoDB is gekoppeld aan de prestatielaag en kan niet worden gewijzigd, tenzij u een ander product SKU is gekozen. Net als bij geheugen in het besturingssysteem, worden oude pagina's uitgewisseld om ruimte voor fresher gegevens te maken. Als u wilt weten welke tabellen de meeste van de buffer InnoDB geheugen verbruiken, kunt u een query de *sys.innodb_buffer_stats_by_table* weergeven.

![Status van de buffer InnoDB](./media/howto-troubleshoot-sys-schema/innodb-buffer-status.png)

In de bovenstaande afbeelding is duidelijk dat dan systeemtabellen en weergaven, elke tabel in de database mysqldatabase033, die als host fungeert voor een van Mijn sites WordPress, 16 KB of 1 pagina van de gegevens in het geheugen in beslag neemt.

### <a name="sysschemaunusedindexes--sysschemaredundantindexes"></a>*Sys.schema_unused_indexes* & *sys.schema_redundant_indexes*

Indexen zijn geweldige tools lezen prestaties te verbeteren, maar ze extra kosten voor INSERT en opslag. *Sys.schema_unused_indexes* en *sys.schema_redundant_indexes* inzicht in niet-gebruikte of dubbele indexen.

![ongebruikte indexen](./media/howto-troubleshoot-sys-schema/unused-indexes.png)

![redundante indexen](./media/howto-troubleshoot-sys-schema/redundant-indexes.png)

## <a name="conclusion"></a>Conclusie

Kortom is de sys_schema een uitstekend hulpprogramma voor beide prestaties afstemmen en het Databaseonderhoud. Zorg ervoor dat om te profiteren van deze functie in uw Azure-Database voor MySQL. 

## <a name="next-steps"></a>Volgende stappen
- Als u wilt zoeken peer antwoorden op uw meest betrokken vragen of een nieuwe vraag/antwoord post, gaat u naar [MSDN-forum](https://social.msdn.microsoft.com/forums/security/en-US/home?forum=AzureDatabaseforMySQL) of [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-database-mysql).
