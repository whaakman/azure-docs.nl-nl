# <a name="use-infrastructure-automation-tools-with-virtual-machines-in-azure"></a>Infrastructuur automatiseringsprogramma's gebruiken met virtuele machines in Azure
Als u wilt maken en beheren van virtuele Azure-machines (VM's) in een consistente, geschaalde manier, is een vorm van automatisering doorgaans gewenst. Er zijn veel hulpprogramma's en oplossingen waarmee u kunt de implementatie van de volledige Azure-infrastructuur en het levenscyclusbeheer van automatiseren. Dit artikel bevat enkele van de infrastructuur automatiseringsprogramma's die u in Azure gebruiken kunt. Deze hulpprogramma's passend vaak een van de volgende methoden:

- De configuratie van virtuele machines automatiseren
    - De volgende hulpprogramma [Ansible](#ansible), [Chef](#chef), en [Puppet](#puppet).
    - Hulpprogramma's die specifiek zijn voor aanpassing van de VM zijn [cloud init](#cloud-init) voor Linux VM's [PowerShell Desired State Configuration (DSC)](#powershell-dsc), en de [Azure aangepaste Scriptextensie](#azure-custom-script-extension) voor alle Virtuele machines in Azure.
 
- Beheer van infrastructuur automatiseren
    - De volgende hulpprogramma [verpakker](#packer) voor het automatiseren van aangepaste VM image te maken, en [Terraform](#terraform) voor het automatiseren van de procedure voor het maken van de infrastructuur.
    - [Azure Automation](#azure-automation) acties kunnen uitvoeren in uw Azure- en on-premises infrastructuur.

- Implementatie van de toepassing en levering automatiseren
    - Voorbeelden zijn onder meer [Visual Studio Team Services](#visual-studio-team-services) en [Jenkins](#jenkins).


## <a name="ansible"></a>Ansible
[Ansible](https://www.ansible.com/) is een automatiseringsengine voor het beheer van de configuratie of maken van VM-toepassingsimplementatie. Ansible maakt gebruik van een agent minder, meestal met SSH-sleutels, om te verifiëren en beheren van de doelmachines. Configuratietaken zijn gedefinieerd in runbooks, met een aantal Ansible modules beschikbaar specifieke taken uit te voeren. Zie voor meer informatie [hoe Ansible werkt](https://www.ansible.com/how-ansible-works).

Leer hoe u het volgende doet:

- [Installeren en configureren van Ansible op Linux voor gebruik met Azure](../articles/virtual-machines/linux/ansible-install-configure.md).
- [Maak een basic VM](../articles/virtual-machines/linux/ansible-create-vm.md).
- [Maakt een complete VM-omgeving met inbegrip van ondersteunende resources](../articles/virtual-machines/linux/ansible-create-complete-vm.md).


## <a name="chef"></a>Chef
[Chef](https://www.chef.io/) is een automatiseringsplatform die helpt bepalen hoe uw infrastructuur is geconfigureerd, geïmplementeerd en beheerd. Extra onderdelen opgenomen Chef Habitat voor automatisering van de levenscyclus van toepassingen in plaats van de infrastructuur en Chef inspectie mogelijk die helpt het automatiseren van naleving van de beveiligings- en vereisten. Chef-Clients worden geïnstalleerd op de doelmachines met een of meer centrale Chef-Servers die opslaan en beheren van de configuraties. Zie voor meer informatie [een overzicht van Chef](https://docs.chef.io/chef_overview.html).

Leer hoe u het volgende doet:

- [Chef implementeren vanuit Azure Marketplace automatiseren](https://azuremarketplace.microsoft.com/marketplace/apps/chef-software.chef-automate?tab=Overview).
- [Chef installeren op Windows en maken van Azure Virtual machines](../articles/virtual-machines/windows/chef-automation.md).


## <a name="puppet"></a>Puppet
[Puppet](https://www.puppet.com) is een automatiseringsplatform voor enterprise-ready die verantwoordelijk is voor het proces voor levering en implementatie van toepassing. Agents zijn geïnstalleerd op de doelmachines om toe te staan, Puppet-Master om uit te voeren manifesten die de gewenste configuratie van de Azure-infrastructuur bepalen en virtuele machines. Puppet kunt integreren met andere oplossingen zoals Jenkins en GitHub voor een verbeterde devops-werkstroom. Zie voor meer informatie [hoe Puppet werkt](https://puppet.com/product/how-puppet-works).

Leer hoe u het volgende doet:

- [Puppet vanuit Azure Marketplace implementeren](https://azuremarketplace.microsoft.com/marketplace/apps/puppet.puppet-enterprise-2016-1?tab=Overview).


## <a name="cloud-init"></a>Cloud-init
[Cloud-init](https://cloudinit.readthedocs.io) is een veelgebruikte benadering voor het aanpassen van een Linux-VM als deze voor de eerste keer wordt opgestart. U kunt cloud init gebruiken voor het installeren van pakketten en bestanden schrijven of om gebruikers en beveiliging te configureren. Omdat cloud init wordt aangeroepen tijdens het opstartproces, zijn er geen extra stappen of de vereiste agents naar uw configuratie toe te passen.  Voor meer informatie over de juiste indeling uw `#cloud-config` bestanden, Zie de [documentatiesite voor cloud-init](http://cloudinit.readthedocs.io/en/latest/topics/format.html#cloud-config-data).  `#cloud-config`bestanden zijn tekstbestanden gecodeerd in base64.

Cloud-init werkt ook via distributies. Bijvoorbeeld, u niet gebruikt **apt get-installatie** of **yum installeren** om een pakket te installeren. In plaats daarvan kunt u een lijst met pakketten te installeren. Het hulpprogramma voor systeemeigen pakket cloud init automatisch gebruikt voor de distro die u selecteert.

 Er wordt om beschikbare installatiekopieën van het cloud-init zijn ingeschakeld in de Azure marketplace gewerkt met onze aangebracht Linux distro partners. Deze installatiekopieën Controleer uw cloud-init-implementaties en configuraties naadloos werken met virtuele machines en virtuele-machineschaalsets. De volgende tabel geeft een overzicht van de huidige beschikbaarheid van de installatiekopieën van cloud-init zijn ingeschakeld op de Azure-platform:

| Uitgever | Aanbieding | SKU | Versie | cloud-init gereed
|:--- |:--- |:--- |:--- |:--- |:--- |
|Canonical |UbuntuServer |16.04 TNS |meest recente |ja | 
|Canonical |UbuntuServer |14.04.5-LTS |meest recente |ja |
|CoreOS |CoreOS |Stabiel |meest recente |ja |
|OpenLogic |CentOS |7 CI |meest recente |preview |
|RedHat |RHEL |7 ONBEWERKTE CI |meest recente |preview |

Meer informatie over cloud-init op Azure meer:

- [Cloud-init-ondersteuning voor virtuele Linux-machines in Azure](../articles/virtual-machines/linux/using-cloud-init.md)
- [Probeer een zelfstudie over automatische configuratie van de virtuele machine met behulp van cloud-init](../articles/virtual-machines/linux/tutorial-automate-vm-deployment.md).


## <a name="powershell-dsc"></a>PowerShell DSC
[PowerShell Desired State Configuration (DSC)](https://msdn.microsoft.com/en-us/powershell/dsc/overview) is een beheerplatform voor het definiëren van de configuratie van de doelcomputers. DSC kan ook worden gebruikt op Linux via de [Open Management Infrastructure (OMI) server](https://collaboration.opengroup.org/omi/).

DSC-configuraties definiëren wat u wilt installeren op een computer en het configureren van de host. Een lokale Configuration Manager (LCM)-engine wordt uitgevoerd op elk doelknooppunt waarmee aangevraagde acties op basis van pushed configuraties worden verwerkt. Een pull-server is een webservice die wordt uitgevoerd op een centrale host voor het opslaan van de DSC-configuraties en de bijbehorende bronnen. De pull-server communiceert met de engine LCM op elke doelhost voor de vereiste configuraties bieden en rapporteren van naleving.

Leer hoe u het volgende doet:

- [Maak een DSC-basisconfiguratie](https://msdn.microsoft.com/powershell/dsc/quickstart).
- [Een DSC-pull-server configureren](https://msdn.microsoft.com/powershell/dsc/pullserver).
- [Gebruik van DSC voor Linux](https://msdn.microsoft.com/powershell/dsc/lnxgettingstarted).


## <a name="azure-custom-script-extension"></a>Extensie voor aangepaste scripts Azure
De extensie Azure aangepaste scripts voor [Linux](../articles/virtual-machines/linux/extensions-customscript.md) of [Windows](../articles/virtual-machines/windows/extensions-customscript.md) worden gedownload en scripts uitgevoerd op Azure Virtual machines. U kunt de uitbreiding kunt gebruiken wanneer u een virtuele machine maken of elk gewenst moment na de virtuele machine gebruikt wordt. 

Scripts kunnen worden gedownload vanuit Azure-opslag of een openbare locatie zoals een GitHub-opslagplaats. Met de extensie voor aangepaste scripts, kunt u scripts schrijven in elke taal die wordt uitgevoerd op de bron-VM. Deze scripts kunnen worden gebruikt voor toepassingen installeren of de virtuele machine naar wens configureren. Als u wilt beveiligen referenties, kan gevoelige informatie, zoals wachtwoorden worden opgeslagen in een beveiligde configuratie. Deze referenties worden alleen ontsleuteld in de virtuele machine.

Leer hoe u het volgende doet:

- [Maak een Linux-VM met de Azure CLI en gebruiken van de aangepaste Scriptextensie](../articles/virtual-machines/scripts/virtual-machines-linux-cli-sample-create-vm-nginx.md?toc=%2fcli%2fazure%2ftoc.json).
- [Een virtuele Windows-machine maken met Azure PowerShell en gebruiken van de aangepaste Scriptextensie](../articles/virtual-machines/scripts/virtual-machines-windows-powershell-sample-create-vm-iis.md?toc=%2fpowershell%2fmodule%2ftoc.json).


## <a name="packer"></a>Verpakker
[Verpakker](https://www.packer.io) automatiseert de build wanneer u een aangepaste installatiekopie van de virtuele machine in Azure maakt. U verpakker gebruiken voor het definiëren van het besturingssysteem en na configuratie scripts uitvoeren die voor het aanpassen van de virtuele machine voor uw specifieke behoeften. Zodra geconfigureerd, wordt de virtuele machine vervolgens vastgelegd als een beheerde schijf-installatiekopie. Verpakker automatiseert het proces voor het maken van de bron VM-, netwerk-en opslagbronnen, configuratiescripts uitvoeren en maak vervolgens een installatiekopie van de virtuele machine.

Leer hoe u het volgende doet:

- [Verpakker gebruiken voor het maken van een Linux-VM-installatiekopie in Azure](../articles/virtual-machines/linux/build-image-with-packer.md).
- [Verpakker gebruiken voor het maken van een virtuele machine van Windows-installatiekopie in Azure](../articles/virtual-machines/windows/build-image-with-packer.md).


## <a name="terraform"></a>Terraform
[Terraform](https://www.terraform.io) is een automation-hulpprogramma waarmee u definieert en maakt u een volledige Azure-infrastructuur met een bepaalde sjabloon indelingstaal - de HashiCorp configuratie taal (lijst met compatibele hardware). Met Terraform definieert u de sjablonen die het proces voor het maken van netwerk-, opslag- en VM-netwerkbronnen voor een bepaalde toepassing oplossing automatiseren. U kunt uw bestaande Terraform sjablonen gebruiken voor andere platforms met Azure consistentie te garanderen en de implementatie van de infrastructuur vereenvoudigen zonder converteren naar een Azure Resource Manager-sjabloon.

Leer hoe u het volgende doet:

- [Installeer en configureer Terraform met Azure](../articles/virtual-machines/linux/terraform-install-configure.md).
- [Maken van een Azure-infrastructuur met Terraform](../articles/virtual-machines/linux/terraform-create-complete-vm.md).


## <a name="azure-automation"></a>Azure Automation
[Azure Automation](https://azure.microsoft.com/services/automation/) runbooks gebruikt om te verwerken van een set taken op de virtuele machines is gericht. Azure Automation wordt gebruikt voor het beheren van bestaande virtuele machines in plaats van te maken van een infrastructuur. Azure Automation kan worden uitgevoerd op zowel Linux en Windows-VM's, evenals on-premises virtuele of fysieke machines met een hybride runbook worker. Runbooks kunnen worden opgeslagen in een resourcebeheerbibliotheek zoals GitHub. Deze runbooks kunnen vervolgens handmatig uitvoeren of volgens een ingesteld schema.

Azure Automation biedt ook een Desired State Configuration (DSC)-service die u maken van de definities kunt voor hoe een bepaalde set van virtuele machines moet worden geconfigureerd. DSC zorgt er daarna dat de vereiste configuratie wordt toegepast en de virtuele machine consistent blijft. Azure Automation DSC wordt uitgevoerd op Windows- en Linux-machines.

Leer hoe u het volgende doet:

- [Maken van een PowerShell-runbook](../articles/automation/automation-first-runbook-textual-powershell.md).
- [Hybride Runbook Worker gebruiken om on-premises resources te beheren](../articles/automation/automation-hybrid-runbook-worker.md).
- [Gebruik Azure Automation DSC](../articles/automation/automation-dsc-getting-started.md).


## <a name="visual-studio-team-services"></a>Visual Studio Team Services
[Services in een team](https://www.visualstudio.com/team-services/) is een suite met hulpprogramma's die u helpen te delen en bijhouden code, geautomatiseerde builds gebruiken en een volledige continue integratie en ontwikkeling (CI/CD) pijplijn maken. Teamservices kan worden geïntegreerd met Visual Studio en andere editors gebruik vereenvoudigen. Teamservices kunnen ook maken en Azure VM's configureren en vervolgens implementeert code toe.

Leer hoe u het volgende doet:

- [Een continue integratie-pijplijn maken met het Team Services](../articles/virtual-machines/windows/tutorial-vsts-iis-cicd.md).


## <a name="jenkins"></a>Jenkins
[Jenkins](https://www.jenkins.io) is een continue integratie-server die u kunt implementeren en testen van toepassingen en geautomatiseerde pijplijnen voor de levering van code maken. Er zijn honderden invoegtoepassingen uit te breiden het Jenkins-kernplatform en u kunt ook integreren met veel andere producten en oplossingen via webhooks. U kunt handmatig Jenkins installeren op een virtuele machine in Azure, Jenkins van uitgevoerd binnen een Docker-container of een vooraf samengestelde Azure Marketplace-installatiekopie gebruiken.

Leer hoe u het volgende doet:

- [Een infrastructuur ontwikkeling maken op een Linux-VM in Azure met Jenkins, GitHub en Docker](../articles/virtual-machines/linux/tutorial-jenkins-github-docker-cicd.md).


## <a name="next-steps"></a>Volgende stappen
Er zijn diverse opties beschikbaar voor gebruik van infrastructuur automatiseringsprogramma's in Azure. U hebt de vrijheid welke oplossing het beste past bij uw behoeften en omgeving gebruiken. Om aan de slag en probeert enkele van de hulpprogramma's voor ingebouwde naar Azure, informatie over het automatiseren van het aanpassen van een [Linux](../articles/virtual-machines/linux/tutorial-automate-vm-deployment.md) of [Windows](../articles/virtual-machines/windows/tutorial-automate-vm-deployment.md) VM.
