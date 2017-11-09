Als op uw virtuele machine (VM) in Azure een opstart- of schijffout optreedt, moet u mogelijk de stappen voor probleemoplossing uitvoeren op de virtuele harde schijf zelf. Een veelvoorkomend voorbeeld is een mislukte toepassingsupdate die verhindert dat de virtuele machine opstart. In dit artikel wordt beschreven hoe u Azure Portal gebruikt om verbinding maken met de virtuele harde schijf van een andere virtuele machine om eventuele fouten te corrigeren en de oorspronkelijke virtuele machine opnieuw te maken.


## <a name="recovery-process-overview"></a>Overzicht van het herstelproces
Het probleemoplossingsproces is als volgt:

1. Verwijder de virtuele machine waarop problemen optreden, maar behoud de virtuele harde schijven.
2. Koppel de virtuele harde schijf aan een andere virtuele machine om de problemen op te lossen.
3. Maak verbinding met de VM voor probleemoplossing. Bewerk bestanden of voer hulpprogramma's uit om fouten te herstellen op de oorspronkelijke virtuele harde schijf.
4. Koppel de virtuele harde schijf van de VM voor probleemoplossing los.
5. Maak een virtuele machine met behulp van de oorspronkelijke virtuele harde schijf.

## <a name="delete-the-original-vm"></a>De oorspronkelijke virtuele machine verwijderen
Virtuele harde schijven en virtuele machines zijn twee verschillende resources in Azure. Een virtuele harde schijf is daar waar het besturingssysteem, toepassingen en configuraties worden opgeslagen. De virtuele machine bestaat uit metagegevens en definieert de grootte of locatie en verwijst naar resources, zoals een virtuele harde schijf of virtuele netwerkinterfacekaart (NIC). Aan elke virtuele harde schijf wordt een lease toegewezen wanneer deze schijf wordt gekoppeld aan een virtuele machine. Hoewel gegevensschijven zelfs wanneer de virtuele machine wordt uitgevoerd, kunnen worden gekoppeld en losgekoppeld, kan de besturingssysteemschijf niet worden losgekoppeld tenzij de VM-resource wordt verwijderd. De lease blijft de besturingssysteemschijf koppelen aan een virtuele machine, zelfs wanneer die VM de status Gestopt en Toewijzing ongedaan gemaakt heeft.

De eerste stap bij het herstellen van uw virtuele machine bestaat uit het verwijderen van de VM-resource zelf. Wanneer de virtuele machine wordt verwijderd, blijven de virtuele harde schijven aanwezig in uw opslagaccount. Nadat de virtuele machine is verwijderd, kunt u de virtuele harde schijf koppelen aan een andere virtuele machine voor probleemoplossing. 

1. Meld u aan bij [Azure Portal](https://portal.azure.com). 
2. Klik op **Virtuele machines (klassiek)** in het menu aan de linkerkant van het scherm.
3. Selecteer de virtuele machine waarop het probleem optreedt, klik op **Schijven** en zoek naar de naam van de virtuele harde schijf. 
4. Selecteer de virtuele harde schijf van het besturingssysteem en controleer de **Locatie** om het opslagaccount waarin deze virtuele harde schijf zich bevindt op te zoeken. In het volgende voorbeeld is de tekenreeks direct vóór '. blob.core.windows.net ' is de naam van het opslagaccount.

    ```
    https://portalvhds73fmhrw5xkp43.blob.core.windows.net/vhds/SCCM2012-2015-08-28.vhd
    ```

    ![Afbeelding van de locatie van de virtuele machine](./media/virtual-machines-classic-recovery-disks-portal/vm-location.png)

5. Klik met de rechtermuisknop op de virtuele machine en selecteer vervolgens **verwijderen**. Zorg ervoor dat de schijven niet zijn geselecteerd wanneer u de virtuele machine verwijdert.
6. Maak een nieuwe VM voor herstel. Deze virtuele machine moet zich in dezelfde regio en resourcegroep (Cloudservice) bevinden als de probleem-VM.
7. Selecteer de VM voor herstel en selecteer vervolgens **Schijven** > **Bestaande koppelen**.
8. Klik om uw bestaande virtuele harde schijf te selecteren op **VHD-bestand**:

    ![Zoeken naar bestaande VHD](./media/virtual-machines-classic-recovery-disks-portal/select-vhd-location.png)

9. Selecteer het opslagaccount > VHD-container > de virtuele harde schijf en klik op de knop **Selecteren** om uw keuze te bevestigen.

    ![Uw bestaande VHD selecteren](./media/virtual-machines-classic-recovery-disks-portal/select-vhd.png)

10. Selecteer met de VH geselecteerd de knop **OK** om de bestaande virtuele harde schijf te koppelen.
11. Na enkele seconden wordt in het deelvenster **Schijven** voor uw virtuele machine uw bestaande virtuele harde schijf weergegeven als een gekoppelde gegevensschijf:

    ![Bestaande virtuele harde schijf gekoppeld als een gegevensschijf](./media/virtual-machines-classic-recovery-disks-portal/attached-disk.png)

## <a name="fix-issues-on-the-original-virtual-hard-disk"></a>Problemen op de oorspronkelijke virtuele harde schijf oplossen
Wanneer de bestaande virtuele harde schijf is gekoppeld, kunt u nu naar wens onderhouds- en probleemoplossingsstappen uitvoeren. Zodra u de problemen hebt opgelost, kunt u doorgaan met de volgende stappen.

## <a name="unmount-and-detach-the-original-virtual-hard-disk"></a>De oorspronkelijke virtuele harde schijf loskoppelen
Wanneer de fouten zijn opgelost koppelt u de bestaande virtuele harde schijf los van uw VM voor probleemoplossing. U kunt de virtuele harde schijf pas samen met een andere virtuele machine gebruiken als de lease die de virtuele harde schijf koppelt aan de VM voor probleemoplossing is vrijgegeven.  

1. Meld u aan bij [Azure Portal](https://portal.azure.com). 
2. Selecteer **Virtuele machines (klassiek)** in het menu aan de linkerkant van het scherm.
3. Zoek de VM voor herstel op. Selecteer schijven, klik met de rechtermuisknop op de schijf en selecteer vervolgens **Loskoppelen**.

## <a name="create-a-vm-from-the-original-hard-disk"></a>Een virtuele machine maken van de oorspronkelijke harde schijf

Gebruik voor het maken van een virtuele machine van de oorspronkelijke virtuele harde schijf [Azure-portal](https://portal.azure.com).

1. Meld u aan bij de [Azure Portal](https://portal.azure.com).
2. Links boven aan de portal, selecteer **nieuw** > **Compute** > **virtuele Machine** > **uit galerie**.
3. Selecteer in de sectie **Een afbeelding kiezen** de optie **Mijn schijven**, en selecteer vervolgens de oorspronkelijke virtuele harde schijf. Controleer de locatie-informatie. Dit is de regio waar de virtuele machine moet worden geïmplementeerd. Selecteer de knop Volgende.
4. Typ in de sectie **Configuratie van de virtuele machine** de naam van de virtuele machine en selecteer een grootte voor de virtuele machine.
