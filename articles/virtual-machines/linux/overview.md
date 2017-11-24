---
title: Overzicht van Linux virtuele machines in Azure | Microsoft Docs
description: Beschrijving van Azure Compute, opslag en netwerken services met Linux virtuele machines.
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
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/17/2017
---
# <a name="azure-and-linux"></a>Azure en Linux
Microsoft Azure is een groeiende verzameling geïntegreerde openbare cloud services analytics, virtuele Machines, databases, mobiel, netwerken, opslag, waaronder en web&mdash;ideaal voor het hosten van uw oplossingen.  Microsoft Azure biedt een schaalbaar platform waarvoor u alleen betaalt voor wat u gebruikt, wanneer u dat gebruikt - zonder dat u hoeft te investeren in on-premises hardware.  Azure is klaar om uw oplossingen op te schalen wanneer u daar ook klaar, welke schaal u ook nodig hebt om te voorzien in de behoeften van uw klanten.

Als u bekend met de verschillende functies van Amazon bent AWS, kunt u de Azure vs AWS bestuderen [definitie toewijzing document](https://azure.microsoft.com/campaigns/azure-vs-aws/mapping/).

## <a name="regions"></a>Regio's
Microsoft Azure-resources worden gedistribueerd over meerdere geografische regio's over de hele wereld.  Een 'regio' geeft meerdere datacenters in één geografisch gebied.  We hebben 34 regio's, algemeen beschikbaar overal ter wereld met een extra 4 gebieden aangekondigd. Omdat we onze wereldwijde dekking - Vouw blijven onderhouden we dat een bijgewerkte lijst met bestaande en nieuwe aangekondigd regio's.

* [Azure-regio's](https://azure.microsoft.com/regions/)

## <a name="availability"></a>Beschikbaarheid
We een bedrijfstak toonaangevende één exemplaar virtuele machine van de serviceovereenkomst van 99,9% opgegeven implementeren van de virtuele machine met premium-opslag voor alle schijven aangekondigd.  In de volgorde voor uw implementatie in aanmerking komt voor onze standaard 99,95% VM Service Level Agreement, moet u nog steeds twee of meer virtuele machines die de werkbelasting binnen een beschikbaarheidsset implementeren. Dit zorgt ervoor uw virtuele machines zijn verdeeld over meerdere domeinen met fouten in onze datacenters, evenals geïmplementeerd op hosts met verschillende onderhoudsvensters. In de volledige [Azure SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_0/) wordt de gegarandeerde beschikbaarheid van Azure als geheel uitgelegd.

## <a name="managed-disks"></a>Beheerde schijven

Schijven ingangen Azure Storage-account maken en beheren op de achtergrond voor u en zorgt ervoor dat er geen zorgen te hoeven maken over de limieten voor schaalbaarheid van het opslagaccount die worden beheerd. U gewoon de schijfgrootte en de prestatielaag (Standard of Premium) opgeven en Azure maken en beheren van de schijf voor u. Zelfs als u schijven toevoegt of de virtuele machine omhoog of omlaag schaalt, hoeft u zich geen zorgen te maken over de gebruikte opslag. Als u nieuwe virtuele machines, maakt [gebruik van de Azure CLI 2.0](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) of Azure portal maken van virtuele machines met beheerde besturingssysteem en gegevensschijven. Als u virtuele machines met niet-beheerde schijven hebt, kunt u [converteren van uw virtuele machines worden ondersteund met schijven beheerd](convert-unmanaged-to-managed-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

U kunt ook uw aangepaste installatiekopieën in één opslagaccount per Azure-regio beheren en deze gebruiken om honderden virtuele machines onder hetzelfde abonnement te maken. Zie [Overzicht van Managed Disks](../windows/managed-disks-overview.md) voor meer informatie over Managed Disks.

## <a name="azure-virtual-machines--instances"></a>Virtuele Machines in Azure & exemplaren
Microsoft Azure ondersteunt de uitvoering van een aantal populaire Linux-distributies geleverd en wordt onderhouden door een aantal partners.  U vindt distributies zoals Red Hat Enterprise, CentOS, SUSE Linux Enterprise, Debian, Ubuntu, virtuele CoreOS, RancherOS, FreeBSD en meer in Azure Marketplace. We actief werken met verschillende Linux-community's om toe te voegen nog meer versies voor de [door Azure goedgekeurde Linux Distros](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) lijst.

Als uw voorkeur Linux distro keuze niet momenteel aanwezig is in de galerie, u kunt 'Bring your own Linux' VM door [maakt en uploadt u een Linux-VHD in Azure](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Virtuele machines van Azure kunt u een breed scala aan oplossingen computergebruik in een flexibele manier te implementeren. U kunt vrijwel elke werkbelasting en een andere taal op bijna elk besturingssysteem - Windows, Linux, implementeren of een aangepaste gemaakt bij een van onze groeiende lijst met partners. Nog niet wordt weergegeven wat u zoekt?  U hoeft niet - u kunt ook uw eigen installatiekopieën zetten van on-premises.

## <a name="vm-sizes"></a>VM-grootten
Wanneer u een virtuele machine in Azure implementeert, gaat u een VM-grootte binnen één van onze reeks grootten die geschikt is voor uw workload selecteren. De grootte zijn ook van invloed op de verwerkingscapaciteit voor voeding, geheugen en opslag van de virtuele machine. U wordt gefactureerd op basis van de hoeveelheid tijd op de virtuele machine wordt uitgevoerd en de toegewezen bronnen verbruiken. Een volledige lijst met [grootten van virtuele Machines](sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Hier volgen enkele algemene richtlijnen voor het selecteren van een VM-grootte van een van onze reeks (A, D, DS, G en GS).
* A-serie VM's zijn onze waarde prijs instapmodellen VMs voor lichte werkbelastingen en scenario's voor ontwikkelen en testen. Ze kunnen algemeen beschikbaar zijn in alle regio's en verbinding maken met en gebruik van alle standaard bronnen beschikbaar voor virtuele machines.
* A-serie grootten (A8 - A11) zijn speciale berekenings-intensieve configuraties geschikt is voor high-performance computing clustertoepassingen.
* Virtuele machines uit de D-serie zijn ontworpen voor het uitvoeren van toepassingen die meer rekenvermogen en tijdelijke schijfprestaties vereisen. Virtuele machines uit de D-serie hebben snellere processors, een hogere geheugen-naar-vCPU-snelheid en een SSD (solid-state drive) voor de tijdelijke schijf.
* Dv2-serie, is de meest recente versie van onze D-reeks, biedt een krachtige CPU. De CPU van de Dv2-serie is ongeveer 35% sneller dan de CPU van de D-serie. Deze is gebaseerd op de nieuwste 2,4 GHz Intel Xeon® E5-2673 v3-processor (Haskell), en met de 2.0-technologie van Intel Turbo versterking 3,2 GHz kunt gaan. De Dv2-serie heeft dezelfde geheugen- en schijfconfiguraties als de D-serie.
* Virtuele machines uit de G-serie bieden het meeste geheugen en worden uitgevoerd op hosts met een processor uit de Intel Xeon E5 V3-familie.

Opmerking: DS-serie en GS-serie virtuele machines toegang hebben tot Premium-opslag - onze SSD back opslag voor hoge prestaties, lage latentie voor i/o-intensieve werkbelastingen. Premium Storage is beschikbaar in bepaalde regio's. Zie deze artikelen voor meer informatie:

* [Premium-opslag: Krachtige opslag voor workloads van de virtuele machine van Azure](../windows/premium-storage.md)

## <a name="automation"></a>Automatisering
Om te zorgen voor een juiste DevOps cultuur, moeten alle infrastructuur code.  Wanneer alle de infrastructuur woont in code die deze gemakkelijk kan worden gemaakt (Phoenix Servers).  Azure werkt met de belangrijkste automatisering tooling zoals Ansible, Chef, SaltStack en Puppet.  Azure heeft ook een eigen tooling voor automation:

* [Azure-sjablonen](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Azure VMAccess](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Azure-ondersteuning voor rolt [cloud init](http://cloud-init.io/) in de meeste Linux Distros die dit ondersteunen.  Op dit moment zijn van Canonical Ubuntu virtuele machines geïmplementeerd met cloud-init zijn standaard ingeschakeld.  Rood hoeden RHEL, CentOS en Fedora ondersteunen cloud init, maar de installatiekopieën van het Azure onderhouden met RedHat hebt niet cloud-init zijn geïnstalleerd.  Voor het gebruik van cloud-init op een serie RedHat OS, moet u een aangepaste installatiekopie maken met cloud-init zijn geïnstalleerd.

* [Met behulp van cloud-init op Azure Linux VM 's](using-cloud-init.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="quotas"></a>Quota
Elke Azure-abonnement heeft standaard de quotalimieten dat kan invloed hebben op de implementatie van een groot aantal virtuele machines voor uw project. De huidige limiet per abonnement is 20 VM's per regio.  De quotalimieten snel kunnen worden verhoogd en gemakkelijk door het indienen van een ondersteuningsticket aanvragen van een limiet verhogen.  Voor meer informatie over de quotalimieten voor:

* [Servicelimieten voor Azure-abonnement](../../azure-subscription-service-limits.md)

## <a name="partners"></a>Partners
Microsoft werkt samen met onze partners om te controleren of de beschikbare installatiekopieën zijn bijgewerkt en geoptimaliseerd voor een Azure-runtime.  Controleer de marketplace pagina's hieronder voor meer informatie over onze partners.

* Linux op Azure - [goedgekeurde distributies](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* SUSE - [Azure Marketplace - SUSE Linux Enterprise Server](https://azuremarketplace.microsoft.com/en-us/marketplace/apps?search=%27SUSE%27)
* RedHat - [Azure Marketplace - Red Hat Enterprise Linux 7.2](https://azure.microsoft.com/marketplace/partners/redhat/redhatenterpriselinux72/)
* Canonieke - [Azure Marketplace - Ubuntu Server 16.04 TNS](https://azure.microsoft.com/marketplace/partners/canonical/ubuntuserver1604lts/)
* Debian - [Azure Marketplace - Debian 8 'Jessie'](https://azure.microsoft.com/marketplace/partners/credativ/debian8/)
* FreeBSD - [Azure Marketplace - FreeBSD 10.3](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd103/)
* Virtuele CoreOS - [Azure Marketplace - virtuele CoreOS (stabiel)](https://azure.microsoft.com/marketplace/partners/coreos/coreosstable/)
* RancherOS - [Azure Marketplace - RancherOS](https://azure.microsoft.com/marketplace/partners/rancher/rancheros/)
* Bitnami - [Bitnami-bibliotheek voor Azure](https://azure.bitnami.com/)
* Mesosphere - [Azure Marketplace - Mesosphere DC/OS op Azure](https://azure.microsoft.com/marketplace/partners/mesosphere/dcosdcos/)
* Docker - [Azure Marketplace - Azure Containerservice met Docker Swarm](https://azure.microsoft.com/marketplace/partners/microsoft/acsswarms/)
* Jenkins - [Azure Marketplace - CloudBees Jenkins Platform](https://azure.microsoft.com/marketplace/partners/cloudbees/jenkins-platformjenkins-platform/)

## <a name="getting-started-with-linux-on-azure"></a>Aan de slag met Linux op Azure
U moet een Azure-account, de Azure CLI geïnstalleerd en een paar SSH openbare en persoonlijke sleutels om te beginnen met Azure.

### <a name="sign-up-for-an-account"></a>Registreren voor een account
De eerste stap bij het gebruik van de Azure-Cloud is aanmelden voor een Azure-account.  Ga naar de [Azure accountaanmelding](https://azure.microsoft.com/pricing/free-trial/) pagina aan de slag.

### <a name="install-the-cli"></a>De CLI installeren
Met uw nieuwe Azure-account, u kunt aan de slag onmiddellijk met de Azure portal een webgebaseerde admin-venster is.  Voor het beheren van de Azure-Cloud via de opdrachtregel, die u installeert de `azure-cli`.  Installeer de [Azure CLI 2.0](/cli/azure/install-azure-cli) op uw Mac of Linux-werkstation.

### <a name="create-an-ssh-key-pair"></a>Een SSH-sleutelpaar maken
U hebt nu een Azure-account, de Azure-web-portal en de Azure CLI.  De volgende stap is het maken van een SSH-sleutelpaar dat wordt gebruikt voor SSH in Linux zonder een wachtwoord te gebruiken.  [SSH-sleutels maken in Linux en Mac](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) wachtwoordloze aanmelding en betere beveiliging.

### <a name="create-a-vm-using-the-cli"></a>Een VM maken via CLI
Maken van een Linux-VM met de CLI is een snelle manier voor het implementeren van een virtuele machine zonder de terminal die in werkt.  Alles wat die u op het webportaal opgeven kunt is beschikbaar via een opdrachtregel vlag of de switch.  

* [Maak een Linux-VM met CLI](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="create-a-vm-in-the-portal"></a>Een virtuele machine maken in de portal
Maken van een Linux-VM in de Azure-web-portal is een manier om eenvoudig en klik op via de verschillende opties om een implementatie.  In plaats van opdrachtregelprogramma vlaggen of switches, bent u een nice web-indeling van de verschillende opties en instellingen weergeven.  Alles beschikbaar via de opdrachtregelinterface is ook beschikbaar in de portal.

* [Maak een Linux-VM met de Portal](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="login-using-ssh-without-a-password"></a>Aanmelding via SSH zonder wachtwoord
De virtuele machine wordt nu uitgevoerd in Azure en u bent klaar om aan te melden.  Wachtwoorden gebruiken om aan te melden via SSH is onveilig en tijd in beslag neemt.  Gebruik van SSH-sleutels is de veiligste manier en ook de snelste manier om aan te melden.  Wanneer u u Linux VM via de portal of de CLI maakt, hebt u twee opties voor verificatie.  Als u een wachtwoord voor SSH kiest, configureert Azure de virtuele machine zodat aanmeldingen via wachtwoorden.  Als u kiest voor het gebruik van een openbare SSH-sleutel, wordt Azure configureert u de virtuele machine zodat alleen aanmeldingen via SSH-sleutels en aanmeldingen wachtwoord wordt uitgeschakeld. Als u wilt beveiligen uw Linux-VM met alleen een SSH-sleutel aanmeldingen toe te staan, gebruiken de openbare SSH-sleuteloptie tijdens het maken van de virtuele machine in de portal of de CLI.

## <a name="related-azure-components"></a>Gerelateerde Azure-onderdelen
## <a name="storage"></a>Storage
* [Inleiding tot Microsoft Azure Storage](../../storage/common/storage-introduction.md)
* [Een schijf toevoegen aan een Linux-VM met de azure-cli](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Hoe u een gegevensschijf koppelen aan een Linux-VM in de Azure portal](attach-disk-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="networking"></a>Netwerken
* [Overzicht van Virtual Network](../../virtual-network/virtual-networks-overview.md)
* [IP-adressen in Azure](../../virtual-network/virtual-network-ip-addresses-overview-arm.md)
* [Openen van poorten voor een Linux-VM in Azure](nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Maken van een volledig gekwalificeerde domeinnaam in de Azure portal](portal-create-fqdn.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="containers"></a>Containers
* [Virtuele Machines en Containers in Azure](containers.md)
* [Azure Container Service Inleiding](../../container-service/container-service-intro.md)
* [Een Azure Container Service-cluster implementeren](../../container-service/dcos-swarm/container-service-deployment.md)

## <a name="next-steps"></a>Volgende stappen
U hebt nu een overzicht van Linux in Azure.  De volgende stap is het zelf aan de slag en maakt u een paar virtuele machines.

* [Bekijk onze groeiende lijst met voorbeelden van Scripts voor algemene taken via AzureCLI](cli-samples.md)
