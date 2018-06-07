---
title: Procedure voor de upgrade/migratie de SKU van PrimaryNodeType aan hogere SKU's | Microsoft Docs
description: Informatie over het upgraden of te migreren de SKU voor PrimaryNodeType aan hogere SKU's met behulp van PowerShell-opdrachten en CLI-opdrachten.
services: service-fabric
documentationcenter: .net
author: v-rachiw
manager: navya
editor: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/08/2018
ms.author: v-rachiw
ms.openlocfilehash: 96956543a44b6d5d967e3bae3fd833b08baf3d6f
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34642730"
---
# <a name="upgrademigrate-the-sku-for-primary-node-type-to-higher-sku"></a>Upgrade/migreren de SKU voor het primaire knooppunttype op hogere SKU

Dit artikel wordt beschreven hoe u de SKU van primaire knooppunttype van service fabric-cluster op hogere SKU met Azure PowerShell upgrade/migreren

## <a name="add-a-new-virtual-machine-scale-set"></a>Voeg een nieuwe virtuele-machineschaalset

Implementeer een nieuwe virtuele-machineschaalset en Load Balancer. De Service Fabric-extensie-configuratie (met name het knooppunttype) van nieuwe virtuele-machineschaalset moet hetzelfde als de oude schaal ingesteld die u probeert te upgraden. Controleer in de Service Fabric explorer uw nieuwe knooppunten zijn beschikbaar

#### <a name="azure-powershell"></a>Azure PowerShell

