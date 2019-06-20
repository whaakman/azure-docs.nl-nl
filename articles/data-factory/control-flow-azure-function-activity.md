---
title: De activiteit Azure-functie in Azure Data Factory | Microsoft Docs
description: Meer informatie over het gebruik van de activiteit Azure-functie om uit te voeren van een Azure-functie in een Data Factory-pijplijn
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/09/2019
author: sharonlo101
ms.author: shlo
manager: craigg
ms.openlocfilehash: dfdfb9e38f16d0077175587933b0800b87cc1931
ms.sourcegitcommit: 22c97298aa0e8bd848ff949f2886c8ad538c1473
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/14/2019
ms.locfileid: "67144117"
---
# <a name="azure-function-activity-in-azure-data-factory"></a>Azure functie-activiteit in Azure Data Factory

De activiteit Azure-functie kunt u uitvoeren [Azure Functions](../azure-functions/functions-overview.md) in een Data Factory-pijplijn. Als u wilt uitvoeren van een Azure-functie, die u wilt maken van een gekoppelde service-verbinding en een activiteit die Hiermee geeft u de Azure-functie die u van plan bent om uit te voeren.

Bekijk de volgende video voor een inleiding van acht minuten en demonstratie van deze functie:

> [!VIDEO https://channel9.msdn.com/shows/azure-friday/Run-Azure-Functions-from-Azure-Data-Factory-pipelines/player]

## <a name="azure-function-linked-service"></a>Azure-functie gekoppelde service

Het retourtype van de Azure-functie is alleen een geldig `JObject`. (Houd er rekening mee dat [JArray](https://www.newtonsoft.com/json/help/html/T_Newtonsoft_Json_Linq_JArray.htm) is *niet* een `JObject`.) Een retourtype dan `JObject` mislukt en de gebruikersfout genereert *antwoord inhoud is niet een geldig JObject*.

| **Eigenschap** | **Beschrijving** | **Vereist** |
| --- | --- | --- |
| type   | De eigenschap type moet worden ingesteld op: **AzureFunction** | ja |
| functie-app-url | De URL voor de Azure-functie-App. De indeling is `https://<accountname>.azurewebsites.net`. Deze URL is de waarde onder **URL** sectie bij het weergeven van uw functie-App in Azure portal  | ja |
| Functietoets | De toegangssleutel voor de Azure-functie. Klik op de **beheren** voor de betreffende functie uit en kopieer een de **functietoets** of de **hostsleutel**. Meer informatie vindt u hier: [Azure Functions-HTTP-triggers en bindingen](../azure-functions/functions-bindings-http-webhook.md#authorization-keys) | ja |
|   |   |   |

## <a name="azure-function-activity"></a>Azure functie-activiteit

| **Eigenschap**  | **Beschrijving** | **Toegestane waarden** | **Vereist** |
| --- | --- | --- | --- |
| name  | Naam van de activiteit in de pijplijn  | String | ja |
| type  | Type activiteit is 'AzureFunctionActivity' | String | ja |
| Gekoppelde service | De gekoppelde Azure-functie-service voor de bijbehorende Azure-functie-App  | Verwijzing naar de gekoppelde service | ja |
| Functienaam  | Naam van de functie in de Azure-functie-App waarmee deze activiteit wordt aangeroepen | String | ja |
| method  | REST-API-methode voor de functieaanroep | Tekenreeks ondersteunde typen: 'OPHALEN', 'POST', 'PLAATS'   | ja |
| koptekst  | Headers die worden verzonden naar de aanvraag. Bijvoorbeeld, de taal en het type instellen op een aanvraag: "headers": {'Accept-taal': ' nl-ons ", 'Content-Type': ' application/json'} | Tekenreeks (of expressie resultType van de tekenreeks) | Nee |
| De hoofdtekst  | de hoofdtekst die samen met de aanvraag wordt verzonden naar de functie api-methode  | Tekenreeks (of expressie resultType van de tekenreeks) of object.   | Vereist voor opslag/POST-methoden |
|   |   |   | |

Het schema van de nettolading van de aanvraag in [verzoek nettolading schema](control-flow-web-activity.md#request-payload-schema) sectie.

## <a name="routing-and-queries"></a>Routering en query 's

De activiteit Azure-functie ondersteunt **routering**. Bijvoorbeeld, als uw Azure-functie is het eindpunt `https://functionAPP.azurewebsites.net/api/<functionName>/<value>?code=<secret>`, dan zal de `functionName` te gebruiken in de activiteit Azure-functie is `<functionName>/<value>`. U kunt deze functie om de gewenste parameteriseren `functionName` tijdens runtime.

De activiteit Azure-functie biedt ook ondersteuning voor **query's**. Een query moet worden opgenomen als onderdeel van de `functionName`. Bijvoorbeeld, als naam van de functie is `HttpTriggerCSharp` en de query die u wilt opnemen, is `name=hello`, en vervolgens kunt u maken de `functionName` in de activiteit Azure-functie als `HttpTriggerCSharp?name=hello`. De parameters van deze functie kan worden gebruikt, zodat de waarde kan worden bepaald tijdens uitvoering.

## <a name="timeout-and-long-running-functions"></a>Time-out en langlopende functies

Azure Functions time-out na 230 seconden, ongeacht de `functionTimeout` instelling die u hebt geconfigureerd in de instellingen. Raadpleeg [dit artikel](../azure-functions/functions-versions.md#timeout) voor meer informatie. Als tijdelijke oplossing voor dit gedrag, gaat u als volgt een asynchrone patroon of duurzame functies gebruiken. Het voordeel van duurzame functies is dat deze verbindingen bieden een mechanisme voor hun eigen status bij te houden, zodat u hoeft voor het implementeren van uw eigen.

Meer informatie over duurzame functies in [in dit artikel](../azure-functions/durable/durable-functions-overview.md). U kunt een activiteit van de functie Azure instellen voor het aanroepen van de functie duurzame, die wordt een antwoord met een andere URI, zoals [in dit voorbeeld](../azure-functions/durable/durable-functions-http-api.md#http-api-url-discovery). Omdat `statusQueryGetUri` retourneert HTTP-Status 202 terwijl de functie wordt uitgevoerd, kunt u de status van de functie opvragen met behulp van een Web-activiteit. Eenvoudig instellen van een Web-activiteit met de `url` veld ingesteld op `@activity('<AzureFunctionActivityName>').output.statusQueryGetUri`. Wanneer de functie duurzame is voltooid, is de uitvoer van de functie is de uitvoer van de Web-activiteit.


## <a name="sample"></a>Voorbeeld

U vindt een voorbeeld van een Data Factory die gebruikmaakt van een Azure-functie om op te halen van de inhoud van een bestand met tar [hier](https://github.com/Azure/Azure-DataFactory/tree/master/SamplesV2/UntarAzureFilesWithAzureFunction).

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de activiteiten in Data Factory in [pijplijnen en activiteiten in Azure Data Factory](concepts-pipelines-activities.md).
