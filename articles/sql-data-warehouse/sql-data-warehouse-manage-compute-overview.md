---
title: Beheren van rekenresources in Azure SQL Data Warehouse | Microsoft Docs
description: Meer informatie over prestaties scale-out mogelijkheden in Azure SQL Data Warehouse. Uitschalen door aan te passen dwu's of kosten te verlagen door het datawarehouse te onderbreken.
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: johnmac
editor: ''
ms.assetid: e13a82b0-abfe-429f-ac3c-f2b6789a70c6
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 02/20/2018
ms.author: elbutter
ms.openlocfilehash: c34e37f0c6393c65d4b60705012769608bb7395b
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/28/2018
---
# <a name="manage-compute-in-azure-sql-data-warehouse"></a>Compute in Azure SQL Data Warehouse beheren
Meer informatie over het beheren van rekenresources in Azure SQL Data Warehouse. Lagere kosten door het datawarehouse onderbreken of schalen van het datawarehouse om te voldoen aan de prestatie-eisen. 

## <a name="what-is-compute-management"></a>Wat is compute management?
De architectuur van SQL Data Warehouse scheidt opslag en berekeningen, zodat deze afzonderlijk kunnen worden geschaald. Als gevolg hiervan kunt u berekenen om te voldoen aan de prestatie-eisen onafhankelijk van de opslag van gegevens schalen. U kunt ook onderbreken en hervatten van de compute-bronnen. Een natuurlijke gevolg van deze architectuur is dat [facturering](https://azure.microsoft.com/pricing/details/sql-data-warehouse/) voor berekeningen en opslag is gescheiden. Als u uw datawarehouse gebruiken voor een tijdje niet wilt, kunt u de compute-kosten besparen door onderbreken compute. 

## <a name="scaling-compute"></a>Compute schalen
U kunt uitbreiden of back compute schalen door het aanpassen van de [datawarehouse eenheden](what-is-a-data-warehouse-unit-dwu-cdwu.md) instellen voor uw datawarehouse. Bij het laden en queryprestaties kunt lineair verhogen als u meer datawarehouse units toevoegen. SQL Data Warehouse biedt [serviceniveaus](performance-tiers.md#service-levels) voor gegevens datawarehouse eenheden die zorgen voor een merkbare veranderingen in de prestaties wanneer u schalen out- of back. 

Zie voor stappen voor scale-out de [Azure-portal](quickstart-scale-compute-portal.md), [PowerShell](quickstart-scale-compute-powershell.md), of [T-SQL](quickstart-scale-compute-tsql.md) snelstartgidsen. U kunt ook scale-out bewerkingen uitvoeren met een [REST-API](sql-data-warehouse-manage-compute-rest-api.md#scale-compute).

Als u een schaalaanpassing, SQL Data Warehouse eerst is funest alle binnenkomende query's en vervolgens teruggedraaid transacties om te controleren of een consistente status. Schalen vindt alleen plaats wanneer het terugdraaien van de transactie voltooid is. Voegt de rekenknooppunten en vervolgens reattaches de storage-laag aan de Compute-laag voor het uitvoeren van een scale het systeem wordt losgekoppeld van de storage-laag van de rekenknooppunten. Elk datawarehouse wordt opgeslagen als 60 distributies die gelijkmatig verdeeld zijn in de rekenknooppunten. Meer computerknooppunten toe te voegen, voegt dat meer rekenkracht. Als het aantal rekenknooppunten toeneemt, afneemt het aantal distributies per rekenknooppunt, bieden meer rekencapaciteit voor uw query's. Evenzo vermindert verlagen datawarehouse units het aantal rekenknooppunten, waardoor de rekenresources voor query's.

De volgende tabel ziet hoe het aantal distributies per Compute knooppunt verandert als de datawarehouse eenheden wijzigen.  DWU6000 biedt 60 rekenknooppunten en veel hogere queryprestaties dan DWU100 bereikt. 

| Datawarehouse-eenheden  | \# van rekenknooppunten | \# van distributies per knooppunt |
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


## <a name="finding-the-right-size-of-data-warehouse-units"></a>Zoeken naar de juiste grootte datawarehouse Units

Als u wilt zien van de prestatievoordelen van uitbreiden, met name voor grotere datawarehouse-eenheden die u wilt gebruiken van ten minste een gegevensset 1 TB. Het aanbevolen aantal datawarehouse Units voor uw datawarehouse vindt probeer omhoog en omlaag schalen. Enkele query's uitvoeren met verschillende aantallen datawarehouse Units na het laden van uw gegevens. Omdat schalen snel is, kunt u verschillende prestatieniveaus proberen in een uur of minder. 

Aanbevelingen voor het vinden van het aanbevolen aantal data warehouse units:

- Beginnen met het selecteren van een kleiner getal datawarehouse Units voor een datawarehouse in ontwikkeling.  Een goed uitgangspunt is DW400 of DW200.
- De toepassingsprestaties van uw, het nummer datawarehouse Units geselecteerd vergeleken met de prestaties die u merkt observeren bewaken.
- Stel een lineaire schaal en bepaalt hoeveel u wilt vergroten of verkleinen van de datawarehouse-eenheden. 
- Blijven correcties totdat u een optimaal prestatieniveau voor uw zakelijke vereisten.

## <a name="when-to-scale-out"></a>Wanneer moet worden uitgebreid
Deze aspecten van de prestaties van invloed is op datawarehouse units uitbreiden:

- Lineair verbetert de prestaties van het systeem voor scans, aggregaties en CTAS-instructies.
- Verhoogt het aantal lezers en writers om gegevens te laden.
- Maximum aantal gelijktijdige query's en gelijktijdigheid sleuven.

Aanbevelingen voor het uitbreiden van data warehouse units:

- Voordat u een zware laad- of transformatiebewerking bewerking uitvoert, kunt u uitschalen naar de gegevens sneller beschikbaar te stellen.
- Tijdens de kantooruren piek, uitbreiden om grotere aantallen gelijktijdige query's mogelijk te maken. 

## <a name="what-if-scaling-out-does-not-improve-performance"></a>Wat gebeurt er als uitbreiden heeft niet de prestaties verbeteren?

Datawarehouse eenheden verhogen van de parallelle uitvoering wordt toegevoegd. Als het werk gelijkmatig verdeeld is over de rekenknooppunten, verbetert de aanvullende parallelle uitvoering de prestaties van query's. Als horizontaal te schalen, is de prestaties van uw niet worden gewijzigd, zijn er enkele redenen waarom dit kan gebeuren. Uw gegevens kunnen worden vervormd via de distributies of query's kunnen worden introductie van een grote hoeveelheid gegevens verplaatsen. Zie voor het onderzoeken van prestatieproblemen query [prestaties probleemoplossing](sql-data-warehouse-troubleshoot.md#performance). 

## <a name="pausing-and-resuming-compute"></a>Rekenuren voor het onderbreken en hervatten
Onderbreken compute zorgt ervoor dat de opslaglaag loskoppelen van de rekenknooppunten. De rekenresources worden vrijgegeven uit uw account. U worden niet in rekening gebracht voor compute terwijl compute is onderbroken. Hervatten van compute reattaches opslag in de rekenknooppunten en kosten voor Compute hervat. Wanneer u een datawarehouse onderbreekt:

* Reken-en geheugenbronnen worden geretourneerd aan de groep met beschikbare resources in het datacenter
* Datawarehouse unit kosten zijn voor de duur van de pauze nul.
* Opslag van gegevens wordt niet beïnvloed en uw gegevens intact blijven. 
* SQL Data Warehouse annuleert alle bewerkingen voor actieve of in de wachtrij.

Wanneer u een datawarehouse hervatten:

* SQL Data Warehouse verkrijgt berekenings-en geheugenbronnen voor uw datawarehouse eenheden instellen.
* COMPUTE-kosten voor uw datawarehouse units hervatten.
* Uw gegevens beschikbaar.
* Nadat het datawarehouse online is, moet u opnieuw opstarten van uw werkbelasting query's.

Als u altijd uw datawarehouse toegankelijk is wilt, kunt schalen omlaag naar de kleinste grootte, in plaats van onderbreken. 

Voor onderbreken en hervatten van de stappen, ziet de [Azure-portal](pause-and-resume-compute-portal.md), of [PowerShell](pause-and-resume-compute-powershell.md) snelstartgidsen. U kunt ook de [onderbreken REST-API](sql-data-warehouse-manage-compute-rest-api.md#pause-compute) of de [hervatten REST-API](sql-data-warehouse-manage-compute-rest-api.md#resume-compute).

## <a name="drain-transactions-before-pausing-or-scaling"></a>Transacties stoppen voor onderbreken of schalen
U wordt aangeraden zodat bestaande transacties te voltooien voordat u een pauze of scale-bewerking starten.

Wanneer u uw SQL Data Warehouse onderbreekt of schaalt, worden uw query’s achter de schermen geannuleerd zodra u het onderbreek- of schaalverzoek start.  Een eenvoudige SELECT-query annuleren is een snelle bewerking en heeft zo goed als geen invloed op de duur van het onderbreken of schalen van uw instantie.  Maar transactiequery’s, die uw gegevens of de structuur van uw gegevens wijzigen, kunnen mogelijk niet snel worden stopgezet.  **Transactiequery’s moeten per definitie volledig worden voltooid of hun wijzigingen volledig terugdraaien.**  Het kan even lang of langer duren om het werk dat door een transactiequery is voltooid, terug te draaien, als het uitvoeren van de oorspronkelijke opdracht van de query.  Als u bijvoorbeeld een query annuleert voor het verwijderen van rijen die al een uur wordt uitgevoerd, kan het systeem er een uur over doen om de verwijderde rijen terug te plaatsen.  Als u onderbreken of schalen uitvoert terwijl er transacties bezig zijn, kan het schalen of onderbreken lang lijken te duren omdat het schalen of onderbreken moet wachten op het terugdraaien van de transacties voordat het kan worden voortgezet.

Zie ook [wat zijn transacties](sql-data-warehouse-develop-transactions.md), en [optimaliseren transacties][optimaliseren transacties](sql-data-warehouse-develop-best-practices-transactions.md).

## <a name="automating-compute-management"></a>Automatiseren compute-beheer
Zie voor het automatiseren van de compute-beheerbewerkingen [compute beheren met Azure functions](manage-compute-with-azure-functions.md).

Elk van de scale-out, onderbreken en hervattingsbewerkingen kan enkele minuten duren. Als u schalen, onderbreken, of automatisch hervat, wordt aangeraden implementeren logica om ervoor te zorgen dat voltooid bepaalde bewerkingen voordat u doorgaat met een andere actie. Controleren of de status van datawarehouse via verschillende eindpunten, kunt u de automatisering van dergelijke bewerkingen correct te implementeren. 

Controleer de status van datawarehouse, se de [PowerShell](quickstart-scale-compute-powershell.md#check-data-warehouse-state) of [T-SQL](quickstart-scale-compute-tsql.md#check-data-warehouse-state) Quick Start. U kunt ook controleren met de status van datawarehouse met een [REST-API](sql-data-warehouse-manage-compute-rest-api.md#check-database-state).


## <a name="permissions"></a>Machtigingen

De machtigingen beschreven in het datawarehouse schalen vereist [ALTER DATABASE](/sql/t-sql/statements/alter-database-azure-sql-data-warehouse.md).  Onderbreken en hervatten, vereisen de [SQL DB Contributor](../active-directory/role-based-access-built-in-roles.md#sql-db-contributor) machtiging specifiek Microsoft.Sql/servers/databases/action.


## <a name="next-steps"></a>Volgende stappen
Een ander aspect van het beheren van rekenresources is toewijzen van verschillende rekenresources voor afzonderlijke query's. Zie voor meer informatie [Resource klassen voor het beheer van de werkbelasting](resource-classes-for-workload-management.md).
