---
title: Knooppunt-eindpunten configureren in Azure Batch-pool | Microsoft Docs
description: Informatie over het configureren of uitschakelen van toegang tot SSH of RDP-poorten op rekenknooppunten in een Azure Batch-pool.
services: batch
author: laurenhughes
manager: jeconnoc
ms.service: batch
ms.topic: article
ms.date: 02/13/2018
ms.author: lahugh
ms.openlocfilehash: a6c2c343b13b77048c772cb1e5c2ba06cf8add50
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55457612"
---
# <a name="configure-or-disable-remote-access-to-compute-nodes-in-an-azure-batch-pool"></a>Configureren of uitschakelen van externe toegang tot rekenknooppunten in een Azure Batch-pool

Standaard kan Batch gebruikmaken van een [knooppunt gebruiker](/rest/api/batchservice/computenode/adduser) met de netwerkverbinding extern verbinden met een knooppunt in een Batch-pool. Bijvoorbeeld, een gebruiker verbinding maken met Remote Desktop (RDP) op poort 3389 voor een rekenknooppunt in een Windows-groep. Op deze manier standaard een gebruiker verbinding kan maken met Secure Shell (SSH) op poort 22 naar een rekenknooppunt in een Linux-groep. 

In uw omgeving moet u mogelijk beperken of uitschakelen van deze standaardinstellingen voor externe toegang. U kunt deze instellingen wijzigen met behulp van de Batch-API's om in te stellen de [PoolEndpointConfiguration](/rest/api/batchservice/pool/add#poolendpointconfiguration) eigenschap. 

