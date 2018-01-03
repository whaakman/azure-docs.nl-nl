---
title: Het oplossen van Azure SQL datawarehouse | Microsoft Docs
description: Het oplossen van Azure SQL datawarehouse.
services: sql-data-warehouse
documentationcenter: NA
author: kevinvngo
manager: jhubbard
editor: 
ms.assetid: 51f1e444-9ef7-4e30-9a88-598946c45196
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 03/30/2017
ms.author: kevin;barbkess
ms.openlocfilehash: 48318397f9c5e463c82320ad9d7c23a1a62af77e
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
# <a name="troubleshooting-azure-sql-data-warehouse"></a>Het oplossen van Azure SQL datawarehouse
In dit onderwerp vindt u enkele van de meer veelvoorkomende vragen die we graag van onze klanten.

## <a name="connecting"></a>Verbinding maken
| Probleem | Oplossing |
|:--- |:--- |
| Aanmelden is mislukt voor gebruiker 'NT AUTHORITY\ANONYMOUS aanmelden'. (Microsoft SQL Server, fout: 18456) |Deze fout treedt op wanneer een AAD-gebruiker probeert verbinding maken met de database master, maar geen een gebruiker in master heeft.  Om dit probleem te verhelpen ofwel opgeven van de SQL Data Warehouse die u wilt verbinding maken met tijdens de verbinding of de gebruiker toevoegen aan de database master.  Zie [beveiligingsoverzicht] [ Security overview] artikel voor meer informatie. |
| De server principal 'gebruik met Windows' kan geen toegang tot de database 'master' in de huidige beveiligingscontext. Kan de standaarddatabase van de gebruiker niet openen. Aanmelding mislukt. Aanmelden is mislukt voor gebruiker 'gebruik met Windows'. (Microsoft SQL Server, fout: 916) |Deze fout treedt op wanneer een AAD-gebruiker probeert verbinding maken met de database master, maar geen een gebruiker in master heeft.  Om dit probleem te verhelpen ofwel opgeven van de SQL Data Warehouse die u wilt verbinding maken met tijdens de verbinding of de gebruiker toevoegen aan de database master.  Zie [beveiligingsoverzicht] [ Security overview] artikel voor meer informatie. |
| CTAIP fout |Deze fout kan optreden wanneer een aanmelding is gemaakt op de hoofddatabase van SQL server, maar niet in de SQL Data Warehouse-database.  Als u deze fout tegenkomt, bekijk de [beveiligingsoverzicht] [ Security overview] artikel.  In dit artikel wordt beschreven hoe u een gebruikersnaam en een gebruiker op de master en hoe u een gebruiker maken in de SQL Data Warehouse-database maken. |
| Geblokkeerd door een Firewall |Azure SQL-databases worden beveiligd door de server en database niveau firewalls om te controleren of alleen bekende IP-adressen die toegang hebben tot een database. De firewalls zijn beveiligd door standaard, wat betekent dat moet u expliciet inschakelen en IP-adres of een bereik aan adressen dat voordat u verbinding kunt maken.  Volg de stappen in voor het configureren van uw firewall om toegang te krijgen, [servertoegang firewall configureren voor uw client-IP-] [ Configure server firewall access for your client IP] in de [inrichting instructies] [Provisioning instructions]. |
| Kan geen verbinding maken met het hulpprogramma of het stuurprogramma |SQL Data Warehouse raadt het gebruik van [SSMS][SSMS], [SSDT voor Visual Studio][SSDT for Visual Studio], of [sqlcmd] [ sqlcmd] query uitvoeren op uw gegevens. Zie voor meer informatie over stuurprogramma's en verbinding maken met SQL Data Warehouse [stuurprogramma's voor Azure SQL Data Warehouse] [ Drivers for Azure SQL Data Warehouse] en [verbinding maken met Azure SQL Data Warehouse] [ Connect to Azure SQL Data Warehouse] artikelen. |

## <a name="tools"></a>Hulpprogramma's
| Probleem | Oplossing |
|:--- |:--- |
| Visual Studio-Objectverkenner ontbreekt AAD-gebruikers |Dit is een bekend probleem.  Als tijdelijke oplossing de gebruikers in bekijken [sys.database_principals][sys.database_principals].  Zie [verificatie met Azure SQL Data Warehouse] [ Authentication to Azure SQL Data Warehouse] voor meer informatie over het gebruik van Azure Active Directory met SQL Data Warehouse. |
|Handmatig uitvoeren van scripts, met behulp van de wizard uitvoeren van scripts of verbinding maakt via SSMS is traag, vastgelopen of produceren fouten| Zorg dat gebruikers zijn gemaakt in de database master. Bij scriptopties, ook voor zorgen dat de engine-editie is ingesteld als 'Microsoft Azure SQL Data Warehouse Edition' en engine-type is 'Microsoft Azure SQL Database'.|

