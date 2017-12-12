---
title: Azure Stream Analytics gebruiken met SQL datawarehouse | Microsoft Docs
description: Tips voor het gebruik van Azure Stream Analytics met Azure SQL Data Warehouse om oplossingen te ontwikkelen.
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: barbkess
editor: 
ms.assetid: 8aeb2247-20c5-4a29-b327-30a8ce09dfdc
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: integrate
ms.date: 10/31/2016
ms.author: cakarst;barbkess
ms.openlocfilehash: c5c0450cba541a9346f023057345c5fc9b147903
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="use-azure-stream-analytics-with-sql-data-warehouse"></a>Azure Stream Analytics gebruiken met SQL datawarehouse
Azure Stream Analytics is een volledig beheerde service lage latentie, maximaal beschikbare, schaalbare complexe verwerking van gebeurtenissen geven via het streamen van gegevens in de cloud. U kunt de basisbegrippen door te lezen [Inleiding tot Azure Stream Analytics][Introduction to Azure Stream Analytics]. U kunt vervolgens informatie over het maken van een end-to-end-oplossing met Stream Analytics volgens de [aan de slag met Azure Stream Analytics] [ Get started using Azure Stream Analytics] zelfstudie.

In dit artikel leert u hoe u uw Azure SQL Data Warehouse-database als uitvoerlocatie voor uw stoom Analytics-taken.

## <a name="prerequisites"></a>Vereisten
Voer eerst de volgende stappen in de [aan de slag met Azure Stream Analytics] [ Get started using Azure Stream Analytics] zelfstudie.  

1. De invoer van een Event Hub maken
2. Configureren en starten van de gebeurtenis generator-toepassing
3. Inrichten van een Stream Analytics-taak
4. Taak invoer- en query opgeven

Vervolgens maakt u een Azure SQL Data Warehouse-database

## <a name="specify-job-output-azure-sql-data-warehouse-database"></a>Geef de taakuitvoer: Azure SQL Data Warehouse-database
### <a name="step-1"></a>Stap 1
Klik in de Stream Analytics-taak op **uitvoer** vanaf de bovenkant van de pagina en klik vervolgens op **uitvoer toevoegen**.

### <a name="step-2"></a>Stap 2
Selecteer de SQL-Database en klik op volgende.

![][add-output]

### <a name="step-3"></a>Stap 3
Voer de volgende waarden op de volgende pagina:

* *Uitvoer Alias*: een beschrijvende naam voor de taakuitvoer van deze.
* *Abonnement*:
  * Als uw SQL Data Warehouse-database zich in hetzelfde abonnement als de Stream Analytics-taak, selecteert u SQL-Database gebruiken uit het huidige abonnement.
  * Als uw database zich in een ander abonnement, selecteert u gebruik SQL Database in een ander abonnement.
* *Database*: Geef de naam van een doeldatabase.
* *Servernaam*: Geef de naam van de server voor de database die u zojuist hebt opgegeven. De Azure-portal kunt u deze worden gevonden.

![][server-name]

* *Gebruikersnaam*: Geef de gebruikersnaam van een account met schrijfmachtigingen voor de database.
* *Wachtwoord*: Geef het wachtwoord op voor het opgegeven gebruikersaccount.
* *Tabel*: Geef de naam van de doeltabel in de database.

![][add-database]

### <a name="step-4"></a>Stap 4
Klik op de knop controleren de taakuitvoer van deze toevoegen en te controleren dat Stream Analytics verbinding met de database maken kan.

![][test-connection]

Als de verbinding met de database is geslaagd, ziet u een melding aan de onderkant van de portal. U kunt klikken op verbinding testen onderaan om de verbinding met de database te testen.

## <a name="next-steps"></a>Volgende stappen
Zie voor een overzicht van de integratie van [overzicht van de integratie van SQL Data Warehouse][SQL Data Warehouse integration overview].

Zie [Overzicht van SQL Data Warehouse voor ontwikkelaars][SQL Data Warehouse development overview] voor meer tips voor ontwikkelaars.

<!--Image references-->

[add-output]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/add-output.png
[server-name]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/dw-server-name.png
[add-database]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/add-database.png
[test-connection]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/test-connection.png

<!--Article references-->

[Introduction to Azure Stream Analytics]: ../stream-analytics/stream-analytics-introduction.md
[Get started using Azure Stream Analytics]: ../stream-analytics/stream-analytics-real-time-fraud-detection.md
[SQL Data Warehouse development overview]:  ./sql-data-warehouse-overview-develop.md
[SQL Data Warehouse integration overview]:  ./sql-data-warehouse-overview-integrate.md

<!--MSDN references-->

<!--Other Web references-->
[Azure Stream Analytics documentation]: http://azure.microsoft.com/documentation/services/stream-analytics/
