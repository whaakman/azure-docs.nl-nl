---
title: Een ILB-listener voor AlwaysOn-beschikbaarheidsgroepen configureren in Azure | Microsoft Docs
description: Deze zelfstudie maakt gebruik van bronnen die zijn gemaakt met het klassieke implementatiemodel en maakt een altijd op beschikbaarheidsgroep-listener in Azure die gebruikmaakt van een interne load balancer.
services: virtual-machines-windows
documentationcenter: na
author: MikeRayMSFT
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 291288a0-740b-4cfa-af62-053218beba77
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/02/2017
ms.author: mikeray
ms.openlocfilehash: fea70b389b1f1d6af963e3f14fdc48e8d857dd53
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="configure-an-ilb-listener-for-always-on-availability-groups-in-azure"></a>Een ILB-listener voor AlwaysOn-beschikbaarheidsgroepen configureren in Azure
> [!div class="op_single_selector"]
> * [Interne listener](../classic/ps-sql-int-listener.md)
> * [Externe-listener](../classic/ps-sql-ext-listener.md)
>
>

## <a name="overview"></a>Overzicht

> [!IMPORTANT]
> Azure heeft twee verschillende implementatiemodellen voor het maken en werken met resources: [Azure Resource Manager en classic](../../../azure-resource-manager/resource-manager-deployment-model.md). In dit artikel bevat informatie over het gebruik van het klassieke implementatiemodel. U wordt aangeraden de meeste nieuwe implementaties het Resource Manager-model gebruiken.

Zie voor meer informatie over het configureren van een listener voor een AlwaysOn-beschikbaarheidsgroep in het Resource Manager-model [een load balancer voor een AlwaysOn-beschikbaarheidsgroep configureren in Azure](../sql/virtual-machines-windows-portal-sql-alwayson-int-listener.md).

De beschikbaarheidsgroep kan de replica's die alleen op lokale of Azure alleen, of die zowel on-premises en Azure voor hybride configuraties omvatten bevatten. Azure replica's kunnen zich binnen dezelfde regio of tussen meerdere regio's die gebruikmaken van meerdere virtuele netwerken. De procedures in dit artikel wordt ervan uitgegaan dat u al hebt [geconfigureerd een beschikbaarheidsgroep](../classic/portal-sql-alwayson-availability-groups.md) maar nog niet zijn ingesteld op een listener.

## <a name="guidelines-and-limitations-for-internal-listeners"></a>Richtlijnen en beperkingen voor interne listeners
Het gebruik van een interne load balancer (ILB) met een beschikbaarheidsgroep-listener in Azure is onderworpen aan de volgende richtlijnen:

* De beschikbaarheidsgroep-listener wordt ondersteund op Windows Server 2008 R2, Windows Server 2012 en Windows Server 2012 R2.
* Slechts één interne beschikbaarheidsgroep-listener wordt ondersteund voor elke cloudservice, omdat de listener is geconfigureerd voor de ILB en er slechts één ILB voor elke cloudservice is. Het is echter mogelijk te maken van meerdere externe listeners. Zie voor meer informatie [een externe-listener voor AlwaysOn-beschikbaarheidsgroepen configureren in Azure](../classic/ps-sql-ext-listener.md).

## <a name="determine-the-accessibility-of-the-listener"></a>De toegankelijkheid van de listener bepalen
[!INCLUDE [ag-listener-accessibility](../../../../includes/virtual-machines-ag-listener-determine-accessibility.md)]

