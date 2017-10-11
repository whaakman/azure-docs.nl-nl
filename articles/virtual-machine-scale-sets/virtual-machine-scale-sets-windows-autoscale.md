---
title: Automatisch schalen Windows virtuele-Machineschaalsets | Microsoft Docs
description: Automatisch schalen instellen voor een Windows virtuele-Machineschaalset met Azure PowerShell
services: virtual-machine-scale-sets
documentationcenter: 
author: Thraka
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 88886cad-a2f0-46bc-8b58-32ac2189fc93
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/27/2016
ms.author: adegeo
ms.openlocfilehash: 91f731bec46c005221f4e66e95822994070d4c26
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="automatically-scale-machines-in-a-virtual-machine-scale-set"></a>Automatisch schalen machines in een virtuele-machineschaalset
Virtuele-machineschaalsets kunnen gemakkelijk te implementeren en beheren van identieke virtuele machines als een set. Schaalsets bieden een uiterst schaalbare en aanpasbare berekeningslaag voor toepassingen die grootschalig en installatiekopieën voor Windows-platform, installatiekopieën van virtuele Linux-platform, aangepaste installatiekopieën en -extensies ondersteunen. Zie voor meer informatie over schaalsets [virtuele Machine Scale ingesteld](virtual-machine-scale-sets-overview.md).

Deze zelfstudie laat zien hoe u een scale set Windows virtuele machines maken en de machines automatisch schalen in de set. Hebt u de schaal en instellen schalen door een Azure Resource Manager-sjabloon maken en implementeren met behulp van Azure PowerShell. Zie [Azure Resource Manager-sjablonen samenstellen](../azure-resource-manager/resource-group-authoring-templates.md) voor meer informatie over sjablonen. Zie voor meer informatie over automatisch schalen van schaalsets, [automatisch schalen en Virtual Machine-schaal stelt](virtual-machine-scale-sets-autoscale-overview.md).

In dit artikel implementeert u de volgende bronnen en -extensies:

* Microsoft.Storage/storageAccounts
* Microsoft.Network/virtualNetworks
* Microsoft.Network/publicIPAddresses
* Microsoft.Network/loadBalancers
* Microsoft.Network/networkInterfaces
* Microsoft.Compute/virtualMachines
* Microsoft.Compute/virtualMachineScaleSets
* Microsoft.Insights.VMDiagnosticsSettings
* Microsoft.Insights/autoscaleSettings

Zie voor meer informatie over het Resource Manager-resources [Azure Resource Manager versus klassieke implementatie](../azure-resource-manager/resource-manager-deployment-model.md).

## <a name="step-1-install-azure-powershell"></a>Stap 1: Azure PowerShell installeren
Zie [installeren en configureren van Azure PowerShell](/powershell/azure/overview) selecteren voor informatie over het installeren van de meest recente versie van Azure PowerShell, uw abonnement en aanmelden bij Azure.

