---
title: Configureren van een Gateway om aanvragen te sturen | Microsoft Docs
description: Informatie over het configureren van de gateway die verantwoordelijk is voor inkomend verkeer voor uw toepassingen die worden uitgevoerd op het net van Service Fabric.
services: service-fabric-mesh
documentationcenter: .net
author: dkkapur
manager: chakdan
editor: ''
ms.assetid: ''
ms.service: service-fabric-mesh
ms.devlang: azure-cli
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/28/2018
ms.author: dekapur
ms.custom: mvc, devcenter
ms.openlocfilehash: 1c75d781c8a83b54ac9474c83388cf02b5d03e3c
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57532079"
---
# <a name="configure-a-gateway-resource-to-route-requests"></a>Configureren van een Gateway-resource om aanvragen te sturen

Een Gateway-resource wordt gebruikt om inkomend verkeer omleiden naar het netwerk waarop uw toepassing. Configureren voor het opgeven van regels waarmee aanvragen worden doorgestuurd naar specifieke services of eindpunten op basis van de structuur van de aanvraag. Zie [Inleiding tot netwerken in Service Fabric NET](service-fabric-mesh-networks-and-gateways.md) voor meer informatie over netwerken en gateways in Mesh. 

Gateway-resources moeten worden gedeclareerd als onderdeel van uw sjabloon voor de implementatie (JSON of yaml) en zijn afhankelijk van een netwerkbron. Dit document bevat een overzicht van de verschillende eigenschappen die kunnen worden ingesteld voor uw gateway en bevat informatie over de configuratie van een voorbeeld-gateway.

## <a name="options-for-configuring-your-gateway-resource"></a>Opties voor het configureren van uw Gateway-resource

Omdat de resource Gateway als een brug tussen het netwerk van uw toepassing en de onderliggende infrastructuur fungeert (de `open` netwerk). U hoeft alleen een configureren (in de preview Mesh er geldt een limiet van één gateway per app). De declaratie voor de resource bestaat uit twee hoofdonderdelen: de eigenschappen en metagegevens van de resource. 

### <a name="gateway-resource-metadata"></a>De metagegevens van de gateway-resource

Een gateway is gedeclareerd met de volgende metagegevens:
* `apiVersion` -moet zijn ingesteld op '2018-09-01-preview' (of later, in de toekomst)
* `name` -een naam voor deze gateway
* `type` -"Microsoft.ServiceFabricMesh/gateways"
* `location` -moet worden ingesteld op de locatie van uw app / network; Meestal is een verwijzing naar de locatieparameter in uw implementatie
* `dependsOn` -het waarvoor deze gateway fungeert als een punt inkomend verkeer voor netwerk

Hier ziet u hoe het eruit ziet in een sjabloon voor de implementatie Azure Resource Manager (JSON): 

```json
{
  "apiVersion": "2018-09-01-preview",
  "name": "myGateway",
  "type": "Microsoft.ServiceFabricMesh/gateways",
  "location": "[parameters('location')]",
  "dependsOn": [
    "Microsoft.ServiceFabricMesh/networks/myNetwork"
  ],
  "properties": {
    [...]
  }
}
```

### <a name="gateway-properties"></a>Eigenschappen van de gateway

De sectie met eigenschappen wordt gebruikt om de netwerken waarvan de gateway veroorzaakt en de regels voor het routeren van aanvragen te definiëren. 

#### <a name="source-and-destination-network"></a>Bron- en netwerk 

Elke gateway vereist een `sourceNetwork` en `destinationNetwork`. Het bron-netwerk is gedefinieerd als het netwerk van waaruit uw app inkomende aanvragen worden ontvangen. De eigenschap name moet altijd worden ingesteld op 'Geopend'. Het doelnetwerk is het netwerk waarop de aanvragen zijn gericht. De waarde van de naam voor deze moet worden ingesteld op de naam van de resource van de lokale netwerk van uw app (volledige verwijzing naar de resource moet bevatten). Zie hieronder voor de configuratie van een voorbeeld van hoe dit voor een implementatie in een netwerk met de naam 'myNetwork eruitziet'.

