---
title: Een webhook niet aanroepen voor een Azure-activiteit logboek waarschuwing | Microsoft Docs
description: Informatie over het logboek activiteitsgebeurtenissen doorsturen naar andere services voor aangepaste acties. U kunt bijvoorbeeld SMS-berichten verzenden, meld bugs of hoogte van een team via een chat of messaging-service.
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 64d333d1-7f37-4a00-9d16-dda6e69a113b
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: johnkem
ms.openlocfilehash: 9872c30d123f0a7443e28dc58ee0d4e16572a390
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/14/2018
---
# <a name="call-a-webhook-on-an-azure-activity-log-alert"></a>Een webhook niet aanroepen voor een Azure-activiteit logboek-waarschuwing
U kunt webhooks gebruiken voor het routeren van een Azure waarschuwingsmeldingen met andere systemen voor naverwerking of voor aangepaste acties. U kunt een webhook gebruiken op een waarschuwing aan het wordt doorgestuurd naar de services die de SMS-berichten aan te melden bugs, om de hoogte van een team via chat of messaging-services, of voor verschillende andere acties te verzenden. U kunt ook een logboek activiteit waarschuwing instellen om e-mail te verzenden wanneer een waarschuwing is geactiveerd.

Dit artikel wordt beschreven hoe u een webhook moet worden aangeroepen wanneer een activiteit Azure log waarschuwing wordt geactiveerd. Ook ziet u hoe de nettolading voor de HTTP POST naar een webhook eruit ziet. Zie voor meer informatie over de installatie en het schema van een Azure metrische waarschuwing [een webhook configureren op een Azure metrische waarschuwing](insights-webhooks-alerts.md). 

> [!NOTE]
> De functie die ondersteuning biedt voor het aanroepen van een webhook met een Azure-activiteit logboek waarschuwing is momenteel in preview.
>
>

