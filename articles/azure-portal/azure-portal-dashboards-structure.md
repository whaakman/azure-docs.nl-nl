---
title: De structuur van Azure Dashboards | Microsoft Docs
description: Dit artikel wordt uitgelegd van de JSON-structuur van een Azure-Dashboard
services: azure-portal
documentationcenter: 
author: adamab
manager: timlt
editor: tysonn
ms.service: azure-portal
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 09/01/2017
ms.author: adamab
ms.openlocfilehash: f71ff9383f20a1a75fd2c1cf4dc3aaf049d970cf
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="the-structure-of-azure-dashboards"></a>De structuur van Azure Dashboards
Dit document helpt bij de structuur van een Azure-dashboard met het volgende dashboard als een voorbeeld:

![voorbeelddashboard](./media/azure-portal-dashboards-structure/sample-dashboard.png)

Aangezien gedeeld [Azure dashboards zijn resources](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview), kan dit dashboard worden weergegeven als JSON.  De volgende JSON vertegenwoordigt het dashboard hierboven weergegeven.

```json

{
    "properties": {
        "lenses": {
            "0": {
                "order": 0,
                "parts": {
                    "0": {
                        "position": {
                            "x": 0,
                            "y": 0,
                            "rowSpan": 2,
                            "colSpan": 3
                        },
                        "metadata": {
                            "inputs": [],
                            "type": "Extension[azure]/HubsExtension/PartType/MarkdownPart",
                            "settings": {
                                "content": {
                                    "settings": {
                                        "content": "## Azure Virtual Machines Overview\r\nNew team members should watch this video to get familiar with Azure Virtual Machines.",
                                        "title": "",
                                        "subtitle": ""
                                    }
                                }
                            }
                        }
                    },
                    "1": {
                        "position": {
                            "x": 3,
                            "y": 0,
                            "rowSpan": 4,
                            "colSpan": 8
                        },
                        "metadata": {
                            "inputs": [],
                            "type": "Extension[azure]/HubsExtension/PartType/MarkdownPart",
                            "settings": {
                                "content": {
                                    "settings": {
                                        "content": "This is the team dashboard for the test VM we use on our team. Here are some useful links:\r\n\r\n1. [Getting started](https://www.contoso.com/tsgs)\r\n1. [Troubleshooting guide](https://www.contoso.com/tsgs)\r\n1. [Architecture docs](https://www.contoso.com/tsgs)",
                                        "title": "Test VM Dashboard",
                                        "subtitle": "Contoso"
                                    }
                                }
                            }
                        }
                    },
                    "2": {
                        "position": {
                            "x": 0,
                            "y": 2,
                            "rowSpan": 2,
                            "colSpan": 3
                        },
                        "metadata": {
                            "inputs": [],
                            "type": "Extension[azure]/HubsExtension/PartType/VideoPart",
                            "settings": {
                                "content": {
                                    "settings": {
                                        "title": "",
                                        "subtitle": "",
                                        "src": "https://www.youtube.com/watch?v=YcylDIiKaSU&list=PLLasX02E8BPCsnETz0XAMfpLR1LIBqpgs&index=4",
                                        "autoplay": false
                                    }
                                }
                            }
                        }
                    },
                    "3": {
                        "position": {
                            "x": 0,
                            "y": 4,
                            "rowSpan": 3,
                            "colSpan": 11
                        },
                        "metadata": {
                            "inputs": [
                                {
                                    "name": "queryInputs",
                                    "value": {
                                        "timespan": {
                                            "duration": "PT1H",
                                            "start": null,
                                            "end": null
                                        },
                                        "id": "/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/contoso/providers/Microsoft.Compute/virtualMachines/myVM1",
                                        "chartType": 0,
                                        "metrics": [
                                            {
                                                "name": "Percentage CPU",
                                                "resourceId": "/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/contoso/providers/Microsoft.Compute/virtualMachines/myVM1"
                                            }
                                        ]
                                    }
                                }
                            ],
                            "type": "Extension/Microsoft_Azure_Monitoring/PartType/MetricsChartPart",
                            "settings": {}
                        }
                    },
                    "4": {
                        "position": {
                            "x": 0,
                            "y": 7,
                            "rowSpan": 2,
                            "colSpan": 3
                        },
                        "metadata": {
                            "inputs": [
                                {
                                    "name": "queryInputs",
                                    "value": {
                                        "timespan": {
                                            "duration": "PT1H",
                                            "start": null,
                                            "end": null
                                        },
                                        "id": "/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/contoso/providers/Microsoft.Compute/virtualMachines/myVM1",
                                        "chartType": 0,
                                        "metrics": [
                                            {
                                                "name": "Disk Read Operations/Sec",
                                                "resourceId": "/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/contoso/providers/Microsoft.Compute/virtualMachines/myVM1"
                                            },
                                            {
                                                "name": "Disk Write Operations/Sec",
                                                "resourceId": "/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/contoso/providers/Microsoft.Compute/virtualMachines/myVM1"
                                            }
                                        ]
                                    }
                                }
                            ],
                            "type": "Extension/Microsoft_Azure_Monitoring/PartType/MetricsChartPart",
                            "settings": {}
                        }
                    },
                    "5": {
                        "position": {
                            "x": 3,
                            "y": 7,
                            "rowSpan": 2,
                            "colSpan": 3
                        },
                        "metadata": {
                            "inputs": [
                                {
                                    "name": "queryInputs",
                                    "value": {
                                        "timespan": {
                                            "duration": "PT1H",
                                            "start": null,
                                            "end": null
                                        },
                                        "id": "/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/contoso/providers/Microsoft.Compute/virtualMachines/myVM1",
                                        "chartType": 0,
                                        "metrics": [
                                            {
                                                "name": "Disk Read Bytes",
                                                "resourceId": "/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/contoso/providers/Microsoft.Compute/virtualMachines/myVM1"
                                            },
                                            {
                                                "name": "Disk Write Bytes",
                                                "resourceId": "/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/contoso/providers/Microsoft.Compute/virtualMachines/myVM1"
                                            }
                                        ]
                                    }
                                }
                            ],
                            "type": "Extension/Microsoft_Azure_Monitoring/PartType/MetricsChartPart",
                            "settings": {}
                        }
                    },
                    "6": {
                        "position": {
                            "x": 6,
                            "y": 7,
                            "rowSpan": 2,
                            "colSpan": 3
                        },
                        "metadata": {
                            "inputs": [
                                {
                                    "name": "queryInputs",
                                    "value": {
                                        "timespan": {
                                            "duration": "PT1H",
                                            "start": null,
                                            "end": null
                                        },
                                        "id": "/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/contoso/providers/Microsoft.Compute/virtualMachines/myVM1",
                                        "chartType": 0,
                                        "metrics": [
                                            {
                                                "name": "Network In",
                                                "resourceId": "/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/contoso/providers/Microsoft.Compute/virtualMachines/myVM1"
                                            },
                                            {
                                                "name": "Network Out",
                                                "resourceId": "/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/contoso/providers/Microsoft.Compute/virtualMachines/myVM1"
                                            }
                                        ]
                                    }
                                }
                            ],
                            "type": "Extension/Microsoft_Azure_Monitoring/PartType/MetricsChartPart",
                            "settings": {}
                        }
                    },
                    "7": {
                        "position": {
                            "x": 9,
                            "y": 7,
                            "rowSpan": 2,
                            "colSpan": 2
                        },
                        "metadata": {
                            "inputs": [
                                {
                                    "name": "id",
                                    "value": "/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/contoso/providers/Microsoft.Compute/virtualMachines/myVM1"
                                }
                            ],
                            "type": "Extension/Microsoft_Azure_Compute/PartType/VirtualMachinePart",
                            "asset": {
                                "idInputName": "id",
                                "type": "VirtualMachine"
                            },
                            "defaultMenuItemId": "overview"
                        }
                    }
                }
            }
        },
        "metadata": {
            "model": {
                "timeRange": {
                    "value": {
                        "relative": {
                            "duration": 24,
                            "timeUnit": 1
                        }
                    },
                    "type": "MsPortalFx.Composition.Configuration.ValueTypes.TimeRange"
                }
            }
        }
    },
    "id": "/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/dashboards/providers/Microsoft.Portal/dashboards/aa9786ae-e159-483f-b05f-1f7f767741a9",
    "name": "aa9786ae-e159-483f-b05f-1f7f767741a9",
    "type": "Microsoft.Portal/dashboards",
    "location": "eastasia",
    "tags": {
        "hidden-title": "Created via API"
    }
}

```

