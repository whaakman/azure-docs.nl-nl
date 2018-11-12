---
title: Toevoegen of verwijderen van knooppunten in een zelfstandige Service Fabric-cluster | Microsoft Docs
description: Informatie over het toevoegen of verwijderen van knooppunten in een Azure Service Fabric-cluster op een fysieke of virtuele machine met Windows Server, wat erop kan on-premises of in de cloud.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: ''
ms.assetid: bc6b8fc0-d2af-42f8-a164-58538be38d02
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/02/2017
ms.author: dekapur
ms.openlocfilehash: 69680331bdad0faa36cb3df6117baf8b358da132
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51251016"
---
# <a name="add-or-remove-nodes-to-a-standalone-service-fabric-cluster-running-on-windows-server"></a>Toevoegen of verwijderen van knooppunten in een zelfstandige Service Fabric-cluster op Windows Server
Nadat u hebt [uw zelfstandige Service Fabric-cluster gemaakt op Windows Server-machines](service-fabric-cluster-creation-for-windows-server.md), uw behoeften (bedrijven) kunnen worden gewijzigd en u moet toevoegen of verwijderen van knooppunten in uw cluster. Dit artikel bevat gedetailleerde stappen om dit te bereiken. Houd er rekening mee dat toevoegen/verwijderen knooppunt functionaliteit wordt niet ondersteund in lokale ontwikkeling clusters.

## <a name="add-nodes-to-your-cluster"></a>Knooppunten toevoegen aan uw cluster

