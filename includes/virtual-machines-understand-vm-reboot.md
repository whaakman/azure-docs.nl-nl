Azure virtuele machines (VM's) kunnen soms opnieuw worden opgestart voor geen zichtbare reden, zonder bewijs van uw hebben gestart de bewerking opnieuw opstarten. Dit artikel worden de acties en gebeurtenissen die kunnen leiden tot virtuele machines op te starten en biedt inzicht in hoe voorkomt problemen met de onverwachte opnieuw opstarten of verminder de impact van dergelijke problemen.

## <a name="configure-the-vms-for-high-availability"></a>De virtuele machines voor hoge beschikbaarheid configureren
De beste manier om het beveiligen van een toepassing die wordt uitgevoerd op Azure tegen VM opnieuw wordt opgestart en uitvaltijd is voor het configureren van de virtuele machines voor hoge beschikbaarheid.

Als u dit niveau van redundantie voor uw toepassingen maken, is het raadzaam dat u twee of meer virtuele machines in een beschikbaarheidsset groeperen. Deze configuratie zorgt ervoor dat ten minste één virtuele machine tijdens beide gepland of ongepland onderhoud, beschikbaar is en voldoet aan de 99,95% [Azure SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_5/).

Zie de volgende artikelen voor meer informatie over beschikbaarheidssets:

- [De beschikbaarheid van virtuele machines beheren](../articles/virtual-machines/windows/manage-availability.md)
- [Beschikbaarheid van virtuele machines configureren](../articles/virtual-machines/windows/classic/configure-availability.md)

## <a name="resource-health-information"></a>Statusgegevens van de resource 
Azure resourcestatus is een service die wordt de status van afzonderlijke Azure-resources en biedt bruikbare richtlijnen voor het oplossen van problemen. In een omgeving waar het is niet mogelijk voor rechtstreekse toegang tot servers of Infrastructuurelementen, is het doel van de resourcestatus in minder tijd dat u aan het oplossen van problemen besteden. Het doel is met name om te beperken van de tijd te bepalen of de hoofdmap van het probleem wordt veroorzaakt in de toepassing of in een gebeurtenis in de Azure-platform. Zie voor meer informatie [Understand and gebruik resourcestatus](../articles/resource-health/resource-health-overview.md).

## <a name="actions-and-events-that-can-cause-the-vm-to-reboot"></a>Acties en gebeurtenissen die ertoe kunnen leiden dat de virtuele machine opnieuw op te starten

### <a name="planned-maintenance"></a>Gepland onderhoud
Microsoft Azure voert regelmatig updates over de hele wereld voor het verbeteren van de betrouwbaarheid, prestaties en beveiliging van de host-infrastructuur waarop virtuele machines. Veel van deze updates, waaronder geheugen behouden updates worden uitgevoerd zonder invloed op uw virtuele machines of cloudservices.

Sommige updates vereisen echter opnieuw worden opgestart. In dergelijke gevallen zijn de virtuele machines afgesloten terwijl we de infrastructuur patch en vervolgens de virtuele machines opnieuw worden gestart.

Om te begrijpen welke Azure gepland onderhoud is en hoe dit invloed heeft op de beschikbaarheid van uw virtuele Linux-machines, Zie de artikelen die hier worden vermeld. De artikelen vindt achtergrondinformatie over de Azure geplande onderhoudsproces en het plannen van gepland onderhoud aan de impact verder te beperken.

- [Gepland onderhoud voor virtuele machines in Azure](../articles/virtual-machines/windows/planned-maintenance.md)
- [Gepland onderhoud op Azure Virtual machines plannen](../articles/virtual-machines/windows/classic/planned-maintenance-schedule.md)

### <a name="memory-preserving-updates"></a>Updates met geheugenbehoud   
Gebruikers krijgen voor deze klasse van updates in Microsoft Azure, niet van invloed op de actieve virtuele machines. Dit zijn veelal updates van onderdelen of services die kunnen worden bijgewerkt zonder de actieve sessie te verstoren. Sommige zijn platform infrastructuurupdates op het besturingssysteem van de host die kunnen worden toegepast zonder opnieuw opstarten van de virtuele machines.

Deze updates met behoud van geheugen worden bewerkstelligd met-technologie waarmee in-place live migratie. Wanneer deze wordt bijgewerkt, de virtuele machine wordt geplaatst in een *onderbroken* status. Deze status blijft het RAM-geheugen behouden terwijl het besturingssysteem van de onderliggende host de vereiste updates en patches ontvangt. De virtuele machine wordt hervat binnen 30 seconden wordt onderbroken. Nadat de virtuele machine wordt hervat, wordt de klok automatisch gesynchroniseerd.

Vanwege de periode korte pauze vermindert implementeren van updates via dit mechanisme aanzienlijk de gevolgen voor de virtuele machines. Niet alle updates kunnen echter op deze manier worden geïmplementeerd. 

Meerdere exemplaren updates (voor VM's in een beschikbaarheidsset) zijn toegepaste één updatedomein tegelijk.

> [!NOTE]
> Linux-machines met oude versie van de kernel een zijn beïnvloed door een paniek kernel tijdens deze updatemethode. Om dit te voorkomen, bijwerken naar kernel versie 3.10.0-327.10.1 of hoger. Zie voor meer informatie [An Azure virtuele Linux-machine op een kernel 3.10 gebaseerde panics na de upgrade van een host knooppunt](https://support.microsoft.com/help/3212236).     
    
### <a name="user-initiated-reboot-or-shutdown-actions"></a>Gebruiker gestart opnieuw opstarten of afsluiten acties
 
Als u opnieuw van de Azure-portal, Azure PowerShell, opdrachtregelinterface of API opnieuw instellen opstarten, vindt u de gebeurtenis in de [Azure Activity Log](../articles/monitoring-and-diagnostics/monitoring-overview-activity-logs.md).

Als u de actie vanuit het besturingssysteem van de VM uitvoert, vindt u de gebeurtenis in het systeemlogboek in Logboeken.

Andere scenario's die doorgaans ertoe leiden dat de virtuele machine opnieuw opstarten bevatten meerdere configuratiewijziging acties. Normaal gesproken ziet u een waarschuwing die aangeeft dat de uitvoering van een bepaalde actie leidt ertoe dat de virtuele machine opnieuw worden opgestart. Voorbeelden zijn VM formaat bewerkingen, het wachtwoord van de Administrator-account wilt wijzigen en het instellen van een statisch IP-adres.

### <a name="azure-security-center-and-windows-update"></a>Azure Security Center en Windows Update
Azure Security Center bewaakt dagelijkse Windows en Linux VM's voor het besturingssysteem updates ontbreken. Security Center haalt een lijst met beschikbare beveiligingsupdates en essentiële updates via Windows Update of Windows Server Update Services (WSUS), afhankelijk van welke service is geconfigureerd op een virtuele machine van Windows. Security Center controleert ook of de nieuwste updates voor Linux-systemen. Als uw VM een systeemupdate ontbreekt is, wordt door Security Center adviseert systeemupdates toe te passen. De toepassing van deze systeemupdates wordt geregeld via het Beveiligingscentrum in de Azure portal. Nadat u de updates hebt toegepast, is het mogelijk dat virtuele machine opnieuw opstarten vereist. Zie voor meer informatie [toepassen systeemupdates in Azure Security Center](../articles/security-center/security-center-apply-system-updates.md).

Zoals lokale servers biedt Azure niet push-updates via Windows Update naar Windows Azure virtuele machines, omdat deze machines zijn bedoeld om te worden beheerd door de gebruikers. U bent, echter aangeraden te laat u de automatische Update van Windows-instelling is ingeschakeld. Automatische installatie van updates van Windows Update kan ook leiden tot opnieuw wordt opgestart om te worden uitgevoerd nadat de updates zijn toegepast. Zie voor meer informatie [Veelgestelde vragen over de Windows Update](https://support.microsoft.com/help/12373/windows-update-faq).

### <a name="other-situations-affecting-the-availability-of-your-vm"></a>In andere gevallen die invloed hebben op de beschikbaarheid van uw virtuele machine
Er zijn andere gevallen waarin Azure actief het gebruik van een virtuele machine mogelijk onderbreken. U ontvangt e-mailmeldingen voordat deze actie wordt ondernomen, zodat u krijgt de gelegenheid de onderliggende problemen oplossen. Voorbeelden van problemen die invloed hebben op de beschikbaarheid van de VM zijn schendingen van de beveiliging en de vervaldatum van betalingswijzen.

### <a name="host-server-faults"></a>Host-server-fouten 
De virtuele machine wordt gehost op een fysieke server die wordt uitgevoerd binnen een Azure-datacenter. De fysieke server wordt uitgevoerd een agent de Hostagent naast enkele andere Azure-onderdelen genoemd. Wanneer deze Azure softwareonderdelen op de fysieke server reageert, activeert het bewakingssysteem opnieuw opstarten van de hostserver te repareren. De virtuele machine vindt meestal binnen vijf minuten opnieuw en blijft live op dezelfde host als voorheen.

Server-fouten worden gewoonlijk veroorzaakt door hardwarefouten, zoals het uitvallen van een harde schijf of een SSD-schijf. Azure continu bewaakt deze instanties, identificeert de onderliggende fouten, en de updates nadat de oplossing is geïmplementeerd en getest.

Omdat sommige host-server-fouten specifiek voor die server zijn kunnen, kan een situatie met een herhaalde VM opnieuw opstarten worden verbeterd door het handmatig opnieuw distribueren van de virtuele machine naar een andere hostserver. Deze bewerking kan worden geactiveerd met behulp van de **implementeren** optie op de detailpagina van de virtuele machine of door te stoppen en opnieuw starten van de virtuele machine in de Azure portal.

### <a name="auto-recovery"></a>Automatisch herstel
Als de host-server kan niet opnieuw voor een bepaalde reden opgestart, initieert de Azure-platform een automatisch herstel actie voor het nemen van de defecte hostserver buiten rotatie voor verder onderzoek. 

Alle virtuele machines op die host worden automatisch verplaatst naar een andere, in orde host-server. Dit proces is meestal voltooid binnen 15 minuten. Zie voor meer informatie over het automatische herstelproces, [automatisch herstel van virtuele machines](https://azure.microsoft.com/blog/service-healing-auto-recovery-of-virtual-machines).

### <a name="unplanned-maintenance"></a>Niet-gepland onderhoud
In zeldzame gevallen, het team van Azure-bewerkingen mogelijk onderhoudsactiviteiten om te controleren of de algemene status van de Azure-platform uit te voeren. Dit probleem kan invloed hebben op de beschikbaarheid van de virtuele machine en dit wordt vaak veroorzaakt in dezelfde automatisch herstel actie zoals eerder beschreven.  

Niet-geplande maintenances omvatten het volgende:

- Urgente knooppunt defragmentatie
- Switch-updates voor urgente netwerk

### <a name="vm-crashes"></a>VM-crashes
Virtuele machines opnieuw opstarten vanwege problemen met de in de virtuele machine zelf. De werkbelasting of de rol die wordt uitgevoerd op de virtuele machine mogelijk een bug controle binnen het gastbesturingssysteem geactiveerd. Voor informatie over het bepalen van de reden voor de crash weergeven het systeem en toepassingslogboeken voor VM's van Windows en de logboeken van de seriële voor virtuele Linux-machines.

### <a name="storage-related-forced-shutdowns"></a>Opslag-gerelateerde geforceerd afsluiten
Virtuele machines in Azure, is afhankelijk van virtuele schijven voor het besturingssysteem en de opslag van gegevens die wordt gehost op de infrastructuur van Azure Storage. Wanneer de beschikbaarheid of de verbinding tussen de virtuele machine en de bijbehorende virtuele schijven is van invloed op meer dan 120 seconden, voert het Azure-platform geforceerd afsluiten van de virtuele machines om te voorkomen dat beschadigde gegevens. De virtuele machines worden automatisch ingeschakeld terug nadat de opslagverbinding is hersteld. 

De duur van het afsluiten kan zo kort vijf minuten wel aanzienlijk langer. Dit is een van de specifieke gevallen die is gekoppeld aan de opslag-gerelateerde geforceerd afsluiten: 

**I/o van meer dan beperkt**

Virtuele machines is mogelijk tijdelijk afgesloten wanneer de i/o-aanvragen zijn consistent beperkt omdat het volume van de i/o-bewerkingen per seconde (IOPS) groter is dan de i/o-limieten voor de schijf. (Standaard schijfopslag is beperkt tot 500 IOPS.) Dit probleem te verhelpen, schijf-striping gebruiken of configureren van de opslagruimte in de Gast-VM, afhankelijk van de werkbelasting. Zie voor meer informatie [Azure VM's configureren voor optimale opslagprestaties](http://blogs.msdn.com/b/mast/archive/2014/10/14/configuring-azure-virtual-machines-for-optimal-storage-performance.aspx).

Hogere IOPS-limieten zijn beschikbaar via Azure Premium-opslag met maximaal 80.000 IOPS. Zie voor meer informatie [High-Performance Premium-opslag](../articles/virtual-machines/windows/premium-storage.md).

### <a name="other-incidents"></a>Andere incidenten
In zeldzame gevallen kan een probleem met de wijdverbreid kan invloed hebben op meerdere servers in een Azure-datacenter. Als dit probleem optreedt, wordt er door het team van Azure e-mailmeldingen verzendt naar de betrokken abonnementen. U kunt controleren de [Azure Service Health dashboard](https://azure.microsoft.com/status/) en de Azure portal voor de status van actieve storingen en voorbij incidenten.
