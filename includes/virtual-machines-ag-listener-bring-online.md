1. Vouw in Failoverclusterbeheer **rollen**, en selecteer vervolgens de beschikbaarheidsgroep.  

2. Op de **Resources** tabblad en klik vervolgens op met de rechtermuisknop op de naam van de listener **eigenschappen**.

3. Klik op de **afhankelijkheden** tabblad. Controleer of de IP-adressen hebt of niet als meerdere resources worden weergegeven, en afhankelijkheden.  

4. Klik op **OK**.

5. Met de rechtermuisknop op de naam van de listener en klik vervolgens op **Online brengen**.

6. Nadat de listener is online op het **Resources** tabblad en klik vervolgens op met de rechtermuisknop op de beschikbaarheidsgroep **eigenschappen**.
   
    ![De beschikbaarheidsgroepresource configureren](./media/virtual-machines-sql-server-configure-alwayson-availability-group-listener/IC678772.gif)

7. Een afhankelijkheid voor de listener naambron (niet de naam IP-adres resources) maken en klik vervolgens op **OK**.
   
    ![Afhankelijkheid van de naam van de listener toevoegen](./media/virtual-machines-sql-server-configure-alwayson-availability-group-listener/IC678773.gif)

8. Start SQL Server Management Studio en maak verbinding met de primaire replica.

9. Ga naar **AlwaysOn hoge beschikbaarheid** > **beschikbaarheidsgroepen** > **\<AvailabilityGroupName\>**   >  **Beschikbaarheidsgroep-Listeners**.  
    De naam van de listener die u hebt gemaakt in Failoverclusterbeheer moet worden weergegeven.

10. Met de rechtermuisknop op de naam van de listener en klik vervolgens op **eigenschappen**.

11. In de **poort** vak het poortnummer opgeven voor de beschikbaarheidsgroep-listener met behulp van de $EndpointPort die u eerder hebt gebruikt (in deze zelfstudie is 1433 de standaardinstelling), en klik vervolgens op **OK**.

