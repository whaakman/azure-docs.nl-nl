---
title: Lagere rekenlagen biedt ondersteuning voor Azure SQL Data Warehouse Gen2 | Microsoft Docs
description: Azure SQL Data Warehouse Gen2 biedt nu ondersteuning voor lagere rekenlagen
author: antvgski
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: design
ms.date: 12/03/2018
ms.author: anvang
ms.reviewer: igorstan
ms.openlocfilehash: 2aa513617f24201dfb341f9ab72ab9e3a221450d
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54819353"
---
# <a name="azure-sql-data-warehouse-gen2-support-for-lower-compute-tiers"></a>Azure SQL Data Warehouse Gen2-ondersteuning voor lagere rekenlagen

Microsoft helpt oppervlaktegebied de op instapniveau kosten van het uitvoeren van een datawarehouse kan verwerken veeleisende van query's door lagere rekenlagen voor de verbluffend snel Azure SQL Data Warehouse Gen2 toe te voegen. Klanten kunnen de clientervaringsfuncties Azure SQL Data Warehouse van toonaangevende prestaties, flexibiliteit en beveiliging beginnen met 100 cDWU (Data Warehouse Units) en schaal binnen enkele minuten naar 30.000 cDWU. Klanten kunnen profiteren van Gen2 prestaties en flexibiliteit met lagere rekenlagen. 

Door slepen en neerzetten het toegangspunt voor datawarehouses bij een volgende generatie, geeft Microsoft de mogelijkheid op waarde gebaseerde klanten die willen evalueren van alle voordelen van een veilige, krachtige datawarehouse zonder te raden welke evaluatieomgeving voor hen het beste is.  Klanten zich omlaag slechts 100 cDWU starten vanuit de huidige 500 cDWU-toegangspunt.  SQL Data Warehouse Gen2 blijft ondersteuning voor onderbreken en hervatten van bewerkingen en inhoudt dan alleen de flexibiliteit in compute.  Gen2 biedt ook ondersteuning voor columnstore is onbeperkte opslagcapaciteit samen met 2,5 keer zoveel geheugen per query maximaal 128 gelijktijdige query's en brengen ervaringen op het gemiddelde van 5 keer betere prestaties in vergelijking met de dezelfde Data Warehouse Unit op adaptieve cache-opslag-functies Gen1 tegen dezelfde prijs.  Geografisch redundante back-ups worden standaard voor Gen2 met ingebouwde gegarandeerde gegevensbeveiliging. Azure SQL Data Warehouse Gen2 is gereed om te schalen wanneer u bent.

De portal biedt momenteel geen ondersteuning de implementatie van of naar de onderste laag Gen2 instanties te schalen. We werken hard te schakelen deze functionaliteit in de tussentijd [dien een ticket in](sql-data-warehouse-get-started-create-support-ticket.md) als u wilt profiteren van deze nieuwe laag.

## <a name="getting-started-with-azure-sql-data-warehouse-gen2"></a>Aan de slag met Azure SQL Data Warehouse Gen2 

Klanten kunnen kiezen voor het implementeren van een nieuwe Gen2 instantie of een upgrade van een bestaand Gen1 datawarehouse-exemplaar om de flexibiliteit en prestaties van de volgende generatie datawarehousing. 

Probeer [Azure SQL datawarehouse Gen2 met geoptimaliseerde Rekenlaag.](https://azure.microsoft.com/services/sql-data-warehouse/?v=17.44)
Upgrade [Azure SQL Data Warehouse Compute geoptimaliseerd Gen1 naar Gen2](https://docs.microsoft.com/azure/sql-data-warehouse/upgrade-to-latest-generation) Bekijk Azure SQL Data Warehouse Gen2 in actie in deze [video van Microsoft Mechanics.](https://www.youtube.com/watch?v=Ap8I3UZonzI&feature=youtu.be)


## <a name="supported-regions-for-lower-compute-tiers"></a>Ondersteunde regio's voor lagere rekenlagen

- Oost-US1 
- US - oost 2
- Europa -west
- Europa - noord
- US - west 2
- Azië - zuidoost
- VS Zuid-centraal
- US - centraal 
- Azië - oost
- Japan - oost
- India - centraal
- Australië - oost
- Canada - midden
- Japan - west 
- Canada - midden

## <a name="next-steps"></a>Volgende stappen

- [Meer informatie](upgrade-to-latest-generation.md) over de prestaties van Compute geoptimaliseerd door te upgraden van SQL Data Warehouse. 
