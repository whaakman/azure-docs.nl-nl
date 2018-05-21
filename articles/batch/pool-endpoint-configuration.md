---
title: Knooppunt-eindpunten configureren in Azure Batch-pool | Microsoft Docs
description: Het configureren of de toegang tot SSH of RDP-poorten op rekenknooppunten in een Azure Batch-pool uitschakelen.
services: batch
author: dlepow
manager: jeconnoc
ms.service: batch
ms.topic: article
ms.date: 02/13/2018
ms.author: danlep
ms.openlocfilehash: 5898206761e5029f94b6d1f1b48223481ae2ca13
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
---
# <a name="configure-or-disable-remote-access-to-compute-nodes-in-an-azure-batch-pool"></a>Configureren of uitschakelen van externe toegang rekenknooppunten in een Azure Batch-toepassingen

Standaard Batch kan een [knooppunt gebruiker](/rest/api/batchservice/computenode/adduser) met de netwerkverbinding extern verbinding met een rekenknooppunt in een Batch-pool. Bijvoorbeeld, een gebruiker verbinding maken met Remote Desktop (RDP) op poort 3389 naar een rekenknooppunt in een Windows-groep. Op deze manier standaard een gebruiker verbinding kan maken door Secure Shell (SSH) op poort 22 naar een rekenknooppunt in een pool van Linux. 

In uw omgeving moet u wellicht te beperken of uitschakelen van deze standaardinstellingen voor externe toegang. U kunt deze instellingen wijzigen met behulp van de Batch-API's om in te stellen de [PoolEndpointConfiguration](/rest/api/batchservice/pool/add#poolendpointconfiguration) eigenschap. 

## <a name="about-the-pool-endpoint-configuration"></a>Over de configuratie van de endpoint van toepassingen
De eindpuntconfiguratie bestaat uit een of meer [network address translation (NAT) pools](/rest/api/batchservice/pool/add#inboundnatpool) frontend-poorten. (Niet hetzelfde als een NAT-pool met de Batch-pool van rekenknooppunten.) U instellen elke NAT-pool om te overschrijven de standaardinstellingen van de verbinding op de rekenknooppunten van de groep. 

De configuratie van elke NAT-pool bevat één of meer [beveiligingsregels voor de groep (NSG) netwerk](/rest/api/batchservice/pool/add#networksecuritygrouprule). Elke regel van het NSG toestaat of weigert bepaald netwerkverkeer naar het eindpunt. U kunt toestaan of weigeren van alle verkeer, verkeer geïdentificeerd door een [service tag](../virtual-network/security-overview.md#service-tags) (zoals 'Internet'), of het verkeer van specifieke IP-adressen of subnetten.

### <a name="considerations"></a>Overwegingen
* De eindpuntconfiguratie groep maakt deel uit van de groep [netwerkconfiguratie](/rest/api/batchservice/pool/add#NetworkConfiguration). De netwerkconfiguratie kunt u eventueel instellingen voor het koppelen van de toepassingen die u wilt opnemen een [virtuele Azure-netwerk](batch-virtual-network.md). Als u de toepassingen in een virtueel netwerk hebt ingesteld, kunt u NSG-regels die gebruikmaken van de adresinstellingen in het virtuele netwerk maken.
* U kunt meerdere NSG-regels configureren wanneer u een NAT-adresgroep configureren. De regels worden gecontroleerd op volgorde van prioriteit. Zodra een regel van toepassing is, worden geen andere regels meer getest.


## <a name="example-deny-all-rdp-traffic"></a>Voorbeeld: Alle RDP-verkeer weigeren

De volgende C#-fragment toont hoe u configureert het RDP-eindpunt op rekenknooppunten in een Windows-groep voor het weigeren van al het netwerkverkeer. Het eindpunt maakt gebruik van een frontend-pool van poorten in het bereik *60000 60099*. 

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

Het volgende fragment van Python laat zien hoe het SSH-eindpunt configureren op rekenknooppunten in een pool van Linux weigeren alle internetverkeer. Het eindpunt maakt gebruik van een frontend-pool van poorten in het bereik *4000 4100*. 

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

## <a name="example-allow-rdp-traffic-from-a-specific-ip-address"></a>Voorbeeld: RDP-verkeer van een specifiek IP-adres toestaan

De volgende C#-fragment toont hoe u configureert het RDP-eindpunt op rekenknooppunten in een Windows-groep zodat RDP alleen toegankelijk is vanaf IP-adres *198.51.100.7*. De tweede regel op NSG weigert verkeer dat niet overeenkomt met het IP-adres.

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

## <a name="example-allow-ssh-traffic-from-a-specific-subnet"></a>Voorbeeld: De SSH-verkeer van een specifiek subnet toestaan

Het volgende Python-fragment toont hoe u configureert het SSH-eindpunt op rekenknooppunten in een pool van Linux toegang toestaan alleen via het subnet *192.168.1.0/24*. De tweede regel op NSG weigert verkeer dat niet overeenkomt met het subnet.

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

- Zie voor meer informatie over het NSG-regels in Azure, [filteren van netwerkverkeer met netwerkbeveiligingsgroepen](../virtual-network/security-overview.md).

- Zie voor een gedetailleerd overzicht van Batch [ontwikkelen grootschalige parallelle compute-oplossingen met Batch](batch-api-basics.md).

