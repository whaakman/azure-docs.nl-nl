---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 276ddf0a70fa450451cd3ddc78c7610c4ab1edc1
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58494922"
---
De beschikbaarheidsgroep-listener is een IP-adres en de naam die de SQL Server-beschikbaarheidsgroep luistert op. Voor het maken van de beschikbaarheidsgroep-listener, het volgende doen:

1. <a name="getnet"></a>Haal de naam van de cluster-resource.

    a. Extern bureaublad gebruiken om verbinding met de Azure-machine die als host fungeert voor de primaire replica te maken. 

    b. Open Failoverclusterbeheer.

    c. Selecteer de **netwerken** knooppunt, en noteer de naam van het cluster-netwerk. Gebruik deze naam in de `$ClusterNetworkName` variabele in het PowerShell-script. In de volgende afbeelding de clusternetwerknaam is **Cluster netwerk 1**:

   ![Naam van clusternetwerk](./media/virtual-machines-ag-listener-configure/90-clusternetworkname.png)

1. <a name="addcap"></a>Voeg de client access point.  
    De client access point is de naam van het netwerk dat toepassingen gebruiken om verbinding maken met de databases in een beschikbaarheidsgroep. Maak de client access point in Failoverclusterbeheer.

    a. Vouw de naam van het cluster en klik vervolgens op **rollen**.

    b. In de **rollen** in het deelvenster met de rechtermuisknop op de naam van de beschikbaarheidsgroep en selecteer vervolgens **Resource toevoegen** > **Client Access Point**.

   ![Client Access Point](./media/virtual-machines-ag-listener-configure/92-addclientaccesspoint.png)

    c. In de **naam** vak, maakt u een naam op voor deze nieuwe listener. 
   De naam voor de nieuwe listener is de naam van het netwerk dat toepassingen gebruiken om verbinding maken met databases in de SQL Server-beschikbaarheidsgroep.

    d. Klik op om het maken van de listener **volgende** twee keer, en klik vervolgens op **voltooien**. Worden niet weergegeven in de listener of resource online op dit moment.

1. De beschikbaarheid van groep-clusterrol offline halen. In **Failoverclusterbeheer** onder **rollen**, met de rechtermuisknop op de rol en selecteert u **functie stoppen**.

1. <a name="congroup"></a>Configureer de IP-resource voor de beschikbaarheidsgroep.

    a. Klik op de **Resources** tabblad uit en vouw vervolgens het clienttoegangspunt dat u hebt gemaakt.  
    De client access point is offline.

   ![Client Access Point](./media/virtual-machines-ag-listener-configure/94-newclientaccesspoint.png) 

    b. Met de rechtermuisknop op de IP-resource en klik vervolgens op Eigenschappen. Noteer de naam van het IP-adres en gebruik deze in de `$IPResourceName` variabele in het PowerShell-script.

    c. Onder **IP-adres**, klikt u op **statisch IP-adres**. Het IP-adres instellen als hetzelfde adres dat u hebt gebruikt toen u het adres van de load balancer in Azure portal instellen.

   ![IP-Resource](./media/virtual-machines-ag-listener-configure/96-ipresource.png) 

    <!-----------------------I don't see this option on server 2016
    1. Disable NetBIOS for this address and click **OK**. Repeat this step for each IP resource if your solution spans multiple Azure VNets. 
    ------------------------->

1. <a name = "dependencyGroup"></a>Controleer de SQL Server-beschikbaarheidsgroepresource afhankelijk van de client access point.

    a. In Failoverclusterbeheer klikt u op **rollen**, en klik vervolgens op de beschikbaarheidsgroep.

    b. Op de **Resources** tabblad onder **andere Resources**, met de rechtermuisknop op de resourcegroep van de beschikbaarheid en klik vervolgens op **eigenschappen**. 

    c. Op het tabblad afhankelijkheden, moet u de naam van de client access point (listener) resource toevoegen.

   ![IP-Resource](./media/virtual-machines-ag-listener-configure/97-propertiesdependencies.png) 

    d. Klik op **OK**.

