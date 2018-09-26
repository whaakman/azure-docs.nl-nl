


## <a name="attach-an-empty-disk"></a>Een lege schijf koppelen
Bezig met koppelen van een lege schijf is een eenvoudige manier om toe te voegen een gegevensschijf, omdat Azure het VHD-bestand voor u gemaakt en in de storage-account opgeslagen.

1. Klik op **virtuele Machines (klassiek)**, en selecteer vervolgens de juiste virtuele machine.

2. Klik in het menu instellingen op **schijven**.

   ![Een nieuwe lege schijf koppelen](./media/howto-attach-disk-windows-linux/menudisksattachnew.png)

3. Klik op de opdrachtbalk op **koppelen nieuwe**.  
    De **nieuwe schijf koppelen** in het dialoogvenster wordt weergegeven.

    ![Een nieuwe schijf koppelen](./media/howto-attach-disk-windows-linux/newdiskdetail.png)

    Vul in de volgende informatie:
    - In **bestandsnaam**, accepteer de standaardnaam of typ een andere naam voor het VHD-bestand. De gegevensschijf maakt gebruik van een automatisch gegenereerde naam, zelfs als u een andere naam voor het VHD-bestand.
    - Selecteer de **Type** van de gegevensschijf. Alle virtuele machines ondersteunen standaardschijven. Groot aantal virtuele machines ook ondersteuning voor premium-schijven.
    - Selecteer de **schijfgrootte (GB)** van de gegevensschijf.
    - Voor **Hostcaching**of Kies geen alleen-lezen.
    - Klik op OK om te voltooien.

4. Nadat de gegevensschijf is gemaakt en gekoppeld, wordt deze weergegeven in de sectie schijven van de virtuele machine.

   ![Nieuwe en lege gegevensschijf is die is gekoppeld](./media/howto-attach-disk-windows-linux/newdiskemptysuccessful.png)

> [!NOTE]
> Nadat u een gegevensschijf hebt toegevoegd, moet u zich aanmeldt bij de virtuele machine en de schijf initialiseren zodat deze kan worden gebruikt.

## <a name="how-to-attach-an-existing-disk"></a>Hoe: een bestaande schijf koppelen
Als u een bestaande schijf wilt koppelen, dient u een .vhd beschikbaar te hebben in een opslagaccount. Gebruik de [Add-AzureVhd](https://msdn.microsoft.com/library/azure/dn495173.aspx) cmdlet het VHD-bestand uploaden naar het opslagaccount. Nadat u hebt gemaakt en het VHD-bestand wordt geüpload, kunt u deze koppelen aan een virtuele machine.

1. Klik op **virtuele Machines (klassiek)**, en selecteer vervolgens de juiste virtuele machine.

2. Klik in het menu instellingen op **schijven**.

3. Klik op de opdrachtbalk op **koppelen aan bestaande**.

    ![Een gegevensschijf koppelen](./media/howto-attach-disk-windows-linux/menudisksattachexisting.png)

4. Klik op **locatie**. De beschikbare opslag-accounts worden weergegeven. Selecteer vervolgens een geschikte storage-account in de lijst.

    ![Schijf storage-account opgeven](./media/howto-attach-disk-windows-linux/existdiskstorageaccounts.png)

5. Een **opslagaccount** bevat een of meer containers die harde schijven (VHD's) bevatten. Selecteer de juiste container in de lijst.

    ![Geef de container van virtuele machines-windows](./media/howto-attach-disk-windows-linux/existdiskcontainers.png)

6. De **VHD's** deelvenster geeft een lijst van de schijven die zijn ondergebracht in de container. Klik op een van de schijven en klik vervolgens op selecteren.

    ![Schijfinstallatiekopie bieden voor virtuele machines-windows](./media/howto-attach-disk-windows-linux/existdiskvhds.png)

7. De **bestaande schijf koppelen** deelvenster wordt weergegeven, met de locatie met de storage-account, container en geselecteerde harde schijf (vhd) om toe te voegen aan de virtuele machine.

  Stel **Hostcaching** op none of lezen alleen, klik vervolgens op OK.

    ![Gegevensschijf die is gekoppeld](./media/howto-attach-disk-windows-linux/exisitingdisksuccessful.png)
