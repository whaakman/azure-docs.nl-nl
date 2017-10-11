In deze stap moet u handmatig de beschikbaarheidsgroeplistener in Failoverclusterbeheer en SQL Server Management Studio maken.

1. Open Failoverclusterbeheer vanuit het knooppunt dat als host fungeert voor de primaire replica.

2. Selecteer de **netwerken** knooppunt en de clusternetwerknaam opmerking. Deze naam wordt gebruikt in de variabele $ClusterNetworkName in het PowerShell-script.

3. De clusternaam uitvouwen en klik vervolgens op **rollen**.

4. In de **rollen** deelvenster met de rechtermuisknop op de naam van de beschikbaarheidsgroep en selecteer vervolgens **Resource toevoegen** > **Client Access Point**.
   
    ![Toevoegen van Client Access Point voor de beschikbaarheidsgroep](./media/virtual-machines-sql-server-configure-alwayson-availability-group-listener/IC678769.gif)

5. In de **naam** vak, een naam op voor deze nieuwe listener te maken, klikt u op **volgende** tweemaal, en klik vervolgens op **voltooien**.  
    Worden niet weergegeven in de listener of de bron online op dit moment.

6. Klik op de **Resources** tabblad uit en vouw vervolgens het clienttoegangspunt dat u zojuist hebt gemaakt. 
    De bron van de IP-adres voor elke clusternetwerk in het cluster wordt weergegeven. Als dit een Azure-only-oplossing, wordt slechts één IP-adres resource wordt weergegeven.

7. Voer een van de volgende bewerkingen uit:
   
   * Een hybride oplossing configureren:
     
        a. Met de rechtermuisknop op de bron van IP-adres dat overeenkomt met het lokale subnet en selecteer vervolgens **eigenschappen**. Noteer de naam IP-adres en de netwerknaam.
   
        b. Selecteer **statisch IP-adres**, een niet-gebruikte IP-adres toewijzen en klik vervolgens op **OK**.
 
   * Een Azure-only-oplossing configureren:

        a. Met de rechtermuisknop op de bron van IP-adres dat overeenkomt met uw Azure-subnet en selecteer vervolgens **eigenschappen**.
       
       > [!NOTE]
       > Als de listener later mislukt on line vanwege een conflicterende IP-adres geselecteerd door DHCP, kunt u een geldig statisch IP-adres configureren in dit eigenschappenvenster.
       > 
       > 

       b. In dezelfde **IP-adres** wijziging van het eigenschappenvenster de **naam IP-adres**.  
        Deze naam wordt gebruikt in de variabele $IPResourceName van het PowerShell-script. Als uw oplossing meerdere virtuele Azure-netwerken omvat, Herhaal deze stap voor elke IP-resource.