U kunt een activiteit logboek waarschuwing instellen met behulp van [Azure PowerShell-cmdlets](insights-powershell-samples.md#create-metric-alerts), een [platformoverschrijdende CLI](insights-cli-samples.md#work-with-alerts), of [Monitor REST-API's van Azure](https://msdn.microsoft.com/library/azure/dn933805.aspx). U kunt de Azure-portal op dit moment niet gebruiken voor het instellen van een activiteit logboek-waarschuwing.

## <a name="authenticate-the-webhook"></a>De webhook verifiëren
De webhook kan verifiëren met behulp van deze methoden:

* **Op basis van tokens autorisatie**. De webhook URI wordt opgeslagen met een token ID. Bijvoorbeeld: `https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`
* **Basisverificatie**. De webhook URI wordt opgeslagen met een gebruikersnaam en wachtwoord. Bijvoorbeeld:`https://userid:password@mysamplealert/webcallback?someparamater=somevalue&foo=bar`

## <a name="payload-schema"></a>De nettolading van schema
De POST-bewerking bevat de volgende JSON-nettolading en het schema voor alle activiteiten op basis van het logboek waarschuwingen. Dit schema is vergelijkbaar met die van waarschuwingen op basis van metrische gegevens.

```json
{
    "WebhookName": "Alert1515526229589",
    "RequestBody": {
        "schemaId": "Microsoft.Insights/activityLogs",
        "data": {
            "status": "Activated",
            "context": {
                "activityLog": {
                    "authorization": {
                        "action": "Microsoft.Compute/virtualMachines/deallocate/action",
                        "scope": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoVM/providers/Microsoft.Compute/virtualMachines/ContosoVM1"
                    },
                    "channels": "Operation",
                    "claims": {
                        "aud": "https://management.core.windows.net/",
                        "iss": "https://sts.windows.net/00000000-0000-0000-0000-000000000000/",
                        "iat": "1234567890",
                        "nbf": "1234567890",
                        "exp": "1234567890",
                        "aio": "Y2NgYBD8ZLlhu27JU6WZsXemMIvVAAA=",
                        "appid": "00000000-0000-0000-0000-000000000000",
                        "appidacr": "2",
                        "e_exp": "262800",
                        "http://schemas.microsoft.com/identity/claims/identityprovider": "https://sts.windows.net/00000000-0000-0000-0000-000000000000/",
                        "http://schemas.microsoft.com/identity/claims/objectidentifier": "00000000-0000-0000-0000-000000000000",
                        "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "00000000-0000-0000-0000-000000000000",
                        "http://schemas.microsoft.com/identity/claims/tenantid": "00000000-0000-0000-0000-000000000000",
                        "uti": "XnCk46TrDkOQXwo49Y8fAA",
                        "ver": "1.0"
                    },
                    "caller": "00000000-0000-0000-0000-000000000000",
                    "correlationId": "00000000-0000-0000-0000-000000000000",
                    "description": "",
                    "eventSource": "Administrative",
                    "eventTimestamp": "2018-01-09T20:11:25.8410967+00:00",
                    "eventDataId": "00000000-0000-0000-0000-000000000000",
                    "level": "Informational",
                    "operationName": "Microsoft.Compute/virtualMachines/deallocate/action",
                    "operationId": "00000000-0000-0000-0000-000000000000",
                    "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoVM/providers/Microsoft.Compute/virtualMachines/ContosoVM1",
                    "resourceGroupName": "ContosoVM",
                    "resourceProviderName": "Microsoft.Compute",
                    "status": "Succeeded",
                    "subStatus": "",
                    "subscriptionId": "00000000-0000-0000-0000-000000000000",
                    "submissionTimestamp": "2018-01-09T20:11:40.2986126+00:00",
                    "resourceType": "Microsoft.Compute/virtualMachines"
                }
            },
            "properties": {}
        }
    },
    "RequestHeader": {
        "Expect": "100-continue",
        "Host": "s1events.azure-automation.net",
        "User-Agent": "IcMBroadcaster/1.0",
        "X-CorrelationContext": "RkkKACgAAAACAAAAEADlBbM7x86VTrHdQ2JlmlxoAQAQALwazYvJ/INPskb8S5QzgDk=",
        "x-ms-request-id": "00000000-0000-0000-0000-000000000000"
    }
}
```

| Elementnaam | Beschrijving |
| --- | --- |
| status |Gebruikt voor metrische waarschuwingen. Voor de activiteit logboek waarschuwingen, altijd ingesteld op geactiveerd.|
| Context |De context van de gebeurtenis. |
| activityLog | De logboekeigenschappen van de gebeurtenis.|
| Autorisatie |De eigenschappen van de op rollen gebaseerde toegangsbeheer (RBAC) van de gebeurtenis. Deze eigenschappen zijn meestal **actie**, **rol**, en **bereik**. |
| actie | De actie die wordt vastgelegd door de waarschuwing. |
| Bereik | Het bereik van de waarschuwing (dat wil zeggen, de bron).|
| kanalen | De bewerking. |
| claims | Er is een verzameling van gegevens die zij gekoppeld aan de claims. |
| oproepende functie |De GUID of de gebruikersnaam van de gebruiker die heeft de bewerking, de UPN-claim of de SPN-claim op basis van beschikbaarheid uitgevoerd. Een null-waarde voor bepaalde systeemaanroepen kan zijn. |
| correlationId |Meestal een GUID in de indeling van tekenreeks. Gebeurtenissen met de **correlationId** deel uitmaken van dezelfde groter actie. Ze hebben meestal hetzelfde **correlationId** waarde. |
| description |De beschrijving van waarschuwing die is ingesteld wanneer de waarschuwing is gemaakt. |
| eventSource |De naam van de Azure-service of de infrastructuur die de gebeurtenis heeft gegenereerd. |
| eventTimestamp |Het tijdstip waarop dat de gebeurtenis heeft plaatsgevonden. |
| eventDataId |De unieke id van de gebeurtenis. |
| niveau |Een van de volgende waarden: kritiek, fout, waarschuwing, informatief of uitgebreid. |
| operationName |De naam van de bewerking. |
| operationId |Meestal een GUID die wordt gedeeld door de gebeurtenissen. De GUID is meestal komt overeen met één bewerking. |
| resourceId |De resource-ID van de betreffende resource. |
| resourceGroupName |De naam van de resourcegroep voor de betreffende resource. |
| resourceProviderName |De resourceprovider van de betreffende resource. |
| status |Een string-waarde die aangeeft van de status van de bewerking. Algemene waarden zijn gestart, wordt uitgevoerd, geslaagd, mislukt, actief en opgelost. |
| subStatus |Omvat gewoonlijk het HTTP-statuscode van de bijbehorende REST-aanroep. Het kan ook andere tekenreeksen die een substatus beschrijven omvatten. Algemene substatus waarden zijn OK (HTTP-statuscode: 200), gemaakt (HTTP-statuscode: 201), geaccepteerde (HTTP-statuscode: 202), geen inhoud (HTTP-statuscode: 204), ongeldige aanvraag (HTTP-statuscode: 400), niet vinden (HTTP-statuscode: 404), Conflict (HTTP-statuscode: 409), interne serverfout (HTTP-statuscode: 500), Service niet beschikbaar (HTTP-statuscode: 503), en de time-out van Gateway (HTTP-statuscode : 504). |
| subscriptionId |De Azure-abonnement-ID. |
| submissionTimestamp |De tijd waarop de gebeurtenis is gegenereerd door de Azure-service die de aanvraag verwerkt. |
| resourceType | Het type resource die de gebeurtenis heeft gegenereerd.|
| properties |Een set van sleutel-waardeparen die informatie over de gebeurtenis heeft. Bijvoorbeeld `Dictionary<String, String>`. |

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over de [activiteitenlogboek](monitoring-overview-activity-logs.md).
* Meer informatie over hoe [Azure Automation-scripts (runbooks) uitvoeren op Azure waarschuwingen](http://go.microsoft.com/fwlink/?LinkId=627081).
* Meer informatie over hoe [een logische app gebruiken voor het verzenden van een SMS-bericht via Twilio vanuit een Azure waarschuwing](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app). In dit voorbeeld is voor metrische waarschuwingen, maar u kunt deze werkt met een activiteit logboek waarschuwing wijzigen.
* Meer informatie over hoe [gebruik van een logische app een toegestane bericht verzenden vanuit een Azure waarschuwing](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app). In dit voorbeeld is voor metrische waarschuwingen, maar u kunt deze werkt met een activiteit logboek waarschuwing wijzigen.
* Meer informatie over hoe [gebruik van een logische app een bericht verzenden naar een Azure-wachtrij door een Azure-waarschuwing](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app). In dit voorbeeld is voor metrische waarschuwingen, maar u kunt deze werkt met een activiteit logboek waarschuwing wijzigen.
