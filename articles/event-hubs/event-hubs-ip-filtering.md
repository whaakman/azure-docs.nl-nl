---
title: Toegang beperken met IP-filters - Azure Event Hubs | Microsoft Docs
description: Gebruik van IP-filtering om verbindingen te blokkeren van bepaalde IP-adressen naar Azure Event Hubs.
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
ms.openlocfilehash: d21bf32ce804d2c8f6177eb7f789474bc41c9733
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2018
ms.locfileid: "53087504"
---
# <a name="restrict-access-to-azure-event-hubs-using-ip-filters"></a>Beperken van toegang tot Azure Event Hubs met behulp van IP-filters
Voor scenario's waarin Azure Event Hubs toegankelijk alleen van bepaalde bekende sites zijn moet, de *IP-filter* functie kunt u regels voor weigeren of verkeer dat afkomstig is van de specifieke IPv4-adressen te configureren. Bijvoorbeeld, kunnen deze adressen die van een zakelijke NAT-gateway zijn.

## <a name="when-to-use"></a>Wanneer gebruikt u dit?

Twee belangrijke gevallen waarin het is nuttig om het blokkeren van Event Hubs voor bepaalde IP-adressen als volgt zijn gebruiken:

- Uw eventhubs moeten ontvangen verkeer alleen vanaf een opgegeven bereik van IP-adressen en alle andere afwijzen. Bijvoorbeeld, gebruikt u Event Hubs met [Azure Express Route] [ express-route] particuliere verbindingen met uw on-premises infrastructuur te maken. 
- U moet verkeer van IP-adressen die zijn geïdentificeerd als verdacht door de beheerder van de Event Hubs afwijzen.

## <a name="how-filter-rules-are-applied"></a>Hoe regels worden toegepast

De IP-filterregels worden toegepast op het niveau van de Event Hubs-naamruimte. Daarom de regels van toepassing op alle verbindingen van clients met behulp van een ondersteund protocol.

Elke verbindingspoging vanaf een IP-adres dat overeenkomt met die een rejecting IP-regel op de Eventhubs-naamruimte wordt geweigerd als niet-geautoriseerde. Het antwoord wordt niet vermeld voor de IP-regel.

## <a name="default-setting"></a>Standaardinstelling

Standaard de **IP-Filter** raster in de portal voor Event Hubs is leeg. Deze instelling betekent dat uw event hub verbindingen van elk IP-adres aanvaardt. Deze instelling is gelijk aan een regel waarmee het 0.0.0.0/0 IP-adresbereik accepteert.

## <a name="ip-filter-rule-evaluation"></a>Evaluatie van IP-filter

IP-filterregels worden toegepast in volgorde en de eerste regel die overeenkomt met het IP-adres bepaalt de actie accepteren of weigeren.

Als u adressen in het bereik 70.37.104.0/24 accepteren en weigeren alle andere wilt, moet de eerste regel in het raster bijvoorbeeld, het adresbereik 70.37.104.0/24 accepteren. De volgende regel moet alle adressen weigeren met behulp van het adresbereik 0.0.0.0/0.

> [!NOTE]
> IP-adressen weigeren kunt voorkomen dat andere Azure-services (zoals Azure Stream Analytics, Azure Virtual Machines of het Device Explorer in de portal) interactie met Event Hubs.

### <a name="creating-an-ip-filter-rule-with-azure-resource-manager-templates"></a>Het maken van een IP-filter rule met Azure Resource Manager-sjablonen

> [!IMPORTANT]
> Virtuele netwerken worden ondersteund **standard** en **toegewezen** lagen van Event Hubs. Het wordt niet ondersteund in de basic-laag. 

De volgende Resource Manager-sjabloon kunt een regel voor IP-filter toe te voegen aan een bestaande Event Hubs-naamruimte.

Sjabloonparameters:

- **ipFilterRuleName** moet een unieke, niet-hoofdlettergevoelig, alfanumerieke tekenreeks, maximaal 128 tekens lang zijn.
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

Volg de instructies voor het implementeren van de sjabloon, [Azure Resource Manager][lnk-deploy].

## <a name="next-steps"></a>Volgende stappen

Virtuele netwerken, Zie de volgende koppeling voor beperken de toegang tot Event Hubs naar Azure:

- [Virtual Network-Service-eindpunten voor Eventhubs][lnk-vnet]

<!-- Links -->

[express-route]:  /azure/expressroute/expressroute-faqs#supported-services
[lnk-deploy]: ../azure-resource-manager/resource-group-template-deploy.md
[lnk-vnet]: event-hubs-service-endpoints.md