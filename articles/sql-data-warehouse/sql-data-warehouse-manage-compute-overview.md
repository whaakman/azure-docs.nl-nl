---
title: Beheren van de rekencapaciteit in Azure SQL Data Warehouse (overzicht) | Microsoft Docs
description: Prestaties scale-out mogelijkheden in Azure SQL Data Warehouse. Uitbreiden door dwu's aan te passen of onderbreken en hervatten van rekenresources kosten besparen.
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: johnmac
editor: 
ms.assetid: e13a82b0-abfe-429f-ac3c-f2b6789a70c6
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 3/23/2017
ms.author: elbutter
ms.openlocfilehash: 0d0d3b94fb50155ce0579d32e8ff78a47b9e3589
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="manage-compute-power-in-azure-sql-data-warehouse-overview"></a>De rekencapaciteit in Azure SQL Data Warehouse (overzicht) beheren
> [!div class="op_single_selector"]
> * [Overzicht](sql-data-warehouse-manage-compute-overview.md)
> * [Portal](sql-data-warehouse-manage-compute-portal.md)
> * [PowerShell](sql-data-warehouse-manage-compute-powershell.md)
> * [REST](sql-data-warehouse-manage-compute-rest-api.md)
> * [TSQL](sql-data-warehouse-manage-compute-tsql.md)
>
>

De architectuur van SQL Data Warehouse scheidt opslag en berekeningen, zodat deze afzonderlijk kunnen worden geschaald. Als gevolg hiervan kan rekencapaciteit worden geschaald om te voldoen aan de prestatie-eisen onafhankelijk van de hoeveelheid gegevens. Een natuurlijke gevolg van deze architectuur is dat [facturering] [ billed] voor berekeningen en opslag is gescheiden. 

Dit overzicht beschrijft uitbreiden hoe werkt met SQL Data Warehouse en hoe u kunt gebruikmaken van de onderbreken, hervatten en schalen van de mogelijkheden van SQL Data Warehouse. 

## <a name="how-compute-management-operations-work-in-sql-data-warehouse"></a>Hoe compute beheerbewerkingen werken in SQL Data Warehouse
De architectuur voor SQL Data Warehouse bestaat uit een beheerknooppunt, rekenknooppunten en de opslaglaag verspreid over 60 distributies. 

Tijdens een normale actieve sessie in SQL Data Warehouse het hoofdknooppunt van het systeem beheert de metagegevens en de gedistribueerde queryoptimalisatie bevat. Onder deze node head zijn uw rekenknooppunten en uw storage-laag. Voor een 400 DWU heeft uw systeem één hoofdknooppunt, vier rekenknooppunten en de storage-laag, die bestaan uit 60 distributies. 

Wanneer u een schaal ondergaan of onderbreken van de bewerking, wordt het systeem eerst is funest alle binnenkomende query's en vervolgens wordt teruggezet transacties om te controleren of een consistente status. Voor scale-bewerkingen vindt schalen alleen plaats wanneer deze transactionele terugdraaien is voltooid. Het systeem voorziet de extra gewenst aantal rekenknooppunten en begint vervolgens met de koppeling van de rekenknooppunten aan de storage-laag voor het uitvoeren van een scale-up. De overbodige knooppunten worden vrijgegeven voor een bewerking omlaag schalen en de resterende compute nodes koppelt zelf opnieuw aan het juiste aantal distributies. Voor een bewerking onderbreken compute alle knooppunten worden vrijgegeven en uw systeem ietwat tal van bewerkingen voor metagegevens uw laatste systeem in een stabiele status laten staan.

| DWU  | \#van rekenknooppunten | \#van distributies per knooppunt |
| ---- | ------------------ | ---------------------------- |
| 100  | 1                  | 60                           |
| 200  | 2                  | 30                           |
| 300  | 3                  | 20                           |
| 400  | 4                  | 15                           |
| 500  | 5                  | 12                           |
| 600  | 6                  | 10                           |
| 1000 | 10                 | 6                            |
| 1200 | 12                 | 5                            |
| 1500 | 15                 | 4                            |
| 2000 | 20                 | 3                            |
| 3000 | 30                 | 2                            |
| 6000 | 60                 | 1                            |

De drie primaire functies voor het beheren van compute zijn:

1. Onderbreken
2. Hervatten
3. Schalen

Elk van deze bewerkingen kan enkele minuten duren. Als u schalen/onderbreken/hervatten automatisch, u mogelijk wilt implementeren logica om ervoor te zorgen dat bepaalde bewerkingen zijn voltooid voordat u doorgaat met een andere actie. 

