

## <a name="what-is-happening"></a>Wat gebeurt er?

Op [3 januari](https://googleprojectzero.blogspot.com/2018/01/reading-privileged-memory-with-side.html) is er een sectorbreed, hardwaregebaseerd beveiligingsprobleem gemeld. Veilig is altijd de hoogste prioriteit en wordt genomen active stappen om ervoor te zorgen dat er geen Azure klant wordt blootgesteld aan deze beveiligingsproblemen te houden van klanten.

Met de openbaarmaking van het beveiligingsprobleem we [het tijdstip gepland onderhoud versnelde](https://azure.microsoft.com/blog/securing-azure-customers-from-cpu-vulnerability/) en begon met de virtuele machines die de update nog steeds nodig automatisch opnieuw wordt opgestart.


## <a name="how-can-i-see-which-of-my-vms-are-already-updated"></a>Hoe kan ik zien welke van mijn VM's al is bijgewerkt? 

U kunt de status van uw VM's bekijken in de [VM-lijst in Azure Portal](https://aka.ms/T08tdc). Hier wordt ook aangegeven of het opnieuw opstarten is gelukt. Bij uw VM's wordt 'Al bijgewerkt' aangegeven als de update is toegepast en 'Gepland' als dit nog niet het geval is. Als u alleen de VM's wilt bekijken met de status 'Gepland', raadpleegt u [Azure Service Health](https://portal.azure.com/).

## <a name="can-i-find-out-exactly-when-my-vms-will-be-rebooted"></a>Vind ik precies wanneer mijn virtuele machines opnieuw wordt opgestart?

De beste manier om een waarschuwing over het opnieuw opstarten is voor het configureren van [gepland gebeurtenissen](https://docs.microsoft.com/azure/virtual-machines/windows/scheduled-events). Dit biedt een melding 15 minuten van de virtuele machine tijdelijk niet beschikbaar vanwege onderhoud.

## <a name="can-i-manually-redeploy-now-to-perform-the-required-maintenance"></a>Kan ik handmatig opnieuw implementeren nu om uit te voeren van het vereiste onderhoud? 

Er zijn geen garanties dat een opnieuw gedistribueerde virtuele machine wordt toegewezen aan een bijgewerkte host. Waar mogelijk, probeert de Azure-infrastructuur toewijzen van virtuele machines voor hosts die al is bijgewerkt. Het is mogelijk dat een virtuele machine opnieuw distribueren kan land op een host niet zijn bijgewerkt, in welk geval u mogelijk een tweede opnieuw opstarten, gedwongen als onderdeel van gepland onderhoud. Handmatige redeploys worden hierdoor niet aanbevolen als tijdelijke oplossing.

## <a name="how-long-will-the-reboot-take"></a>Hoe lang duurt het opnieuw opstarten? 

De meeste herstarts duurt ongeveer **30 minuten**.

## <a name="does-the-guest-os-need-to-be-updated"></a>Ondersteunt de Gast OS moeten worden bijgewerkt? 

Deze update Azure-infrastructuur, lost het bekend beveiligingsprobleem op het niveau van de hypervisor en vereist geen een update voor uw Windows- of Linux-VM-installatiekopieën. Als altijd, moet u blijven echter aanbevolen beveiligingsprocedures voor uw VM-installatiekopieën toepassen. Neem contact op met de leverancier van uw besturingssysteem voor updates en instructies, indien nodig. Voor klanten met virtuele Windows Server-machines zijn er instructies gepubliceerd. Deze vindt u [hier](../articles/virtual-machines/windows/mitigate-se.md).

## <a name="will-there-be-a-performance-impact-as-a-result-of-resolving-this-update"></a>Komt er ook van invloed op de prestaties als gevolg van het oplossen van deze update?

Het merendeel van de Azure-klanten hebben invloed op de merkbare prestaties met deze update niet zien. We hebben veel aandacht besteed aan optimalisatie van de CPU en het I/O-pad van de schijf. Een merkbare invloed op de prestaties na toepassing van de oplossing is dan ook niet geconstateerd. Wel kan een klein aantal klanten enige invloed op de netwerkprestatie ondervinden. Dit kan worden verholpen door het gebruik van Azure versnelde netwerken voor [Windows](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-powershell) of [Linux](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli), dit is een gratis functionaliteit die beschikbaar zijn voor alle Azure-klanten.

## <a name="i-follow-your-recommendations-for-high-availability-will-my-environment-remain-highly-available-during-the-reboot"></a>Ik ga als volgt uw aanbevelingen voor hoge beschikbaarheid, Mijn omgeving maximaal beschikbaar blijven tijdens het opnieuw opstarten?

Ja, virtuele machines die worden geïmplementeerd in een beschikbaarheidsset ingesteld of de virtuele-machineschaalsets constructie voor de Update-domeinen (UD). Bij het uitvoeren van onderhoud wordt Azure zich houdt aan de beperking UD en niet opnieuw wordt opgestart van virtuele machines van verschillende UD (binnen dezelfde beschikbaarheidsset). Raadpleeg voor meer informatie over hoge beschikbaarheid, [de beschikbaarheid van Windows virtuele machines in Azure beheren](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability) of [de beschikbaarheid van Linux virtuele machines in Azure beheren](https://docs.microsoft.com/azure/virtual-machines/linux/manage-availability).

## <a name="i-have-architected-my-business-continuitydisaster-recovery-plan-using-region-pairs-will-reboots-to-my-vms-occur-in-region-pairs-at-the-same-time"></a>Ik hebt mijn zakelijke continuïteit/noodherstelplan regio paren met ontworpen. Opnieuw wordt opgestart naar Mijn virtuele machines zich in paren regio op hetzelfde moment?

Normaal gesproken wordt gepland onderhoud voor Azure in gekoppelde regio's per regio geïmplementeerd. Dit minimaliseert het risico op onderbrekingen in beide regio's. Echter, vanwege de urgente aard van deze beveiligingsupdate we zijn uit te rollen de update naar alle regio's gelijktijdig.

## <a name="what-about-paas-services-on-azure"></a>Hoe zit het PaaS-services op Azure?  

De Azure-platform-services, waaronder web & mobile, gegevensservices, IoT, zonder server, enzovoort hebben oplossing voor de beveiligingslekken. Er is geen actie nodig is voor klanten die gebruikmaken van deze services.

## <a name="intel-released-additional-guidance-on-january-22-2018-related-to-the-security-vulnerabilities--will-this-guidance-cause-any-additional-maintenance-activities-by-azure"></a>Intel uitgebracht aanvullende richtlijnen op januari 22, 2018 betrekking hebben op de beveiligingsproblemen.  In deze richtlijnen, wordt geen extra onderhoudsactiviteiten door Azure?  

Azure oplossingen eerder aangekondigd op 3 Jan 2018 worden hierdoor niet beïnvloed door de [richtlijnen bijgewerkt](https://newsroom.intel.com/news/root-cause-of-reboot-issue-identified-updated-guidance-for-customers-and-partners/) van Intel. Er zijn geen extra onderhoud activiteit van de klant VM's als gevolg van deze nieuwe gegevens.
 

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie, [beveiligen van Azure-klanten CPU beveiligingslek](https://azure.microsoft.com/blog/securing-azure-customers-from-cpu-vulnerability/).
