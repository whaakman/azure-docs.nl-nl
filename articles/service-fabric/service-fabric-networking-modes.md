---
title: Netwerken modi voor services van Azure Service Fabric-container configureren | Microsoft Docs
description: Informatie over het instellen van de verschillende netwerken modi die worden ondersteund door Azure Service Fabric.
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: d552c8cd-67d1-45e8-91dc-871853f44fc6
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: f8e3af4e183952aaac5a8320966aab035b90a1a7
ms.sourcegitcommit: 7f1ce8be5367d492f4c8bb889ad50a99d85d9a89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2017
---
# <a name="service-fabric-container-networking-modes"></a>Netwerken modi voor service Fabric-container

Een Azure Service Fabric-cluster voor de container services maakt gebruik van **nat** netwerken modus standaard. Wanneer meer dan één containerservice op dezelfde poort luistert en nat-modus wordt gebruikt, kunnen implementatiefouten optreden. Ter ondersteuning van meerdere container op dezelfde poort luistert, Service Fabric biedt **Open** netwerken modus (versie 5.7 en hoger). In Open modus heeft elke containerservice een interne dynamisch toegewezen IP-adres dat ondersteuning biedt voor meerdere services op dezelfde poort luistert.  

Als u een containerservice met een statisch eindpunt in uw servicemanifest hebt, kunt u maken en verwijderen van nieuwe services met behulp van de Open modus zonder implementatiefouten. Hetzelfde docker-compose.yml-bestand kan ook worden gebruikt met toewijzingen van statische poort voor het maken van meerdere services.

Als een containerservice opnieuw wordt opgestart of naar een ander knooppunt in het cluster, wordt het IP-adres verandert. Om deze reden wordt niet aanbevolen voor het detecteren van containerservices met behulp van het dynamisch toegewezen IP-adres. Alleen de Service Fabric Naming Service of de DNS-Service moet worden gebruikt voor servicedetectie. 

>[!WARNING]
>Azure kunt een totaal van 4096 IP-adressen per virtueel netwerk. De som van het aantal knooppunten en het aantal container service-exemplaren (die gebruikmaken van Open modus) kan niet groter zijn dan 4096 IP-adressen binnen een virtueel netwerk. Voor scenario's met high-density aangeraden netwerken nat-modus.
>

## <a name="set-up-open-networking-mode"></a>Open netwerken modus instellen

1. De Azure Resource Manager-sjabloon instellen. In de **fabricSettings** sectie, schakelt u de DNS-Service en de IP-Provider: 

    ```json
    "fabricSettings": [
                {
                    "name": "DnsService",
                    "parameters": [
                       {
                            "name": "IsEnabled",
                            "value": "true"
                      }
                    ]
                },
                {
                    "name": "Hosting",
                    "parameters": [
                      { 
                            "name": "IPProviderEnabled",
                            "value": "true"
                      }
                    ]
                },
                {
                    "name":  "Trace/Etw", 
                    "parameters": [
                    {
                            "name": "Level",
                            "value": "5"
                    }
                    ]
                },
                {
                    "name": "Setup",
                    "parameters": [
                    {
                            "name": "ContainerNetworkSetup",
                            "value": "true"
                    }
                    ]
                }
            ],
    ```