## <a name="performance"></a>Prestaties
| Probleem | Oplossing |
|:--- |:--- |
| Query-prestaties probleemoplossing |Als u probeert een bepaalde query oplossen, beginnen met een [leren van het bewaken van uw query's][Learning how to monitor your queries]. |
| Slechte queryprestaties en planningen is vaak een resultaat van ontbrekende statistieken |De meest voorkomende oorzaak van slechte prestaties is een gebrek aan statistics voor tabellen.  Zie [tabelstatistieken onderhouden] [ Statistics] voor meer informatie over het maken van statistieken en waarom ze zijn essentieel voor de prestaties van uw. |
| Lage gelijktijdigheid / query's in de wachtrij |Informatie over [werkbelasting management] [ Workload management] is het belangrijk om te begrijpen hoe geheugentoewijzing met gelijktijdigheid van taken te verdelen. |
| Het implementeren van aanbevolen procedures |De beste plaats om te starten voor meer informatie over het verbeteren van de prestaties van query's is [aanbevolen procedures voor SQL Data Warehouse] [ SQL Data Warehouse best practices] artikel. |
| Hoe verbeteren de prestaties met schalen |Soms de oplossing voor het verbeteren van de prestaties is toe te voegen meer rekenkracht van uw query's door [schalen van uw SQL Data Warehouse][Scaling your SQL Data Warehouse]. |
| Slechte queryprestaties als gevolg van slechte index kwaliteit |Enkele voorbeelden van momenten query's kunnen vertraging vanwege [slechte columnstore-index kwaliteit][Poor columnstore index quality].  Raadpleeg dit artikel voor meer informatie en hoe [samenstellen segment kwaliteitsverbetering][Rebuild indexes to improve segment quality]. |

## <a name="system-management"></a>Systeembeheer
| Probleem | Oplossing |
|:--- |:--- |
| Bericht 40847: Kan de bewerking niet uitvoeren omdat de server hiermee het toegestane quotum van de Database Transaction Unit van 45000 overschrijdt. |Verlaag de [DWU] [ DWU] van de database die u probeert te maken of [aanvragen van een verhoging van het quotum][request a quota increase]. |
| Ruimtegebruik onderzoeken |Zie [tabel grootten] [ Table sizes] om te begrijpen van het gebruik van de ruimte van uw systeem. |
| Het beheer van tabellen |Zie de [tabel overzicht] [ Overview] artikel voor meer informatie over het beheren van uw tabellen.  In dit artikel bevat ook koppelingen naar meer gedetailleerde onderwerpen zoals [tabel gegevenstypen][Data types], [distribueren van een tabel][Distribute], [Indexeren van een tabel][Index], [partitioneren van een tabel][Partition], [tabelstatistieken onderhouden] [ Statistics] en [tijdelijke tabellen][Temporary]. |
|Transparent data encryption (TDE) voortgangsbalk wordt niet bijgewerkt in de Azure Portal|U kunt de status van TDE via weergeven [powershell](/powershell/module/azurerm.sql/get-azurermsqldatabasetransparentdataencryption).|

## <a name="polybase"></a>PolyBase
| Probleem | Oplossing |
|:--- |:--- |
| Laden is mislukt vanwege de grote rijen |Ondersteuning voor grote rij is momenteel niet beschikbaar voor Polybase.  Dit betekent dat als de tabel VARCHAR(MAX), NVARCHAR(MAX) of VARBINARY(MAX) bevat, externe tabellen kunnen niet worden gebruikt om uw gegevens te laden.  Belasting voor grote rijen is momenteel alleen ondersteund door Azure Data Factory (met BCP), Azure Stream Analytics, SSIS, BCP of de SQLBulkCopy .NET-klasse. PolyBase-ondersteuning voor grote rijen wordt in een toekomstige release toegevoegd. |
| BCP belasting van de tabel met maximale-gegevenstype is mislukt |Er is een bekend probleem dat is vereist dat VARCHAR(MAX), NVARCHAR(MAX) of VARBINARY(MAX) aan het einde van de tabel in bepaalde scenario's worden geplaatst.  Verplaats het maximum aantal kolommen aan het einde van de tabel. |

## <a name="differences-from-sql-database"></a>Verschillen met SQL-Database
| Probleem | Oplossing |
|:--- |:--- |
| Niet-ondersteunde functies van de SQL-Database |Zie [niet-ondersteunde tabelfuncties][Unsupported table features]. |
| Niet-ondersteunde gegevenstypen van de SQL-Database |Zie [niet-ondersteunde gegevenstypen][Unsupported data types]. |
| VERWIJDEREN en beperkingen van de UPDATE |Zie [UPDATE tijdelijke oplossingen][UPDATE workarounds], [verwijderen tijdelijke oplossingen] [ DELETE workarounds] en [CTAS omzeilen met behulp van niet-ondersteunde UPDATE en DELETE syntaxis][Using CTAS to work around unsupported UPDATE and DELETE syntax]. |
| MERGE-instructie wordt niet ondersteund. |Zie [samenvoegen tijdelijke oplossingen][MERGE workarounds]. |
| Beperkingen van de opgeslagen procedure |Zie [opgeslagen procedure beperkingen] [ Stored procedure limitations] enkele van de beperkingen van opgeslagen procedures te begrijpen. |
| UDF's bieden geen ondersteuning voor SELECT-instructies |Dit is een beperking van onze UDF's.  Zie [CREATE FUNCTION] [ CREATE FUNCTION] voor de syntaxis die wordt ondersteund. |

## <a name="next-steps"></a>Volgende stappen
Als u zijn kan niet zoeken naar een oplossing voor het bovenstaande probleem, Hier volgen enkele andere resources die u kunt proberen.

* [Blogs]
* [Functieverzoeken]
* [Video's]
* [CAT-teamblogs]
* [Ondersteuningsticket maken]
* [MSDN-forum]
* [Stack Overflow-forum]
* [Twitter]

<!--Image references-->

<!--Article references-->
[Security overview]: ./sql-data-warehouse-overview-manage-security.md
[SSMS]: https://msdn.microsoft.com/library/mt238290.aspx
[SSDT for Visual Studio]: ./sql-data-warehouse-install-visual-studio.md
[Drivers for Azure SQL Data Warehouse]: ./sql-data-warehouse-connection-strings.md
[Connect to Azure SQL Data Warehouse]: ./sql-data-warehouse-connect-overview.md
[Ondersteuningsticket maken]: ./sql-data-warehouse-get-started-create-support-ticket.md
[Scaling your SQL Data Warehouse]: ./sql-data-warehouse-manage-compute-overview.md
[DWU]: ./sql-data-warehouse-overview-what-is.md
[request a quota increase]: ./sql-data-warehouse-get-started-create-support-ticket.md
[Learning how to monitor your queries]: ./sql-data-warehouse-manage-monitor.md
[Provisioning instructions]: ./sql-data-warehouse-get-started-provision.md
[Configure server firewall access for your client IP]: ./sql-data-warehouse-get-started-provision.md
[SQL Data Warehouse best practices]: ./sql-data-warehouse-best-practices.md
[Table sizes]: ./sql-data-warehouse-tables-overview.md#table-size-queries
[Unsupported table features]: ./sql-data-warehouse-tables-overview.md#unsupported-table-features
[Unsupported data types]: ./sql-data-warehouse-tables-data-types.md#unsupported-data-types
[Overview]: ./sql-data-warehouse-tables-overview.md
[Data types]: ./sql-data-warehouse-tables-data-types.md
[Distribute]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[Temporary]: ./sql-data-warehouse-tables-temporary.md
[Poor columnstore index quality]: ./sql-data-warehouse-tables-index.md#causes-of-poor-columnstore-index-quality
[Rebuild indexes to improve segment quality]: ./sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality
[Workload management]: ./sql-data-warehouse-develop-concurrency.md
[Using CTAS to work around unsupported UPDATE and DELETE syntax]: ./sql-data-warehouse-develop-ctas.md#using-ctas-to-work-around-unsupported-features
[UPDATE workarounds]: ./sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-update-statements
[DELETE workarounds]: ./sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-delete-statements
[MERGE workarounds]: ./sql-data-warehouse-develop-ctas.md#replace-merge-statements
[Stored procedure limitations]: ./sql-data-warehouse-develop-stored-procedures.md#limitations
[Authentication to Azure SQL Data Warehouse]: ./sql-data-warehouse-authentication.md
[Working around the PolyBase UTF-8 requirement]: ./sql-data-warehouse-load-polybase-guide.md#working-around-the-polybase-utf-8-requirement

<!--MSDN references-->
[sys.database_principals]: https://msdn.microsoft.com/library/ms187328.aspx
[CREATE FUNCTION]: https://msdn.microsoft.com/library/mt203952.aspx
[sqlcmd]: https://azure.microsoft.com/en-us/documentation/articles/sql-data-warehouse-get-started-connect-sqlcmd/

<!--Other Web references-->
[Blogs]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[CAT-teamblogs]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Functieverzoeken]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[MSDN-forum]: https://social.msdn.microsoft.com/Forums/home?forum=AzureSQLDataWarehouse
[Stack Overflow-forum]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Video's]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse
