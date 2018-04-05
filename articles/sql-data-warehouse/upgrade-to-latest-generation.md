---
title: Upgrade naar de nieuwste van Azure SQL Data Warehouse | Microsoft Docs
description: Stappen om Azure SQL Data Warehouse een upgrade uitvoert naar de nieuwste generatie van Azure hardware- en storage-architectuur.
services: sql-data-warehouse
author: kevinvngo
manager: craigg-msft
ms.services: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 04/02/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: 42b716274e655bf91f72c1b3ab207b8a5f1ccee0
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2018
---
# <a name="upgrade-to-latest-generation-of-azure-sql-data-warehouse-in-the-azure-portal"></a>Upgrade uitvoeren naar Azure SQL Data Warehouse nieuwste generatie in de Azure portal

De Azure portal gebruiken om bij te werken van uw Azure SQL Data Warehouse voor het gebruik van de nieuwste Azure hardware- en storage-architectuur. Door een upgrade uitvoert, kunt u profiteren van betere prestaties, grotere schaalbaarheid en onbeperkte opslag voor de columnstore-indexen.  

## <a name="applies-to"></a>Van toepassing op
Deze upgrade geldt voor datawarehouses in de optimaliseren voor de prestatielaag elasticiteit.  De instructies voor het upgraden van een datawarehouse van de optimaliseren voor de prestatielaag elasticiteit naar de optimaliseren voor de prestatielaag Compute. 

## <a name="sign-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Meld u aan bij [Azure Portal](https://portal.azure.com/).

## <a name="before-you-begin"></a>Voordat u begint

1. Als de geoptimaliseerde voor elasticiteit datawarehouse moet worden bijgewerkt, is onderbroken, [hervatten van het datawarehouse](pause-and-resume-compute-portal.md).
2. Worden voorbereid voor een aantal minuten uitvaltijd. 
3. Het upgradeproces alle sessies beëindigd en wordt alle verbindingen. Voordat u bijwerkt, moet u dat uw query's hebben voltooid. Als u een upgrade met transacties in uitvoering wordt gestart, is het terugdraaien van de tijd kan worden uitgebreid. 

## <a name="start-the-upgrade"></a>De upgrade start

1. Open uw datawarehouse in de Azure portal en klikt u op **Upgrade naar geoptimaliseerd voor Compute**.
2. U ziet de optimaliseren voor Compute prestaties laag keuzes. De standaardselectie is vergelijkbaar met het huidige niveau vóór de upgrade.
3. Kies een laag met prestaties. De prijs van de optimaliseren voor de prestatielaag Compute is momenteel half-uitgeschakeld tijdens de preview-periode.
4. Klik op **Upgrade**.
5. Controleer de status in de Azure portal.
6. Wacht tot het datawarehouse worden gewijzigd in Online.

## <a name="rebuild-columnstore-indexes"></a>Columnstore-indexen opnieuw samenstellen

Zodra uw datawarehouse online is, kunt u het laden van gegevens en query's uitvoeren. Prestaties kan echter zijn in eerste instantie vertragen, omdat een achtergrondproces de gegevens naar de nieuwe hardware is gemigreerd. 

Als u wilt dat de gegevens voor het migreren van zo snel mogelijk, wordt u aangeraden de columnstore-indexen opnieuw opbouwen. Om dit te doen, Zie de richtlijnen voor [columnstore-indexen ter verbetering van de kwaliteit van segment opnieuw opbouwen](sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality). 

## <a name="next-steps"></a>Volgende stappen
Uw datawarehouse is online. Zie voor het gebruik van de nieuwe prestatiefuncties [Resource klassen voor het beheer van de werkbelasting](resource-classes-for-workload-management.md).
 