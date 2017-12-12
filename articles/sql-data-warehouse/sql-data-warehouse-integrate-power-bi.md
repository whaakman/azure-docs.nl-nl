---
title: Power BI gebruiken met SQL datawarehouse | Microsoft Docs
description: Tips voor het gebruik van Power BI met Azure SQL Data Warehouse om oplossingen te ontwikkelen.
services: sql-data-warehouse
documentationcenter: NA
author: mlee3gsd
manager: jhubbard
editor: 
ms.assetid: b12bee87-2268-40c2-81bf-ab27588b32e8
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: integrate
ms.date: 10/31/2016
ms.author: martinle;barbkess
ms.openlocfilehash: 4ea9a2ff0c95a73b348d3b48e9e62957d5cce31c
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="use-power-bi-with-sql-data-warehouse"></a>Power BI gebruiken met SQL datawarehouse
Als met Azure SQL Database kan SQL Data Warehouse Direct Connect gebruiker gebruikmaken van krachtige logische naar beneden duwen naast de analytische functies van Power BI.  Query's worden met Direct Connect terug naar uw Azure SQL Data Warehouse in realtime verzonden als u de gegevens verkennen.  Deze, gecombineerd met de schaal van SQL Data Warehouse, kunnen gebruikers dynamische om rapporten te maken in minuten op basis van terabytes aan gegevens.  Bovendien kan de introductie van de openen in Power BI-knop gebruikers direct verbinding maken met Power BI hun SQL Data Warehouse zonder gegevens te verzamelen uit andere delen van Azure.

Als u Direct Connect Ga Opmerking:

* Geef de volledig gekwalificeerde servernaam wanneer verbinding wordt gemaakt (Zie hieronder voor meer informatie)
* Zorg ervoor dat de firewallregels voor de database zijn ingesteld op 'Toegang tot Azure-services toestaan'.
* Elke actie zoals een kolom selecteren of een filter toe te voegen rechtstreeks een query uit op het datawarehouse
* Tegels worden vernieuwd ongeveer elke 15 minuten (vernieuwen niet hoeft te worden gepland)
* Met Q & A is niet beschikbaar voor gegevenssets die Direct Connect
* Wijzigingen in het schema worden niet automatisch opgepikt
* Alle Direct verbinding maken met query's na 2 minuten

Deze beperkingen en -opmerkingen kunnen wijzigen, omdat we verder te verbeteren de ervaring. De stappen om verbinding te worden hieronder beschreven.  

## <a name="using-the-open-in-power-bi-button"></a>De knop 'Openen in Power BI'
De eenvoudigste manier om te verplaatsen tussen uw SQL Data Warehouse en Power BI is met de openen in Power BI-knop. Deze knop kunt u naadloos begint met het maken van nieuwe dashboards in Power BI.  

1. Aan de slag gaat u naar uw SQL Data Warehouse-exemplaar in de Azure portal.
2. Klik op de knop Openen in Power BI.
3. Als we geen kunnen u rechtstreeks aanmelden, of als u een Power BI-account niet hebt, u moet aan te melden.  
4. U wordt omgeleid naar de pagina SQL Data Warehouse verbinding met de gegevens van uw SQL Data Warehouse vooraf worden ingevuld.
5. Na het invoeren van uw referenties u volledig wordt verbonden met uw SQL Data Warehouse.

## <a name="connecting-through-the-power-bi-portal"></a>Verbinding maken via de portal Power BI
Naast het gebruik van de openen in Power BI-knop, kunnen gebruikers ook verbinding maken met hun SQL Data Warehouse via de Power BI-Portal.

1. Klik op gegevens ophalen onderaan in het navigatiedeelvenster.
2. Selecteer 'Databases'.
3. Selecteer één keer op de pagina Databases 'Azure SQL Data Warehouse' en klik op 'Connect'.
4. Voer de noodzakelijke verbindingsgegevens.  De servernaam en databasenaam vindt in de Azure Portal.
5. U wordt omgeleid naar de hoofdpagina van Power BI en nadat de verbinding wordt gemaakt van een nieuwe vermelding onder 'Gegevenssets' wordt weergegeven met de naam van uw exemplaar.  
6. U kunt klikken op de nieuwe gegevensset om te verkennen van alle tabellen en weergaven in de database. Als u een kolom selecteert, stuurt een query terug naar de bron, het visuele element dynamisch te maken. Deze visuele elementen kunnen worden opgeslagen in een nieuw rapport en terug vastgemaakt aan uw dashboard.

<!--Image references-->

<!--Article references-->
[SQL Data Warehouse development overview]:  ./sql-data-warehouse-overview-develop/
[SQL Data Warehouse integration overview]:  ./sql-data-warehouse-overview-integration/

<!--MSDN references-->

<!--Other Web references-->
