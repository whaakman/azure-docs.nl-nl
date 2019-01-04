---
title: De activiteit Azure-functie in Azure Data Factory | Microsoft Docs
description: Meer informatie over het gebruik van de activiteit Azure-functie om uit te voeren van een Azure-functie in een Data Factory-pijplijn
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 12/20/2018
ms.author: douglasl
ms.openlocfilehash: 974b45566255154bddc969b2a5ab3db0d2bf4f31
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54019279"
---
# <a name="azure-function-activity-in-azure-data-factory"></a>Azure functie-activiteit in Azure Data Factory

De activiteit Azure-functie kunt u uitvoeren [Azure Functions](../azure-functions/functions-overview.md) in een Data Factory-pijplijn. Als u wilt uitvoeren van een Azure-functie, die u wilt maken van een gekoppelde service-verbinding en een activiteit die Hiermee geeft u de Azure-functie die u van plan bent om uit te voeren.

## <a name="azure-function-linked-service"></a>Azure-functie gekoppelde service

Het retourtype van de Azure-functie is een geldige jobject is leeggemaakt. Iets anders mislukt en de algemene gebruikersfout genereert *fout aanvragende eindpunt*.

| **Eigenschap** | **Beschrijving** | **Vereist** |
| --- | --- | --- |
| type   | De eigenschap type moet worden ingesteld op: **AzureFunction** | ja |
| functie-app-url | De URL voor de Azure-functie-App. De indeling is `https://<accountname>.azurewebsites.net`. Deze URL is de waarde onder **URL** sectie bij het weergeven van uw functie-App in Azure portal  | ja |
| Functietoets | De toegangssleutel voor de Azure-functie. Klik op de **beheren** voor de betreffende functie uit en kopieer een de **functietoets** of de **hostsleutel**. Meer informatie vindt u hier: [Azure Functions-HTTP-triggers en bindingen](../azure-functions/functions-bindings-http-webhook.md#authorization-keys) | ja |
|   |   |   |

## <a name="azure-function-activity"></a>Azure functie-activiteit

| **Eigenschap**  | **Beschrijving** | **Toegestane waarden** | **Vereist** |
| --- | --- | --- | --- |
| naam  | Naam van de activiteit in de pijplijn  | Reeks | ja |
| type  | Type activiteit is 'AzureFunctionActivity' | Reeks | ja |
| Gekoppelde service | De gekoppelde Azure-functie-service voor de bijbehorende Azure-functie-App  | Verwijzing naar de gekoppelde service | ja |
| Functienaam  | Naam van de functie in de Azure-functie-App waarmee deze activiteit wordt aangeroepen | Reeks | ja |
| method  | REST-API-methode voor de functieaanroep | Tekenreeks ondersteunde typen: 'OPHALEN', 'POST', 'PLAATS'   | ja |
| koptekst  | Headers die worden verzonden naar de aanvraag. Bijvoorbeeld, de taal en het type instellen op een aanvraag: "headers": {'Accept-taal': ' nl-ons ", 'Content-Type': ' application/json'} | Tekenreeks (of expressie resultType van de tekenreeks) | Nee |
| hoofdtekst  | de hoofdtekst die samen met de aanvraag wordt verzonden naar de functie api-methode  | Tekenreeks (of expressie resultType van de tekenreeks) of object.   | Vereist voor opslag/POST-methoden |
|   |   |   | |

Het schema van de nettolading van de aanvraag in [verzoek nettolading schema](control-flow-web-activity.md#request-payload-schema) sectie.

## <a name="more-info"></a>Meer informatie

De activiteit Azure-functie ondersteunt **routering**. Bijvoorbeeld, als uw app gebruikmaakt van de volgende routering - `https://functionAPP.azurewebsites.net/api/functionName/{value}?code=<secret>` - de `functionName` is `functionName/{value}`, die u kunt parameters voor de gewenste `functionName` tijdens runtime.

De activiteit Azure-functie biedt ook ondersteuning voor **query's**. Een query moet deel uitmaken van de `functionName` - bijvoorbeeld `HttpTriggerCSharp2?name=hello` - waar de `function name` is `HttpTriggerCSharp2`.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de activiteiten in Data Factory in [pijplijnen en activiteiten in Azure Data Factory](concepts-pipelines-activities.md).