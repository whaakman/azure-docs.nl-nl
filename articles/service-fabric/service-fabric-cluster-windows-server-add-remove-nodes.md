---
title: Toevoegen of verwijderen van knooppunten aan een zelfstandige Service Fabric-cluster | Microsoft Docs
description: Informatie over het toevoegen of verwijderen van knooppunten aan een Azure Service Fabric-cluster op een fysieke of virtuele machine met Windows Server, kan nu on-premises of in een cloud.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: bc6b8fc0-d2af-42f8-a164-58538be38d02
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/02/2017
ms.author: dekapur
ms.openlocfilehash: 252dcdf0ff9e1fecd6665808bfe7978a4417018b
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/06/2017
---
# <a name="add-or-remove-nodes-to-a-standalone-service-fabric-cluster-running-on-windows-server"></a>Toevoegen of verwijderen van knooppunten naar een zelfstandige Service Fabric-cluster met Windows Server
Nadat u hebt [uw zelfstandige Service Fabric-cluster gemaakt op Windows Server-machines](service-fabric-cluster-creation-for-windows-server.md), uw (bedrijfsbehoeften) kunnen worden gewijzigd en moet u toevoegen of verwijderen van knooppunten aan het cluster. In dit artikel biedt gedetailleerde stappen om dit te bereiken. Houd er rekening mee toevoegen of verwijderen knooppunt functionaliteit wordt niet ondersteund in lokale ontwikkeling clusters.

## <a name="add-nodes-to-your-cluster"></a>Knooppunten toevoegen aan het cluster