```json 
"properties": {
  "description": "Service Fabric Mesh Gateway",
  "sourceNetwork": {
    "name": "Open"
  },
  "destinationNetwork": {
    "name": "[resourceId('Microsoft.ServiceFabricMesh/networks', 'myNetwork')]"
  },
  [...]
}
```

#### <a name="rules"></a>Regels 

Een gateway kan hebben meerdere Routering regels die hoe inkomend verkeer specificeren wordt verwerkt. Een regel voor doorsturen definieert u de relatie tussen de luisterende poort en het eindpunt van de bestemming voor een bepaalde toepassing. Er is een 1:1 toewijzing tussen poort: eindpunt voor TCP-regels voor doorsturen. Voor HTTP-regels voor doorsturen, kunt u complexere regels voor doorsturen die het pad van de aanvraag en eventueel headers, om te bepalen hoe de aanvraag wordt gerouteerd, onderzoekt instellen. 

Regels voor doorsturen zijn opgegeven op basis van per poort. Elke poort inkomend verkeer heeft een eigen reeks regels binnen het eigenschappengedeelte van de configuratie van uw gateway. 

#### <a name="tcp-routing-rules"></a>Regels voor het doorsturen van TCP 

Een regel voor het doorsturen van TCP bestaat uit de volgende eigenschappen: 
* `name` -verwijzing naar de regel die kan bestaan uit een tekenreeks van uw keuze 
* `port` -poort voor luisteren naar binnenkomende aanvragen 
* `destination` -endpoint-specificatie met `applicationName`, `serviceName`, en `endpointName`, voor waar de aanvragen worden doorgestuurd moeten naar

Hier volgt een voorbeeld van de regel voor het doorsturen van TCP:

```json
"properties": {
  [...]
  "tcp": [
    {
      "name": "web",
      "port": 80,
      "destination": {
        "applicationName": "myApp",
        "serviceName": "myService",
        "endpointName": "myListener"
      }
    }
  ]
}
```


#### <a name="http-routing-rules"></a>Regels voor het doorsturen van HTTP 

