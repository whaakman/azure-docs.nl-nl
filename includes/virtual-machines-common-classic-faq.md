


In dit artikel komen enkele veelgestelde vragen aan bod over virtuele machines van Azure die zijn gemaakt met het klassieke implementatiemodel.

## <a name="can-i-migrate-my-vm-created-in-the-classic-deployment-model-to-the-new-resource-manager-model"></a>Kan ik een VM die is gemaakt met het klassieke implementatiemodel migreren naar het nieuwe model van Resource Manager?
Ja. Instructies voor het migreren vindt u hier:

* [Migreren van klassiek model naar Azure Resource Manager met Azure PowerShell](../articles/virtual-machines/windows/migration-classic-resource-manager-ps.md).
* [Migreren van klassiek model naar Azure Resource Manager met Azure CLI](../articles/virtual-machines/virtual-machines-linux-cli-migration-classic-resource-manager.md).

## <a name="what-can-i-run-on-an-azure-vm"></a>Wat kan ik uitvoeren op een VM van Azure?
Alle abonnees kunnen serversoftware uitvoeren op een virtuele machine van Azure. U kunt recente versies van Windows Server uitvoeren, evenals een aantal Linux-distributies. Zie deze artikelen voor meer informatie over ondersteuning:

• Voor VM's van Windows -- [Ondersteuning van Microsoft-serversoftware voor Azure Virtual Machines](http://go.microsoft.com/fwlink/p/?LinkId=393550)

• Voor VM's van Linux -- [Linux op door Azure goedgekeurde distributies](http://go.microsoft.com/fwlink/p/?LinkId=393551)

