---
title: Firewallregels voor Azure Eventhubs | Microsoft Docs
description: Gebruik firewallregels om verbindingen van specifieke IP-adressen naar Azure Event Hubs te staan.
services: event-hubs
documentationcenter: ''
author: spelluru
manager: timlt
ms.service: event-hubs
ms.devlang: na
ms.custom: seodec18
ms.topic: article
ms.date: 12/06/2018
ms.author: spelluru
ms.openlocfilehash: 707290d7bf453ca71dd3c5cf8b39c917b3a1c479
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53268271"
---
# <a name="use-firewall-rules"></a>Firewall-regels gebruiken

Voor scenario's waarin Azure Event Hubs toegankelijk alleen van bepaalde bekende sites zijn moet, kunnen firewall-regels u regels voor het accepteren van verkeer dat afkomstig is van de specifieke IPv4-adressen te configureren. Bijvoorbeeld, kunnen deze adressen die van een zakelijke NAT-gateway zijn.

## <a name="when-to-use"></a>Wanneer gebruikt u dit?

Als u wilt instellen van uw Event Hubs-naamruimte als zodanig dat deze moet ontvangen verkeer van alleen een opgegeven bereik van IP-adressen en alle andere afwijzen en vervolgens kunt u gebruikmaken van een *firewallregel* Event Hub-eindpunten van blokkeren andere IP-adressen. Bijvoorbeeld, gebruikt u Event Hubs met [Azure Express Route] [ express-route] particuliere verbindingen met uw on-premises infrastructuur te maken.

## <a name="how-filter-rules-are-applied"></a>Hoe regels worden toegepast

De IP-filterregels worden toegepast op het niveau van de Event Hubs-naamruimte. Daarom de regels van toepassing op alle verbindingen van clients met behulp van een ondersteund protocol.

Elke verbindingspoging vanaf een IP-adres dat komt niet overeen met een toegestane IP-regel op de Event Hubs naamruimte wordt geweigerd als niet-geautoriseerde. Het antwoord wordt niet vermeld voor de IP-regel.

## <a name="default-setting"></a>Standaardinstelling

Standaard de **IP-Filter** raster in de portal voor Event Hubs is leeg. Deze instelling betekent dat uw event hub verbindingen van elk IP-adres aanvaardt. Deze instelling is gelijk aan een regel waarmee het 0.0.0.0/0 IP-adresbereik accepteert.

## <a name="ip-filter-rule-evaluation"></a>Evaluatie van IP-filter

IP-filterregels worden toegepast in volgorde en de eerste regel die overeenkomt met het IP-adres bepaalt de actie accepteren of weigeren.

>[!WARNING]
> Implementatie van Firewalls kunt voorkomen dat andere Azure-services interactie met Event Hubs.
>
> Vertrouwde Microsoft-services worden niet ondersteund wanneer IP-filters (Firewalls) worden geïmplementeerd en beschikbaar gesteld binnenkort.
>
> Algemene Azure-scenario's die niet met IP-filters werken (Let op: de lijst is **niet** volledig)-
> - Azure Monitor
> - Azure Stream Analytics
> - Integratie met Azure Event Grid
> - Azure IoT Hub-Routes
> - Azure IoT Device Explorer
> - Azure Data Explorer
>
> De onderstaande Microsoft services vereist zijn om te worden in een virtueel netwerk
> - Azure Web Apps
> - Azure Functions

### <a name="creating-a-firewall-rule-with-azure-resource-manager-templates"></a>Een firewallregel maken met Azure Resource Manager-sjablonen

> [!IMPORTANT]
> Firewall-regels worden ondersteund in **standard** en **toegewezen** lagen van Event Hubs. Het wordt niet ondersteund in de basic-laag.

De volgende Resource Manager-sjabloon kunt een regel voor IP-filter toe te voegen aan een bestaande Event Hubs-naamruimte.

Sjabloonparameters:

- **ipMask** is één IPv4-adres of een blok IP-adressen in CIDR-notatie. Bijvoorbeeld, in CIDR vertegenwoordigt notatie 70.37.104.0/24 de 256 IPv4-adressen van 70.37.104.0 tot 70.37.104.255, met 24 uur per dag die wijzen op het aantal bits aanzienlijke voorvoegsel voor het bereik.

> [!NOTE]
> Er zijn geen regels voor weigeren mogelijk, de Azure Resource Manager-sjabloon is de standaardactie die is ingesteld op **'Toestaan'** die verbindingen niet beperken.
> Bij het maken van regels voor Virtueelnetwerk of Firewalls, moeten we wijzigen de ***"defaultAction"***
> 
> uit
> ```json
> "defaultAction": "Allow"
> ```
> tot
> ```json
> "defaultAction": "Deny"
> ```
>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "eventhubNamespaceName": {
        "type": "string",
        "metadata": {
          "description": "Name of the Event Hubs namespace"
        }
      },
      "location": {
        "type": "string",
        "metadata": {
          "description": "Location for Namespace"
        }
      }
    },
    "variables": {
      "namespaceNetworkRuleSetName": "[concat(parameters('eventhubNamespaceName'), concat('/', 'default'))]",
    },
    "resources": [
      {
        "apiVersion": "2018-01-01-preview",
        "name": "[parameters('eventhubNamespaceName')]",
        "type": "Microsoft.EventHub/namespaces",
        "location": "[parameters('location')]",
        "sku": {
          "name": "Standard",
          "tier": "Standard"
        },
        "properties": { }
      },
      {
        "apiVersion": "2018-01-01-preview",
        "name": "[variables('namespaceNetworkRuleSetName')]",
        "type": "Microsoft.EventHub/namespaces/networkruleset",
        "dependsOn": [
          "[concat('Microsoft.EventHub/namespaces/', parameters('eventhubNamespaceName'))]"
        ],
        "properties": {
          "virtualNetworkRules": [<YOUR EXISTING VIRTUAL NETWORK RULES>],
          "ipRules": 
          [
            {
                "ipMask":"10.1.1.1",
                "action":"Allow"
            },
            {
                "ipMask":"11.0.0.0/24",
                "action":"Allow"
            }
          ],
          "defaultAction": "Deny"
        }
      }
    ],
    "outputs": { }
  }
```

Volg de instructies voor het implementeren van de sjabloon, [Azure Resource Manager][lnk-deploy].

## <a name="next-steps"></a>Volgende stappen

Virtuele netwerken, Zie de volgende koppeling voor beperken de toegang tot Event Hubs naar Azure:

- [Virtual Network-Service-eindpunten voor Eventhubs][lnk-vnet]

<!-- Links -->

[express-route]:  /azure/expressroute/expressroute-faqs#supported-services
[lnk-deploy]: ../azure-resource-manager/resource-group-template-deploy.md
[lnk-vnet]: event-hubs-service-endpoints.md