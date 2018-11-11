---
title: Een externe Listener configureren voor Always On Availability Groups | Microsoft Docs
description: Deze zelfstudie leert u de stappen voor het maken van een altijd op beschikbaarheidsgroep-Listener in Azure die extern toegankelijk is via het openbare virtuele IP-adres van de gekoppelde cloud-service.
services: virtual-machines-windows
documentationcenter: na
author: MikeRayMSFT
manager: craigg
editor: ''
tags: azure-service-management
ms.assetid: a2453032-94ab-4775-b976-c74d24716728
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/31/2017
ms.author: mikeray
ms.openlocfilehash: 449df8e49eb63cb6e52cd4ec25dafc2bb0851347
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51241756"
---
# <a name="configure-an-external-listener-for-always-on-availability-groups-in-azure"></a>Een externe listener configureren voor Always On Availability Groups in Azure
> [!div class="op_single_selector"]
> * [Interne Listener](../classic/ps-sql-int-listener.md)
> * [Externe Listener](../classic/ps-sql-ext-listener.md)
> 
> 

In dit onderwerp ziet u hoe het configureren van een listener voor een AlwaysOn-beschikbaarheidsgroep die extern toegankelijk is op het internet. Dit wordt mogelijk gemaakt door te koppelen van de cloudservice **openbare virtuele IP (VIP)** -adres met de listener.

> [!IMPORTANT] 
> Azure heeft twee verschillende implementatiemodellen voor het maken van en werken met resources: [Resource Manager en klassieke](../../../azure-resource-manager/resource-manager-deployment-model.md). In dit artikel bevat informatie over met behulp van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken.

De beschikbaarheidsgroep kunnen bevatten replica's die alleen, on-premises zijn Azure alleen, of zowel on-premises en Azure omvatten voor hybride configuraties. Azure replica's kunnen zich binnen dezelfde regio of in meerdere regio's met behulp van meerdere virtuele netwerken (VNets). De onderstaande stappen wordt ervan uitgegaan dat u al hebt [geconfigureerd een beschikbaarheidsgroep](../classic/portal-sql-alwayson-availability-groups.md) , maar niet een listener hebt geconfigureerd.

## <a name="guidelines-and-limitations-for-external-listeners"></a>Richtlijnen en beperkingen voor externe listeners
Houd rekening met de volgende richtlijnen over de beschikbaarheidsgroeplistener in Azure wanneer u implementeert met behulp van het openbare VIP-adres van cloud-service:

