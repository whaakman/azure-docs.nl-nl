---
title: Webhook-activiteit in Azure Data Factory | Microsoft Docs
description: De Webhook-activiteit heeft de uitvoering van de pijplijn niet verder voordat valideert de gekoppelde gegevensset aan bepaalde criteria die de gebruiker opgeeft.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 03/25/2019
ms.author: shlo
ms.openlocfilehash: 0b8b892f02e54c3b0ddb155af97ce63ff115bb1f
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/27/2019
ms.locfileid: "58522935"
---
# <a name="webhook-activity-in-azure-data-factory"></a>Webhook-activiteit in Azure Data Factory
U kunt een webactiviteit hook gebruiken voor het beheren van de uitvoering van pijplijnen met uw aangepaste code. Met behulp van de webhook-activiteit, kunnen klanten een eindpunt aanroepen en een URL voor terugbellen voor doorgeven. De uitvoering van de pijplijn wacht tot de callback gebruikt om te worden aangeroepen voordat u doorgaat met de volgende activiteit.

## <a name="syntax"></a>Syntaxis

```json

{
    "name": "MyWebHookActivity",
    "type": "WebHook",
    "typeProperties": {
        "method": "POST",
        "url": "<URLEndpoint>",
        "headers": {
            "Content-Type": "application/json"
        },
        "body": {
            "key": "value"
        },
        "timeout": "00:03:00",
        "authentication": {
            "type": "ClientCertificate",
            "pfx": "****",
            "password": "****"
        }
    }
}

```


## <a name="type-properties"></a>Type-eigenschappen



Eigenschap | Description | Toegestane waarden | Vereist
-------- | ----------- | -------------- | --------
naam | Naam van de web-hook-activiteit | String | Ja |
type | Moet worden ingesteld op **WebHook**. | String | Ja |
method | De rest-API-methode voor het doel-eindpunt. | tekenreeks. Ondersteunde typen: 'POST' | Ja |
url | Doel-eindpunt en het pad | Tekenreeks (of expressie resultType van de tekenreeks). | Ja |
Headers | Headers die worden verzonden naar de aanvraag. Bijvoorbeeld, de taal en het type instellen op een aanvraag: "headers": {'Accept-taal': ' nl-ons ", 'Content-Type': ' application/json'}. | Tekenreeks (of expressie resultType van de tekenreeks) | Ja, de Content-type-header is vereist. "headers": {'Content-Type': ' application/json'} |
hoofdtekst | Hiermee geeft u de nettolading die wordt verzonden naar het eindpunt. | De hoofdtekst van de doorgegeven aan de terugbellen URI moet een geldige JSON. Het schema van de nettolading van de aanvraag in [verzoek nettolading schema](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Fdocs.microsoft.com%2Fen-us%2Fazure%2Fdata-factory%2Fcontrol-flow-web-activity%23request-payload-schema&amp;data=02%7C01%7Cshlo%40microsoft.com%7Cde517eae4e7f4f2c408d08d6b167f6b1%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C636891457414397501&amp;sdata=ljUZv5csQQux2TT3JtTU9ZU8e1uViRzuX5DSNYkL0uE%3D&amp;reserved=0) sectie. | Ja |
verificatie | De verificatiemethode die wordt gebruikt voor het aanroepen van het eindpunt. Ondersteunde typen zijn 'Basic' of "ClientCertificate." Zie voor meer informatie, [verificatie](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Fdocs.microsoft.com%2Fen-us%2Fazure%2Fdata-factory%2Fcontrol-flow-web-activity%23authentication&amp;data=02%7C01%7Cshlo%40microsoft.com%7Cde517eae4e7f4f2c408d08d6b167f6b1%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C636891457414397501&amp;sdata=GdA1%2Fh2pAD%2BSyWJHSW%2BSKucqoAXux%2F4L5Jgndd3YziM%3D&amp;reserved=0) sectie. Als verificatie niet vereist is, sluit u deze eigenschap. | Tekenreeks (of expressie resultType van de tekenreeks) | Nee |
timeout | Hoe lang de activiteit wordt gewacht op het &#39;callbackuri&#39; worden aangeroepen. Hoe lang de activiteit wacht callbackuri worden aangeroepen. Standaardwaarde is 10mins ("00: 10:00 '). De indeling is Timespan dat wil zeggen d.hh:mm:ss | String | Nee |

## <a name="additional-notes"></a>Aanvullende opmerkingen

Azure Data Factory een extra eigenschap 'callbackuri' wordt doorgegeven in de hoofdtekst van het aan de url-eindpunt en wordt verwacht dat deze uri op die moet worden aangeroepen voordat de time-outwaarde die is opgegeven. Als de uri niet aangeroepen is, mislukt de activiteit met de status 'TimedOut'.

De web-hook-activiteit zelf niet kan alleen als de aanroep naar de aangepast eindpunt is mislukt. Een foutmelding krijgt, kan worden toegevoegd aan de hoofdtekst van de callback en gebruikt in een volgende activiteit.

## <a name="next-steps"></a>Volgende stappen
Zie andere controlestroomactiviteiten die door Data Factory worden ondersteund:

- [If Condition Activity](control-flow-if-condition-activity.md)
- [Execute Pipeline Activity](control-flow-execute-pipeline-activity.md)
- [Voor elke activiteit](control-flow-for-each-activity.md)
- [Get Metadata Activity](control-flow-get-metadata-activity.md)
- [Lookup Activity](control-flow-lookup-activity.md)
- [Webactiviteit](control-flow-web-activity.md)
- [Until Activity](control-flow-until-activity.md)
