---
title: Automatisch schalen gebruiken voor het verzenden van e-mail en webhook waarschuwingsmeldingen
description: "Informatie over het gebruik van acties voor automatisch schalen naar web-URL's worden aangeroepen of een e-mailmeldingen verzenden in Azure Monitor. "
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 04/03/2017
ms.author: ancav
ms.component: autoscale
ms.openlocfilehash: c655f117009c0c7c682ad92a951edc7e12255bc2
ms.sourcegitcommit: e37fa6e4eb6dbf8d60178c877d135a63ac449076
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2018
ms.locfileid: "53326107"
---
# <a name="use-autoscale-actions-to-send-email-and-webhook-alert-notifications-in-azure-monitor"></a>Acties voor automatisch schalen gebruiken voor het verzenden van e-mail en webhook-meldingen van waarschuwingen in Azure Monitor
Dit artikel leest u hoe triggers zo instellen dat u kunt specifieke web-URL's aanroepen of verzenden van e-mailberichten op basis van de acties voor automatisch schalen in Azure.  

## <a name="webhooks"></a>Webhooks
Webhooks kunt u de Azure-waarschuwingen doorsturen naar andere systemen voor na verwerking of aangepaste meldingen. Bijvoorbeeld, routering de waarschuwing naar services die een binnenkomende webaanvraag voor het verzenden van dat SMS, bugs, het logboek op de hoogte stellen een team met behulp van chatten of messaging-services kunnen worden verwerkt, enzovoort. De webhook-URI moet een geldige HTTP of HTTPS-eindpunt.

## <a name="email"></a>Email
E-mailadres kan worden verzonden naar elk geldig e-mailadres. Beheerders en medebeheerders van het abonnement waarin de regel wordt uitgevoerd worden, ook geïnformeerd.

## <a name="cloud-services-and-web-apps"></a>Cloudservices en Web-Apps
U kunt zich aanmelden via de Azure-portal voor Cloud Services en -serverfarms (Web-Apps).

* Kies de **schalen door** metrische gegevens.

![schalen met](./media/autoscale-webhook-email/insights-autoscale-notify.png)

## <a name="virtual-machine-scale-sets"></a>Schaalsets voor virtuele machines
Voor nieuwere virtuele Machines die zijn gemaakt met Resource Manager (virtuele-machineschaalsets), kunt u dit configureren met behulp van REST-API, Resource Manager-sjablonen, PowerShell en CLI. Een interface van de portal is nog niet beschikbaar.
Wanneer met de REST-API of Resource Manager-sjabloon, neemt u de meldingen-element met de volgende opties.

```
"notifications": [
      {
        "operation": "Scale",
        "email": {
          "sendToSubscriptionAdministrator": false,
          "sendToSubscriptionCoAdministrators": false,
          "customEmails": [
              "user1@mycompany.com",
              "user2@mycompany.com"
              ]
        },
        "webhooks": [
          {
            "serviceUri": "https://foo.webhook.example.com?token=abcd1234",
            "properties": {
              "optional_key1": "optional_value1",
              "optional_key2": "optional_value2"
            }
          }
        ]
      }
    ]
```
| Veld | Verplichte? | Description |
| --- | --- | --- |
| bewerking |ja |de waarde moet 'Schaal' |
| sendToSubscriptionAdministrator |ja |waarde moet 'true' of 'false' |
| sendToSubscriptionCoAdministrators |ja |waarde moet 'true' of 'false' |
| customEmails |ja |waarde mag null [] of string-matrix van e-mailberichten |
| Webhooks |ja |waarde kan null zijn of een geldige Uri zijn |
| serviceUri |ja |een geldig https-Uri |
| properties |ja |waarde moet leeg zijn {} of sleutel / waarde-paren kan bevatten |

## <a name="authentication-in-webhooks"></a>Verificatie in webhooks
De webhook kunt verifiëren met behulp van verificatie op basis van tokens, waar u de URI van de webhook opslaan met een token-ID als een queryparameter. Bijvoorbeeld: https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue

## <a name="autoscale-notification-webhook-payload-schema"></a>Automatisch schalen melding webhook-schema voor nettolading
Wanneer de melding voor automatisch schalen wordt gegenereerd, wordt de volgende metagegevens in de nettolading van de webhook opgenomen:

```
{
        "version": "1.0",
        "status": "Activated",
        "operation": "Scale In",
        "context": {
                "timestamp": "2016-03-11T07:31:04.5834118Z",
                "id": "/subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/autoscalesettings/myautoscaleSetting",
                "name": "myautoscaleSetting",
                "details": "Autoscale successfully started scale operation for resource 'MyCSRole' from capacity '3' to capacity '2'",
                "subscriptionId": "s1",
                "resourceGroupName": "rg1",
                "resourceName": "MyCSRole",
                "resourceType": "microsoft.classiccompute/domainnames/slots/roles",
                "resourceId": "/subscriptions/s1/resourceGroups/rg1/providers/microsoft.classicCompute/domainNames/myCloudService/slots/Production/roles/MyCSRole",
                "portalLink": "https://portal.azure.com/#resource/subscriptions/s1/resourceGroups/rg1/providers/microsoft.classicCompute/domainNames/myCloudService",
                "oldCapacity": "3",
                "newCapacity": "2"
        },
        "properties": {
                "key1": "value1",
                "key2": "value2"
        }
}
```


| Veld | Verplichte? | Description |
| --- | --- | --- |
| status |ja |De status die aangeeft dat een actie voor automatisch schalen is gegenereerd |
| bewerking |ja |Voor een verhoging van de exemplaren worden 'Scale Out' en voor een afname in exemplaren, is 'Schaal In' |
| Context |ja |De context van de actie voor automatisch schalen |
| tijdstempel |ja |Tijdstempel wanneer het voor automatisch schalen is geactiveerd |
| id |Ja |Resource Manager-ID van de instelling voor automatisch schalen |
| naam |Ja |De naam van de instelling voor automatisch schalen |
| details |Ja |Uitleg van de actie die de service voor automatisch schalen heeft geduurd en de wijziging in het aantal instanties |
| subscriptionId |Ja |Abonnements-ID van de doelresource die is die wordt geschaald |
| resourceGroupName |Ja |Naam van de resourcegroep van de doelresource die is die wordt geschaald |
| resourceName |Ja |Naam van de doelresource die is die wordt geschaald |
| ResourceType |Ja |De drie ondersteunde waarden: "microsoft.classiccompute/domainnames/slots/roles" - Service in de Cloud-functies, 'microsoft.compute/virtualmachinescalesets' - Virtual Machine Scale Sets, en "Microsoft.Web/serverfarms" - Web-App |
| resourceId |Ja |Resource Manager-ID van de doelresource die is die wordt geschaald |
| portalLink |Ja |Azure portal koppelen aan de pagina overzicht van de doelresource |
| oldCapacity |Ja |Het huidige (oude) aantal instanties wanneer automatisch schalen een schaalactie heeft |
| newCapacity |Ja |De nieuwe versie van het aantal instanties dat automatisch schalen geschaald de resource |
| Properties |Nee |Optioneel. Set < sleutel, waarde > paren (bijvoorbeeld, Dictionary < String, String >). Het eigenschappenveld is optioneel. In een aangepaste gebruikersinterface of de werkstroom voor logische Apps die zijn gebaseerd, kunt u sleutels en waarden die kunnen worden doorgegeven met behulp van de nettolading. Een alternatieve manier om door te geven van aangepaste eigenschappen terug naar de uitgaande webhook-aanroep is het gebruik van de webhook URI zelf (zoals queryparameters) |
