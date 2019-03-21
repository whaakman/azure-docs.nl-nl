---
title: Converteren van JSON-gegevens met Liquid transformaties - Azure Logic Apps | Microsoft Docs
description: Transformaties of kaarten voor geavanceerde JSON-transformaties met Liquid sjabloon en Logic Apps maken
services: logic-apps
ms.service: logic-apps
author: divyaswarnkar
ms.author: divswa
ms.reviewer: estfan, LADocs
ms.suite: integration
ms.topic: article
ms.date: 08/16/2018
ms.openlocfilehash: 5472a8ce2670a34174d6d39f0d90faca8a7002ad
ms.sourcegitcommit: ab6fa92977255c5ecbe8a53cac61c2cd2a11601f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58292883"
---
# <a name="perform-advanced-json-transformations-with-liquid-templates-in-azure-logic-apps"></a>Geavanceerde JSON-transformaties met Liquid-sjablonen in Azure Logic Apps uitvoeren

U kunt eenvoudige JSON-transformaties uitvoeren in uw logische apps met de eigen gegevens bewerking acties zoals **opstellen** of **JSON parseren**. Voor het uitvoeren van geavanceerde JSON-transformaties, kunt u sjablonen of kaarten met [Liquid](https://shopify.github.io/liquid/), dit is een open-source-sjabloontaal voor flexibele web-apps. Een Liquid sjabloon definieert informatie over het transformeren van JSON-uitvoer en ondersteunt meer complexe JSON-transformaties, zoals iteraties, besturingselement stromen, variabelen, enzovoort. 

Voordat u een Liquid transformatie in uw logische app uitvoeren kunt, moet u eerst de JSON definiëren de toewijzing van de JSON met Liquid-sjabloon en store die zijn toegewezen in uw integratie-account. In dit artikel leest u hoe het maken en gebruiken van deze Liquid sjabloon of de kaart. 

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u geen abonnement hebt, kunt u [beginnen met een gratis Azure-account](https://azure.microsoft.com/free/). Of, [zich aanmelden voor een abonnement op gebruiksbasis](https://azure.microsoft.com/pricing/purchase-options/).

* Basiskennis over [over het maken van logische apps](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* Een eenvoudige [integratie-account](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md)

* Basiskennis over [Liquid sjabloontaal.](https://shopify.github.io/liquid/)

## <a name="create-liquid-template-or-map-for-your-integration-account"></a>Liquid sjabloon of de kaart voor uw integratie-account maken

1. In dit voorbeeld maakt de voorbeeldsjabloon vloeistof die worden beschreven in deze stap. In de Liquid sjabloon, kunt u [vloeistof filters](https://shopify.github.io/liquid/basics/introduction/#filters), welke gebruik [DotLiquid](https://dotliquidmarkup.org/) en C# naamconventies. 

   > [!NOTE]
   > Zorg ervoor dat de filterfunctie namen gebruiken *hetzelfde hoofdlettergebruik* in uw sjabloon. Anders wordt de filters werken niet.

   ```json
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

2. Meld u aan bij [Azure Portal](https://portal.azure.com). Selecteer in het hoofdmenu van Azure **alle resources**. In het zoekvak, zoek en selecteer uw integratie-account.

   ![Integratie-account selecteren](./media/logic-apps-enterprise-integration-liquid-transform/select-integration-account.png)

3.  Onder **onderdelen**, selecteer **Maps**.

    ![Selecteer maps](./media/logic-apps-enterprise-integration-liquid-transform/add-maps.png)

4. Kies **toevoegen** en geef de volgende gegevens voor de kaart:

   | Eigenschap | Waarde | Beschrijving | 
   |----------|-------|-------------|
   | **Naam** | JsonToJsonTemplate | De naam voor de kaart 'JsonToJsonTemplate' in dit voorbeeld is | 
   | **Toewijzingstype** | **Liquid** | Het type voor uw kaart. Voor JSON naar JSON-transformatie, selecteert u **liquid**. | 
   | **Kaart** | "SimpleJsonToJsonTemplate.liquid" | Een bestaand Liquid sjabloon of kaart bestand moet worden gebruikt voor transformatie 'SimpleJsonToJsonTemplate.liquid' in dit voorbeeld. Als u dit bestand zoekt, kunt u de bestandenkiezer. |
   ||| 

   ![Liquid sjabloon toevoegen](./media/logic-apps-enterprise-integration-liquid-transform/add-liquid-template.png)
    
## <a name="add-the-liquid-action-for-json-transformation"></a>De Liquid actie voor JSON-transformatie toevoegen

1. Volg deze stappen om in Azure portal, [maken van een lege, logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md).

2. In de Logic App Designer voegen de [aanvraagtrigger](../connectors/connectors-native-reqres.md#use-the-http-request-trigger) aan uw logische app.

3. Kies onder de trigger **nieuwe stap**. 
   Typ 'vloeistof' als filter in het zoekvak en selecteer deze actie: **JSON transformeren naar JSON - vloeistof**

   ![Zoek en selecteer Liquid actie](./media/logic-apps-enterprise-integration-liquid-transform/search-action-liquid.png)

4. Klik in de **inhoud** zodat de lijst met dynamische inhoud wordt weergegeven en selecteer de **hoofdtekst** token.
  
   ![Selecteer de hoofdtekst](./media/logic-apps-enterprise-integration-liquid-transform/select-body.png)
 
5. Uit de **kaart** , selecteert u uw Liquid sjabloon, waarmee "JsonToJsonTemplate" in dit voorbeeld.

   ![Kaart selecteren](./media/logic-apps-enterprise-integration-liquid-transform/select-map.png)

   Als de lijst met toewijzingen leeg is, is waarschijnlijk uw logische app niet gekoppeld aan uw integratie-account. 
   Als u wilt uw logische app een koppeling naar het integratieaccount met de Liquid sjabloon of de kaart, de volgende stappen uit:

   1. Selecteer op het menu van uw logische app, **Werkstroominstellingen**.

   2. Uit de **selecteert u een integratieaccount** lijst, selecteert u uw integratieaccount en kies **opslaan**.

      ![Logica-app voor het integratieaccount koppelen](./media/logic-apps-enterprise-integration-liquid-transform/link-integration-account.png)

## <a name="test-your-logic-app"></a>Uw logische app testen

JSON-invoer aan uw logische app van boeken [Postman](https://www.getpostman.com/postman) of een vergelijkbaar hulpprogramma. De getransformeerde JSON-uitvoer van uw logische app ziet eruit zoals in dit voorbeeld:
  
![Voorbeeld van uitvoer](./media/logic-apps-enterprise-integration-liquid-transform/example-output-jsontojson.png)

## <a name="more-liquid-action-examples"></a>Meer voorbeelden van Liquid actie
Vloeistof is niet beperkt tot alleen JSON-transformaties. Hier volgen andere acties beschikbaar transformatie die gebruikmaken van vloeistof.

* JSON transformeren naar tekst
  
  Hier volgt de Liquid sjabloon die wordt gebruikt voor dit voorbeeld:
   
   ``` json
   {{content.firstName | Append: ' ' | Append: content.lastName}}
   ```
   Hier vindt u van Voorbeeldinvoer en uitvoer:
  
   ![Van voorbeelduitvoer-JSON naar tekst](./media/logic-apps-enterprise-integration-liquid-transform/example-output-jsontotext.png)

* XML naar JSON transformeren
  
  Hier volgt de Liquid sjabloon die wordt gebruikt voor dit voorbeeld:
   
   ``` json
   [{% JSONArrayFor item in content -%}
        {{item}}
    {% endJSONArrayFor -%}]
   ```
   Hier vindt u van Voorbeeldinvoer en uitvoer:

   ![Voorbeeld van XML in JSON-uitvoer](./media/logic-apps-enterprise-integration-liquid-transform/example-output-xmltojson.png)

* XML transformeren naar tekst
  
  Hier volgt de Liquid sjabloon die wordt gebruikt voor dit voorbeeld:

   ``` json
   {{content.firstName | Append: ' ' | Append: content.lastName}}
   ```

   Hier vindt u van Voorbeeldinvoer en uitvoer:

   ![Voorbeeld van uitvoer XML-tekst](./media/logic-apps-enterprise-integration-liquid-transform/example-output-xmltotext.png)

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over het Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md "meer informatie over Enterprise Integration Pack")  
* [Meer informatie over maps](../logic-apps/logic-apps-enterprise-integration-maps.md "meer informatie over enterprise integration-kaarten")  

