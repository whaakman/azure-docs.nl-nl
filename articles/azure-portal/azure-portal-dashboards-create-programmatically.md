---
title: Programmatische wijze Azure-Dashboards maken | Microsoft Docs
description: In dit artikel wordt uitgelegd hoe u Azure-Dashboards via een programma te maken.
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
ms.author: adamab
ms.openlocfilehash: 8ac3bb2c95420eb4a608f003f3d937e3a47c272b
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39448227"
---
# <a name="programmatically-create-azure-dashboards"></a>Programmatische wijze Azure-Dashboards maken

Dit document begeleidt bij het proces van via een programma maken en publiceren van de Azure-dashboards. Het dashboard dat hieronder wordt weergegeven wordt in het hele document verwezen.

![voorbeelddashboard](./media/azure-portal-dashboards-create-programmatically/sample-dashboard.png)

## <a name="overview"></a>Overzicht

Gedeelde dashboards in Azure worden [resources](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) net als bij virtuele machines en opslagaccounts.  Daarom, ze kunnen worden beheerd via een programma via de [Azure Resource Manager REST API's](/rest/api/), wordt de [Azure CLI](https://docs.microsoft.com/cli/azure), [Azure PowerShell-opdrachten](https://docs.microsoft.com/powershell/azure/get-started-azureps?view=azurermps-4.2.0), en veel [ Azure-portal](https://portal.azure.com) functies bouwen boven op deze API's om gemakkelijker resourcebeheer.  

Elk van deze API's en hulpprogramma's biedt manieren om u te maken, weergeven, ophalen, wijzigen en verwijderen van resources.  Omdat dashboards resources zijn, kunt u Kies uw favoriete API / hulpprogramma voor het gebruik.

Ongeacht welk hulpmiddel u gebruikt, moet u een JSON-weergave van uw dashboard-object maken voordat u een resource maken API kunt aanroepen. Dit object bevat informatie over de onderdelen (ook wel) tegels) op het dashboard. Deze bevat grootte, positie, ze zijn gebonden aan resources en Gebruikersaanpassingen.

