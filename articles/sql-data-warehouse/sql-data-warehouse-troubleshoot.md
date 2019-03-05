---
title: Het oplossen van Azure SQL datawarehouse | Microsoft Docs
description: Problemen oplossen met Azure SQL datawarehouse.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 12/04/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: 5c9d8330e857a8f76e143631f17e29ba44244a88
ms.sourcegitcommit: 3f4ffc7477cff56a078c9640043836768f212a06
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/04/2019
ms.locfileid: "57307918"
---
# <a name="troubleshooting-azure-sql-data-warehouse"></a>Oplossen van problemen met Azure SQL datawarehouse
In dit artikel geeft een lijst met veelgestelde vragen voor het oplossen van problemen.

## <a name="connecting"></a>Verbinding maken
| Probleem                                                        | Oplossing                                                   |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| Aanmelden is mislukt voor gebruiker 'NT AUTHORITY\ANONIEME LOGON'. (Microsoft SQL Server, Error: 18456) | Deze fout treedt op wanneer een AAD-gebruiker probeert verbinding te maken met de hoofddatabase, maar heeft geen een gebruiker in de master.  Dit probleem oplossen door op de SQL Data Warehouse die u wilt verbinding maken met tijdens de verbinding of de gebruiker toevoegen aan de database master opgeven.  Zie [beveiligingsoverzicht] [ Security overview] artikel voor meer informatie. |
| De server principal 'MyUserName' is niet toegang hebben tot de database 'master' in de huidige beveiligingscontext. Kan de standaarddatabase van de gebruiker niet openen. Aanmelding mislukt. Aanmelden is mislukt voor gebruiker 'MyUserName'. (Microsoft SQL Server, Error: 916) | Deze fout treedt op wanneer een AAD-gebruiker probeert verbinding te maken met de hoofddatabase, maar heeft geen een gebruiker in de master.  Dit probleem oplossen door op de SQL Data Warehouse die u wilt verbinding maken met tijdens de verbinding of de gebruiker toevoegen aan de database master opgeven.  Zie [beveiligingsoverzicht] [ Security overview] artikel voor meer informatie. |
| CTAIP fout                                                  | Deze fout kan optreden wanneer een aanmelding is gemaakt op de hoofddatabase van SQL server, maar niet in de SQL Data Warehouse-database.  Als u deze fout optreedt, kijk dan eens de [beveiligingsoverzicht] [ Security overview] artikel.  In dit artikel wordt uitgelegd hoe u een aanmelding en gebruiker maken op de master en over het maken van een gebruiker in de SQL Data Warehouse-database. |
| Geblokkeerd door Firewall                                          | Azure SQL-databases worden beveiligd door de server en database niveau firewalls om te controleren of alleen bekende IP-adressen die toegang hebben tot een database. De firewalls zijn beveiligd door standaard, wat betekent dat u expliciet moet inschakelen en IP-adres of adresbereik voordat u verbinding kunt maken.  U configureert de firewall om toegang te krijgen, volg de stappen in [server firewall-toegang configureren voor uw client-IP-] [ Configure server firewall access for your client IP] in de [inrichting instructies] [Provisioning instructions]. |
| Kan geen verbinding maken met het hulpprogramma of het stuurprogramma                           | SQL Data Warehouse adviseert [SSMS][SSMS], [SSDT voor Visual Studio][SSDT for Visual Studio], of [sqlcmd] [ sqlcmd] aan uw gegevens op te vragen. Zie voor meer informatie over stuurprogramma's en verbinding maken met SQL Data Warehouse [stuurprogramma's voor Azure SQL Data Warehouse] [ Drivers for Azure SQL Data Warehouse] en [verbinding maken met Azure SQL Data Warehouse] [ Connect to Azure SQL Data Warehouse] artikelen. |

## <a name="tools"></a>Hulpprogramma's
| Probleem                                                        | Oplossing                                                   |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| Visual Studio-Objectverkenner ontbreekt AAD-gebruikers           | Dit is een bekend probleem.  Als tijdelijke oplossing, bekijk de gebruikers in [sys.database_principals][sys.database_principals].  Zie [verificatie met Azure SQL Data Warehouse] [ Authentication to Azure SQL Data Warehouse] voor meer informatie over het gebruik van Azure Active Directory met SQL Data Warehouse. |
| Handmatig uitvoeren van scripts, met behulp van de wizard uitvoeren van scripts of via SSMS verbinding maken met is traag, vastgelopen of producent-fouten | Zorg ervoor dat gebruikers zijn gemaakt in de database master. In scriptopties, ook voor zorgen dat de engine-editie is ingesteld als 'Microsoft Azure SQL Data Warehouse Edition' en engine-type is 'Microsoft Azure SQL Database'. |
| Genereren van scripts mislukt in SSMS                             | Genereren van een script voor SQL datawarehouse mislukt als de optie '-script genereren voor afhankelijke objecten'-optie is ingesteld op 'True'. Als tijdelijke oplossing, handmatig gebruikers moeten gaan naar Extra -> Options -> SQL Server Object Explorer script genereren voor afhankelijke opties -> en ingesteld op false |

