---
title: Firewallregels voor Azure Service Bus | Microsoft Docs
description: Het gebruik van firewallregels om verbindingen van specifieke IP-adressen met Azure Service Bus te staan.
services: service-bus
documentationcenter: ''
author: axisc
manager: timlt
editor: spelluru
ms.service: service-bus
ms.devlang: na
ms.topic: article
ms.date: 01/23/2019
ms.author: aschhab
ms.openlocfilehash: 52475a3976ef8897ffed07c499eb06e7476c0156
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58136290"
---
# <a name="use-firewall-rules"></a>Firewall-regels gebruiken

Voor scenario's waarin Azure Service Bus alleen toegankelijk zijn van bepaalde bekende sites is, kunnen Firewall-regels u regels voor het accepteren van verkeer dat afkomstig is van de specifieke IPv4-adressen te configureren. Bijvoorbeeld, kunnen deze adressen die van een zakelijke NAT-gateway zijn.

## <a name="when-to-use"></a>Wanneer gebruikt u dit?

Als u op zoek bent naar Service Bus-instellingen zoals die moet ontvangen verkeer alleen vanaf een opgegeven bereik van IP-adressen en alle andere afwijzen en vervolgens kunt u gebruikmaken van een *Firewall* moet worden geblokkeerd dat Service Bus-eindpunten van andere IP-adressen. Bijvoorbeeld, gebruikt u Service Bus met [Azure Express Route] [ express-route] particuliere verbindingen met uw on-premises infrastructuur te maken. 

## <a name="how-filter-rules-are-applied"></a>Hoe regels worden toegepast

De IP-filterregels worden toegepast op het niveau van de Service Bus-naamruimte. Daarom de regels van toepassing op alle verbindingen van clients met behulp van een ondersteund protocol.

Elke verbindingspoging vanaf een IP-adres dat komt niet overeen met een toegestane IP-regel op de Service Bus naamruimte wordt geweigerd als niet-geautoriseerde. Het antwoord wordt niet vermeld voor de IP-regel.

## <a name="default-setting"></a>Standaardinstelling

Standaard de **IP-Filter** raster in de portal voor Service Bus is leeg. Deze instelling betekent dat uw naamruimte verbindingen elk IP-adres aanvaardt. Deze instelling is gelijk aan een regel waarmee het 0.0.0.0/0 IP-adresbereik accepteert.

## <a name="ip-filter-rule-evaluation"></a>Evaluatie van IP-filter

IP-filterregels worden toegepast in volgorde en de eerste regel die overeenkomt met het IP-adres bepaalt de actie accepteren of weigeren.

>[!WARNING]
> Implementatie van Firewall-regels kunt voorkomen dat andere Azure-services interactie met Service Bus.
>
> Vertrouwde Microsoft-services worden niet ondersteund wanneer IP-filters (Firewall-regels) worden geïmplementeerd en beschikbaar gesteld binnenkort.
>
> Algemene Azure-scenario's die niet met IP-filters werken (Let op: de lijst is **niet** volledig)-
> - Azure Monitor
> - Azure Stream Analytics
> - Integratie met Azure Event Grid
> - Azure IoT Hub Routes
> - Azure IoT Device Explorer
> - Azure Data Explorer
>
> De onderstaande Microsoft services vereist zijn om te worden in een virtueel netwerk
> - Azure App Service
> - Azure Functions

### <a name="creating-a-virtual-network-and-firewall-rule-with-azure-resource-manager-templates"></a>Het maken van een virtueel netwerk en firewall-regel met Azure Resource Manager-sjablonen

> [!IMPORTANT]
> Firewalls en virtuele netwerken worden alleen ondersteund in de **premium** laag van Service Bus.

De volgende Resource Manager-sjabloon kunt een regel voor virtuele netwerken toe te voegen aan een bestaande Service Bus-naamruimte.

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
      "servicebusNamespaceName": {
        "type": "string",
        "metadata": {
          "description": "Name of the Service Bus namespace"
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
      "namespaceNetworkRuleSetName": "[concat(parameters('servicebusNamespaceName'), concat('/', 'default'))]",
    },
    "resources": [
      {
        "apiVersion": "2018-01-01-preview",
        "name": "[parameters('servicebusNamespaceName')]",
        "type": "Microsoft.ServiceBus/namespaces",
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
        "type": "Microsoft.ServiceBus/namespaces/networkruleset",
        "dependsOn": [
          "[concat('Microsoft.ServiceBus/namespaces/', parameters('servicebusNamespaceName'))]"
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

Virtuele netwerken, Zie de volgende koppeling voor beperken de toegang tot Azure Service Bus:

- [Virtual Network-Service-eindpunten voor Servicebus][lnk-vnet]

<!-- Links -->

[lnk-deploy]: ../azure-resource-manager/resource-group-template-deploy.md
[lnk-vnet]: service-bus-service-endpoints.md
[express-route]:  /azure/expressroute/expressroute-faqs#supported-services
