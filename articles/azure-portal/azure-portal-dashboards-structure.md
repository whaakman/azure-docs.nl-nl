---
title: De structuur van Azure-Dashboards | Microsoft Docs
description: Dit artikel wordt uitgelegd dat de JSON-structuur van een Azure-Dashboard
services: azure-portal
documentationcenter: ''
author: adamabmsft
manager: dougeby
editor: tysonn
ms.service: azure-portal
ms.devlang: NA
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 09/01/2017
ms.author: cwatson
ms.openlocfilehash: 76f4a52b702a609d7181b9c6c0f2ce600d8a6aac
ms.sourcegitcommit: f58fc4748053a50c34a56314cf99ec56f33fd616
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/04/2018
ms.locfileid: "48267984"
---
# <a name="the-structure-of-azure-dashboards"></a>De structuur van Azure-Dashboards
Dit document begeleidt bij de structuur van een Azure-dashboard met het volgende dashboard als een voorbeeld:

![voorbeelddashboard](./media/azure-portal-dashboards-structure/sample-dashboard.png)

Omdat gedeelde [Azure-dashboards worden resources](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview), kan dit dashboard worden weergegeven als JSON.  De volgende JSON Hiermee geeft u het dashboard gevisualiseerd hierboven.

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

We nemen de relevante secties van de JSON.  De eigenschappen van het hoogste niveau __id__, __naam__, __type__, __locatie__, en __tags__ eigenschappen zijn gedeeld met alle Azure-resource-typen. Dat wil zeggen, hoeven ze geen zoveel te doen met de inhoud van het dashboard.

### <a name="the-id-property"></a>De eigenschap-id

De Azure-resource-id, onderworpen is aan de [naamgevingsconventies van Azure-resources](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions). Wanneer de portal een dashboard maakt een id in het algemeen worden gekozen in de vorm van een guid, maar u bent een geldige naam kunt gebruiken wanneer u ze via een programma maken. 

### <a name="the-name-property"></a>De eigenschap name
De naam is het segment van de resource-Id die niet het abonnement, resourcetype of informatie over resource. Het is in feite het laatste segment van de resource-id.

### <a name="the-type-property"></a>De eigenschap type
Alle dashboards zijn van het type __Microsoft.Portal/dashboards__.

### <a name="the-location-property"></a>De locatie-eigenschap
In tegenstelling tot andere resources geen dashboards een runtime-component.  Voor dashboards, geeft de locatie aan de primaire geografische locatie waarin de JSON-weergave van het dashboard. De waarde moet een van de codes die kunnen worden opgehaald met behulp van de [locaties API op de resource abonnementen](https://docs.microsoft.com/rest/api/resources/subscriptions).

### <a name="the-tags-property"></a>De eigenschap tags
Tags zijn een algemene functie van Azure-resources waarmee u uw resource door willekeurige naam / waardeparen organiseren. Voor dashboards, er is een speciale tag met de naam __verborgen titel__. Als uw dashboard deze eigenschap is ingevuld is, wordt deze gebruikt als de weergavenaam voor uw dashboard in de portal. Azure resource-id's kan niet worden gewijzigd, maar de codes kunnen. Deze tag kunt u een manier om een u weergavenaam voor uw dashboard.

`"tags": { "hidden-title": "Created via API" }`

### <a name="the-properties-object"></a>Het object voor eigenschappen
Het object voor eigenschappen bevat twee eigenschappen, __lenzen__ en __metagegevens__. De __lenzen__ eigenschap bevat informatie over de tegels (ook wel) onderdelen) op het dashboard.  De __metagegevens__ eigenschap is er voor mogelijke toekomstige functies.

### <a name="the-lenses-property"></a>De eigenschap lenzen
De __lenzen__ eigenschap bevat het dashboard. Houd er rekening mee dat de lenzen van het object in dit voorbeeld bevat één eigenschap, genaamd "0". Lenzen zijn een groepering concept, dat momenteel niet is geïmplementeerd in dashboards. Nu hebben al uw dashboards deze één eigenschap op de lens-object opnieuw met de naam '0'.

### <a name="the-lens-object"></a>Het object lens
Het object onder het '0' heeft twee eigenschappen __volgorde__ en __delen__.  In de huidige versie van dashboards, __volgorde__ is altijd 0. De __delen__ eigenschap bevat een object waarin de afzonderlijke onderdelen (ook wel) tegels) op het dashboard.

