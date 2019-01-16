---
title: Apps migreren naar de meest recente schema - Azure Logic Apps | Microsoft Docs
description: Uw logische apps migreren naar de nieuwste schemaversie
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.assetid: 3e177e49-fd69-43e9-9b9b-218abb250c31
ms.topic: article
ms.date: 08/25/2018
ms.openlocfilehash: bf27739bd42106550c18e3bbc27a1ff8b3770747
ms.sourcegitcommit: 3ba9bb78e35c3c3c3c8991b64282f5001fd0a67b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/15/2019
ms.locfileid: "54321123"
---
# <a name="migrate-logic-apps-to-latest-schema-version"></a>Logische apps migreren naar de nieuwste schemaversie

Voor het verplaatsen van uw bestaande logische apps naar de nieuwste schema, de volgende stappen uit: 

1. In de [Azure-portal](https://portal.azure.com), opent u uw logische app in Logic App Designer.

2. Kies in het menu van uw logische app **overzicht**. Kies op de werkbalk **Schema bijwerken**.

   > [!NOTE]
   > Als u ervoor kiest **Schema bijwerken**, automatisch de migratiestappen uitgevoerd en zorgt voor de code-uitvoer voor uw Azure Logic Apps. U kunt deze uitvoer kunt gebruiken voor het bijwerken van de definitie van uw logische app. Echter, zorg ervoor dat u aanbevolen procedures volgt, zoals beschreven in de volgende **Best practices** sectie.

   ![Schema bijwerken](./media/connectors-schema-migration/update-schema.png)

   De pagina Schema bijwerken wordt weergegeven en ziet u een koppeling naar een document met een beschrijving van de verbeteringen in het nieuwe schema.

## <a name="best-practices"></a>Aanbevolen procedures

Hier volgen enkele aanbevolen procedures voor het migreren van uw logische apps naar de nieuwste schemaversie:

* Kopieer het gemigreerde script naar een nieuwe logische app. De oude versie niet worden overschreven, totdat u klaar bent met testen en controleren of uw gemigreerde app werkt zoals verwacht.

* Test uw logische app **voordat** deze in productie neemt.

* Nadat u de migratie hebt voltooid, start u het bijwerken van uw logische apps gebruiken de [beheerde API's](../connectors/apis-list.md) waar mogelijk. Bijvoorbeeld starten met behulp van de Dropbox v2 overal dat u DropBox v1 gebruikt.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over het [handmatig migreren uw logische apps](../logic-apps/logic-apps-schema-2015-08-01.md)
