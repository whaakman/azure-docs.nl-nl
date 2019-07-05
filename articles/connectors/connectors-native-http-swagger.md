---
title: Verbinding maken met REST-eindpunten van Azure Logic Apps
description: REST-eindpunten in geautomatiseerde taken, processen en werkstromen met behulp van Azure Logic Apps bewaken
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: conceptual
ms.date: 07/05/2019
tags: connectors
ms.openlocfilehash: f0410ed7a98e4838e41407868cf26b5254811ae3
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2019
ms.locfileid: "67541577"
---
# <a name="call-rest-endpoints-by-using-azure-logic-apps"></a>REST-eindpunten worden aangeroepen met behulp van Azure Logic Apps

Met [Azure Logic Apps](../logic-apps/logic-apps-overview.md) en de ingebouwde HTTP + Swagger-connector, kunt u werkstromen die regelmatig via een REST-eindpunt aanroepen automatiseren een [Swagger-bestand](https://swagger.io) door logic apps te bouwen. De HTTP + Swagger activeren en actie werken op dezelfde manier als de [HTTP-trigger en actie](connectors-native-http.md) maar een betere ervaring in de Logic App Designer bieden bij het blootstellen van de structuur van de API en de uitvoer wordt beschreven in het Swagger-bestand. Voor het implementeren van een polling-trigger, gaat u als volgt het polling-patroon dat wordt beschreven in [maken van aangepaste API's voor andere API's, services en systemen aanroepen vanuit logic apps](../logic-apps/logic-apps-create-api-app.md#polling-triggers).

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u nog geen abonnement op Azure hebt, [registreer u dan nu voor een gratis Azure-account](https://azure.microsoft.com/free/).

* De URL voor het Swagger-bestand waarin het doel REST-eindpunt

  Normaal gesproken de REST-eindpunt moet voldoen aan deze criteria voor de connector om te werken:

  * Het Swagger-bestand moet worden gehost op een HTTPS-URL openbaar toegankelijk is.

  * Het Swagger-bestand moet hebben [Cross-Origin Resource Sharing (CORS)](https://docs.microsoft.com/rest/api/storageservices/cross-origin-resource-sharing--cors--support-for-the-azure-storage-services) ingeschakeld.

  Als u wilt verwijzen naar een Swagger-bestand dat niet wordt gehost of die niet voldoet aan de beveiligings- en cross-origin-vereisten, kunt u [het Swagger-bestand uploaden naar een blobcontainer in Azure storage-account](#host-swagger), en dus CORS inschakelen in dit storage-account dat kunt u verwijzen naar het bestand.

  De voorbeelden in dit onderwerp gebruiken de [Cognitive Services Face-API](https://docs.microsoft.com/azure/cognitive-services/face/overview), waarvoor een [Cognitive Services-account en de toegangssleutel](../cognitive-services/cognitive-services-apis-create-account.md).

* Basiskennis over [over het maken van logische apps](../logic-apps/quickstart-create-first-logic-app-workflow.md). Als u geen ervaring met logische apps, raadpleegt u [wat is Azure Logic Apps?](../logic-apps/logic-apps-overview.md)

* De logische app van waar u aan te roepen van het doel-eindpunt. Beginnen met de HTTP + Swagger activeert, [maken van een lege, logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md). Om te gebruiken de HTTP + Swagger-actie beginnen uw logische app met een trigger die u wilt. In dit voorbeeld wordt de HTTP + Swagger-trigger als de eerste stap.

## <a name="add-an-http--swagger-trigger"></a>Toevoegen van een HTTP + Swagger trigger

Deze ingebouwde trigger stuurt een HTTP-aanvraag naar een URL voor een Swagger-bestand dat u een beschrijving van een REST-API en retourneert een antwoord dat de inhoud van het bestand bevat.

1. Meld u aan bij [Azure Portal](https://portal.azure.com). Open uw lege, logische app in Logic App Designer.

1. De ontwerpfunctie in het zoekvak, typ 'swagger' als filter. Uit de **Triggers** in de lijst met de **HTTP + Swagger** trigger.

   ![Selecteer HTTP + Swagger activeren](./media/connectors-native-http-swagger/select-http-swagger-trigger.png)

1. In de **SWAGGER-EINDPUNT-URL** vak, voer de URL voor het Swagger-bestand en selecteer **volgende**.

   In dit voorbeeld wordt de Swagger-URL die zich in de regio VS-West voor de [Cognitive Services Face-API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236):

   `https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/export?DocumentFormat=Swagger&ApiName=Face%20API%20-%20V1.0`

   ![Voer de URL voor Swagger-eindpunt](./media/connectors-native-http-swagger/http-swagger-trigger-parameters.png)

1. Wanneer de ontwerpfunctie de bewerkingen die worden beschreven door de Swagger-bestand geeft, selecteert u de bewerking die u wilt gebruiken.

   ![Bewerkingen in de Swagger-bestand](./media/connectors-native-http-swagger/http-swagger-trigger-operations.png)

1. Geef de waarden voor de trigger parameters, die variëren op basis van de geselecteerde bewerking, die u wilt opnemen in de aanroep van het eindpunt. Het terugkeerpatroon voor hoe vaak u wilt dat de trigger instellen om aan te roepen van het eindpunt.

   In dit voorbeeld wijzigt de naam van de trigger "HTTP + Swagger activeren: Face - detecteren' zodat de stap een meer beschrijvende naam heeft.

   ![Bewerkingsdetails](./media/connectors-native-http-swagger/http-swagger-trigger-operation-details.png)

1. Als u wilt toevoegen van andere beschikbare parameters, opent u de **toevoegen van nieuwe parameter** lijst en selecteer de parameters die u wilt.

   Zie voor meer informatie over verificatie die beschikbaar zijn voor HTTP + Swagger [verifiëren HTTP-triggers en acties](../logic-apps/logic-apps-workflow-actions-triggers.md#connector-authentication).

1. Doorgaan met het ontwikkelen van uw logische app-werkstroom met acties die worden uitgevoerd wanneer de trigger wordt geactiveerd.

1. Wanneer u klaar bent, moet u uw logische app opslaan. Selecteer op de werkbalk van de ontwerper **opslaan**.

## <a name="add-an-http--swagger-action"></a>Toevoegen van een HTTP + Swagger actie

Deze ingebouwde actie maakt een HTTP-aanvraag naar de URL voor het Swagger-bestand dat u een beschrijving van een REST-API en retourneert een antwoord dat de inhoud van het bestand bevat.

1. Meld u aan bij [Azure Portal](https://portal.azure.com). Open uw logische app in Logic App Designer.

1. Onder de stap waarin u wilt toevoegen van de HTTP + Swagger actie, selecteer **nieuwe stap**.

   Als u wilt toevoegen een actie tussen fasen, de aanwijzer over de pijl tussen fasen. Selecteer het plusteken ( **+** ) die wordt weergegeven, en selecteer vervolgens **een actie toevoegen**.

1. De ontwerpfunctie in het zoekvak, typ 'swagger' als filter. Uit de **acties** in de lijst met de **HTTP + Swagger** actie.

    ![Selecteer HTTP + Swagger actie](./media/connectors-native-http-swagger/select-http-swagger-action.png)

1. In de **SWAGGER-EINDPUNT-URL** vak, voer de URL voor het Swagger-bestand en selecteer **volgende**.

   In dit voorbeeld wordt de Swagger-URL die zich in de regio VS-West voor de [Cognitive Services Face-API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236):

   `https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/export?DocumentFormat=Swagger&ApiName=Face%20API%20-%20V1.0`

   ![Voer de URL voor Swagger-eindpunt](./media/connectors-native-http-swagger/http-swagger-action-parameters.png)

1. Wanneer de ontwerpfunctie de bewerkingen die worden beschreven door de Swagger-bestand geeft, selecteert u de bewerking die u wilt gebruiken.

   ![Bewerkingen in de Swagger-bestand](./media/connectors-native-http-swagger/http-swagger-action-operations.png)

1. Geef de waarden voor de actieparameters, die variëren op basis van de geselecteerde bewerking, die u wilt opnemen in de aanroep van het eindpunt.

   In dit voorbeeld heeft geen parameters, maar wijzigt de naam van de actie ' HTTP + Swagger actie: Face - identificeren"zodat de stap een meer beschrijvende naam heeft.

   ![Bewerkingsdetails](./media/connectors-native-http-swagger/http-swagger-action-operation-details.png)

1. Als u wilt toevoegen van andere beschikbare parameters, opent u de **toevoegen van nieuwe parameter** lijst en selecteer de parameters die u wilt.

   Zie voor meer informatie over verificatie die beschikbaar zijn voor HTTP + Swagger [verifiëren HTTP-triggers en acties](../logic-apps/logic-apps-workflow-actions-triggers.md#connector-authentication).

1. Wanneer u klaar bent, moet u uw logische app opslaan. Selecteer op de werkbalk van de ontwerper **opslaan**.

<a name="host-swagger"></a>

## <a name="host-swagger-in-azure-storage"></a>Host-Swagger in Azure Storage

U kunt verwijzen naar een Swagger-bestand dat niet wordt gehost of die niet voldoen aan de cross-origin-vereisten voor beveiliging en van dat bestand uploaden naar blob-container in Azure storage-account en het inschakelen van CORS voor dat opslagaccount. Als u wilt maken, instellen en opslaan van bestanden van Swagger in Azure Storage, de volgende stappen uit:

1. [Maak een Azure Storage-account](../storage/common/storage-create-storage-account.md).

1. Nu moet u CORS inschakelen voor de blob. Selecteer in het menu van uw opslagaccount **CORS**. Op de **Blob-service** tabblad, geef deze waarden en selecteer vervolgens **opslaan**.

   | Eigenschap | Value |
   |----------|-------|
   | **Toegestane oorsprongen** | `*` |
   | **Toegestane methoden** | `GET`, `HEAD`, `PUT` |
   | **Toegestane kopteksten** | `*` |
   | **Beschikbaar gemaakte kopteksten** | `*` |
   | **Maximumleeftijd** (in seconden) | `200` |
   |||

   Hoewel dit voorbeeld wordt de [Azure-portal](https://portal.azure.com), kunt u een hulpprogramma zoals [Azure Storage Explorer](https://storageexplorer.com/), of automatisch deze instelling configureren met behulp van dit voorbeeld [PowerShell-script](https://github.com/logicappsio/EnableCORSAzureBlob/blob/master/EnableCORSAzureBlob.ps1).

1. [Maak een blobcontainer](../storage/blobs/storage-quickstart-blobs-portal.md). Op van de container **overzicht** venster **toegangsniveau wijzigen**. Uit de **openbaar toegangsniveau** in de lijst met **Blob (anonieme leestoegang voor alleen blobs)** , en selecteer **OK**.

1. [Het Swagger-bestand uploaden naar de blob-container](../storage/blobs/storage-quickstart-blobs-portal.md#upload-a-block-blob), hetzij via de [Azure-portal](https://portal.azure.com) of [Azure Storage Explorer](https://storageexplorer.com/).

1. Als u wilt verwijzen naar het bestand in de blob-container, gebruikt u een HTTPS-koppeling die volgt op deze indeling hoofdlettergevoelig is:

   `https://<storage-account-name>.blob.core.windows.net/<blob-container-name>/<swagger-file-name>`

## <a name="connector-reference"></a>Connector-verwijzing

Hier vindt u meer informatie over de uitvoer van een HTTP + Swagger trigger of actie. HTTP + Swagger aanroep retourneert deze informatie:

| Naam van eigenschap | Type | Description |
|---------------|------|-------------|
| Headers | object | De headers van de aanvraag |
| De hoofdtekst | object | JSON-object | Het object met de inhoud van de hoofdtekst van de aanvraag |
| Statuscode | int | De statuscode van de aanvraag |
|||

| Statuscode | Description |
|-------------|-------------|
| 200 | OK |
| 202 | Geaccepteerd |
| 400 | Ongeldig verzoek |
| 401 | Niet geautoriseerd |
| 403 | Verboden |
| 404 | Niet gevonden |
| 500 | Interne serverfout. Er is een onbekende fout opgetreden. |
|||

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over andere [Logic Apps-connectors](../connectors/apis-list.md)