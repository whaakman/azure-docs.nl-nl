---
title: Automatisch schalen van Azure gebruiken met metrische gegevens voor gasten in een Linux-schaalsetsjabloon | Microsoft Docs
description: Leer hoe u automatisch schalen met metrische gegevens voor gasten in een sjabloon voor Linux virtuele-Machineschaalset opgehaald
services: virtual-machine-scale-sets
documentationcenter: ''
author: mayanknayar
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: na
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: manayar
ms.openlocfilehash: e30fdb684fbabbdcea334115e3f645e63dec6623
ms.sourcegitcommit: e37fa6e4eb6dbf8d60178c877d135a63ac449076
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2018
ms.locfileid: "53322633"
---
# <a name="autoscale-using-guest-metrics-in-a-linux-scale-set-template"></a>Automatisch schalen met metrische gegevens voor gasten in een Linux-schaalsetsjabloon

Er zijn twee soorten metrische gegevens in Azure die worden verzameld van virtuele machines en schaalsets: sommige afkomstig zijn van de host-VM en anderen afkomstig zijn van de Gast-VM. Op hoog niveau, als u standaard CPU, schijf en netwerk metrische gegevens, zijn klikt u vervolgens metrische gegevens voor hosts waarschijnlijk geschikt. Als u echter een grotere selectie van metrische gegevens moet, klikt u vervolgens zijn metrische gegevens voor gasten waarschijnlijk een betere keuze. Laten we eens in de verschillen tussen de twee:

Metrische gegevens voor hosts zijn eenvoudiger en betrouwbaarder. Ze hoeven niet extra instellingen omdat ze worden verzameld door de host-VM, terwijl de metrische gegevens voor gasten vereisen dat u installeert de [Windows Azure Diagnostics-extensie](../virtual-machines/windows/extensions-diagnostics-template.md) of de [Linux Azure Diagnostics-extensie](../virtual-machines/linux/diagnostic-extension.md)in de Gast-VM. Een veelvoorkomende reden voor het gebruik van metrische gegevens voor gasten in plaats van metrische gegevens voor hosts is dat metrische gegevens voor gasten een grotere selectie van metrische gegevens dan metrische gegevens voor hosts bieden. Een voorbeeld is het geheugengebruik metrische gegevens die alleen beschikbaar via de metrische gegevens voor gasten zijn. De metrische gegevens voor ondersteunde hosts staan [hier](../monitoring-and-diagnostics/monitoring-supported-metrics.md), en vindt u metrische gegevens voor veelgebruikte gasten [hier](../azure-monitor/platform/autoscale-common-metrics.md). Dit artikel wordt beschreven hoe u wijzigt de [minimaal levensvatbare schaalsets sjabloon](./virtual-machine-scale-sets-mvss-start.md) gebruik regels voor automatisch schalen op basis van metrische gegevens voor gasten voor Linux-schaalsets.

## <a name="change-the-template-definition"></a>De sjabloondefinitie van de wijzigen

