---
title: Beheren van compute-resource in Azure SQL Data Warehouse | Microsoft Docs
description: Meer informatie over de prestaties van scale-out mogelijkheden in Azure SQL Data Warehouse. De schaal vergroten door aan te passen van dwu's en lagere kosten voor het datawarehouse onderbreekt.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 04/17/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: 53a801a367e6948c3070224b7ff36a013a1faab3
ms.sourcegitcommit: 1fb353cfca800e741678b200f23af6f31bd03e87
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/30/2018
ms.locfileid: "43300847"
---
# <a name="manage-compute-in-azure-sql-data-warehouse"></a>Compute in Azure SQL Data Warehouse beheren
Meer informatie over het beheren van rekenresources in Azure SQL Data Warehouse. Lagere kosten door het datawarehouse onderbreken of schalen van het datawarehouse om te voldoen aan de prestatievereisten. 

## <a name="what-is-compute-management"></a>Wat is de compute-beheer?
De architectuur van SQL Data Warehouse scheidt opslagruimte en rekenkracht, zodat deze onafhankelijk worden geschaald. Als gevolg hiervan kunt u schaal de rekenkracht om te voldoen aan de prestatievereisten die onafhankelijk van de opslag van gegevens. U kunt ook onderbreken en hervatten van de compute-resources. Een natuurlijke gevolg van deze architectuur is dat [facturering](https://azure.microsoft.com/pricing/details/sql-data-warehouse/) is voor rekenen en opslag gescheiden. Als u niet hoeft te gebruiken van uw datawarehouse even, kunt u de kosten voor rekenuren opslaan onderbreekt compute. 

## <a name="scaling-compute"></a>Compute schalen
U kunt omhoog of omlaag schalen back compute door aan te passen de [datawarehouse-eenheden](what-is-a-data-warehouse-unit-dwu-cdwu.md) instellen voor uw datawarehouse. Het laden en de queryprestaties kunt lineair verhogen als u meer datawarehouse-eenheden toevoegen. 

Zie voor stappen van de scale-out, de [Azure-portal](quickstart-scale-compute-portal.md), [PowerShell](quickstart-scale-compute-powershell.md), of [T-SQL](quickstart-scale-compute-tsql.md) snelstartgidsen. U kunt ook uitvoeren met scale-out-bewerkingen met een [REST-API](sql-data-warehouse-manage-compute-rest-api.md#scale-compute).

Als u een schaalbewerking, SQL Data Warehouse beëindigt eerst alle binnenkomende query's en worden vervolgens terug transacties om te controleren of een consistente status. Schalen treedt alleen op als het terugdraaien van de transactie voltooid is. Compute-knooppunten wordt toegevoegd en klikt u vervolgens de opslaglaag in de rekenlaag doelschijf voor een schaalbewerking het systeem losgekoppeld van de opslaglaag van de Compute-knooppunten. Elk datawarehouse wordt opgeslagen als 60 distributies die gelijkmatig worden gedistribueerd naar de rekenknooppunten. Meer computerknooppunten toe te voegen, voegt dat meer rekenkracht. Als het aantal rekenknooppunten toeneemt, wordt het aantal distributies per knooppunt afneemt, bieden meer rekenkracht voor uw query's. Evenzo, waardoor de datawarehouse-eenheden vermindert het aantal rekenknooppunten, waardoor de rekenresources voor query's.

De volgende tabel ziet u hoe het aantal distributies per wijzigingen voor Compute-knooppunt als de datawarehouse-eenheden wijzigen.  DWU6000 biedt 60 Compute-knooppunten en nog veel hogere queryprestaties dan DWU100 bereikt. 

| Datawarehouse-eenheden  | \# van Compute-knooppunten | \# van distributies per knooppunt |
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


## <a name="finding-the-right-size-of-data-warehouse-units"></a>Zoeken naar de juiste grootte van de datawarehouse-eenheden

Als u wilt zien van de prestatievoordelen van het uitschalen, met name voor grotere datawarehouse-eenheden, die u wilt gebruiken ten minste een gegevensset van 1 TB. Probeer omhoog en omlaag schalen om het aanbevolen aantal dwu's voor uw datawarehouse. Enkele query's uitvoeren met verschillende aantallen datawarehouse-eenheden na het laden van uw gegevens. Omdat schalen snel gaat, kunt u verschillende prestatieniveaus uitproberen in een uur of minder. 

Aanbevelingen voor het vinden van het aanbevolen aantal datawarehouse-eenheden:

- Voor een datawarehouse in de ontwikkeling, beginnen door het selecteren van een kleiner aantal dwu's.  Een goed uitgangspunt is DW400 of DW200.
- Bewaak de prestaties van uw toepassing, het aantal dwu's geselecteerd ten opzichte van de prestaties die u ziet dat uw probleem.
- Wordt ervan uitgegaan dat een lineaire schaal en bepalen hoeveel u wilt vergroten of verkleinen van de datawarehouse-eenheden. 
- Gaan correcties totdat u een optimaal prestatieniveau voor uw zakelijke vereisten.

## <a name="when-to-scale-out"></a>Wanneer u om uit te schalen
Uitschalen datawarehouse-eenheden heeft gevolgen voor deze aspecten van de prestaties:

- Lineair verbetert de prestaties van het systeem voor scans en aggregaties CTAS-instructies.
- Vergroot het aantal lezers en schrijvers om gegevens te laden.
- Maximum aantal gelijktijdige query's en gelijktijdigheidssleuven in beslag.

Aanbevelingen voor het uitschalen van gegevens datawarehouse-eenheden:

- Voordat u een zware laad- of transformatiebewerking bewerking uitvoert, de schaal vergroten tot de gegevens sneller beschikbaar te stellen.
- Tijdens piekuren voor bedrijven, de schaal vergroten om grotere aantallen gelijktijdige query's mogelijk te maken. 

## <a name="what-if-scaling-out-does-not-improve-performance"></a>Wat gebeurt er als uitschalen heeft niet de prestaties verbeteren?

Datawarehouse eenheden verhogen van de parallelle uitvoering toe te voegen. Als het werk wordt evenredig verdeeld tussen de rekenknooppunten, verbetert de extra parallelle uitvoering de prestaties van query's. Als de prestaties van uw is niet gewijzigd, uitschalen, zijn er enkele redenen waarom dit kan gebeuren. Uw gegevens mogelijk worden vervormd via de distributies of query's mogelijk worden introductie van een grote hoeveelheid verplaatsing van gegevens. Zie voor het onderzoeken van prestatieproblemen van query [prestatieproblemen oplossen](sql-data-warehouse-troubleshoot.md#performance). 

## <a name="pausing-and-resuming-compute"></a>Onderbreken en hervatten van rekenactiviteiten
Onderbreken compute zorgt ervoor dat de opslaglaag los te koppelen van de Compute-knooppunten. De Compute-resources worden vrijgegeven uit uw account. U betaalt geen voor compute terwijl de rekenprocessen worden onderbroken. Hervatten van rekenactiviteiten doelschijf opslag naar de rekenknooppunten en kosten in rekening gebracht voor rekenkracht hervat. Wanneer u een datawarehouse onderbreken:

* Reken-en geheugenresources keert terug naar de pool van beschikbare resources in het datacenter
* Datawarehouse-kosten per eenheid zijn nul voor de duur van het onderbreken.
* De opslag van gegevens wordt niet beïnvloed en uw gegevens intact blijft. 
* SQL Data Warehouse annuleert alle bewerkingen voor die wordt uitgevoerd of in de wachtrij.

Wanneer u een datawarehouse hervatten:

* SQL Data Warehouse verkrijgt reken-en geheugenresources voor uw datawarehouse-eenheden instellen.
* COMPUTE-kosten voor uw datawarehouse units hervatten.
* Uw gegevens beschikbaar.
* Nadat het datawarehouse online is, moet u opnieuw opstarten van uw werkbelasting query's.

Als u wilt dat altijd uw datawarehouse toegankelijk is, kunt u overwegen schalen omlaag naar de kleinste grootte, in plaats van onderbreken. 

Voor onderbreken en hervatten van de stappen, Zie de [Azure-portal](pause-and-resume-compute-portal.md), of [PowerShell](pause-and-resume-compute-powershell.md) snelstartgidsen. U kunt ook de [onderbreken REST-API](sql-data-warehouse-manage-compute-rest-api.md#pause-compute) of de [hervatten REST-API](sql-data-warehouse-manage-compute-rest-api.md#resume-compute).

## <a name="drain-transactions-before-pausing-or-scaling"></a>Transacties stoppen voor onderbreken of schalen
U wordt aangeraden zodat bestaande transacties worden voltooid voordat u een bewerking onderbreken of schalen initiëren.

Wanneer u uw SQL Data Warehouse onderbreekt of schaalt, worden uw query’s achter de schermen geannuleerd zodra u het onderbreek- of schaalverzoek start.  Een eenvoudige SELECT-query annuleren is een snelle bewerking en heeft zo goed als geen invloed op de duur van het onderbreken of schalen van uw instantie.  Maar transactiequery’s, die uw gegevens of de structuur van uw gegevens wijzigen, kunnen mogelijk niet snel worden stopgezet.  **Transactiequery’s moeten per definitie volledig worden voltooid of hun wijzigingen volledig terugdraaien.**  Het kan even lang of langer duren om het werk dat door een transactiequery is voltooid, terug te draaien, als het uitvoeren van de oorspronkelijke opdracht van de query.  Als u bijvoorbeeld een query annuleert voor het verwijderen van rijen die al een uur wordt uitgevoerd, kan het systeem er een uur over doen om de verwijderde rijen terug te plaatsen.  Als u onderbreken of schalen uitvoert terwijl er transacties bezig zijn, kan het schalen of onderbreken lang lijken te duren omdat het schalen of onderbreken moet wachten op het terugdraaien van de transacties voordat het kan worden voortgezet.

Zie ook [inzicht krijgen in transacties](sql-data-warehouse-develop-transactions.md), en [transacties optimaliseren](sql-data-warehouse-develop-best-practices-transactions.md).

## <a name="automating-compute-management"></a>Automatiseren van compute-beheer
Zie voor het automatiseren van de compute-beheerbewerkingen [beheren Computing met Azure functions](manage-compute-with-azure-functions.md).

Elk van de scale-out, onderbreken en hervattingsbewerkingen kan enkele minuten duren. Als u schalen, onderbreken, of automatisch hervat, wordt aanbevolen voor het implementeren van logica om ervoor te zorgen dat wordt bepaalde bewerkingen zijn voltooid voordat u doorgaat met een andere actie. De status van datawarehouse via verschillende eindpunten te controleren, kunt u goed implementeren automatisering van dergelijke bewerkingen plaatsvindt. 

U kunt de status van datawarehouse controleren de [PowerShell](quickstart-scale-compute-powershell.md#check-data-warehouse-state) of [T-SQL](quickstart-scale-compute-tsql.md#check-data-warehouse-state) Quick Start. U kunt ook met de status van datawarehouse controleren een [REST-API](sql-data-warehouse-manage-compute-rest-api.md#check-database-state).


## <a name="permissions"></a>Machtigingen

De machtigingen die worden beschreven in het datawarehouse schalen vereist [ALTER DATABASE](/sql/t-sql/statements/alter-database-azure-sql-data-warehouse).  Onderbreken en hervatten vereisen de [' SQL DB Contributor '](../role-based-access-control/built-in-roles.md#sql-db-contributor) machtiging, specifiek Microsoft.Sql/servers/databases/action.


## <a name="next-steps"></a>Volgende stappen
Een ander aspect van het beheren van rekenresources is verschillende compute-resources toewijzen voor afzonderlijke query's. Zie voor meer informatie, [resourceklassen voor het beheer van de werkbelasting](resource-classes-for-workload-management.md).
