## <a name="nic"></a>NIC
Een bron-interface (netwerkinterfacekaart) biedt een netwerkverbinding met een bestaand subnet in een VNet-resource. Hoewel u een NIC als zelfstandige object maken kunt, moet u deze koppelen aan een ander object daadwerkelijk om verbinding te bieden. Een NIC kan worden gebruikt om een virtuele machine met een subnet, een openbare IP-adres of een load balancer.  

| Eigenschap | Beschrijving | Voorbeeldwaarden |
| --- | --- | --- |
| **virtuele machine** |Virtuele machine de NIC is gekoppeld. |/Subscriptions/{GUID}/../Microsoft.COMPUTE/virtualMachines/vm1 |
| **MAC-adres** |MAC-adres voor de NIC |een waarde tussen 4 en 30 in |
| **networkSecurityGroup** |NSG die is gekoppeld naar de NIC. |/Subscriptions/{GUID}/../Microsoft.Network/networkSecurityGroups/myNSG1 |
| **dnsSettings** |DNS-instellingen voor de NIC |Zie [PIP](#Public-IP-address) |

Een Network Interface Card of NIC, vertegenwoordigt een netwerkinterface die gekoppeld aan een virtuele machine (VM worden kan). Een virtuele machine kan een of meer NIC's hebben.

![NIC's op een enkele virtuele machine](./media/resource-groups-networking/Figure3.png)

### <a name="ip-configurations"></a>IP-configuraties
NIC's hebben een onderliggend object met de naam **ipConfigurations** met de volgende eigenschappen:

| Eigenschap | Beschrijving | Voorbeeldwaarden |
| --- | --- | --- |
| **subnet** |Subnet van de NIC is onnected aan. |/Subscriptions/{GUID}/../Microsoft.Network/virtualNetworks/myvnet1/subnets/mysub1 |
| **privateIPAddress** |IP-adres voor de NIC in het subnet |10.0.0.8 |
| **privateIPAllocationMethod** |IP-toewijzingsmethode |Dynamische of statische |
| **enableIPForwarding** |Hiermee wordt aangegeven of de NIC kan worden gebruikt voor routering |waar of ONWAAR |
| **primaire** |Hiermee wordt aangegeven of de NIC is de primaire NIC voor de virtuele machine |waar of ONWAAR |
| **publicIPAddress** |PIP die zijn gekoppeld aan de NIC |Zie [DNS-instellingen](#DNS-settings) |
| **loadbalancerbackendaddresspools gebruikt** |Back-end-adresgroepen die de NIC is gekoppeld | |
| **loadBalancerInboundNatRules** |Binnenkomende NAT-regels voor load balancer is die de NIC is gekoppeld aan | |

Voorbeeld openbare IP-adres in JSON-indeling:

    {
        "name": "lb-nic1-be",
        "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/nrprg/providers/Microsoft.Network/networkInterfaces/lb-nic1-be",
        "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
        "type": "Microsoft.Network/networkInterfaces",
        "location": "eastus",
        "properties": {
            "provisioningState": "Succeeded",
            "resourceGuid": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
            "ipConfigurations": [
                {
                    "name": "NIC-config",
                    "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/nrprg/providers/Microsoft.Network/networkInterfaces/lb-nic1-be/ipConfigurations/NIC-config",
                    "etag": "W/\"0027f1a2-3ac8-49de-b5d5-fd46550500b1\"",
                    "properties": {
                        "provisioningState": "Succeeded",
                        "privateIPAddress": "10.0.0.4",
                        "privateIPAllocationMethod": "Dynamic",
                        "subnet": {
                            "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/NRPRG/providers/Microsoft.Network/virtualNetworks/NRPVnet/subnets/NRPVnetSubnet"
                        },
                        "loadBalancerBackendAddressPools": [
                            {
                                "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool"
                            }
                        ],
                        "loadBalancerInboundNatRules": [
                            {
                                "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1"
                            }
                        ]
                    }
                }
            ],
            "dnsSettings": { ... },
            "macAddress": "00-0D-3A-10-F1-29",
            "enableIPForwarding": false,
            "primary": true,
            "virtualMachine": {
                "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/nrprg/providers/Microsoft.Compute/virtualMachines/web1"
            }
        }
    }

### <a name="additional-resources"></a>Aanvullende bronnen
* Lees de [REST-API-naslagdocumentatie](https://msdn.microsoft.com/library/azure/mt163579.aspx) voor NIC's.