Het volgende voorbeeld wordt een Azure PowerShell gebruikt voor het implementeren van de bijgewerkte Resource Manager-sjabloon *template.json* met behulp van de resourcegroep met de naam *myResourceGroup*:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName myResourceGroup -TemplateFile template.json -TemplateParameterFile parameters.json
```

#### <a name="azure-cli"></a>Azure-CLI

De volgende opdracht maakt gebruik van Azure Service Fabric CLI voor het implementeren van de bijgewerkte Resource Manager-sjabloon *template.json* met behulp van de resourcegroep met de naam *myResourceGroup*:

```CLI
az group deployment create --resource-group myResourceGroup --template-file template.json --parameters parameters.json
```

Raadpleeg de volgende voorbeeld json-sjabloon voor het toevoegen van nieuwe virtuele machine scale set resource met het primaire knooppunttype in een bestaand cluster wijzigen

```json
        {
            "apiVersion": "[variables('vmssApiVersion')]",
            "type": "Microsoft.Compute/virtualMachineScaleSets",
            "name": "[parameters('vmNodeType2Name')]",
            "location": "[parameters('computeLocation')]",
            "dependsOn": [
                "[concat('Microsoft.Network/virtualNetworks/', parameters('virtualNetworkName'))]",
                "[concat('Microsoft.Network/loadBalancers/', concat('LB','-', parameters('clusterName'),'-',parameters('vmNodeType2Name')))]",
                "[concat('Microsoft.Storage/storageAccounts/', parameters('supportLogStorageAccountName'))]",
                "[concat('Microsoft.Storage/storageAccounts/', parameters('applicationDiagnosticsStorageAccountName'))]"
            ],
            "properties": {
                "overprovision": "[parameters('overProvision')]",
                "upgradePolicy": {
                    "mode": "Automatic"
                },
                "virtualMachineProfile": {
                    "extensionProfile": {
                        "extensions": [
                            {
                                "name": "[concat(parameters('vmNodeType2Name'),'_ServiceFabricNode')]",
                                "properties": {
                                    "type": "ServiceFabricNode",
                                    "autoUpgradeMinorVersion": true,
                                    "protectedSettings": {
                                        "StorageAccountKey1": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('supportLogStorageAccountName')),'2015-05-01-preview').key1]",
                                        "StorageAccountKey2": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('supportLogStorageAccountName')),'2015-05-01-preview').key2]"
                                    },
                                    "publisher": "Microsoft.Azure.ServiceFabric",
                                    "settings": {
                                        "clusterEndpoint": "[reference(parameters('clusterName')).clusterEndpoint]",
                                        "nodeTypeRef": "[parameters('vmNodeType0Name')]",
                                        "dataPath": "D:\\\\SvcFab",
                                        "durabilityLevel": "Bronze",
                                        "enableParallelJobs": true,
                                        "nicPrefixOverride": "[parameters('subnet0Prefix')]",
                                        "certificate": {
                                            "thumbprint": "[parameters('certificateThumbprint')]",
                                            "x509StoreName": "[parameters('certificateStoreValue')]"
                                        }
                                    },
                                    "typeHandlerVersion": "1.0"
                                }
                            },
                            {
                                "name": "[concat('VMDiagnosticsVmExt','_vmNodeType2Name')]",
                                "properties": {
                                    "type": "IaaSDiagnostics",
                                    "autoUpgradeMinorVersion": true,
                                    "protectedSettings": {
                                        "storageAccountName": "[parameters('applicationDiagnosticsStorageAccountName')]",
                                        "storageAccountKey": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('applicationDiagnosticsStorageAccountName')),'2015-05-01-preview').key1]",
                                        "storageAccountEndPoint": "https://core.windows.net/"
                                    },
                                    "publisher": "Microsoft.Azure.Diagnostics",
                                    "settings": {
                                        "WadCfg": {
                                            "DiagnosticMonitorConfiguration": {
                                                "overallQuotaInMB": "50000",
                                                "EtwProviders": {
                                                    "EtwEventSourceProviderConfiguration": [
                                                        {
                                                            "provider": "Microsoft-ServiceFabric-Actors",
                                                            "scheduledTransferKeywordFilter": "1",
                                                            "scheduledTransferPeriod": "PT5M",
                                                            "DefaultEvents": {
                                                                "eventDestination": "ServiceFabricReliableActorEventTable"
                                                            }
                                                        },
                                                        {
                                                            "provider": "Microsoft-ServiceFabric-Services",
                                                            "scheduledTransferPeriod": "PT5M",
                                                            "DefaultEvents": {
                                                                "eventDestination": "ServiceFabricReliableServiceEventTable"
                                                            }
                                                        }
                                                    ],
                                                    "EtwManifestProviderConfiguration": [
                                                        {
                                                            "provider": "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8",
                                                            "scheduledTransferLogLevelFilter": "Information",
                                                            "scheduledTransferKeywordFilter": "4611686018427387904",
                                                            "scheduledTransferPeriod": "PT5M",
                                                            "DefaultEvents": {
                                                                "eventDestination": "ServiceFabricSystemEventTable"
                                                            }
                                                        }
                                                    ]
                                                }
                                            }
                                        },
                                        "StorageAccount": "[parameters('applicationDiagnosticsStorageAccountName')]"
                                    },
                                    "typeHandlerVersion": "1.5"
                                }
                            }
                        ]
                    },
                    "networkProfile": {
                        "networkInterfaceConfigurations": [
                            {
                                "name": "[concat(parameters('nicName'), '-2')]",
                                "properties": {
                                    "ipConfigurations": [
                                        {
                                            "name": "[concat(parameters('nicName'),'-',2)]",
                                            "properties": {
                                                "loadBalancerBackendAddressPools": [
                                                    {
                                                        "id": "[variables('lbPoolID2')]"
                                                    }
                                                ],
                                                "loadBalancerInboundNatPools": [
                                                    {
                                                        "id": "[variables('lbNatPoolID2')]"
                                                    }
                                                ],
                                                "subnet": {
                                                    "id": "[variables('subnet0Ref')]"
                                                }
                                            }
                                        }
                                    ],
                                    "primary": true
                                }
                            }
                        ]
                    },
                    "osProfile": {
                        "adminPassword": "[parameters('adminPassword')]",
                        "adminUsername": "[parameters('adminUsername')]",
                        "computernamePrefix": "[parameters('vmNodeType2Name')]",
                        "secrets": [
                            {
                                "sourceVault": {
                                    "id": "[parameters('sourceVaultValue')]"
                                },
                                "vaultCertificates": [
                                    {
                                        "certificateStore": "[parameters('certificateStoreValue')]",
                                        "certificateUrl": "[parameters('certificateUrlValue')]"
                                    }
                                ]
                            }
                        ]
                    },
                    "storageProfile": {
                        "imageReference": {
                            "publisher": "[parameters('vmImagePublisher')]",
                            "offer": "[parameters('vmImageOffer')]",
                            "sku": "[parameters('vmImageSku')]",
                            "version": "[parameters('vmImageVersion')]"
                        },
                        "osDisk": {
                            "caching": "ReadOnly",
                            "createOption": "FromImage",
                            "managedDisk": {
                                "storageAccountType": "[parameters('storageAccountType')]"
                            }
                        }
                    }
                }
            },
            "sku": {
                "name": "[parameters('vmNodeType2Size')]",
                "capacity": "[parameters('nt2InstanceCount')]",
                "tier": "Standard"
            },
            "tags": {
                "resourceType": "Service Fabric",
                "clusterName": "[parameters('clusterName')]"
            }
        },
