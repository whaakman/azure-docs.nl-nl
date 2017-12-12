---
title: Azure-Dashboards programmatisch maken | Microsoft Docs
description: In dit artikel wordt uitgelegd hoe u programmatisch Azure om Dashboards te maken.
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
ms.openlocfilehash: d9acb58791cb1412d5e67479ca6490e1548be2c8
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="programmatically-create-azure-dashboards"></a>Azure-Dashboards via een programma maken

Dit document begeleidt bij het proces van het programmatisch maken en publiceren van Azure dashboards. Het dashboard dat hieronder wordt weergegeven wordt in het hele document verwezen.

![voorbeelddashboard](./media/azure-portal-dashboards-create-programmatically/sample-dashboard.png)

## <a name="overview"></a>Overzicht

Dashboards in Azure worden gedeeld [resources](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) net als bij virtuele machines en opslagaccounts.  Daarom deze kunnen worden beheerd via een programma de [Azure Resource Manager REST API's](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-rest-api), wordt de [Azure CLI](https://docs.microsoft.com/cli/azure/overview), [Azure PowerShell-opdrachten](https://docs.microsoft.com/powershell/azure/get-started-azureps?view=azurermps-4.2.0), en veel [ Azure-portal](https://portal.azure.com) functies Voortborduren deze API's voor het beheren van bronnen eenvoudiger.  

Elk van deze API's en hulpprogramma's voor biedt manieren om te maken, weergeven, ophalen, wijzigen en verwijderen van bronnen.  Aangezien dashboards bronnen, kunt u Kies uw favoriete API-hulpprogramma om te gebruiken.

Ongeacht welk hulpmiddel u gebruikt, moet u een JSON-weergave van uw dashboard-object maken voordat u kunt het maken van een resource API aanroepen. Dit object bevat informatie over het delen (ook tegels) op het dashboard. Dit omvat grootten, posities, ze zijn gebonden aan resources en Gebruikersaanpassingen.

De meest praktische manier voor het bouwen van dit JSON-document is met [de portal](https://portal.azure.com/) interactief toevoegen en uw tegels positie. U vervolgens exporteren de JSON. Maak ten slotte een sjabloon uit de resultatenset voor later gebruik in scripts, programma's en hulpprogramma's voor implementatie.

## <a name="create-a-dashboard"></a>Een dashboard maken

Voor het maken van een nieuw dashboard gebruikt u de nieuwe dashboard-opdracht in het hoofdvenster van de portal.

![nieuwe dashboard-opdracht](./media/azure-portal-dashboards-create-programmatically/new-dashboard-command.png)

U kunt vervolgens de tegel galerie gebruiken om te zoeken en tegels toevoegen. Tegels worden toegevoegd door te slepen en neer te zetten. Sommige tegels ondersteuning voor vergroten/verkleinen via een greep, terwijl andere ondersteuning corrigeert grootten die kunnen worden weergegeven in het contextmenu.

### <a name="drag-handle"></a>Sleep greep
![Sleep greep](./media/azure-portal-dashboards-create-programmatically/drag-handle.png)

### <a name="fixed-sizes-via-context-menu"></a>Vaste grootte via het snelmenu
![contextmenu grootten](./media/azure-portal-dashboards-create-programmatically/sizes-context-menu.png)

## <a name="share-the-dashboard"></a>Deel het dashboard

Nadat u hebt het dashboard naar wens die de volgende stappen uit om te publiceren van het dashboard (met behulp van de opdracht Share) en vervolgens de resource explorer voor het ophalen van de JSON zijn geconfigureerd.

![opdracht share](./media/azure-portal-dashboards-create-programmatically/share-command.png)

Klikken op de Share-opdracht, ziet u een dialoogvenster waarin u wordt gevraagd u kiezen welke groep abonnement en de resource om naar te publiceren. Houd rekening met het [u moet schrijftoegang hebben](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) aan het abonnement en de resource-groep die u kiest.

![delen en toegang](./media/azure-portal-dashboards-create-programmatically/sharing-and-access.png)

## <a name="fetch-the-json-representation-of-the-dashboard"></a>De JSON-weergave van het dashboard ophalen

Publicatie duurt slechts enkele seconden.  Wanneer deze voltooid, wordt de volgende stap is om naar de [Resource Explorer](https://portal.azure.com/#blade/HubsExtension/ArmExplorerBlade) voor het ophalen van de JSON.

![resource explorer bladeren](./media/azure-portal-dashboards-create-programmatically/browse-resource-explorer.png)

Van de resource explorer, gaat u naar de groep abonnement en de resource die u hebt gekozen. Klik op de resource nieuw gepubliceerde dashboard om de JSON zichtbaar te maken.

![resource explorer json](./media/azure-portal-dashboards-create-programmatically/resource-explorer-json.png)

## <a name="create-a-template-from-the-json"></a>Een sjabloon maken van de JSON

De volgende stap is het maken van een sjabloon van deze JSON zodat deze opnieuw kan worden gebruikt via een programma met de juiste Resourcemanagement API's, opdrachtregelprogramma's, of binnen de portal.

Het is niet nodig zijn voor een volledig begrip van de JSON-structuur dashboard om een sjabloon te maken. In de meeste gevallen die u wilt behouden de structuur en configuratie van elke tegel en vervolgens voorzien van de reeks ze die naar verwijst bent Azure-resources. Bekijk uw geëxporteerde JSON-dashboard en alle exemplaren van Azure resource-id vinden. Onze dashboard voorbeeld bevat meerdere tegels dat alle op een enkele virtuele machine van Azure wijzen. Dat komt omdat deze één resource onze dashboard alleen bekeken. Als u de voorbeeld-json (geleverd aan het einde van het document) zoekt ' / abonnementen ', vinden van meerdere exemplaren van deze id.

`/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/contoso/providers/Microsoft.Compute/virtualMachines/myVM1`

Voor het publiceren van dit dashboard voor elke virtuele machine moet in de toekomst u elke instantie van deze tekenreeks in de JSON voorzien. 

Er zijn twee versies van API's die het maken van resources in Azure. [Imperatieve API's](https://docs.microsoft.com/rest/api/resources/resources) die één resource tegelijk, maken en een [implementatie op basis van sjabloon](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy) systeem die u kunt het maken van het indelen van meerdere afhankelijke resources, met één API-aanroep. De laatste ondersteunt parameterisering en templating zodat we deze voor het voorbeeld gebruiken.

## <a name="programmatically-create-a-dashboard-from-your-template-using-a-template-deployment"></a>Een dashboard programmatisch van uw sjabloon met de sjabloonimplementatie van een maken

Azure biedt de mogelijkheid voor het indelen van de implementatie van meerdere resources. U maakt een implementatiesjabloon waarin de set met resources die u implementeert en hun onderlinge relaties.  De JSON-indeling van elke resource is hetzelfde als wanneer u ze één voor één zijn gemaakt. Het verschil is dat de [sjabloontaal](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates) worden enkele concepten zoals variabelen, parameters en elementaire functies toegevoegd. Dit uitgebreide-syntaxis wordt alleen ondersteund in de context van de sjabloonimplementatie van een en werkt niet als gebruikt voor de imperatieve API's die eerder is besproken.

Als u deze route gaat, wordt parameterisering moet worden gedaan met behulp van de syntaxis van de sjabloon.  U Vervang alle exemplaren van de resource-id we eerder zagen zoals hier wordt weergegeven.

### <a name="example-json-property-with-hard-coded-resource-id"></a>Voorbeeld van de JSON-eigenschap met de vastgelegde resource-Id
`id: “/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/contoso/providers/Microsoft.Compute/virtualMachines/myVM1”`

### <a name="example-json-property-converted-to-a-parameterized-version-based-on-template-parameters"></a>Voorbeeld van de JSON-eigenschap geconverteerd naar een geparameteriseerde versie op basis van Sjabloonparameters

`id: "[resourceId(parameters('virtualMachineResourceGroup'), ‘Microsoft.Compute/virtualMachines’, parameters('virtualMachineName'))]"`

U moet ook declareren van bepaalde vereiste sjabloon metagegevens en de parameters die aan de bovenkant van het json-sjabloon als volgt:

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

__Hier ziet u de volledige sjabloon werken aan het einde van dit document.__

Zodra u de sjabloon hebt ontworpen u kunt implementeren met behulp van de [REST-API's](https://docs.microsoft.com/rest/api/resources/deployments), [PowerShell](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy), wordt de [Azure CLI](https://docs.microsoft.com/cli/azure/group/deployment#az_group_deployment_create), of de [sjabloonpagina voor implementatie van de portal ](https://portal.azure.com/#create/Microsoft.Template).

Hier vindt u twee versies van onze voorbeeld dashboard JSON. De eerste is de versie die wordt geëxporteerd vanuit de portal die al is gebonden aan een resource. De tweede is de sjabloonversie die via een programma kan worden gebonden aan een virtuele machine en geïmplementeerd met Azure Resource Manager.

## <a name="json-representation-of-our-example-dashboard-before-templating"></a>De JSON-weergave van onze dashboard voorbeeld (vóór templating)

Dit is wat u kunt verwachten om te zien als u de instructies eerder voor het ophalen van de JSON-weergave van een dashboard dat al is geïmplementeerd. Noteer de vastgelegde resource-id's die laten zien dat dit dashboard op een specifieke virtuele machine van Azure wijst.

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

### <a name="template-representation-of-our-example-dashboard"></a>Sjabloon representatie van onze voorbeeld-dashboard

De sjabloonversie van het dashboard drie parameters met de naam is gedefinieerd __virtualMachineName__, __virtualMachineResourceGroup__, en __dashboardName__.  De parameters kunnen u dit dashboard wijst op een andere virtuele machine van Azure, elke keer dat u implementeert. De parameters-id's worden gemarkeerd als u wilt weergeven, kunt u dit dashboard programmatisch geconfigureerd en geïmplementeerd om te verwijzen naar een virtuele machine van Azure. De eenvoudigste manier om te testen met deze functie is de volgende sjabloon kopiëren en plakken in de [sjabloonpagina voor implementatie van Azure portal](https://portal.azure.com/#create/Microsoft.Template). 

In dit voorbeeld wordt een dashboard zelfstandig geïmplementeerd, maar de taal van de sjabloon kunt u meerdere resources implementeren en een of meer dashboards aan zijkant bundelen ze. 

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