
1. In de **hybride verbindingen** blade, klikt u op de hybride verbinding die u zojuist hebt gemaakt en klik op **Listener Setup**.
   
    ![Klik op Listener Setup](./media/app-service-hybrid-connections-manager-install/D04ClickListenerSetup.png)
2. De **hybride verbindingseigenschappen** blade wordt geopend. Onder **On-premises hybride Verbindingsbeheer**, kies **handmatig downloaden en configureren**, sla het gedownloade pakket van HybridConnectionManager.msi en kopieer de verbindingsreeks voor de gateway.
   
    ![Klik hier om te installeren](./media/app-service-hybrid-connections-manager-install/D05ClickToInstallHCM.png)
3. Typ de volgende opdracht om het installatieprogramma starten vanaf een beheerdersopdrachtprompt:
   
        start HybridConnectionManager.msi
4. Nadat het installatieprogramma wordt uitgevoerd, klikt u op **niet nu**, bladert u naar de map %ProgramFiles%\Microsoft\HybridConnectionManager, HCMConfigWizard.exe uitvoeren en klik op **Ja** in de **gebruikersaccount Besturingselement** dialoogvenster.
5. Plak de verbindingsreeks voor hybride die u eerder hebt gekopieerd en klik op **OK**. 
   
    ![Installeren](./media/app-service-hybrid-connections-manager-install/D08aHCMInstallManual.png)
6. Wanneer de installatie is voltooid, klikt u op **sluiten**.
   
    ![Klik op sluiten](./media/app-service-hybrid-connections-manager-install/D09HCMInstallComplete.png)
   
    Op de **hybride verbindingen** blade de **Status** kolom ziet u nu **verbonden**. 
   
    ![Status verbonden](./media/app-service-hybrid-connections-manager-install/D10HCStatusConnected.png)