Dit artikel is gericht op het maken van een listener die gebruikmaakt van een ILB. Als u een openbare of externe-listener moet, raadpleegt u de versie van dit artikel dat instelling van bespreekt een [externe listener](../classic/ps-sql-ext-listener.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

## <a name="create-load-balanced-vm-endpoints-with-direct-server-return"></a>VM-eindpunten taakverdeling met direct server return maken
U eerst maken een ILB door het uitvoeren van het script later in deze sectie.

Maak een eindpunt taakverdeling voor elke virtuele machine die als host fungeert voor een Azure-replica. Als u replica's in meerdere regio's hebt, moet elke replica voor de regio die zich in dezelfde cloudservice in hetzelfde virtuele netwerk van Azure. Maken van replica's die meerdere Azure-regio's omvatten voor beschikbaarheid, moet meerdere virtuele netwerken configureren. Zie voor meer informatie over het configureren van cross-virtuele netwerkverbindingen [virtueel netwerk configureren voor verbinding met het virtuele netwerk](../../../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md).

1. In de Azure portal, gaat u naar elke virtuele machine die als host fungeert voor een replica om de details weer te geven.

2. Klik op de **eindpunten** tabblad voor elke virtuele machine.

3. Controleer de **naam** en **openbare poort** van de listener-eindpunt dat u wilt gebruiken zijn niet al in gebruik. In het voorbeeld in deze sectie wordt de naam is *MyEndpoint*, en de poort is *1433*.

4. Download en installeer de meest recente op de lokale client [PowerShell-module](https://azure.microsoft.com/downloads/).

5. Azure PowerShell te starten.  
    Een nieuwe PowerShell-sessie wordt geopend met de Azure beheerdersrechten modules geladen.

6. Voer `Get-AzurePublishSettingsFile` uit. Deze cmdlet wordt verwezen naar een browser om te downloaden van een bestand met publicatie-instellingen naar een lokale map. U mogelijk gevraagd om uw referenties aanmelden om uw Azure-abonnement.

7. Voer de volgende `Import-AzurePublishSettingsFile` opdracht met het pad van het bestand publiceren instellingen die u hebt gedownload:

        Import-AzurePublishSettingsFile -PublishSettingsFile <PublishSettingsFilePath>

    Nadat het instellingenbestand publiceren is geïmporteerd, kunt u uw Azure-abonnement in de PowerShell-sessie kunt beheren.

8. Voor *ILB*, een statisch IP-adres toewijzen. Controleer de configuratie van de huidige virtueel netwerk met de volgende opdracht:

        (Get-AzureVNetConfig).XMLConfiguration
9. Opmerking de *Subnet* naam voor het subnet waarin de virtuele machines die als host fungeren de replica's. Deze naam wordt gebruikt in de parameter $SubnetName in het script.

10. Opmerking de *VirtualNetworkSite* naam en het starten van *AddressPrefix* voor het subnet waarin de virtuele machines die als host fungeren van de replica's. Zoekt een beschikbaar IP-adres door beide waarden geven de `Test-AzureStaticVNetIP` opdracht en gecontroleerd door de *AvailableAddresses*. Bijvoorbeeld, als de naam van het virtuele netwerk *MyVNet* en heeft een subnet-adresbereik dat begint bij *172.16.0.128*, de volgende opdracht zou lijst beschikbare adressen:

        (Test-AzureStaticVNetIP -VNetName "MyVNet"-IPAddress 172.16.0.128).AvailableAddresses
11. Selecteer een van de beschikbare adressen en deze gebruiken in de parameter $ILBStaticIP van het script in de volgende stap.

12. Kopieer het volgende PowerShell-script naar een teksteditor en stel de variabele waarden aanpassen aan uw omgeving. Standaardwaarden zijn opgegeven voor een aantal parameters.  

    Bestaande implementaties die gebruikmaken van affiniteitsgroepen kunnen een ILB niet toevoegen. Zie voor meer informatie over de vereisten voor de ILB [interne load balancer overzicht](../../../load-balancer/load-balancer-internal-overview.md).

    Ook als de beschikbaarheidsgroep Azure-regio's omvat, moet u het script eenmaal uitgevoerd in elk datacenter voor de cloudservice en de knooppunten die zich in dat datacenter bevinden.

        # Define variables
        $ServiceName = "<MyCloudService>" # the name of the cloud service that contains the availability group nodes
        $AGNodes = "<VM1>","<VM2>","<VM3>" # all availability group nodes containing replicas in the same cloud service, separated by commas
        $SubnetName = "<MySubnetName>" # subnet name that the replicas use in the virtual network
        $ILBStaticIP = "<MyILBStaticIPAddress>" # static IP address for the ILB in the subnet
        $ILBName = "AGListenerLB" # customize the ILB name or use this default value

        # Create the ILB
        Add-AzureInternalLoadBalancer -InternalLoadBalancerName $ILBName -SubnetName $SubnetName -ServiceName $ServiceName -StaticVNetIPAddress $ILBStaticIP

        # Configure a load-balanced endpoint for each node in $AGNodes by using ILB
        ForEach ($node in $AGNodes)
        {
            Get-AzureVM -ServiceName $ServiceName -Name $node | Add-AzureEndpoint -Name "ListenerEndpoint" -LBSetName "ListenerEndpointLB" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -InternalLoadBalancerName $ILBName -DirectServerReturn $true | Update-AzureVM
        }

13. Nadat u de variabelen hebt ingesteld, Kopieer het script in de teksteditor op uw PowerShell-sessie uit te voeren. Als de prompt nog steeds  **>>** , druk op Enter opnieuw te controleren of het script wordt gestart.

## <a name="verify-that-kb2854082-is-installed-if-necessary"></a>Controleer of KB2854082 wordt indien nodig geïnstalleerd
[!INCLUDE [kb2854082](../../../../includes/virtual-machines-ag-listener-kb2854082.md)]

## <a name="open-the-firewall-ports-in-availability-group-nodes"></a>De firewallpoorten openen in de beschikbaarheid van groepsknooppunten
[!INCLUDE [firewall](../../../../includes/virtual-machines-ag-listener-open-firewall.md)]

## <a name="create-the-availability-group-listener"></a>Maken van de beschikbaarheidsgroep-listener

Maak de beschikbaarheidsgroep-listener in twee stappen. Eerst de clusterbron van client access point maken en configureren van afhankelijkheden. Ten tweede configureren de clusterbronnen in PowerShell.

### <a name="create-the-client-access-point-and-configure-the-cluster-dependencies"></a>De client access point maken en configureren van de cluster-afhankelijkheden
[!INCLUDE [firewall](../../../../includes/virtual-machines-ag-listener-create-listener.md)]

### <a name="configure-the-cluster-resources-in-powershell"></a>Configureer de clusterbronnen in PowerShell
1. Voor de ILB, moet u het IP-adres van de ILB die eerder is gemaakt. Gebruik het volgende script om dit IP-adres in PowerShell:

        # Define variables
        $ServiceName="<MyServiceName>" # the name of the cloud service that contains the AG nodes
        (Get-AzureInternalLoadBalancer -ServiceName $ServiceName).IPAddress

2. Op een van de virtuele machines, kopieert u het PowerShell-script voor uw besturingssysteem naar een teksteditor en stel de variabelen aan de waarden die u eerder hebt genoteerd.

    Voor Windows Server 2012 of hoger, gebruikt u het volgende script:

        # Define variables
        $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
        $IPResourceName = "<IPResourceName>" # the IP address resource name
        $ILBIP = “<X.X.X.X>” # the IP address of the ILB

        Import-Module FailoverClusters

        Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"="59999";"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}

    Voor Windows Server 2008 R2, gebruikt u het volgende script:

        # Define variables
        $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
        $IPResourceName = "<IPResourceName>" # the IP address resource name
        $ILBIP = “<X.X.X.X>” # the IP address of the ILB

        Import-Module FailoverClusters

        cluster res $IPResourceName /priv enabledhcp=0 address=$ILBIP probeport=59999  subnetmask=255.255.255.255

3. Nadat u de variabelen hebt ingesteld, opent u een Windows PowerShell-venster met verhoogde bevoegdheid, het script in de teksteditor te plakken in uw PowerShell-sessie uit te voeren. Als de prompt nog steeds  **>>** , druk op Enter opnieuw om er zeker van te zijn dat het script wordt gestart.

4. Herhaal de voorgaande stappen voor elke virtuele machine.  
    Dit script voor het configureren van de bron van het IP-adres met het IP-adres van de cloudservice en andere parameters, zoals de testpoort ingesteld. Wanneer de bron van het IP-adres online is gebracht, kan het reageren op de polling op de testpoort van het eindpunt met gelijke taakverdeling die u eerder hebt gemaakt.

## <a name="bring-the-listener-online"></a>De listener online brengen
[!INCLUDE [Bring-Listener-Online](../../../../includes/virtual-machines-ag-listener-bring-online.md)]

## <a name="follow-up-items"></a>Items worden opgevolgd
[!INCLUDE [Follow-up](../../../../includes/virtual-machines-ag-listener-follow-up.md)]

## <a name="test-the-availability-group-listener-within-the-same-virtual-network"></a>Testen van de beschikbaarheidsgroep-listener (binnen hetzelfde virtuele netwerk)
[!INCLUDE [Test-Listener-Within-VNET](../../../../includes/virtual-machines-ag-listener-test.md)]

## <a name="next-steps"></a>Volgende stappen
[!INCLUDE [Listener-Next-Steps](../../../../includes/virtual-machines-ag-listener-next-steps.md)]
