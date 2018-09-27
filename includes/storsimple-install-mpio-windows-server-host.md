#### <a name="to-install-mpio-on-the-host"></a>Het installeren van MPIO op de host
1. Open Serverbeheer op uw Windows Server-host. Serverbeheer wordt standaard gestart wanneer een lid van de groep Administrators zich aanmeldt bij een computer waarop Windows Server 2012 R2 of Windows Server 2012 wordt uitgevoerd. Als de Server Manager nog niet is geopend, klikt u op **Start > Serverbeheer**.
   
    ![Serverbeheer](./media/storsimple-install-mpio-windows-server/IC740997.png)
2. Klik op **Serverbeheer > Dashboard > functies en onderdelen toevoegen**. Hiermee start u de **functies en onderdelen toevoegen** wizard.
   
    ![Toevoegen van de Wizard functies en onderdelen 1](./media/storsimple-install-mpio-windows-server/IC740998.png)
3. In de **functies en onderdelen toevoegen** wizard de volgende handelingen uit:
   
   * Op de **voordat u begint met** pagina, klikt u op **volgende**.
   * Op de **installatietype selecteren** pagina, accepteer de standaardinstelling van **op basis van rollen of op basis van functie** installatie. Klik op **Volgende**.
     
       ![Wizard functies en onderdelen 2 toevoegen](./media/storsimple-install-mpio-windows-server/IC740999.png)
   * Op de **Selecteer doelserver** pagina, kies **Selecteer een server uit de servergroep**. Uw host-server moet automatisch worden gedetecteerd. Klik op **Volgende**.
   * Op de **Serverfuncties selecteren** pagina, klikt u op **volgende**.
   * Op de **functies selecteren** pagina, selecteert u **Multipath i/o**, en klikt u op **volgende**.
     
       ![Wizard functies en onderdelen 5 toevoegen](./media/storsimple-install-mpio-windows-server/IC741000.png)
   * Op de **installatie-instellingen bevestigen** pagina, Controleer of de selectie en selecteer vervolgens **de doelserver automatisch opnieuw opstarten indien nodig**, zoals hieronder weergegeven. Klik op **Install**.
     
       ![Wizard functies en onderdelen 8 toevoegen](./media/storsimple-install-mpio-windows-server/IC741001.png)
   * U krijgt bericht wanneer de installatie voltooid is. Klik op **Sluiten** om de wizard te sluiten.
     
       ![Wizard functies en onderdelen 9 toevoegen](./media/storsimple-install-mpio-windows-server/IC741002.png)

