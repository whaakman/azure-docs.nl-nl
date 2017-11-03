---
title: Aangepaste code voor Azure Logic Apps met Azure Functions | Microsoft Docs
description: Maken en uitvoeren van aangepaste code voor Azure Logic Apps met Azure Functions
services: logic-apps,functions
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: 9fab1050-cfbc-4a8b-b1b3-5531bee92856
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.custom: H1Hack27Feb2017
ms.date: 10/18/2016
ms.author: LADocs; jehollan
ms.openlocfilehash: 18442c87b049200fac5ed41cc7034ba7a848b8d3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="add-and-run-custom-code-for-logic-apps-through-azure-functions"></a>Toevoegen en het uitvoeren van aangepaste code voor logic apps via Azure Functions

Als u wilt uitvoeren aangepaste codefragmenten van C# of node.js in logic apps, kunt u aangepaste functies via Azure Functions. 
[Azure Functions](../azure-functions/functions-overview.md) biedt server gratis computergebruik in Microsoft Azure en zijn handig voor het uitvoeren van deze taken:

* Geavanceerde opmaak of compute van velden in logic apps
* Berekeningen in een werkstroom.
* De logic app-functionaliteit met functies die worden ondersteund in C# of node.js uitbreiden

## <a name="create-custom-functions-for-your-logic-apps"></a>Aangepaste functies voor uw logische apps maken

Het is raadzaam dat u een functie in de Azure Functions-portal vanuit maken de **Generic Webhook - knooppunt** of **Generic Webhook - C#** sjablonen. Het resultaat maakt een sjabloon die accepteert een door het automatisch ingevuld `application/json` van een logische app. Functies die u van deze sjablonen maakt worden automatisch gedetecteerd en worden weergegeven in de ontwerpfunctie Logic App onder **Azure Functions in mijn regio.**

In de Azure-portal op de **integreren** deelvenster voor de functie, de sjabloon moet worden weergegeven die **modus** ingesteld op **Webhook** en **Webhook type** is ingesteld op **algemene JSON**. 

Webhook functies een aanvraag accepteren en doorgegeven aan de methode via een `data` variabele. U kunt toegang tot de eigenschappen van uw payload met puntnotatie zoals `data.function-name`. Bijvoorbeeld, lijkt een eenvoudige JavaScript-functie die een DateTime-waarde naar een datumtekenreeks converteert op het volgende voorbeeld:

```
function start(req, res){
    var data = req.body;
    res = {
        body: data.date.ToDateString();
    }
}
```

## <a name="call-azure-functions-from-logic-apps"></a>Azure-functies aanroepen vanuit logic apps

Lijst van de containers in uw abonnement en selecteert u de functie die u aanroepen wilt, in Logic App-ontwerper, klikt u op de **acties** menu en selecteer een van de **Azure Functions in mijn regio**.

Nadat u de functie selecteert, wordt u gevraagd om op te geven van een object van de nettolading van de invoer. Dit object is het bericht dat de logische app naar de functie verzendt en een JSON-object moet. Bijvoorbeeld, als u wilt doorgeven de **laatst gewijzigd** datum na een trigger Salesforce de nettolading van de functie als volgt uitzien in dit voorbeeld:

![Datum van laatste wijziging][1]

## <a name="trigger-logic-apps-from-a-function"></a>Trigger logische apps van een functie

U kunt een logische app van binnen een functie activeren. Zie [Logic apps als aanroepbare eindpunten](logic-apps-http-endpoint.md). Een logische app met een handmatige trigger maken en klik vervolgens in de functie genereert een HTTP POST naar de handmatige trigger-URL met de nettolading van de gewenste naar de logische app verzonden.

### <a name="create-a-function-from-logic-app-designer"></a>Een functie maken vanuit Logic App-ontwerper

U kunt ook een node.js-webhook-functie maken vanuit de designer. Selecteer eerst **Azure Functions in mijn regio** en kies vervolgens een container voor uw functie. Als u nog een container hebt, moet u maken vanuit de [Azure Functions-portal](https://functions.azure.com/signin). Selecteer vervolgens **nieuw**.  

Geef het contextobject dat u van plan bent om door te geven in een functie voor het genereren van een sjabloon op basis van de gegevens die u wilt berekenen. Dit object moet een JSON-object. Bijvoorbeeld, als u in inhoud van het bestand van een FTP-actie doorgeeft, de nettolading van de context ziet eruit als in dit voorbeeld:

![De nettolading van de context][2]

> [!NOTE]
> Omdat dit object is niet geconverteerd als een tekenreeks, wordt de inhoud rechtstreeks naar de JSON-nettolading toegevoegd. Echter, treedt een fout als het object geen JSON-token is (dat wil zeggen, een tekenreeks of een JSON-object/array). Toevoegen als u wilt Converteer het object als een tekenreeks, aanhalingstekens in de eerste illustratie in dit artikel.
> 

De ontwerpfunctie genereert vervolgens een functie-sjabloon dat u inline kunt maken. Variabelen zijn vooraf gemaakt op basis van de context die u wilt doorgeven aan de functie.

<!--Image references-->
[1]: ./media/logic-apps-azure-functions/callfunction.png
[2]: ./media/logic-apps-azure-functions/createfunction.png