## <a name="about-the-pool-endpoint-configuration"></a>Over de configuratie van de pool-eindpunt
De endpoint-configuratie bestaat uit een of meer [network address translation (NAT) pools](/rest/api/batchservice/pool/add#inboundnatpool) van front-endpoorten. (Niet hetzelfde als een NAT-pool met de Batch-pool van rekenknooppunten). U instellen elke NAT-pool voor de onderdrukking van de standaardinstellingen op de rekenknooppunten van de pool. 

De configuratie van elke NAT-pool bevat één of meer [regels voor de netwerkbeveiligingsgroep (NSG) netwerk](/rest/api/batchservice/pool/add#networksecuritygrouprule). Elke NSG-regel toestaat of weigert bepaald netwerkverkeer naar het eindpunt. U kunt toestaan of weigeren van al het verkeer, verkeer geïdentificeerd door een [servicetag](../virtual-network/security-overview.md#service-tags) (zoals 'Internet'), of verkeer van specifieke IP-adressen of subnetten.

### <a name="considerations"></a>Overwegingen
* De configuratie van de pool-eindpunt maakt deel uit van een van de pool [netwerkconfiguratie](/rest/api/batchservice/pool/add#NetworkConfiguration). De netwerkconfiguratie kan desgewenst instellingen voor deelname aan de groep van een [virtueel Azure-netwerk](batch-virtual-network.md). Als u de pool in een virtueel netwerk hebt ingesteld, kunt u NSG-regels die gebruikmaken van adresinstellingen in het virtuele netwerk maken.
* U kunt meerdere NSG-regels configureren wanneer u een NAT-pool configureren. De regels worden in volgorde van prioriteit gecontroleerd. Zodra een regel van toepassing is, worden geen andere regels meer getest.


## <a name="example-deny-all-rdp-traffic"></a>Voorbeeld: Alle RDP-verkeer weigeren

De volgende C# fragment toont hoe u het RDP-eindpunt configureren op rekenknooppunten in een Windows-groep om te weigeren alle netwerkverkeer. Het eindpunt maakt gebruik van een front-endgroep van de poorten in het bereik *60000 60099*. 

```csharp
pool.NetworkConfiguration = new NetworkConfiguration
{
    EndpointConfiguration = new PoolEndpointConfiguration(new InboundNatPool[]
    {
      new InboundNatPool("RDP", InboundEndpointProtocol.Tcp, 3389, 60000, 60099, new NetworkSecurityGroupRule[]
        {
            new NetworkSecurityGroupRule(162, NetworkSecurityGroupRuleAccess.Deny, "*"),
        })
    })    
};
```

## <a name="example-deny-all-ssh-traffic-from-the-internet"></a>Voorbeeld: Alle SSH-verkeer vanaf internet weigeren

In het volgende Python-codefragment laat zien hoe het configureren van het SSH-eindpunt op rekenknooppunten in een Linux-groep om alle internetverkeer te weigeren. Het eindpunt maakt gebruik van een front-endgroep van de poorten in het bereik *4000 4100*. 

```python
pool.network_configuration=batchmodels.NetworkConfiguration(
    endpoint_configuration=batchmodels.PoolEndpointConfiguration(
        inbound_nat_pools=[batchmodels.InboundNATPool(
            name='SSH',
            protocol='tcp',
            backend_port=22,
            frontend_port_range_start=4000,
            frontend_port_range_end=4100,
            network_security_group_rules=[
                batchmodels.NetworkSecurityGroupRule(
                priority=170,
                access=batchmodels.NetworkSecurityGroupRuleAccess.deny,
                source_address_prefix='Internet'
                )
            ]
        )
        ]
    ) 
)
```

## <a name="example-allow-rdp-traffic-from-a-specific-ip-address"></a>Voorbeeld: RDP-verkeer van een specifiek IP-adres

De volgende C# fragment toont hoe u het RDP-eindpunt configureren op rekenknooppunten in een Windows-groep, zodat RDP-toegang alleen vanaf IP-adres *198.51.100.7*. De tweede NSG-regel weigert verkeer dat niet overeenkomt met het IP-adres.

```csharp
pool.NetworkConfiguration = new NetworkConfiguration
{
    EndpointConfiguration = new PoolEndpointConfiguration(new InboundNatPool[]
    {
        new InboundNatPool("RDP", InboundEndpointProtocol.Tcp, 3389, 7500, 8000, new NetworkSecurityGroupRule[]
        {   
            new NetworkSecurityGroupRule(179,NetworkSecurityGroupRuleAccess.Allow, "198.51.100.7"),
            new NetworkSecurityGroupRule(180,NetworkSecurityGroupRuleAccess.Deny, "*")
        })
    })    
};
```

## <a name="example-allow-ssh-traffic-from-a-specific-subnet"></a>Voorbeeld: SSH-verkeer van een specifiek subnet toestaan

Het volgende fragment van Python ziet u hoe het configureren van het SSH-eindpunt op rekenknooppunten in een Linux-groep zodat toegang alleen vanaf het subnet *192.168.1.0/24*. De tweede NSG-regel weigert verkeer dat niet overeenkomt met het subnet.

```python
pool.network_configuration=batchmodels.NetworkConfiguration(
    endpoint_configuration=batchmodels.PoolEndpointConfiguration(
        inbound_nat_pools=[batchmodels.InboundNATPool(
            name='SSH',
            protocol='tcp',
            backend_port=22,
            frontend_port_range_start=4000,
            frontend_port_range_end=4100,
            network_security_group_rules=[
                batchmodels.NetworkSecurityGroupRule(
                priority=170,
                access='allow',
                source_address_prefix='192.168.1.0/24'
                ),
                batchmodels.NetworkSecurityGroupRule(
                priority=175,
                access='deny',
                source_address_prefix='*'
                )
            ]
        )
        ]
    )
)
```

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over NSG-regels in Azure, [netwerkverkeer filteren met netwerkbeveiligingsgroepen](../virtual-network/security-overview.md).

- Zie voor een gedetailleerd overzicht van Batch, [grootschalige parallelle rekenoplossingen ontwikkelen met Batch](batch-api-basics.md).