```

## <a name="remove-old-virtual-machine-scale-set"></a>Verwijder de oude virtuele-machineschaalset

1. VM-instanties van de oude virtuele-machineschaalset instellen met opzet te verwijderen van knooppunt uitschakelen. Deze bewerking kan lang duren om te voltooien. U kunt in één keer uitschakelen en ze moeten in de wachtrij geplaatst. Wacht totdat alle knooppunten zijn uitgeschakeld. 
#### <a name="azure-powershell"></a>Azure PowerShell
Het volgende voorbeeld maakt gebruik van Azure Service Fabric PowerShell uitschakelen exemplaar van het knooppunt met de naam *NTvm1_0* van de oude virtuele-machineschaalset set met de naam *NTvm1*:
```powershell
Disable-ServiceFabricNode -NodeName NTvm1_0 -Intent RemoveNode
```
#### <a name="azure-cli"></a>Azure-CLI
De volgende opdracht maakt gebruik van Azure Service Fabric CLI uitschakelen exemplaar van het knooppunt met de naam *NTvm1_0* van de oude virtuele-machineschaalset set met de naam *NTvm1*:
```CLI
sfctl node disable --node-name "_NTvm1_0" --deactivation-intent RemoveNode
```
2. De volledige schaalset verwijderen. Wacht totdat de Inrichtingsstatus schaalset is geslaagd
#### <a name="azure-powershell"></a>Azure PowerShell
Het volgende voorbeeld wordt een Azure PowerShell gebruikt om te verwijderen van de volledige schaal set met de naam *NTvm1* van de resourcegroep met de naam *myResourceGroup*:
```powershell
Remove-AzureRmVmss -ResourceGroupName myResourceGroup -VMScaleSetName NTvm1
```
#### <a name="azure-cli"></a>Azure-CLI
De volgende opdracht maakt gebruik van Azure Service Fabric CLI verwijderen van de volledige schaal set met de naam *NTvm1* van de resourcegroep met de naam *myResourceGroup*:

```CLI
az vmss delete --name NTvm1 --resource-group myResourceGroup
```

## <a name="remove-load-balancer-related-to-old-scale-set"></a>Load Balancer die betrekking hebben op de oude schaalset verwijderen

Load Balancer die betrekking hebben op de oude schaalset verwijderen. Deze stap wordt een korte periode van uitvaltijd voor het cluster

#### <a name="azure-powershell"></a>Azure PowerShell

Het volgende voorbeeld wordt een Azure PowerShell gebruikt om te verwijderen van de Load balancer met de naam *LB-mijncluster-NTvm1* betrekking hebben op de oude scale ingesteld op basis van de resourcegroep met de naam *myResourceGroup*:

```powershell
Remove-AzureRmLoadBalancer -Name LB-myCluster-NTvm1 -ResourceGroupName myResourceGroup
```

#### <a name="azure-cli"></a>Azure-CLI

De volgende opdracht maakt gebruik van Azure Service Fabric CLI verwijderen van de Load balancer met de naam *LB-mijncluster-NTvm1* betrekking hebben op de oude scale ingesteld op basis van de resourcegroep met de naam *myResourceGroup*:

```CLI
az network lb delete --name LB-myCluster-NTvm1 --resource-group myResourceGroup
```

## <a name="remove-public-ip-related-to-old-scale-set"></a>Openbare IP-adres die zijn gerelateerd aan de oude schaalset verwijderen

Store DNS-instellingen van het openbare IP-adres betrekking hebben op de oude schaal instelt in variabele en verwijder vervolgens het openbare IP-adres

#### <a name="azure-powershell"></a>Azure PowerShell

Het volgende voorbeeld wordt een Azure PowerShell gebruikt voor het opslaan van DNS-instellingen van het openbare IP-adres met de naam *LBIP-mijncluster-NTvm1* in de variabele en het IP-adres verwijderen:

```powershell
$oldprimaryPublicIP = Get-AzureRmPublicIpAddress -Name LBIP-myCluster-NTvm1  -ResourceGroupName myResourceGroup
$primaryDNSName = $oldprimaryPublicIP.DnsSettings.DomainNameLabel
$primaryDNSFqdn = $oldprimaryPublicIP.DnsSettings.Fqdn
Remove-AzureRmPublicIpAddress -Name LBIP-myCluster-NTvm1 -ResourceGroupName myResourceGroup
```

#### <a name="azure-cli"></a>Azure-CLI

De volgende opdracht maakt gebruik van Azure Service Fabric CLI DNS-instellingen van het openbare IP-adres met de naam ophalen *LBIP-mijncluster-NTvm1* en verwijdert u het IP-adres:

```CLI
az network public-ip show --name LBIP-myCluster-NTvm1 --resource-group myResourceGroup
az network public-ip delete --name LBIP-myCluster-NTvm1 --resource-group myResourceGroup
```

## <a name="update-public-ip-address-related-to-new-scale-set"></a>Update openbaar IP-adres is gerelateerd aan de nieuwe scale set

Update DNS-instellingen van het openbare IP-adres die zijn gerelateerd aan de nieuwe scale ingesteld met de DNS-instellingen van het openbare IP-adres die betrekking hebben op de oude schaalset

#### <a name="azure-powershell"></a>Azure PowerShell
Het volgende voorbeeld wordt een Azure PowerShell gebruikt om bij te werken van DNS-instellingen van het openbare IP-adres met de naam *LBIP-mijncluster-NTvm3* met DNS-instellingen opgeslagen in de variabelen in een eerdere stap:

```powershell
$PublicIP = Get-AzureRmPublicIpAddress -Name LBIP-myCluster-NTvm3  -ResourceGroupName myResourceGroup
$PublicIP.DnsSettings.DomainNameLabel = $primaryDNSName
$PublicIP.DnsSettings.Fqdn = $primaryDNSFqdn
Set-AzureRmPublicIpAddress -PublicIpAddress $PublicIP
```

#### <a name="azure-cli"></a>Azure-CLI

De volgende opdracht maakt gebruik van Azure Service Fabric CLI DNS-instellingen van het openbare IP-adres met de naam bijwerken *LBIP-mijncluster-NTvm3* met DNS-instellingen van de oude openbare IP-adres in een eerdere stap hebt verzameld:

```CLI
az network public-ip update --name LBIP-myCluster-NTvm3 --resource-group myResourceGroup --dns-name myCluster
```

## <a name="remove-knowledge-of-service-fabric-node-from-fm"></a>Kennis van service fabric-knooppunt verwijderen uit FM

Service Fabric waarschuwen dat de knooppunten, zijn niet beschikbaar zijn verwijderd uit het cluster. (Deze opdracht uitvoeren voor alle VM-van de oude virtuele-machineschaalset exemplaren) (Als het niveau van de duurzaamheid van de oude virtuele-machineschaalset zilver of goud, deze stap mogelijk niet nodig. Aangezien deze stap wordt uitgevoerd door het systeem automatisch.)

#### <a name="azure-powershell"></a>Azure PowerShell
Het volgende voorbeeld maakt gebruik van Azure Service Fabric PowerShell op de hoogte van de service-infrastructuur die het knooppunt met de naam wilt *NTvm1_0* is verwijderd:

```powershell
Remove-ServiceFabricNodeState -NodeName NTvm1_0
```

#### <a name="azure-cli"></a>Azure-CLI

De volgende opdracht maakt gebruik van Azure Service Fabric CLI op de hoogte van de service-infrastructuur die het knooppunt met de naam wilt *NTvm1_0* is verwijderd:

```CLI
sfctl node remove-state --node-name _NTvm1_0
```
