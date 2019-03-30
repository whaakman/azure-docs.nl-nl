---
title: Extern verbinding maken met een Azure Service Fabric-clusterknooppunt | Microsoft Docs
description: Leer hoe u extern verbinding maken met een exemplaar schaalset (een Service Fabric-cluster-knooppunt).
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: chackdan
editor: ''
ms.assetid: 5441e7e0-d842-4398-b060-8c9d34b07c48
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/23/2018
ms.author: aljo
ms.openlocfilehash: 4cc2d6355a0147c33048f1c2c27a3648b9223db4
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58663771"
---
# <a name="remote-connect-to-a-virtual-machine-scale-set-instance-or-a-cluster-node"></a>Extern verbinding maken met een instantie van virtuele-machineschaalset of een clusterknooppunt
In een Service Fabric-cluster worden uitgevoerd in Azure, elk cluster-knooppunttype die u definieert [stelt u een virtuele-machineschaalset in afzonderlijke](service-fabric-cluster-nodetypes.md).  U kunt extern verbinding maken met specifieke schaalsetinstanties (knooppunten).  In tegenstelling tot single instance virtuele machines hebben niet schaalsetinstanties hun eigen virtuele IP-adressen. Dit kan lastig zijn wanneer u zoekt een IP-adres en poort die u gebruiken kunt voor externe verbinding met een specifiek exemplaar.

Als u wilt zoeken in een IP-adres en poort die u gebruiken kunt voor externe verbinding met een specifiek exemplaar, kunt u de volgende stappen uitvoeren.

1. De binnenkomende NAT-regels voor Remote Desktop Protocol (RDP) ophalen.

    Elk knooppunttype gedefinieerd in het cluster is meestal een eigen virtuele IP-adres en een specifieke load balancer. De load balancer voor een knooppunttype heet standaard, met de volgende indeling: *LB-{clusternaam}-{knooppunttype}*, bijvoorbeeld *LB-mijncluster-FrontEnd*. 
    
    Selecteer op de pagina voor de load balancer in Azure portal, **instellingen** > **binnenkomende NAT-regels**: 

    ![Load balancer inkomende NAT-regels](./media/service-fabric-cluster-remote-connect-to-azure-cluster-node/lb-window.png)

    De volgende schermafbeelding ziet u de binnenkomende NAT-regels voor een knooppunttype met de naam FrontEnd: 

    ![Load balancer inkomende NAT-regels](./media/service-fabric-cluster-remote-connect-to-azure-cluster-node/nat-rules.png)

    Voor elk knooppunt, het IP-adres wordt weergegeven in de **bestemming** kolom de **doel** kolom geeft exemplaar in de schaalset, en de **SERVICE** kolom geeft het poortnummer. Voor externe verbindingen, worden poorten toegewezen aan elk knooppunt in oplopende volgorde die begint met poort 3389.

    U vindt hier ook de binnenkomende NAT-regels in de `Microsoft.Network/loadBalancers` sectie van de Resource Manager-sjabloon voor uw cluster.
    
2. Om te bevestigen dat de binnenkomende poort op de doel-toewijzing voor een knooppunt, kunt u klikken op de regel en bekijk de **poort doel** waarde. De volgende schermafbeelding ziet u de binnenkomende NAT-regel voor de **front-end (1 exemplaar)** knooppunt in de vorige stap. U ziet dat, hoewel het (inkomend) poortnummer 3390, de doelpoort is toegewezen aan poort 3389, de poort voor de RDP-service op het doel.  

    ![Toewijzing van het doel-poort](./media/service-fabric-cluster-remote-connect-to-azure-cluster-node/port-mapping.png)

    De doelpoort is standaard voor Windows-clusters, poort 3389, waarmee aan de RDP-service op het doelknooppunt wordt toegewezen. Voor Linux-clusters is de doelpoort poort 22, waarmee wordt toegewezen aan de Secure Shell (SSH)-service.

3. Extern verbinding maken met het specifieke knooppunt (schaalsetinstantie). U kunt de gebruikersnaam en wachtwoord die u hebt ingesteld tijdens het maken van het cluster of andere referenties die u hebt geconfigureerd. 

    De volgende schermafbeelding ziet u met behulp van de verbinding met extern bureaublad verbinding maken met de **front-end (1 exemplaar)** knooppunt in een Windows-cluster:
    
    ![Verbinding met extern bureaublad](./media/service-fabric-cluster-remote-connect-to-azure-cluster-node/rdp-connect.png)

    Op Linux-knooppunten, kunt u verbinding maken met SSH (in het volgende voorbeeld wordt gebruikgemaakt van het hetzelfde IP-adres en de poort voor beknoptheid):

    ``` bash
    ssh SomeUser@40.117.156.199 -p 3390
    ```


Lees de volgende artikelen voor de volgende stappen:
* Zie de [overzicht van de functie 'Overal implementeren' en een vergelijking met Azure beheerde clusters](service-fabric-deploy-anywhere.md).
* Meer informatie over [cluster security](service-fabric-cluster-security.md).
* [De waarden RDP-poortbereik bijwerken](./scripts/service-fabric-powershell-change-rdp-port-range.md) op cluster-VM's na de implementatie
* [De gebruikersnaam van beheerder en het wachtwoord wijzigen](./scripts/service-fabric-powershell-change-rdp-user-and-pw.md) voor cluster-VM's