De meest praktische manier om op te bouwen in dit JSON-document is met [de portal](https://portal.azure.com/) interactief toevoegen en de positie van uw tegels. U exporteert vervolgens de JSON. Ten slotte maakt u een sjabloon vanuit het resultaat voor later gebruik in scripts, programma's en hulpprogramma's voor implementatie.

## <a name="create-a-dashboard"></a>Een dashboard maken

Als u wilt een nieuw dashboard maken, gebruikt u de nieuwe dashboard-opdracht in het hoofdvenster van de portal.

![nieuwe dashboard-opdracht](./media/azure-portal-dashboards-create-programmatically/new-dashboard-command.png)

U kunt vervolgens de tegelgalerie gebruiken om te zoeken en tegels toevoegen. Tegels zijn toegevoegd door te slepen en neer te zetten. Sommige tegels ondersteuning vergroten of verkleinen via een slepen-ingang, terwijl andere ondersteuning corrigeert groottes die kunnen worden weergegeven in het contextmenu.

### <a name="drag-handle"></a>Slepen-ingang
![Sleep de greep](./media/azure-portal-dashboards-create-programmatically/drag-handle.png)

### <a name="fixed-sizes-via-context-menu"></a>Vaste grootten via contextmenu
![contextmenu grootten](./media/azure-portal-dashboards-create-programmatically/sizes-context-menu.png)

## <a name="share-the-dashboard"></a>Het dashboard delen

Nadat u hebt het dashboard naar wens die de volgende stappen uit om te publiceren van het dashboard (met behulp van de opdracht Share) en vervolgens de resource explorer gebruiken om op te halen van de JSON zijn geconfigureerd.

![opdracht delen](./media/azure-portal-dashboards-create-programmatically/share-command.png)

Klikken op de Share-opdracht ziet u een dialoogvenster waarin u wordt gevraagd te kiezen welke groep en de resourcegroep om naar te publiceren. Houd rekening met het [hebt u toegang voor schrijven](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) aan de groep en de resourcegroep die u kiest.

![delen en toegang](./media/azure-portal-dashboards-create-programmatically/sharing-and-access.png)

## <a name="fetch-the-json-representation-of-the-dashboard"></a>De JSON-weergave van het dashboard ophalen

Publicatie duurt slechts een paar seconden.  Wanneer dit voltooid, wordt de volgende stap is om naar de [Resource Explorer](https://portal.azure.com/#blade/HubsExtension/ArmExplorerBlade) voor het ophalen van de JSON.

![resource explorer bladeren](./media/azure-portal-dashboards-create-programmatically/browse-resource-explorer.png)

Vanuit de resource explorer, navigeert u naar de groep en de resourcegroep die u hebt gekozen. Klik op het zojuist gepubliceerde dashboard-resource om de JSON weer te geven.

![resource explorer json](./media/azure-portal-dashboards-create-programmatically/resource-explorer-json.png)

## <a name="create-a-template-from-the-json"></a>Een sjabloon maken van de JSON

De volgende stap is het maken van een sjabloon uit deze JSON, zodat deze opnieuw kan worden gebruikt via een programma met de juiste Resourcemanagement API's en opdrachtregelprogramma's, of in de portal.

Het is niet nodig voor een volledig begrip van de structuur van de JSON dashboard om een sjabloon te maken. In de meeste gevallen die u wilt behouden van de structuur en configuratie van elke tegel en vervolgens voorzien van de set met ze die naar verwijst bent Azure-resources. Ga naar het geëxporteerde JSON-dashboard en alle instanties van Azure resource-id's. Ons voorbeeld van dashboard heeft meerdere tegels die allemaal op een enkele Azure-machine verwijzen. Dat komt doordat het dashboard alleen gekeken naar deze één resource. Als u de voorbeeld-json (opgenomen aan het einde van het document) zoeken naar "/ abonnementen", vinden van meerdere exemplaren van deze id.

`/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/contoso/providers/Microsoft.Compute/virtualMachines/myVM1`

Als u wilt publiceren dit dashboard voor elke virtuele machine moet in de toekomst u voorzien van elk exemplaar van deze tekenreeks in de JSON. 

Er zijn twee versies van API's die resources in Azure maken. [Imperatieve API's](https://docs.microsoft.com/rest/api/resources/resources) die een resource maken op een tijdstip, en een [implementatie op basis van sjabloon van](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy) systeem dat het maken van de kunt indelen van meerdere, afhankelijke resources, met één API-aanroep. De laatste systeemeigen ondersteuning biedt voor parameterisering en templating zodat we deze in ons voorbeeld gebruiken.

## <a name="programmatically-create-a-dashboard-from-your-template-using-a-template-deployment"></a>Een dashboard via een programma te maken van uw sjabloon met behulp van de sjabloonimplementatie van een

Azure biedt de mogelijkheid voor het indelen van de implementatie van meerdere resources. U maakt een implementatiesjabloon waarin de set met resources om te implementeren en de relaties tussen deze.  De JSON-indeling van elke resource is hetzelfde als wanneer u ze zijn gemaakt, kunt u één voor één. Het verschil is dat de [sjabloontaal](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates) enkele concepten, zoals variabelen, parameters en algemene functies toegevoegd. Dit uitgebreide-syntaxis wordt alleen ondersteund in de context van de sjabloonimplementatie van een en werkt niet als u gebruikt met de imperatieve API's die eerder zijn besproken.

Als u deze route gaat, wordt de parameterisering moet worden uitgevoerd met behulp van de syntaxis van de sjabloon.  U Vervang alle exemplaren van de resource-id eerder gevonden zoals hier wordt weergegeven.

### <a name="example-json-property-with-hard-coded-resource-id"></a>Voorbeeld van JSON-eigenschap met de vastgelegde resource-Id
`id: "/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/contoso/providers/Microsoft.Compute/virtualMachines/myVM1"`

### <a name="example-json-property-converted-to-a-parameterized-version-based-on-template-parameters"></a>Voorbeeld van JSON-eigenschap geconverteerd naar een geparameteriseerde versie op basis van Sjabloonparameters

`id: "[resourceId(parameters('virtualMachineResourceGroup'), 'Microsoft.Compute/virtualMachines', parameters('virtualMachineName'))]"`

U moet ook declareren sommige vereiste metagegevens en de parameters die aan de bovenkant van het json-sjabloon als volgt:

```json

{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "virtualMachineName": {
            "type": "string"
        },
        "virtualMachineResourceGroup": {
            "type": "string"
        },
        "dashboardName": {
            "type": "string"
        }
    },
    "variables": {},

    ... rest of template omitted ...
```

__Hier ziet u de volledige, werkende sjabloon aan het einde van dit document.__

Nadat u de sjabloon hebt gemaakt kunt u implementeren met behulp van de [REST-API's](https://docs.microsoft.com/rest/api/resources/deployments), [PowerShell](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy), wordt de [Azure CLI](https://docs.microsoft.com/cli/azure/group/deployment#az-group-deployment-create), of de [sjabloonpagina voor implementatie van de portal ](https://portal.azure.com/#create/Microsoft.Template).

Hier volgen twee versies van het dashboard voorbeeld van JSON. De eerste is de versie die we van de portal die al is gebonden aan een resource hebt geëxporteerd. De tweede is de sjabloonversie die via een programma kan worden gebonden aan een virtuele machine en geïmplementeerd met behulp van Azure Resource Manager.

## <a name="json-representation-of-our-example-dashboard-before-templating"></a>JSON-weergave van het dashboard voorbeeld (vóór templating)

Dit is wat u kunt verwachten om te zien als u de instructies eerder voor het ophalen van de JSON-weergave van een dashboard dat al is geïmplementeerd. Houd er rekening mee de vastgelegde resource-id's die laten zien dat dit dashboard op een specifieke Azure-machine wijst.

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
        "metadata": { }
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

### <a name="template-representation-of-our-example-dashboard"></a>Weergave van de sjabloon van ons voorbeeld van dashboard

De versie van het dashboard bevat de gedefinieerde drie parameters met de naam __virtualMachineName__, __virtualMachineResourceGroup__, en __dashboardName__.  De parameters kunnen u dit dashboard verwijzen naar een andere Azure-machine telkens wanneer u implementeert. De parameters-id's zijn gemarkeerd als u wilt weergeven, kunt u dit dashboard via een programma geconfigureerd en geïmplementeerd om te verwijzen naar een virtuele machine van Azure. De eenvoudigste manier voor het testen van deze functie is op de volgende sjabloon te kopiëren en plak deze in de [sjabloonpagina voor implementatie van Azure portal](https://portal.azure.com/#create/Microsoft.Template). 

In dit voorbeeld implementeert u een dashboard op zichzelf, maar de taal van de sjabloon kunt u meerdere resources implementeren en een of meer dashboards naast bundelen ze. 

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "virtualMachineName": {
            "type": "string"
        },
        "virtualMachineResourceGroup": {
            "type": "string"
        },
        "dashboardName": {
            "type": "string"
        }
    },
    "variables": {},
    "resources": [
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
                                                "id": "[resourceId(parameters('virtualMachineResourceGroup'), 'Microsoft.Compute/virtualMachines', parameters('virtualMachineName'))]",
                                                "chartType": 0,
                                                "metrics": [
                                                    {
                                                        "name": "Percentage CPU",
                                                        "resourceId": "[resourceId(parameters('virtualMachineResourceGroup'), 'Microsoft.Compute/virtualMachines', parameters('virtualMachineName'))]"
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
                                                "id": "[resourceId(parameters('virtualMachineResourceGroup'), 'Microsoft.Compute/virtualMachines', parameters('virtualMachineName'))]",
                                                "chartType": 0,
                                                "metrics": [
                                                    {
                                                        "name": "Disk Read Operations/Sec",
                                                        "resourceId": "[resourceId(parameters('virtualMachineResourceGroup'), 'Microsoft.Compute/virtualMachines', parameters('virtualMachineName'))]"
                                                    },
                                                    {
                                                        "name": "Disk Write Operations/Sec",
                                                        "resourceId": "[resourceId(parameters('virtualMachineResourceGroup'), 'Microsoft.Compute/virtualMachines', parameters('virtualMachineName'))]"
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
                                                "id": "[resourceId(parameters('virtualMachineResourceGroup'), 'Microsoft.Compute/virtualMachines', parameters('virtualMachineName'))]",
                                                "chartType": 0,
                                                "metrics": [
                                                    {
                                                        "name": "Disk Read Bytes",
                                                        "resourceId": "[resourceId(parameters('virtualMachineResourceGroup'), 'Microsoft.Compute/virtualMachines', parameters('virtualMachineName'))]"
                                                    },
                                                    {
                                                        "name": "Disk Write Bytes",
                                                        "resourceId": "[resourceId(parameters('virtualMachineResourceGroup'), 'Microsoft.Compute/virtualMachines', parameters('virtualMachineName'))]"
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
                                                "id": "[resourceId(parameters('virtualMachineResourceGroup'), 'Microsoft.Compute/virtualMachines', parameters('virtualMachineName'))]",
                                                "chartType": 0,
                                                "metrics": [
                                                    {
                                                        "name": "Network In",
                                                        "resourceId": "[resourceId(parameters('virtualMachineResourceGroup'), 'Microsoft.Compute/virtualMachines', parameters('virtualMachineName'))]"
                                                    },
                                                    {
                                                        "name": "Network Out",
                                                        "resourceId": "[resourceId(parameters('virtualMachineResourceGroup'), 'Microsoft.Compute/virtualMachines', parameters('virtualMachineName'))]"
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
                                            "value": "[resourceId(parameters('virtualMachineResourceGroup'), 'Microsoft.Compute/virtualMachines', parameters('virtualMachineName'))]"
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
                }
            },
            "metadata": { },
            "apiVersion": "2015-08-01-preview",
            "type": "Microsoft.Portal/dashboards",
            "name": "[parameters('dashboardName')]",
            "location": "westus",
            "tags": {
                "hidden-title": "[parameters('dashboardName')]"
            }
        }
    ]
}


```
