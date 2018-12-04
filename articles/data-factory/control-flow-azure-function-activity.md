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
ms.devlang: na
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: douglasl
ms.openlocfilehash: ef93c62a2e2084a43eeda578c889a568d04db4f1
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52856086"
---
# <a name="azure-function-activity-in-azure-data-factory"></a>Azure functie-activiteit in Azure Data Factory

De activiteit Azure-functie kunt u uitvoeren [Azure Functions](../azure-functions/functions-overview.md) in een Data Factory-pijplijn. Als u wilt uitvoeren van een Azure-functie, die u wilt maken van een gekoppelde service-verbinding en een activiteit die Hiermee geeft u de Azure-functie die u van plan bent om uit te voeren.

## <a name="azure-function-linked-service"></a>Azure-functie gekoppelde service

| **Eigenschap** | **Beschrijving** | **Vereist** |
| --- | --- | --- |
| type   | De eigenschap type moet worden ingesteld op: **AzureFunction** | ja |
| functie-app-url | De URL voor de Azure-functie-App. De indeling is `https://<accountname>.azurewebsites.net`. Deze URL is de waarde onder **URL** sectie bij het weergeven van uw functie-App in Azure portal  | ja |
| Functietoets | De toegangssleutel voor de Azure-functie. Klik op de **beheren** voor de betreffende functie uit en kopieer een de **functietoets** of de **hostsleutel**. Ontdek meer hier: [Azure Functions-HTTP-triggers en bindingen](../azure-functions/functions-bindings-http-webhook.md#authorization-keys) | ja |
|   |   |   |

## <a name="azure-function-activity"></a>Azure functie-activiteit

| **Eigenschap**  | **Beschrijving** | **Toegestane waarden** | **Vereist** |
| --- | --- | --- | --- |
| naam  | Naam van de activiteit in de pijplijn  | Reeks | ja |
| type  | Type activiteit is 'AzureFunctionActivity' | Reeks | ja |
| Gekoppelde service | De gekoppelde Azure-functie-service voor de bijbehorende Azure-functie-App  | Verwijzing naar de gekoppelde service | ja |
| Functienaam  | Naam van de functie in de Azure-functie-App waarmee deze activiteit wordt aangeroepen | Reeks | ja |
| method  | REST-API-methode voor de functieaanroep | Tekenreeks ondersteunde typen: 'ophalen', 'POST', 'plaats'   | ja |
| koptekst  | Headers die worden verzonden naar de aanvraag. Bijvoorbeeld, de taal en het type instellen op een aanvraag: "headers": {'Accept-taal': ' nl-ons ", 'Content-Type': ' application/json'} | Tekenreeks (of expressie resultType van de tekenreeks) | Nee |
| hoofdtekst  | de hoofdtekst die samen met de aanvraag wordt verzonden naar de functie api-methode  | Tekenreeks (of expressie resultType van de tekenreeks).   | Vereist voor opslag/POST-methoden |
|   |   |   | |

Het schema van de nettolading van de aanvraag in [verzoek nettolading schema](control-flow-web-activity.md#request-payload-schema) sectie.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de activiteiten in Data Factory in [pijplijnen en activiteiten in Azure Data Factory](concepts-pipelines-activities.md).