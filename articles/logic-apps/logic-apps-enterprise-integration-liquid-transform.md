---
title: Converteren van JSON-gegevens met vloeibare transformaties - Azure Logic Apps | Microsoft Docs
description: Transformaties of kaarten voor geavanceerde JSON transformaties met Logic Apps en vloeibare sjabloon maken.
services: logic-apps
documentationcenter: 
author: divyaswarnkar
manager: anneta
editor: 
ms.assetid: 
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/05/2017
ms.author: LADocs; divswa
ms.openlocfilehash: c1a1a5530c19d39a8e37d122235c8340caa88570
ms.sourcegitcommit: 817c3db817348ad088711494e97fc84c9b32f19d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/20/2018
---
# <a name="perform-advanced-json-transformations-with-a-liquid-template"></a>Geavanceerde JSON-transformaties aan een sjabloon voor vloeibare uitvoeren

Logische Apps van Azure ondersteunt eenvoudige JSON-transformaties via native gegevenstype bewerking acties zoals **opstellen** of **parseren JSON**. U kunt de vloeibare sjablonen met uw logische apps gebruiken voor geavanceerde JSON-transformaties. 
[Vloeibare](https://shopify.github.io/liquid/) is een van de open source-sjabloontaal voor flexibele web-apps.
 
Informatie over het gebruik van een vloeibare kaart of de sjabloon, die ondersteuning biedt voor complexere JSON-transformaties, zoals iteraties, besturingselement stromen en variabelen in dit artikel. Voordat u een vloeibare transformatie in uw logische app uitvoeren kunt, moet u de toewijzing van JSON naar JSON met een vloeibare toewijzen en sla die zijn toegewezen in uw account integratie definiëren.

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u geen abonnement hebt, kunt u [beginnen met een gratis Azure-account](https://azure.microsoft.com/free/). U kunt eventueel ook direct kiezen voor [een Betalen per gebruik-abonnement](https://azure.microsoft.com/pricing/purchase-options/).

* Elementaire kennis over [logic apps maken](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* Een eenvoudige [integratie-Account](logic-apps-enterprise-integration-create-integration-account.md)


## <a name="create-a-liquid-template-or-map-for-your-integration-account"></a>Een vloeibare sjabloon of de kaart voor uw account integratie maken

1. Maak de vloeibare voorbeeldsjabloon voor dit voorbeeld. De vloeibare sjabloon definieert hoe voor het transformeren van JSON-invoer zoals hier wordt beschreven:

   ``` json
   {%- assign deviceList = content.devices | Split: ', ' -%}
      {
        "fullName": "{{content.firstName | Append: ' ' | Append: content.lastName}}",
        "firstNameUpperCase": "{{content.firstName | Upcase}}",
        "phoneAreaCode": "{{content.phone | Slice: 1, 3}}",
        "devices" : [
        {%- for device in deviceList -%}
            {%- if forloop.Last == true -%}
            "{{device}}"
            {%- else -%}
            "{{device}}",
            {%- endif -%}
        {%- endfor -%}
        ]
      }
   ```
   > [!NOTE]
   > Als uw vloeibare sjabloon maakt gebruik van een [filters](https://shopify.github.io/liquid/basics/introduction/#filters), deze filters moeten beginnen met hoofdletters. 

2. Meld u aan bij [Azure Portal](https://portal.azure.com).

3. Kies in het Azure hoofdmenu **alle resources**. 

4. Zoek in het zoekvak en selecteert u uw integratie-account.

   ![Integratie-account selecteren](./media/logic-apps-enterprise-integration-liquid-transform/select-integration-account.png)

5.  Selecteer op de tegel van het account integratie **Maps**.

   ![Selecteer maps](./media/logic-apps-enterprise-integration-liquid-transform/add-maps.png)

6. Kies **toevoegen** en deze informatie geven voor de kaart:

   * **Naam**: de naam voor uw kaart 'JsontoJsonTemplate' in dit voorbeeld is
   * **Type toewijzing**: het type voor uw kaart. U moet selecteren voor JSON voor JSON-transformatie **vloeibare**.
   * **Kaart**: een bestaand vloeibare sjabloon of kaart bestand moet worden gebruikt voor transformatie 'SimpleJsonToJsonTemplate.liquid' in dit voorbeeld. Dit bestand zoekt, kunt u de bestandskiezer.

   ![Vloeibare sjabloon toevoegen](./media/logic-apps-enterprise-integration-liquid-transform/add-liquid-template.png)
    
## <a name="add-the-liquid-action-for-json-transformation"></a>De vloeibare actie voor JSON-transformatie toevoegen

1. [Een logische app maken](../logic-apps/quickstart-create-first-logic-app-workflow.md).

2. Voeg de [aanvraag trigger](../connectors/connectors-native-reqres.md#use-the-http-request-trigger) aan uw logische app.

3. Kies **+ een nieuwe stap > een actie toevoegen**. Voer in het zoekvak *vloeibare*, en selecteer **vloeistof - JSON transformeren naar JSON**.

   ![Zoek en selecteer vloeibare actie](./media/logic-apps-enterprise-integration-liquid-transform/search-action-liquid.png)

4. In de **inhoud** de optie **hoofdtekst** uit de lijst met dynamische inhoud of de parameterlijst met afhankelijk van wat wordt weergegeven.
  
   ![Hoofdtekst selecteren](./media/logic-apps-enterprise-integration-liquid-transform/select-body.png)
 
5. Van de **kaart** , selecteert u uw vloeibare sjabloon 'JsonToJsonTemplate' in dit voorbeeld.

   ![Selecteer de kaart](./media/logic-apps-enterprise-integration-liquid-transform/select-map.png)

   Als de lijst leeg is, wordt uw logische app waarschijnlijk niet gekoppeld aan uw account integratie. 
   Als u wilt uw logische app koppelen aan het account voor integratie met de sjabloon vloeibare of kaart, de volgende stappen uit:

   1. Selecteer op uw logische app-menu **Werkstroominstellingen**.
   2. Van de **selecteert u een account integratie** lijst, selecteert u uw integratie-account en kies **opslaan**.

   ![Logic app koppelen bij integratie-account](./media/logic-apps-enterprise-integration-liquid-transform/link-integration-account.png)

## <a name="test-your-logic-app"></a>Uw logische app testen

Na de JSON-invoer aan uw logische app van [Postman](https://www.getpostman.com/postman) of een vergelijkbaar hulpprogramma. De getransformeerde JSON-uitvoer van uw logische app ziet er in dit voorbeeld:
  
![Voorbeeld van uitvoer](./media/logic-apps-enterprise-integration-liquid-transform/example-output.png)

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over het Enterprise-integratiepakket](../logic-apps/logic-apps-enterprise-integration-overview.md "meer informatie over Enterprise Integration Pack")  
* [Meer informatie over maps](../logic-apps/logic-apps-enterprise-integration-maps.md "meer informatie over enterprise integration maps")  