2. De sectie network profiel instellen voor het toestaan van meerdere IP-adressen worden geconfigureerd op elk knooppunt van het cluster. Het volgende voorbeeld wordt een vijf IP-adressen per knooppunt voor een Windows-/ Linux-Service Fabric-cluster. U kunt vijf exemplaren van de service luistert op de poort op elk knooppunt hebben.

    ```json
    "variables": {
        "nicName": "NIC",
        "vmName": "vm",
        "virtualNetworkName": "VNet",
        "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',variables('virtualNetworkName'))]",
        "vmNodeType0Name": "[toLower(concat('NT1', variables('vmName')))]",
        "subnet0Name": "Subnet-0",
        "subnet0Prefix": "10.0.0.0/24",
        "subnet0Ref": "[concat(variables('vnetID'),'/subnets/',variables('subnet0Name'))]",
        "lbID0": "[resourceId('Microsoft.Network/loadBalancers',concat('LB','-', parameters('clusterName'),'-',variables('vmNodeType0Name')))]",
        "lbIPConfig0": "[concat(variables('lbID0'),'/frontendIPConfigurations/LoadBalancerIPConfig')]",
        "lbPoolID0": "[concat(variables('lbID0'),'/backendAddressPools/LoadBalancerBEAddressPool')]",
        "lbProbeID0": "[concat(variables('lbID0'),'/probes/FabricGatewayProbe')]",
        "lbHttpProbeID0": "[concat(variables('lbID0'),'/probes/FabricHttpGatewayProbe')]",
        "lbNatPoolID0": "[concat(variables('lbID0'),'/inboundNatPools/LoadBalancerBEAddressNatPool')]"
    }
    "networkProfile": {
                "networkInterfaceConfigurations": [
                  {
                    "name": "[concat(parameters('nicName'), '-0')]",
                    "properties": {
                      "ipConfigurations": [
                        {
                          "name": "[concat(parameters('nicName'),'-',0)]",
                          "properties": {
                            "primary": "true",
                            "loadBalancerBackendAddressPools": [
                              {
                                "id": "[variables('lbPoolID0')]"
                              }
                            ],
                            "loadBalancerInboundNatPools": [
                              {
                                "id": "[variables('lbNatPoolID0')]"
                              }
                            ],
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 1)]",
                          "properties": {
                            "primary": "false",
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 2)]",
                          "properties": {
                            "primary": "false",
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 3)]",
                          "properties": {
                            "primary": "false",
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 4)]",
                          "properties": {
                            "primary": "false",
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 5)]",
                          "properties": {
                            "primary": "false",
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
              }
   ```
 
3. Voor Windows clusters, stelt u een regel voor Azure Netwerkbeveiligingsgroep (NSG) die wordt geopend poort UDP/53 voor het virtuele netwerk met de volgende waarden:

   |Instelling |Waarde | |
   | --- | --- | --- |
   |Prioriteit |2000 | |
   |Naam |Custom_Dns  | |
   |Bron |VirtualNetwork | |
   |Doel | VirtualNetwork | |
   |Service | DNS (UDP/53) | |
   |Actie | Toestaan  | |
   | | |

4. Geef de modus voor netwerken in het toepassingsmanifest voor elke service: `<NetworkConfig NetworkType="Open">`. **Open** networking modus resultaten in de service een toegewijde IP-adres ophalen. Als een modus niet is opgegeven, de service wordt standaard ingesteld op **nat** modus. In het volgende voorbeeld van de manifest de `NodeContainerServicePackage1` en `NodeContainerServicePackage2` services kunnen elke luisteren op dezelfde poort (beide services luisteren `Endpoint1`). Wanneer Open netwerken modus wordt opgegeven, `PortBinding` configuraties kunnen niet worden opgegeven.

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <ApplicationManifest ApplicationTypeName="NodeJsApp" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
      <Description>Calculator Application</Description>
      <Parameters>
        <Parameter Name="ServiceInstanceCount" DefaultValue="3"></Parameter>
        <Parameter Name="MyCpuShares" DefaultValue="3"></Parameter>
      </Parameters>
      <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="NodeContainerServicePackage1" ServiceManifestVersion="1.0"/>
        <Policies>
          <ContainerHostPolicies CodePackageRef="NodeContainerService1.Code" Isolation="hyperv">
           <NetworkConfig NetworkType="Open"/>
          </ContainerHostPolicies>
        </Policies>
      </ServiceManifestImport>
      <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="NodeContainerServicePackage2" ServiceManifestVersion="1.0"/>
        <Policies>
          <ContainerHostPolicies CodePackageRef="NodeContainerService2.Code" Isolation="default">
            <NetworkConfig NetworkType="Open"/>
          </ContainerHostPolicies>
        </Policies>
      </ServiceManifestImport>
    </ApplicationManifest>
    ```

    U kunt combineren en overeenkomen met verschillende netwerken modi alle services in een toepassing voor een Windows-cluster. Sommige services kunnen Open modus gebruiken, terwijl andere nat-modus. Wanneer een service is geconfigureerd voor gebruik van nat-modus, moet de poort die de service luistert op uniek zijn.

    >[!NOTE]
    >Op Linux-clusters wordt combineren netwerken modi voor verschillende services niet ondersteund. 
    >

## <a name="next-steps"></a>Volgende stappen
* [Inzicht krijgen in het Service Fabric-toepassingsmodel](service-fabric-application-model.md)
* [Meer informatie over de Service Fabric-service manifest resources](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-service-manifest-resources)
* [Een Windows-container implementeren op Service Fabric op Windows Server 2016](service-fabric-get-started-containers.md)
* [Implementeert een Docker-container op Service Fabric op Linux](service-fabric-get-started-containers-linux.md)
