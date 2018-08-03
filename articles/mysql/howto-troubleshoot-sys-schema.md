---
title: Over het gebruik sys_schema voor het afstemmen van prestaties en onderhoud van de Database in Azure Database for MySQL
description: Dit artikel wordt beschreven hoe u met sys_schema prestatieproblemen vinden en onderhouden van de database in Azure Database voor MySQL.
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.topic: article
ms.date: 08/01/2018
ms.openlocfilehash: 1e10e3b1b5f4518732408f254eb5767acb8485c6
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39446904"
---
# <a name="how-to-use-sysschema-for-performance-tuning-and-database-maintenance-in-azure-database-for-mysql"></a>Over het gebruik van sys_schema voor prestaties afstemmen en het Databaseonderhoud in Azure Database for MySQL

De MySQL-performance_schema, de eerste beschikbaar in de MySQL-5.5 biedt instrumentatie voor veel belangrijke serverresources, zoals de toewijzing van geheugen, opgeslagen programma's, metagegevens vergrendelen, enzovoort. Echter, de performance_schema bevat meer dan 80 tabellen en samenvoegen van tabellen in de performance_schema, evenals de tabellen uit de information_schema ophalen van de benodigde gegevens vaak vereist. Voortbouwend op zowel performance_schema en information_schema, de sys_schema biedt een krachtige reeks [gebruiksvriendelijke weergaven](https://dev.mysql.com/doc/refman/5.7/en/sys-schema-views.html) in een database alleen-lezen en volledig is ingeschakeld in Azure Database voor MySQL versie 5.7.

![weergaven van sys_schema](./media/howto-troubleshoot-sys-schema/sys-schema-views.png)

Er zijn 52 weergaven in de sys_schema en elke weergave bevat een van de volgende voorvoegsels:

- Host_summary of i/o-: i/o-gerelateerde latenties.
- InnoDB: Status van de buffer InnoDB en wordt vergrendeld.
- Geheugen: Geheugengebruik door de host en de gebruikers.
- Schema: Schema-gerelateerde informatie, zoals Automatische toename, indexen, enzovoort.
- Overzicht: Informatie over SQL-instructies gebruikt; leidde tot volledige tabelcontrole uitgevoerd, of lang moment dat de query-instructie kan zijn.
- Gebruiker: Resources verbruikt en gegroepeerd per gebruiker. Voorbeelden zijn bestand i/o's, verbindingen en geheugen.
- Wacht: Wacht gebeurtenissen gegroepeerd op de host of de gebruiker.

Nu gaan we bekijken enkele algemene patronen in het gebruik van de sys_schema. Begint met, we de gebruikspatronen u groeperen in twee categorieën: **afstemmen van prestaties** en **onderhoud van de Database**.

## <a name="performance-tuning"></a>Prestaties afstemmen

### <a name="sysusersummarybyfileio"></a>*sys.user_summary_by_file_io*

I/o is de meest veeleisende bewerking in de database. Kan zien we de gemiddelde i/o-latentie door het opvragen van de *sys.user_summary_by_file_io* weergeven. Met de standaard die 125 GB van de ingerichte opslag, Mijn i/o-latentie ongeveer 15 seconden is.

![i/o-latentie: 125 GB](./media/howto-troubleshoot-sys-schema/io-latency-125GB.png)

Omdat Azure Database for MySQL i/o wordt met betrekking tot opslag, wordt geschaald nadat het verhogen van de ingerichte opslag tot 1 TB, vermindert de latentie voor i/o-571 MS.

![i/o-latentie: 1TB](./media/howto-troubleshoot-sys-schema/io-latency-1TB.png)

### <a name="sysschematableswithfulltablescans"></a>*sys.schema_tables_with_full_table_scans*

Ondanks de zorgvuldige planning kijken, kunnen nog steeds veel query's resulteren in volledige tabelscans. Voor meer informatie over de typen van indexen en hoe u ze optimaliseren, kunt u verwijzen naar dit artikel: [problemen oplossen met de prestaties van query's](./howto-troubleshoot-query-performance.md). Volledige tabelscans zijn resource-intensieve en verlagen van de databaseprestaties van uw. De snelste manier om te vinden van tabellen met volledige tabelcontrole uitgevoerd wordt op query de *sys.schema_tables_with_full_table_scans* weergeven.

![volledige tabelscans](./media/howto-troubleshoot-sys-schema/full-table-scans.png)

### <a name="sysusersummarybystatementtype"></a>*sys.user_summary_by_statement_type*

Voor het oplossen van prestatieproblemen met de database, het misschien wel handig om te identificeren van de gebeurtenissen die plaatsvinden binnen uw database, en met behulp van de *sys.user_summary_by_statement_type* weergave alleen de slag gaat.

![overzicht per instructie](./media/howto-troubleshoot-sys-schema/summary-by-statement.png)

Azure Database for MySQL besteed aan het 53 minuten leegmaken van het querylogboek slog 44579 tijden in dit voorbeeld. Dat is een lange periode en veel IOs. U kunt deze activiteit beperken door een van beide uitschakelen uw logboek voor langzame query's of verklein de frequentie van de aanmelding voor langzame query's Azure-portal.

## <a name="database-maintenance"></a>Databaseonderhoud

### <a name="sysinnodbbufferstatsbytable"></a>*sys.innodb_buffer_stats_by_table*

De buffergroep InnoDB bevindt zich in het geheugen en is de belangrijkste cache vormt tussen de DBMS-systemen en de opslag. De grootte van de buffergroep InnoDB is gekoppeld aan de prestatielaag en kan niet worden gewijzigd, tenzij een ander product SKU is gekozen. Net als bij geheugen in het besturingssysteem, worden oude pagina's uitgewisseld om ruimte voor recentere gegevens te maken. Als u wilt weten welke tabellen de meeste van de buffer InnoDB geheugen gebruiken, kunt u een query de *sys.innodb_buffer_stats_by_table* weergeven.

![Status van de buffer InnoDB](./media/howto-troubleshoot-sys-schema/innodb-buffer-status.png)

In de afbeelding hierboven is het duidelijk dat dan systeemtabellen en weergaven, elke tabel in de database mysqldatabase033, die als host fungeert voor een van mijn WordPress-sites, 16 KB of pagina 1 van de gegevens in het geheugen in beslag neemt.

### <a name="sysschemaunusedindexes--sysschemaredundantindexes"></a>*Sys.schema_unused_indexes* & *sys.schema_redundant_indexes*

Indexen zijn geweldige hulpprogramma's om meer prestaties te verbeteren, maar ze tot extra kosten voor ingevoegd en de opslag leiden. *Sys.schema_unused_indexes* en *sys.schema_redundant_indexes* inzicht geven in niet-gebruikte of gedupliceerde indexen.

![niet-gebruikte indexen](./media/howto-troubleshoot-sys-schema/unused-indexes.png)

![redundante indexen](./media/howto-troubleshoot-sys-schema/redundant-indexes.png)

## <a name="conclusion"></a>Conclusie

Kortom is de sys_schema een uitstekend hulpprogramma voor beide prestaties afstemmen en het Databaseonderhoud. Zorg ervoor dat u profiteren van deze functie in uw Azure Database voor MySQL. 

## <a name="next-steps"></a>Volgende stappen
- Peer-antwoorden op uw meest betrokken vragen of een nieuwe vraag/antwoord plaatsen, gaat u naar [MSDN-forum](https://social.msdn.microsoft.com/forums/security/en-US/home?forum=AzureDatabaseforMySQL) of [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-database-mysql).
