
 
De recente openbaarmaking van een [nieuwe klasse van CPU-beveiligingsproblemen](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/ADV180002) die bekend staat als speculatieve uitvoering side-kanaal aanvallen heeft geleid tot vragen van klanten meer duidelijkheid te zoeken. 

 
## <a name="azure-infrastructure"></a>Azure-infrastructuur

De problemen beschreven in de openbaarmaking beveiligingslek kunnen worden gebruikt voor de grens van een hypervisor negeren en het vrijgeven van geheugen tussen twee CO gehoste virtuele machines. Zoals vermeld in een eerdere [blogbericht](https://azure.microsoft.com/blog/securing-azure-customers-from-cpu-vulnerability/), Azure oplossingen ter bescherming van klanten via deze kwetsbaarheid mogelijk heeft toegepast.  Microsoft raadt altijd aan klanten aanbevolen beveiligingsprocedures voor uw VM-installatiekopieën met inbegrip van alle beveiligingsupdates installeren van de leverancier van het besturingssysteem toe te passen.

## <a name="paas-services-on-azure"></a>PaaS-Services op Azure
Azure PaaS-aanbiedingen hebben oplossingen standaard geïmplementeerd. Er is geen actie vereist van klanten. Hieronder vindt u een Azure Cloud Services-uitzondering.  


## <a name="azure-cloud-services"></a>Azure Cloud Services

[Azure Cloud Services](https://azure.microsoft.com/services/cloud-services/) bij automatische updates is automatisch ingeschakeld ontvangt u een versie van het Gastbesturingssysteem met oplossingen voor deze beveiligingsproblemen. 

De volgende Gast OS releases omvatten updates met bescherming tegen speculatieve side kanaal beveiligingslekken:

* WA-GUEST-OS-5.15_201801-01
* WA-GUEST-OS-4.50_201801-01


Moet ook een klein aantal klanten die gebruikmaken van Azure Cloud Services voor het hosten van niet-vertrouwde code inschakelen [Kernel virtuele adres maken van schaduwkopieën](#enabling-kernel-virtual-address-shadowing-on-windows-server) naast het bijwerken van het Gastbesturingssysteem. Dit biedt extra beveiliging tegen speculatieve uitvoering side-kanaal beveiligingsproblemen. Dit kan worden bereikt via een taak starten. Hieronder vindt u meer informatie over welke klanten en gebruiksscenario's voor deze functie en het in te schakelen, vereisen.


## <a name="azure-virtual-machines-windows--linux"></a>Virtuele Machines in Azure (Windows & Linux)

Microsoft raadt altijd aan dat klanten alle beveiligingsupdates installeren. 

De beveiliging januari 2018 updatepakket bevat oplossingen voor deze beveiligingsproblemen en meer. Controleer of u een ondersteunde antivirusprogramma toepassing worden uitgevoerd voordat u updates voor het besturingssysteem installeert. Neem contact op met de leverancier van de antivirussoftware voor informatie over de compatibiliteit. 

Adres speculatieve uitvoering beveiligingslekken, updates voor de Linux kernel vereist en kan worden verkregen door uw provider distributie indien beschikbaar. 

Een klein aantal klanten die gebruikmaken van Azure Virtual Machines (Windows) om niet-vertrouwde code te hosten, moet ook inschakelen [Kernel virtuele adres maken van schaduwkopieën](#enabling-kernel-virtual-address-shadowing-on-windows-server) die zorgt voor extra beveiliging tegen speculatieve uitvoering side-kanaal beveiligingsproblemen.  Hieronder vindt u meer informatie over welke klanten en het gebruik scenario waarbij deze functie en het in te schakelen.


## <a name="enabling-kernel-virtual-address-shadowing-on-windows-server"></a>Adres van de Kernel virtuele beheren op WindowsServer op afstand inschakelen

Klanten die niet-vertrouwde code uitvoeren met Windows Server moeten een functie Kernel virtuele adres maken van schaduwkopieën die biedt beveiliging voor systemen waarop niet-vertrouwde code wordt uitgevoerd met weinig gebruikersrechten inschakelen.

Deze extra beveiliging kan invloed hebben op prestaties en is standaard uitgeschakeld. Kernel virtuele adres maken van schaduwkopieën wordt beschermd tegen openbaarmaking van informatie van proces naar proces en kernel-naar-proces.

Uw virtuele machines, Cloud Services of lokale servers lopen risico als ze in een van de volgende categorieën vallen:

* Azure genest virtualisatie Hyper-V-Hosts
* Extern bureaublad-Services Hosts (RDSH)
* Virtuele machines of cloudservices met niet-vertrouwde code zoals containers of niet-vertrouwde uitbreidingen voor de database, niet-vertrouwde webinhoud of werkbelastingen die code die wordt geleverd door externe bronnen worden uitgevoerd.

Een voorbeeld van een scenario waarbij Kernel virtuele adres maken van schaduwkopieën nodig: 

|     |
|-----|
|Azure een virtuele machine wordt uitgevoerd een service waarbij niet-vertrouwde gebruikers kunnen indienen JavaScript-code die wordt uitgevoerd met beperkte rechten. Op dezelfde virtuele machine bestaat er een bijzondere rechten proces die geheime gegevens die niet toegankelijk is voor die gebruikers bevat. In dit geval is het nodig is om te beveiligen tegen openbaarmaking van tussen de twee Kernel virtuele adres maken van schaduwkopieën.|
|     | 

Specifieke instructies voor het inschakelen van de Kernel virtuele adres maken van schaduwkopieën zijn beschikbaar via [Windows Server KB4072698](https://support.microsoft.com/help/4072698/windows-server-guidance-to-protect-against-the-speculative-execution).


> [!NOTE]
> Op het moment van publicatie is Kernel virtuele adres maken van schaduwkopieën alleen beschikbaar in Windows Server 2016, Windows Server 2012 R2 en Windows Server 2008 R2.  
>
>

Als u een Linux-Server uitvoert, neem contact op met de leverancier van uw besturingssysteem voor updates en instructies.

## <a name="branch-target-injection-mitigation-support-microcode"></a>Vertakking doel injectie risicobeperking ondersteuning (Microcode)

Klanten die gebruikmaken van hulpprogramma's waarmee de aanwezigheid van beperkingen op basis van microcode gedetecteerd rapporteren dat Azure niet-gepatchte is. Dit is onjuist. Op het moment van deze publicatie vertakking doel injectie risicobeperking ondersteuning is niet beschikbaar gemaakt van de Hypervisor Azure in Azure Virtual Machines of Azure Cloud Services. Dit betekent dat virtuele Machines zich niet bewust zijn van de aanwezigheid van microcode en een instructieset uitgebreide niet gebruiken. Dit betekent niet dat Azure kwetsbaar voor aanvallen van cross-VM speculatieve uitvoering side kanaal is.
 
Als wij met partners uit de sector samenwerken kan meer updates beschikbaar worden.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie, [beveiligen van Azure-klanten CPU beveiligingslek](https://azure.microsoft.com/blog/securing-azure-customers-from-cpu-vulnerability/).