## <a name="common-resource-properties"></a>Algemene broneigenschappen

Stel de relevante secties van de JSON uitgesplitst.  De eigenschappen van het hoogste niveau __id__, __naam__, __type__, __locatie__, en __labels__ eigenschappen zijn gedeeld door alle Azure-resource-typen. Dat wil zeggen, hebben niet ze veel te doen met de inhoud van het dashboard.

### <a name="the-id-property"></a>De eigenschap-id

De Azure-resource-id, onderworpen aan de [naamgevingsregels van Azure-resources](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions). Wanneer de portal wordt een dashboard gemaakt in het algemeen gekozen een id in de vorm van een guid, maar u bent geldige naam kunt gebruiken wanneer u deze via een programma maakt. 

### <a name="the-name-property"></a>De eigenschap name
De naam is het segment van de resource-Id dat het abonnement, brontype of informatie over de resource niet omvat. In wezen, is het laatste segment van de resource-id.

### <a name="the-type-property"></a>De eigenschap type
Alle dashboards zijn van het type __Microsoft.Portal/dashboards__.

### <a name="the-location-property"></a>De locatie-eigenschap
In tegenstelling tot andere resources hebben geen dashboards een runtime-component.  Voor dashboards geeft de locatie van de primaire geografische locatie waar de JSON-weergave van het dashboard worden opgeslagen. De waarde moet een van de codes die kunnen worden opgehaald met behulp van de [locaties API op de resource abonnementen](https://docs.microsoft.com/rest/api/resources/subscriptions).

### <a name="the-tags-property"></a>De eigenschap labels
Labels zijn een algemene functie van Azure-resources waarmee u uw resource door willekeurige naam-waardeparen te organiseren. Voor dashboards, er is een speciale tag aangeroepen __verborgen titel__. Als uw dashboard, deze eigenschap is ingevuld heeft, wordt deze gebruikt als de weergavenaam voor het dashboard in de portal. Azure resource-id kan niet worden gewijzigd, maar de labels kunnen. Deze tag biedt een manier om een u weergavenaam voor uw dashboard.

`"tags": { "hidden-title": "Created via API" }`

### <a name="the-properties-object"></a>Het Eigenschappenobject
Het Eigenschappenobject bevat twee eigenschappen __lenzen__ en __metagegevens__. De __lenzen__ eigenschap bevat informatie over de tegels (ook onderdelen) op het dashboard.  De __metagegevens__ eigenschap is er voor mogelijke toekomstige functies.

### <a name="the-lenses-property"></a>De eigenschap lenzen
De __lenzen__ eigenschap bevat het dashboard. Opmerking dat de lenzen in dit voorbeeld object bevat één eigenschap genaamd "0". Lenzen zijn een concept, de groepering die momenteel niet is geïmplementeerd in dashboards. Op dit moment hebben al uw dashboards deze eigenschap op het lens-object, opnieuw, '0' genoemd.

### <a name="the-lens-object"></a>Het object lens
Het object onder de '0' heeft twee eigenschappen __volgorde__ en __delen__.  In de huidige versie van dashboards, __volgorde__ is altijd 0. De __delen__ eigenschap bevat een object dat de afzonderlijke onderdelen (ook bepaalt tegels) op het dashboard.

De __delen__ object bevat een eigenschap voor elk onderdeel, waarbij de naam van de eigenschap een getal is. Dit nummer is niet van belang. 

### <a name="the-part-object"></a>Het object deel
Elk object afzonderlijk onderdeel heeft een __positie__, en __metagegevens__.

### <a name="the-position-object"></a>Het Positieobject
De __positie__ eigenschap bevat de informatie van de grootte en locatie van uitgedrukt in het gedeelte __x__, __y__, __rowSpan__, en __colSpan__. De waarden zijn in termen van raster eenheden. Deze eenheden raster zijn zichtbaar wanneer het dashboard in de modus aanpassen is zoals hier wordt weergegeven. Als u een tegel wilt om een breedte van de twee raster eenheden, een hoogte van een raster eenheid en een locatie in het bovenste linkerbovenhoek van het dashboard en vervolgens de positie object uitziet:

`location: { x: 0, y: 0, rowSpan: 2, colSpan: 1 }`

![raster-eenheden](./media/azure-portal-dashboards-structure/grid-units.png)

### <a name="the-metadata-object"></a>Het metagegevensobject
Elk onderdeel heeft een eigenschap metadata, een object slechts één vereiste eigenschap aangeroepen __type__. Deze tekenreeks de portal wordt uitgelegd welke tegel om weer te geven. Onze dashboard voorbeeld maakt gebruik van deze typen tegels:


1. `Extension/Microsoft_Azure_Monitoring/PartType/MetricsChartPart`: Wordt gebruikt om weer te geven metrische gegevens controleren
1. `Extension[azure]/HubsExtension/PartType/MarkdownPart`: Wordt gebruikt om weer te geven tekst of afbeeldingen met basic opmaak voor lijsten, koppelingen, enzovoort.
1. `Extension[azure]/HubsExtension/PartType/VideoPart`– Gebruikt voor het weergeven van video's van YouTube, Channel9 en een ander type video in een video html-tag werkt.
1. `Extension/Microsoft_Azure_Compute/PartType/VirtualMachinePart`– Gebruikt voor het weergeven van de naam en de status van een virtuele machine van Azure.

Elk type onderdeel heeft een eigen configuratie. De mogelijke configuratie-eigenschappen worden genoemd __invoer__, __instellingen__, en __asset__. 

### <a name="the-inputs-object"></a>De invoer-object
De invoer-object bevat doorgaans informatie waaraan een tegel is gebonden aan een resource-exemplaar.  Het deel van de virtuele machine in onze voorbeelddashboard bevat een enkele invoer die u de Azure-resource-id gebruikt om de binding express.  De indeling van deze resource-id is consistent voor alle Azure-resources.

```json
"inputs":
[
    {
        "name": "id",
        "value": "/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/contoso/providers/Microsoft.Compute/virtualMachines/myVM1"
    }
]

```
Het deel van de grafiek metrische gegevens heeft een één invoer waarin de resource moet worden gebonden, evenals informatie over de metric(s) wordt weergegeven. Hier volgt de invoer voor de tegel die de metrische gegevens netwerk In- en netwerk wordt weergegeven.

```json
“inputs”:
[
    {
        "name": "queryInputs",
        "value": 
        {
            "timespan": 
            {
                "duration": "PT1H",
                "start": null,
                "end": null
           },
            "id": "/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/contoso/providers/Microsoft.Compute/virtualMachines/myVM1",
            "chartType": 0,
            "metrics": 
            [
                {
                    "name": "Network In",
                    "resourceId": "/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/contoso/providers/Microsoft.Compute/virtualMachines/myVM1"
                },
                {
                    "name": "Network Out",
                    "resourceId": "/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/contoso/providers/Microsoft.Compute/virtualMachines/myVM1"
                }
            ]
        }
    }
]

```

### <a name="the-settings-object"></a>Het instellingenobject
Het instellingenobject bevat de configureerbare elementen van een onderdeel.  In ons voorbeelddashboard de Markdown-onderdeel instellingen gebruikt voor het opslaan van de aangepaste markdown-inhoud, evenals een configureerbare titel en subtitel.

```json
"settings": 
{
    "content": 
    {
        "settings": 
        {
            "content": "This is the team dashboard for the test VM we use on our team. Here are some useful links:\r\n\r\n1. [Getting started](https://www.contoso.com/tsgs)\r\n1. [Troubleshooting guide](https://www.contoso.com/tsgs)\r\n1. [Architecture docs](https://www.contoso.com/tsgs)",
            "title": "Test VM Dashboard",
            "subtitle": "Contoso"
        }
    }
}

```

Op dezelfde manier heeft de video tegel een eigen instellingen met een verwijzing naar de video af te spelen, een instelling en informatie over de optionele titel.

```json
"settings": 
{
   "content": 
    {
        "settings": 
        {
            "title": "",
            "subtitle": "",
            "src": "https://www.youtube.com/watch?v=YcylDIiKaSU&list=PLLasX02E8BPCsnETz0XAMfpLR1LIBqpgs&index=4",
            "autoplay": false
        }
    }
}

```

### <a name="the-asset-object"></a>Het object asset
Tegels die zijn gebonden aan de eerste klas beheerbare portal objecten (activa genoemd), hebben deze relatie uitgedrukt via het asset-object.  In ons voorbeeld-dashboard bevat de virtuele machine-tegel deze beschrijving asset.  De __idInputName__ eigenschap de portal te zien dat de id bevat de unieke id voor de activa in dit geval de resource-id invoeren. De meeste Azure brontypen hebben assets die zijn gedefinieerd in de portal.

`"asset": {    "idInputName": "id",    "type": "VirtualMachine"    }`