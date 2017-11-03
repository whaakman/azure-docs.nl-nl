---
title: OpenAPI uitbreidingen voor aangepaste connectors - Azure Logic Apps | Microsoft Docs
description: Uw aangepaste connector OpenAPI functionaliteit uitbreiden
author: ecfan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.workload: logic-apps
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/22/2017
ms.author: LADocs; estfan
ms.openlocfilehash: e8565019bd4631043485224c73c2fe60e633951c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="extend-your-custom-connectors-openapi-with-advanced-functionality"></a>Uw aangepaste connector OpenAPI functionaliteit uitbreiden

Als u aangepaste connectors voor Azure Logic Apps, Microsoft Flow of Microsoft PowerApps maakt, moet u een definitiebestand OpenAPI die een taal networkdirect machineleesbare document met een beschrijving van uw API bewerkingen en parameters opgeven. U kunt ook deze uitbreidingen OpenAPI opnemen samen met de OpenAPI out-of-the-box-functionaliteit, wanneer u aangepaste connectors voor Logic Apps en stroom maken:

* [`summary`](#summary)
* [`x-ms-summary`](#x-ms-summary)
* [`description`](#description)
* [`x-ms-visibility`](#x-ms-visibility)
* [`x-ms-dynamic-values`](#x-ms-dynamic-values)
* [`x-ms-dynamic-schema`](#x-ms-dynamic-schema)

Hier vindt u meer informatie over deze uitbreidingen:

<a name="summary"></a>

## <a name="summary"></a>Samenvatting

Hiermee geeft u de titel voor de actie (bewerking). </br>
Van toepassing op: Operations </br>
Aanbevolen: Gebruik *zin* voor `summary`. </br>
Voorbeeld: "wanneer een gebeurtenis wordt toegevoegd aan agenda ' of 'E-mailbericht verzenden'

!['Samenvatting' voor elke bewerking](./media/custom-connector-openapi-extensions/summary.png)

``` json
"actions" {
  "Send_an_email": {
    /// Other action properties here...
    "summary": "Send an email",
    /// Other action properties here...
  }
},
```

<a name="x-ms-summary"></a>

## <a name="x-ms-summary"></a>x-ms-overzicht

Hiermee geeft u de titel voor een entiteit. </br>
Van toepassing op: Parameters, antwoordschema </br>
Aanbevolen: Gebruik *title geval* voor `x-ms-summary`. </br>
Voorbeeld: 'Agenda-ID', 'Onderwerp', 'Beschrijving van gebeurtenis', enzovoort

!["x-ms-overzicht' voor elke entiteit](./media/custom-connector-openapi-extensions/x-ms-summary.png)

``` json
"actions" {
  "Send_an_email": {
    /// Other action properties here...
    "parameters": [ 
      {
        /// Other parameters here...
        "x-ms-summary": "Subject",
        /// Other parameters here...
      }
    ]
  }
},
```

<a name="description"></a>

## <a name="description"></a>description

Hiermee geeft u een uitgebreide uitleg over de functionaliteit van de bewerking of indeling en functie van een entiteit. </br>
Van toepassing op: bewerkingen, Parameters, antwoordschema </br>
Aanbevolen: Gebruik *zin* voor `description`. </br>
Voorbeeld: 'deze bewerking wordt geactiveerd wanneer een nieuwe gebeurtenis wordt toegevoegd aan de agenda', 'Geeft u het onderwerp van het e-mailbericht', enzovoort

!['beschrijving' voor elke bewerking of de entiteit](./media/custom-connector-openapi-extensions/description.png)

``` json
"actions" {
  "Send_an_email": {
     "description": "Specify the subject of the mail",
     /// Other action properties here...
  }
},
```

<a name="visibility"></a>

## <a name="x-ms-visibility"></a>x-ms-zichtbaarheid

Hiermee wordt de zichtbaarheid van de gebruiker gerichte voor een entiteit. </br>
Mogelijke waarden: `important`, `advanced`, en`internal` </br>
Van toepassing op: bewerkingen, Parameters, schema's

* `important`bewerkingen en parameters worden altijd weergegeven op de gebruiker eerst.
* `advanced`bewerkingen en parameters zijn verborgen in een menu Extra.
* `internal`bewerkingen en parameters zijn verborgen voor de gebruiker.

> [!NOTE] 
> Voor parameters die zijn `internal` en `required`, u **moet** leveren van standaardwaarden voor deze parameters.

Voorbeeld: De **Zie voor meer informatie** menu en **geavanceerde opties weergeven** menu verborgen `advanced` bewerkingen en parameters.

!["x-ms-zichtbaarheid' voor het weergeven of verbergen bewerkingen en parameters](./media/custom-connector-openapi-extensions/x-ms-visibility.png)

``` json
"actions" {
  "Send_an_email": {
     /// Other action properties here...
     "parameters": [
         {
           "name": "Subject",
           "type": "string",
           "description": "Specify the subject of the mail",
           "x-ms-summary": "Subject",
           "x-ms-visibility": "important",
           /// Other parameter properties here...
         }
     ]
     /// Other action properties here...
  }
},
```

<a name="x-ms-dynamic-values"></a>

## <a name="x-ms-dynamic-values"></a>x-ms-dll-waarden

Geeft een ingevulde lijst voor de gebruiker zodat ze invoerparameters voor een bewerking kunnen selecteren. </br>
Van toepassing op: Parameters </br>
Het gebruik van: Voeg de `x-ms-dynamic-values` object voor de parameter-definitie. Bijvoorbeeld, ziet u dit [OpenAPI voorbeeld](https://procsi.blob.core.windows.net/blog-images/sampleDynamicSwagger.json).

!["x-ms-dll-waarden' voor het weergeven van lijsten](./media/custom-connector-openapi-extensions/x-ms-dynamic-values.png)

### <a name="properties-for-x-ms-dynamic-values"></a>Eigenschappen voor de x-ms-dll-waarden

| Naam | Vereist of optioneel | Beschrijving | 
| ---- | -------------------- | ----------- | 
| **operationID** | Vereist | De bewerking aan te roepen voor het invullen van de lijst | 
| **waarde pad** | Vereist | Een tekenreeks in het object binnen `value-collection` die verwijst naar de waarde van parameter. </br>Als `value-collection` is niet opgegeven, wordt het antwoord wordt geëvalueerd als een matrix. | 
| **titel** | Optioneel | Een tekenreeks in het object binnen `value-collection` die verwijst naar de waarde beschrijving. </br>Als `value-collection` is niet opgegeven, wordt het antwoord wordt geëvalueerd als een matrix. | 
| **waarde-verzameling** | Optioneel | Een tekenreeks die in een matrix met objecten in de nettolading van de reactie resulteert | 
| **parameters** | Optioneel | Een object waarvan de eigenschappen de invoerparameters om op te roepen, een bewerking voor dynamische-waarden opgeven | 
|||| 

Hier volgt een voorbeeld waarin de eigenschappen in `x-ms-dynamic-values`:

``` json
"x-ms-dynamic-values": {
  "operationId": "PopulateDropdown",
  "value-path": "name",
  "value-title": "properties/displayName",
  "value-collection": "value",
  "parameters": {
     "staticParameter": "<value>",
     "dynamicParameter": {
        "parameter": "<value-to-pass-to-dynamicParameter>"
     }
  }
}
```

## <a name="example-all-the-openapi-extensions-up-to-this-point"></a>Voorbeeld: Alle OpenAPI uitbreidingen naar dit punt

``` json
"/api/lists/{listID-dynamic}": {
    "get": {
        "description": "Get items from a single list - uses dynamic values and outputs dynamic schema",
        "summary": "Gets items from the selected list",
        "operationID": "GetListItems",
        "parameters": [
           {
             "name": "listID-dynamic",
             "type": "string",
             "in": "path",
             "description": "Select the list from where you want outputs",
             "required": true,
             "x-ms-summary": "Select List",
             "x-ms-dynamic-values": {
                "operationID": "GetLists",
                "value-path": "id",
                "value-title": "name"
             }
           }
        ]
    }
}
```

<a name="x-ms-dynamic schema"></a>

## <a name="x-ms-dynamic-schema"></a>x-ms-dll-schema

Geeft aan dat het schema voor de huidige parameter of reactie dynamische. Dit object kunt aanroepen van een bewerking die gedefinieerd door de waarde van dit veld, dynamisch detecteren van het schema en de juiste gebruikersinterface voor het verzamelen van gebruikersinvoer weergeven of beschikbare velden weergeven. 

Van toepassing op: Parameters, -antwoord

Het gebruik van: Voeg de `x-ms-dynamic-schema` -object op voor een definitie van aanvraag parameter of antwoord hoofdtekst. Zie voor een voorbeeld [OpenAPI voorbeeld](https://procsi.blob.core.windows.net/blog-images/sampleDynamicSwagger.json).

In dit voorbeeld ziet u hoe de invoer vorm verandert, op basis van het item dat de gebruiker in de vervolgkeuzelijst selecteert:

!['x-ms-dll-schema' voor dynamische parameters](./media/custom-connector-openapi-extensions/x-ms-dynamic-schema.png)

En dit voorbeeld ziet u hoe de uitvoer wijzigen, op basis van het item dat de gebruiker in de vervolgkeuzelijst selecteert. In deze versie van de gebruiker 'Auto':

!["x-ms-dll-schema-antwoord" voor het geselecteerde item 'Auto'](./media/custom-connector-openapi-extensions/x-ms-dynamic-schema-output1.png)

In deze versie van de gebruiker 'Voeding':

!["x-ms-dll-schema-antwoord" voor het geselecteerde item 'Voeding'](./media/custom-connector-openapi-extensions/x-ms-dynamic-schema-output2.png)

### <a name="properties-for-x-ms-dynamic-schema"></a>Eigenschappen voor de x-ms-dll-schema

| Naam | Vereist of optioneel | Beschrijving | 
| ---- | -------------------- | ----------- | 
| **operationID** | Vereist | De bewerking aan te roepen voor het ophalen van het schema | 
| **parameters** | Vereist | Een object waarvan de eigenschappen de invoerparameters vereist opgeven voor het aanroepen van een dynamische-schema-bewerking | 
| **waarde pad** |Optioneel | Een padtekenreeks die verwijst naar de eigenschap met het schema. </br>Als niet wordt opgegeven, het antwoord wordt aangenomen dat het schema in het hoofdobject eigenschappen bevatten. | 
|||| 

Hier volgt een voorbeeld van een dynamische parameter:

``` json
{
  "name": "dynamicListSchema",
  "in": "body",
  "description": "Dynamic schema for items in the selected list",
  "schema": {
    "type": "object",
    "x-ms-dynamic-schema": {
        "operationID": "GetListSchema",
        "parameters": {
          "listID": {
            "parameter": "listID-dynamic"
          }
        },
        "value-path": "items"
    }
  }
}
```

Hier volgt een voorbeeld van een dynamische reactie:

``` json
"DynamicResponseGetListSchema": {
   "type": "object",
   "x-ms-dynamic-schema": {
      "operationID": "GetListSchema",
      "parameters": {
         "listID": {
            "parameter": "listID-dynamic"
         }
      },
      "value-path": "items"
    }
}
```

## <a name="next-steps"></a>Volgende stappen

* [Beschrijf uw aangepaste API's en -connectors](../logic-apps/custom-connector-api-postman-collection.md)
* [Logic Apps: Registreer uw connector](../logic-apps/logic-apps-custom-connector-register.md)
* [Stroom: Registreer uw connector](https://ms.flow.microsoft.com/documentation/register-custom-api/#register-your-custom-connector)