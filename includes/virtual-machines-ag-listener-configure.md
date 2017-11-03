De beschikbaarheidsgroep-listener is een IP-adres en het netwerk de naam die de SQL Server-beschikbaarheidsgroep luistert op. Voor het maken van de beschikbaarheidsgroep-listener, het volgende doen:

1. <a name="getnet"></a>De naam van de clusterbron van het netwerk worden opgehaald.

    a. Met RDP verbinding maken met de virtuele machine van Azure die als host fungeert voor de primaire replica. 

    b. Open Failoverclusterbeheer.

    c. Selecteer de **netwerken** knooppunt en noteer de naam van het cluster-netwerk. Gebruik deze naam in de `$ClusterNetworkName` variabele in het PowerShell-script. In de volgende afbeelding is het de clusternetwerknaam **Cluster netwerk 1**:

   ![De clusternetwerknaam](./media/virtual-machines-ag-listener-configure/90-clusternetworkname.png)

2. <a name="addcap"></a>Toevoegen van de client access point.  
    De client access point is de naam van het netwerk waarmee toepassingen verbinding met de databases in een beschikbaarheidsgroep. Maken van de client access point in Failoverclusterbeheer.

    a. De clusternaam uitvouwen en klik vervolgens op **rollen**.

    b. In de **rollen** deelvenster met de rechtermuisknop op de naam van de beschikbaarheidsgroep en selecteer vervolgens **Resource toevoegen** > **Client Access Point**.

   ![Client Access Point](./media/virtual-machines-ag-listener-configure/92-addclientaccesspoint.png)

    c. In de **naam** vak, maakt u een naam op voor deze nieuwe listener. 
   De naam voor de nieuwe listener is de naam van het netwerk dat toepassingen gebruiken om verbinding te databases in de SQL Server-beschikbaarheidsgroep.
   
    d. Voor het voltooien van de listener te maken, klikt u op **volgende** tweemaal, en klik vervolgens op **voltooien**. Worden niet weergegeven in de listener of de bron online op dit moment.

3. <a name="congroup"></a>Configureer de IP-resource voor de beschikbaarheidsgroep.

    a. Klik op de **Resources** tabblad uit en vouw vervolgens de client access point-u hebt gemaakt.  
    De client access point is offline.

   ![Client Access Point](./media/virtual-machines-ag-listener-configure/94-newclientaccesspoint.png) 

    b. Met de rechtermuisknop op de IP-bron en klik vervolgens op Eigenschappen. Noteer de naam van het IP-adres en deze gebruiken in de `$IPResourceName` variabele in het PowerShell-script.

    c. Onder **IP-adres**, klikt u op **statisch IP-adres**. Het IP-adres instellen als hetzelfde adres dat u hebt gebruikt toen u het adres van de load balancer op de Azure-portal instellen.

   ![IP-Resource](./media/virtual-machines-ag-listener-configure/96-ipresource.png) 

    <!-----------------------I don't see this option on server 2016
    1. Disable NetBIOS for this address and click **OK**. Repeat this step for each IP resource if your solution spans multiple Azure VNets. 
    ------------------------->

4. <a name = "dependencyGroup"></a>Maak de SQL Server-beschikbaarheidsgroepresource afhankelijk van de client access point.

    a. In Failoverclusterbeheer klikt u op **rollen**, en klik vervolgens op de beschikbaarheidsgroep.

    b. Op de **Resources** tabblad onder **andere Resources**, met de rechtermuisknop op de beschikbaarheid van resourcegroep en klik vervolgens op **eigenschappen**. 

    c. Voeg de naam van de bron van client access point (de listener) op het tabblad afhankelijkheden.

   ![IP-Resource](./media/virtual-machines-ag-listener-configure/97-propertiesdependencies.png) 

    d. Klik op **OK**.

5. <a name="listname"></a>Controleer de afhankelijk van het IP-adres van client access point resource.

    a. In Failoverclusterbeheer klikt u op **rollen**, en klik vervolgens op de beschikbaarheidsgroep. 

    b. Op de **Resources** tabblad, met de rechtermuisknop op de client access point-resource onder **servernaam**, en klik vervolgens op **eigenschappen**. 

   ![IP-Resource](./media/virtual-machines-ag-listener-configure/98-dependencies.png) 

    c. Klik op de **afhankelijkheden** tabblad. Controleer of het IP-adres een afhankelijkheid. Als dit niet het geval is, stelt u een afhankelijkheid op het IP-adres. Controleer of de IP-adressen hebt of niet als er meerdere resources die worden vermeld, en afhankelijkheden. Klik op **OK**. 

   ![IP-Resource](./media/virtual-machines-ag-listener-configure/98-propertiesdependencies.png) 

    d. Met de rechtermuisknop op de naam van de listener en klik vervolgens op **Online brengen**. 

    >[!TIP]
    >U kunt controleren of de afhankelijkheden juist zijn geconfigureerd. In Failoverclusterbeheer, gaat u aan rollen, met de rechtermuisknop op de beschikbaarheidsgroep, klikt u op **meer acties**, en klik vervolgens op **Afhankelijkheidsrapport weergeven**. De afhankelijkheden juist zijn geconfigureerd, de beschikbaarheidsgroep is afhankelijk van de naam van het netwerk als de netwerknaam is afhankelijk van het IP-adres. 


6. <a name="setparam"></a>Stel de clusterparameters in PowerShell.
    
    a. Kopieer de volgende PowerShell-script naar een van uw SQL Server-exemplaren. De variabelen voor uw omgeving bijwerken.     
    
    ```PowerShell
    $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
    $IPResourceName = "<IPResourceName>" # the IP Address resource name
    $ILBIP = “<n.n.n.n>” # the IP Address of the Internal Load Balancer (ILB). This is the static IP address for the load balancer you configured in the Azure portal.
    [int]$ProbePort = <nnnnn>
    
    Import-Module FailoverClusters
    
    Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
    ```

    b. Stel de clusterparameters door het PowerShell-script wordt uitgevoerd op één van de clusterknooppunten.  

    > [!NOTE]
    > Als uw SQL Server-exemplaren in verschillende regio's zijn, moet u het PowerShell-script twee keer uitgevoerd. De eerste keer gebruikt de `$ILBIP` en `$ProbePort` van het eerste gebied. De tweede keer gebruikt de `$ILBIP` en `$ProbePort` van de tweede regio. De naam van het cluster-netwerk en de naam van de cluster-IP-resource zijn hetzelfde. 
