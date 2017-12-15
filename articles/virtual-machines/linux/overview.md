---
title: Overzicht van virtuele Linux-machines in Azure | Microsoft Docs
description: Hier vindt u informatie over Azure Compute, Storage en netwerkservices voor gebruik met virtuele Linux-machines.
services: virtual-machines-linux
documentationcenter: virtual-machines-linux
author: rickstercdn
manager: timlt
editor: 
ms.assetid: 7965a80f-ea24-4cc2-bc43-60b574101902
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: overview
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/14/2016
ms.author: rclaus
ms.custom: H1Hack27Feb2017, mvc
ms.openlocfilehash: cef9abddf980c695040e99995eb325eeb182fad4
ms.sourcegitcommit: c7215d71e1cdeab731dd923a9b6b6643cee6eb04
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/17/2017
---
# <a name="azure-and-linux"></a>Azure en Linux
Microsoft Azure is een groeiende verzameling geïntegreerde, openbare cloudservices, waaronder analyses, VM's, databases, mobiele toepassingen, netwerken, opslag en het web&mdash;ideaal voor het hosten van uw oplossingen.  Microsoft Azure biedt een schaalbaar platform waarvoor u alleen betaalt voor wat u gebruikt, wanneer u dat gebruikt - zonder dat u hoeft te investeren in on-premises hardware.  Azure is klaar om uw oplossingen op te schalen wanneer u daar ook klaar, welke schaal u ook nodig hebt om te voorzien in de behoeften van uw klanten.

