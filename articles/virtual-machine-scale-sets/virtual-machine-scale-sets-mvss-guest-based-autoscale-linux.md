---
title: Gebruik Azure automatisch schalen met gast metrische gegevens in een sjabloon van Linux scale set | Microsoft Docs
description: Meer informatie over hoe u met behulp van de Gast metrische gegevens in een sjabloon van Linux virtuele-Machineschaalset voor automatisch schalen
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: na
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: negat
ms.openlocfilehash: 8e822d83dd3bafabfea60ad50224c87df226bdc6
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/20/2017
---
# <a name="autoscale-using-guest-metrics-in-a-linux-scale-set-template"></a>Met behulp van de Gast metrische gegevens in een sjabloon van Linux scale set voor automatisch schalen

Er zijn twee soorten metrische gegevens in Azure die worden verzameld uit de virtuele machines en schalen sets: sommige afkomstig zijn van de VM-host en anderen afkomstig zijn van de Gast-VM. Op hoog niveau, als u standaard CPU, schijf en metrische gegevens voor netwerk, zijn vervolgens host metrische gegevens waarschijnlijk geschikt. Als u echter een ruimere selectie van metrische gegevens moet, klikt u vervolgens zijn Gast metrische gegevens waarschijnlijk beter past. We gaan verdiepen in de verschillen tussen de twee:

Host metrische gegevens zijn eenvoudiger en betrouwbaarder. Ze behoeven geen aanvullende instellingen omdat ze zijn verzameld door de host VM, terwijl de Gast metrische gegevens, moet u voor het installeren van de [Windows Azure Diagnostics extensie](../virtual-machines/windows/extensions-diagnostics-template.md) of de [Linux Azure Diagnostics extensie](../virtual-machines/linux/diagnostic-extension.md)in de Gast-VM. Een veelvoorkomende reden Gast metrische gegevens gebruiken in plaats van de host metrische gegevens is dat Gast metrische gegevens bestaan uit een grotere selectie van metrische gegevens dan host metrische gegevens. Een voorbeeld is geheugenverbruik hoger metrische gegevens, die alleen beschikbaar via de Gast metrische gegevens. De metrische gegevens van ondersteunde host staan [hier](../monitoring-and-diagnostics/monitoring-supported-metrics.md), en veelgebruikte Gast metrische gegevens staan [hier](../monitoring-and-diagnostics/insights-autoscale-common-metrics.md). Dit artikel laat zien hoe u wijzigt de [minimale levensvatbaar schaalset sjabloon](./virtual-machine-scale-sets-mvss-start.md) automatisch schalen regels op basis van de Gast metrische gegevens voor Linux-schaalsets.

## <a name="change-the-template-definition"></a>De sjabloondefinitie wijzigen

De minimale levensvatbaar scale set sjabloon kan worden gezien [hier](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json), en de sjabloon voor het implementeren van de schaal Linux ingesteld met automatisch schalen op basis van het gastbesturingssysteem kan worden bekeken [hier](https://raw.githubusercontent.com/gatneil/mvss/guest-based-autoscale-linux/azuredeploy.json). We bekijken de diff gebruikt voor het maken van deze sjabloon (`git diff minimum-viable-scale-set existing-vnet`) stuk door stuk:

Voeg eerst de parameters voor `storageAccountName` en `storageAccountSasToken`. De agent diagnostics slaat metrische gegevens op in een [tabel](../cosmos-db/table-storage-how-to-use-dotnet.md) in dit opslagaccount. Vanaf de Diagnostics Linux Agent versie 3.0, met behulp van een toegangssleutel voor opslag is niet langer ondersteund. Gebruik in plaats daarvan een [SAS-Token](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

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

Wijzig vervolgens de schaalaanpassingsset `extensionProfile` om op te nemen van de extensie voor diagnostische gegevens. Geef de bron-ID van de schaal instelt voor het verzamelen van meetgegevens van, evenals de storage-account en SAS-token te gebruiken voor het opslaan van de metrische gegevens in deze configuratie. Geef op hoe vaak de metrische gegevens worden geaggregeerd (in dit geval wordt elke minuut) en welke metrische gegevens bijhouden (in dit geval, percentage gebruikt geheugen). Zie voor meer informatie over deze configuratie gedetailleerde en metrische gegevens dan percentage geheugen gebruikt, [deze documentatie](../virtual-machines/linux/diagnostic-extension.md).

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

Voeg een `autoscaleSettings` bron configureren voor automatisch schalen op basis van deze metrische gegevens. Deze bron heeft een `dependsOn` component die verwijst naar de schaal instellen om ervoor te zorgen dat de schaalaanpassingsset voordat u probeert te schalen deze bestaat. Als u een andere waarde automatisch te schalen op kiest, gebruikt u de `counterSpecifier` van de configuratie voor de uitbreiding van diagnostische gegevens als de `metricName` in de configuratie voor automatisch schalen. Zie voor meer informatie over de configuratie voor automatisch schalen, de [aanbevolen procedures voor automatisch schalen](..//monitoring-and-diagnostics/insights-autoscale-best-practices.md) en de [Azure Monitor REST-API-naslagdocumentatie](https://msdn.microsoft.com/library/azure/dn931928.aspx).

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