## <a name="performance"></a>Prestaties
| Probleem                                                        | Oplossing                                                   |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| Voor het oplossen van een query-prestaties                            | Als u probeert om op te lossen van een bepaalde query, begint u met [leren hoe u kunt voor het bewaken van uw query's][Learning how to monitor your queries]. |
| Slechte queryprestaties en -abonnementen is vaak een resultaat van de ontbrekende statistieken | De meest voorkomende oorzaak van slechte prestaties is gebrek aan statistieken voor uw tabellen.  Zie [tabelstatistieken beheren] [ Statistics] voor meer informatie over het maken van statistieken en waarom ze essentieel zijn voor de prestaties van uw. |
| Onvoldoende gelijktijdigheid / query's in de wachtrij geplaatst                             | Informatie over [beheer van de werkbelasting] [ Workload management] is het belangrijk om te begrijpen hoe u taakverdeling tussen de toewijzing van geheugen met gelijktijdigheid. |
| Over het implementeren van best practices                              | De beste plaats om te starten voor meer informatie over manieren om queryprestaties te verbeteren is [best practices voor SQL Data Warehouse] [ SQL Data Warehouse best practices] artikel. |
| Over het verbeteren van de prestaties met schalen                      | Soms de oplossing voor het verbeteren van de prestaties is het simpelweg toevoegen van meer rekenkracht voor uw query's door [schalen van uw SQL Data Warehouse][Scaling your SQL Data Warehouse]. |
| Slechte queryprestaties als gevolg van de index van slechte kwaliteit     | Enkele voorbeelden van momenten query's kunnen worden vertraagd vanwege [slechte columnstore-index kwaliteit][Poor columnstore index quality].  Raadpleeg dit artikel voor meer informatie en hoe u [opnieuw opbouwen van indexen voor het verbeteren van segmentkwaliteit][Rebuild indexes to improve segment quality]. |

## <a name="system-management"></a>Systeembeheer
| Probleem                                                        | Oplossing                                                   |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| Msg 40847: Kan de bewerking niet uitvoeren omdat de server hiermee het toegestane quotum van de Database Transaction Units van 45000 overschrijdt. | Verlaag de [DWU] [ DWU] van de database die u probeert te maken of [een verhoging aanvragen][request a quota increase]. |
| Ruimteverbruik onderzoeken                              | Zie [tabel grootten] [ Table sizes] om te begrijpen van het ruimtegebruik van uw systeem. |
| Het beheer van tabellen                                    | Zie de [tabeloverzicht] [ Overview] artikel voor meer informatie over het beheren van uw tabellen.  In dit artikel bevat ook koppelingen naar meer gedetailleerde onderwerpen zoals [gegevenstypes tabel][Data types], [distribueren van een tabel][Distribute], [Indexeren van een tabel][Index], [partitioneren van een tabel][Partition], [tabelstatistieken beheren] [ Statistics] en [tijdelijke tabellen][Temporary]. |
| Transparent data encryption (TDE) voortgangsbalk wordt niet bijgewerkt in de Azure Portal | U kunt de status van TDE via weergeven [powershell](/powershell/module/az.sql/get-azsqldatabasetransparentdataencryption). |

## <a name="polybase"></a>PolyBase
| Probleem                                           | Oplossing                                                   |
| :---------------------------------------------- | :----------------------------------------------------------- |
| Laden is mislukt vanwege de grote rijen                | Ondersteuning voor grote rij is momenteel niet beschikbaar voor Polybase.  Dit betekent dat als uw tabel VARCHAR(MAX), NVARCHAR(MAX) of VARBINARY(MAX) bevat, externe tabellen kunnen niet worden gebruikt om uw gegevens te laden.  Het laden van grote rijen is momenteel alleen ondersteund via Azure Data Factory (met BCP), Azure Stream Analytics, SSIS, BCP of de klasse .NET SqlBulkCopy uitvoert. PolyBase-ondersteuning voor grote rijen wordt toegevoegd in een toekomstige release. |
| BCP belasting van de tabel met maximale-gegevenstype is mislukt | Er is een bekend probleem waarvoor is vereist dat VARCHAR(MAX), NVARCHAR(MAX) of VARBINARY(MAX) aan het einde van de tabel in sommige scenario's worden geplaatst.  Verplaats uw maximum aantal kolommen aan het einde van de tabel. |

## <a name="differences-from-sql-database"></a>Verschillen met SQL-Database
| Probleem                                 | Oplossing                                                   |
| :------------------------------------ | :----------------------------------------------------------- |
| Niet-ondersteunde SQL-Database-functies     | Zie [niet-ondersteunde tabelfuncties][Unsupported table features]. |
| Niet-ondersteunde SQL-Database-gegevenstypen   | Zie [niet-ondersteunde gegevenstypen][Unsupported data types].        |
| VERWIJDEREN en UPDATE-beperkingen         | Zie [UPDATE oplossingen][UPDATE workarounds], [verwijderen oplossingen] [ DELETE workarounds] en [met behulp van CTAS omzeilen niet-ondersteunde UPDATE en DELETE syntaxis][Using CTAS to work around unsupported UPDATE and DELETE syntax]. |
| MERGE-instructie wordt niet ondersteund      | Zie [samenvoegen oplossingen][MERGE workarounds].                  |
| Beperkingen van de opgeslagen procedure          | Zie [opgeslagen procedure beperkingen] [ Stored procedure limitations] om enkele van de beperkingen van opgeslagen procedures te begrijpen. |
| UDF's bieden geen ondersteuning voor SELECT-instructies | Dit is een beperking van onze UDF's.  Zie [CREATE FUNCTION] [ CREATE FUNCTION] voor de syntaxis wordt ondersteund. |

## <a name="next-steps"></a>Volgende stappen
Voor meer hulp bij het vinden van oplossing voor uw probleem volgen hier enkele andere bronnen die u kunt proberen.

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
[Security overview]: sql-data-warehouse-overview-manage-security.md
[SSMS]: /sql/ssms/download-sql-server-management-studio-ssms
[SSDT for Visual Studio]: sql-data-warehouse-install-visual-studio.md
[Drivers for Azure SQL Data Warehouse]: sql-data-warehouse-connection-strings.md
[Connect to Azure SQL Data Warehouse]: sql-data-warehouse-connect-overview.md
[Ondersteuningsticket maken]: sql-data-warehouse-get-started-create-support-ticket.md
[Scaling your SQL Data Warehouse]: sql-data-warehouse-manage-compute-overview.md
[DWU]: sql-data-warehouse-overview-what-is.md
[request a quota increase]: sql-data-warehouse-get-started-create-support-ticket.md
[Learning how to monitor your queries]: sql-data-warehouse-manage-monitor.md
[Provisioning instructions]: sql-data-warehouse-get-started-provision.md
[Configure server firewall access for your client IP]: sql-data-warehouse-get-started-provision.md
[SQL Data Warehouse best practices]: sql-data-warehouse-best-practices.md
[Table sizes]: sql-data-warehouse-tables-overview.md#table-size-queries
[Unsupported table features]: sql-data-warehouse-tables-overview.md#unsupported-table-features
[Unsupported data types]: sql-data-warehouse-tables-data-types.md#unsupported-data-types
[Overview]: sql-data-warehouse-tables-overview.md
[Data types]: sql-data-warehouse-tables-data-types.md
[Distribute]: sql-data-warehouse-tables-distribute.md
[Index]: sql-data-warehouse-tables-index.md
[Partition]: sql-data-warehouse-tables-partition.md
[Statistics]: sql-data-warehouse-tables-statistics.md
[Temporary]: sql-data-warehouse-tables-temporary.md
[Poor columnstore index quality]: sql-data-warehouse-tables-index.md#causes-of-poor-columnstore-index-quality
[Rebuild indexes to improve segment quality]: sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality
[Workload management]: resource-classes-for-workload-management.md
[Using CTAS to work around unsupported UPDATE and DELETE syntax]: sql-data-warehouse-develop-ctas.md#using-ctas-to-work-around-unsupported-features
[UPDATE workarounds]: sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-update-statements
[DELETE workarounds]: sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-delete-statements
[MERGE workarounds]: sql-data-warehouse-develop-ctas.md#replace-merge-statements
[Stored procedure limitations]: sql-data-warehouse-develop-stored-procedures.md#limitations
[Authentication to Azure SQL Data Warehouse]: sql-data-warehouse-authentication.md


<!--MSDN references-->
[sys.database_principals]: /sql/relational-databases/system-catalog-views/sys-database-principals-transact-sql
[CREATE FUNCTION]: /sql/t-sql/statements/create-function-sql-data-warehouse
[sqlcmd]: sql-data-warehouse-get-started-connect-sqlcmd.md

<!--Other Web references-->
[Blogs]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[CAT-teamblogs]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Functieverzoeken]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[MSDN-forum]: https://social.msdn.microsoft.com/Forums/home?forum=AzureSQLDataWarehouse
[Stack Overflow-forum]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Video's]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse
