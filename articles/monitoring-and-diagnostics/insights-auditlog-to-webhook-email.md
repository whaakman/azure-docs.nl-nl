---
title: Een webhook aanroepen in Azure Activity Log waarschuwingen | Microsoft Docs
description: Activiteit logboekgebeurtenissen naar andere services voor aangepaste acties worden doorgestuurd. Bijvoorbeeld SMS-bericht verzenden, meld bugs of hoogte van een team via chat/messaging-service.
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
ms.openlocfilehash: 08467aed4e1601b32598fc42515d9c38b601a9d4
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="call-a-webhook-on-azure-activity-log-alerts"></a>Een webhook aanroepen in Azure Activity Log waarschuwingen
Webhooks kunt u een Azure waarschuwingsmeldingen versturen met andere systemen voor na verwerking of aangepaste acties. U kunt een webhook gebruiken op een waarschuwing aan het wordt doorgestuurd naar de services die de SMS-bericht verzenden, meld bugs, een team via chat/berichtenservices melden of komen een aantal andere acties. Dit artikel wordt beschreven hoe u een webhook moet worden aangeroepen wanneer een waarschuwing wordt geactiveerd voor een Azure Activity Log instelt. Ook ziet u hoe de nettolading voor de HTTP POST naar een webhook eruit ziet. Voor informatie over de installatie en het schema voor een Azure metrische waarschuwing [deze pagina vindt u in plaats daarvan](insights-webhooks-alerts.md). U kunt ook een waarschuwing activiteitenlogboek instellen voor het verzenden van e-mailbericht wanneer geactiveerd.

> [!NOTE]
> Deze functie is momenteel in preview en wordt verwijderd op een bepaald moment in de toekomst.
>
>

U kunt instellen dat een activiteitenlogboek waarschuwing met de [Azure PowerShell-Cmdlets](insights-powershell-samples.md#create-metric-alerts), [platformoverschrijdende CLI](insights-cli-samples.md#work-with-alerts), of [REST-API van Azure-Monitor](https://msdn.microsoft.com/library/azure/dn933805.aspx). U instellen niet op dit moment een met behulp van de Azure-portal.

## <a name="authenticating-the-webhook"></a>Verifiëren van de webhook
De webhook kunt verifiëren met behulp van deze methoden:

1. **Op basis van tokens autorisatie** -de webhook URI wordt opgeslagen met een token ID, bijvoorbeeld:`https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`
2. **Basisverificatie** -de webhook URI wordt opgeslagen met een gebruikersnaam en wachtwoord, bijvoorbeeld:`https://userid:password@mysamplealert/webcallback?someparamater=somevalue&foo=bar`

## <a name="payload-schema"></a>De nettolading van schema
De POST-bewerking bevat de volgende JSON-nettolading en het schema voor alle waarschuwingen op basis van het activiteitenlogboek. Dit schema is vergelijkbaar met op waarschuwingen op basis van metrische gegevens.

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
| status |Gebruikt voor metrische waarschuwingen. Altijd ingesteld op 'geactiveerd' voor activiteitenlogboek van waarschuwingen. |
| Context |De context van de gebeurtenis. |
| activityLog | De logboekeigenschappen van de gebeurtenis.|
| Autorisatie |De RBAC-eigenschappen van de gebeurtenis. Deze omvatten doorgaans de "action", 'rol' en het 'bereik'. |
| actie | De actie opgenomen door de waarschuwing. |
| Bereik | Bereik van de waarschuwing (eenledige resource).|
| kanalen | Bewerking |
| claims | Een verzameling gegevens is op is gekoppeld aan de claims. |
| oproepende functie |GUID of de gebruikersnaam van de gebruiker die de bewerking, UPN-claim of SPN claim op basis van beschikbaarheid uitgevoerd. Kan niet null zijn voor bepaalde systeemaanroepen zijn. |
| correlationId |Meestal een GUID in de indeling van tekenreeks. Gebeurtenissen met correlationId deel uitmaken van dezelfde groter actie en meestal delen een correlationId. |
| description |Beschrijving van de waarschuwing als set tijdens het maken van de waarschuwing. |
| eventSource |Naam van de Azure-service of de infrastructuur die de gebeurtenis heeft gegenereerd. |
| eventTimestamp |Tijdstip waarop die de gebeurtenis heeft plaatsgevonden. |
| eventDataId |De unieke id voor de gebeurtenis. |
| niveau |Een van de volgende waarden: 'Kritiek', 'Fout', 'Waarschuwing', 'Ter informatie' en "Verbose." |
| operationName |De naam van de bewerking. |
| operationId |Meestal een GUID gedeeld door de gebeurtenissen die overeenkomt met één bewerking. |
| resourceId |Bron-ID van de betrokken resource. |
| resourceGroupName |Naam van de resourcegroep voor de betrokken resource |
| resourceProviderName |De resourceprovider van de betrokken resource. |
| status |De tekenreeks. De status van de bewerking. Algemene waarden zijn: 'Gestart', 'Wordt uitgevoerd', 'Geslaagd', 'Is mislukt', 'Active', 'Opgelost'. |
| subStatus |Omvat gewoonlijk het HTTP-statuscode van de bijbehorende REST-aanroep. Het kan ook andere tekenreeksen met een beschrijving van een substatus omvatten. Algemene substatus waarden zijn: OK (HTTP-statuscode: 200), gemaakt (HTTP-statuscode: 201), geaccepteerde (HTTP-statuscode: 202), geen inhoud (HTTP-statuscode: 204), ongeldige aanvraag (HTTP-statuscode: 400), niet vinden (HTTP-statuscode: 404), Conflict (HTTP-statuscode: 409), interne serverfout (HTTP-statuscode: 500), Service niet beschikbaar (HTTP-statuscode: 503), time-out van Gateway (HTTP-statuscode: 504) |
| subscriptionId |Azure-abonnement-ID. |
| submissionTimestamp |Tijd waarop de gebeurtenis is gegenereerd door de Azure-service die de aanvraag verwerkt. |
| resourceType | Het type resource die de gebeurtenis heeft gegenereerd.|
| properties |Een set `<Key, Value>` paren (dat wil zeggen `Dictionary<String, String>`) die bevat details over de gebeurtenis. |

## <a name="next-steps"></a>Volgende stappen
* [Meer informatie over het activiteitenlogboek](monitoring-overview-activity-logs.md)
* [Azure Automation-scripts (Runbooks) op waarschuwingen van Azure uitvoeren](http://go.microsoft.com/fwlink/?LinkId=627081)
* [Logische Apps gebruiken voor het verzenden van een SMS-bericht via Twilio vanuit een Azure waarschuwing](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app). In dit voorbeeld is voor metrische waarschuwingen, maar kan worden gewijzigd om te werken met een waarschuwing activiteitenlogboek.
* [Logische App gebruiken voor het verzenden van een toegestane bericht door een Azure-waarschuwing](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app). In dit voorbeeld is voor metrische waarschuwingen, maar kan worden gewijzigd om te werken met een waarschuwing activiteitenlogboek.
* [Logische App gebruiken voor het verzenden van een bericht naar een Azure-wachtrij door een Azure-waarschuwing](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app). In dit voorbeeld is voor metrische waarschuwingen, maar kan worden gewijzigd om te werken met een waarschuwing activiteitenlogboek.
