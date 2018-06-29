---
title: Azure Service Bus-IP-verbindingsfilters | Microsoft Docs
description: Het gebruik van IP-filtering om verbindingen te blokkeren van specifieke IP-adressen aan Azure Service Bus.
services: service-bus
documentationcenter: ''
author: clemensv
manager: timlt
ms.service: service-bus
ms.devlang: na
ms.topic: article
ms.date: 06/26/2018
ms.author: clemensv
ms.openlocfilehash: e009bb9120fafc6edf60b68fab3336b9d1add507
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37035905"
---
# <a name="use-ip-filters"></a>IP-filters gebruiken

Voor scenario's waarin Azure Service Bus alleen toegankelijk zijn van bepaalde bekende sites is, de *IP-filter* functie kunt u regels voor weigeren of verkeer die afkomstig zijn van specifieke IPv4-adressen te configureren. Bijvoorbeeld deze adressen mogelijk zijn de eigenschappen van een zakelijke NAT-gateway.

## <a name="when-to-use"></a>Wanneer gebruikt u dit?

Er zijn twee specifieke gevallen waarin het is handig voor het blokkeren van Service Bus-eindpunten voor bepaalde IP-adressen:

- Service Bus moet verkeer alleen ontvangen van een opgegeven IP-adressen en alle andere afwijzen. Bijvoorbeeld, het gebruik van Service Bus met [Azure Express Route] [ express-route] particuliere verbindingen met uw on-premises infrastructuur te maken.
- U moet verkeer van IP-adressen die als verdacht zijn geïdentificeerd door de beheerder van de Service Bus afwijzen.

## <a name="how-filter-rules-are-applied"></a>Hoe filterregels worden toegepast

De IP-filter-regels worden toegepast op het niveau van de Service Bus-naamruimte. Daarom de regels van toepassing op alle verbindingen van clients met behulp van een ondersteund protocol.

Elke verbindingspoging van een IP-adres dat overeenkomt met die een rejecting IP-regel op de Servicebus-naamruimte wordt geweigerd als onbevoegde. De IP-regel niet wordt vermeld in het antwoord.

## <a name="default-setting"></a>Standaardinstelling

Standaard de **IP-Filter** raster in de portal voor Service Bus is leeg. Deze instelling betekent dat uw naamruimte verbindingen elk IP-adres aanvaardt. Deze instelling komt overeen met een regel die de 0.0.0.0/0 IP-adresbereik accepteert.

## <a name="ip-filter-rule-evaluation"></a>IP-filter Regeltoepassing

IP-filterregels in volgorde worden toegepast en de eerste regel die overeenkomt met het IP-adres bepaalt de actie accepteren of weigeren.

Als u wilt adressen in het bereik 70.37.104.0/24 accepteren en weigeren alle andere, moet de eerste regel in het raster het adresbereik 70.37.104.0/24 accepteren. De volgende regel moet alle adressen weigeren met behulp van de 0.0.0.0/0 bereik.

> [!NOTE]
> IP-adressen weigeren kunt voorkomen dat andere Azure-services (zoals Azure Stream Analytics, Azure Virtual Machines of de Explorer-apparaat in de portal) interactie met Service Bus.

### <a name="creating-a-virtual-network-rule-with-azure-resource-manager-templates"></a>De regel van een virtueel netwerk maken met Azure Resource Manager-sjablonen

De volgende Resource Manager-sjabloon kunt een regel van het virtuele netwerk toe te voegen aan een bestaande Service Bus-naamruimte.

Sjabloonparameters:

- **ipFilterRuleName** moet een unieke, niet-hoofdlettergevoelige alfanumerieke tekenreeks van maximaal 128 tekens lang zijn.
- **ipFilterAction** is **afwijzen** of **accepteren** als de actie voor de IP-filter-regel van toepassing.
- **ipMask** is één IPv4-adres of een blok IP-adressen in CIDR-notatie. Bijvoorbeeld, in CIDR vertegenwoordigt notatie 70.37.104.0/24 de 256 IPv4-adressen van 70.37.104.0 tot 70.37.104.255 met 24 die wijzen op het aantal bits aanzienlijke voorvoegsel voor het bereik.

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
          "ipFilterRuleName":{  
             "type":"string",
             "metadata":{  
                "description":"Name of the Authorization rule"
             }
          },
          "ipFilterAction":{  
             "type":"string",
             "allowedValues": ["Reject", "Accept"],
             "metadata":{  
                "description":"IP Filter Action"
             }
          },
          "IpMask":{  
             "type":"string",
             "metadata":{  
                "description":"IP Mask"
             }
          }
      },
    "resources": [
        {
            "apiVersion": "2018-01-01-preview",
            "name": "[concat(parameters('namespaceName'), '/', parameters('ipFilterRuleName'))]",
            "type": "Microsoft.ServiceBus/Namespaces/IPFilterRules",
            "properties": {
                "FilterName":"[parameters('ipFilterRuleName')]",
                "Action":"[parameters('ipFilterAction')]",              
                "IpMask": "[parameters('IpMask')]"
            }
        } 
    ]
}
```

Volg de instructies voor het als sjabloon wilt implementeren, [Azure Resource Manager][lnk-deploy].

## <a name="next-steps"></a>Volgende stappen

Virtuele netwerken, Zie de volgende koppeling voor beperkingsfacet toegang tot Azure Service Bus:

- [Virtual Network Service-eindpunten voor Servicebus][lnk-vnet]

<!-- Links -->

[lnk-deploy]: ../azure-resource-manager/resource-group-template-deploy.md
[lnk-vnet]: service-bus-service-endpoints.md
[express-route]:  /azure/expressroute/expressroute-faqs#supported-services