De minimale levensvatbare schaalsetsjabloon kan worden gezien [hier](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json), en de sjabloon voor de Linux-schaalset implementeren met automatisch schalen op basis van een Gast kan worden bekeken [hier](https://raw.githubusercontent.com/gatneil/mvss/guest-based-autoscale-linux/azuredeploy.json). We bekijken de diff gebruikt voor het maken van deze sjabloon (`git diff minimum-viable-scale-set existing-vnet`) stuk door stuk:

Voeg parameters voor het eerst toe `storageAccountName` en `storageAccountSasToken`. De diagnostics-agent slaat metrische gegevens op in een [tabel](../cosmos-db/table-storage-how-to-use-dotnet.md) in dit opslagaccount. Vanaf de diagnostische gegevens over Linux Agent versie 3.0, met behulp van een toegangssleutel voor opslag is niet meer ondersteund. Gebruik in plaats daarvan een [SAS-Token](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

```diff
     },
     "adminPassword": {
       "type": "securestring"
+    },
+    "storageAccountName": {
+      "type": "string"
+    },
+    "storageAccountSasToken": {
+      "type": "securestring"
     }
   },
```

Wijzig vervolgens de schaalset `extensionProfile` om op te nemen van de extensie voor diagnostische gegevens. In deze configuratie geeft u de resource-ID van de schaalset voor het verzamelen van metrische gegevens uit, evenals de storage-account en de SAS-token te gebruiken voor het opslaan van de metrische gegevens. Geef op hoe vaak de metrische gegevens (in dit geval elke minuut) worden opgeteld en welke metrische gegevens om bij te houden (in dit geval, percentage gebruikt geheugen). Zie voor meer informatie over deze configuratie gedetailleerde en metrische gegevens dan percentage geheugen gebruikt, [deze documentatie](../virtual-machines/linux/diagnostic-extension.md).

```diff
                 }
               }
             ]
+          },
+          "extensionProfile": {
+            "extensions": [
+              {
+                "name": "LinuxDiagnosticExtension",
+                "properties": {
+                  "publisher": "Microsoft.Azure.Diagnostics",
+                  "type": "LinuxDiagnostic",
+                  "typeHandlerVersion": "3.0",
+                  "settings": {
+                    "StorageAccount": "[parameters('storageAccountName')]",
+                    "ladCfg": {
+                      "diagnosticMonitorConfiguration": {
+                        "performanceCounters": {
+                          "sinks": "WADMetricJsonBlob",
+                          "performanceCounterConfiguration": [
+                            {
+                              "unit": "percent",
+                              "type": "builtin",
+                              "class": "memory",
+                              "counter": "percentUsedMemory",
+                              "counterSpecifier": "/builtin/memory/percentUsedMemory",
+                              "condition": "IsAggregate=TRUE"
+                            }
+                          ]
+                        },
+                        "metrics": {
+                          "metricAggregation": [
+                            {
+                              "scheduledTransferPeriod": "PT1M"
+                            }
+                          ],
+                          "resourceId": "[resourceId('Microsoft.Compute/virtualMachineScaleSets', 'myScaleSet')]"
+                        }
+                      }
+                    }
+                  },
+                  "protectedSettings": {
+                    "storageAccountName": "[parameters('storageAccountName')]",
+                    "storageAccountSasToken": "[parameters('storageAccountSasToken')]",
+                    "sinksConfig": {
+                      "sink": [
+                        {
+                          "name": "WADMetricJsonBlob",
+                          "type": "JsonBlob"
+                        }
+                      ]
+                    }
+                  }
+                }
+              }
+            ]
           }
         }
       }
```

Voeg een `autoscaleSettings` resource die u wilt configureren voor automatisch schalen op basis van deze metrische gegevens. Deze resource heeft een `dependsOn` component die verwijst naar de schaal instellen om ervoor te zorgen dat de schaalset bestaat voordat u probeert deze te schalen. Als u ervoor kiest een verschillende metrische waarde voor automatisch schalen op, gebruikt u de `counterSpecifier` uit de configuratie van de extensie voor diagnostische gegevens als de `metricName` in de configuratie voor automatisch schalen. Zie voor meer informatie over de configuratie voor automatisch schalen, de [aanbevolen procedures voor automatisch schalen](..//azure-monitor/platform/autoscale-best-practices.md) en de [Azure Monitor REST API-referentiedocumentatie](https://msdn.microsoft.com/library/azure/dn931928.aspx).

```diff
+    },
+    {
+      "type": "Microsoft.Insights/autoscaleSettings",
+      "apiVersion": "2015-04-01",
+      "name": "guestMetricsAutoscale",
+      "location": "[resourceGroup().location]",
+      "dependsOn": [
+        "Microsoft.Compute/virtualMachineScaleSets/myScaleSet"
+      ],
+      "properties": {
+        "name": "guestMetricsAutoscale",
+        "targetResourceUri": "[resourceId('Microsoft.Compute/virtualMachineScaleSets', 'myScaleSet')]",
+        "enabled": true,
+        "profiles": [
+          {
+            "name": "Profile1",
+            "capacity": {
+              "minimum": "1",
+              "maximum": "10",
+              "default": "3"
+            },
+            "rules": [
+              {
+                "metricTrigger": {
+                  "metricName": "/builtin/memory/percentUsedMemory",
+                  "metricNamespace": "",
+                  "metricResourceUri": "[resourceId('Microsoft.Compute/virtualMachineScaleSets', 'myScaleSet')]",
+                  "timeGrain": "PT1M",
+                  "statistic": "Average",
+                  "timeWindow": "PT5M",
+                  "timeAggregation": "Average",
+                  "operator": "GreaterThan",
+                  "threshold": 60
+                },
+                "scaleAction": {
+                  "direction": "Increase",
+                  "type": "ChangeCount",
+                  "value": "1",
+                  "cooldown": "PT1M"
+                }
+              },
+              {
+                "metricTrigger": {
+                  "metricName": "/builtin/memory/percentUsedMemory",
+                  "metricNamespace": "",
+                  "metricResourceUri": "[resourceId('Microsoft.Compute/virtualMachineScaleSets', 'myScaleSet')]",
+                  "timeGrain": "PT1M",
+                  "statistic": "Average",
+                  "timeWindow": "PT5M",
+                  "timeAggregation": "Average",
+                  "operator": "LessThan",
+                  "threshold": 30
+                },
+                "scaleAction": {
+                  "direction": "Decrease",
+                  "type": "ChangeCount",
+                  "value": "1",
+                  "cooldown": "PT1M"
+                }
+              }
+            ]
+          }
+        ]
+      }
     }
   ]
 }
```





## <a name="next-steps"></a>Volgende stappen

[!INCLUDE [mvss-next-steps-include](../../includes/mvss-next-steps.md)]
