

## <a name="what-is-happening"></a>Wat gebeurt er?

Er is een algemeen geaccepteerde, op hardware gebaseerde beveiligingsprobleem [vermeld op 3 januari](https://googleprojectzero.blogspot.com/2018/01/reading-privileged-memory-with-side.html). Veilig is altijd de hoogste prioriteit en wordt genomen active stappen om ervoor te zorgen dat er geen Azure klant wordt blootgesteld aan deze beveiligingsproblemen te houden van klanten.

Met de openbaarmaking van het beveiligingsprobleem we [het tijdstip gepland onderhoud versnelde](https://azure.microsoft.com/blog/securing-azure-customers-from-cpu-vulnerability/) en begon met de virtuele machines die de update nog steeds nodig automatisch opnieuw wordt opgestart.
 
## <a name="how-can-i-see-which-of-my-vms-are-already-updated"></a>Hoe kan ik zien welke van mijn VM's al is bijgewerkt? 

U ziet de status van uw virtuele machines, en als het opnieuw opstarten voltooid in de [VM lijst in de Azure portal](https://aka.ms/T08tdc). Uw virtuele machines worden weergegeven als 'Al bijgewerkt' als de update is toegepast, of 'Gepland' als de update nog steeds vereist is. Als u zien alleen uw virtuele machines wilt 'Geplande' naar verwijzen uw [Azure-servicestatus](https://portal.azure.com/).

## <a name="can-i-find-out-exactly-when-my-vms-will-be-rebooted"></a>Vind ik precies wanneer mijn virtuele machines opnieuw wordt opgestart?

De beste manier om een waarschuwing over het opnieuw opstarten is voor het configureren van [gepland gebeurtenissen](https://docs.microsoft.com/azure/virtual-machines/windows/scheduled-events). Dit biedt een melding 15 minuten van de virtuele machine tijdelijk niet beschikbaar vanwege onderhoud.

## <a name="can-i-manually-redeploy-now-to-perform-the-required-maintenance"></a>Kan ik handmatig opnieuw implementeren nu om uit te voeren van het vereiste onderhoud? 

Er zijn geen garanties dat een opnieuw gedistribueerde virtuele machine wordt toegewezen aan een bijgewerkte host. Waar mogelijk, probeert de Azure-infrastructuur toewijzen van virtuele machines voor hosts die al is bijgewerkt. Het is mogelijk dat een virtuele machine opnieuw distribueren kan land op een host niet zijn bijgewerkt, in welk geval u mogelijk een tweede opnieuw opstarten, gedwongen als onderdeel van gepland onderhoud. Handmatige redeploys worden hierdoor niet aanbevolen als tijdelijke oplossing.

## <a name="how-long-will-the-reboot-take"></a>Hoe lang duurt het opnieuw opstarten? 

De meeste herstarts duurt ongeveer **30 minuten**.

## <a name="does-the-guest-os-need-to-be-updated"></a>Ondersteunt de Gast OS moeten worden bijgewerkt? 

Deze update Azure-infrastructuur, lost het bekend beveiligingsprobleem op het niveau van de hypervisor en vereist geen een update voor uw Windows- of Linux-VM-installatiekopieën. Als altijd, moet u blijven echter aanbevolen beveiligingsprocedures voor uw VM-installatiekopieën toepassen. Neem contact op met de leverancier van uw besturingssysteem voor updates en instructies, indien nodig. Voor Windows Server-VM-klanten richtlijnen nu is gepubliceerd en is beschikbaar [hier](../articles/virtual-machines/windows/mitigate-se.md).

## <a name="will-there-be-a-performance-impact-as-a-result-of-resolving-this-update"></a>Komt er ook van invloed op de prestaties als gevolg van het oplossen van deze update?

Het merendeel van de Azure-klanten hebben invloed op de merkbare prestaties met deze update niet zien. We hebt gewerkt om te optimaliseren voor de CPU en schijf-i/o-pad en de gevolgen voor de merkbare prestaties niet ziet nadat de oplossing is toegepast. Een klein aantal klanten ondervinden sommige netwerken invloed op de prestaties. Dit kan worden verholpen door het gebruik van Azure versnelde netwerken voor [Windows](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-powershell) of [Linux](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli), dit is een gratis functionaliteit die beschikbaar zijn voor alle Azure-klanten.

## <a name="i-follow-your-recommendations-for-high-availability-will-my-environment-remain-highly-available-during-the-reboot"></a>Ik ga als volgt uw aanbevelingen voor hoge beschikbaarheid, Mijn omgeving maximaal beschikbaar blijven tijdens het opnieuw opstarten?

Ja, virtuele machines die worden geïmplementeerd in een beschikbaarheidsset ingesteld of de virtuele-machineschaalsets constructie voor de Update-domeinen (UD). Bij het uitvoeren van onderhoud wordt Azure zich houdt aan de beperking UD en niet opnieuw wordt opgestart van virtuele machines van verschillende UD (binnen dezelfde beschikbaarheidsset). Raadpleeg voor meer informatie over hoge beschikbaarheid, [de beschikbaarheid van Windows virtuele machines in Azure beheren](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability) of [de beschikbaarheid van Linux virtuele machines in Azure beheren](https://docs.microsoft.com/azure/virtual-machines/linux/manage-availability).

## <a name="i-have-architected-my-business-continuitydisaster-recovery-plan-using-region-pairs-will-reboots-to-my-vms-occur-in-region-pairs-at-the-same-time"></a>Ik hebt mijn zakelijke continuïteit/noodherstelplan regio paren met ontworpen. Opnieuw wordt opgestart naar Mijn virtuele machines zich in paren regio op hetzelfde moment?

Normaal gesproken worden gebeurtenissen voor gepland onderhoud Azure geïmplementeerd voor gekoppelde regio's een tegelijkertijd het risico van een onderbreking in beide regio's minimaliseren. Echter, vanwege de urgente aard van deze beveiligingsupdate we zijn uit te rollen de update naar alle regio's gelijktijdig.

## <a name="what-about-paas-services-on-azure"></a>Hoe zit het PaaS-services op Azure?  

De Azure-platform-services, waaronder web & mobile, gegevensservices, IoT, zonder server, enzovoort hebben oplossing voor de beveiligingslekken. Er is geen actie nodig is voor klanten die gebruikmaken van deze services.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie, [beveiligen van Azure-klanten CPU beveiligingslek](https://azure.microsoft.com/blog/securing-azure-customers-from-cpu-vulnerability/).
