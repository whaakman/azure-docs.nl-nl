


## <a name="attach-an-empty-disk"></a>Een lege schijf koppelen
Koppelen van een lege schijf is een eenvoudige manier om een gegevensschijf niet toevoegen omdat Azure het VHD-bestand voor u gemaakt en in het opslagaccount opgeslagen.

1. Klik op **virtuele Machines (klassiek)**, en selecteer vervolgens de juiste virtuele machine.

2. Klik in het menu instellingen **schijven**.

   ![Een nieuwe lege schijf koppelen](./media/howto-attach-disk-windows-linux/menudisksattachnew.png)

3. Klik op de opdrachtbalk op **Attach nieuwe**.  
    De **nieuwe schijf koppelen** dialoogvenster wordt weergegeven.

    ![Een nieuwe schijf koppelen](./media/howto-attach-disk-windows-linux/newdiskdetail.png)

    Vul in de volgende informatie:
    - In **bestandsnaam**, accepteer de standaardnaam of typ een andere naam voor het VHD-bestand. De gegevensschijf gebruikt een automatisch gegenereerde naam, zelfs als u een andere naam voor het VHD-bestand typen.
    - Selecteer de **Type** van de gegevensschijf. Alle virtuele machines ondersteunen standaardschijven. Premium-schijven bieden ook ondersteuning voor veel virtuele machines.
    - Selecteer de **grootte (GB)** van de gegevensschijf.
    - Voor **Hostcaching**, kiest u geen of alleen-lezen.
    - Klik op OK om te voltooien.

4. Nadat de gegevensschijf is gemaakt en gekoppeld, wordt deze vermeld in de sectie schijven van de virtuele machine.

   ![Nieuwe en lege gegevensschijf gekoppeld](./media/howto-attach-disk-windows-linux/newdiskemptysuccessful.png)

> [!NOTE]
> Nadat u een gegevensschijf toevoegen, moet u zich aanmeldt bij de virtuele machine en de schijf initialiseren zodat deze kan worden gebruikt.

## <a name="how-to-attach-an-existing-disk"></a>Procedure: een bestaande schijf koppelen
Als u een bestaande schijf wilt koppelen, dient u een .vhd beschikbaar te hebben in een opslagaccount. Gebruik de [Add-AzureVhd](https://msdn.microsoft.com/library/azure/dn495173.aspx) cmdlet het VHD-bestand uploaden naar het opslagaccount. Nadat u hebt gemaakt en het VHD-bestand wordt geüpload, kunt u het koppelen aan een VM.

1. Klik op **virtuele Machines (klassiek)**, en selecteer vervolgens de juiste virtuele machine.

2. Klik in het menu instellingen **schijven**.

3. Klik op de opdrachtbalk op **Attach bestaande**.

    ![Een gegevensschijf koppelen](./media/howto-attach-disk-windows-linux/menudisksattachexisting.png)

4. Klik op **locatie**. De beschikbare opslagaccounts weergegeven. Selecteer vervolgens een juiste storage-account in de lijst.

    ![Schijf storage-account opgeven](./media/howto-attach-disk-windows-linux/existdiskstorageaccounts.png)

5. Een **opslagaccount** bevat een of meer containers die harde schijven (VHD's) bevatten. Selecteer de juiste container in de lijst.

    ![Container van virtuele machines-windows bieden](./media/howto-attach-disk-windows-linux/existdiskcontainers.png)

6. De **VHD's** deelvenster worden de schijven die zijn ondergebracht in de container. Klik op een van de schijven en klik op selecteren.

    ![Schijfimage bieden voor virtuele machines-windows](./media/howto-attach-disk-windows-linux/existdiskvhds.png)

7. De **bestaande schijf koppelen** Configuratiescherm opnieuw wordt weergegeven met de locatie met de storage-account, de container en de geselecteerde harde schijf (vhd) toevoegen aan de virtuele machine.

  Stel **Hostcaching** op none of lezen alleen, klik vervolgens op OK.

    ![Gegevensschijf gekoppeld](./media/howto-attach-disk-windows-linux/exisitingdisksuccessful.png)
