---
title: REST-eindpunten aanroepen vanuit Azure Logic Apps | Microsoft Docs
description: Automatiseren van taken en werkstromen die met de REST-eindpunten communiceren met behulp van HTTP + Swagger-connector in Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, jehollan, LADocs
ms.assetid: eccfd87c-c5fe-4cf7-b564-9752775fd667
tags: connectors
ms.topic: article
ms.date: 07/18/2016
ms.openlocfilehash: bb89c76fb19bff74d39d919c8d2e65d430cb0566
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55817747"
---
# <a name="call-rest-endpoints-with-http--swagger-connector-in-azure-logic-apps"></a>Aanroepen van REST-eindpunten met HTTP + Swagger-connector in Azure Logic Apps

U kunt een eersteklas connector met een REST-eindpunt via een [Swagger-document](https://swagger.io) bij het gebruik van de HTTP + Swagger actie in uw werkstroom voor logische Apps. U kunt ook logische apps voor het aanroepen van een REST-eindpunt met een eersteklas ervaring voor de Logic App Designer uitbreiden.

Zie voor meer informatie over het maken van logische apps met connectors, [maken van een nieuwe logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="use-http--swagger-as-a-trigger-or-an-action"></a>Gebruik HTTP + Swagger als een trigger of een actie

De HTTP + Swagger activeren en actie werken op dezelfde manier als de [HTTP-actie](connectors-native-http.md) maar een betere ervaring in Logic App Designer bieden bij het blootstellen van de structuur van de API en de uitvoer van de [Swagger-metagegevens](https://swagger.io). U kunt ook de HTTP + Swagger-connector als een trigger. Als u wilt voor het implementeren van een polling-trigger, volgt u de polling-patroon dat wordt beschreven in [maken van aangepaste API's voor andere API's, services en systemen aanroepen vanuit logic apps](../logic-apps/logic-apps-create-api-app.md#polling-triggers).

Meer informatie over [logic app-triggers en acties](connectors-overview.md).

Hier volgt een voorbeeld van hoe u moet gebruiken de HTTP + Swagger-bewerkings-als een actie in een werkstroom in een logische app.

1. Selecteer de **nieuwe stap** knop.
2. Selecteer **een actie toevoegen**.
3. Typ in het zoekvak actie **swagger** aan de lijst met de HTTP + Swagger-actie.
   
    ![Selecteer HTTP + Swagger actie](./media/connectors-native-http-swagger/using-action-1.png)
4. Typ de URL voor een Swagger-document:
   
   * Als u wilt werken uit de ontwerper van logische App, moet de URL een HTTPS-eindpunt en CORS hebt ingeschakeld.
   * Als het Swagger-document niet aan deze vereiste voldoen, kunt u Azure Storage gebruiken met CORS ingeschakeld voor het opslaan van het document.
5. Klik op **volgende** worden gelezen en weergegeven in de Swagger-document.
6. Voeg parameters die vereist voor de HTTP-aanroep zijn.
   
    ![HTTP-actie](./media/connectors-native-http-swagger/using-action-2.png)
7. Als u wilt opslaan en publiceren van uw logische app, klikt u op **opslaan** op de werkbalk ontwerper.

### <a name="host-swagger-from-azure-storage"></a>Swagger van de host uit Azure Storage
Het is raadzaam om te verwijzen naar een Swagger-document dat niet wordt gehost of die niet voldoet aan de beveiligings- en cross-origin-vereisten voor de ontwerpfunctie. U lost dit probleem, kunt u het Swagger-document opslaan in Azure Storage en inschakelen van CORS om te verwijzen naar het document.  

Hier volgen de stappen voor het maken, configureren en Swagger documenten opslaan in Azure Storage:

1. [Een Azure storage-account maken met Azure Blob storage](../storage/common/storage-create-storage-account.md). Als u deze stap, stel de machtigingen op **openbare toegang**.

2. CORS inschakelen in de blob. 

   U kunt gebruiken om automatisch te configureren met deze instelling, [dit PowerShell-script](https://github.com/logicappsio/EnableCORSAzureBlob/blob/master/EnableCORSAzureBlob.ps1).

3. Het Swagger-bestand uploaden naar de blob. 

   U kunt uitvoeren in deze stap van de [Azure-portal](https://portal.azure.com) of van een hulpprogramma zoals [Azure Storage Explorer](http://storageexplorer.com/).

4. Verwijzen naar een HTTPS-koppeling naar het document in Azure Blob-opslag. 

   Deze indeling maakt gebruik van de koppeling:

   `https://*storageAccountName*.blob.core.windows.net/*container*/*filename*`

## <a name="technical-details"></a>Technische details
Hieronder vindt u de details voor de triggers en acties die deze HTTP + Swagger connector ondersteunt.

## <a name="http--swagger-triggers"></a>HTTP + Swagger-triggers
Een trigger is een gebeurtenis die kan worden gebruikt om de werkstroom die gedefinieerd in een logische app te starten. [Meer informatie over triggers.](connectors-overview.md) HTTP + Swagger-connector heeft een trigger.

| Trigger | Description |
| --- | --- |
| HTTP + Swagger |Voert u een HTTP-aanroep en keert de inhoud van de reactie |

## <a name="http--swagger-actions"></a>HTTP + Swagger-acties
Een actie is een bewerking die wordt uitgevoerd door de werkstroom die gedefinieerd in een logische app. [Meer informatie over acties.](connectors-overview.md) HTTP + Swagger-connector heeft een mogelijke actie.

| Bewerking | Description |
| --- | --- |
| HTTP + Swagger |Voert u een HTTP-aanroep en keert de inhoud van de reactie |

### <a name="action-details"></a>Actiedetails
HTTP + Swagger connector wordt geleverd met een mogelijke actie. Hieronder vindt u informatie over elk van de acties, de vereiste en optionele velden en de bijbehorende uitvoergegevens die gekoppeld aan hun gebruik zijn.

#### <a name="http--swagger"></a>HTTP + Swagger
Maakt een uitgaande HTTP-aanvraag met hulp van Swagger-metagegevens.
Een sterretje (*) betekent dat een vereist veld.

| Weergavenaam | Naam van eigenschap | Description |
| --- | --- | --- |
| Methode * |method |HTTP-woord te gebruiken. |
| URI * |uri |URI voor de HTTP-aanvraag. |
| Headers |Headers |Een JSON-object van HTTP-headers om op te nemen. |
| Hoofdtekst |hoofdtekst |De hoofdtekst van de HTTP-aanvraag. |
| Authentication |verificatie |Verificatietype moet worden gebruikt voor de aanvraag. Zie voor meer informatie de [HTTP-connector](connectors-native-http.md#authentication). |

**Uitvoergegevens**

HTTP-antwoord

| Naam van eigenschap | Gegevenstype | Description |
| --- | --- | --- |
| Headers |object |Antwoordheaders |
| Hoofdtekst |object |Responsobject |
| Statuscode |int |HTTP-statuscode |

### <a name="http-responses"></a>HTTP-antwoorden
U kunt bepaalde antwoorden krijgen bij het maken van aanroepen naar verschillende acties. Hieronder volgt een tabel waarin de bijbehorende antwoorden en beschrijvingen.

| Name | Description |
| --- | --- |
| 200 |OK |
| 202 |Geaccepteerd |
| 400 |Ongeldig verzoek |
| 401 |Niet geautoriseerd |
| 403 |Verboden |
| 404 |Niet gevonden |
| 500 |Er is een interne serverfout opgetreden. Er is een onbekende fout opgetreden. |

- - -
## <a name="next-steps"></a>Volgende stappen

* [Een logische app maken](../logic-apps/quickstart-create-first-logic-app-workflow.md)
* [Andere connectors zoeken](apis-list.md)
