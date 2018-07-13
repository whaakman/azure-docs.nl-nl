## <a name="use-the-azure-portal"></a>Azure Portal gebruiken
1. Selecteer de virtuele machine die u wilt implementeren, en selecteer vervolgens de *opnieuw implementeren* knop in de *instellingen* blade. Mogelijk moet u scrollen om te controleren de **ondersteuning en probleemoplossing** sectie met de knop 'Implementeren' zoals in het volgende voorbeeld:
   
    ![Azure VM-beheerblade](./media/virtual-machines-common-redeploy-to-new-node/vmoverview.png)
2. Om te bevestigen dat de bewerking, selecteer de *opnieuw implementeren* knop:
   
    ![Opnieuw implementeren van een VM-beheerblade](./media/virtual-machines-common-redeploy-to-new-node/redeployvm.png)
3. De **Status** van de virtuele machine wordt gewijzigd in *bijwerken* als de virtuele machine wordt voorbereid om te implementeren, zoals wordt weergegeven in het volgende voorbeeld:
   
    ![Virtuele machine bijwerken](./media/virtual-machines-common-redeploy-to-new-node/vmupdating.png)
4. De **Status** wordt gewijzigd in *vanaf* als de virtuele machine wordt opgestart op een nieuwe Azure-host, zoals wordt weergegeven in het volgende voorbeeld:
   
    ![VM gestart](./media/virtual-machines-common-redeploy-to-new-node/vmstarting.png)
5. Nadat de virtuele machine klaar is met het opstartproces, de **Status** retourneert vervolgens naar *met*, die wijzen op de virtuele machine is opnieuw is gedistribueerd:
   
    ![VM uitgevoerd](./media/virtual-machines-common-redeploy-to-new-node/vmrunning.png)

