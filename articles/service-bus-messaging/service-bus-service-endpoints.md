---
title: Virtual Network-service-eindpunten en regels voor Azure Service Bus | Microsoft Docs
description: Een service-eindpunt voor Microsoft.ServiceBus toevoegen aan een virtueel netwerk.
services: event-hubs
documentationcenter: ''
author: clemensv
manager: timlt
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.date: 09/05/2018
ms.author: clemensv
ms.openlocfilehash: 05930dfce64378d792213ccaefa3d15057bd5dfd
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47404994"
---
# <a name="use-virtual-network-service-endpoints-with-azure-service-bus"></a>Service-eindpunten voor Virtueelnetwerk gebruiken met Azure Service Bus

De integratie van Service Bus met [service-eindpunten voor Virtueelnetwerk (VNet)] [ vnet-sep] kunt veilige toegang tot berichten mogelijkheden van workloads, zoals virtuele machines die zijn gekoppeld aan virtuele netwerken , met het netwerkpad verkeer aan beide uiteinden worden beveiligd. 

Eenmaal is geconfigureerd om te worden gekoppeld aan ten minste één virtueel netwerk subnet service-eindpunt, wordt de betreffende Service Bus-naamruimte niet meer verkeer vanaf elke locatie accepteert maar geautoriseerd virtuele netwerken. Vanuit het perspectief van het virtuele netwerk, een Service Bus-naamruimte binden aan een service-eindpunt configureert u een geïsoleerde netwerken tunnel vanuit het subnet van het virtuele netwerk naar de messaging-service.

Het resultaat is een privé- en geïsoleerd relatie tussen de werkbelastingen die zijn gebonden aan het subnet en de bijbehorende Service Bus-naamruimte, ondanks het waarneembare netwerkadres van de berichten service eindpunt wordt in een openbare IP-adresbereik.

## <a name="enable-service-endpoints-with-service-bus"></a>Met Service Bus-service-eindpunten inschakelen

Virtuele netwerken worden alleen ondersteund in [Premium-laag](service-bus-premium-messaging.md) Service Bus-naamruimten. 

Een belangrijk aandachtspunt bij het gebruik van VNet-service-eindpunten met Service Bus is dat u deze eindpunten in toepassingen die elkaar Standard en Premium-laag Service Bus-naamruimten niet moet inschakelen. Omdat de Standard-laag biedt geen ondersteuning voor vnet's, is het eindpunt is beperkt tot alleen Premium-laag-naamruimten. Het VNet wordt geblokkeerd verkeer naar de Standard-naamruimte. 

## <a name="advanced-security-scenarios-enabled-by-vnet-integration"></a>Geavanceerde beveiliging mogelijke scenario's met VNet-integratie 

Oplossingen die voorziet in een hechte en compartmentalized beveiliging vereisen, en waarbij virtuele subnetten hardwareoplossing tussen de compartmentalized services, moeten doorgaans nog steeds communicatiepaden tussen services die zich bevinden in deze secties.

Een direct IP-route tussen de afdelingen, inclusief de uitvoering van HTTPS via TCP/IP, voert u het risico van misbruik van zwakke plekken van de netwerklaag op omhoog. Messaging-services bieden een volledig geïsoleerd communicatiepaden, waar berichten ook naar de schijf als ze worden overgedragen tussen de partijen worden geschreven. Workloads in twee verschillende virtuele netwerken die zijn beide gekoppeld aan hetzelfde exemplaar van de Service Bus kunnen communiceren efficiënt en betrouwbaar via berichten, terwijl de respectieve netwerk isolatie grens integriteit behouden blijft.
 
Dit betekent dat de beveiliging van uw gevoelige cloudoplossingen niet alleen toegang krijgen tot toonaangevende betrouwbare en schaalbare asynchrone messaging mogelijkheden van Azure, maar ze kunnen nu gebruiken messaging communicatiepaden tussen veilige oplossing maken die reizigerscompartimenten zijn inherent veiliger is dan wat met een peer-to-peer communicatiemodus is, met inbegrip van HTTPS en andere TLS beveiligde socket-protocollen.

## <a name="binding-service-bus-to-virtual-networks"></a>Servicebus binding met een virtueel netwerk

*Regels voor virtueel netwerk* zijn van de firewall beveiligingsfunctie die bepaalt of de server van uw Azure Service Bus-verbindingen van een bepaald virtueel netwerksubnet accepteert.

Een Service Bus-naamruimte binden aan een virtueel netwerk is een proces in twee stappen. U moet eerst maken een **service-eindpunt voor Virtueelnetwerk** op een Virtueelnetwerk, subnet en inschakelen voor "Microsoft.ServiceBus" als beschreven in de [overzicht van service-eindpunt] [ vnet-sep]. Nadat u het service-eindpunt hebt toegevoegd, verbindt u de Service Bus-naamruimte toe met een *regel voor virtuele netwerken*.

De regel voor virtuele netwerken is een benoemde koppeling van de Service Bus-naamruimte met een subnet van een virtueel netwerk. Terwijl de regel bestaat, worden alle werkbelastingen die zijn gekoppeld aan het subnet toegang tot de Service Bus-naamruimte toegekend. Servicebus zelf nooit uitgaande verbindingen maakt, heeft niet nodig om toegang te krijgen, en is daarom nooit toegang verleend tot uw subnet door in te schakelen met deze regel.

### <a name="creating-a-virtual-network-rule-with-azure-resource-manager-templates"></a>Het maken van een regel voor virtuele netwerken met Azure Resource Manager-sjablonen

De volgende Resource Manager-sjabloon kunt een regel voor virtuele netwerken toe te voegen aan een bestaande Service Bus-naamruimte.

Sjabloonparameters:

* **namespaceName**: Service Bus-naamruimte.
* **vnetRuleName**: naam voor de regel van het Virtueelnetwerk moet worden gemaakt.
* **virtualNetworkingSubnetId**: volledig gekwalificeerde pad van de Resource Manager voor het subnet van het virtuele netwerk; bijvoorbeeld `/subscriptions/{id}/resourceGroups/{rg}/providers/Microsoft.Network/virtualNetworks/{vnet}/subnets/default` voor de standaard-subnet van een virtueel netwerk.

Sjabloon:

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
            "type":"Microsoft.ServiceBus/namespaces/VirtualNetworkRules",           
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
- [Azure Service Bus-IP-filtering][ip-filtering]

[vnet-sep]: ../virtual-network/virtual-network-service-endpoints-overview.md
[lnk-deploy]: ../azure-resource-manager/resource-group-template-deploy.md
[ip-filtering]: service-bus-ip-filtering.md