Een regel voor het doorsturen van HTTP-bestaat uit de volgende eigenschappen: 
* `name` -verwijzing naar de regel die kan bestaan uit een tekenreeks van uw keuze 
* `port` -poort voor luisteren naar binnenkomende aanvragen 
* `hosts` -een matrix van beleidsregels die betrekking hebben op aanvragen die afkomstig zijn voor de verschillende 'hosts' op de hierboven opgegeven poort. Hosts zijn de set toepassingen en services die kunnen worden uitgevoerd in het netwerk en inkomende aanvragen, dat wil zeggen een web-app kunnen fungeren. Host-beleidsregels worden geïnterpreteerd in volgorde, dus u kunt het volgende in aflopende niveaus van specifieke maken
    * `name` -de DNS-naam van de host waarop de volgende regels voor doorsturen zijn opgegeven. Met behulp van ' * ' hier maakt regels voor doorsturen voor alle hosts.
    * `routes` -een matrix van het beleid voor deze specifieke host
        * `match` -specificatie van de structuur van de binnenkomende aanvraag voor deze regel wilt toepassen, op basis van een `path`
            * `path` -bevat een `value` (binnenkomende URI), `rewrite` (zoals u wilt het verzoek om te worden doorgestuurd), en een `type` (op dit moment kunnen alleen worden "Het voorvoegsel")
            * `header` -is een optionele matrix met waarden die headers moeten overeenkomen met in de koptekst van de aanvraag dat als de aanvraag komt overeen met het opgegeven pad (hierboven).
              * elke vermelding bevat `name` (naam van de koptekst zodat deze overeenkomen met), `value` (string-waarde van de koptekst in de aanvraag), en een `type` (op dit moment kunnen alleen worden ingesteld op ' Exact")
        * `destination` -Als de aanvraag overeenkomt met, de verzameling wordt doorgestuurd naar deze bestemming, die is opgegeven met behulp van een `applicationName`, `serviceName`, en `endpointName`

Hier volgt een voorbeeld van de HTTP-regel voor doorsturen die van toepassing zijn op aanvragen die afkomstig zijn op poort 80, voor alle hosts die worden bediend door apps in dit netwerk. Als de aanvraag-URL heeft een structuur die het opgegeven pad, dat wil zeggen, overeenkomt met `<IPAddress>:80/pickme/<requestContent>`, wordt deze doorgestuurd naar de `myListener` eindpunt.  

```json
"properties": {
  [...]
  "http": [
    {
      "name": "web",
      "port": 80,
      "hosts": [
        {
          "name": "*",
          "routes": [
            {
              "match": {
                "path": {
                  "value": "/pickme",
                  "rewrite": "/",
                  "type": "Prefix"
                }
              },
              "destination": {
                "applicationName": "meshApp",
                "serviceName": "myService",
                "endpointName": "myListener"
              }
            }
          ]
        }
      ]
    }
  ]
}
```

## <a name="sample-config-for-a-gateway-resource"></a>Voorbeeld-configuratie voor een Gateway-resource 

Hier volgt een volledige resourceconfiguratie voor Gateway ziet eruit als (dit is een aanpassing van het voorbeeld inkomend beschikbaar in de [Mesh-voorbeelden opslagplaats](https://github.com/Azure-Samples/service-fabric-mesh/blob/2018-09-01-preview/templates/ingress/meshingress.linux.json)):

```json
{
  "apiVersion": "2018-09-01-preview",
  "name": "ingressGatewayLinux",
  "type": "Microsoft.ServiceFabricMesh/gateways",
  "location": "[parameters('location')]",
  "dependsOn": [
    "Microsoft.ServiceFabricMesh/networks/meshNetworkLinux"
  ],
  "properties": {
    "description": "Service Fabric Mesh Gateway for Linux mesh samples.",
    "sourceNetwork": {
      "name": "Open"
    },
    "destinationNetwork": {
      "name": "[resourceId('Microsoft.ServiceFabricMesh/networks', 'meshNetworkLinux')]"
    },
    "http": [
      {
        "name": "web",
        "port": 80,
        "hosts": [
          {
            "name": "*",
            "routes": [
              {
                "match": {
                  "path": {
                    "value": "/hello",
                    "rewrite": "/",
                    "type": "Prefix"
                  }
                },
                "destination": {
                  "applicationName": "meshAppLinux",
                  "serviceName": "helloWorldService",
                  "endpointName": "helloWorldListener"
                }
              },
              {
                "match": {
                  "path": {
                    "value": "/counter",
                    "rewrite": "/",
                    "type": "Prefix"
                  }
                },
                "destination": {
                  "applicationName": "meshAppLinux",
                  "serviceName": "counterService",
                  "endpointName": "counterServiceListener"
                }
              }
            ]
          }
        ]
      }
    ]
  }
}
```

Deze gateway is geconfigureerd voor een Linux-toepassing 'meshAppLinux', die uit ten minste twee services, "helloWorldService" en "counterService" bestaat, op poort 80 luistert. Afhankelijk van de URL-structuur van de inkomende aanvraag, wordt deze de aanvraag doorsturen naar een van deze services. 
* "<IPAddress>: 80/helloWorld/\<aanvraag\>" zou leiden tot een aanvraag wordt omgeleid naar de 'helloWorldListener' in de helloWorldService. 
* "<IPAddress>: 80/item/\<aanvraag\>" zou leiden tot een aanvraag wordt omgeleid naar de 'counterListener' in de counterService. 

## <a name="next-steps"></a>Volgende stappen
* Implementeer de [inkomend voorbeeld](https://github.com/Azure-Samples/service-fabric-mesh/tree/2018-09-01-preview/templates/ingress) gateways in actie zien