De __delen__ -object bevat een eigenschap voor elk onderdeel, waarbij de naam van de eigenschap een getal is. Dit nummer is niet van belang. 

### <a name="the-part-object"></a>Het object deel
Elk object afzonderlijk onderdeel heeft een __positie__, en __metagegevens__.

### <a name="the-position-object"></a>Het Positieobject
De __positie__ eigenschap bevat de grootte en locatie-informatie voor het onderdeel als uitgedrukt __x__, __y__, __rowSpan__, en __colSpan__. De waarden zijn in termen van raster eenheden. Deze eenheden raster zijn zichtbaar als het dashboard zich in de modus aanpassen zoals hier wordt weergegeven. Als u een tegel wilt om een breedte van de twee eenheden van de grid, een hoogte van een raster unit en een locatie in de rechterbovenhoek linkerbovenhoek van het dashboard en vervolgens het Positieobject er als uitzien volgt:

`location: { x: 0, y: 0, rowSpan: 2, colSpan: 1 }`

![raster-eenheden](./media/azure-portal-dashboards-structure/grid-units.png)

### <a name="the-metadata-object"></a>Het metagegevensobject
Elk onderdeel heeft een metagegevenseigenschap, een object vereist slechts één eigenschap, genaamd __type__. Deze tekenreeks wordt in de portal de tegel die om weer te geven. Ons voorbeeld van dashboard maakt gebruik van deze typen tegels:


1. `Extension/Microsoft_Azure_Monitoring/PartType/MetricsChartPart` : Wordt gebruikt om weer te geven metrische gegevens controleren
1. `Extension[azure]/HubsExtension/PartType/MarkdownPart` : Wordt gebruikt om weer te geven tekst of afbeeldingen met eenvoudige opmaak voor lijsten, koppelingen, enzovoort.
1. `Extension[azure]/HubsExtension/PartType/VideoPart` : Wordt gebruikt om video's van YouTube, Channel 9 en een ander type van de video die in een video HTML-code werkt weer te geven.
1. `Extension/Microsoft_Azure_Compute/PartType/VirtualMachinePart` : Wordt gebruikt om de naam en de status van een virtuele Azure-machine weer te geven.

Elk type onderdeel heeft een eigen configuratie. De mogelijke configuratie-eigenschappen worden genoemd __invoer__, __instellingen__, en __asset__. 

### <a name="the-inputs-object"></a>De invoer voor het object
De invoer voor het object in het algemeen bevat informatie die wordt gebonden een tegel aan een resource-exemplaar.  Het deel van de virtuele machine in onze voorbeelddashboard bevat een één invoer die gebruikmaakt van de Azure-resource-id om de binding.  De indeling van deze resource-id is consistent voor alle Azure-resources.

```json
"inputs":
[
    {
        "name": "id",
        "value": "/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/contoso/providers/Microsoft.Compute/virtualMachines/myVM1"
    }
]

```
Het deel van de grafiek metrische gegevens is een één invoer waarin de resource moet worden verbonden, evenals informatie over de metrische waarde(n) die worden weergegeven. Hier volgt de invoer voor de tegel waarin de metrische gegevens voor inkomend en uitgaand netwerkverkeer wordt weergegeven.

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
De instellingenobject bevat de configureerbare elementen van een onderdeel.  In het voorbeelddashboard van de Markdown-onderdeel instellingen gebruikt voor het opslaan van de aangepaste markdown-inhoud en een configureerbare titel en subtitel.

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

Op dezelfde manier heeft de videotegel een eigen instellingen met een verwijzing naar de video af te spelen, een instelling voor automatisch afspelen en informatie over de optionele titel.

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
Tegels die zijn gebonden aan eersteklas beheerbare portal objecten (activa genoemd) hebben deze relatie uitgedrukt via de object asset.  In ons voorbeeld van dashboard bevat de virtuele machine-tegel deze beschrijving asset.  De __idInputName__ eigenschap vertelt ons de portal dat de id bevat de unieke id voor de activa in dit geval de resource-id invoeren. De meeste Azure resourcetypen zijn assets die zijn gedefinieerd in de portal.

`"asset": {    "idInputName": "id",    "type": "VirtualMachine"    }`
