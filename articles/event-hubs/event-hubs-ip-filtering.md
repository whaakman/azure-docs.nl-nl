---
title: Azure Event Hubs-IP-verbindingsfilters | Microsoft Docs
description: Gebruik van IP-filtering om verbindingen te blokkeren van specifieke IP-adressen voor Azure Event Hubs.
services: event-hubs
documentationcenter: ''
author: clemensv
manager: timlt
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.date: 06/26/2018
ms.author: clemensv
ms.openlocfilehash: 425a5b641fbfd2e52e1294c6317b51ff2a584aa3
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37035917"
---
# <a name="use-ip-filters"></a>IP-filters gebruiken

Voor scenario's waarin Azure Event Hubs alleen toegankelijk zijn van bepaalde bekende sites is, de *IP-filter* functie kunt u regels voor weigeren of verkeer die afkomstig zijn van specifieke IPv4-adressen te configureren. Bijvoorbeeld deze adressen mogelijk zijn de eigenschappen van een zakelijke NAT-gateway.

## <a name="when-to-use"></a>Wanneer gebruikt u dit?

Twee belangrijke gevallen waarin het is handig voor het blokkeren van Event Hubs-eindpunten voor bepaalde IP-adressen als volgt zijn gebruiken:

- Uw event hubs moeten verkeer alleen ontvangen van een opgegeven IP-adressen en alle andere afwijzen. Bijvoorbeeld, het gebruik van Event Hubs met [Azure Express Route] [ express-route] particuliere verbindingen met uw on-premises infrastructuur te maken. 
- U moet verkeer van IP-adressen die door de beheerder van Event Hubs zijn geïdentificeerd als verdacht afwijzen.

## <a name="how-filter-rules-are-applied"></a>Hoe filterregels worden toegepast

De IP-filter-regels worden toegepast op het niveau van de naamruimte Event Hubs. Daarom de regels van toepassing op alle verbindingen van clients met behulp van een ondersteund protocol.

Elke verbindingspoging van een IP-adres dat overeenkomt met die een rejecting IP-regel op de naamruimte van Event Hubs wordt geweigerd als onbevoegde. De IP-regel niet wordt vermeld in het antwoord.

## <a name="default-setting"></a>Standaardinstelling

Standaard de **IP-Filter** raster in de portal voor Event Hubs is leeg. Deze instelling betekent dat uw event hub verbindingen van elk IP-adres aanvaardt. Deze instelling komt overeen met een regel die de 0.0.0.0/0 IP-adresbereik accepteert.

## <a name="ip-filter-rule-evaluation"></a>IP-filter Regeltoepassing

IP-filterregels in volgorde worden toegepast en de eerste regel die overeenkomt met het IP-adres bepaalt de actie accepteren of weigeren.

Als u wilt adressen in het bereik 70.37.104.0/24 accepteren en weigeren alle andere, moet de eerste regel in het raster het adresbereik 70.37.104.0/24 accepteren. De volgende regel moet alle adressen weigeren met behulp van de 0.0.0.0/0 bereik.

> [!NOTE]
> IP-adressen weigeren kunt voorkomen dat andere Azure-services (zoals Azure Stream Analytics, Azure Virtual Machines of de Explorer-apparaat in de portal) interactie met Event Hubs.

### <a name="creating-a-virtual-network-rule-with-azure-resource-manager-templates"></a>De regel van een virtueel netwerk maken met Azure Resource Manager-sjablonen

De volgende Resource Manager-sjabloon kunt een regel van het virtuele netwerk toe te voegen aan een bestaande Event Hubs-naamruimte.

Sjabloonparameters:

- **ipFilterRuleName** moet een unieke, niet-hoofdlettergevoelige, alfanumerieke tekenreeks op, maximaal 128 tekens lang zijn.
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
            "type": "Microsoft.EventHub/Namespaces/IPFilterRules",
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

Virtuele netwerken, Zie de volgende koppeling voor beperkingsfacet toegang tot de Event Hubs naar Azure:

- [Virtual Network Service-eindpunten voor Event Hubs][lnk-vnet]

<!-- Links -->

[express-route]:  /azure/expressroute/expressroute-faqs#supported-services
[lnk-deploy]: ../azure-resource-manager/resource-group-template-deploy.md
[lnk-vnet]: event-hubs-service-endpoints.md