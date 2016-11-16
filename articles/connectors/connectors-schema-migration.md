---
title: Logische apps migreren naar preview-schemaversie 2015-08-01 | Microsoft Azure Docs
description: U kunt uw logische apps eenvoudig migreren naar de nieuwste schemaversie. Volg deze stappen.
services: logic-apps
documentationcenter: 
author: MSFTMAN
manager: erikre
editor: 
tags: connectors
ms.assetid: 3e177e49-fd69-43e9-9b9b-218abb250c31
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/23/2016
ms.author: deonhe
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: ab22e0369781f9f9afb9b3df9e7fdd54660a959d


---
# <a name="how-to-migrate-logic-apps-to-schema-version-20150801preview"></a>Logische apps migreren naar preview-schemaversie 2015-08-01
U verplaatst uw bestaande logische apps als volgt naar het nieuwe schema:  

1. Open uw logische app in de Azure Portal  
2. Klik op Schema bijwerken:
   
   ![API-pictogram][step1]   
   De pagina Schema bijwerken wordt weergegeven en bevat een koppeling naar een document dat gedetailleerde informatie bevat over de verbeteringen in het nieuwe schema: ![API-pictogram][step2]

> [!NOTE]
> Wanneer u **Schema bijwerken** selecteert, worden automatisch de migratiestappen uitgevoerd en krijgt u de code-uitvoer. U kunt deze gebruiken om uw definitie bij te werken, maar zorg ervoor dat u de richtlijnen voor codering volg, zoals die zijn beschreven in de **aanbevolen procedures** hieronder.
> 
> 

## <a name="best-practices-when-migrating-your-logic-apps-to-the-latest-schema-version"></a>Aanbevolen procedures voor het migreren van uw logische apps naar de nieuwste schemaversie:
* Kopieer het gemigreerde script naar een nieuwe logische app. Overschrijf de oude pas nadat u deze hebt getest en hebt bevestigd dat de gemigreerde apps werken zoals verwacht.
* Uw logische app testen **voordat** u deze in productie neemt
* Nadat de migratie is voltooid, werkt u waar mogelijk uw logische apps bij, zodat deze gebruikmaken van de [beheerde API's](apis-list.md). U kunt bijvoorbeeld starten met het gebruik van de Dropbox v2-versie overal waar u DropBox v1 gebruikt.

## <a name="whats-next"></a>Volgend onderwerp
* [Meer informatie over het handmatig migreren van uw Logic Apps](../app-service-logic/app-service-logic-schema-2015-08-01.md)

<!--Icon references-->
[Stap 1]: ./media/connectors-schema-migration/migrateschema1.png
[Stap 2]: ./media/connectors-schema-migration/migrateschema2.png









<!--HONumber=Nov16_HO2-->


