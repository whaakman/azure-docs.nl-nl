---
title: Logic Apps B2B edifact decoderen oplossen UNH2.5 - Azure Logic Apps | Microsoft Docs
description: Azure Logic Apps B2B edifact decoderen oplossen UNH2.5
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: cf44af18-1fe5-41d5-9e06-cc57a968207c
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/27/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: 62ad8183cc6e9f56255b2729a04ee7710d00a21a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-handle-edifact-documents-having-unh25-segment"></a>Hoe moet worden verwerkt EDIFACT documenten UNH2.5 segment
Wanneer UNH2.5 aanwezig in het document EDIFACT is, wordt deze gebruikt voor het opzoeken van het schema. 

Voorbeeld: Het veld UNH is **EAN008** in het bericht EDIFACT  
UNH + SSDD1 + ORDERS: D: 03B: ONGEDAAN MAKEN:**EAN008**'  

Stappen voor het afhandelen van het bericht 
1. Het schema bijwerken
2. Controleer de instellingen van de overeenkomst  

## <a name="update-the-schema"></a>Het schema bijwerken
Voor het verwerken van het bericht, moet u een schema met de naam UNH2.5 hoofdknooppunt implementeren.  Voor een voorbeeld gegeven, zijn de naam van het schema-hoofdmap **EFACT_D03B_ORDERS_EAN008**  

Voor elke D03B_ORDERS met een andere UNH2.5 segment, zou u moet een afzonderlijk schema implementeren.  

## <a name="add-schema-to-the-edifact-agreement"></a>Schema niet toevoegen aan de overeenkomst EDIFACT
### <a name="edifact-decode"></a>EDIFACT decoderen
Het schema configureren voor het decoderen van het binnenkomende bericht in de EDIFACT ontvangen overeenkomst instellingen
1. Het schema niet toevoegen aan de integratie-account    
2. Het schema te configureren in de EDIFACT ontvangen overeenkomst instellingen. 
3. Selecteer EDIFACT overeenkomst en klik op **bewerken als JSON**.  UNH2.5 waarde toevoegen in de overeenkomst ontvangen **schemaReferences**
![](./media/logic-apps-enterprise-integration-edifact_inputfile_unh2.5/image1.png)

### <a name="edifact-encode"></a>EDIFACT coderen
Het schema voor het coderen van het binnenkomende bericht configureren in de instellingen voor EDIFACT overeenkomst verzenden
1. Het schema niet toevoegen aan de integratie-account    
2. Configureer het schema in de instellingen voor EDIFACT overeenkomst verzenden. 
3. Selecteer EDIFACT overeenkomst en klik op **bewerken als JSON**.  UNH2.5 waarde toevoegen in de overeenkomst verzenden **schemaReferences**
![](./media/logic-apps-enterprise-integration-edifact_inputfile_unh2.5/image2.png)

## <a name="next-steps"></a>Volgende stappen
* [Meer informatie over de integratie account overeenkomsten](../logic-apps/logic-apps-enterprise-integration-agreements.md "meer informatie over enterprise integration-overeenkomsten")  