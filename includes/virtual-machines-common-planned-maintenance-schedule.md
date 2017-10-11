

## <a name="multi-and-single-instance-vms"></a>Multi- en virtuele machines van één exemplaar
Veel klanten uitgevoerd op Azure aantal kritieke dat ze plannen kunnen wanneer de bijbehorende virtuele machines ondergaan gepland onderhoud vanwege de uitvaltijd--ongeveer 15 minuten--die deze gebeurtenis treedt op tijdens het onderhoud. U kunt beschikbaarheidssets gebruiken om te controleren wanneer ingerichte virtuele machines gepland onderhoud wordt.

Er zijn twee mogelijke configuraties voor virtuele machines die worden uitgevoerd op Azure. Virtuele machines zijn geconfigureerd als één of meerdere exemplaren. Als VMs in een beschikbaarheidsset zijn, zijn vervolgens die geconfigureerd als meerdere exemplaren. Houd er rekening mee, zelfs één virtuele machines kunnen worden geïmplementeerd in een beschikbaarheidsset, zodat ze worden behandeld als meerdere exemplaren. Als virtuele machines niet in een beschikbaarheidsset, zijn vervolgens die geconfigureerd als één exemplaar.  Zie voor meer informatie over beschikbaarheidssets [de beschikbaarheid van uw Windows virtuele Machines beheren](../articles/virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) of [beheren van de beschikbaarheid van uw virtuele Linux-Machines](../articles/virtual-machines/linux/manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Gepland onderhoud updates naar de single instance en meerdere exemplaren VM's afzonderlijk gebeuren. Opnieuw configureren van uw virtuele machines worden single instance (als ze meerdere exemplaren zijn) of meerdere exemplaren (als ze single instance zijn), kunt u bepalen wanneer de bijbehorende virtuele machines gepland onderhoud ontvangt. Zie [gepland onderhoud voor Azure Linux virtuele machines](../articles/virtual-machines/linux/planned-maintenance.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) of [gepland onderhoud voor Windows Azure virtuele machines](../articles/virtual-machines/windows/planned-maintenance.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) voor meer informatie over gepland onderhoud voor Azure Virtual machines.

## <a name="for-multi-instance-configuration"></a>Configuratie van meerdere instanties
U kunt de tijd die is gepland onderhoud van invloed is op uw virtuele machines die worden geïmplementeerd in een configuratie van de Beschikbaarheidsset is ingesteld door het verwijderen van deze virtuele machines van beschikbaarheidssets selecteren.

1. Een e-mailbericht verzonden naar u zeven kalenderdagen vóór het geplande onderhoud aan uw virtuele machines in een configuratie met meerdere exemplaren. De abonnement-id's en de namen van de betrokken virtuele machines van meerdere exemplaren worden opgenomen in de hoofdtekst van het e-mailbericht.
2. Tijdens deze zeven dagen, kunt u de tijd die uw exemplaren worden bijgewerkt door het verwijderen van uw exemplaar van meerdere virtuele machines in deze regio uit hun beschikbaarheidsset kiezen. Deze wijziging in de configuratie wordt opgestart, als de virtuele Machine wordt verplaatst van één fysieke host, gericht voor onderhoud, naar een andere fysieke host die niet is gericht voor onderhoud.
3. U kunt de virtuele machine verwijderen uit de beschikbaarheidsset voor de Azure-portal.

   1. Selecteer in de portal voor de virtuele machine verwijderen uit de Beschikbaarheidsset.  

   2. Onder **instellingen**, klikt u op **beschikbaarheidsset**.

      ![Selectie van de Beschikbaarheidsset](./media/virtual-machines-planned-maintenance-schedule/availabilitysetselection.png)

   3. In de beschikbaarheid van de vervolgkeuzemenu instellen, selecteert u "Geen deel uit van een beschikbaarheidsset."

      ![Verwijderen uit de Set](./media/virtual-machines-planned-maintenance-schedule/availabilitysetwarning.png)

   4. Klik aan de bovenkant **opslaan**. Klik op **Ja** om te bevestigen dat deze actie de virtuele machine opnieuw wordt gestart.

   >[!TIP]
   >U kunt de VM naar meerdere exemplaren later opnieuw configureren door een van de vermelde beschikbaarheidssets te selecteren.

4. Virtuele machines die zijn verwijderd uit de beschikbaarheidssets worden verplaatst naar de Single Instance hosts en worden niet bijgewerkt tijdens het geplande onderhoud op beschikbaarheid instellen configuraties.
5. Zodra de update van virtuele machines beschikbaarheid instellen is voltooid (volgens de planning die worden beschreven in de oorspronkelijke e-mail), moet u de virtuele machines toevoegen in hun beschikbaarheidssets. Als onderdeel van een beschikbaarheidsset configureert u de virtuele machines opnieuw als meerdere exemplaren en resulteert in een opnieuw opstarten. Normaal gesproken zodra alle meerdere exemplaren updates zijn voltooid voor de volledige Azure-omgeving, volgt single instance onderhoud.

Een virtuele machine verwijderen uit een beschikbaarheidsset kan ook worden bereikt met Azure PowerShell:

```
Get-AzureVM -ServiceName "<VmCloudServiceName>" -Name "<VmName>" | Remove-AzureAvailabilitySet | Update-AzureVM
```

## <a name="for-single-instance-configuration"></a>Voor de configuratie van één exemplaar
U kunt de tijd die is gepland onderhoud van invloed is op u virtuele machines in een configuratie met Single instance door deze virtuele machines toe te voegen in beschikbaarheidssets selecteren.

Stapsgewijs

1. Een e-mailbericht verzonden naar u zeven kalenderdagen vóór het geplande onderhoud naar virtuele machines in een configuratie met één exemplaar. De abonnement-id's en de namen van de betrokken Single Instance virtuele machines zijn opgenomen in de hoofdtekst van het e-mailbericht.
2. Tijdens deze zeven dagen, kunt u de tijd die uw exemplaar opnieuw wordt opgestart door uw Single instance VM's toe te voegen aan een beschikbaarheidsset in die dezelfde regio kiezen. Deze wijziging in de configuratie wordt opgestart, als de virtuele Machine wordt verplaatst van één fysieke host, gericht voor onderhoud, naar een andere fysieke host die niet is gericht voor onderhoud.
3. Volg hier instructies voor het toevoegen van bestaande virtuele machines in beschikbaarheidssets met de Azure-portal en Azure PowerShell. (Zie de Azure PowerShell-voorbeeldtoepassing die u als volgt.)
4. Zodra deze VMs zijn geconfigureerd als meerdere exemplaren, worden uitgesloten van het geplande onderhoud Single instance virtuele machines.
5. Zodra de single instance VM-update is voltooid (volgens de planning in de oorspronkelijke e-mail), kunt u de virtuele machines terugkeren naar de single instance door het verwijderen van de virtuele machines van hun beschikbaarheidssets.

Een virtuele machine toe te voegen aan een beschikbaarheidsset ook kan worden bereikt met Azure PowerShell:

    Get-AzureVM -ServiceName "<VmCloudServiceName>" -Name "<VmName>" | Set-AzureAvailabilitySet -AvailabilitySetName "<AvSetName>" | Update-AzureVM

<!--Anchors-->



<!--Link references-->
[Virtual Machines Manage Availability]: virtual-machines-windows-tutorial.md
[Understand planned versus unplanned maintenance]: virtual-machines-manage-availability.md#Understand-planned-versus-unplanned-maintenance/
