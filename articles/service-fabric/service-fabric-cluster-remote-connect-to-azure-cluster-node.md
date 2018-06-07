---
title: Externe verbinding in het knooppunt van een Azure Service Fabric-cluster | Microsoft Docs
description: Informatie over het op afstand verbinding maken met een scale set-exemplaar (een Service Fabric-clusterknooppunt).
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: timlt
editor: ''
ms.assetid: 5441e7e0-d842-4398-b060-8c9d34b07c48
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/23/2018
ms.author: aljo
ms.openlocfilehash: 28424f9a7a0f77882ee3360c5599549303075c18
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34642570"
---
# <a name="remote-connect-to-a-virtual-machine-scale-set-instance-or-a-cluster-node"></a>Extern verbinding maken met een virtuele machine scale set-exemplaar of een clusterknooppunt
In een Service Fabric-cluster worden uitgevoerd in Azure, elk knooppunttype cluster die u definieert [stelt u een afzonderlijke virtuele-machineschaalset](service-fabric-cluster-nodetypes.md).  U kunt extern verbinding maken met specifieke scale set exemplaren (knooppunten).  In tegenstelling tot de single instance virtuele machines hebben niet scale set exemplaren hun eigen virtuele IP-adressen. Dit kan lastig zijn wanneer u zoekt een IP-adres en de poort die u gebruiken kunt voor externe verbinding met een specifiek exemplaar.

Ga voor een IP-adres en de poort die u gebruiken kunt voor externe verbinding met een specifiek exemplaar, moet u de volgende stappen uitvoeren.

1. De binnenkomende NAT-regels voor Remote Desktop Protocol (RDP) ophalen.

    Normaal gesproken heeft elk knooppunttype gedefinieerd in het cluster een eigen virtuele IP-adres en een specifieke load balancer. De load balancer voor een knooppunttype heet standaard met de volgende indeling: *LB-{clusternaam}-{knooppunttype}*, bijvoorbeeld *LB-mijncluster-FrontEnd*. 
    
    Selecteer op de pagina voor de load balancer in Azure-portal **instellingen** > **binnenkomende NAT-regels**: 

    ![Load balancer inkomende NAT-regels](./media/service-fabric-cluster-remote-connect-to-azure-cluster-node/lb-window.png)

    De volgende schermafbeelding ziet de binnenkomende NAT-regels voor een knooppunttype met de naam FrontEnd: 

    ![Load balancer inkomende NAT-regels](./media/service-fabric-cluster-remote-connect-to-azure-cluster-node/nat-rules.png)

    Voor elk knooppunt het IP-adres wordt weergegeven in de **bestemming** kolom de **doel** kolom geeft het exemplaar van de set schaal en de **SERVICE** kolom biedt het poortnummer. Voor externe verbinding zijn aan elk knooppunt in oplopende volgorde die begint met poort 3389 poorten toegewezen.

    U vindt ook de binnenkomende NAT-regels in de `Microsoft.Network/loadBalancers` gedeelte van de Resource Manager-sjabloon voor uw cluster.
    
2. Om te bevestigen de binnenkomende poort op de doel-poorttoewijzing voor een knooppunt, kunt u klikken op de regel en bekijk de **poort doel** waarde. De volgende schermafbeelding ziet u de binnenkomende NAT-regel voor de **FrontEnd (1 exemplaar)** knooppunt in de vorige stap. Merk op dat, hoewel het poortnummer (inkomend) 3390, de doelpoort is toegewezen aan poort 3389, de poort voor de RDP-service op het doel.  

    ![Toewijzing van het doel-poort](./media/service-fabric-cluster-remote-connect-to-azure-cluster-node/port-mapping.png)

    Standaard voor Windows-clusters, is de doelpoort poort 3389, dat wordt toegewezen aan de RDP-service op het doelknooppunt. Voor Linux-clusters is de doelpoort poort 22, waarmee wordt toegewezen aan de service Secure Shell (SSH).

3. Extern verbinding maken met het specifieke knooppunt (schaalset exemplaar). U kunt de gebruikersnaam en wachtwoord die u hebt ingesteld tijdens het maken van het cluster of andere referenties die u hebt geconfigureerd. 

    De volgende schermafbeelding ziet u verbinding met extern bureaublad verbinding maken met de **FrontEnd (1 exemplaar)** knooppunt in een Windows-cluster:
    
    ![Verbinding met extern bureaublad](./media/service-fabric-cluster-remote-connect-to-azure-cluster-node/rdp-connect.png)

    Op Linux-knooppunten, kunt u verbinding maken met SSH (in het volgende voorbeeld wordt gebruikgemaakt van de hetzelfde IP-adres en poort als beknopt alternatief bevat):

    ``` bash
    ssh SomeUser@40.117.156.199 -p 3390
    ```


Lees de volgende artikelen voor de volgende stappen:
* Zie de [overzicht van de functie 'Overal implementeren' en een vergelijking met Azure beheerde clusters](service-fabric-deploy-anywhere.md).
* Meer informatie over [beveiliging cluster](service-fabric-cluster-security.md).
* [Bijwerken van de waarden voor het RDP-poort](./scripts/service-fabric-powershell-change-rdp-port-range.md) op cluster virtuele machines na implementatie
* [De beheerdersgebruikersnaam en wachtwoord wijzigen](./scripts/service-fabric-powershell-change-rdp-user-and-pw.md) voor cluster virtuele machines