1. <a name="listname"></a>Controleer de afhankelijk van het IP-adres van client access point resource.

    a. In Failoverclusterbeheer klikt u op **rollen**, en klik vervolgens op de beschikbaarheidsgroep. 

    b. Op de **Resources** tabblad, met de rechtermuisknop op de client access point resource onder **servernaam**, en klik vervolgens op **eigenschappen**. 

   ![IP-Resource](./media/virtual-machines-ag-listener-configure/98-dependencies.png) 

    c. Klik op de **afhankelijkheden** tabblad. Controleer of het IP-adres is een afhankelijkheid. Als dat niet het geval is, stelt u een afhankelijkheid op het IP-adres. Controleer of de IP-adressen hebt of niet als er meerdere resources die worden vermeld, en afhankelijkheden. Klik op **OK**. 

   ![IP-Resource](./media/virtual-machines-ag-listener-configure/98-propertiesdependencies.png) 

    >[!TIP]
    >U kunt controleren of de afhankelijkheden juist zijn geconfigureerd. In Failoverclusterbeheer, gaat u aan rollen, met de rechtermuisknop op de beschikbaarheidsgroep, klikt u op **meer acties**, en klik vervolgens op **Afhankelijkheidsrapport weergeven**. Wanneer de afhankelijkheden juist zijn geconfigureerd, de beschikbaarheidsgroep is afhankelijk van de netwerknaam en de naam van het netwerk is afhankelijk van het IP-adres. 


1. <a name="setparam"></a>Stel de clusterparameters in PowerShell.

   a. Kopieer de volgende PowerShell-script naar een van uw SQL Server-exemplaren. Werk de variabelen voor uw omgeving.

   - `$ListenerILBIP` de IP-adres dat u hebt gemaakt op de Azure load balancer voor de beschikbaarheidsgroep-listener is.
    
   - `$ListenerProbePort` is de poort die u hebt geconfigureerd op de Azure load balancer voor de beschikbaarheidsgroep-listener.

   ```powershell
   $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
   $IPResourceName = "<IPResourceName>" # the IP Address resource name
   $ListenerILBIP = "<n.n.n.n>" # the IP Address of the Internal Load Balancer (ILB). This is the static IP address for the load balancer you configured in the Azure portal.
   [int]$ListenerProbePort = <nnnnn>
  
   Import-Module FailoverClusters

   Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ListenerILBIP";"ProbePort"=$ListenerProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
   ```

   b. De clusterparameters instellen door het PowerShell-script uitgevoerd op een van de clusterknooppunten.  

   > [!NOTE]
   > Als uw SQL Server-exemplaren in verschillende regio's zijn, moet u het PowerShell-script worden twee keer uitgevoerd. De eerste keer gebruikt de `$ListenerILBIP` en `$ListenerProbePort` van de eerste regio. De tweede keer gebruikt de `$ListenerILBIP` en `$ListenerProbePort` vanaf de tweede regio. De naam van het cluster-netwerk en de naam van de cluster-IP-resource zijn ook verschillende voor elke regio.

1. De beschikbaarheid van groep clusterrol online brengen. In **Failoverclusterbeheer** onder **rollen**, klik met de rechtermuisknop op de rol en selecteer **rol starten**.

Herhaal de stappen hierboven om in te stellen van de clusterparameters voor het IP-adres van het WSFC-cluster.

1. De naam van de IP-adres van het WSFC-Cluster-IP-adres ophalen. In **Failoverclusterbeheer** onder **Clusterkernresources**, zoek **servernaam**.

1. Met de rechtermuisknop op **IP-adres**, en selecteer **eigenschappen**.

1. Kopieer de **naam** van het IP-adres. Kan het zijn `Cluster IP Address`. 

1. <a name="setwsfcparam"></a>Stel de clusterparameters in PowerShell.
  
   a. Kopieer de volgende PowerShell-script naar een van uw SQL Server-exemplaren. Werk de variabelen voor uw omgeving.

   - `$ClusterCoreIP` is het IP-adres dat u hebt gemaakt in de Azure load balancer voor de clusterbron van de WSFC-core. Dit wijkt af van het IP-adres voor de beschikbaarheidsgroep-listener.

   - `$ClusterProbePort` is de poort die u hebt geconfigureerd in de Azure load balancer voor de WSFC-statustest. Dit wijkt af van de test voor de beschikbaarheidsgroep-listener.

   ```powershell
   $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
   $IPResourceName = "<ClusterIPResourceName>" # the IP Address resource name
   $ClusterCoreIP = "<n.n.n.n>" # the IP Address of the Cluster IP resource. This is the static IP address for the load balancer you configured in the Azure portal.
   [int]$ClusterProbePort = <nnnnn> # The probe port from the WSFCEndPointprobe in the Azure portal. This port must be different from the probe port for the availability group listener probe port.
  
   Import-Module FailoverClusters
  
   Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ClusterCoreIP";"ProbePort"=$ClusterProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
   ```

   b. De clusterparameters instellen door het PowerShell-script uitgevoerd op een van de clusterknooppunten.  

>[!WARNING]
>De health testpoort beschikbaarheidsgroeplistener moet afwijken van de cluster core IP-adres health testpoort. In deze voorbeelden wordt de listener-poort is 59999 en het IP-adres van de cluster-core 58888 is. Beide poorten vereist een inkomende firewall-regel voor toestaan.