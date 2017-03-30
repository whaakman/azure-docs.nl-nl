Azure-cloudoplossingen zijn gebouwd op virtuele machines (emulatie van de fysieke computerhardware), waarmee software-implementaties flexibeler kunnen worden verpakt en resources beter kunnen worden geconsolideerd dan bij fysieke hardware. Met [Docker](https://www.docker.com)-containers en het docker-ecosysteem zijn de manieren waarop u gedistribueerde software kunt ontwikkelen, verzenden en beheren ingrijpend uitgebreid. Toepassingscode in een container wordt geïsoleerd van de host-VM en andere containers op dezelfde virtuele machine. Deze isolatie biedt u meer flexibiliteit voor ontwikkeling en implementatie.

Azure biedt de volgende Docker-waarden:

* [Veel](../articles/virtual-machines/virtual-machines-linux-docker-machine.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) [verschillende](../articles/virtual-machines/virtual-machines-linux-dockerextension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) manieren om Docker-hosts te maken voor containers die geschikt zijn voor uw situatie
* De [Azure Container Service](https://azure.microsoft.com/documentation/services/container-service/) maakt clusters van containerhosts met behulp van orchestrators zoals **Marathon** en **Swarm**.
* [Azure Resource Manager](../articles/azure-resource-manager/resource-group-overview.md) en [resourcegroepsjablonen](../articles/resource-group-authoring-templates.md) voor eenvoudiger implementeren en bijwerken van complexe gedistribueerde toepassingen
* Integratie met een breed scala aan zowel bedrijfseigen als open-source beheerhulpprogramma's

En omdat u programmatisch virtuele machines en Linux-containers kunt maken in Azure, kunt u ook *orchestration*-hulpprogramma's voor VM’s en containers gebruiken voor het maken van groepen virtuele machines en voor het implementeren van toepassingen in zowel Linux-containers als [Windows-containers](https://msdn.microsoft.com/virtualization/windowscontainers/about/about_overview).

Dit artikel beschrijft deze begrippen niet alleen op hoog niveau, maar bevat ook koppelingen naar meer informatie, zelfstudies en producten die gerelateerd zijn aan het gebruik van containers en clusters in Azure. Als u al op de hoogte bent van al deze informatie en alleen op zoek bent naar de koppelingen, vindt u deze hier: [hulpprogramma's voor het werken met containers](#tools-for-working-with-azure-vms-and-containers).

## <a name="the-difference-between-virtual-machines-and-containers"></a>Het verschil tussen virtuele machines en containers
Virtuele machines worden uitgevoerd in een geïsoleerde hardwarevirtualisatieomgeving die wordt geleverd door een [hypervisor](http://en.wikipedia.org/wiki/Hypervisor). In Azure handelt de service voor [virtuele machines](https://azure.microsoft.com/services/virtual-machines/) dat allemaal voor u af: u maakt virtuele machines door het besturingssysteem te kiezen en te configureren &mdash;of door een aangepaste VM-installatiekopie te uploaden. Virtuele machines vormen een betrouwbare, beproefde technologie. Bovendien zijn er veel hulpprogramma's beschikbaar om het besturingssysteem en de toepassingen op virtuele machines te beheren.  Apps op een virtuele machine zijn verborgen voor het hostbesturingssysteem. Vanuit het perspectief van een toepassing op of een gebruiker van een virtuele machine lijkt de virtuele machine een autonome fysieke computer.

[Linux-containers](http://en.wikipedia.org/wiki/LXC) en containers die zijn gemaakt en worden gehost met dockerhulpprogramma's, maken voor het bieden van isolatie geen gebruik van een hypervisor. Bij toepassing van containers gebruikt de containerhost processen en functies voor het isoleren van het bestandssysteem van de Linux-kernel om de apps, bepaalde functies van de kernel en het eigen geïsoleerde bestandssysteem aan de container weer te geven. Vanuit het perspectief van een app die binnen een container wordt uitgevoerd, lijkt de container een unieke instantie van een besturingssysteem. Een ingesloten app kan geen processen of andere resources buiten de container zien.

In een dockercontainer worden veel minder resources gebruikt dan op een virtuele machine. Dockercontainers maken gebruik van toepassingsisolatie en van een uitvoeringsmodel dat de kernel van de dockerhost niet deelt. De container legt veel minder beslag op de schijf omdat deze niet een volledig besturingssysteem bevat. In vergelijking met een virtuele machine is de opstarttijd aanzienlijk korter en de vereiste schijfruimte aanzienlijk kleiner.
Windows-containers bieden dezelfde voordelen als Linux-containers voor apps die in Windows worden uitgevoerd. Windows-containers ondersteunen het Docker-installatiekopieformaat en Docker-API, maar ze kunnen ook worden beheerd met behulp van PowerShell. Er zijn twee containerruntimes beschikbaar met Windows-containers: Windows Server-containers en Hyper-V-containers. Hyper-V-containers bieden een extra isolatielaag door elke container op een uiterst geoptimaliseerde virtuele machine te hosten. Zie [Informatie over Windows-containers](https://msdn.microsoft.com/virtualization/windowscontainers/about/about_overview) voor meer informatie over Windows-containers. Leer hoe u [een Azure Container Service-cluster implementeert](/articles/container-service/container-service-deployment.md) om aan de slag te gaan met Windows-containers in Azure.

## <a name="what-are-containers-good-for"></a>Waar zijn containers goed voor?

Voordelen van containers:

* Toepassingscode kan snel worden ontwikkeld en op grote schaal worden gedeeld
* Apps kunnen snel en op een betrouwbare manier worden getest
* Apps kunnen snel en op een betrouwbare manier worden geïmplementeerd

Containers worden uitgevoerd op een containerhost &mdash; een besturingssysteem. In Azure betekent dit een virtuele Azure-machine. Zelfs als het idee van containers u aanspreekt, hebt u nog steeds een VM-infrastructuur nodig die als host fungeert voor de containers. Voordelen zijn echter dat het containers niet uitmaakt op welke VM ze worden uitgevoerd (of de container een Linux- of Windows-uitvoeringsomgeving wil is bijvoorbeeld wel van belang).


## <a name="what-are-containers-good-for"></a>Waar zijn containers goed voor?
Ze zijn goed voor een groot aantal dingen, maar ze stimuleren &mdash;net als [Azure Cloud Services](https://azure.microsoft.com/services/cloud-services/) en [Azure Service Fabric](../articles/service-fabric/service-fabric-overview.md)&mdash; het maken van uit een enkele service bestaande, microservicegerichte gedistribueerde toepassingen, waarbij het toepassingsontwerp is gebaseerd op kleinere, samenstelbare delen in plaats van grotere, sterker gekoppelde onderdelen.

Dit geldt met name in openbare cloudomgevingen zoals Azure, waarin u virtuele machines huurt wanneer en waar u maar wilt. Niet alleen krijgt u isolatie en snelle implementatie en hulpprogramma's voor orchestration, u kunt ook efficiëntere beslissingen over de toepassingsinfrastructuur nemen.

Mogelijk hebt u bijvoorbeeld een implementatie die bestaat uit 9 virtuele Azure-machines met een grote omvang voor een maximaal beschikbare, gedistribueerde toepassing. Als de onderdelen van deze toepassing kunnen worden geïmplementeerd in containers, kunt u mogelijk slechts 4 virtuele machines gebruiken en de onderdelen van uw toepassing binnen 20 containers implementeren voor redundantie en taakverdeling.

Dit is uiteraard slechts een voorbeeld, maar als u dit in uw scenario kunt doen, kunt u rekening houden met pieken in gebruik met meer containers in plaats van meer virtuele Azure-machines en de resterende totale CPU-belasting nog veel efficiënter gebruiken dan voorheen.

Bovendien zijn er veel scenario's die zich niet lenen voor een benadering op basis van microservices; u weet zelf het beste of microservices en containers u kunnen helpen.

### <a name="container-benefits-for-developers"></a>Voordelen van containers voor ontwikkelaars
In het algemeen is het eenvoudig om te zien dat containertechnologie een stap vooruit is, maar er zijn ook specifiekere voordelen. Laten we het voorbeeld nemen van Docker-containers. In dit onderwerp duiken we nu niet dieper in Docker (lees [Wat is Docker?](https://www.docker.com/whatisdocker/) voor meer informatie, of [wikipedia](http://wikipedia.org/wiki/Docker_%28software%29)), maar Docker en het Docker-ecosysteem bieden enorm veel voordelen voor ontwikkelaars en IT-professionals.

Ontwikkelaars grijpen snel naar Docker-containers, omdat ze met name het gebruik van Linux- en Windows-containers gemakkelijk maken:

* Ze kunnen eenvoudige, incrementele opdrachten gebruiken voor het maken van een vaste installatiekopie die gemakkelijk te implementeren is en kunnen het bouwen van die installatiekopieën automatiseren met behulp van een dockerfile
* Ze kunnen deze installatiekopieën eenvoudig delen met eenvoudige push- en pull-opdrachten in [git](https://git-scm.com/)-stijl voor [openbare](https://registry.hub.docker.com/) of [persoonlijke dockerregisters](../articles/virtual-machines/virtual-machines-linux-docker-registry-in-blob-storage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* Ze kunnen denken in termen van geïsoleerde toepassingsonderdelen in plaats van computers
* Ze kunnen een groot aantal hulpprogramma's gebruiken die werken met dockercontainers en verschillende basisinstallatiekopieën

### <a name="container-benefits-for-operations-and-it-professionals"></a>Voordelen van containers voor operationele en IT-professionals
IT- en operationele professionals profiteren ook van de combinatie van containers en virtuele machines.

* ingesloten services zijn geïsoleerd van de uitvoeringsomgeving van de VM-host
* ingesloten code is verifieerbare identiek
* ingesloten services kunnen worden gestart, gestopt en snel worden verplaatst tussen ontwikkel-, test- en productieomgevingen

Van functies zoals deze &mdash; en er zijn meer &mdash; worden gevestigde bedrijven blij. Professionele IT-organisaties hebben hierbij de taak resources &mdash; inclusief pure verwerkingskracht &mdash; te koppelen aan de vereiste taken, niet alleen om het hoofd boven water te houden, maar om de klanttevredenheid en het bereik te vergroten. Kleine bedrijven, ISV's en start-ups hebben exact dezelfde vereisten, maar kunnen deze anders beschrijven.

## <a name="what-are-virtual-machines-good-for"></a>Waar zijn virtuele machines goed voor?
Virtuele machines vormen de backbone van cloudcomputing, en dat gaat niet veranderen. Virtuele machines starten trager, hebben een grotere schijfruimte en worden niet rechtstreeks toegewezen aan een architectuur van microservices, maar hebben zeer belangrijke voordelen:

1. Standaard beschikken ze over veel krachtigere standaardbeveiligingsinstellingen voor de hostcomputer
2. Ze ondersteunen alle belangrijke besturingssysteem- en toepassingsconfiguraties
3. Ze hebben lang bestaande hulpprogramma-ecosystemen voor opdracht en controle
4. Ze bieden de uitvoeringsomgeving voor hostcontainers

Het laatste punt is belangrijk, omdat een ingesloten toepassing nog steeds een specifiek besturingssysteem en CPU-type vereist, afhankelijk van de aanroepen die de toepassing doet. Het is belangrijk te onthouden dat u containers installeert op virtuele machines omdat ze de toepassingen bevatten die u wilt implementeren; containers zijn geen vervanging voor virtuele machines of besturingssystemen.

## <a name="high-level-feature-comparison-of-vms-and-containers"></a>Vergelijking van functies op hoog niveau van virtuele machines en containers
In de volgende tabel wordt op een zeer hoog niveau het soort functieverschillen beschreven dat &mdash; zonder veel extra werk &mdash; bestaat tussen virtuele machines en Linux-containers. Houd er rekening mee dat sommige functies mogelijk meer of minder wenselijk zijn afhankelijk van uw eigen toepassingsbehoeften en dat, net als bij alle software, extra werk leidt tot ondersteuning van meer functies, met name op het gebied van beveiliging.

| Functie | VM's | Containers |
|:--- | --- | --- |
| 'Standaard' beveiligingsondersteuning |in een hogere mate |in een iets lagere mate |
| Geheugen op schijf vereist |Volledig besturingssysteem plus apps |Alleen app-vereisten |
| Tijd om opnieuw te starten |Aanzienlijk langer: opstarten van het besturingssysteem plus laden van app |Aanzienlijk korter: alleen apps moeten worden gestart omdat de kernel al wordt uitgevoerd |
| Draagbaarheid |Draagbaar met de juiste voorbereiding |Draagbaar binnen installatiekopieformaat; doorgaans minder |
| Automatisering van installatiekopieën |Varieert aanzienlijk, afhankelijk van besturingssysteem en apps |[Docker-register](https://registry.hub.docker.com/); overige |

## <a name="creating-and-managing-groups-of-vms-and-containers"></a>Groepen virtuele machines en containers maken en beheren
Op dit moment denkt een ontwerper, ontwikkelaar of IT-specialist mogelijk, "Ik kan dit ALLEMAAL automatiseren; dit IS echt deze echt Data-Center-As-A-Service!".

Dit klopt, het kan, en er is aantal systemen, waarvan vele die u waarschijnlijk al gebruikt, die groepen virtuele Azure-machines kunnen beheren en aangepaste code kunnen invoeren met scripts, vaak met de [CustomScriptingExtension voor Windows](https://msdn.microsoft.com/library/azure/dn781373.aspx) of de [CustomScriptingExtension voor Linux](https://azure.microsoft.com/blog/2014/08/20/automate-linux-vm-customization-tasks-using-customscript-extension/). U kunt &mdash; en hebt dit misschien al gedaan &mdash; uw Azure-implementaties automatiseren met behulp van PowerShell- of Azure CLI-scripts.

Deze mogelijkheden worden vervolgens vaak gemigreerd naar hulpprogramma's zoals [Puppet](https://puppetlabs.com/) en [Chef](https://www.chef.io/) om het maken en configureren voor VM’s op de gewenste schaal te automatiseren. (Hier zijn enkele koppelingen met informatie over [het gebruik van deze hulpprogramma's met Azure](#tools-for-working-with-containers).)

### <a name="azure-resource-group-templates"></a>Azure-resourcegroepsjablonen
Recenter heeft Azure de [Azure-bronbeheer](../articles/resource-manager-deployment-model.md) REST API uitgebracht en PowerShell- en Azure CLI-hulpprogramma’s bijgewerkt zodat deze gemakkelijk te gebruiken zijn. U kunt volledige toepassingstopologieën implementeren, wijzigen of opnieuw implementeren met behulp van [Azure Resource Manager-sjablonen](../articles/resource-group-authoring-templates.md) met de Azure-bronbeheer-API via:

* de [Azure Portal met behulp van sjablonen](https://github.com/Azure/azure-quickstart-templates)&mdash;, gebruik de knop 'DeployToAzure'
* de [Azure CLI](../articles/virtual-machines/virtual-machines-linux-cli-deploy-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* de [Azure PowerShell-modules](../articles/virtual-machines/virtual-machines-linux-cli-deploy-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="deployment-and-management-of-entire-groups-of-azure-vms-and-containers"></a>Implementatie en beheer van hele groepen virtuele Azure-machines en containers
Er zijn enkele populaire systemen die hele groepen virtuele machines kunnen implementeren en Docker (of andere Linux-containerhostsystemen) erop kunnen installeren als automatiseerbare groep. Zie het gedeelte [containers en hulpprogramma's](#containers-and-vm-technologies) hieronder voor rechtstreekse koppelingen. Er zijn verschillende systemen die dit in hogere of mindere mate doen en dit is geen volledige lijst. Afhankelijk van uw vaardigheden en scenario's zijn ze wel of niet bruikbaar.

Docker heeft een eigen set hulpprogramma's voor het maken van virtuele machines ([docker-machine](../articles/virtual-machines/virtual-machines-linux-docker-machine.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) en een taakverdelend docker-containerhulpprogramma voor clusterbeheer ([Swarm](../articles/virtual-machines/virtual-machines-linux-docker-swarm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). Daarnaast wordt de [Azure Docker VM-extensie](https://github.com/Azure/azure-docker-extension/blob/master/README.md) geleverd met standaardondersteuning voor [ `docker-compose` ](https://docs.docker.com/compose/), dat geconfigureerde toepassingscontainers kan implementeren in meerdere containers.

Daarnaast kunt u [Data Center Operating System (DCOS) van Mesosphere](http://docs.mesosphere.com/install/azurecluster/) uitproberen. DCOS is gebaseerd op de open-source [mesos](http://mesos.apache.org/) 'kernel voor gedistribueerde systemen' waarmee u uw datacenter kunt behandelen als één adresseerbare service. DCOS heeft ingebouwde pakketten voor verschillende belangrijke systemen zoals [Spark](http://spark.apache.org/) en [Kafka](http://kafka.apache.org/) (en andere) evenals ingebouwde services zoals [Marathon](https://mesosphere.github.io/marathon/) (een controlesysteem voor containers) en [Chronos](https://mesos.github.io/chronos/) (een gedistribueerde planner). Mesos is afgeleid van ervaringen opgedaan bij Twitter, AirBnb en andere grote internetbedrijven. U kunt ook **Swarm** gebruiken als de orchestration-engine.

Ook is [Kubernetes](https://azure.microsoft.com/blog/2014/08/28/hackathon-with-kubernetes-on-azure/) een open-source systeem voor het beheer van virtuele machines en containergroepen afgeleid van ervaringen opgedaan bij Google. U kunt [Kubernetes zelfs gebruiken met Weave om netwerkondersteuning te bieden](https://github.com/GoogleCloudPlatform/kubernetes/blob/master/docs/getting-started-guides/coreos/azure/README.md#kubernetes-on-azure-with-coreos-and-weave).

[Deis](http://deis.io/overview/) is een open-source 'Platform-as-a-Service' (PaaS) waarmee u gemakkelijk toepassingen op uw eigen servers kunt implementeren en beheren. Deis is gebouwd op Docker en CoreOS en biedt een lichtgewicht PaaS voorzien van door Heroku geïnspireerde werkstroom. U kunt eenvoudig [een Azure VM-groep met 3 knooppunten maken en Deis installeren](../articles/virtual-machines/virtual-machines-linux-deis-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) in Azure en vervolgens [een Hello World Go-toepassing installeren](../articles/virtual-machines/virtual-machines-linux-deis-cluster.md#deploy-and-scale-a-hello-world-application).

[CoreOS](https://coreos.com/os/docs/latest/booting-on-azure.html), een Linux-distributie met een geoptimaliseerde footprint, ondersteuning voor Docker en een eigen containersysteem genoemd [rkt](https://github.com/coreos/rkt), heeft ook een beheerhulpprogramma voor containergroepen met de naam [Fleet](https://coreos.com/using-coreos/clustering/).

Ubuntu, een andere populaire Linux-distributie biedt goede ondersteuning voor Docker, maar ondersteunt ook [Linux-clusters (LXC-stijl)](https://help.ubuntu.com/lts/serverguide/lxc.html).

## <a name="tools-for-working-with-azure-vms-and-containers"></a>Hulpprogramma's voor het werken met virtuele Azure-machines en containers
Als u wilt werken met containers en virtuele Azure-machines hebt u hulpprogramma's nodig. Dit gedeelte bevat een lijst met slechts enkele van de meest bruikbare of belangrijke concepten en hulpprogramma's over containers, groepen en de grotere configuratie- en orchestration-hulpprogramma’s die ermee worden gebruikt.

> [!NOTE]
> Dit gebied verandert bijzonder snel en wij doen wel ons best om dit onderwerp en de koppelingen up-to-date te houden, maar misschien is dit een onmogelijke taak. Zoek regelmatig naar interessante onderwerpen om op de hoogte te blijven!
>
>

### <a name="containers-and-vm-technologies"></a>Containers en VM-technologieën
Enkele Linux-containertechnologieën:

* [Docker](https://www.docker.com)
* [LXC](https://linuxcontainers.org/)
* [CoreOS en rkt](https://github.com/coreos/rkt)
* [Open Container Project](http://opencontainers.org/)
* [RancherOS](http://rancher.com/rancher-os/)

Koppelingen naar Windows-containers:

* [Windows Containers](https://msdn.microsoft.com/virtualization/windowscontainers/about/about_overview)

Koppelingen naar Visual Studio Docker:

* [Visual Studio 2015 RC-hulpprogramma’s voor Docker - preview](https://visualstudiogallery.msdn.microsoft.com/6f638067-027d-4817-bcc7-aa94163338f0)

Docker-hulpprogramma's:

* [Docker-daemon](https://docs.docker.com/installation/#installation)
* Docker-clients
  * [Windows-dockerclient op Chocolatey](https://chocolatey.org/packages/docker)
  * [Installatie-instructies voor Docker](https://docs.docker.com/installation/#installation)

Docker in Microsoft Azure:

* [Docker VM-extensie voor Linux in Azure](../articles/virtual-machines/virtual-machines-linux-dockerextension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Gebruikershandleiding voor Azure Docker VM-extensie](https://github.com/Azure/azure-docker-extension/blob/master/README.md)
* [De Docker VM-extensie gebruiken met de Azure-opdrachtregelinterface (Azure CLI)](../articles/virtual-machines/linux/classic/cli-use-docker.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [De Docker VM-extensie gebruiken vanuit Azure Portal](../articles/virtual-machines/linux/classic/portal-use-docker.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Docker-machine gebruiken in Azure](../articles/virtual-machines/virtual-machines-linux-docker-machine.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Docker gebruiken met Swarm in Azure](../articles/virtual-machines/virtual-machines-linux-docker-swarm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Aan de slag met Docker en Compose in Azure](../articles/virtual-machines/virtual-machines-linux-docker-compose-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Een Azure-resourcegroepsjabloon gebruiken om snel een Docker-host in Azure te maken](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu)
* [De ingebouwde ondersteuning voor `compose` ](https://github.com/Azure/azure-docker-extension#11-public-configuration-keys) voor ingesloten toepassingen
* [Een persoonlijk Docker-register implementeren in Azure](../articles/virtual-machines/virtual-machines-linux-docker-registry-in-blob-storage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Linux-distributies en Azure voorbeelden:

* [CoreOS](https://coreos.com/os/docs/latest/booting-on-azure.html)

Configuratie, clusterbeheer en container-orchestration:

* [Fleet in CoreOS](https://coreos.com/using-coreos/clustering/)
* Deis

  * [Een Azure VM-groep met 3 knooppunten maken, Deis installeren en een Hello World Go-toepassing starten](../articles/virtual-machines/virtual-machines-linux-deis-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* Kubernetes

  * [Volledige handleiding voor geautomatiseerde Kubernetes-clusterimplementatie met CoreOS en Weave](https://github.com/GoogleCloudPlatform/kubernetes/blob/master/docs/getting-started-guides/coreos/azure/README.md#kubernetes-on-azure-with-coreos-and-weave)
  * [Kubernetes Visualizer](https://azure.microsoft.com/blog/2014/08/28/hackathon-with-kubernetes-on-azure/)
* [Mesos](http://mesos.apache.org/)

  * [Data Center Operating System (DCOS) van Mesosphere](http://beta-docs.mesosphere.com/install/azurecluster/)
* [Jenkins](https://jenkins-ci.org/) en [Hudson](http://hudson-ci.org/)

  * [Blog: Jenkins Slave Plug-in voor Azure](http://msopentech.com/blog/2014/09/23/announcing-jenkins-slave-plugin-azure/)
  * [GitHub-repo: Jenkins Storage Plug-in voor Azure](https://github.com/jenkinsci/windows-azure-storage-plugin)
  * [Externe partij: Hudson Slave Plug-in voor Azure](http://wiki.hudson-ci.org/display/HUDSON/Azure+Slave+Plugin)
  * [Externe partij: Hudson Storage Plug-in voor Azure](https://github.com/hudson3-plugins/windows-azure-storage-plugin)
* [Azure Automation](https://azure.microsoft.com/services/automation/)

  * [Video: Het gebruik van Azure Automation met virtuele Linux-machines](http://channel9.msdn.com/Shows/Azure-Friday/Azure-Automation-104-managing-Linux-and-creating-Modules-with-Joe-Levy)
* Powershell DSC voor Linux

  * [Blog: Powershell DSC gebruiken met Linux](http://blogs.technet.com/b/privatecloud/archive/2014/05/19/powershell-dsc-for-linux-step-by-step.aspx)
  * [GitHub: Docker Client DSC](https://github.com/anweiss/DockerClientDSC)

## <a name="next-steps"></a>Volgende stappen
Bekijk [Docker-](https://www.docker.com) en [Windows-containers](https://msdn.microsoft.com/virtualization/windowscontainers/about/about_overview).

<!--Anchors-->
[microservices]: http://martinfowler.com/articles/microservices.html
[microservice]: http://martinfowler.com/articles/microservices.html
<!--Image references-->
