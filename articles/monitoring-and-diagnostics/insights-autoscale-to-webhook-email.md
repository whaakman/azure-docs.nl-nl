---
title: Gebruikt automatisch schalen acties voor het verzenden van e-mail en webhook waarschuwingsmeldingen. | Microsoft Docs
description: 'Informatie over het gebruiken van automatisch schalen acties kunnen worden gebeld web-URL''s of verzenden van e-mailmeldingen in de Azure-Monitor. '
author: anirudhcavale
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: eb9a4c98-0894-488c-8ee8-5df0065d094f
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/03/2017
ms.author: ancav
ms.openlocfilehash: 16caf14028494800e9259f0296c292b606d0210a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="use-autoscale-actions-to-send-email-and-webhook-alert-notifications-in-azure-monitor"></a>Acties voor automatisch schalen gebruiken voor het verzenden van e-mail en -webhook-waarschuwingsmeldingen in de Azure-Monitor
Dit artikel ziet u hoe u triggers hebt instelt, zodat u kunt specifieke web-URL's aanroepen of verzenden van e-mailberichten op basis van de acties voor automatisch schalen in Azure.  

## <a name="webhooks"></a>Webhooks.
Webhooks kunt u voor het routeren van de Azure waarschuwingsmeldingen met andere systemen voor na verwerking of aangepaste meldingen. Bijvoorbeeld: de waarschuwing routering naar services die een binnenkomende webaanvragen naar verzenden dat SMS, meld bugs melden dat een team chat gebruiken of messaging-services kunnen verwerken, enzovoort. De webhook URI moet een geldige HTTP of HTTPS-eindpunt.

## <a name="email"></a>E-mail
E-mail kan worden verzonden naar een geldig e-mailadres. Beheerders en medebeheerders van het abonnement waarop de regel wordt uitgevoerd, wordt ook gewaarschuwd.

## <a name="cloud-services-and-web-apps"></a>Cloudservices en Web-Apps
U kunt aanmelden via de Azure-portal voor Cloudservices en serverfarms (Web-Apps).

* Kies de **schalen door** metriek.

![schalen door](./media/insights-autoscale-to-webhook-email/insights-autoscale-notify.png)

## <a name="virtual-machine-scale-sets"></a>Virtuele-machineschaalsets
Voor nieuwere virtuele Machines met Resource Manager (virtuele-machineschaalsets) gemaakt, kunt u dit met behulp van REST-API, Resource Manager-sjablonen, PowerShell en CLI. De interface van de portal is nog niet beschikbaar.
Wanneer u de REST-API of Resource Manager-sjabloon, neemt u de meldingen element met de volgende opties.

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
| Veld | Verplichte? | Beschrijving |
| --- | --- | --- |
| bewerking |ja |de waarde moet 'Schaal' |
| sendToSubscriptionAdministrator |ja |waarde moet 'true' of 'false' |
| sendToSubscriptionCoAdministrators |ja |waarde moet 'true' of 'false' |
| customEmails |ja |mogelijke waarden zijn null [] of string-matrix van e-mailberichten |
| Webhooks. |ja |mogelijke waarden zijn null of geldig Uri |
| serviceUri |ja |een geldige https Uri |
| properties |ja |waarde moet leeg {} opzoeken of sleutel-waardeparen kan bevatten |

## <a name="authentication-in-webhooks"></a>Verificatie in webhooks.
De webhook kunt verifiëren met behulp van verificatie op basis van tokens, waar u de webhook URI opslaan met een token ID als een queryparameter. Bijvoorbeeld: https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue

## <a name="autoscale-notification-webhook-payload-schema"></a>Schema voor automatisch schalen melding webhook nettolading
Wanneer de melding voor automatisch schalen die wordt gegenereerd, worden de volgende metagegevens is opgenomen in de nettolading van de webhook:

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


| Veld | Verplichte? | Beschrijving |
| --- | --- | --- |
| status |ja |De status die aangeeft dat een actie voor automatisch schalen die is gegenereerd |
| bewerking |ja |Voor een verhoging van exemplaren zal moeten 'Scale Out' en voor een afname in exemplaren, deze 'Schaal In' |
| Context |ja |De context van de actie voor automatisch schalen |
| tijdstempel |ja |Tijdstempel wanneer de actie voor automatisch schalen is geactiveerd |
| id |Ja |Bronnenbeheerder-ID van de instelling voor automatisch schalen |
| naam |Ja |De naam van de instelling voor automatisch schalen |
| Meer informatie |Ja |Uitleg van de actie die de service automatisch schalen duurde en de wijziging in het aantal exemplaren |
| subscriptionId |Ja |Abonnement-ID van de doelresource die wordt geschaald |
| resourceGroupName |Ja |Naam van de resourcegroep van de doelresource die wordt geschaald |
| resourceName |Ja |Naam van de doelresource die wordt geschaald |
| resourceType |Ja |De drie ondersteunde waarden: 'microsoft.classiccompute/domainnames/slots/roles' - functies, 'microsoft.compute/virtualmachinescalesets' - virtuele Machine-Schaalsets, en 'Microsoft.Web/serverfarms' - Web-App Service in de Cloud |
| resourceId |Ja |Bronnenbeheerder-ID van de doelresource die wordt geschaald |
| portalLink |Ja |Azure portal-koppeling naar de pagina overzicht van de doelbron |
| oldCapacity |Ja |Het huidige (oude) aantal exemplaren wanneer automatisch schalen een schaalactie heeft |
| newCapacity |Ja |De nieuwe versie van het aantal instanties dat automatisch schalen die de resource toe aan geschaald |
| Eigenschappen |Nee |Optioneel. Reeks < sleutel, waarde > paren (bijvoorbeeld woordenlijst < String, String >). De van eigenschappenveld is optioneel. U kunt in een aangepaste gebruikersinterface of de werkstroom van Logic app op basis van sleutels en waarden die kunnen worden doorgegeven met behulp van de nettolading van de invoeren. Een andere manier om door te geven van aangepaste eigenschappen terug naar de uitgaande webhook-aanroep is het gebruik van de webhook URI zelf (als queryparameters) |
