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
ms.openlocfilehash: cd177b1ebcb5d236ce265dc153ee6a02125a69df
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="advanced-json-transformations-using-liquid-template"></a>Geavanceerde JSON transformaties met vloeibare sjabloon
Logische Apps van Azure ondersteunt eenvoudige JSON-transformaties via systeemeigen gegevensbewerking acties zoals opstellen of JSON niet parseren. Logic Apps nu biedt ook ondersteuning voor geavanceerde JSON-transformaties met vloeibare sjablonen. [Vloeibare](https://shopify.github.io/liquid/) is een van de open source-sjabloontaal voor flexibele web-apps.
 
Informatie over het gebruik van een vloeibare kaart of de sjabloon, die ondersteuning voor complexere JSON-transformaties, zoals iteraties, besturingselement stromen en variabelen bieden in dit artikel. U moet JSON en JSON-toewijzing met deze kaart vloeibare definiëren en die zijn toegewezen in uw account integratie opslaan voordat u een vloeibare transformatie in uw logische app kunt uitvoeren.

## <a name="prerequisites"></a>Vereisten
Hier volgen de vereisten voor het gebruik van de vloeibare actie:

* Een Azure-abonnement. Als u geen abonnement hebt, kunt u [beginnen met een gratis Azure-account](https://azure.microsoft.com/free/). U kunt eventueel ook direct kiezen voor [een Betalen per gebruik-abonnement](https://azure.microsoft.com/pricing/purchase-options/).

* Elementaire kennis over [het maken van logische apps](../logic-apps/logic-apps-create-a-logic-app.md).

* Een eenvoudige [integratie Account](logic-apps-enterprise-integration-create-integration-account.md).


## <a name="create-and-add-liquid-template-or-map-to-integration-account"></a>Vloeibare sjabloon toevoegen of toewijzen aan integratie-account maken

1. Maak de vloeibare voorbeeldsjabloon voor dit voorbeeld. De vloeibare sjabloon definieert hoe voor het transformeren van JSON-invoer zoals hier wordt beschreven:

   ```
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

4. Geef uw account integratie in het zoekvak. Selecteer uw account.

   ![Integratie-account selecteren](./media/logic-apps-enterprise-integration-liquid-transform/select-integration-account.png)

5.  Selecteer op de tegel van het account integratie **Maps**.

   ![Selecteer maps](./media/logic-apps-enterprise-integration-liquid-transform/add-maps.png)

6. Kies **toevoegen** en deze informatie geven voor de kaart:
  * **Naam**: de naam voor uw kaart 'JsontoJsonTemplate' in dit voorbeeld is.
  * **Type toewijzing**: het type voor uw kaart. U moet selecteren voor JSON voor JSON-transformatie **vloeibare**.
  * **Kaart**: een bestaand vloeibare sjabloon of kaart bestand moet worden gebruikt voor transformatie 'SimpleJsonToJsonTemplate.liquid' in dit voorbeeld. Gebruik de bestandskiezer kunt u dit bestand niet vinden.

    ![Vloeibare sjabloon toevoegen](./media/logic-apps-enterprise-integration-liquid-transform/add-liquid-template.png)

    
## <a name="add-the-liquid-action-to-transform-json-in-your-logic-app"></a>De vloeibare actie voor het transformeren van JSON in uw logische app toevoegen

1. [Een logische app maken](logic-apps-create-a-logic-app.md).

2. Voeg de [aanvraag trigger](../connectors/connectors-native-reqres.md#use-the-http-request-trigger) aan uw logische app.

3. Kies **+ een nieuwe stap > een actie toevoegen**. Zoeken naar *vloeibare* in het zoekvak. Selecteer **vloeistof - transformatie JSON naar JSON**.

  ![Zoeken-actie-liquide](./media/logic-apps-enterprise-integration-liquid-transform/search-action-liquid.png)

4. In de **inhoud** de optie **hoofdtekst** uit de lijst met dynamische inhoud of de parameterlijst met afhankelijk van wat wordt weergegeven. 
  
  ![Selecteer hoofdtekst](./media/logic-apps-enterprise-integration-liquid-transform/select-body.png)
 
5. Van de **kaart** vervolgkeuzelijst, selecteert u uw vloeibare sjabloon JsonToJsonTemplate in dit voorbeeld.

  ![Selecteer-kaart](./media/logic-apps-enterprise-integration-liquid-transform/select-map.png)

   Als de lijst leeg is, wordt uw logische app waarschijnlijk niet is gekoppeld aan uw account integratie. Als u wilt uw logische app koppelen aan het account voor integratie met de sjabloon vloeibare of kaart, de volgende stappen uit:

   1. Selecteer op uw logische app-menu **Werkstroominstellingen**. 
   2. Van de **selecteert u een account integratie** lijst, selecteert u uw integratie-account en kies **opslaan**.

     ![Logic app koppelen bij integratie-account](./media/logic-apps-enterprise-integration-liquid-transform/link-integration-account.png)


## <a name="test-your-logic-app"></a>Uw logische app testen

   Na de JSON-invoer aan uw logische app van [Postman](https://www.getpostman.com/postman) of een vergelijkbaar hulpprogramma. De getransformeerde JSON-uitvoer van uw logische app ziet er in dit voorbeeld:
  
   ![Voorbeeld van uitvoer](./media/logic-apps-enterprise-integration-liquid-transform/example-output.png)


## <a name="next-steps"></a>Volgende stappen
* [Meer informatie over het Enterprise-integratiepakket](../logic-apps/logic-apps-enterprise-integration-overview.md "meer informatie over Enterprise Integration Pack")  
* [Meer informatie over maps](../logic-apps/logic-apps-enterprise-integration-maps.md "meer informatie over enterprise integration maps")  

