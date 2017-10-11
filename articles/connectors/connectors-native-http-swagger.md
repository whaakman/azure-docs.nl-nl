---
title: Aanroepen van REST-eindpunten met HTTP- en Swagger-connector voor Azure Logic Apps | Microsoft Docs
description: Verbinding maken met de REST-eindpunten vanuit logic apps via Swagger met de HTTP- + Swagger connector
services: logic-apps
author: jeffhollan
manager: anneta
editor: 
documentationcenter: 
tags: connectors
ms.assetid: eccfd87c-c5fe-4cf7-b564-9752775fd667
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2016
ms.author: jehollan; LADocs
ms.openlocfilehash: 3e9229d94e96aad7b769d0e55d208d856e3b80bc
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="get-started-with-the-http--swagger-action"></a>Aan de slag met de HTTP- + Swagger actie

U kunt geen klas connector maken voor een willekeurig eindpunt REST via een [Swagger-document](https://swagger.io) bij het gebruik van de HTTP- + Swagger actie in de werkstroom van uw logische app. U kunt logische apps voor het aanroepen van een REST-eindpunt met een uitstekende Logic App-ontwerper ervaring ook uitbreiden.

Zie voor meer informatie over het maken van logische apps met connectors, [maken van een nieuwe logische app](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="use-http--swagger-as-a-trigger-or-an-action"></a>Gebruik HTTP + Swagger als een trigger of een actie

De HTTP- + de Swagger activeren en de actie werken op dezelfde manier als de [HTTP-actie](connectors-native-http.md) maar bieden een betere ervaring in Logic App-ontwerper bij het blootstellen van de structuur van de API en de uitvoer van de [Swagger-metagegevens](https://swagger.io). U kunt ook HTTP gebruiken + Swagger connector als een trigger. Als u implementeren, een polling-trigger wilt, volgt u het polling-patroon dat wordt beschreven in [maken van aangepaste API's om aan te roepen andere API's, services en systemen van logic apps](../logic-apps/logic-apps-create-api-app.md#polling-triggers).

Meer informatie over [logic app triggers en acties](connectors-overview.md).

Hier volgt een voorbeeld van het gebruik van de HTTP-+ Swagger-bewerkings-als een actie in een werkstroom in een logische app.

1. Selecteer de **nieuwe stap** knop.
2. Selecteer **een actie toevoegen**.
3. Typ in het zoekvak actie **swagger** aan de lijst het HTTP-Swagger-actie.
   
    ![Selecteer HTTP + Swagger actie](./media/connectors-native-http-swagger/using-action-1.png)
4. Typ de URL voor een Swagger-document:
   
   * Om te werken vanuit Logic App Designer, moet de URL een HTTPS-eindpunt en CORS is ingeschakeld.
   * Als de Swagger-document niet aan deze vereiste voldoen, kunt u [Azure Storage met CORS ingeschakeld](#hosting-swagger-from-storage) voor het opslaan van het document.
5. Klik op **volgende** om te lezen en weergeven van de Swagger-document.
6. Voeg in de parameters die vereist voor de HTTP-aanroep zijn.
   
    ![HTTP-bewerking voltooien](./media/connectors-native-http-swagger/using-action-2.png)
7. Als u wilt opslaan en publiceren van uw logische app, klikt u op **opslaan** op designer werkbalk.

### <a name="host-swagger-from-azure-storage"></a>Host Swagger uit Azure Storage
Het is raadzaam om te verwijzen naar een Swagger-document dat niet wordt gehost of die niet voldoen aan de beveiliging en cross-origin-vereisten voor de designer. U lost dit probleem, kunt u de Swagger-document opslaan in Azure Storage en inschakelen van CORS om te verwijzen naar het document.  

Hier volgen de stappen voor het maken, configureren en opslaan van documenten Swagger in Azure Storage:

1. [Een Azure storage-account maken met Azure Blob storage](../storage/common/storage-create-storage-account.md). Als u deze stap, stel de machtigingen op **openbare toegang**.

2. CORS inschakelen in de blob. 

   U kunt gebruiken om automatisch te configureren met deze instelling, [dit PowerShell-script](https://github.com/logicappsio/EnableCORSAzureBlob/blob/master/EnableCORSAzureBlob.ps1).

3. Het Swagger-bestand uploaden naar de blob. 

   U kunt uitvoeren in deze stap van de [Azure-portal](https://portal.azure.com) of vanuit een hulpprogramma zoals [Azure Opslagverkenner](http://storageexplorer.com/).

4. Verwijzen naar een HTTPS-koppeling in het document in Azure Blob-opslag. 

   De koppeling maakt gebruik van deze indeling:

   `https://*storageAccountName*.blob.core.windows.net/*container*/*filename*`

## <a name="technical-details"></a>Technische details
Hieronder vindt u de details voor de triggers en acties die in het volgende HTTP- + Swagger connector ondersteunt.

## <a name="http--swagger-triggers"></a>HTTP- + Swagger-triggers
Een trigger is een gebeurtenis die kan worden gebruikt om de werkstroom die gedefinieerd in een logische app te starten. [Meer informatie over triggers.](connectors-overview.md) De HTTP- + Swagger connector heeft een trigger.

| Trigger | Beschrijving |
| --- | --- |
| HTTP- + Swagger |Maken van een HTTP-aanroep en de antwoordinhoud wordt geretourneerd |

## <a name="http--swagger-actions"></a>HTTP- + Swagger-acties
Een actie is een bewerking die wordt uitgevoerd door de werkstroom die gedefinieerd in een logische app. [Meer informatie over acties.](connectors-overview.md) De HTTP- + Swagger connector heeft een mogelijke actie.

| Actie | Beschrijving |
| --- | --- |
| HTTP- + Swagger |Maken van een HTTP-aanroep en de antwoordinhoud wordt geretourneerd |

### <a name="action-details"></a>Actiedetails
De HTTP- + Swagger connector wordt geleverd met een mogelijke actie. Hieronder vindt u informatie over elk van de acties, hun vereiste en optionele invoervelden en de bijbehorende uitvoerdetails die gekoppeld aan hun gebruik zijn.

#### <a name="http--swagger"></a>HTTP- + Swagger
Controleer een uitgaande HTTP-aanvraag met hulp van Swagger-metagegevens.
Een sterretje (*) betekent een verplicht veld.

| Weergavenaam | De naam van eigenschap | Beschrijving |
| --- | --- | --- |
| Methode * |Methode |HTTP-term moet worden gebruikt. |
| URI * |URI |De URI voor de HTTP-aanvraag. |
| Headers |Headers |Een JSON-object van het HTTP-headers te nemen. |
| Hoofdtekst |Hoofdtekst |De hoofdtekst van de HTTP-aanvraag. |
| Authentication |Verificatie |De verificatie moet worden gebruikt voor de aanvraag. Zie voor meer informatie de [HTTP connector](connectors-native-http.md#authentication). |

**Uitvoerdetails**

HTTP-antwoord

| De naam van eigenschap | Gegevenstype | Beschrijving |
| --- | --- | --- |
| Headers |object |Antwoordheaders |
| Hoofdtekst |object |Response-object |
| Statuscode |int |HTTP-statuscode |

### <a name="http-responses"></a>HTTP-antwoorden
Bij het aanroepen op diverse acties, krijgt u mogelijk bepaalde antwoorden. Hier volgt een tabel staat aangegeven van de bijbehorende antwoorden en beschrijvingen.

| Naam | Beschrijving |
| --- | --- |
| 200 |OK |
| 202 |Geaccepteerd |
| 400 |Onjuiste aanvraag |
| 401 |Niet geautoriseerd |
| 403 |Is niet toegestaan |
| 404 |Niet gevonden |
| 500 |Interne serverfout. Er is een onbekende fout opgetreden. |

- - -
## <a name="next-steps"></a>Volgende stappen

* [Een logische app maken](../logic-apps/logic-apps-create-a-logic-app.md)
* [Andere connectors zoeken](apis-list.md)