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
ms.openlocfilehash: 341ab32ad0ec691285fbf1537ee298ab30156a5d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
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

```
{
        "status": "Activated",
        "context": {
                "resourceProviderName": "Microsoft.Web",
                "event": {
                        "$type": "Microsoft.WindowsAzure.Management.Monitoring.Automation.Notifications.GenericNotifications.Datacontracts.InstanceEventContext, Microsoft.WindowsAzure.Management.Mon.Automation",
                        "authorization": {
                                "action": "Microsoft.Web/sites/start/action",
                                "scope": "/subscriptions/s1/resourcegroups/rg1/providers/Microsoft.Web/sites/leoalerttest"
                        },
                        "eventDataId": "327caaca-08d7-41b1-86d8-27d0a7adb92d",
                        "category": "Administrative",
                        "caller": "myname@mycompany.com",
                        "httpRequest": {
                                "clientRequestId": "f58cead8-c9ed-43af-8710-55e64def208d",
                                "clientIpAddress": "104.43.166.155",
                                "method": "POST"
                        },
                        "status": "Succeeded",
                        "subStatus": "OK",
                        "level": "Informational",
                        "correlationId": "4a40beaa-6a63-4d92-85c4-923a25abb590",
                        "eventDescription": "",
                        "operationName": "Microsoft.Web/sites/start/action",
                        "operationId": "4a40beaa-6a63-4d92-85c4-923a25abb590",
                        "properties": {
                                "$type": "Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage",
                                "statusCode": "OK",
                                "serviceRequestId": "f7716681-496a-4f5c-8d14-d564bcf54714"
                        }
                },
                "timestamp": "Friday, March 11, 2016 9:13:23 PM",
                "id": "/subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/alertrules/alertonevent2",
                "name": "alertonevent2",
                "description": "test alert on event start",
                "conditionType": "Event",
                "subscriptionId": "s1",
                "resourceId": "/subscriptions/s1/resourcegroups/rg1/providers/Microsoft.Web/sites/leoalerttest",
                "resourceGroupName": "rg1"
        },
        "properties": {
                "key1": "value1",
                "key2": "value2"
        }
}
```

| Elementnaam | Beschrijving |
| --- | --- |
| status |Gebruikt voor metrische waarschuwingen. Altijd ingesteld op 'geactiveerd' voor activiteitenlogboek van waarschuwingen. |
| Context |De context van de gebeurtenis. |
| resourceProviderName |De resourceprovider van de betrokken resource. |
| conditionType |Altijd 'gebeurtenis'. |
| naam |Naam van de waarschuwingsregel. |
| id |Bron-ID van de waarschuwing. |
| description |Beschrijving van de waarschuwing als set tijdens het maken van de waarschuwing. |
| subscriptionId |Azure-abonnement-ID. |
| tijdstempel |Tijd waarop de gebeurtenis is gegenereerd door de Azure-service die de aanvraag verwerkt. |
| resourceId |Bron-ID van de betrokken resource. |
| resourceGroupName |Naam van de resourcegroep voor de betrokken resource |
| properties |Een set `<Key, Value>` paren (dat wil zeggen `Dictionary<String, String>`) die bevat details over de gebeurtenis. |
| Gebeurtenis |Element met metagegevens over de gebeurtenis. |
| Autorisatie |De RBAC-eigenschappen van de gebeurtenis. Deze omvatten doorgaans de "action", 'rol' en het 'bereik'. |
| category |De categorie van de gebeurtenis. Ondersteunde waarden zijn: administratieve, waarschuwing, beveiliging, ServiceHealth, aanbeveling. |
| aanroeper |E-mailadres van de gebruiker die de bewerking, UPN-claim of SPN claim op basis van beschikbaarheid uitgevoerd. Kan niet null zijn voor bepaalde systeemaanroepen zijn. |
| correlationId |Meestal een GUID in de indeling van tekenreeks. Gebeurtenissen met correlationId deel uitmaken van dezelfde groter actie en meestal delen een correlationId. |
| eventDescription |De beschrijving van de statische tekst van de gebeurtenis. |
| eventDataId |De unieke id voor de gebeurtenis. |
| EventSource |Naam van de Azure-service of de infrastructuur die de gebeurtenis heeft gegenereerd. |
| httpRequest |Omvat gewoonlijk het 'clientRequestId', "clientIpAddress" en "method" (bijv. HTTP-methode plaatsen). |
| niveau |Een van de volgende waarden: 'Kritiek', 'Fout', 'Waarschuwing', 'Ter informatie' en "Verbose." |
| operationId |Meestal een GUID gedeeld door de gebeurtenissen die overeenkomt met één bewerking. |
| operationName |De naam van de bewerking. |
| properties |Eigenschappen van de gebeurtenis. |
| status |De tekenreeks. De status van de bewerking. Algemene waarden zijn: 'Gestart', 'Wordt uitgevoerd', 'Geslaagd', 'Is mislukt', 'Active', 'Opgelost'. |
| subStatus |Omvat gewoonlijk het HTTP-statuscode van de bijbehorende REST-aanroep. Het kan ook andere tekenreeksen met een beschrijving van een substatus omvatten. Algemene substatus waarden zijn: OK (HTTP-statuscode: 200), gemaakt (HTTP-statuscode: 201), geaccepteerde (HTTP-statuscode: 202), geen inhoud (HTTP-statuscode: 204), ongeldige aanvraag (HTTP-statuscode: 400), niet vinden (HTTP-statuscode: 404), Conflict (HTTP-statuscode: 409), interne serverfout (HTTP-statuscode: 500), Service niet beschikbaar (HTTP-statuscode: 503), time-out van Gateway (HTTP-statuscode: 504) |

## <a name="next-steps"></a>Volgende stappen
* [Meer informatie over het activiteitenlogboek](monitoring-overview-activity-logs.md)
* [Azure Automation-scripts (Runbooks) op waarschuwingen van Azure uitvoeren](http://go.microsoft.com/fwlink/?LinkId=627081)
* [Logische Apps gebruiken voor het verzenden van een SMS-bericht via Twilio vanuit een Azure waarschuwing](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app). In dit voorbeeld is voor metrische waarschuwingen, maar kan worden gewijzigd om te werken met een waarschuwing activiteitenlogboek.
* [Logische App gebruiken voor het verzenden van een toegestane bericht door een Azure-waarschuwing](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app). In dit voorbeeld is voor metrische waarschuwingen, maar kan worden gewijzigd om te werken met een waarschuwing activiteitenlogboek.
* [Logische App gebruiken voor het verzenden van een bericht naar een Azure-wachtrij door een Azure-waarschuwing](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app). In dit voorbeeld is voor metrische waarschuwingen, maar kan worden gewijzigd om te werken met een waarschuwing activiteitenlogboek.
