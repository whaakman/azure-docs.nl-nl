---
title: Virtual Network-service-eindpunten en regels voor Azure Event Hubs | Microsoft Docs
description: Een service-eindpunt voor Microsoft.EventHub toevoegen aan een virtueel netwerk.
services: event-hubs
documentationcenter: ''
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.date: 08/16/2018
ms.author: shvija
ms.openlocfilehash: 29b5f877065029dc271e49c1afd6d547def58a6e
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/18/2018
ms.locfileid: "49408129"
---
# <a name="use-virtual-network-service-endpoints-with-azure-event-hubs"></a>Service-eindpunten voor Virtueelnetwerk gebruiken met Azure Event Hubs

De integratie van Event Hubs met [Service-eindpunten voor Virtueelnetwerk (VNet)] [ vnet-sep] kunt veilige toegang tot messaging mogelijkheden van werkbelastingen, zoals virtuele machines die zijn gekoppeld aan virtuele netwerken, met het netwerkpad verkeer aan beide uiteinden worden beveiligd. 

> [!IMPORTANT]
> Virtuele netwerken worden ondersteund **standard** en **toegewezen** lagen van Event Hubs. Het wordt niet ondersteund in de basic-laag. 

Eenmaal is geconfigureerd om te worden gekoppeld aan ten minste één virtueel netwerk subnet service-eindpunt, wordt de respectieve Event Hubs-naamruimte niet meer accepteert verkeer vanaf elke locatie maar geautoriseerd subnetten in virtuele netwerken. Vanuit het perspectief virtueel netwerk met de binding van een Event Hubs-naamruimte met een service-eindpunt configureert een geïsoleerde netwerken tunnel vanuit het subnet van het virtuele netwerk naar de messaging-service.

Het resultaat is een privé- en geïsoleerd relatie tussen de werkbelastingen die zijn gebonden aan het subnet en de respectieve Event Hubs-naamruimte, ondanks het waarneembare netwerkadres van de berichten service eindpunt wordt in een openbare IP-adresbereik.

## <a name="advanced-security-scenarios-enabled-by-vnet-integration"></a>Geavanceerde beveiliging mogelijke scenario's met VNet-integratie 

Oplossingen die voorziet in een hechte en compartmentalized beveiliging vereisen, en waarbij virtuele subnetten hardwareoplossing tussen de compartmentalized services, moeten doorgaans nog steeds communicatiepaden tussen services die zich bevinden in deze secties.

Een direct IP-route tussen de afdelingen, inclusief de uitvoering van HTTPS via TCP/IP, voert u het risico van misbruik van zwakke plekken van de netwerklaag op omhoog. Messaging-services bieden een volledig geïsoleerd communicatiepaden, waar berichten ook naar de schijf als ze worden overgedragen tussen de partijen worden geschreven. Workloads in twee verschillende virtuele netwerken die zijn beide gekoppeld aan hetzelfde exemplaar van de Event Hubs kunnen communiceren efficiënt en betrouwbaar via berichten, terwijl de respectieve netwerk isolatie grens integriteit behouden blijft.
 
Dit betekent dat de beveiliging van uw gevoelige cloudoplossingen niet alleen toegang krijgen tot toonaangevende betrouwbare en schaalbare asynchrone messaging mogelijkheden van Azure, maar ze kunnen nu gebruiken messaging communicatiepaden tussen veilige oplossing maken die reizigerscompartimenten zijn inherent veiliger is dan wat met een peer-to-peer communicatiemodus is, met inbegrip van HTTPS en andere TLS beveiligde socket-protocollen.

## <a name="bind-event-hubs-to-virtual-networks"></a>Eventhubs koppelen aan virtuele netwerken

*Regels voor virtueel netwerk* zijn van de firewall beveiligingsfunctie die bepaalt of de Azure Event Hubs-naamruimte verbindingen van het subnet van een bepaalde virtuele netwerk aanvaardt.

Een Event Hubs-naamruimte binden aan een virtueel netwerk is een proces in twee stappen. U moet eerst maken een **service-eindpunt voor Virtueelnetwerk** op een Virtueelnetwerk, subnet en inschakelen voor "Microsoft.EventHub" als beschreven in de [overzicht van service-eindpunt] [ vnet-sep]. Nadat u het service-eindpunt hebt toegevoegd, verbindt u de Event Hubs-naamruimte toe met een *regel voor virtuele netwerken*.

De regel voor virtuele netwerken is een benoemde koppeling van de Event Hubs-naamruimte met een subnet van een virtueel netwerk. Terwijl de regel bestaat, worden alle werkbelastingen die zijn gekoppeld aan het subnet toegang tot de Event Hubs-naamruimte toegekend. Eventhubs zelf nooit uitgaande verbindingen maakt, heeft niet nodig om toegang te krijgen en is daarom nooit toegang verleend tot uw subnet door in te schakelen met deze regel.

### <a name="create-a-virtual-network-rule-with-azure-resource-manager-templates"></a>Een regel voor virtuele netwerken maken met Azure Resource Manager-sjablonen

De volgende Resource Manager-sjabloon kunt een regel voor virtuele netwerken toe te voegen aan een bestaande Event Hubs-naamruimte.

Sjabloonparameters:

* **namespaceName**: Event Hubs-naamruimte.
* **vnetRuleName**: naam voor de regel van het Virtueelnetwerk moet worden gemaakt.
* **virtualNetworkingSubnetId**: volledig gekwalificeerde pad van de Resource Manager voor het subnet van het virtuele netwerk; bijvoorbeeld `subscriptions/{id}/resourceGroups/{rg}/providers/Microsoft.Network/virtualNetworks/{vnet}/subnets/default` voor de standaard-subnet van een virtueel netwerk.

```json
{  
   "$schema":"http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
   "contentVersion":"1.0.0.0",
   "parameters":{     
          "namespaceName":{  
             "type":"string",
             "metadata":{  
                "description":"Name of the namespace"
             }
          },
          "vnetRuleName":{  
             "type":"string",
             "metadata":{  
                "description":"Name of the Authorization rule"
             }
          },
          "virtualNetworkSubnetId":{  
             "type":"string",
             "metadata":{  
                "description":"subnet Azure Resource Manager ID"
             }
          }
      },
    "resources": [
        {
            "apiVersion": "2018-01-01-preview",
            "name": "[concat(parameters('namespaceName'), '/', parameters('vnetRuleName'))]",
            "type":"Microsoft.EventHub/namespaces/VirtualNetworkRules",         
            "properties": {             
                "virtualNetworkSubnetId": "[parameters('virtualNetworkSubnetId')]"  
            }
        } 
    ]
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