## <a name="step-2-create-a-resource-group-and-a-storage-account"></a>Stap 2: Een resourcegroep en een opslagaccount maken
1. **Een resourcegroep maken** – alle bronnen moeten worden geïmplementeerd in een resourcegroep. Gebruik [New-AzureRmResourceGroup](https://msdn.microsoft.com/library/mt603739.aspx) voor het maken van een resourcegroep met de naam **vmsstestrg1**.
2. **Een opslagaccount maken** – dit opslagaccount is waar de sjabloon is opgeslagen. Gebruik [nieuw AzureRmStorageAccount](https://msdn.microsoft.com/library/mt607148.aspx) voor het maken van een opslagaccount met de naam **vmsstestsa**.

## <a name="step-3-create-the-template"></a>Stap 3: De sjabloon maken
Een Azure Resource Manager-sjabloon maakt het u mogelijk te implementeren en beheren van de Azure-resources samen met behulp van een JSON-beschrijving van de resources en de bijbehorende implementatie-parameters.

1. In uw favoriete editor het bestand C:\VMSSTemplate.json maken en voeg de initiële JSON-structuur ter ondersteuning van de sjabloon.

    ```json
    {
      "$schema":"http://schema.management.azure.com/schemas/2014-04-01-preview/VM.json",
      "contentVersion": "1.0.0.0",
      "parameters": {
      },
      "variables": {
      },
      "resources": [
      ]
    }
    ```

2. Parameters zijn niet altijd vereist, maar ze bieden een manier voor het invoeren van waarden wanneer de sjabloon wordt geïmplementeerd. Deze parameters onder het bovenliggende element voor parameters die u hebt toegevoegd aan de sjabloon toevoegen.

    ```json   
    "vmName": { "type": "string" },
    "vmSSName": { "type": "string" },
    "instanceCount": { "type": "string" },
    "adminUsername": { "type": "string" },
    "adminPassword": { "type": "securestring" },
    "resourcePrefix": { "type": "string" }
    ```
   
    * Een naam voor de afzonderlijke virtuele machine die wordt gebruikt voor toegang tot de machines in de schaal is ingesteld.
    * De naam van het opslagaccount waarin de sjabloon is opgeslagen.
    * Het aantal virtuele machines in eerste instantie maken in de schaalset.
    * De naam en het wachtwoord van de administrator-account op de virtuele machines.
    * Een voorvoegsel voor de resources die worden gemaakt ter ondersteuning van de schaal is ingesteld.

3. Variabelen kunnen worden gebruikt in een sjabloon om op te geven waarden die kunnen worden regelmatig gewijzigd of waarden die moeten worden gemaakt uit een combinatie van parameterwaarden. Deze variabelen onder het bovenliggende element van variabelen die u hebt toegevoegd aan de sjabloon toevoegen.

    ```json
    "dnsName1": "[concat(parameters('resourcePrefix'),'dn1')]",
    "dnsName2": "[concat(parameters('resourcePrefix'),'dn2')]",
    "publicIP1": "[concat(parameters('resourcePrefix'),'ip1')]",
    "publicIP2": "[concat(parameters('resourcePrefix'),'ip2')]",
    "loadBalancerName": "[concat(parameters('resourcePrefix'),'lb1')]",
    "virtualNetworkName": "[concat(parameters('resourcePrefix'),'vn1')]",
    "nicName": "[concat(parameters('resourcePrefix'),'nc1')]",
    "lbID": "[resourceId('Microsoft.Network/loadBalancers',variables('loadBalancerName'))]",
    "frontEndIPConfigID": "[concat(variables('lbID'),'/frontendIPConfigurations/loadBalancerFrontEnd')]",
    "storageAccountSuffix": [ "a", "g", "m", "s", "y" ],
    "diagnosticsStorageAccountName": "[concat(parameters('resourcePrefix'), 'a')]",
    "accountid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/', resourceGroup().name,'/providers/','Microsoft.Storage/storageAccounts/', variables('diagnosticsStorageAccountName'))]",
      "wadlogs": "<WadCfg> <DiagnosticMonitorConfiguration overallQuotaInMB=\"4096\" xmlns=\"http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration\"> <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter=\"Error\"/> <WindowsEventLog scheduledTransferPeriod=\"PT1M\" > <DataSource name=\"Application!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"Security!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"System!*[System[(Level = 1 or Level = 2)]]\" /></WindowsEventLog>",
    "wadperfcounter": "<PerformanceCounters scheduledTransferPeriod=\"PT1M\"><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% Processor Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU utilization\" locale=\"en-us\"/></PerformanceCounterConfiguration></PerformanceCounters>",
    "wadcfgxstart": "[concat(variables('wadlogs'),variables('wadperfcounter'),'<Metrics resourceId=\"')]",
    "wadmetricsresourceid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name ,'/providers/','Microsoft.Compute/virtualMachineScaleSets/',parameters('vmssName'))]",
    "wadcfgxend": "[concat('\"><MetricAggregation scheduledTransferPeriod=\"PT1H\"/><MetricAggregation scheduledTransferPeriod=\"PT1M\"/></Metrics></DiagnosticMonitorConfiguration></WadCfg>')]"
    ```
   
    * DNS-namen die door de netwerkinterfaces worden gebruikt.

        * De namen van de IP-adres en de voorvoegsels voor het virtuele netwerk en de subnetten.
        * De namen en id's van het virtuele netwerk en voor de load balancer, netwerkinterfaces.
        * Namen van opslagaccounts voor de accounts die zijn gekoppeld aan de computers in de schaal is ingesteld.
        * Instellingen voor de extensie voor diagnostische gegevens die op de virtuele machines is geïnstalleerd. Zie voor meer informatie over de extensie voor diagnostische gegevens [maken van een virtuele Windows-machine met de controle en diagnostische gegevens met Azure Resource Manager-sjabloon](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

4. De storage-account resource onder het bovenliggende element van resources die u hebt toegevoegd aan de sjabloon toevoegen. Deze sjabloon worden gebruikt voor een lus aanbevolen vijf storage-accounts maken waarin het besturingssysteem schijven en diagnostische gegevens worden opgeslagen. Deze set accounts kan maximaal 100 virtuele machines in een schaalset, de huidige maximale is ondersteunen. De naam van elk opslagaccount is met een letter eenheidsaanduiding die is gedefinieerd in de variabelen die in combinatie met het voorvoegsel op dat u in de parameters voor de sjabloon opgeeft.

    ```json
    {
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[concat(parameters('resourcePrefix'), variables('storageAccountSuffix')[copyIndex()])]",
      "apiVersion": "2015-06-15",
      "copy": {
        "name": "storageLoop",
        "count": 5
      },
      "location": "[resourceGroup().location]",
      "properties": { "accountType": "Standard_LRS" }
    },
    ```

5. Het virtuele netwerk resource toevoegen. Zie voor meer informatie [Netwerkresourceprovider](../virtual-network/resource-groups-networking.md).

    ```json
    {
      "apiVersion": "2015-06-15",
      "type": "Microsoft.Network/virtualNetworks",
      "name": "[variables('virtualNetworkName')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "addressSpace": { "addressPrefixes": [ "10.0.0.0/16" ] },
        "subnets": [
          {
            "name": "subnet1",
            "properties": { "addressPrefix": "10.0.0.0/24" }
          }
        ]
      }
    },
    ```

6. Het openbare IP-adres resources die worden gebruikt door de load balancer en network interface toevoegen.

    ```json
    {
      "apiVersion": "2016-03-30",
      "type": "Microsoft.Network/publicIPAddresses",
      "name": "[variables('publicIP1')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "publicIPAllocationMethod": "Dynamic",
        "dnsSettings": {
          "domainNameLabel": "[variables('dnsName1')]"
        }
      }
    },
    {
      "apiVersion": "2016-03-30",
      "type": "Microsoft.Network/publicIPAddresses",
      "name": "[variables('publicIP2')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "publicIPAllocationMethod": "Dynamic",
        "dnsSettings": {
          "domainNameLabel": "[variables('dnsName2')]"
        }
      }
    },
    ```

7. De load balancer-resource die wordt gebruikt door de schaalaanpassingsset toevoegen. Zie voor meer informatie [Azure Resource Manager-ondersteuning voor de Load Balancer](../load-balancer/load-balancer-arm.md).

    ```json   
    {
      "apiVersion": "2015-06-15",
      "name": "[variables('loadBalancerName')]",
      "type": "Microsoft.Network/loadBalancers",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIP1'))]"
      ],
      "properties": {
        "frontendIPConfigurations": [
          {
            "name": "loadBalancerFrontEnd",
            "properties": {
              "publicIPAddress": {
                "id": "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIP1'))]"
              }
            }
          }
        ],
        "backendAddressPools": [ { "name": "bepool1" } ],
        "inboundNatPools": [
          {
            "name": "natpool1",
            "properties": {
              "frontendIPConfiguration": {
                "id": "[variables('frontEndIPConfigID')]"
              },
              "protocol": "tcp",
              "frontendPortRangeStart": 50000,
              "frontendPortRangeEnd": 50500,
              "backendPort": 3389
            }
          }
        ]
      }
    },
    ```

8. De netwerkbron van de interface die wordt gebruikt door de afzonderlijke virtuele machine toevoegen. Omdat computers in een scale-set niet toegankelijk zijn via een openbaar IP-adres, wordt een aparte virtuele machine gemaakt in hetzelfde virtuele netwerk voor extern toegang tot de machines.

    ```json  
    {
      "apiVersion": "2016-03-30",
      "type": "Microsoft.Network/networkInterfaces",
      "name": "[variables('nicName')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIP2'))]",
        "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
      ],
      "properties": {
        "ipConfigurations": [
          {
            "name": "ipconfig1",
            "properties": {
              "privateIPAllocationMethod": "Dynamic",
              "publicIPAddress": {
                "id": "[resourceId('Microsoft.Network/publicIPAddresses', variables('publicIP2'))]"
              },
              "subnet": {
                "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/virtualNetworks/',variables('virtualNetworkName'),'/subnets/subnet1')]"
              }
            }
          }
        ]
      }
    },
    ```

9. De afzonderlijke virtuele machine in hetzelfde netwerk bevindt als de scale-set toevoegen.

    ```json
    {
      "apiVersion": "2016-03-30",
      "type": "Microsoft.Compute/virtualMachines",
      "name": "[parameters('vmName')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "storageLoop",
        "[concat('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
      ],
      "properties": {
        "hardwareProfile": { "vmSize": "Standard_A1" },
        "osProfile": {
          "computername": "[parameters('vmName')]",
          "adminUsername": "[parameters('adminUsername')]",
          "adminPassword": "[parameters('adminPassword')]"
        },
        "storageProfile": {
          "imageReference": {
            "publisher": "MicrosoftWindowsServer",
            "offer": "WindowsServer",
            "sku": "2012-R2-Datacenter",
            "version": "latest"
          },
          "osDisk": {
            "name": "[concat(parameters('resourcePrefix'), 'os1')]",
            "vhd": {
              "uri":  "[concat('https://',parameters('resourcePrefix'),'a.blob.core.windows.net/vhds/',parameters('resourcePrefix'),'os1.vhd')]"
            },
            "caching": "ReadWrite",
            "createOption": "FromImage"        
          }
        },
        "networkProfile": {
          "networkInterfaces": [
            {
              "id": "[resourceId('Microsoft.Network/networkInterfaces',variables('nicName'))]"
            }
          ]
        }
      }
    },
    ```

10. De virtuele-machineschaalset resource toevoegen en geef de extensie voor diagnostische gegevens die is geïnstalleerd op alle virtuele machines in de schaalset. Veel van de instellingen voor deze resource zijn vergelijkbaar met de bron van de virtuele machine. De belangrijkste verschillen zijn de capaciteit-element waarmee het aantal virtuele machines in de schaalset en upgradePolicy waarmee wordt aangegeven hoe updates worden aangebracht in virtuele machines. De set scale is niet gemaakt totdat alle opslagaccounts zijn gemaakt met het element dependsOn zoals is opgegeven.

    ```json
    {
      "type": "Microsoft.Compute/virtualMachineScaleSets",
      "apiVersion": "2016-03-30",
      "name": "[parameters('vmSSName')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "storageLoop",
        "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]",
        "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]"
      ],
      "sku": {
        "name": "Standard_A1",
        "tier": "Standard",
        "capacity": "[parameters('instanceCount')]"
      },
      "properties": {
        "upgradePolicy": {
          "mode": "Manual"
        },
        "virtualMachineProfile": {
          "storageProfile": {
            "osDisk": {
              "vhdContainers": [
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[0], '.blob.core.windows.net/vhds')]",
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[1], '.blob.core.windows.net/vhds')]",
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[2], '.blob.core.windows.net/vhds')]",
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[3], '.blob.core.windows.net/vhds')]",
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[4], '.blob.core.windows.net/vhds')]"
              ],
              "name": "vmssosdisk",
              "caching": "ReadOnly",
              "createOption": "FromImage"
            },
            "imageReference": {
              "publisher": "MicrosoftWindowsServer",
              "offer": "WindowsServer",
              "sku": "2012-R2-Datacenter",
              "version": "latest"
            }
          },
          "osProfile": {
            "computerNamePrefix": "[parameters('vmSSName')]",
            "adminUsername": "[parameters('adminUsername')]",
            "adminPassword": "[parameters('adminPassword')]"
          },
          "networkProfile": {
            "networkInterfaceConfigurations": [
              {
                "name": "networkconfig1",
                "properties": {
                  "primary": "true",
                  "ipConfigurations": [
                    {
                      "name": "ip1",
                      "properties": {
                        "subnet": {
                          "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/virtualNetworks/',variables('virtualNetworkName'),'/subnets/subnet1')]"
                        },
                        "loadBalancerBackendAddressPools": [
                          {
                            "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/loadBalancers/',variables('loadBalancerName'),'/backendAddressPools/bepool1')]"
                          }
                        ],
                        "loadBalancerInboundNatPools": [
                          {
                            "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/loadBalancers/',variables('loadBalancerName'),'/inboundNatPools/natpool1')]"
                          }
                        ]
                      }
                    }
                  ]
                }
              }
            ]
          },
          "extensionProfile": {
            "extensions": [
              {
                "name": "Microsoft.Insights.VMDiagnosticsSettings",
                "properties": {
                  "publisher": "Microsoft.Azure.Diagnostics",
                  "type": "IaaSDiagnostics",
                  "typeHandlerVersion": "1.5",
                  "autoUpgradeMinorVersion": true,
                  "settings": {
                    "xmlCfg": "[base64(concat(variables('wadcfgxstart'),variables('wadmetricsresourceid'),variables('wadcfgxend')))]",
                    "storageAccount": "[variables('diagnosticsStorageAccountName')]"
                  },
                  "protectedSettings": {
                    "storageAccountName": "[variables('diagnosticsStorageAccountName')]",
                    "storageAccountKey": "[listkeys(variables('accountid'), '2015-06-15').key1]",
                    "storageAccountEndPoint": "https://core.windows.net"
                  }
                }
              }
            ]
          }
        }
      }
    },
    ```

11. De resource autoscaleSettings die definieert hoe de schaalaanpassingsset worden aangepast op basis van het processorgebruik op de virtuele machines in de schaalset toevoegen.

    ```json
    {
      "type": "Microsoft.Insights/autoscaleSettings",
      "apiVersion": "2015-04-01",
      "name": "[concat(parameters('resourcePrefix'),'as1')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachineScaleSets/',parameters('vmSSName'))]"
      ],
      "properties": {
        "enabled": true,
        "name": "[concat(parameters('resourcePrefix'),'as1')]",
        "profiles": [
          {
            "name": "Profile1",
            "capacity": {
              "minimum": "1",
              "maximum": "10",
              "default": "1"
            },
            "rules": [
              {
                "metricTrigger": {
                  "metricName": "\\Processor(_Total)\\% Processor Time",
                  "metricNamespace": "",
                  "metricResourceUri": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Compute/virtualMachineScaleSets/',parameters('vmSSName'))]",
                  "timeGrain": "PT1M",
                  "statistic": "Average",
                  "timeWindow": "PT5M",
                  "timeAggregation": "Average",
                  "operator": "GreaterThan",
                  "threshold": 50.0
                },
                "scaleAction": {
                  "direction": "Increase",
                  "type": "ChangeCount",
                  "value": "1",
                  "cooldown": "PT5M"
                }
              }
            ]
          }
        ],
        "targetResourceUri": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/', resourceGroup().name,'/providers/Microsoft.Compute/virtualMachineScaleSets/',parameters('vmSSName'))]"
      }
    }
    ```

    Deze waarden zijn belangrijk voor deze zelfstudie:
    
    * **metricName**  
    Deze waarde is hetzelfde als het prestatiemeteritem dat is gedefinieerd in de variabele wadperfcounter. De extensie voor diagnostische gegevens worden verzameld met behulp van die variabele, de **processor(_Totaal)\% processortijd** teller.
    
    * **metricResourceUri**  
    Deze waarde is de resource-id van de virtuele-machineschaalset.
    
    * **timeGrain**  
    Deze waarde is de granulatie van de metrische gegevens die worden verzameld. In deze sjabloon wordt deze ingesteld op 1 minuut.
    
    * **statistiek**  
    Deze waarde bepaalt hoe de metrische gegevens voor het automatisch vergroten/verkleinen actie worden gecombineerd. De mogelijke waarden zijn: gemiddelde, Min, Max. Het gemiddelde totale CPU-gebruik van de virtuele machines in deze sjabloon is verzameld.
    
    * **waarde voor timeWindow**  
    Deze waarde is het tijdbereik waarin de gegevens worden verzameld. Deze moet tussen 5 minuten en 12 uur.
    
    * **TimeAggregation van**  
    de waarde bepaalt hoe de gegevens die worden verzameld gedurende een bepaalde periode moeten worden gecombineerd. De standaardwaarde is de gemiddelde. De mogelijke waarden zijn: gemiddeld, Minimum, Maximum, laatste, totaal, Count.
    
    * **operator**  
    Deze waarde is de operator die wordt gebruikt om de metrische gegevens en de drempelwaarde te vergelijken. De mogelijke waarden zijn: gelijk is aan NotEquals, groter dan, GreaterThanOrEqual, LessThan, LessThanOrEqual.
    
    * **drempelwaarde**  
    Deze waarde is de waarde die de schaalactie activeert. Machines worden in deze sjabloon wordt toegevoegd aan de schaal ingesteld als het gemiddelde CPU-gebruik door machines in de set meer dan 50 is %.
    
    * **richting**  
    Deze waarde bepaalt de actie die wordt uitgevoerd wanneer de drempelwaarde wordt bereikt. De mogelijke waarden zijn vergroten of verkleinen. In deze sjabloon wordt het aantal virtuele machines in de schaalset verhoogd als de drempelwaarde meer dan 50% van de gedefinieerde tijdsduur is.
    
    * **type**  
    Deze waarde is het type actie die moet worden uitgevoerd en moet worden ingesteld op ChangeCount.
    
    * **waarde**  
    Deze waarde is het aantal virtuele machines die worden toegevoegd of verwijderd uit de schaalaanpassingsset. Deze waarde moet 1 of hoger. De standaardwaarde is 1. In deze sjabloon, het aantal machines in de schaal instellen toeneemt 1 als de drempelwaarde wordt voldaan.
    
    * **cooldown**  
    Deze waarde is de hoeveelheid tijd sinds de laatste vergroten/verkleinen actie wachten voordat de volgende actie optreedt. Deze waarde moet tussen 1 minuut en één week.

12. Sla het sjabloonbestand.    

## <a name="step-4-upload-the-template-to-storage"></a>Stap 4: De sjabloon uploaden naar de opslag
De sjabloon kan worden geüpload, zolang u weet dat de naam en de primaire sleutel van het opslagaccount dat u in stap 1 hebt gemaakt.

1. Stel een variabele die u specificeert de naam van het opslagaccount dat u in stap 1 hebt gemaakt in de Microsoft Azure PowerShell-venster.
   
    ```powershell
    $storageAccountName = "vmstestsa"
    ```

2. Instellen van een variabele waarin de primaire sleutel van het opslagaccount.
   
    ```powershell
    $storageAccountKey = "<primary-account-key>"
    ```
   
   U kunt deze sleutel verkrijgen door te klikken op het sleutelpictogram wanneer u de opslagbronnen account bekijkt in de Azure-portal.
3. Maak het storage account context-object dat wordt gebruikt voor bewerkingen met de storage-account te valideren.
   
    ```powershell
    $ctx = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
    ```

4. De container voor het opslaan van de sjabloon maken.

    ```powershell
    $containerName = "templates"
    New-AzureStorageContainer -Name $containerName -Context $ctx  -Permission Blob
    ```

5. Het sjabloonbestand uploaden naar de nieuwe container.

    ```powershell   
    $blobName = "VMSSTemplate.json"
    $fileName = "C:\" + $BlobName
    Set-AzureStorageBlobContent -File $fileName -Container $containerName -Blob  $blobName -Context $ctx
    ```

## <a name="step-5-deploy-the-template"></a>Stap 5: De sjabloon implementeren
Nu dat u de sjabloon hebt gemaakt, kunt u beginnen met het implementeren van de resources. Gebruik deze opdracht om het proces te starten:

```powershell
New-AzureRmResourceGroupDeployment -Name "vmsstestdp1" -ResourceGroupName "vmsstestrg1" -TemplateUri "https://vmsstestsa.blob.core.windows.net/templates/VMSSTemplate.json"
```

Wanneer u op invoert, wordt u gevraagd waarden op te geven voor de variabelen die u hebt toegewezen. Deze waarden bevatten:

```powershell
vmName: vmsstestvm1
  vmSSName: vmsstest1
  instanceCount: 5
  adminUserName: vmadmin1
  adminPassword: VMpass1
  resourcePrefix: vmsstest
```

Het duurt ongeveer 15 minuten voor alle resources worden geïmplementeerd.

> [!NOTE]
> U kunt ook de mogelijkheid van de portal gebruiken voor het implementeren van de resources. Gebruik deze koppeling: ' https://portal.azure.com/#create/Microsoft.Template/uri/<link to VM Scale Set JSON template>'
> 
> 

## <a name="step-6-monitor-resources"></a>Stap 6: Monitor resources
U kunt sommige informatie ophalen over virtuele-machineschaalsets met behulp van deze methoden:

* De Azure portal - momenteel krijgt u een beperkte hoeveelheid informatie via de portal.
* De [Azure Resource Explorer](https://resources.azure.com/) -dit hulpprogramma is het meest geschikt voor de huidige status van uw scale set verkennen. Volg dit pad en ziet u de instantieweergave van de schaalaanpassingsset die u hebt gemaakt:
  
    abonnementen > {uw abonnement} > resourceGroups > vmsstestrg1 > providers > Microsoft.Compute > virtualMachineScaleSets > vmsstest1 > virtuele machines

* Azure PowerShell - Gebruik deze opdracht kunt u enkele gegevens:

  ```powershell
  Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"
  ```
  
  of
  
  ```powershell
  Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceView
  ```

* Verbinding maken met de afzonderlijke virtuele machine net zoals u zou een andere machine en klikt u op afstand toegang de virtuele machines in de schaal instelt tot voor het bewaken van afzonderlijke processen.

> [!NOTE]
> Een complete REST-API voor het verkrijgen van informatie over schaalsets vindt u in [virtuele Machine Scale ingesteld](https://msdn.microsoft.com/library/mt589023.aspx)

## <a name="step-7-remove-the-resources"></a>Stap 7: De resources verwijderen
Omdat u in rekening voor resources die worden gebruikt in Azure gebracht, maar het is altijd een goede gewoonte resources verwijderen die niet langer nodig zijn. U hoeft niet elke resource afzonderlijk verwijderen uit een resourcegroep. U kunt de resourcegroep verwijderen en de bijhorende resources automatisch worden verwijderd.

  ```powershell
  Remove-AzureRmResourceGroup -Name vmsstestrg1
  ```

Als u behouden van de resourcegroep wilt, kunt u de schaal instelt alleen verwijderen.

  ```powershell
  Remove-AzureRmVmss -ResourceGroupName "resource group name" –VMScaleSetName "scale set name"
  ```

## <a name="next-steps"></a>Volgende stappen
* Beheren van de schaalaanpassingsset die u zojuist hebt gemaakt met de informatie in [beheren van virtuele machines in een virtuele-Machineschaalset](virtual-machine-scale-sets-windows-manage.md).
* Raadpleeg [Verticaal automatisch schalen met virtuele-machineschaalsets](virtual-machine-scale-sets-vertical-scale-reprovision.md) voor meer informatie over verticaal schalen
* Voorbeelden van Azure Monitor bewakingsfuncties in [Azure Monitor PowerShell snel starten-voorbeelden](../monitoring-and-diagnostics/insights-powershell-samples.md)
* Meer informatie over functies in [acties automatisch schalen gebruiken voor het verzenden van e-mail en -webhook-waarschuwingsmeldingen in de Azure-Monitor](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md)
* Meer informatie over hoe [controlelogboeken voor gebruik met het verzenden van e-mail en webhook-waarschuwingsmeldingen in de Azure-Monitor](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md)