1. Voorbereiden van de virtuele machine/machine die u toevoegen aan het cluster de stappen die worden beschreven wilt in [plannen en voorbereiden van uw implementatie van Service Fabric-cluster](service-fabric-cluster-creation-for-windows-server.md)
2. Identificeren welke foutdomein en upgradedomein die u gaat deze VM/machine toevoegen
3. Extern bureaublad (RDP) naar de virtuele machine/machine die u wilt toevoegen aan het cluster
4. Kopieer of [de zelfstandige downloaden voor Service Fabric voor Windows Server](http://go.microsoft.com/fwlink/?LinkId=730690) aan de virtuele machine/machine en pak het pakket
5. Powershell uitvoeren met verhoogde bevoegdheden en navigeer naar de locatie van de uitgepakte pakket
6. Voer de *AddNode.ps1* script met de parameters met een beschrijving van het nieuwe knooppunt om toe te voegen. In het volgende voorbeeld wordt een nieuw knooppunt VM5 aangeroepen toegevoegd, met het type NodeType0 en IP-adres 182.17.34.52, in UD1 en fd: / dc1/r0. De *ExistingClusterConnectionEndPoint* een eindpunt van de verbinding voor een knooppunt al in het bestaande cluster, wat kan het IP-adres zijn *eventuele* knooppunt in het cluster.

    ```
    .\AddNode.ps1 -NodeName VM5 -NodeType NodeType0 -NodeIPAddressorFQDN 182.17.34.52 -ExistingClientConnectionEndpoint 182.17.34.50:19000 -UpgradeDomain UD1 -FaultDomain fd:/dc1/r0 -AcceptEULA
    ```
    Nadat het script is voltooid, kunt u controleren of het nieuwe knooppunt is toegevoegd door het uitvoeren van de [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps) cmdlet.

7. De consistentie op verschillende knooppunten in het cluster, moet u een configuratie-upgrade initiëren. Voer [Get-ServiceFabricClusterConfiguration](/powershell/module/servicefabric/get-servicefabricclusterconfiguration?view=azureservicefabricps) ophalen van de meest recente bestand in de configuratie en het zojuist toegevoegde knooppunt toevoegen aan de sectie 'Knooppunten'. Het is ook raadzaam altijd de meest recente clusterconfiguratie in het geval dat u redploy een cluster met dezelfde configuratie moet hebben.

    ```
        {
            "nodeName": "vm5",
            "iPAddress": "182.17.34.52",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc1/r0",
            "upgradeDomain": "UD1"
        }
    ```
8. Voer [Start ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps) om te beginnen met de upgrade.

    ```
    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path to Configuration File>

    ```
    U kunt de voortgang van de upgrade in Service Fabric Explorer. U kunt ook uitvoeren [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps)

### <a name="add-nodes-to-clusters-configured-with-windows-security-using-gmsa"></a>Knooppunten toevoegen aan clusters die zijn geconfigureerd met behulp van gMSA Windows-beveiliging
Clusters die zijn geconfigureerd met een groep beheerde Service Account(gMSA) (https://technet.microsoft.com/library/hh831782.aspx), worden een nieuw knooppunt toegevoegd met behulp van een upgrade van een configuratie:
1. Voer [Get-ServiceFabricClusterConfiguration](/powershell/module/servicefabric/get-servicefabricclusterconfiguration?view=azureservicefabricps) op een van de bestaande knooppunten ophalen van de meest recente bestand in de configuratie en het toevoegen van meer informatie over het nieuwe knooppunt dat u wilt toevoegen in de sectie 'Knooppunten'. Zorg ervoor dat het nieuwe knooppunt maakt deel uit van dezelfde beheerd groepsaccount. Deze account moet een beheerder zijn op alle machines.

    ```
        {
            "nodeName": "vm5",
            "iPAddress": "182.17.34.52",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc1/r0",
            "upgradeDomain": "UD1"
        }
    ```
2. Voer [Start ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps) om te beginnen met de upgrade.

    ```
    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path to Configuration File>
    ```
    U kunt de voortgang van de upgrade in Service Fabric Explorer. U kunt ook uitvoeren [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps)

### <a name="add-node-types-to-your-cluster"></a>Knooppunttypen toevoegen aan het cluster
Uw-configuratie zodanig dat het nieuwe knooppunttype 'NodeTypes' in sectie onder 'Eigenschappen' en wilt beginnen met een configuratie wijzigen om een nieuw knooppunttype toevoegen, bijwerken met behulp van [Start ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps). Zodra de upgrade is voltooid, kunt u nieuwe knooppunten toevoegen aan het cluster met dit knooppunttype.

## <a name="remove-nodes-from-your-cluster"></a>Knooppunten van het cluster verwijderen
Een knooppunt kan worden verwijderd uit een cluster met behulp van de upgrade van een configuratie op de volgende manier:

1. Voer [Get-ServiceFabricClusterConfiguration](/powershell/module/servicefabric/get-servicefabricclusterconfiguration?view=azureservicefabricps) ophalen van de meest recente bestand in de configuratie en *verwijderen* het knooppunt uit de sectie 'Knooppunten'.
De parameter 'NodesToBeRemoved' toevoegen aan de sectie 'Instellen' in de sectie 'FabricSettings'. De '' waarde '' moet een door komma's gescheiden lijst met knooppuntnamen van knooppunten die moeten worden verwijderd.

    ```
         "fabricSettings": [
            {
            "name": "Setup",
            "parameters": [
                {
                "name": "FabricDataRoot",
                "value": "C:\\ProgramData\\SF"
                },
                {
                "name": "FabricLogRoot",
                "value": "C:\\ProgramData\\SF\\Log"
                },
                {
                "name": "NodesToBeRemoved",
                "value": "vm0, vm1"
                }
            ]
            }
        ]
    ```
2. Voer [Start ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps) om te beginnen met de upgrade.

    ```
    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path to Configuration File>

    ```
    U kunt de voortgang van de upgrade in Service Fabric Explorer. U kunt ook uitvoeren [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps)

> [!NOTE]
> Verwijderen van knooppunten mogelijk meerdere upgrades initiëren. Sommige knooppunten zijn gemarkeerd met `IsSeedNode=”true”` labelen en een query uitgevoerd op het cluster kunnen worden geïdentificeerd met manifest `Get-ServiceFabricClusterManifest`. Verwijderen van deze knooppunten kan het langer duren dan andere omdat de seed-knooppunten moeten worden verplaatst in dergelijke scenario's. Het cluster moet minimaal 3 primaire knooppunt type knooppunten onderhouden.
> 
> 

### <a name="remove-node-types-from-your-cluster"></a>Knooppunttypen verwijderen uit het cluster
Voordat u een knooppunttype verwijdert, Controleer als er knooppunten verwijzen naar het knooppunttype. Verwijder deze knooppunten voordat u het bijbehorende knooppunttype verwijdert. Zodra alle bijbehorende knooppunten zijn verwijderd, kunt u de NodeType verwijderen uit de clusterconfiguratie en beginnen met een configuratie met behulp van upgrade [Start ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps).


### <a name="replace-primary-nodes-of-your-cluster"></a>Vervang primaire knooppunten van het cluster
De vervanging van de primaire knooppunten moet één knooppunt uitgevoerd na de andere in plaats van te verwijderen en vervolgens toe te voegen in batches.


## <a name="next-steps"></a>Volgende stappen
* [Configuratie-instellingen voor zelfstandige Windows-cluster](service-fabric-cluster-manifest.md)
* [Beveiligen van een zelfstandige cluster op Windows met behulp van X509 certificaten](service-fabric-windows-cluster-x509-security.md)
* [Een zelfstandige Service Fabric-cluster maken met Azure Virtual machines waarop Windows wordt uitgevoerd](service-fabric-cluster-creation-with-windows-azure-vms.md)