Als u bekend met de verschillende functies van AWS van Amazon, kunt u het document [AWS voor vergelijking van Azure-services](https://azure.microsoft.com/campaigns/azure-vs-aws/mapping/) doornemen.

## <a name="regions"></a>Regio's
Resources van Microsoft Azure worden verdeeld over verschillende geografische regio's over de hele wereld.  Een 'regio' vertegenwoordigt meerdere datacenters in één geografisch gebied.  We hebben 34 regio's die algemeen beschikbaar zijn, verspreid over de hele wereld, en er komen nog eens vier regio's bij. Omdat we onze wereldwijde dekking steeds verder willen uitbreiden, wordt de lijst met bestaande en nieuw aangekondigd regio's steeds bijgewerkt.

* [Azure-regio's](https://azure.microsoft.com/regions/)

## <a name="availability"></a>Beschikbaarheid
We hebben een toonaangevende serviceovereenkomst (SLA) van 99,9% aangekondigd voor één instantie van VM's, met als enige voorwaarde dat de virtuele machine wordt geïmplementeerd met premium-opslag voor alle schijven.  Om uw implementatie van VM's in aanmerking te laten komen voor onze SLA van 99,95%, moet u minimaal twee of meer VM's waarop uw workload wordt uitgevoerd, implementeren binnen een beschikbaarheidsset. Dit zorgt ervoor dat uw VM's worden verdeeld over meerdere foutdomeinen in onze datacenters en worden geïmplementeerd op hosts met verschillende onderhoudsvensters. In de volledige [Azure SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_0/) wordt de gegarandeerde beschikbaarheid van Azure als geheel uitgelegd.

## <a name="managed-disks"></a>Beheerde schijven

Managed Disks beheert het maken/beheren van Azure Storage-accounts op de achtergrond en zorgt ervoor dat u zich geen zorgen hoeft te maken over de schaalbaarheidslimieten van het opslagaccount. U hoeft alleen maar de schijfgrootte en prestatielaag (Standard of Premium) op te geven en Azure maakt en beheert de schijf voor u. Zelfs als u schijven toevoegt of de virtuele machine omhoog of omlaag schaalt, hoeft u zich geen zorgen te maken over de gebruikte opslag. Als u nieuwe VM's gaat maken, [gebruik dan de Azure CLI 2.0](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) of Azure Portal om VM's te maken met een beheerd besturingssysteem en beheerde gegevensschijven. Als u VM's hebt met niet-beheerde schijven, kunt u [deze converteren om ze te ondersteunen met Managed Disks](convert-unmanaged-to-managed-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

U kunt ook uw aangepaste installatiekopieën in één opslagaccount per Azure-regio beheren en deze gebruiken om honderden virtuele machines onder hetzelfde abonnement te maken. Zie [Overzicht van Managed Disks](../windows/managed-disks-overview.md) voor meer informatie over Managed Disks.

## <a name="azure-virtual-machines--instances"></a>Virtuele Azure-machines en instanties
Microsoft Azure ondersteunt de uitvoering van een aantal populaire Linux-distributies die worden geleverd en onderhouden door een aantal partners.  In de Microsoft Azure Marketplace vindt u onder andere distributies zoals Red Hat Enterprise, CentOS, SUSE Linux Enterprise, Debian, Ubuntu, CoreOS, RancherOS en FreeBSD. We werken actief samen met verschillende Linux-community's om zelfs nog meer varianten toe te voegen aan [de lijst met Linux-distributies die zijn goedgekeurd voor Azure](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Als de Linux-distributie van uw keuze niet op de lijst staat, kunt u zelf een Linux-VM maken ('Bring Your Own Linux') door [een Linux-VHD te maken en uploaden in Azure](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Met virtuele Azure-machines kunt u een brede reeks computeroplossingen op flexibele wijze inzetten. U kunt vrijwel elke werkbelasting en elke taal op vrijwel ieder besturingssysteem implementeren: Windows, Linux of een aangepaste variant die wordt aangeboden door organisaties uit de groeiende lijst met partners. Nog steeds niet gevonden wat u zoekt?  Geen probleem. U kunt ook uw eigen on-premises installatiekopieën gebruiken.

## <a name="vm-sizes"></a>VM-grootten
Wanneer u een VM of virtuele machine implementeert in Azure, moet u uit ons aanbod van VM-grootten een grootte selecteren die geschikt is voor uw workload. De grootte heeft ook invloed op de verwerkingskracht, het geheugen en de opslagcapaciteit van de virtuele machine. U wordt gefactureerd op basis van de hoeveelheid tijd die de VM actief is en de toegewezen bronnen verbruikt. Hier vindt u [een volledige lijst met grootten voor virtuele machines](sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Hier volgen enkele algemene richtlijnen voor het selecteren van een VM-grootte uit een van onze series (A, D, DS, G en GS).
* VM's uit de A-serie zijn onze instapmodellen: VM's voor lichte workloads en scenario's voor ontwikkelen en testen. Deze machines zijn algemeen beschikbaar in alle regio's en kunnen worden verbonden met alle standaardresources die beschikbaar zijn voor virtuele machines.
* De grootten uit de A-serie (A8 - A11) zijn speciale, rekenintensieve configuraties die geschikt zijn voor high-performance computing-clustertoepassingen.
* Virtuele machines uit de D-serie zijn ontworpen voor het uitvoeren van toepassingen die meer rekenvermogen en tijdelijke schijfprestaties vereisen. Virtuele machines uit de D-serie hebben snellere processors, een hogere geheugen-naar-vCPU-snelheid en een SSD (solid-state drive) voor de tijdelijke schijf.
* De Dv2-serie is de meest recente versie van onze D-serie en biedt een krachtigere CPU. De CPU van de Dv2-serie is ongeveer 35% sneller dan de CPU van de D-serie. Deze is gebaseerd op de nieuwste generatie Intel Xeon® E5-2673 v3-processor van 2,4 GHz (Haskell). Met Intel Turbo Boost Technology 2.0 kunnen ze maar liefst 3,2 GHz bereiken. De Dv2-serie heeft dezelfde geheugen- en schijfconfiguraties als de D-serie.
* Virtuele machines uit de G-serie bieden het meeste geheugen en worden uitgevoerd op hosts met een processor uit de Intel Xeon E5 V3-familie.

Opmerking: virtuele machines uit de DS- en GS-serie hebben toegang tot Premium Storage, onze opslag met SSD-schijven met hoge prestaties en lage latentie voor I/O-intensieve workloads. Premium Storage is beschikbaar in bepaalde regio's. Zie deze artikelen voor meer informatie:

* [Premium Storage: opslag met hoge prestaties voor de workloads van virtuele Azure-machines](../windows/premium-storage.md)

## <a name="automation"></a>Automatisering
Voor een juiste DevOps-cultuur moet alle infrastructuur uit code bestaan.  Als alle infrastructuur bestaat uit code, kan deze namelijk eenvoudig opnieuw worden opgebouwd (Phoenix Servers).  Azure werkt met de belangrijkste automatiseringstools zoals Ansible, Chef, SaltStack en Puppet.  Azure heeft ook eigen tools voor automatisering:

* [Azure-sjablonen](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Azure VMAccess](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Azure is bezig met het uitrollen van ondersteuning voor [cloud init](http://cloud-init.io/) voor de meeste Linux-distributies die dit ondersteunen.  Op dit moment worden VM's met Ubuntu van Canonical geïmplementeerd met cloud-init standaard ingeschakeld.  Red Hats RHEL, CentOS en Fedora ondersteunen cloud init, maar cloud-init wordt niet geïnstalleerd met de installatiekopieën van Azure die worden onderhouden door RedHat.  Als u cloud-init wilt gebruiken met een OS uit de RedHat-familie, moet u een aangepaste installatiekopie maken met cloud-init geïnstalleerd.

* [Cloud-init gebruiken op virtuele Linux-machines in Azure](using-cloud-init.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="quotas"></a>Quota
Elk Azure-abonnement heeft een standaard-quotumlimiet die van invloed kan zijn op de implementatie van een groot aantal VM’s voor uw project. De huidige limiet per abonnement is 20 VM's per regio.  De quotalimiet kan snel en gemakkelijk worden verhoogd door het indienen van een ondersteuningsticket met het verzoek om de limiet te verhogen.  Meer informatie over quotalimieten vindt u hier:

* [Azure-abonnement en servicelimieten, quota's en beperkingen](../../azure-subscription-service-limits.md)

## <a name="partners"></a>Partners
Microsoft werkt nauw samen met onze partners om ervoor te zorgen dat de beschikbare installatiekopieën worden bijgewerkt en geoptimaliseerd voor een Azure-runtime.  Meer informatie over onze partners kunt u vinden op hun pagina's in de Marketplace.

* [Linux op door Azure goedgekeurde distributies](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* SUSE - [Azure Marketplace - SUSE Linux Enterprise Server](https://azuremarketplace.microsoft.com/en-us/marketplace/apps?search=%27SUSE%27)
* Redhat - [Azure Marketplace - RedHat Enterprise Linux 7.2](https://azure.microsoft.com/marketplace/partners/redhat/redhatenterpriselinux72/)
* Canonical - [Azure Marketplace - Ubuntu Server 16.04 LTS](https://azure.microsoft.com/marketplace/partners/canonical/ubuntuserver1604lts/)
* Debian - [Azure Marketplace - Debian 8 "Jessie"](https://azure.microsoft.com/marketplace/partners/credativ/debian8/)
* FreeBSD - [Azure Marketplace - FreeBSD 10.3](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd103/)
* CoreOS - [Azure Marketplace - CoreOS (Stable)](https://azure.microsoft.com/marketplace/partners/coreos/coreosstable/)
* RancherOS - [Azure Marketplace - RancherOS](https://azure.microsoft.com/marketplace/partners/rancher/rancheros/)
* Bitnami - [Bitnami Library voor Azure](https://azure.bitnami.com/)
* Mesosphere - [Azure Marketplace - Mesosphere DC/OS voor Azure](https://azure.microsoft.com/marketplace/partners/mesosphere/dcosdcos/)
* Docker - [Azure Marketplace - Azure Container Service met Docker Swarm](https://azure.microsoft.com/marketplace/partners/microsoft/acsswarms/)
* Jenkins - [Azure Marketplace - CloudBees Jenkins Platform](https://azure.microsoft.com/marketplace/partners/cloudbees/jenkins-platformjenkins-platform/)

## <a name="getting-started-with-linux-on-azure"></a>Aan de slag met Linux in Azure
Om aan de slag te gaan met Azure, hebt u het volgende nodig: een Azure-account, Azure CLI en een combinatie van een openbare en persoonlijke SSH-sleutel.

### <a name="sign-up-for-an-account"></a>Registreren voor een account
Het eerste wat u moet doen, is een Azure-account maken.  Ga naar [deze pagina](https://azure.microsoft.com/pricing/free-trial/) om direct te beginnen.

### <a name="install-the-cli"></a>De CLI installeren
U kunt uw nieuwe Azure-account direct gebruiken via Azure Portal ,een beheerinterface die via een browser werkt.  Als u de Azure-cloud wilt beheren via de opdrachtregel, installeert u de `azure-cli`.  Installeer de [Azure CLI 2.0](/cli/azure/install-azure-cli) op uw Mac of Linux-werkstation.

### <a name="create-an-ssh-key-pair"></a>Een SSH-sleutelpaar maken
U hebt nu toegang tot een Azure-account, Azure Portal en de Azure CLI.  De volgende stap is het maken van een SSH-sleutelpaar dat wordt gebruikt om via SSH toegang te krijgen tot Linux zonder een wachtwoord te gebruiken.  [Maak SSH-sleutels in Linux en Mac OS](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) om aanmelding zonder wachtwoord mogelijk te maken en betere beveiliging in te stellen.

### <a name="create-a-vm-using-the-cli"></a>Een VM maken via CLI
Het maken van een Linux-VM met de CLI is een snelle manier om een virtuele machine te implementeren zonder de terminal te verlaten waarin u werkt.  Alles wat u kunt opgeven in Azure Portal is op de opdrachtregel beschikbaar via een vlag of schakeloptie.  

* [Een Linux-VM maken met de CLI](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="create-a-vm-in-the-portal"></a>Een VM maken in de portal
Als u ervoor kiest om een Linux-VM te maken in Azure Portal, kunt u eenvoudig allerlei opties bekijken en zo tot de gewenste implementatie komen.  In plaats van vlaggen of schakelopties in te voeren op de opdrachtregel, kunt u in een overzichtelijke interface de verschillende opties en instellingen weergeven en selecteren.  Alles wat beschikbaar is via de opdrachtregelinterface, is ook beschikbaar in de portal.

* [Een Linux-VM maken met de portal](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="login-using-ssh-without-a-password"></a>Aanmelden met SSH zonder wachtwoord
De VM wordt nu uitgevoerd in Azure en u kunt zich gaan aanmelden.  Aanmelden via SSH met behulp van wachtwoorden is onveilig en tijdrovend.  Het gebruik van SSH-sleutels is niet alleen de veiligste manier om u aan te melden, maar ook de snelste manier.  Wanneer u een Linux-VM maakt via de portal of de CLI, kunt u kiezen uit twee verificatiemogelijkheden.  Als u een wachtwoord kiest voor SSH, wordt de virtuele machine zo geconfigureerd door Azure dat aanmeldingen via wachtwoorden zijn toegestaan.  Als u kiest voor een openbare SSH-sleutel, wordt de virtuele machine zo geconfigureerd dat alleen aanmeldingen via SSH-sleutels zijn toegestaan. Aanmelden met behulp van een wachtwoord is dan niet mogelijk. Als u uw Linux-VM wilt beveiligen door alleen aanmeldingen met een SSH-sleutel toe te staan, gebruikt u de optie voor openbare SSH-sleutels tijdens het maken van de virtuele machine in de portal of de CLI.

## <a name="related-azure-components"></a>Gerelateerde Azure-onderdelen
## <a name="storage"></a>Storage
* [Inleiding tot Microsoft Azure Storage](../../storage/common/storage-introduction.md)
* [Een schijf toevoegen aan een virtuele Linux-machine](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Een gegevensschijf koppelen aan een Linux-VM in Azure Portal](attach-disk-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="networking"></a>Netwerken
* [Overzicht van Virtual Network](../../virtual-network/virtual-networks-overview.md)
* [IP-adressen in Azure](../../virtual-network/virtual-network-ip-addresses-overview-arm.md)
* [Poorten openen voor een Linux-VM in Azure](nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Een Fully Qualified Domain Name maken in Azure Portal](portal-create-fqdn.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="containers"></a>Containers
* [Virtuele machines en containers in Azure](containers.md)
* [Kennismaking met Azure Container Service](../../container-service/container-service-intro.md)
* [Een Azure Container Service-cluster implementeren](../../container-service/dcos-swarm/container-service-deployment.md)

## <a name="next-steps"></a>Volgende stappen
U hebt nu een algemeen beeld van Linux in Azure.  De volgende stap is om zelf aan de slag te gaan en een paar virtuele machines te maken.

* [Lijst met voorbeelden van scripts voor algemene taken via Azure CLI](cli-samples.md)