* De beschikbaarheidsgroep-listener wordt ondersteund op Windows Server 2008 R2, Windows Server 2012 en Windows Server 2012 R2.
* De clienttoepassing moet zich bevinden op een andere cloudservice dan de versie die uw virtuele machines van de beschikbaarheidsgroep bevat. Azure biedt geen ondersteuning voor directe server geretourneerd met de client en server in dezelfde cloudservice.
* Standaard is de stappen in dit artikel laten zien hoe een listener voor het gebruik van het hulpprogramma voor het virtuele IP (VIP) van de cloud-serviceadres configureren. Het is echter mogelijk om te reserveren en meerdere VIP-adressen voor je cloudservice maken. Hiermee kunt u de stappen in dit artikel gebruiken om te maken van meerdere listeners die gekoppeld aan een andere VIP-adres zijn. Zie voor meer informatie over het maken van meerdere VIP-adressen [meerdere VIP's per cloudservice](../../../load-balancer/load-balancer-multivip.md).
* Als u een listener voor een hybride omgeving maakt, moet de on-premises netwerk verbinding met het openbare Internet naast de site-naar-site-VPN-verbinding met het Azure-netwerk hebben. Wanneer u in het Azure-subnet, is de beschikbaarheidsgroep-listener bereikbaar is alleen via het openbare IP-adres van de respectievelijke cloud-service.
* Het wordt niet ondersteund voor het maken van een externe listener in dezelfde cloudservice wanneer u hebt ook een interne listener met behulp van de interne Load Balancer (ILB).

## <a name="determine-the-accessibility-of-the-listener"></a>Bepalen van de toegankelijkheid van de listener
[!INCLUDE [ag-listener-accessibility](../../../../includes/virtual-machines-ag-listener-determine-accessibility.md)]

In dit artikel is gericht op het maken van een listener die gebruikmaakt van **externe taakverdeling**. Als u een listener die exclusief zijn voor uw virtuele netwerk is wilt, raadpleegt u de versie van dit artikel met stappen voor het instellen van een [listener met ILB](../classic/ps-sql-int-listener.md)

## <a name="create-load-balanced-vm-endpoints-with-direct-server-return"></a>Taakverdeling eindpunten voor virtuele machine met direct server return maken
Externe load balancing maakt gebruik van de virtuele het openbare virtuele IP-adres van de cloudservice die als host fungeert voor uw virtuele machines. U hoeft dus niet te maken of configureren van de load balancer in dit geval.

U moet een load balancing-eindpunt maken voor elke virtuele machine die als host fungeert voor de replica van een Azure. Als u replica's in meerdere regio's hebt, moet elke replica voor de regio die zich in dezelfde cloudservice in hetzelfde VNet. Replica's van beschikbaarheidsgroepen maken die meerdere Azure-regio's omvatten is vereist voor het configureren van meerdere VNets. Zie voor meer informatie over het configureren van andere VNet-connectiviteit [VNet-naar-VNet-connectiviteit configureren](../../../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md).

1. In de Azure-portal, gaat u naar elke virtuele machine die als host fungeert voor een replica en bekijk de details.
2. Klik op de **eindpunten** tabblad voor elk van de virtuele machines.
3. Controleer de **naam** en **openbare poort** van de listener-eindpunt u wilt gebruiken niet wordt al gebruikt. In het onderstaande voorbeeld is de naam 'MyEndpoint' en de poort "1433".
4. Op uw lokale client, download en installeer [de meest recente PowerShell-module](https://azure.microsoft.com/downloads/).
5. Start **Azure PowerShell**. Een nieuwe PowerShell-sessie wordt geopend met de Azure-administratieve modules geladen.
6. Voer **Get-AzurePublishSettingsFile**. Deze cmdlet wordt u naar een browser voor het downloaden van een publish settings-bestand naar een lokale map. U mogelijk gevraagd de referenties van uw aanmelding voor uw Azure-abonnement.
7. Voer de **importeren AzurePublishSettingsFile** opdracht met het pad van het publish settings-bestand dat u hebt gedownload:
   
        Import-AzurePublishSettingsFile -PublishSettingsFile <PublishSettingsFilePath>
   
    Zodra het publish settings-bestand is geïmporteerd, kunt u uw Azure-abonnement in de PowerShell-sessie kunt beheren.
    
1. Kopieer het onderstaande PowerShell-script in een teksteditor en stel de variabele waarden op basis van uw omgeving (standaardinstellingen zijn opgegeven voor bepaalde parameters). Houd er rekening mee dat als uw beschikbaarheidsgroep Azure-regio's omvat, u moet het script eenmaal uitgevoerd in elk datacenter voor de service in de cloud en de knooppunten die zich in dit datacenter bevinden.
   
        # Define variables
        $ServiceName = "<MyCloudService>" # the name of the cloud service that contains the availability group nodes
        $AGNodes = "<VM1>","<VM2>","<VM3>" # all availability group nodes containing replicas in the same cloud service, separated by commas
   
        # Configure a load balanced endpoint for each node in $AGNodes, with direct server return enabled
        ForEach ($node in $AGNodes)
        {
            Get-AzureVM -ServiceName $ServiceName -Name $node | Add-AzureEndpoint -Name "ListenerEndpoint" -Protocol "TCP" -PublicPort 1433 -LocalPort 1433 -LBSetName "ListenerEndpointLB" -ProbePort 59999 -ProbeProtocol "TCP" -DirectServerReturn $true | Update-AzureVM
        }

2. Nadat u de variabelen hebt ingesteld, kopieert u het script van de teksteditor in uw Azure PowerShell-sessie uit te voeren. Als u nog steeds weergegeven in de prompt >>, typt u ENTER opnieuw om te controleren of het script wordt uitgevoerd.

## <a name="verify-that-kb2854082-is-installed-if-necessary"></a>Controleer of KB2854082 is geïnstalleerd, indien nodig
[!INCLUDE [kb2854082](../../../../includes/virtual-machines-ag-listener-kb2854082.md)]

## <a name="open-the-firewall-ports-in-availability-group-nodes"></a>De firewall-poorten openen in de beschikbaarheid van groepsknooppunten
[!INCLUDE [firewall](../../../../includes/virtual-machines-ag-listener-open-firewall.md)]

## <a name="create-the-availability-group-listener"></a>De beschikbaarheidsgroep-listener maken

Maak de beschikbaarheidsgroep-listener in twee stappen. Eerst de clusterresource van client access point maken en configureren van afhankelijkheden. Ten tweede de clusterbronnen configureren met PowerShell.

### <a name="create-the-client-access-point-and-configure-the-cluster-dependencies"></a>De client access point maken en configureren van de cluster-afhankelijkheden
[!INCLUDE [firewall](../../../../includes/virtual-machines-ag-listener-create-listener.md)]

### <a name="configure-the-cluster-resources-in-powershell"></a>De clusterbronnen in PowerShell configureren
1. Voor de externe load balancing, moet u het openbare virtuele IP-adres van de cloudservice die uw replica's bevat. Meld u aan bij de Azure-portal. Navigeer naar de cloudservice met uw virtuele machine van de beschikbaarheidsgroep. Open de **Dashboard** weergeven.
2. Noteer het adres dat wordt weergegeven onder **openbare virtueel IP-adres (VIP)**. Als uw oplossing VNets omvat, kunt u deze stap herhalen voor elke cloudservice met een virtuele machine die als host fungeert voor een replica.
3. Op een van de virtuele machines, Kopieer het onderstaande PowerShell-script in een teksteditor en stel de variabelen in op de waarden die u eerder hebt genoteerd.
   
        # Define variables
        $ClusterNetworkName = "<ClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
        $IPResourceName = "<IPResourceName>" # the IP Address resource name
        $CloudServiceIP = "<X.X.X.X>" # Public Virtual IP (VIP) address of your cloud service
   
        Import-Module FailoverClusters
   
        # If you are using Windows Server 2012 or higher, use the Get-Cluster Resource command. If you are using Windows Server 2008 R2, use the cluster res command. Both commands are commented out. Choose the one applicable to your environment and remove the # at the beginning of the line to convert the comment to an executable line of code.
   
        # Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$CloudServiceIP";"ProbePort"="59999";"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"OverrideAddressMatch"=1;"EnableDhcp"=0}
        # cluster res $IPResourceName /priv enabledhcp=0 overrideaddressmatch=1 address=$CloudServiceIP probeport=59999  subnetmask=255.255.255.255
4. Eenmaal u de variabelen hebt ingesteld, open een verhoogde Windows PowerShell-venster en vervolgens het script van de teksteditor kopiëren en plakken in uw Azure PowerShell-sessie uit te voeren. Als u nog steeds weergegeven in de prompt >>, typt u ENTER opnieuw om te controleren of het script wordt uitgevoerd.
5. Herhaal deze procedure op elke virtuele machine. Met dit script wordt de bron-IP-adres geconfigureerd met het IP-adres van de cloudservice en andere parameters, zoals de testpoort ingesteld. Wanneer de bron-IP-adres online wordt gebracht, kan deze vervolgens reageren op de polling op de testpoort van het eindpunt met load balancing eerder in deze zelfstudie hebt gemaakt.

## <a name="bring-the-listener-online"></a>De listener online brengen
[!INCLUDE [Bring-Listener-Online](../../../../includes/virtual-machines-ag-listener-bring-online.md)]

## <a name="follow-up-items"></a>Opvolging items
[!INCLUDE [Follow-up](../../../../includes/virtual-machines-ag-listener-follow-up.md)]

## <a name="test-the-availability-group-listener-within-the-same-vnet"></a>Testen van de beschikbaarheidsgroep-listener (binnen hetzelfde VNet)
[!INCLUDE [Test-Listener-Within-VNET](../../../../includes/virtual-machines-ag-listener-test.md)]

## <a name="test-the-availability-group-listener-over-the-internet"></a>Testen van de beschikbaarheidsgroep-listener (via het internet)
Voor toegang tot de listener van buiten het virtuele netwerk, moet u extern/openbare taakverdeling (beschreven in dit onderwerp) in plaats van de ILB, dit is alleen toegankelijk binnen hetzelfde VNet. In de connection string geeft u de naam van de cloudservice. Bijvoorbeeld, als u een cloudservice met de naam van de had *mycloudservice*, de sqlcmd-instructie zijn als volgt:

    sqlcmd -S "mycloudservice.cloudapp.net,<EndpointPort>" -d "<DatabaseName>" -U "<LoginId>" -P "<Password>"  -Q "select @@servername, db_name()" -l 15

In tegenstelling tot het vorige voorbeeld, moet SQL-verificatie worden gebruikt, omdat de aanroeper niet kan windows-verificatie via internet gebruiken. Zie voor meer informatie, [AlwaysOn-beschikbaarheidsgroep in Azure VM: Client Connectiviteitsscenario's](https://blogs.msdn.com/b/sqlcat/archive/2014/02/03/alwayson-availability-group-in-windows-azure-vm-client-connectivity-scenarios.aspx). Wanneer u SQL-verificatie gebruikt, zorg ervoor dat u dezelfde aanmeldgegevens op beide replica's maakt. Zie voor meer informatie over het oplossen van aanmeldingen met Availability Groups [aanmeldingen toewijzen of gebruik van SQL database-gebruiker om te verbinden met andere replica's en toewijzen aan de beschikbaarheidsdatabases opgenomen](https://blogs.msdn.com/b/alwaysonpro/archive/2014/02/19/how-to-map-logins-or-use-contained-sql-database-user-to-connect-to-other-replicas-and-map-to-availability-databases.aspx).

Als wordt altijd op replica's in verschillende subnetten, clients moeten opgeven **MultisubnetFailover = True** in de verbindingsreeks. Dit resulteert in parallelle verbindingspogingen met replica's in de verschillende subnetten. Houd er rekening mee dat dit scenario geldt ook voor de implementatie van een regio-overschrijdende AlwaysOn-beschikbaarheidsgroep.

## <a name="next-steps"></a>Volgende stappen
[!INCLUDE [Listener-Next-Steps](../../../../includes/virtual-machines-ag-listener-next-steps.md)]