1. Voorbereiden van de virtuele machine/de machine die u toevoegen aan uw cluster wilt met de volgende stappen wordt beschreven in [plannen en voorbereiden van de implementatie van uw Service Fabric-cluster](service-fabric-cluster-creation-for-windows-server.md)
2. Identificeren welke foutdomein en upgradedomein zult u deze virtuele machine/machine toevoegen
3. Extern bureaublad (RDP) op de virtuele machine/machine die u wilt toevoegen aan het cluster
4. Kopiëren of [downloaden van het zelfstandige pakket voor Service Fabric voor Windows Server](https://go.microsoft.com/fwlink/?LinkId=730690) aan de virtuele machine/machine en pak het pakket
5. Powershell ook uitvoeren met verhoogde bevoegdheden en navigeer naar de locatie van de uitgepakte pakket
6. Voer de *AddNode.ps1* script met de parameters met een beschrijving van het nieuwe knooppunt toe te voegen. In het volgende voorbeeld wordt een nieuw knooppunt met de naam VM5 toegevoegd, met het type NodeType0 en IP-adres 182.17.34.52, in UD1 en fd: / dc1/r0. De *ExistingClusterConnectionEndPoint* een verbindingseindpunt voor een knooppunt in het bestaande cluster, al is dat het IP-adres is *eventuele* knooppunt in het cluster.

    ```
    .\AddNode.ps1 -NodeName VM5 -NodeType NodeType0 -NodeIPAddressorFQDN 182.17.34.52 -ExistingClientConnectionEndpoint 182.17.34.50:19000 -UpgradeDomain UD1 -FaultDomain fd:/dc1/r0 -AcceptEULA
    ```
    Nadat het script is voltooid, kunt u controleren of het nieuwe knooppunt is toegevoegd door het uitvoeren van de [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps) cmdlet.

7. Om te zorgen voor consistentie op verschillende knooppunten in het cluster, moet u een configuratie-upgrade start. Voer [Get-ServiceFabricClusterConfiguration](/powershell/module/servicefabric/get-servicefabricclusterconfiguration?view=azureservicefabricps) voor het ophalen van de meest recente configuratiebestand en het zojuist toegevoegde knooppunt toevoegen aan de sectie 'Knooppunten'. Het is ook raadzaam altijd de meest recente configuratie van het cluster beschikbaar is in het geval dat u implementeert een cluster met dezelfde configuratie wilt hebben.

    ```
        {
            "nodeName": "vm5",
            "iPAddress": "182.17.34.52",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc1/r0",
            "upgradeDomain": "UD1"
        }
    ```
8. Voer [Start ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps) om de upgrade te starten.

    ```
    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path to Configuration File>

    ```
    U kunt de voortgang van de upgrade van Service Fabric Explorer. U kunt ook uitvoeren [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps)

### <a name="add-nodes-to-clusters-configured-with-windows-security-using-gmsa"></a>Knooppunten toevoegen aan de clusters die zijn geconfigureerd met Windows-beveiliging met behulp van gMSA
Voor clusters die zijn geconfigureerd met Group Managed Service Account(gMSA) (https://technet.microsoft.com/library/hh831782.aspx), een nieuw knooppunt kan worden toegevoegd met behulp van een configuratie-upgrade:
1. Voer [Get-ServiceFabricClusterConfiguration](/powershell/module/servicefabric/get-servicefabricclusterconfiguration?view=azureservicefabricps) op een van de bestaande knooppunten voor het ophalen van de meest recente configuratiebestand en meer informatie over het nieuwe knooppunt dat u wilt toevoegen in de sectie 'Knooppunten' toevoegen. Zorg ervoor dat het nieuwe knooppunt maakt deel uit van dezelfde beheerd groepsaccount. Dit account moet een beheerder zijn op alle machines.

    ```
        {
            "nodeName": "vm5",
            "iPAddress": "182.17.34.52",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc1/r0",
            "upgradeDomain": "UD1"
        }
    ```
2. Voer [Start ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps) om de upgrade te starten.

    ```
    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path to Configuration File>
    ```
    U kunt de voortgang van de upgrade van Service Fabric Explorer. U kunt ook uitvoeren [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps)

### <a name="add-node-types-to-your-cluster"></a>Knooppunttypen toevoegen aan uw cluster
Als u wilt een nieuw knooppunttype toevoegen, wijzigen de configuratie zodanig dat het nieuwe knooppunttype in 'NodeTypes' onder 'Eigenschappen' sectie en beginnen met een configuratie met behulp van een upgrade uitvoert [Start ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps). Nadat de upgrade is voltooid, kunt u nieuwe knooppunten toevoegen aan uw cluster met dit knooppunttype.

## <a name="remove-nodes-from-your-cluster"></a>Knooppunten van het cluster verwijderen
Een knooppunt kan worden verwijderd uit een cluster met behulp van de upgrade van een configuratie, op de volgende manier:

1. Voer [Get-ServiceFabricClusterConfiguration](/powershell/module/servicefabric/get-servicefabricclusterconfiguration?view=azureservicefabricps) om op te halen van de meest recente configuratiebestand en *verwijderen* het knooppunt uit de sectie 'Knooppunten'.
De parameter 'NodesToBeRemoved' toevoegen aan de sectie 'Instellen' in de sectie 'Instelling FabricSettings'. De 'waarde' moet een door komma's gescheiden lijst met knooppuntnamen van de knooppunten die moeten worden verwijderd.

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
2. Voer [Start ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps) om de upgrade te starten.

    ```
    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path to Configuration File>

    ```
    U kunt de voortgang van de upgrade van Service Fabric Explorer. U kunt ook uitvoeren [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps)

> [!NOTE]
> Het verwijderen van knooppunten kan meerdere upgrades initiëren. Sommige knooppunten zijn gemarkeerd met `IsSeedNode=”true”` markeren en kunnen worden geïdentificeerd door het opvragen van het cluster met behulp van het manifest `Get-ServiceFabricClusterManifest`. Het verwijderen van deze knooppunten duurt mogelijk langer dan andere omdat het seed-knooppunten worden verplaatst moeten om in dergelijke scenario's. Het cluster moet minimaal 3 knooppunten van de primaire knooppunt type behouden.
> 
> 

### <a name="remove-node-types-from-your-cluster"></a>Knooppunttypen van het cluster verwijderen
Voordat u verwijdert een knooppunttype, Controleer Double-waarde of er zijn geen knooppunten die verwijst naar het knooppunttype. Verwijder deze knooppunten voordat u het bijbehorende knooppunttype verwijdert. Wanneer alle bijbehorende knooppunten zijn verwijderd, kunt u de NodeType verwijderen uit de configuratie van het cluster en beginnen met een configuratie met behulp van een upgrade uitvoert [Start ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps).


### <a name="replace-primary-nodes-of-your-cluster"></a>Vervang primaire knooppunten van het cluster
De vervanging van de primaire knooppunten moet één knooppunt uitgevoerd na de andere, in plaats van te verwijderen en vervolgens toe te voegen in batches.


## <a name="next-steps"></a>Volgende stappen
* [Configuratie-instellingen voor zelfstandige Windows-cluster](service-fabric-cluster-manifest.md)
* [Een zelfstandige cluster beveiligen op Windows met behulp van X509 certificaten](service-fabric-windows-cluster-x509-security.md)
* [Een zelfstandige Service Fabric-cluster maken met virtuele Azure-machines waarop Windows wordt uitgevoerd](service-fabric-cluster-creation-with-windows-azure-vms.md)

