---
title: Virtueel netwerk service-eindpunten en regels voor Azure Event Hubs | Microsoft Docs
description: Een service-eindpunt Microsoft.ServiceBus toevoegen aan een virtueel netwerk.
services: event-hubs
documentationcenter: ''
author: clemensv
manager: timlt
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.date: 06/26/2018
ms.author: clemensv
ms.openlocfilehash: a23e5414cd3c60192badfee65b14c49cd5e96f4e
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37035910"
---
# <a name="use-virtual-network-service-endpoints-with-azure-event-hubs"></a>Service-eindpunten voor virtueel netwerk gebruiken met Azure Event Hubs

De integratie van Event Hubs met [virtueel netwerk (VNet) Service-eindpunten] [ vnet-sep] Hiermee schakelt u veilige toegang tot messaging mogelijkheden uit werkbelastingen, zoals virtuele machines die zijn gebonden aan virtuele netwerken met het netwerkpad verkeer aan beide uiteinden worden beveiligd. 

Eenmaal worden gekoppeld aan ten minste één virtueel netwerk subnet service-eindpunt is geconfigureerd, wordt de respectieve Event Hubs-naamruimte niet meer verkeer vanaf elke locatie accepteert maar geautoriseerd virtuele netwerken. Vanuit het perspectief van virtueel netwerk wordt een naamruimte Event Hubs binden aan een service-eindpunt configureert u een geïsoleerde netwerken tunnel van het subnet van het virtuele netwerk met de messaging-service.

Het resultaat is een privé en geïsoleerd relatie tussen de werkbelastingen die zijn gebonden aan het subnet en de betreffende naamruimte Event Hubs, altijd een beroep het waarneembare netwerkadres van het messaging service eindpunt wordt in een openbare IP-adresbereik.

## <a name="advanced-security-scenarios-enabled-by-vnet-integration"></a>Geavanceerde beveiliging mogelijke scenario's met VNet-integratie 

Oplossingen die dicht en compartmentalized beveiliging vereisen en waarin virtueel netwerksubnetten opgeven voor de segmentering tussen de compartmentalized services, moeten doorgaans nog steeds communicatiepaden tussen services die zich in deze secties.

Alle direct IP-route tussen de afdelingen, inclusief de uitvoering van HTTPS via TCP/IP, wordt het risico op misbruik van zwakke plekken die vanaf het netwerk op omhoog. Messaging-services bieden volledig geïsoleerd communicatiepaden, waar berichten zelfs worden geschreven naar de schijf als ze worden overgedragen tussen partijen. Werkbelastingen in twee afzonderlijke virtuele netwerken die zijn beide gekoppeld aan hetzelfde exemplaar van Event Hubs kunnen communiceren efficiënt en betrouwbaar via berichten, terwijl de respectieve netwerk isolatie grens integriteit blijft behouden.
 
Dit betekent dat de beveiliging van uw gevoelige cloudoplossingen niet alleen toegang te krijgen tot de toonaangevende betrouwbaar en schaalbaar asynchrone messaging mogelijkheden van Azure, maar ze kunnen nu gebruiken messaging communicatiepaden tussen veilige oplossing maken die reizigerscompartimenten zijn veiliger dan haalbare met een peer-to-peer communicatiemodus, met inbegrip van HTTPS en andere protocollen socket TLS beveiligd is.

## <a name="bind-event-hubs-to-virtual-networks"></a>Event Hubs binden aan virtuele netwerken

*Virtueel netwerk regels* zijn van de functie van de firewall-beveiliging die bepaalt of de server van uw Azure Event Hubs verbindingen van een bepaald virtueel netwerksubnet aanvaardt.

Een naamruimte Event Hubs binden aan een virtueel netwerk is een proces. U moet eerst maken een **Virtual Network service-eindpunt** op een virtueel netwerksubnet en inschakelen voor "Microsoft.ServiceBus' als beschreven in de [overzicht van de service-eindpunt] [ vnet-sep]. Nadat u het service-eindpunt hebt toegevoegd, u de naamruimte van Event Hubs binden aan deze met een *virtueel netwerk regel*.

De regel van het virtuele netwerk is een benoemde koppeling van de Event Hubs-naamruimte met een virtueel netwerksubnet. Terwijl de regel bestaat, krijgen alle werkbelastingen die zijn gebonden aan het subnet toegang tot de Event Hubs-naamruimte. Event Hubs zelf nooit uitgaande verbindingen vaststelt, heeft geen nodig om toegang te krijgen en daarom nooit krijgt toegang tot uw subnet doordat deze regel.

### <a name="create-a-virtual-network-rule-with-azure-resource-manager-templates"></a>Een regel virtueel netwerk maken met Azure Resource Manager-sjablonen

De volgende Resource Manager-sjabloon kunt een regel van het virtuele netwerk toe te voegen aan een bestaande Event Hubs-naamruimte.

Sjabloonparameters:

* **namespaceName**: Event Hubs-naamruimte.
* **vnetRuleName**: naam voor de regel van het virtuele netwerk moet worden gemaakt.
* **virtualNetworkingSubnetId**: volledig gekwalificeerde Resource Manager-pad voor het subnet van het virtuele netwerk; bijvoorbeeld `subscriptions/{id}/resourceGroups/{rg}/providers/Microsoft.Network/virtualNetworks/{vnet}/subnets/default` voor het subnet standaard van een virtueel netwerk.

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

Volg de instructies voor het als sjabloon wilt implementeren, [Azure Resource Manager][lnk-deploy].

## <a name="next-steps"></a>Volgende stappen

Zie de volgende koppelingen voor meer informatie over virtuele netwerken:

- [Virtueel netwerk van Azure service-eindpunten][vnet-sep]
- [Azure Event Hubs-IP-filtering][ip-filtering]

[vnet-sep]: ../virtual-network/virtual-network-service-endpoints-overview.md
[lnk-deploy]: ../azure-resource-manager/resource-group-template-deploy.md
[ip-filtering]: event-hubs-ip-filtering.md