Voor het uitvoeren van ontwikkel- en testtaken voor installatiekopieën van Windows-clients zijn bepaalde versies van Windows 7 en Windows 8.1 beschikbaar voor MSDN-abonnees met Azure-voordelen en MSDN-abonnees met Pay-As-You-Go Dev/Test. Zie het Engelstalige blogbericht [Windows Client images for MSDN subscribers](https://azure.microsoft.com/blog/2014/05/29/windows-client-images-on-azure/) voor meer informatie, zoals instructies en beperkingen.

## <a name="why-are-affinity-groups-being-deprecated"></a>Waarom worden affiniteitsgroepen afgeschaft?
Affiniteitsgroepen zijn een verouderd concept voor een geografische groepering van cloudservice-implementaties en opslagaccounts van een klant in Azure. De groepen zijn destijds ontwikkeld om de prestaties van het VM-VM-netwerk in de vroege Azure-netwerkontwerpen te verbeteren. Affiniteitsgroepen boden ook ondersteuning voor de eerste versie van virtuele netwerken (VNets), die beperkt waren tot een kleine set hardware in een regio.

Het huidige Azure-netwerk in een regio is zodanig ontworpen dat affiniteitsgroepen niet meer nodig zijn. Virtuele netwerken ondersteunen nu ook een regionaal bereik en dus is een affiniteitsgroep niet meer vereist wanneer u een virtueel netwerk gebruikt. Dankzij deze verbeteringen krijgen klanten niet meer het advies om affiniteitsgroepen te gebruiken, omdat deze in sommige scenario's beperkingen kunnen veroorzaken. Als u affiniteitsgroepen gebruikt, worden uw virtuele machines onnodig gekoppeld aan specifieke hardware, waardoor de keuze van beschikbare VM-grootten afneemt. Een ander punt is dat er capaciteitsfouten kunnen optreden wanneer u probeert nieuwe VM's toe te voegen en de specifieke hardware die is gekoppeld aan de affiniteitsgroep, bijna geen capaciteit meer heeft.

De functies van affiniteitsgroepen zijn al afgeschaft in het implementatiemodel Azure Resource Manager en in Azure Portal. Voor de klassieke Azure-portal wordt de ondersteuning voor het maken van affiniteitsgroepen inmiddels afgebouwd. Dit geldt ook voor het maken van opslagresources die zijn gekoppeld aan een affiniteitsgroep. U hoeft geen wijzigingen aan te brengen in bestaande cloudservices die een affiniteitsgroep gebruiken. Het wordt echter afgeraden om affiniteitsgroepen te gebruiken voor nieuwe cloudservices, tenzij dit wordt geadviseerd door een ondersteuningsmedewerker van Azure.

## <a name="how-much-storage-can-i-use-with-a-virtual-machine"></a>Hoeveel opslagruimte kan ik gebruiken met een virtuele machine?
Elke gegevensschijf kan maximaal 1 TB groot zijn. Het aantal gegevensschijven dat u kunt gebruiken, is afhankelijk van de grootte van de virtuele machine. Zie [Grootten voor virtuele machines](../articles/virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) voor meer informatie.

Een Azure-opslagaccount biedt opslag voor de schijf met het besturingssysteem en eventuele gegevensschijven. Elke schijf is een VHD-bestand dat wordt opgeslagen als een pagina-blob. Zie [deze pagina](http://go.microsoft.com/fwlink/p/?LinkId=396819) voor prijsinformatie.

## <a name="which-virtual-hard-disk-types-can-i-use"></a>Welke typen virtuele harde schijf kan ik gebruiken?
Azure ondersteunt alleen vaste, virtuele harde schijven met de indeling VHD. Als u een VHDX hebt die u wilt gebruiken in Azure, moet u deze eerst converteren met behulp van Hyper-V Manager of de cmdlet [convert-VHD](http://go.microsoft.com/fwlink/p/?LinkId=393656). Daarna gebruikt u de cmdlet [Add-AzureVHD](https://msdn.microsoft.com/library/azure/dn495173.aspx) (in de modus Service Management) om de VHD te uploaden naar een opslagaccount in Azure, zodat u de schijf kunt gebruiken met virtuele machines.

* Zie [Een virtuele harde schijf met het Linux-besturingssysteem maken en uploaden](../articles/virtual-machines/linux/classic/create-upload-vhd-classic.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) voor instructies voor Linux.

## <a name="are-these-virtual-machines-the-same-as-hyper-v-virtual-machines"></a>Zijn deze virtuele machines hetzelfde als virtuele machines van Hyper-V?
Op veel punten zijn ze vergelijkbaar met Hyper-V-VM's van de 'eerste generatie', maar ze zijn niet precies hetzelfde. Beide typen bieden gevirtualiseerde hardware, en de virtuele harde schijven met VHD-indeling zijn compatibel. Dit betekent dat u ze kunt verplaatsen tussen Hyper-V en Azure. Er zijn echter drie belangrijke verschillen die gebruikers van Hyper-V soms verrassen:

* Azure biedt geen consoletoegang tot een virtuele machine. Een virtuele machine is pas toegankelijk nadat deze volledig is opgestart.
* VM's van Azure hebben in de meeste [grootten](../articles/virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) maar één virtuele netwerkadapter, wat betekent dat ze ook maar één extern IP-adres kunnen hebben. (De grootten A8 en A9 gebruiken een tweede netwerkadapter voor de communicatie van toepassingen tussen instanties in beperkte scenario's.)
* VM's van Azure bieden geen ondersteuning voor functies van Hyper-V-VM's van de tweede generatie. Zie voor meer informatie over deze functies [Specificaties van virtuele machines voor Hyper-V](http://technet.microsoft.com/library/dn592184.aspx) en [Overzicht van virtuele machines van generatie 2](https://technet.microsoft.com/library/dn282285.aspx).

## <a name="can-these-virtual-machines-use-my-existing-on-premises-networking-infrastructure"></a>Kunnen deze virtuele machines mijn bestaande, on-premises netwerkinfrastructuur gebruiken?
Voor virtuele machines die zijn gemaakt in het klassieke implementatiemodel kunt u Azure Virtual Network gebruiken om uw bestaande infrastructuur uit te breiden. De methode is vergelijkbaar met het opzetten van een filiaal. U kunt virtuele particuliere netwerken (VPN's) inrichten en beheren in Azure, maar u kunt ze ook op een veilige manier verbinden met een on-premises IT-infrastructuur. Zie [Overzicht van Azure Virtual Network](../articles/virtual-network/virtual-networks-overview.md) voor meer informatie.

Op het moment dat u de virtuele machine maakt, moet u het netwerk opgeven waarvan de virtuele machine deel moet uitmaken. Het is niet mogelijk om een bestaande virtuele machine te koppelen aan een virtueel netwerk. U dit probleem omzeilen door de virtuele harde schijf (VHD) los te koppelen van de bestaande virtuele machine en de schijf vervolgens gebruiken om een nieuwe virtuele machine te maken met de gewenste netwerkconfiguratie.

## <a name="how-can-i-access--my-virtual-machine"></a>Hoe krijg ik toegang tot mijn virtuele machine?
U moet een externe verbinding tot stand brengen om u aan te melden bij de virtuele machine. Dit kan met de functie Verbinding met extern bureaublad voor een virtuele machine van Windows of via een Secure Shell (SSH) voor een VM van Linux. Instructies vindt u hier:

* [Aanmelden bij een virtuele machine met Windows Server](../articles/virtual-machines/windows/classic/connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json). Er worden maximaal twee gelijktijdige verbindingen ondersteund, tenzij de server is geconfigureerd als een host voor sessies van Extern bureaublad-services.  
* [Aanmelden bij een virtuele machine met Linux](../articles/virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). SSH biedt standaard ondersteuning voor maximaal tien gelijktijdige verbindingen. U kunt dit aantal verhogen door het configuratiebestand te bewerken.

Als er problemen optreden met Extern bureaublad of SSH, installeert en gebruikt u de extensie [VMAccess](../articles/virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) om het probleem op te lossen.

Voor Windows-VM's gelden nog deze extra opties:

* Zoek de virtuele machine in de Azure-portal en klik vervolgens op **externe toegang opnieuw instellen** uit de opdrachtbalk.
* Raadpleeg [Problemen oplossen met verbindingen van Extern bureaublad met virtuele Azure-machines waarop Windows wordt uitgevoerd](../articles/virtual-machines/windows/troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Gebruik externe communicatie van Windows PowerShell om verbinding te maken met de VM of maak extra eindpunten zodat andere resources verbinding kunnen maken met de VM. Zie [Eindpunten voor een virtuele machine instellen](../articles/virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) voor meer informatie.

Als u bekend bent met Hyper-V, verwacht u misschien een hulpprogramma zoals VMConnect. Azure biedt echter geen vergelijkbaar hulpprogramma omdat consoletoegang tot een virtuele machine niet wordt ondersteund.

## <a name="can-i-use-the-temporary-disk-the-d-drive-for-windows-or-devsdb1-for-linux-to-store-data"></a>Kan ik de tijdelijke schijf (het station D: voor Windows of /dev/sdb1 voor Linux) gebruiken voor het opslaan van gegevens?
U moet de tijdelijke schijf (standaard het station D: voor Windows of /dev/sdb1 voor Linux) niet gebruiken voor het opslaan van gegevens. Deze schijven zijn alleen als tijdelijke opslag bedoeld, wat betekent dat u het risico loopt gegevens kwijt te raken die dan niet kunnen worden hersteld. Dit kan gebeuren wanneer de virtuele machine wordt verplaatst naar een andere host. Hiervoor zijn verschillende redenen te bedenken, zoals het aanpassen van de grootte van een virtuele machine, het bijwerken van de host of een hardwarefout op de host.

## <a name="how-can-i-change-the-drive-letter-of-the-temporary-disk"></a>Hoe wijzig ik de stationsletter van de tijdelijke schijf?
Op een virtuele machine van Windows kunt u de stationsletter wijzigen door het wisselbestand te verplaatsen en stationsletters opnieuw toe te wijzen. Het is wel essentieel dat u de stappen in een bepaalde volgorde uitvoert. Zie [Use the D: drive as a data drive on a Windows VM](../articles/virtual-machines/windows/change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) (Het station D: als gegevensstation gebruiken op een Windows-VM) voor instructies.

## <a name="how-can-i-upgrade-the-guest-operating-system"></a>Hoe kan ik het gastbesturingssysteem upgraden?
De term 'upgraden' betekent meestal overstappen naar een recentere versie van het besturingssysteem, met behoud van de huidige hardware. In het geval van VM's van Azure verschilt het proces voor het overstappen naar een recentere versie voor Linux- en Windows-machines:

* Gebruik voor VM's van Linux de hulpprogramma's en procedures voor pakketbeheer die beschikbaar zijn voor de distributie.
* Voor een virtuele machine van Windows moet u de server migreren, bijvoorbeeld met het hulpprogramma voor migratie van Windows-servers. U moet niet proberen om het gastbesturingssysteem te upgraden terwijl dit deel uitmaakt van Azure. Dit wordt namelijk niet ondersteund vanwege het risico de toegang tot de virtuele machine te verliezen. Als er tijdens de upgrade problemen optreden, is de kans aanwezig dat u geen Extern bureaublad-sessie kunt starten en de problemen dus niet kunt oplossen.

Zie [Functies en onderdelen migreren in Windows Server](http://go.microsoft.com/fwlink/p/?LinkId=396940) voor algemene informatie over de hulpprogramma's en processen voor het migreren van een computer met Windows Server.

## <a name="whats-the-default-user-name-and-password-on-the-virtual-machine"></a>Welke gebruikersnaam en welk wachtwoord worden standaard gebruikt op een virtuele machine?
De installatiekopieën die worden verstrekt door Azure bevatten geen vooraf geconfigureerde gebruikersnaam en wachtwoord. Wanneer u met behulp van een van deze installatiekopieën een virtuele machine maakt, moet u een gebruikersnaam en wachtwoord opgeven, die u daarna gebruikt om u aan te melden bij de virtuele machine.

Als u de gebruikersnaam of het wachtwoord bent vergeten en u de VM-agent hebt geïnstalleerd, kunt u de extensie [VMAccess](../articles/virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) installeren en gebruiken om het probleem op te lossen.

Aanvullende details:

* De Linux-installatiekopieën, als u de Azure-portal gebruikt, 'azureuser' wordt weergegeven als een standaardnaam van de gebruiker maar u kunt dit wijzigen met behulp van 'Uit galerie' in plaats van snel maken als de manier waarop de virtuele machine maken. Als u Uit galerie gebruikt, kunt u ook beslissen of u zich wilt aanmelden met een wachtwoord, een SSH-sleutel of beide. Het gebruikersaccount is een niet-bevoegde gebruiker met 'sudo'-toegang voor het uitvoeren van opdrachten met verhoogde bevoegdheden. Het root-account is uitgeschakeld.
* Voor Windows-installatiekopieën moet u een gebruikersnaam en wachtwoord opgeven wanneer u de virtuele machine maakt. Het account wordt toegevoegd aan de groep Administrators.

## <a name="can-azure-run-anti-virus-on-my-virtual-machines"></a>Kan Azure een antivirusprogramma uitvoeren op mijn virtuele machines?
Azure biedt verschillende opties voor antivirusoplossingen, maar het beheer hiervan moet u zelf regelen. Zo is het mogelijk dat u een afzonderlijk abonnement nodig hebt voor anti-malwaresoftware en moet u bepalen wanneer er scans worden uitgevoerd en updates worden geïnstalleerd. U kunt ondersteuning voor een antivirusprogramma toevoegen met behulp van een VM-extensie voor Microsoft Antimalware, Symantec Endpoint Protection of TrendMicro Deep Security Agent wanneer u een virtuele Windows-computer maakt. Dit kan trouwens ook op een later tijdstip. De extensies van Symantec en TrendMicro kunt u gebruiken met een gratis, tijdelijk proefabonnement of met een bestaand zakelijk abonnement. Microsoft Antimalware is gratis. Zie deze artikelen voor meer informatie:

* [How to install and configure Symantec Endpoint Protection on a Windows VM](http://go.microsoft.com/fwlink/p/?LinkId=404207) (Symantec Endpoint Protection installeren en configureren op een VM van Azure)
* [How to install and configure Trend Micro Deep Security as a Service on an Azure VM](http://go.microsoft.com/fwlink/p/?LinkId=404206) (Trend Micro Deep Security als een service installeren en configureren op een VM van Azure)
* [Deploying Antimalware Solutions on Azure Virtual Machines](https://azure.microsoft.com/blog/2014/05/13/deploying-antimalware-solutions-on-azure-virtual-machines/) (Antimalware-oplossingen implementeren op virtuele machines van Azure)

## <a name="what-are-my-options-for-backup-and-recovery"></a>Wat zijn de opties voor back-up en herstel?
Azure Backup is beschikbaar als preview in bepaalde regio's. Zie [Back-ups maken van virtuele machines van Azure](../articles/backup/backup-azure-arm-vms.md) voor meer informatie. Andere oplossingen zijn beschikbaar van gecertificeerde partners. Als u wilt weten wat er op dit moment beschikbaar is, zoekt u Azure Marketplace.

Een andere mogelijkheid is om de voorzieningen voor momentopnamen van blob-opslag te gebruiken. Het is belangrijk dat u de VM afsluit voordat u een bewerking gaat uitvoeren die afhankelijk is van een momentopname van een blob. Hierdoor worden openstaande schrijfbewerkingen worden opgeslagen en wordt het bestandssysteem in een consistente status geplaatst.

## <a name="how-does-azure-charge-for-my-vm"></a>Hoe worden er in Azure kosten in rekening gebracht voor mijn VM?
Azure rekent een uurprijs op basis van de grootte en het besturingssysteem van de VM. Voor niet-hele uren worden alleen de minuten van gebruik in rekening gebracht. Als u de VM maakt aan de hand van een VM-installatiekopie met bepaalde vooraf geïnstalleerde software, kunnen er extra softwarekosten bijkomen (op uurbasis). In Azure worden afzonderlijke kosten in rekening gebracht voor opslag voor de besturingssysteemschijf en de gegevensschijven van de VM. Tijdelijke opslagruimte is gratis.

Er worden kosten in rekening gebracht wanneer de VM de status Uitvoeren of Gestopt heeft, maar niet bij de status Gestopt (Toewijzing ongedaan gemaakt). Als u een VM in de status Gestopt (Toewijzing ongedaan gemaakt) wilt zetten, voert u een van de volgende handelingen uit:

* Afgesloten of de virtuele machine verwijderen uit de Azure-portal.
* Gebruik de cmdlet Stop-AzureVM, beschikbaar in de Azure PowerShell-module.
* Gebruik de bewerking Rol afsluiten in de REST-API van Servicebeheer en geef StoppedDeallocated op voor het element PostShutdownAction.

Zie [Prijzen van virtuele machines](https://azure.microsoft.com/pricing/details/virtual-machines/) voor meer informatie.

## <a name="will-azure-reboot-my-vm-for-maintenance"></a>Wordt mijn VM opnieuw opgestart door Azure voor onderhoud?
Azure zal uw VM soms opnieuw opstarten als onderdeel van reguliere, geplande onderhoudsupdates in de Azure-datacenters.

Niet-gepland onderhoudsgebeurtenissen kunnen optreden wanneer Azure een ernstige hardwareprobleem vaststelt dat betrekking heeft op uw VM. Voor niet-geplande gebeurtenissen word de VM automatisch gemigreerd naar een goed werkende host en wordt de VM opnieuw opgestart.

In het geval van een zelfstandige VM (een VM die geen deel uitmaakt van een beschikbaarheidsset) wordt er vanuit Azure ten minste één week voor gepland onderhoud een e-mail verstuurd naar de servicebeheerder van het abonnement met de mededeling dat de VM tijdens dit onderhoud opnieuw kan worden opgestart. Toepassingen die worden uitgevoerd op de VM's kunnen tijdelijk niet beschikbaar zijn.

Ook kunt u de Azure portal of Azure PowerShell om de logboeken opnieuw worden opgestart bij het opstarten is opgetreden vanwege gepland onderhoud bekijken. Zie voor meer informatie [VM-opstartlogboeken bekijken](https://azure.microsoft.com/blog/2015/04/01/viewing-vm-reboot-logs/).

Om redundantie te bieden, plaatst u twee of meer vergelijkbaar geconfigureerde VM's in dezelfde beschikbaarheidsset. Op deze manier zorgt u ervoor dat er ten minste één VM beschikbaar is tijdens gepland of ongepland onderhoud. Azure garandeert bepaalde niveaus van VM-beschikbaarheid voor deze configuratie. Zie [Beschikbaarheid van virtuele machines beheren](../articles/virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) voor meer informatie.

## <a name="additional-resources"></a>Aanvullende resources
[Informatie over Azure Virtual Machines](../articles/virtual-machines/virtual-machines-linux-about.md)

[Virtuele machines maken en beheren met Azure CLI](../articles/virtual-machines/linux/tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[Windows-VM's maken en beheren met Azure PowerShell](../articles/virtual-machines/windows/tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

