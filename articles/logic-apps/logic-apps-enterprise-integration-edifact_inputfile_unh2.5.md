---
title: EDIFACT-berichten met UNH 2.5 segements - Azure Logic Apps verwerken | Microsoft Docs
description: EDIFACT-documenten met UNH2.5-segmenten in Azure Logic Apps met Enterprise Integration Pack oplossen
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.assetid: cf44af18-1fe5-41d5-9e06-cc57a968207c
ms.date: 04/27/2017
ms.openlocfilehash: 926c9ebe8675d8b50d4544be813ae0b15492ae35
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60681653"
---
# <a name="handle-edifact-documents-with-unh25-segments-in-azure-logic-apps"></a>EDIFACT-documenten met UNH2.5-segmenten in Azure Logic Apps

Wanneer UNH2.5 aanwezig in het EDIFACT-document is, wordt deze gebruikt voor het opzoeken van het schema. 

Voorbeeld: Het veld UNH **EAN008** in het EDIFACT-bericht  
UNH + SSDD1 + ORDERS: D: 03B: ONGEDAAN MAKEN:**EAN008**'  

Stappen voor het afhandelen van het bericht 
1. Het schema bijwerken
2. Controleer de instellingen van de overeenkomst  

## <a name="update-the-schema"></a>Het schema bijwerken
Voor het verwerken van het bericht, moet u een schema met de naam van het hoofdknooppunt UNH2.5 implementeren.  Voor een voorbeeld gegeven, de naam van het schema-hoofdmap zou worden **EFACT_D03B_ORDERS_EAN008**  

Voor elke D03B_ORDERS met een andere UNH2.5-segment, zou u moet een afzonderlijk schema implementeren.  

## <a name="add-schema-to-the-edifact-agreement"></a>Schema toevoegen aan de EDIFACT-overeenkomst
### <a name="edifact-decode"></a>EDIFACT-decodering
Als u wilt het binnenkomende bericht decoderen, configureert u het schema in het EDIFACT overeenkomst ontvangstinstellingen
1. Het schema toevoegen aan het integratieaccount    
2. Configureren van het schema in het EDIFACT-overeenkomst ontvangen instellingen. 
3. Selecteer de EDIFACT-overeenkomst en klik op **bewerken as JSON**.  UNH2.5-waarde toevoegen in de overeenkomst ontvangen **schemaReferences**
![](./media/logic-apps-enterprise-integration-edifact_inputfile_unh2.5/image1.png)

### <a name="edifact-encode"></a>EDIFACT-codering
Als u wilt het binnenkomende bericht coderen, configureert u het schema in de verzendinstellingen van de EDIFACT-overeenkomst
1. Het schema toevoegen aan het integratieaccount    
2. Configureer het schema in de verzendinstellingen van de EDIFACT-overeenkomst. 
3. Selecteer de EDIFACT-overeenkomst en klik op **bewerken as JSON**.  UNH2.5-waarde toevoegen in de overeenkomst verzenden **schemaReferences**
![](./media/logic-apps-enterprise-integration-edifact_inputfile_unh2.5/image2.png)

## <a name="next-steps"></a>Volgende stappen
* [Meer informatie over de integratie-account overeenkomsten](../logic-apps/logic-apps-enterprise-integration-agreements.md "meer informatie over enterprise integration-overeenkomsten")  