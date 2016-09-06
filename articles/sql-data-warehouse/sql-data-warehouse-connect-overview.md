<properties
   pageTitle="Verbinding maken met Azure SQL Data Warehouse | Microsoft Azure"
   description="Overzicht van verbindingen voor het maken van verbinding met Azure SQL Data Warehouse"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/27/2016"
   ms.author="sonyama;barbkess"/>

# Verbinding maken met Azure SQL Data Warehouse

> [AZURE.SELECTOR]
- [Overzicht](sql-data-warehouse-connect-overview.md)
- [Authentication](sql-data-warehouse-authentication.md)
- [Stuurprogramma's](sql-data-warehouse-connection-strings.md)

Overzicht van verbinding maken met Azure SQL Data Warehouse. 

## Verbindingstekenreeks voor ontdekken in de portal

De SQL Data Warehouse wordt gekoppeld aan een Azure SQL Server. Als u verbinding wilt maken, hebt u de volledig gekwalificeerde naam van de server nodig.  Bijvoorbeeld: **mijnserver**.database.windows.net.

Ga als volgt te werk om de volledig gekwalificeerde servernaam te vinden:

1. Ga naar de [Azure Portal][].
2. Klik op **SQL-databases** en vervolgens op de database waarmee u verbinding wilt maken. In dit voorbeeld wordt de voorbeelddatabase AdventureWorksDW gebruikt.
3. Zoek de volledige servernaam.

    ![Volledige servernaam][1]

## Verbindingsinstellingen

SQL Data Warehouse standaardiseert enkele instellingen tijdens het maken van de verbinding en het maken van objecten. Deze instellingen kunnen niet worden genegeerd.

| Database-instelling       | Waarde                        |
| :--------------------- | :--------------------------- |
| [ANSI_NULLS][]         | AAN                           |
| [QUOTED_IDENTIFIERS][] | AAN                           |
| [DATEFORMAT][]         | mdy                          |
| [DATEFIRST][]          | 7                            |

## Verbindingen en query's bewaken

Als er verbinding is gemaakt en er een sessie tot stand is gebracht, bent u klaar om query’s op te stellen en deze te verzenden naar SQL Data Warehouse.  Zie [Monitor your workload using DMVs][] (Uw werkbelasting bewaken met behulp van DMV's) voor meer informatie over het bewaken van sessies en query's.

## Volgende stappen

Zie [Query’s uitvoeren bij Visual Studio][] als u wilt beginnen met het uitvoeren van query’s bij uw datawarehouse met Visual Studio en andere toepassingen. 

<!--Articles-->
[Query’s uitvoeren bij Visual Studio]: ./sql-data-warehouse-query-visual-studio.md
[Monitor your workload using DMVs]: ./sql-data-warehouse-manage-monitor.md

<!--MSDN references-->
[ANSI_NULLS]: https://msdn.microsoft.com/library/ms188048.aspx
[QUOTED_IDENTIFIERS]: https://msdn.microsoft.com/library/ms174393.aspx
[DATEFORMAT]: https://msdn.microsoft.com/library/ms189491.aspx
[DATEFIRST]: https://msdn.microsoft.com/library/ms181598.aspx

<!--Other-->
[Azure Portal]: https://portal.azure.com

<!--Image references-->
[1]: media/sql-data-warehouse-connect-overview/get-server-name.png





<!--HONumber=ago16_HO5-->


