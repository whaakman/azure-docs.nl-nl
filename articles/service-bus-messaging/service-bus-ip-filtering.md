---
title: Azure Service Bus-IP-verbindingsfilters | Microsoft Docs
description: Het gebruik van IP-filtering om verbindingen te blokkeren van bepaalde IP-adressen met Azure Service Bus.
services: service-bus
documentationcenter: ''
author: clemensv
manager: timlt
ms.service: service-bus
ms.devlang: na
ms.topic: article
ms.date: 09/26/2018
ms.author: clemensv
ms.openlocfilehash: ccb759a9151d734aa99a6f6b9c68e6072874dd84
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47394825"
---
# <a name="use-ip-filters"></a>IP-filters gebruiken

Voor scenario's waarin Azure Service Bus alleen toegankelijk zijn van bepaalde bekende sites is, de *IP-filter* functie kunt u regels voor weigeren of verkeer dat afkomstig is van de specifieke IPv4-adressen te configureren. Bijvoorbeeld, kunnen deze adressen die van een zakelijke NAT-gateway zijn.

## <a name="when-to-use"></a>Wanneer gebruikt u dit?

Er zijn twee specifieke gebruiksscenario's waarin het is nuttig voor het blokkeren van Service Bus-eindpunten voor bepaalde IP-adressen:

- Service Bus moet ontvangen verkeer alleen vanaf een opgegeven bereik van IP-adressen en alle andere afwijzen. Bijvoorbeeld, gebruikt u Service Bus met [Azure Express Route] [ express-route] particuliere verbindingen met uw on-premises infrastructuur te maken.
- U moet verkeer van IP-adressen die zijn geïdentificeerd als verdacht door de beheerder van de Service Bus afwijzen.

## <a name="how-filter-rules-are-applied"></a>Hoe regels worden toegepast

De IP-filterregels worden toegepast op het niveau van de Service Bus-naamruimte. Daarom de regels van toepassing op alle verbindingen van clients met behulp van een ondersteund protocol.

Elke verbindingspoging vanaf een IP-adres dat overeenkomt met die een rejecting IP-regel op de Servicebus-naamruimte wordt geweigerd als niet-geautoriseerde. Het antwoord wordt niet vermeld voor de IP-regel.

## <a name="default-setting"></a>Standaardinstelling

Standaard de **IP-Filter** raster in de portal voor Service Bus is leeg. Deze instelling betekent dat uw naamruimte verbindingen elk IP-adres aanvaardt. Deze instelling is gelijk aan een regel waarmee het 0.0.0.0/0 IP-adresbereik accepteert.

## <a name="ip-filter-rule-evaluation"></a>Evaluatie van IP-filter

IP-filterregels worden toegepast in volgorde en de eerste regel die overeenkomt met het IP-adres bepaalt de actie accepteren of weigeren.

Als u adressen in het bereik 70.37.104.0/24 accepteren en weigeren alle andere wilt, moet de eerste regel in het raster bijvoorbeeld, het adresbereik 70.37.104.0/24 accepteren. De volgende regel moet alle adressen weigeren met behulp van het adresbereik 0.0.0.0/0.

> [!NOTE]
> IP-adressen weigeren kunt voorkomen dat andere Azure-services (zoals Azure Stream Analytics, Azure Virtual Machines of het Device Explorer in de portal) interactie met Service Bus.

### <a name="creating-a-virtual-network-rule-with-azure-resource-manager-templates"></a>Het maken van een regel voor virtuele netwerken met Azure Resource Manager-sjablonen

De volgende Resource Manager-sjabloon kunt een regel voor virtuele netwerken toe te voegen aan een bestaande Service Bus-naamruimte.

Sjabloonparameters:

- **ipFilterRuleName** moet een unieke, niet-hoofdlettergevoelig, alfanumerieke tekenreeks maximaal 128 tekens lang zijn.
- **ipFilterAction** is **afwijzen** of **accepteren** als de actie om toe te passen voor de IP-filter rule.
- **ipMask** is één IPv4-adres of een blok IP-adressen in CIDR-notatie. Bijvoorbeeld, in CIDR vertegenwoordigt notatie 70.37.104.0/24 de 256 IPv4-adressen van 70.37.104.0 tot 70.37.104.255, met 24 uur per dag die wijzen op het aantal bits aanzienlijke voorvoegsel voor het bereik.

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

Volg de instructies voor het implementeren van de sjabloon, [Azure Resource Manager][lnk-deploy].

## <a name="next-steps"></a>Volgende stappen

Virtuele netwerken, Zie de volgende koppeling voor beperken de toegang tot Azure Service Bus:

- [Virtual Network-Service-eindpunten voor Servicebus][lnk-vnet]

<!-- Links -->

[lnk-deploy]: ../azure-resource-manager/resource-group-template-deploy.md
[lnk-vnet]: service-bus-service-endpoints.md
[express-route]:  /azure/expressroute/expressroute-faqs#supported-services