Controleren of de status van de database via verschillende eindpunten kunt u voor het implementeren van correct automatisering van dergelijke bewerkingen. De portal biedt de huidige status melding na voltooiing van een bewerking en de databases, maar is niet toegestaan voor de programmatische controle van status. 

>  [!NOTE]
>
>  COMPUTE beheerfunctionaliteit bestaat niet in alle eindpunten.
>
>  

|              | Onderbreken/hervatten | Schalen | Controleer de databasestatus van de |
| ------------ | ------------ | ----- | -------------------- |
| Azure Portal | Ja          | Ja   | **Nee**               |
| PowerShell   | Ja          | Ja   | Ja                  |
| REST-API     | Ja          | Ja   | Ja                  |
| T-SQL        | **Nee**       | Ja   | Ja                  |



<a name="scale-compute-bk"></a>

## <a name="scale-compute"></a>Scale compute

Prestaties in SQL Data Warehouse wordt gemeten in [datawarehouse units (dwu's)] [-datawarehouse units (dwu's)] Dit is een abstracte meting van de compute-bronnen zoals CPU, geheugen en i/o-bandbreedte. Een gebruiker die u wenst te schalen van de prestaties van hun systeem kunt doen met behulp van verschillende middelen, zoals via de portal, T-SQL en REST-API's. 

### <a name="how-do-i-scale-compute"></a>Hoe ik compute schalen?
COMPUTE power voor u SQL Data Warehouse wordt beheerd door de DWU-instelling te wijzigen. Prestaties neemt lineair toe naarmate u meer DWU voor bepaalde bewerkingen toevoegen.  We bieden DWU-aanbiedingen die ervoor zorgen dat de prestaties van uw merkbaar wordt veranderen wanneer u uw systeem omhoog of omlaag schalen. 

U kunt een van deze afzonderlijke methoden gebruiken om aan te passen dwu's.

