---
title: Netwerken modi voor services van Azure Service Fabric-container configureren | Microsoft Docs
description: Informatie over het instellen van de verschillende netwerken modi die ondersteuning biedt voor Azure Service Fabric.
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
ms.openlocfilehash: 855e315f66858210875039f91f7f05055ff7d9b9
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/11/2017
---
# <a name="service-fabric-container-networking-modes"></a>Netwerken modi voor service Fabric-container

De standaardwaarde netwerken modus in de Service Fabric-cluster voor containerservices aangeboden is de `nat` netwerken modus. Met de `nat` netwerken met meer dan één containers service luistert naar de dezelfde poort leidt tot implementatiefouten modus. Voor verschillende uitgevoerd services die luisteren op dezelfde poort-, Service Fabric ondersteunt de `Open` netwerken modus (versie 5.7 of hoger). Met de `Open` modus netwerken elke containerservice een dynamisch toegewezen IP-adres intern zodat meerdere services om te luisteren op dezelfde poort opgehaald.   

Dus met een type één service met een statische eindpunt gedefinieerd in het servicemanifest, nieuwe services kunnen worden gemaakt en verwijderd zonder implementatiefouten met behulp van de `Open` netwerken modus. Op deze manier kunnen worden gebruikt dezelfde `docker-compose.yml` bestand met toewijzingen van statische poort voor het maken van meerdere services.

Met behulp van het dynamisch toegewezen IP-adres voor het detecteren van de services wordt niet aangeraden sinds de IP-adreswijzigingen wanneer de service opnieuw wordt gestart of verplaatst naar een ander knooppunt. Gebruik alleen de **Service Fabric Naming Service** of de **DNS-Service** voor servicedetectie. 


> [!WARNING]
> Alleen een totaal van 4096 IP-adressen per vNET in Azure zijn toegestaan. Dus de som van het aantal knooppunten en het aantal container service-exemplaren (met `Open` netwerken) mag niet meer dan 4096 binnen een vNET. Voor dergelijke high-density scenario's, de `nat` netwerken modus wordt aanbevolen.
>

## <a name="setting-up-open-networking-mode"></a>Open netwerken modus instellen

1. Instellen van de Azure Resource Manager-sjabloon te maken met DNS-Service en de IP-Provider onder `fabricSettings`. 

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

2. De sectie network profiel instellen voor het toestaan van meerdere IP-adressen worden geconfigureerd op elk knooppunt van het cluster. Het volgende voorbeeld wordt een vijf IP-adressen per knooppunt (dus kunt u vijf service-exemplaren die luisteren naar de poort op elk knooppunt hebt) voor Windows of Linux Service Fabric-cluster.

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
 

3. Voor Windows clusters, stelt u een NSG regel openen van UDP/53-poort voor het vNET met de volgende waarden:

   | Prioriteit |    Naam    |    Bron      |  Doel   |   Service    | Actie |
   |:--------:|:----------:|:--------------:|:--------------:|:------------:|:------:|
   |     2000 | Custom_Dns | VirtualNetwork | VirtualNetwork | DNS (UDP/53) | Toestaan  |


4. Geef de modus voor netwerken in het app-manifest voor elke service `<NetworkConfig NetworkType="Open">`.  De modus `Open` resulteert in de service een toegewijde IP-adres ophalen. Als een modus niet is opgegeven, wordt het basic standaard `nat` modus. Dus in het volgende voorbeeld manifest `NodeContainerServicePackage1` en `NodeContainerServicePackage2` kan elke luisteren op dezelfde poort (beide services luisteren `Endpoint1`). Wanneer de `Open` netwerken modus serienummergroep opgegeven, is `PortBinding` configs kan niet worden opgegeven.

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
U kunt combineren en overeenkomen met verschillende netwerken modi alle services in een toepassing voor een Windows-cluster. U kunt dus sommige services hebben op `Open` modus en sommige op `nat` netwerken modus. Als een service is geconfigureerd met `nat`, de poort die wordt geluisterd naar moet uniek zijn. De combinatie van netwerken modi voor verschillende services wordt niet ondersteund op Linux-clusters. 


## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd over netwerken modi die worden aangeboden door de Service Fabric.  

* [Service Fabric-toepassingsmodel](service-fabric-application-model.md)
* [Service Fabric-service manifest resources](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-service-manifest-resources)
* [Een Windows-container implementeren op Service Fabric op Windows Server 2016](service-fabric-get-started-containers.md)
* [Implementeert een Docker-container op Service Fabric op Linux](service-fabric-get-started-containers-linux.md)
