---
title: Netwerken patronen voor Azure Service Fabric | Microsoft Docs
description: Algemene netwerken patronen voor Service Fabric en het maken van een cluster met behulp van Azure-netwerkfuncties, beschrijft.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/19/2018
ms.author: ryanwi
ms.openlocfilehash: 0f655becfac05acfacfeef12edd68b37835420bf
ms.sourcegitcommit: 817c3db817348ad088711494e97fc84c9b32f19d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/20/2018
---
# <a name="service-fabric-networking-patterns"></a>Service Fabric netwerken patronen
U kunt uw Azure Service Fabric-cluster integreren met andere Azure-netwerkfuncties. In dit artikel wordt beschreven hoe u voor het maken van clusters die gebruikmaken van de volgende functies:

- [Bestaand virtueel netwerk of subnet](#existingvnet)
- [Statische openbare IP-adres](#staticpublicip)
- [Alleen voor interne load balancer](#internallb)
- [Interne en externe load balancer](#internalexternallb)

Service Fabric wordt uitgevoerd in een standaard virtuele-machineschaalset. Functionaliteit die u in een virtuele-machineschaalset gebruiken kunt, kunt u met een Service Fabric-cluster. De netwerken secties van de Azure Resource Manager-sjablonen voor virtuele-machineschaalsets en Service Fabric zijn identiek. Nadat u op een bestaand virtueel netwerk implementeert, is het eenvoudig andere netwerkfuncties, zoals Azure ExpressRoute, Azure VPN-Gateway, een netwerkbeveiligingsgroep en virtueel netwerk peering opnemen.

Service Fabric is uniek in vergelijking met andere netwerkfuncties in één aspect. De [Azure-portal](https://portal.azure.com) intern maakt gebruik van de Service Fabric-resourceprovider aanroepen naar een cluster kunt u informatie over de knooppunten en toepassingen. De Service Fabric-resourceprovider vereist openbaar toegankelijk inkomende toegang tot de HTTP-gateway-poort (poort 19080, standaard) op de management-eindpunt. [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) maakt gebruik van het eindpunt voor het beheren van uw cluster. De Service Fabric-resourceprovider gebruikt deze poort om informatie over uw cluster ook moet worden weergegeven in de Azure-portal. 

Als poort 19080 is niet toegankelijk is vanaf de Service Fabric-resourceprovider, een bericht zoals *knooppunten is niet gevonden* wordt weergegeven in de portal en de lijst met knooppunt en de toepassing is leeg. Als u zien van het cluster in de Azure portal wilt, de load balancer moet een openbaar IP-adres beschikbaar en de netwerkbeveiligingsgroep moet de binnenkomende 19080-poortverkeer toestaan. Als uw installatie voldoet niet aan deze vereisten voldoet, wordt in de Azure-portal de status van het cluster niet weergegeven.

## <a name="templates"></a>Sjablonen

Alle Service Fabric-sjablonen zijn in [GitHub](https://github.com/Azure/service-fabric-scripts-and-templates/tree/master/templates/networking). U moet voor het implementeren van de sjablonen als kunnen-worden met behulp van de volgende PowerShell-opdrachten. Als u de bestaande Azure Virtual Network-sjabloon of de statische openbare IP-sjabloon implementeert, lees eerst de [initiële setup](#initialsetup) sectie van dit artikel.

<a id="initialsetup"></a>
## <a name="initial-setup"></a>Eerste installatie

### <a name="existing-virtual-network"></a>Bestaand virtueel netwerk

In het volgende voorbeeld we beginnen met een bestaand virtueel netwerk met de naam ExistingRG-vnet, in de **ExistingRG** resourcegroep. De naam van het subnet is standaard. Deze standaardresources worden gemaakt wanneer u de Azure-portal kunt maken van een standaard virtuele machine (VM). U kunt het virtuele netwerk en subnet zonder te maken van de virtuele machine maken, maar het belangrijkste doel van een cluster toe te voegen aan een bestaand virtueel netwerk is voor een netwerkverbinding met andere virtuele machines. Maken van de virtuele machine biedt een goed voorbeeld van hoe een bestaand virtueel netwerk wordt meestal gebruikt. Als uw Service Fabric-cluster maakt gebruik van alleen een interne load balancer, zonder een openbaar IP-adres, kunt u de virtuele machine en de openbare IP-adres gebruiken als een veilige *springen vak*.

### <a name="static-public-ip-address"></a>Statische openbare IP-adres

Een statische openbare IP-adres is doorgaans een speciale resource die wordt afzonderlijk beheerd vanaf de virtuele machine of virtuele machines die aan toegewezen. Het ingericht in een specifieke netwerken resourcegroep (in plaats om te groeperen in de Service Fabric-clusterbron zelf). Maak een statische openbare IP-adres met de naam staticIP1 in dezelfde ExistingRG resourcegroep, in de Azure portal of met behulp van PowerShell:

```powershell
PS C:\Users\user> New-AzureRmPublicIpAddress -Name staticIP1 -ResourceGroupName ExistingRG -Location westus -AllocationMethod Static -DomainNameLabel sfnetworking

Name                     : staticIP1
ResourceGroupName        : ExistingRG
Location                 : westus
Id                       : /subscriptions/1237f4d2-3dce-1236-ad95-123f764e7123/resourceGroups/ExistingRG/providers/Microsoft.Network/publicIPAddresses/staticIP1
Etag                     : W/"fc8b0c77-1f84-455d-9930-0404ebba1b64"
ResourceGuid             : 77c26c06-c0ae-496c-9231-b1a114e08824
ProvisioningState        : Succeeded
Tags                     :
PublicIpAllocationMethod : Static
IpAddress                : 40.83.182.110
PublicIpAddressVersion   : IPv4
IdleTimeoutInMinutes     : 4
IpConfiguration          : null
DnsSettings              : {
                             "DomainNameLabel": "sfnetworking",
                             "Fqdn": "sfnetworking.westus.cloudapp.azure.com"
                           }
```

### <a name="service-fabric-template"></a>Service Fabric-sjabloon

In de voorbeelden in dit artikel gebruiken we de Service Fabric-template.json. De wizard voor standaard portal kunt u de sjabloon downloaden van de portal voordat u een cluster maakt. Ook kunt u een van de [voorbeeldsjablonen](https://github.com/Azure-Samples/service-fabric-cluster-templates), bijvoorbeeld de [beveiligde vijf knooppunten Service Fabric-cluster](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Windows-1-NodeTypes-Secure).

<a id="existingvnet"></a>
## <a name="existing-virtual-network-or-subnet"></a>Bestaand virtueel netwerk of subnet

1. De subnet-parameter wijzigen in de naam van het bestaande subnet en voeg vervolgens twee nieuwe parameters om te verwijzen naar de bestaande virtuele netwerk:

    ```
        "subnet0Name": {
                "type": "string",
                "defaultValue": "default"
            },
            "existingVNetRGName": {
                "type": "string",
                "defaultValue": "ExistingRG"
            },

            "existingVNetName": {
                "type": "string",
                "defaultValue": "ExistingRG-vnet"
            },
            /*
            "subnet0Name": {
                "type": "string",
                "defaultValue": "Subnet-0"
            },
            "subnet0Prefix": {
                "type": "string",
                "defaultValue": "10.0.0.0/24"
            },*/
    ```


2. Wijzig de `vnetID` variabele om te verwijzen naar de bestaande virtuele netwerk:

    ```
            /*old "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',parameters('virtualNetworkName'))]",*/
            "vnetID": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', parameters('existingVNetRGName'), '/providers/Microsoft.Network/virtualNetworks/', parameters('existingVNetName'))]",
    ```

3. Verwijder `Microsoft.Network/virtualNetworks` van uw resources zo Azure geen maakt een nieuw virtueel netwerk:

    ```
    /*{
    "apiVersion": "[variables('vNetApiVersion')]",
    "type": "Microsoft.Network/virtualNetworks",
    "name": "[parameters('virtualNetworkName')]",
    "location": "[parameters('computeLocation')]",
    "properities": {
        "addressSpace": {
            "addressPrefixes": [
                "[parameters('addressPrefix')]"
            ]
        },
        "subnets": [
            {
                "name": "[parameters('subnet0Name')]",
                "properties": {
                    "addressPrefix": "[parameters('subnet0Prefix')]"
                }
            }
        ]
    },
    "tags": {
        "resourceType": "Service Fabric",
        "clusterName": "[parameters('clusterName')]"
    }
    },*/
    ```

4. Commentaar of het virtuele netwerk van de `dependsOn` kenmerk van `Microsoft.Compute/virtualMachineScaleSets`, zodat u niet afhankelijk zijn van een nieuw virtueel netwerk maken:

    ```
    "apiVersion": "[variables('vmssApiVersion')]",
    "type": "Microsoft.Computer/virtualMachineScaleSets",
    "name": "[parameters('vmNodeType0Name')]",
    "location": "[parameters('computeLocation')]",
    "dependsOn": [
        /*"[concat('Microsoft.Network/virtualNetworks/', parameters('virtualNetworkName'))]",
        */
        "[Concat('Microsoft.Storage/storageAccounts/', variables('uniqueStringArray0')[0])]",

    ```

5. De sjabloon implementeren:

    ```powershell
    New-AzureRmResourceGroup -Name sfnetworkingexistingvnet -Location westus
    New-AzureRmResourceGroupDeployment -Name deployment -ResourceGroupName sfnetworkingexistingvnet -TemplateFile C:\SFSamples\Final\template\_existingvnet.json
    ```

    Na de implementatie, het virtuele netwerk bevatten de nieuwe VM-schaalset. De virtuele machine scale set knooppunttype moet het bestaande virtuele netwerk en subnet worden weergegeven. U kunt Remote Desktop Protocol (RDP) ook gebruiken voor toegang tot de virtuele machine die was al niet in het virtuele netwerk en ingesteld om te pingen van de schaal van de nieuwe virtuele machines:

    ```
    C:>\Users\users>ping 10.0.0.5 -n 1
    C:>\Users\users>ping NOde1000000 -n 1
    ```

Zie voor een ander voorbeeld [dat is niet specifiek voor Service Fabric](https://github.com/gbowerman/azure-myriad/tree/master/existing-vnet).


<a id="staticpublicip"></a>
## <a name="static-public-ip-address"></a>Statische openbare IP-adres

1. Voeg parameters voor de naam van de bestaande statische IP-resourcegroep, de naam en de volledig gekwalificeerde domeinnaam (FQDN):

    ```
    "existingStaticIPResourceGroup": {
                "type": "string"
            },
            "existingStaticIPName": {
                "type": "string"
            },
            "existingStaticIPDnsFQDN": {
                "type": "string"
    }
    ```

2. Verwijder de `dnsName` parameter. (Het statische IP-adres heeft al een.)

    ```
    /*
    "dnsName": {
        "type": "string"
    },
    */
    ```

3. Een variabele om te verwijzen naar de bestaande statisch IP-adres toevoegen:

    ```
    "existingStaticIP": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', parameters('existingStaticIPResourceGroup'), '/providers/Microsoft.Network/publicIPAddresses/', parameters('existingStaticIPName'))]",
    ```

4. Verwijder `Microsoft.Network/publicIPAddresses` van uw resources zo Azure geen maakt een nieuw IP-adres:

    ```
    /*
    {
        "apiVersion": "[variables('publicIPApiVersion')]",
        "type": "Microsoft.Network/publicIPAddresses",
        "name": "[concat(parameters('lbIPName'),)'-', '0')]",
        "location": "[parameters('computeLocation')]",
        "properties": {
            "dnsSettings": {
                "domainNameLabel": "[parameters('dnsName')]"
            },
            "publicIPAllocationMethod": "Dynamic"        
        },
        "tags": {
            "resourceType": "Service Fabric",
            "clusterName": "[parameters('clusterName')]"
        }
    }, */
    ```

5. Commentaar of het IP-adres van de `dependsOn` kenmerk van `Microsoft.Network/loadBalancers`, zodat u niet afhankelijk zijn van het maken van een nieuw IP-adres:

    ```
    "apiVersion": "[variables('lbIPApiVersion')]",
    "type": "Microsoft.Network/loadBalancers",
    "name": "[concat('LB', '-', parameters('clusterName'), '-', parameters('vmNodeType0Name'))]",
    "location": "[parameters('computeLocation')]",
    /*
    "dependsOn": [
        "[concat('Microsoft.Network/publicIPAddresses/', concat(parameters('lbIPName'), '-', '0'))]"
    ], */
    "properties": {
    ```

6. In de `Microsoft.Network/loadBalancers` resource, wijzig de `publicIPAddress` element van `frontendIPConfigurations` om te verwijzen naar de bestaande statisch IP-adres in plaats van een nieuwe:

    ```
                "frontendIPConfigurations": [
                        {
                            "name": "LoadBalancerIPConfig",
                            "properties": {
                                "publicIPAddress": {
                                    /*"id": "[resourceId('Microsoft.Network/publicIPAddresses',concat(parameters('lbIPName'),'-','0'))]"*/
                                    "id": "[variables('existingStaticIP')]"
                                }
                            }
                        }
                    ],
    ```

7. In de `Microsoft.ServiceFabric/clusters` resource wijzigen `managementEndpoint` op de DNS-FQDN van het statische IP-adres. Als u een beveiligde cluster gebruikt, controleert u of u kunt wijzigen *http://* naar *https://*. (Houd er rekening mee dat deze stap alleen voor Service Fabric-clusters geldt. Als u van een virtuele-machineschaalset gebruikmaakt, deze stap overslaan.)

    ```
                    "fabricSettings": [],
                    /*"managementEndpoint": "[concat('http://',reference(concat(parameters('lbIPName'),'-','0')).dnsSettings.fqdn,':',parameters('nt0fabricHttpGatewayPort'))]",*/
                    "managementEndpoint": "[concat('http://',parameters('existingStaticIPDnsFQDN'),':',parameters('nt0fabricHttpGatewayPort'))]",
    ```

8. De sjabloon implementeren:

    ```powershell
    New-AzureRmResourceGroup -Name sfnetworkingstaticip -Location westus

    $staticip = Get-AzureRmPublicIpAddress -Name staticIP1 -ResourceGroupName ExistingRG

    $staticip

    New-AzureRmResourceGroupDeployment -Name deployment -ResourceGroupName sfnetworkingstaticip -TemplateFile C:\SFSamples\Final\template\_staticip.json -existingStaticIPResourceGroup $staticip.ResourceGroupName -existingStaticIPName $staticip.Name -existingStaticIPDnsFQDN $staticip.DnsSettings.Fqdn
    ```

Na de implementatie kunt u zien dat de load balancer is gebonden aan de openbare statische IP-adres van de resourcegroep. De Service Fabric-eindpunt voor de verbinding van client en [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) endpoint-punt met de DNS-FQDN van het statische IP-adres.

<a id="internallb"></a>
## <a name="internal-only-load-balancer"></a>Alleen voor interne load balancer

Dit scenario worden de externe load balancer in de Service Fabric standaardsjabloon vervangen door een interne load balancer. Zie de vorige sectie voor gevolgen voor de Azure-portal en voor de Service Fabric-resourceprovider.

1. Verwijder de `dnsName` parameter. (Dit niet nodig.)

    ```
    /*
    "dnsName": {
        "type": "string"
    },
    */
    ```

2. Als u een statische toewijzingsmethode gebruikt, kunt u eventueel een parameter statische IP-adres toevoegen. Als u een dynamische toewijzingsmethode gebruikt, hoeft u geen Voer deze stap.

    ```
            "internalLBAddress": {
                "type": "string",
                "defaultValue": "10.0.0.250"
            }
    ```

3. Verwijder `Microsoft.Network/publicIPAddresses` van uw resources zo Azure geen maakt een nieuw IP-adres:

    ```
    /*
    {
        "apiVersion": "[variables('publicIPApiVersion')]",
        "type": "Microsoft.Network/publicIPAddresses",
        "name": "[concat(parameters('lbIPName'),)'-', '0')]",
        "location": "[parameters('computeLocation')]",
        "properties": {
            "dnsSettings": {
                "domainNameLabel": "[parameters('dnsName')]"
            },
            "publicIPAllocationMethod": "Dynamic"        
        },
        "tags": {
            "resourceType": "Service Fabric",
            "clusterName": "[parameters('clusterName')]"
        }
    }, */
    ```

4. Verwijder de IP-adres `dependsOn` kenmerk van `Microsoft.Network/loadBalancers`, zodat u niet afhankelijk zijn van het maken van een nieuw IP-adres. Toevoegen van het virtuele netwerk `dependsOn` kenmerk omdat de load balancer nu is afhankelijk van het subnet van het virtuele netwerk:

    ```
                "apiVersion": "[variables('lbApiVersion')]",
                "type": "Microsoft.Network/loadBalancers",
                "name": "[concat('LB','-', parameters('clusterName'),'-',parameters('vmNodeType0Name'))]",
                "location": "[parameters('computeLocation')]",
                "dependsOn": [
                    /*"[concat('Microsoft.Network/publicIPAddresses/',concat(parameters('lbIPName'),'-','0'))]"*/
                    "[concat('Microsoft.Network/virtualNetworks/',parameters('virtualNetworkName'))]"
                ],
    ```

5. Wijzigen van de load balancer `frontendIPConfigurations` instellen uit met behulp van een `publicIPAddress`, voor het gebruik van een subnet en `privateIPAddress`. `privateIPAddress`een vooraf gedefinieerde statische interne IP-adres gebruikt. Voor het gebruik van een dynamische IP-adres, verwijder de `privateIPAddress` element en wijzig vervolgens `privateIPAllocationMethod` naar **dynamische**.

    ```
                "frontendIPConfigurations": [
                        {
                            "name": "LoadBalancerIPConfig",
                            "properties": {
                                /*
                                "publicIPAddress": {
                                    "id": "[resourceId('Microsoft.Network/publicIPAddresses',concat(parameters('lbIPName'),'-','0'))]"
                                } */
                                "subnet" :{
                                    "id": "[variables('subnet0Ref')]"
                                },
                                "privateIPAddress": "[parameters('internalLBAddress')]",
                                "privateIPAllocationMethod": "Static"
                            }
                        }
                    ],
    ```

6. In de `Microsoft.ServiceFabric/clusters` resource wijzigen `managementEndpoint` om te verwijzen naar het adres van de interne load balancer. Als u een beveiligde cluster gebruikt, controleert u of u kunt wijzigen *http://* naar *https://*. (Houd er rekening mee dat deze stap alleen voor Service Fabric-clusters geldt. Als u van een virtuele-machineschaalset gebruikmaakt, deze stap overslaan.)

    ```
                    "fabricSettings": [],
                    /*"managementEndpoint": "[concat('http://',reference(concat(parameters('lbIPName'),'-','0')).dnsSettings.fqdn,':',parameters('nt0fabricHttpGatewayPort'))]",*/
                    "managementEndpoint": "[concat('http://',reference(variables('lbID0')).frontEndIPConfigurations[0].properties.privateIPAddress,':',parameters('nt0fabricHttpGatewayPort'))]",
    ```

7. De sjabloon implementeren:

    ```powershell
    New-AzureRmResourceGroup -Name sfnetworkinginternallb -Location westus

    New-AzureRmResourceGroupDeployment -Name deployment -ResourceGroupName sfnetworkinginternallb -TemplateFile C:\SFSamples\Final\template\_internalonlyLB.json
    ```

Na de implementatie maakt gebruik van de load balancer het particuliere 10.0.0.250 statische IP-adres. Als u een andere computer die hetzelfde virtuele netwerk hebt, gaat u naar het interne [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) eindpunt. Houd er rekening mee dat deze verbinding met een van de knooppunten achter de load balancer maakt.

<a id="internalexternallb"></a>
## <a name="internal-and-external-load-balancer"></a>Interne en externe load balancer

In dit scenario kunt u beginnen met de bestaande één knooppunt type externe load balancer en toevoegen van een interne load balancer van hetzelfde knooppunttype. Een back-end-poort gekoppeld aan een back-end-adresgroep kan alleen worden toegewezen aan een enkele load balancer. Kies welke load balancer wordt de poorten van uw toepassing, en welke load balancer heeft uw eindpunten voor beheer (poorten 19000 en 19080). Als u de eindpunten voor beheer op de interne load balancer plaatst, houd rekening met de Service Fabric-resource provider-beperkingen die eerder in dit artikel wordt besproken. In het voorbeeld gebruiken we, het beheer eindpunten blijven op de externe load balancer. U ook een poort 80 toepassing poort toevoegen en plaats het op de interne load balancer.

In een cluster met twee knooppunttype is één knooppunttype op de externe load balancer. Het knooppunttype is op de interne load balancer. Voor het gebruik van een cluster met twee knooppunttype in de portal gemaakt twee knooppunttype sjabloon (die wordt geleverd met twee netwerktaakverdelers), de tweede load balancer overschakelen naar een interne load balancer. Zie voor meer informatie de [alleen interne load balancer](#internallb) sectie.

1. De parameter statische interne load balancer-IP-adres toevoegen. (Zie de vorige secties van dit artikel voor opmerkingen over het gebruik van een dynamische IP-adres,.)

    ```
            "internalLBAddress": {
                "type": "string",
                "defaultValue": "10.0.0.250"
            }
    ```

2. Een parameter van de poort 80 toepassing toevoegen.

3. Toe te voegen interne versies van de bestaande netwerken variabelen, kopiëren en plakken en toevoegen '-Int ' op de naam:

    ```
    /* Add internal load balancer networking variables */
            "lbID0-Int": "[resourceId('Microsoft.Network/loadBalancers', concat('LB','-', parameters('clusterName'),'-',parameters('vmNodeType0Name'), '-Internal'))]",
            "lbIPConfig0-Int": "[concat(variables('lbID0-Int'),'/frontendIPConfigurations/LoadBalancerIPConfig')]",
            "lbPoolID0-Int": "[concat(variables('lbID0-Int'),'/backendAddressPools/LoadBalancerBEAddressPool')]",
            "lbProbeID0-Int": "[concat(variables('lbID0-Int'),'/probes/FabricGatewayProbe')]",
            "lbHttpProbeID0-Int": "[concat(variables('lbID0-Int'),'/probes/FabricHttpGatewayProbe')]",
            "lbNatPoolID0-Int": "[concat(variables('lbID0-Int'),'/inboundNatPools/LoadBalancerBEAddressNatPool')]",
            /* Internal load balancer networking variables end */
    ```

4. Als u met de portal gegenereerde sjabloon die toepassing poort 80 gebruikt begint, de portal standaardsjabloon AppPort1 toegevoegd (poort 80) op de externe load balancer. In dit geval AppPort1 verwijderen uit de externe load balancer `loadBalancingRules` en tests, zodat u deze aan de interne load balancer toevoegen kunt:

    ```
    "loadBalancingRules": [
        {
            "name": "LBHttpRule",
            "properties":{
                "backendAddressPool": {
                    "id": "[variables('lbPoolID0')]"
                },
                "backendPort": "[parameters('nt0fabricHttpGatewayPort')]",
                "enableFloatingIP": "false",
                "frontendIPConfiguration": {
                    "id": "[variables('lbIPConfig0')]"            
                },
                "frontendPort": "[parameters('nt0fabricHttpGatewayPort')]",
                "idleTimeoutInMinutes": "5",
                "probe": {
                    "id": "[variables('lbHttpProbeID0')]"
                },
                "protocol": "tcp"
            }
        } /* Remove AppPort1 from the external load balancer.
        {
            "name": "AppPortLBRule1",
            "properties": {
                "backendAddressPool": {
                    "id": "[variables('lbPoolID0')]"
                },
                "backendPort": "[parameters('loadBalancedAppPort1')]",
                "enableFloatingIP": "false",
                "frontendIPConfiguration": {
                    "id": "[variables('lbIPConfig0')]"            
                },
                "frontendPort": "[parameters('loadBalancedAppPort1')]",
                "idleTimeoutInMinutes": "5",
                "probe": {
                    "id": "[concate(variables('lbID0'), '/probes/AppPortProbe1')]"
                },
                "protocol": "tcp"
            }
        }*/

    ],
    "probes": [
        {
            "name": "FabricGatewayProbe",
            "properties": {
                "intervalInSeconds": 5,
                "numberOfProbes": 2,
                "port": "[parameters('nt0fabricTcpGatewayPort')]",
                "protocol": "tcp"
            }
        },
        {
            "name": "FabricHttpGatewayProbe",
            "properties": {
                "intervalInSeconds": 5,
                "numberOfProbes": 2,
                "port": "[parameters('nt0fabricHttpGatewayPort')]",
                "protocol": "tcp"
            }
        } /* Remove AppPort1 from the external load balancer.
        {
            "name": "AppPortProbe1",
            "properties": {
                "intervalInSeconds": 5,
                "numberOfProbes": 2,
                "port": "[parameters('loadBalancedAppPort1')]",
                "protocol": "tcp"
            }
        } */

    ],
    "inboundNatPools": [
    ```

5. Toevoegen van een tweede `Microsoft.Network/loadBalancers` resource. Het lijkt op de interne load balancer gemaakt in de [alleen interne load balancer](#internallb) sectie, maar gebruikt u de '-Int ' load balancer-variabelen en alleen de toepassing-poort 80 implementeert. Hiermee worden `inboundNatPools`en bewaar deze RDP-eindpunten op de openbare load balancer. Als u wilt dat RDP in de interne load balancer, verplaatsen `inboundNatPools` van de externe de load balancer aan deze interne load balancer:

    ```
            /* Add a second load balancer, configured with a static privateIPAddress and the "-Int" load balancer variables. */
            {
                "apiVersion": "[variables('lbApiVersion')]",
                "type": "Microsoft.Network/loadBalancers",
                /* Add "-Internal" to the name. */
                "name": "[concat('LB','-', parameters('clusterName'),'-',parameters('vmNodeType0Name'), '-Internal')]",
                "location": "[parameters('computeLocation')]",
                "dependsOn": [
                    /* Remove public IP dependsOn, add vnet dependsOn
                    "[concat('Microsoft.Network/publicIPAddresses/',concat(parameters('lbIPName'),'-','0'))]"
                    */
                    "[concat('Microsoft.Network/virtualNetworks/',parameters('virtualNetworkName'))]"
                ],
                "properties": {
                    "frontendIPConfigurations": [
                        {
                            "name": "LoadBalancerIPConfig",
                            "properties": {
                                /* Switch from Public to Private IP address
                                */
                                "publicIPAddress": {
                                    "id": "[resourceId('Microsoft.Network/publicIPAddresses',concat(parameters('lbIPName'),'-','0'))]"
                                }
                                */
                                "subnet" :{
                                    "id": "[variables('subnet0Ref')]"
                                },
                                "privateIPAddress": "[parameters('internalLBAddress')]",
                                "privateIPAllocationMethod": "Static"
                            }
                        }
                    ],
                    "backendAddressPools": [
                        {
                            "name": "LoadBalancerBEAddressPool",
                            "properties": {}
                        }
                    ],
                    "loadBalancingRules": [
                        /* Add the AppPort rule. Be sure to reference the "-Int" versions of backendAddressPool, frontendIPConfiguration, and the probe variables. */
                        {
                            "name": "AppPortLBRule1",
                            "properties": {
                                "backendAddressPool": {
                                    "id": "[variables('lbPoolID0-Int')]"
                                },
                                "backendPort": "[parameters('loadBalancedAppPort1')]",
                                "enableFloatingIP": "false",
                                "frontendIPConfiguration": {
                                    "id": "[variables('lbIPConfig0-Int')]"
                                },
                                "frontendPort": "[parameters('loadBalancedAppPort1')]",
                                "idleTimeoutInMinutes": "5",
                                "probe": {
                                    "id": "[concat(variables('lbID0-Int'),'/probes/AppPortProbe1')]"
                                },
                                "protocol": "tcp"
                            }
                        }
                    ],
                    "probes": [
                    /* Add the probe for the app port. */
                    {
                            "name": "AppPortProbe1",
                            "properties": {
                                "intervalInSeconds": 5,
                                "numberOfProbes": 2,
                                "port": "[parameters('loadBalancedAppPort1')]",
                                "protocol": "tcp"
                            }
                        }
                    ],
                    "inboundNatPools": [
                    ]
                },
                "tags": {
                    "resourceType": "Service Fabric",
                    "clusterName": "[parameters('clusterName')]"
                }
            },
    ```

6. In `networkProfile` voor de `Microsoft.Compute/virtualMachineScaleSets` resource toevoegen van de interne back-end-adresgroep:

    ```
    "loadBalancerBackendAddressPools": [
                                                        {
                                                            "id": "[variables('lbPoolID0')]"
                                                        },
                                                        {
                                                            /* Add internal BE pool */
                                                            "id": "[variables('lbPoolID0-Int')]"
                                                        }
    ],
    ```

7. De sjabloon implementeren:

    ```powershell
    New-AzureRmResourceGroup -Name sfnetworkinginternalexternallb -Location westus

    New-AzureRmResourceGroupDeployment -Name deployment -ResourceGroupName sfnetworkinginternalexternallb -TemplateFile C:\SFSamples\Final\template\_internalexternalLB.json
    ```

Na de implementatie ziet u twee load balancers in de resourcegroep. Als u de load balancers bladert, ziet u de openbare IP-adres en het beheer eindpunten (poorten 19000 en 19080) toegewezen aan het openbare IP-adres. Ook ziet u het statische interne IP-adres en de toepassing eindpunt (poort 80) toegewezen aan de interne load balancer. Beide netwerktaakverdelers gebruiken de dezelfde virtuele machine scale set back-end-pool.

## <a name="next-steps"></a>Volgende stappen
[Een cluster maken](service-fabric-cluster-creation-via-arm.md)
