---
title: Een ILB-listener voor AlwaysOn-beschikbaarheidsgroepen configureren in Azure | Microsoft Docs
description: In deze zelfstudie maakt gebruik van resources die zijn gemaakt met het klassieke implementatiemodel en maakt een Always On-listener voor beschikbaarheidsgroep in Azure die gebruikmaakt van een interne load balancer.
services: virtual-machines-windows
documentationcenter: na
author: MikeRayMSFT
manager: craigg
editor: ''
tags: azure-service-management
ms.assetid: 291288a0-740b-4cfa-af62-053218beba77
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/02/2017
ms.author: mikeray
ms.openlocfilehash: 0466265ad5a24e8ea6dc5079e2b4006d74e7dde0
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38452529"
---
# <a name="configure-an-ilb-listener-for-always-on-availability-groups-in-azure"></a>Een ILB-listener voor AlwaysOn-beschikbaarheidsgroepen configureren in Azure
> [!div class="op_single_selector"]
> * [Interne listener](../classic/ps-sql-int-listener.md)
> * [Externe listener](../classic/ps-sql-ext-listener.md)
>
>

## <a name="overview"></a>Overzicht

> [!IMPORTANT]
> Azure heeft twee verschillende implementatiemodellen voor het maken van en werken met resources: [Azure Resource Manager en klassiek](../../../azure-resource-manager/resource-manager-deployment-model.md). In dit artikel bevat informatie over het gebruik van het klassieke implementatiemodel. We raden aan dat de meeste nieuwe implementaties het Resource Manager-model gebruiken.

Zie voor meer informatie over het configureren van een listener voor een AlwaysOn-beschikbaarheidsgroep in het Resource Manager-model [een load balancer voor een AlwaysOn-beschikbaarheidsgroep configureren in Azure](../sql/virtual-machines-windows-portal-sql-alwayson-int-listener.md).

De beschikbaarheidsgroep kan replica's die alleen on-premises of Azure alleen, of die zowel on-premises en Azure voor hybride configuraties omvatten bevatten. Azure replica's kunnen zich binnen dezelfde regio of in meerdere regio's die gebruikmaken van meerdere virtuele netwerken. De procedures in dit artikel wordt ervan uitgegaan dat u al hebt [geconfigureerd een beschikbaarheidsgroep](../classic/portal-sql-alwayson-availability-groups.md) maar nog niet zijn geconfigureerd op een listener.

## <a name="guidelines-and-limitations-for-internal-listeners"></a>Richtlijnen en beperkingen voor interne listeners
Het gebruik van een interne load balancer (ILB) met een listener voor de beschikbaarheidsgroep in Azure is onderworpen aan de volgende richtlijnen:

* De beschikbaarheidsgroep-listener wordt ondersteund op Windows Server 2008 R2, Windows Server 2012 en Windows Server 2012 R2.
* Slechts één interne beschikbaarheidsgroep-listener wordt ondersteund voor elke service in de cloud, omdat de listener is geconfigureerd voor de ILB en er slechts één ILB voor elke service in de cloud is. Het is echter mogelijk te maken van meerdere externe listeners. Zie voor meer informatie, [een externe listener voor AlwaysOn-beschikbaarheidsgroepen configureren in Azure](../classic/ps-sql-ext-listener.md).

## <a name="determine-the-accessibility-of-the-listener"></a>Bepalen van de toegankelijkheid van de listener
[!INCLUDE [ag-listener-accessibility](../../../../includes/virtual-machines-ag-listener-determine-accessibility.md)]

