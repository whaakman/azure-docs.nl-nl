## <a name="use-the-azure-portal"></a>Azure Portal gebruiken
1. Selecteer de virtuele machine die u wilt implementeren, en selecteer vervolgens de *implementeren* knop in de *instellingen* blade. Mogelijk moet u scrollen om te controleren de **ondersteuning en probleemoplossing** sectie met de knop 'opnieuw implementeren, zoals in het volgende voorbeeld:
   
    ![Azure VM-blade](./media/virtual-machines-common-redeploy-to-new-node/vmoverview.png)
2. Om te controleren van de bewerking, selecteer de *implementeren* knop:
   
    ![Een VM-blade implementeren](./media/virtual-machines-common-redeploy-to-new-node/redeployvm.png)
3. De **Status** van de virtuele machine wordt gewijzigd in *Updating* als de virtuele machine wordt voorbereid om te implementeren, zoals wordt weergegeven in het volgende voorbeeld:
   
    ![Virtuele machine bijwerken](./media/virtual-machines-common-redeploy-to-new-node/vmupdating.png)
4. De **Status** wordt gewijzigd in *starten* als de virtuele machine wordt gestart op een nieuwe Azure-host, zoals wordt weergegeven in het volgende voorbeeld:
   
    ![VM starten](./media/virtual-machines-common-redeploy-to-new-node/vmstarting.png)
5. Nadat de virtuele machine is het opstartproces voltooid de **Status** waarna u terugkeert naar *met*, die wijzen op de virtuele machine is opnieuw is gedistribueerd:
   
    ![Virtuele machine die wordt uitgevoerd](./media/virtual-machines-common-redeploy-to-new-node/vmrunning.png)

