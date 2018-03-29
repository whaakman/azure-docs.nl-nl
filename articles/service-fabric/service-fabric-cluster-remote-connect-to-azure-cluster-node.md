---
title: Externe verbinding in het knooppunt van een Azure Service Fabric-cluster | Microsoft Docs
description: Informatie over het op afstand verbinding maken met een scale set-exemplaar (een Service Fabric-clusterknooppunt).
services: service-fabric
documentationcenter: .net
author: ChackDan
manager: timlt
editor: ''
ms.assetid: 5441e7e0-d842-4398-b060-8c9d34b07c48
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/23/2018
ms.author: chackdan
ms.openlocfilehash: 8c7d5446429089a0fc931175b55e81e1ad0c97a0
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/28/2018
---
# <a name="remote-connect-to-a-virtual-machine-scale-set-instance-or-a-cluster-node"></a>Extern verbinding maken met een virtuele machine scale set-exemplaar of een clusterknooppunt
In een Service Fabric-cluster worden uitgevoerd in Azure, elk knooppunttype cluster die u definieert [stelt u een afzonderlijke virtuele-machineschaalset](service-fabric-cluster-nodetypes.md).  U kunt extern verbinding maken met specifieke scale set exemplaren (of clusterknooppunten).  In tegenstelling tot de single instance virtuele machines hebben niet scale set exemplaren hun eigen virtuele IP-adressen. Dit kan lastig zijn wanneer u zoekt een IP-adres en de poort die u gebruiken kunt voor externe verbinding met een specifiek exemplaar.

Ga voor een IP-adres en de poort die u gebruiken kunt voor externe verbinding met een specifiek exemplaar, moet u de volgende stappen uitvoeren.

1. Het virtuele IP-adres vinden voor het knooppunttype met het ophalen van de binnenkomende NAT-regels voor Remote Desktop Protocol (RDP).

    Eerst ophalen van de binnenkomende NAT-regels waarden die zijn gedefinieerd als onderdeel van de resourcedefinitie voor `Microsoft.Network/loadBalancers`.
    
    Selecteer in de Azure-portal op de pagina load balancer **instellingen** > **binnenkomende NAT-regels**. Hiermee krijgt u het IP-adres en poort die u gebruiken kunt voor externe verbinding met de eerste schaal instantie ingesteld. 
    
    ![Load balancer][LBBlade]
    
    In de volgende afbeelding, het IP-adres en poort zijn **104.42.106.156** en **3389**.
    
    ![NAT-regels][NATRules]

2. De poort die u gebruiken kunt voor externe verbinding met de specifieke scale set exemplaar of het knooppunt niet vinden.

    Schaalset exemplaren kaart aan knooppunten. Gebruik de informatie van de set schalen om te bepalen van de exacte poort te gebruiken.
    
    Poorten zijn toegewezen in oplopende volgorde die overeenkomt met het exemplaar van de set schaal. Voor het vorige voorbeeld van het type FrontEnd-knooppunt in de volgende tabel geeft een lijst van de poorten voor elk van de exemplaren van de vijf knooppunten. Dezelfde toewijzing van toepassing op uw scale set-exemplaar.
    
    | **Virtuele-machineschaalset exemplaar** | **Poort** |
    | --- | --- |
    | FrontEnd_0 |3389 |
    | FrontEnd_1 |3390 |
    | FrontEnd_2 |3391 |
    | FrontEnd_3 |3392 |
    | FrontEnd_4 |3393 |
    | FrontEnd_5 |3394 |

3. Extern verbinding maken met het specifieke scale set-exemplaar.

    De volgende afbeelding ziet verbinding met extern bureaublad verbinding maken met de FrontEnd_1 scale set exemplaar:
    
    ![Verbinding met extern bureaublad][RDP]


Lees de volgende artikelen voor de volgende stappen:
* Zie de [overzicht van de functie 'Overal implementeren' en een vergelijking met Azure beheerde clusters](service-fabric-deploy-anywhere.md).
* Meer informatie over [beveiliging cluster](service-fabric-cluster-security.md).
* [Bijwerken van de waarden voor het RDP-poort](./scripts/service-fabric-powershell-change-rdp-port-range.md) op cluster virtuele machines na implementatie
* [De beheerdersgebruikersnaam en wachtwoord wijzigen](./scripts/service-fabric-powershell-change-rdp-user-and-pw.md) voor cluster virtuele machines

<!--Image references-->
[LBBlade]: ./media/service-fabric-cluster-remote-connect-to-azure-cluster-node/LBBlade.png
[NATRules]: ./media/service-fabric-cluster-remote-connect-to-azure-cluster-node/NATRules.png
[RDP]: ./media/service-fabric-cluster-remote-connect-to-azure-cluster-node/RDP.png
