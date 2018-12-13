---
title: Netwerkmodi voor Azure Service Fabric containerservices configureren | Microsoft Docs
description: Meer informatie over het instellen van de verschillende netwerken modi die worden ondersteund door Azure Service Fabric.
services: service-fabric
documentationcenter: .net
author: TylerMSFT
manager: timlt
editor: ''
ms.assetid: d552c8cd-67d1-45e8-91dc-871853f44fc6
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 2/23/2018
ms.author: twhitney, subramar
ms.openlocfilehash: 55f388ed15167c5bc7262e194e09e4a92ba50af4
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52866063"
---
# <a name="service-fabric-container-networking-modes"></a>Netwerkmodi voor service Fabric-containers

Een Azure Service Fabric-cluster voor de container services maakt gebruik van **nat** netwerken modus standaard. Als meer dan één containerservice op dezelfde poort luistert en nat-modus wordt gebruikt, worden fouten bij de implementatie kunnen optreden. Ter ondersteuning van meerdere containerservices op dezelfde poort luistert, Service Fabric biedt **Open** netwerken-modus (versie 5.7 en hoger). In de Open-modus, elke containerservice heeft een interne dynamisch toegewezen IP-adres dat ondersteuning biedt voor meerdere services op dezelfde poort luistert.  

Als u een containerservice met een statisch eindpunt in het servicemanifest hebt, kunt u maken en verwijderen van nieuwe services met behulp van Open-modus zonder fouten bij de implementatie. De dezelfde docker-compose.yml-bestand kan ook worden gebruikt met statische poorttoewijzingen te maken van meerdere services.

Wanneer een containerservice opnieuw wordt opgestart of naar een ander knooppunt in het cluster verplaatst, wordt het IP-adres verandert. Om deze reden wordt niet aanbevolen met dynamisch toegewezen IP-adres voor het detecteren van containerservices. Alleen de Service Fabric Naming-Service of de DNS-Service moet worden gebruikt voor de servicedetectie. 

>[!WARNING]
>Met Azure kunt een totaal van 4096 IP-adressen per virtueel netwerk. De som van het aantal knooppunten en het aantal exemplaren van de container service (die gebruikmaakt van Open-modus) kan niet groter zijn dan 4096 IP-adressen binnen een virtueel netwerk. Voor scenario's met hoge dichtheid raden wij netwerken nat-modus.
>

## <a name="set-up-open-networking-mode"></a>Open netwerken modus instellen

1. Instellen van de Azure Resource Manager-sjabloon. In de **instelling fabricSettings** sectie, schakelt u de DNS-Service en de IP-Provider: 

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

2. De sectie netwerk profiel instellen om toe te staan van meerdere IP-adressen worden geconfigureerd op elk knooppunt van het cluster. Het volgende voorbeeld stelt u vijf IP-adressen per knooppunt voor een Windows/Linux Service Fabric-cluster. U kunt vijf exemplaren van de service luistert op de poort op elk knooppunt hebben. Als u wilt de vijf IP-adressen worden toegankelijk is vanaf de Azure Load Balancer, inschrijven voor de vijf IP-adressen in de Azure Load Balancer-back-end-adresgroep zoals hieronder wordt weergegeven.

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
                            "loadBalancerBackendAddressPools": [
                              {
                                "id": "[variables('lbPoolID0')]"
                              }
                            ],
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 2)]",
                          "properties": {
                            "primary": "false",
                            "loadBalancerBackendAddressPools": [
                              {
                                "id": "[variables('lbPoolID0')]"
                              }
                            ],
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 3)]",
                          "properties": {
                            "primary": "false",
                            "loadBalancerBackendAddressPools": [
                              {
                                "id": "[variables('lbPoolID0')]"
                              }
                            ],
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 4)]",
                          "properties": {
                            "primary": "false",
                            "loadBalancerBackendAddressPools": [
                              {
                                "id": "[variables('lbPoolID0')]"
                              }
                            ],
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 5)]",
                          "properties": {
                            "primary": "false",
                            "loadBalancerBackendAddressPools": [
                              {
                                "id": "[variables('lbPoolID0')]"
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
              }
   ```
 
3. Voor alleen voor Windows-clusters, stelt u een regel voor Azure Netwerkbeveiligingsgroep (NSG) die wordt geopend poort UDP/53 voor het virtuele netwerk met de volgende waarden:

   |Instelling |Waarde | |
   | --- | --- | --- |
   |Prioriteit |2000 | |
   |Naam |Custom_Dns  | |
   |Bron |VirtualNetwork | |
   |Doel | VirtualNetwork | |
   |Service | DNS (UDP/53) | |
   |Bewerking | Toestaan  | |
   | | |

4. Geef de modus voor netwerken in het toepassingsmanifest voor elke service: `<NetworkConfig NetworkType="Open">`. **Open** netwerken modus resultaten in de service aan een toegewezen IP-adres. Als een modus niet is opgegeven, de service standaard **nat** modus. In het volgende voorbeeld van de manifest de `NodeContainerServicePackage1` en `NodeContainerServicePackage2` services kunnen elk luisteren op dezelfde poort (beide services luisteren op `Endpoint1`). Als de Open VPN-modus is opgegeven, `PortBinding` configuraties kunnen niet worden opgegeven.

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

    U kunt combineren en deze verschillende netwerken modi in services binnen een toepassing voor een Windows-cluster. Sommige services kunnen Open modus gebruiken terwijl anderen nat-modus gebruiken. Wanneer een service is geconfigureerd voor gebruik van nat-modus, moet de poort die de service luistert uniek zijn.

    >[!NOTE]
    >Op Linux-clusters wordt met een combinatie van netwerkmodi voor andere services niet ondersteund. 
    >

5. Wanneer de **Open** modus is ingeschakeld, de **eindpunt** definitie in het servicemanifest moet expliciet verwijzen naar de codepakket dat overeenkomt met het eindpunt, zelfs als de service-pakket slechts één code heeft pakket erin. 
   
   ```xml
   <Resources>
     <Endpoints>
       <Endpoint Name="ServiceEndpoint" Protocol="http" Port="80" CodePackageRef="Code"/>
     </Endpoints>
   </Resources>
   ```
   
6. Voor Windows, een virtuele machine opnieuw opstarten zorgt ervoor dat het open netwerk opnieuw worden gemaakt. Dit is om een onderliggende probleem in de netwerkstack te verhelpen. Standaard is het netwerk opnieuw te maken. Als dit gedrag worden uitgeschakeld moet, kan de volgende configuratie worden gebruikt gevolgd door een upgrade van een configuratie.

```json
"fabricSettings": [
                {
                    "name": "Setup",
                    "parameters": [
                    {
                            "name": "SkipContainerNetworkResetOnReboot",
                            "value": "true"
                    }
                    ]
                }
            ],          
 ``` 
 
## <a name="next-steps"></a>Volgende stappen
* [Inzicht krijgen in het Service Fabric-toepassingsmodel](service-fabric-application-model.md)
* [Meer informatie over de Service Fabric service manifest-resources](https://docs.microsoft.com/azure/service-fabric/service-fabric-service-manifest-resources)
* [Een Windows-container implementeren in Service Fabric in Windows Server 2016](service-fabric-get-started-containers.md)
* [Een Docker-container implementeren in Service Fabric in Linux](service-fabric-get-started-containers-linux.md)