* [Schaal rekenkracht met Azure portal][Scale compute power with Azure portal]
* [Schaal rekenkracht met PowerShell][Scale compute power with PowerShell]
* [De rekencapaciteit schaal met REST-API 's][Scale compute power with REST APIs]
* [Schaal rekenkracht met TSQL][Scale compute power with TSQL]

### <a name="how-many-dwus-should-i-use"></a>Het aantal dwu's moet ik gebruiken?

Om te bepalen wat voor u de ideale DWU-waarde is, kunt u omhoog en omlaag schalen en na het laden van uw gegevens enkele query's uitvoeren. Omdat schalen snel is, kunt u verschillende prestatieniveaus proberen in een uur of minder. 

> [!Note] 
> SQL Data Warehouse is ontworpen voor grote hoeveelheden gegevens verwerken. Als u wilt zien van de werkelijke capaciteit voor het schalen, met name bij een groter aantal dwu's, die u wilt gebruiken een grote gegevensset die nadert of hoger is dan 1 TB.

Aanbevelingen voor het vinden van de beste DWU voor uw workload:

1. Beginnen met het selecteren van een kleinere DWU-prestatieniveau voor een datawarehouse in ontwikkeling.  Een goed uitgangspunt is DW400 of DW200.
2. Controleren van de toepassingsprestaties van uw, het aantal dwu's geselecteerd observeren vergeleken met de prestaties die u zien.
3. Bepalen hoeveel prestatievermogen sneller of langzamer moet u het niveau van de optimale prestaties voor uw vereisten bereiken door ervan uitgaande dat lineaire schaal.
4. Vergroten of verkleinen het aantal dwu's in verhouding tot hoe veel sneller of trager u wilt dat uw werkbelasting uit te voeren. 
5. Blijven correcties totdat u een optimaal prestatieniveau voor uw zakelijke vereisten.

> [!NOTE]
>
> Prestaties van query's worden alleen met meer garandeert verhoogd als het werk kan worden gesplitst tussen rekenknooppunten. Als u merkt dat schalen niet van de prestaties van uw wijzigen is, check onze artikelen om te controleren of uw gegevens ongelijkmatig verdeeld is of als u een grote hoeveelheid gegevensverplaatsing introduceren afstemmen van de prestaties. 

### <a name="when-should-i-scale-dwus"></a>Wanneer moet ik dwu's schalen?
Dwu's schalen, wijzigt de volgende belangrijke scenario's:

1. Prestaties van het systeem voor scans, aggregaties en CTAS instructies wijzigen lineair
2. Het aantal en schrijfprogramma verhogen bij het laden met PolyBase
3. Maximum aantal gelijktijdige query's en gelijktijdigheid sleuven

Aanbevelingen voor het schalen van dwu's:

1. Voordat u een zware laad- of transformatiebewerking bewerking uitvoert, opschalen dwu's zodat uw gegevens sneller beschikbaar is.
2. Schalen tijdens kantooruren piek, zodat een groot aantal gelijktijdige query's. 

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a>De rekencapaciteit onderbreken
[!INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

Onderbreken van een database, een van deze afzonderlijke methoden te gebruiken.

* [Compute onderbreken met de Azure-portal][Pause compute with Azure portal]
* [De rekencapaciteit onderbreken met PowerShell][Pause compute with PowerShell]
* [De rekencapaciteit onderbreken met REST-API 's][Pause compute with REST APIs]

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a>Compute hervatten
[!INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]

Voor het hervatten van een database, een van deze afzonderlijke methoden te gebruiken.

* [De berekeningen hervatten met Azure portal][Resume compute with Azure portal]
* [De berekeningen hervatten met PowerShell][Resume compute with PowerShell]
* [De berekeningen hervatten met REST-API 's][Resume compute with REST APIs]

<a name="check-compute-bk"></a>

## <a name="check-database-state"></a>Controleer de databasestatus van de 

Voor het hervatten van een database, een van deze afzonderlijke methoden te gebruiken.

- [Controleer de databasestatus van de met T-SQL][Check database state with T-SQL]
- [Controleer de databasestatus van de met PowerShell][Check database state with PowerShell]
- [Controleer de databasestatus van de met de REST-API 's][Check database state with REST APIs]

## <a name="permissions"></a>Machtigingen

De machtigingen beschreven in de database schalen vereist [ALTER DATABASE][ALTER DATABASE].  Onderbreken en hervatten, vereisen de [SQL DB Contributor] [ SQL DB Contributor] machtiging specifiek Microsoft.Sql/servers/databases/action.

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Volgende stappen
Raadpleeg de volgende artikelen om te begrijpen enkele concepten die aanvullende prestatie:

* [Werkbelasting en gelijktijdigheid management][Workload and concurrency management]
* [Overzicht van de tabel-ontwerp][Table design overview]
* [Tabeldistributie][Table distribution]
* [Tabel indexeren][Table indexing]
* [Partitioneren van tabel][Table partitioning]
* [Tabelstatistieken][Table statistics]
* [Aanbevolen procedures][Best practices]

<!--Image reference-->

<!--Article references-->
[billed]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[Scale compute power with Azure portal]: ./sql-data-warehouse-manage-compute-portal.md#scale-compute-power
[Scale compute power with PowerShell]: ./sql-data-warehouse-manage-compute-powershell.md#scale-compute-bk
[Scale compute power with REST APIs]: ./sql-data-warehouse-manage-compute-rest-api.md#scale-compute-bk
[Scale compute power with TSQL]: ./sql-data-warehouse-manage-compute-tsql.md#scale-compute-bk

[capacity limits]: ./sql-data-warehouse-service-capacity-limits.md

[Pause compute with Azure portal]:  ./sql-data-warehouse-manage-compute-portal.md#pause-compute-bk
[Pause compute with PowerShell]: ./sql-data-warehouse-manage-compute-powershell.md#pause-compute-bk
[Pause compute with REST APIs]: ./sql-data-warehouse-manage-compute-rest-api.md#pause-compute-bk

[Resume compute with Azure portal]:  ./sql-data-warehouse-manage-compute-portal.md#resume-compute-bk
[Resume compute with PowerShell]: ./sql-data-warehouse-manage-compute-powershell.md#resume-compute-bk
[Resume compute with REST APIs]: ./sql-data-warehouse-manage-compute-rest-api.md#resume-compute-bk

[Check database state with T-SQL]: ./sql-data-warehouse-manage-compute-tsql.md#check-database-state-and-operation-progress
[Check database state with PowerShell]: ./sql-data-warehouse-manage-compute-powershell.md#check-database-state
[Check database state with REST APIs]: ./sql-data-warehouse-manage-compute-rest-api.md#check-database-state

[Workload and concurrency management]: ./sql-data-warehouse-develop-concurrency.md
[Table design overview]: ./sql-data-warehouse-tables-overview.md
[Table distribution]: ./sql-data-warehouse-tables-distribute.md
[Table indexing]: ./sql-data-warehouse-tables-index.md
[Table partitioning]: ./sql-data-warehouse-tables-partition.md
[Table statistics]: ./sql-data-warehouse-tables-statistics.md
[Best practices]: ./sql-data-warehouse-best-practices.md
[development overview]: ./sql-data-warehouse-overview-develop.md

[SQL DB Contributor]: ../active-directory/role-based-access-built-in-roles.md#sql-db-contributor

<!--MSDN references-->
[ALTER DATABASE]: https://msdn.microsoft.com/library/mt204042.aspx

<!--Other Web references-->
[Azure portal]: http://portal.azure.com/