In dit artikel richt zich op het maken van een listener die gebruikmaakt van een ILB. Als u een openbare of externe listener nodig hebt, raadpleegt u de versie van dit artikel dat instelling van bespreekt een [externe listener](../classic/ps-sql-ext-listener.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

## <a name="create-load-balanced-vm-endpoints-with-direct-server-return"></a>Taakverdeling eindpunten voor virtuele machine met direct server return maken
U maakt eerst een ILB door het uitvoeren van het script later in deze sectie.

Maak een eindpunt met load balancing voor elke virtuele machine die als host fungeert voor een Azure-replica. Als u replica's in meerdere regio's hebt, moet elke replica voor deze regio zich in dezelfde cloudservice in dezelfde Azure virtual network. Het maken van beschikbaarheid van replica's die meerdere Azure-regio's omvatten, moet meerdere virtuele netwerken configureren. Zie voor meer informatie over het configureren van cross-verbinding met het virtuele netwerk [virtueel netwerk configureren voor verbinding met het virtuele netwerk](../../../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md).

1. In de Azure-portal, gaat u naar elke virtuele machine die als host fungeert voor een replica om de details weer te geven.

2. Klik op de **eindpunten** tabblad voor elke virtuele machine.

3. Controleer de **naam** en **openbare poort** van de listener-eindpunt dat u wilt gebruiken zijn niet al in gebruik. In het voorbeeld in deze sectie wordt de naam is *MyEndpoint*, en de poort is *1433*.

4. Download en installeer de meest recente op uw lokale client [PowerShell-module](https://azure.microsoft.com/downloads/).

5. Start Azure PowerShell.  
    Een nieuwe PowerShell-sessie wordt geopend, met de Azure-administratieve modules geladen.

6. Voer `Get-AzurePublishSettingsFile` uit. Deze cmdlet wordt u naar een browser voor het downloaden van een publish settings-bestand naar een lokale map. U mogelijk gevraagd uw aanmeldingsreferenties voor uw Azure-abonnement.

7. Voer de volgende `Import-AzurePublishSettingsFile` opdracht met het pad van het publish settings-bestand dat u hebt gedownload:

        Import-AzurePublishSettingsFile -PublishSettingsFile <PublishSettingsFilePath>

    Nadat het publish settings-bestand is geïmporteerd, kunt u uw Azure-abonnement in de PowerShell-sessie kunt beheren.

8. Voor *ILB*, een statisch IP-adres toewijzen. Controleer de huidige configuratie van virtueel netwerk met de volgende opdracht:

        (Get-AzureVNetConfig).XMLConfiguration
9. Houd er rekening mee de *Subnet* naam voor het subnet waarin de virtuele machines die als host fungeren de replica's. Deze naam wordt gebruikt in de parameter $SubnetName in het script.

10. Houd er rekening mee de *VirtualNetworkSite* naam en het starten van *AddressPrefix* voor het subnet met de virtuele machines die als host de replica's fungeren. Zoeken naar een beschikbaar IP-adres door het doorgeven van beide waarden aan de `Test-AzureStaticVNetIP` beheren en controleren door de *AvailableAddresses*. Bijvoorbeeld, als de naam van het virtuele netwerk *MyVNet* en heeft een subnet-adresbereik dat begint bij *172.16.0.128*, de volgende opdracht zou lijst met beschikbare adressen:

        (Test-AzureStaticVNetIP -VNetName "MyVNet"-IPAddress 172.16.0.128).AvailableAddresses
11. Selecteer een van de beschikbare adressen en worden gebruikt in de parameter $ILBStaticIP van het script in de volgende stap.

12. Kopieer het volgende PowerShell-script naar een teksteditor en stel de variabele waarden op basis van uw omgeving. Standaardwaarden hebt opgegeven voor bepaalde parameters.  

    Bestaande implementaties die gebruikmaken van affiniteitsgroepen kunnen een ILB niet toevoegen. Zie voor meer informatie over de vereisten voor ILB [overzicht van interne load balancer](../../../load-balancer/load-balancer-internal-overview.md).

    Ook als uw beschikbaarheidsgroep Azure-regio's omvat, moet u het script eenmaal uitgevoerd in elk datacenter voor de service in de cloud en de knooppunten die zich in dit datacenter bevinden.

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

13. Nadat u de variabelen hebt ingesteld, kopieert u het script van de teksteditor aan uw PowerShell-sessie uit te voeren. Als u nog steeds weergegeven in de prompt **>>**, druk op Enter opnieuw te controleren of het script wordt uitgevoerd.

## <a name="verify-that-kb2854082-is-installed-if-necessary"></a>Controleer of KB2854082 is geïnstalleerd, indien nodig
[!INCLUDE [kb2854082](../../../../includes/virtual-machines-ag-listener-kb2854082.md)]

## <a name="open-the-firewall-ports-in-availability-group-nodes"></a>De firewall-poorten openen in de beschikbaarheid van groepsknooppunten
[!INCLUDE [firewall](../../../../includes/virtual-machines-ag-listener-open-firewall.md)]

## <a name="create-the-availability-group-listener"></a>De beschikbaarheidsgroep-listener maken

Maak de beschikbaarheidsgroep-listener in twee stappen. Eerst de clusterresource van client access point maken en configureren van afhankelijkheden. Ten tweede configureren de clusterbronnen in PowerShell.

### <a name="create-the-client-access-point-and-configure-the-cluster-dependencies"></a>De client access point maken en configureren van de cluster-afhankelijkheden
[!INCLUDE [firewall](../../../../includes/virtual-machines-ag-listener-create-listener.md)]

### <a name="configure-the-cluster-resources-in-powershell"></a>De clusterbronnen in PowerShell configureren
1. Voor ILB, moet u het IP-adres van de ILB die eerder is gemaakt. Als u dit IP-adres in PowerShell, gebruikt u het volgende script:

        # Define variables
        $ServiceName="<MyServiceName>" # the name of the cloud service that contains the AG nodes
        (Get-AzureInternalLoadBalancer -ServiceName $ServiceName).IPAddress

2. Kopieer het PowerShell-script voor het besturingssysteem naar een teksteditor op een van de virtuele machines, en stelt u de variabelen met de waarden die u eerder hebt genoteerd.

    Voor Windows Server 2012 of hoger, moet u het volgende script gebruiken:

        # Define variables
        $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
        $IPResourceName = "<IPResourceName>" # the IP address resource name
        $ILBIP = "<X.X.X.X>" # the IP address of the ILB

        Import-Module FailoverClusters

        Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"="59999";"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}

    Voor Windows Server 2008 R2, moet u het volgende script gebruiken:

        # Define variables
        $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
        $IPResourceName = "<IPResourceName>" # the IP address resource name
        $ILBIP = "<X.X.X.X>" # the IP address of the ILB

        Import-Module FailoverClusters

        cluster res $IPResourceName /priv enabledhcp=0 address=$ILBIP probeport=59999  subnetmask=255.255.255.255

3. Nadat u de variabelen hebt ingesteld, open een verhoogde Windows PowerShell-venster, het script van de teksteditor te plakken in uw PowerShell-sessie uit te voeren. Als u nog steeds weergegeven in de prompt **>>**, druk op Enter opnieuw om het ervoor te zorgen dat het script wordt uitgevoerd.

4. Herhaal de voorgaande stappen voor elke virtuele machine.  
    Met dit script wordt de resource van het IP-adres geconfigureerd met het IP-adres van de cloudservice en andere parameters, zoals de testpoort ingesteld. Als de IP-adresresource online is geplaatst, kan het reageren op de polling op de testpoort van het eindpunt met gelijke die u eerder hebt gemaakt.

## <a name="bring-the-listener-online"></a>De listener online brengen
[!INCLUDE [Bring-Listener-Online](../../../../includes/virtual-machines-ag-listener-bring-online.md)]

## <a name="follow-up-items"></a>Opvolging items
[!INCLUDE [Follow-up](../../../../includes/virtual-machines-ag-listener-follow-up.md)]

## <a name="test-the-availability-group-listener-within-the-same-virtual-network"></a>Testen van de beschikbaarheidsgroep-listener (binnen hetzelfde virtuele netwerk)
[!INCLUDE [Test-Listener-Within-VNET](../../../../includes/virtual-machines-ag-listener-test.md)]

## <a name="next-steps"></a>Volgende stappen
[!INCLUDE [Listener-Next-Steps](../../../../includes/virtual-machines-ag-listener-next-steps.md)]
