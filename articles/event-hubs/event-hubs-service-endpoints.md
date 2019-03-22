---
title: Virtual Network-service-eindpunten - Azure Event Hubs | Microsoft Docs
description: In dit artikel bevat informatie over hoe u een service-eindpunt voor Microsoft.EventHub toevoegt aan een virtueel netwerk.
services: event-hubs
documentationcenter: ''
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.date: 03/12/2019
ms.author: shvija
ms.openlocfilehash: 7b5a62f81238d1ae2b627c395613066350b36efe
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57887592"
---
# <a name="use-virtual-network-service-endpoints-with-azure-event-hubs"></a>Service-eindpunten voor Virtueelnetwerk gebruiken met Azure Event Hubs

De integratie van Event Hubs met [Service-eindpunten voor Virtueelnetwerk (VNet)] [ vnet-sep] kunt veilige toegang tot messaging mogelijkheden van werkbelastingen, zoals virtuele machines die zijn gekoppeld aan virtuele netwerken, met het netwerkpad verkeer aan beide uiteinden worden beveiligd.

Eenmaal is geconfigureerd voor het gebonden aan ten minste één virtueel netwerk subnet service-eindpunt, wordt de respectieve Event Hubs-naamruimte niet meer accepteert verkeer vanaf elke locatie maar geautoriseerd subnetten in virtuele netwerken. Vanuit het perspectief virtueel netwerk met de binding van een Event Hubs-naamruimte met een service-eindpunt configureert een geïsoleerde netwerken tunnel vanuit het subnet van het virtuele netwerk naar de messaging-service. 

Het resultaat is een privé- en geïsoleerd relatie tussen de werkbelastingen die zijn gebonden aan het subnet en de respectieve Event Hubs-naamruimte, ondanks het waarneembare netwerkadres van de berichten service eindpunt wordt in een openbare IP-adresbereik. Er is een uitzondering op dit gedrag. Inschakelen van een service-eindpunt, standaard, kunt u de regel denyall in de IP-firewall die zijn gekoppeld aan het virtuele netwerk. U kunt specifieke IP-adressen toevoegen in de IP-firewall voor toegang tot het openbare eindpunt van de Event Hub. 


>[!WARNING]
> De implementatie van de integratie van virtuele netwerken kan voorkomen dat andere Azure-services interactie hebben met Event Hubs.
>
> Vertrouwde Microsoft-services worden niet ondersteund wanneer virtuele netwerken worden geïmplementeerd.
>
> Algemene Azure-scenario's die niet met virtuele netwerken werken (Let op: de lijst is **niet** volledig)-
> - Azure Monitor
> - Azure Stream Analytics
> - Integratie met Azure Event Grid
> - Azure IoT Hub Routes
> - Azure IoT Device Explorer
> - Azure Data Explorer
>
> De onderstaande Microsoft services vereist zijn om te worden in een virtueel netwerk
> - Azure Web Apps
> - Azure Functions

> [!IMPORTANT]
> Virtuele netwerken worden ondersteund in de lagen **Standard** en **Dedicated** van Event Hubs. Deze worden niet ondersteund in de Basic-laag.

## <a name="advanced-security-scenarios-enabled-by-vnet-integration"></a>Geavanceerde beveiliging mogelijke scenario's met VNet-integratie 

Oplossingen die voorziet in een hechte en compartmentalized beveiliging vereisen, en waarbij virtuele subnetten hardwareoplossing tussen de compartmentalized services, moeten nog steeds communicatiepaden tussen services die zich bevinden in die ruimten.

Een direct IP-route tussen de afdelingen, inclusief de uitvoering van HTTPS via TCP/IP, voert u het risico van misbruik van zwakke plekken van de netwerklaag op omhoog. Messaging-services bieden een volledig geïsoleerd communicatiepaden, waar berichten ook naar de schijf als ze worden overgedragen tussen de partijen worden geschreven. Workloads in twee verschillende virtuele netwerken die zijn beide gekoppeld aan hetzelfde exemplaar van de Event Hubs kunnen communiceren efficiënt en betrouwbaar via berichten, terwijl de respectieve netwerk isolatie grens integriteit behouden blijft.
 
Dit betekent dat de beveiliging van uw gevoelige cloudoplossingen niet alleen toegang krijgen tot toonaangevende betrouwbare en schaalbare asynchrone messaging mogelijkheden van Azure, maar ze kunnen nu gebruiken messaging communicatiepaden tussen veilige oplossing maken die reizigerscompartimenten zijn inherent veiliger is dan wat met een peer-to-peer communicatiemodus is, met inbegrip van HTTPS en andere TLS beveiligde socket-protocollen.

