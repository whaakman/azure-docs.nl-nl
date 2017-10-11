---
title: Een externe-Listener voor AlwaysOn-beschikbaarheidsgroepen configureren | Microsoft Docs
description: Deze zelfstudie leert u stappen voor het maken van een altijd op beschikbaarheidsgroep-Listener in Azure die extern toegankelijk is via de openbare virtuele IP-adres van de gekoppelde cloud-service.
services: virtual-machines-windows
documentationcenter: na
author: MikeRayMSFT
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: a2453032-94ab-4775-b976-c74d24716728
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/31/2017
ms.author: mikeray
ms.openlocfilehash: 8e506be42aea4fb3c48c29b771a78dcf694f4518
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="configure-an-external-listener-for-always-on-availability-groups-in-azure"></a>Een externe-listener voor AlwaysOn-beschikbaarheidsgroepen configureren in Azure
> [!div class="op_single_selector"]
> * [Interne Listener](../classic/ps-sql-int-listener.md)
> * [Externe-Listener](../classic/ps-sql-ext-listener.md)
> 
> 

Dit onderwerp leest u hoe u configureert een listener voor een AlwaysOn-beschikbaarheidsgroep die extern toegankelijk is via het internet. Dit wordt mogelijk gemaakt door te koppelen van de cloudservice **openbare virtuele IP-adres (VIP)** adres met de listener.

> [!IMPORTANT] 
> Azure heeft twee verschillende implementatiemodellen voor het maken en werken met resources: [Resource Manager en Classic](../../../azure-resource-manager/resource-manager-deployment-model.md). In dit artikel bevat informatie over met behulp van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken.

De beschikbaarheidsgroep kunt replica's die zich on-premises alleen Azure alleen, of bevatten zowel on-premises en Azure span voor hybride configuraties. Azure replica's kunnen zich binnen dezelfde regio of tussen meerdere regio's met meerdere virtuele netwerken (vnet's). De volgende stappen wordt ervan uitgegaan dat u al hebt [geconfigureerd een beschikbaarheidsgroep](../classic/portal-sql-alwayson-availability-groups.md) maar een listener niet hebt geconfigureerd.

## <a name="guidelines-and-limitations-for-external-listeners"></a>Richtlijnen en beperkingen voor externe listeners
Houd rekening met de volgende richtlijnen over de beschikbaarheidsgroeplistener in Azure wanneer u implementeert met behulp van de cloud service symphysis VIP-adres:

* De beschikbaarheidsgroep-listener wordt ondersteund op Windows Server 2008 R2, Windows Server 2012 en Windows Server 2012 R2.
* De clienttoepassing moet zich bevinden op een andere cloudservice dan de database met uw virtuele machines van de beschikbaarheidsgroep. Azure biedt geen ondersteuning voor rechtstreekse serverretournering met de client en server in dezelfde cloudservice.
* Standaard is de stappen in dit artikel laten zien hoe een listener voor het gebruik van het hulpprogramma voor het virtuele IP-adres (VIP) van de cloud-serviceadres configureren. Het is echter mogelijk te reserveren en meerdere VIP-adressen voor uw cloudservice maken. Hiermee kunt u de stappen in dit artikel gebruiken voor het maken van meerdere-listeners die gekoppeld aan een andere VIP zijn. Zie voor meer informatie over het maken van meerdere VIP-adressen [meerdere VIP's per cloudservice](../../../load-balancer/load-balancer-multivip.md).
* Als u een listener voor een hybride omgeving maken wilt, moet de on-premises netwerk verbonden zijn met het openbare Internet naast de site-naar-site VPN-verbinding met het Azure-netwerk hebben. De beschikbaarheidsgroep-listener is in de Azure-subnet bereikbaar alleen voor het openbare IP-adres van de respectievelijke cloud-service.
* Maken van een externe-listener in dezelfde cloudservice waar hebt u ook een interne listener met behulp van de interne Load Balancer (ILB) wordt niet ondersteund.

## <a name="determine-the-accessibility-of-the-listener"></a>De toegankelijkheid van de listener bepalen
[!INCLUDE [ag-listener-accessibility](../../../../includes/virtual-machines-ag-listener-determine-accessibility.md)]

In dit artikel is gericht op het maken van een listener die gebruikmaakt van **externe load balancing**. Als u een listener waarvoor privé met het virtuele netwerk is wilt, raadpleegt u de versie van dit artikel die voorziet in stappen voor het instellen van een [listener met ILB](../classic/ps-sql-int-listener.md)

## <a name="create-load-balanced-vm-endpoints-with-direct-server-return"></a>VM-eindpunten taakverdeling met direct server return maken
Externe load balancing maakt gebruik van de virtuele de openbare virtuele IP-adres van de cloudservice die als host fungeert voor uw virtuele machines. U hoeft dus niet te maken of configureren van de load balancer in dit geval.

U moet een eindpunt met gelijke taakverdeling maken voor elke virtuele machine die als host fungeert voor de replica van een Azure. Als u replica's in meerdere regio's hebt, moet elke replica voor de regio die zich in dezelfde cloudservice in hetzelfde VNet. Beschikbaarheidsgroep maken van replica's die meerdere Azure-regio's omvatten is vereist voor het configureren van meerdere VNets. Zie voor meer informatie over het configureren van cross-VNet-connectiviteit [configureren VNet-naar-VNet-connectiviteit](../../../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md).

1. Navigeer naar elke VM die als host fungeert voor een replica in de Azure portal en bekijk de details.
2. Klik op de **eindpunten** tabblad voor elk van de virtuele machines.
3. Controleer de **naam** en **openbare poort** van de listener eindpunt dat u wilt gebruiken is niet al in gebruik. In het onderstaande voorbeeld is de naam 'MyEndpoint' en de poort is "1433".
4. Op de lokale client downloaden en installeren [de meest recente PowerShell-module](https://azure.microsoft.com/downloads/).
5. Start **Azure PowerShell**. Een nieuwe PowerShell-sessie is geopend met de Azure beheerdersrechten modules geladen.
6. Voer **Get-AzurePublishSettingsFile**. Deze cmdlet wordt verwezen naar een browser om te downloaden van een bestand met publicatie-instellingen naar een lokale map. U mogelijk gevraagd om uw referenties aanmelden voor uw Azure-abonnement.
7. Voer de **importeren AzurePublishSettingsFile** opdracht met het pad van het bestand publiceren instellingen die u hebt gedownload:
   
        Import-AzurePublishSettingsFile -PublishSettingsFile <PublishSettingsFilePath>
   
    Zodra het instellingenbestand publiceren wordt geïmporteerd, kunt u uw Azure-abonnement in de PowerShell-sessie kunt beheren.
    
1. Kopieer het onderstaande PowerShell-script in een teksteditor en stel de variabele waarden aanpassen aan uw omgeving (standaardwaarden zijn opgegeven voor sommige parameters). Houd er rekening mee dat als uw beschikbaarheidsgroep Azure-regio's omvat, u moet het script eenmaal uitgevoerd in elk datacenter voor de cloudservice en de knooppunten die zich in dat datacenter bevinden.
   
        # Define variables
        $ServiceName = "<MyCloudService>" # the name of the cloud service that contains the availability group nodes
        $AGNodes = "<VM1>","<VM2>","<VM3>" # all availability group nodes containing replicas in the same cloud service, separated by commas
   
        # Configure a load balanced endpoint for each node in $AGNodes, with direct server return enabled
        ForEach ($node in $AGNodes)
        {
            Get-AzureVM -ServiceName $ServiceName -Name $node | Add-AzureEndpoint -Name "ListenerEndpoint" -Protocol "TCP" -PublicPort 1433 -LocalPort 1433 -LBSetName "ListenerEndpointLB" -ProbePort 59999 -ProbeProtocol "TCP" -DirectServerReturn $true | Update-AzureVM
        }

2. Als u de variabelen hebt ingesteld, Kopieer het script in de teksteditor in uw Azure PowerShell-sessie uit te voeren. Als de prompt nog steeds >>, typt u ENTER opnieuw te controleren of het script wordt gestart.

## <a name="verify-that-kb2854082-is-installed-if-necessary"></a>Controleer of KB2854082 wordt indien nodig geïnstalleerd
[!INCLUDE [kb2854082](../../../../includes/virtual-machines-ag-listener-kb2854082.md)]

## <a name="open-the-firewall-ports-in-availability-group-nodes"></a>De firewallpoorten openen in de beschikbaarheid van groepsknooppunten
[!INCLUDE [firewall](../../../../includes/virtual-machines-ag-listener-open-firewall.md)]

## <a name="create-the-availability-group-listener"></a>Maken van de beschikbaarheidsgroep-listener

Maak de beschikbaarheidsgroep-listener in twee stappen. Eerst de clusterbron van client access point maken en configureren van afhankelijkheden. Ten tweede de clusterbronnen configureren met PowerShell.

### <a name="create-the-client-access-point-and-configure-the-cluster-dependencies"></a>De client access point maken en configureren van de cluster-afhankelijkheden
[!INCLUDE [firewall](../../../../includes/virtual-machines-ag-listener-create-listener.md)]

### <a name="configure-the-cluster-resources-in-powershell"></a>Configureer de clusterbronnen in PowerShell
1. Externe load balancing, moet u de openbare virtuele IP-adres van de cloudservice die uw replica's bevat. Meld u aan bij de Azure-portal. Navigeer naar de cloudservice die uw beschikbaarheidsgroep VM bevat. Open de **Dashboard** weergeven.
2. Let op het adres dat in **adres van de openbare virtuele IP-adres (VIP)**. Als uw oplossing VNets omvat, moet u deze stap herhalen voor elke cloudservice die een virtuele machine die als host fungeert voor een replica bevat.
3. Op een van de virtuele machines, Kopieer het onderstaande PowerShell-script in een teksteditor en stel de variabelen op de waarden die u eerder hebt genoteerd.
   
        # Define variables
        $ClusterNetworkName = "<ClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
        $IPResourceName = "<IPResourceName>" # the IP Address resource name
        $CloudServiceIP = "<X.X.X.X>" # Public Virtual IP (VIP) address of your cloud service
   
        Import-Module FailoverClusters
   
        # If you are using Windows Server 2012 or higher, use the Get-Cluster Resource command. If you are using Windows Server 2008 R2, use the cluster res command. Both commands are commented out. Choose the one applicable to your environment and remove the # at the beginning of the line to convert the comment to an executable line of code.
   
        # Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$CloudServiceIP";"ProbePort"="59999";"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"OverrideAddressMatch"=1;"EnableDhcp"=0}
        # cluster res $IPResourceName /priv enabledhcp=0 overrideaddressmatch=1 address=$CloudServiceIP probeport=59999  subnetmask=255.255.255.255
4. Eenmaal u de variabelen hebt ingesteld, opent u een Windows PowerShell-venster met verhoogde bevoegdheid, en vervolgens Kopieer het script in de teksteditor en plak in uw Azure PowerShell-sessie uit te voeren. Als de prompt nog steeds >>, typt u ENTER opnieuw te controleren of het script wordt gestart.
5. Herhaal deze procedure op elke virtuele machine. Dit script voor het configureren van de bron-IP-adres met het IP-adres van de cloudservice en stelt u andere parameters zoals de testpoort. Wanneer de bron-IP-adres online wordt gebracht, kan deze vervolgens reageren op de polling op de testpoort van het eindpunt taakverdeling eerder in deze zelfstudie hebt gemaakt.

## <a name="bring-the-listener-online"></a>De listener online brengen
[!INCLUDE [Bring-Listener-Online](../../../../includes/virtual-machines-ag-listener-bring-online.md)]

## <a name="follow-up-items"></a>Items worden opgevolgd
[!INCLUDE [Follow-up](../../../../includes/virtual-machines-ag-listener-follow-up.md)]

## <a name="test-the-availability-group-listener-within-the-same-vnet"></a>Testen van de beschikbaarheidsgroep-listener (binnen hetzelfde VNet)
[!INCLUDE [Test-Listener-Within-VNET](../../../../includes/virtual-machines-ag-listener-test.md)]

## <a name="test-the-availability-group-listener-over-the-internet"></a>Testen van de beschikbaarheidsgroep-listener (via het internet)
Voor toegang tot de listener van buiten het virtuele netwerk, moet u extern/openbare load balancing (in dit onderwerp beschreven) in plaats van een ILB, dit is alleen toegankelijk binnen hetzelfde VNet. In de verbindingsreeks, moet u de naam van de cloud-service opgeven. Bijvoorbeeld, als u een cloudservice met de naam van de had *mycloudservice*, de instructie sqlcmd zou als volgt zijn:

    sqlcmd -S "mycloudservice.cloudapp.net,<EndpointPort>" -d "<DatabaseName>" -U "<LoginId>" -P "<Password>"  -Q "select @@servername, db_name()" -l 15

In tegenstelling tot het vorige voorbeeld moet SQL-verificatie worden gebruikt, omdat de aanroeper kan geen van windows-verificatie via internet gebruikmaken. Zie voor meer informatie [AlwaysOn-beschikbaarheidsgroep in Azure VM: Client-scenario's voor connectiviteit](http://blogs.msdn.com/b/sqlcat/archive/2014/02/03/alwayson-availability-group-in-windows-azure-vm-client-connectivity-scenarios.aspx). Wanneer u SQL-verificatie gebruikt, zorg er dan voor dat u dezelfde aanmeldingsnaam maakt op beide replica's. Zie voor meer informatie over het oplossen van aanmeldingen met beschikbaarheidsgroepen [aanmeldingen toewijzen of gebruik opgenomen SQL database-gebruiker om te verbinden met andere replica's en toewijzen aan de beschikbaarheidsdatabases](http://blogs.msdn.com/b/alwaysonpro/archive/2014/02/19/how-to-map-logins-or-use-contained-sql-database-user-to-connect-to-other-replicas-and-map-to-availability-databases.aspx).

Als wordt altijd op replica's in verschillende subnetten, moeten clients opgeven **MultisubnetFailover = True** in de verbindingstekenreeks. Dit resulteert in parallelle verbindingspogingen met replica's in de verschillende subnetten. Houd er rekening mee dat dit scenario een regio-overschrijdende altijd op beschikbaarheidsgroep-implementatie bevat.

## <a name="next-steps"></a>Volgende stappen
[!INCLUDE [Listener-Next-Steps](../../../../includes/virtual-machines-ag-listener-next-steps.md)]

