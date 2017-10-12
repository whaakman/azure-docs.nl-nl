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
ms.openlocfilehash: a5a73a9f124e5339b61dbc49021444a208a471f0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-migrate-logic-apps-to-schema-version-2015-08-01-preview"></a>Logische apps migreren naar preview-schemaversie 2015-08-01
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
* [Meer informatie over het handmatig migreren van uw Logic Apps](../logic-apps/logic-apps-schema-2015-08-01.md)

<!--Icon references-->
[step1]: ./media/connectors-schema-migration/migrateschema1.png
[step2]: ./media/connectors-schema-migration/migrateschema2.png