## <a name="bind-event-hubs-to-virtual-networks"></a>Eventhubs koppelen aan virtuele netwerken

*Regels voor virtueel netwerk* zijn van de firewall beveiligingsfunctie die bepaalt of de Azure Event Hubs-naamruimte verbindingen van het subnet van een bepaalde virtuele netwerk aanvaardt.

Een Event Hubs-naamruimte binden aan een virtueel netwerk is een proces in twee stappen. U moet eerst maken een **service-eindpunt voor Virtueelnetwerk** op een Virtueelnetwerk, subnet en inschakelen voor "Microsoft.EventHub" als beschreven in de [overzicht van service-eindpunt] [ vnet-sep]. Nadat u het service-eindpunt hebt toegevoegd, verbindt u de Event Hubs-naamruimte toe met een *regel voor virtuele netwerken*.

De regel voor virtuele netwerken is een koppeling van de Event Hubs-naamruimte met een subnet van een virtueel netwerk. Terwijl de regel bestaat, worden alle werkbelastingen die zijn gekoppeld aan het subnet toegang tot de Event Hubs-naamruimte toegekend. Eventhubs zelf nooit uitgaande verbindingen maakt, heeft niet nodig om toegang te krijgen en is daarom nooit toegang verleend tot uw subnet door in te schakelen met deze regel.

### <a name="create-a-virtual-network-rule-with-azure-resource-manager-templates"></a>Een regel voor virtuele netwerken maken met Azure Resource Manager-sjablonen

De volgende Resource Manager-sjabloon kunt een regel voor virtuele netwerken toe te voegen aan een bestaande Event Hubs-naamruimte.

Sjabloonparameters:

* **namespaceName**: Event Hubs-naamruimte.
* **vnetRuleName**: Naam voor de regel voor virtuele netwerken worden gemaakt.
* **virtualNetworkingSubnetId**: Volledig gekwalificeerde pad van de Resource Manager voor het subnet van het virtuele netwerk; bijvoorbeeld, `subscriptions/{id}/resourceGroups/{rg}/providers/Microsoft.Network/virtualNetworks/{vnet}/subnets/default` voor de standaard-subnet van een virtueel netwerk.

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
      "virtualNetworkName": {
        "type": "string",
        "metadata": {
          "description": "Name of the Virtual Network Rule"
        }
      },
      "subnetName": {
        "type": "string",
        "metadata": {
          "description": "Name of the Virtual Network Sub Net"
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
      "subNetId": "[resourceId('Microsoft.Network/virtualNetworks/subnets/', parameters('virtualNetworkName'), parameters('subnetName'))]"
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
        "apiVersion": "2017-09-01",
        "name": "[parameters('virtualNetworkName')]",
        "location": "[parameters('location')]",
        "type": "Microsoft.Network/virtualNetworks",
        "properties": {
          "addressSpace": {
            "addressPrefixes": [
              "10.0.0.0/23"
            ]
          },
          "subnets": [
            {
              "name": "[parameters('subnetName')]",
              "properties": {
                "addressPrefix": "10.0.0.0/23",
                "serviceEndpoints": [
                  {
                    "service": "Microsoft.EventHub"
                  }
                ]
              }
            }
          ]
        }
      },
      {
        "apiVersion": "2018-01-01-preview",
        "name": "[variables('namespaceNetworkRuleSetName')]",
        "type": "Microsoft.EventHub/namespaces/networkruleset",
        "dependsOn": [
          "[concat('Microsoft.EventHub/namespaces/', parameters('eventhubNamespaceName'))]"
        ],
        "properties": {
          "virtualNetworkRules": 
          [
            {
              "subnet": {
                "id": "[variables('subNetId')]"
              },
              "ignoreMissingVnetServiceEndpoint": false
            }
          ],
          "ipRules":[<YOUR EXISTING IP RULES>],
          "defaultAction": "Deny"
        }
      }
    ],
    "outputs": { }
  }
```

Volg de instructies voor het implementeren van de sjabloon, [Azure Resource Manager][lnk-deploy].

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over virtuele netwerken, de volgende koppelingen:

- [Azure virtual network-service-eindpunten][vnet-sep]
- [Azure Event Hubs-IP-filtering][ip-filtering]

[vnet-sep]: ../virtual-network/virtual-network-service-endpoints-overview.md
[lnk-deploy]: ../azure-resource-manager/resource-group-template-deploy.md
[ip-filtering]: event-hubs-ip-filtering.md
