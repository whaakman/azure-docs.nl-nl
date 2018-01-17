---
title: SAP NetWeaver op Microsoft Azure SUSE Linux VM's testen | Microsoft Docs
description: SAP NetWeaver testen op Microsoft Azure SUSE Linux VM's
services: virtual-machines-linux
documentationcenter: 
author: hermanndms
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: 645e358b-3ca1-4d3d-bf70-b0f287498d7a
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 09/14/2017
ms.author: hermannd
ms.openlocfilehash: 072a70c1da74b3b50ad8c0a93ee3c079a724d81f
ms.sourcegitcommit: a0d2423f1f277516ab2a15fe26afbc3db2f66e33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2018
---
# <a name="running-sap-netweaver-on-microsoft-azure-suse-linux-vms"></a>SAP NetWeaver uitvoeren op Microsoft Azure SUSE Linux VM's
In dit artikel beschrijft de verschillende overwegingen wanneer u SAP NetWeaver op virtuele Microsoft Azure SUSE Linux-machines (VM's) uitvoert. SAP NetWeaver wordt vanaf 19 mei 2016 officieel ondersteund in SUSE Linux virtuele machines in Azure. Alle gegevens met betrekking tot Linux-versies, SAP kernel-versies en andere vereisten vindt u in SAP-notitie 1928533 ' SAP-toepassingen in Azure: ondersteunde producten en Azure VM typen '.
Aanvullende documentatie over SAP op Linux VM's vindt u hier: [SAP op Linux virtuele machines (VM's) met behulp van](get-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Met behulp van de volgende informatie moet u een aantal mogelijke valkuilen voorkomen.

## <a name="suse-images-on-azure-for-running-sap"></a>Installatiekopieën van SUSE op Azure voor het uitvoeren van SAP
Voor het uitvoeren van SAP NetWeaver op Azure, SUSE Linux Enterprise Server SLES 12 (SPx) of SLES gebruiken voor SAP - Zie ook SAP-notitie 1928533. Een speciale SUSE installatiekopie in Azure Marketplace ('SLES 11 SP3 voor SAP CAL'), maar de installatiekopie is niet bedoeld voor algemeen gebruik. Gebruik geen deze installatiekopie omdat deze bestemd voor de [SAP-Cloudbibliotheek toestel](https://cal.sap.com/) oplossing.  

U moet het framework van de implementatie van Azure Resource Manager gebruiken voor alle installaties op Azure. Om te zoeken naar SUSE SLES installatiekopieën en versies met Azure PowerShell of de Azure-opdrachtregelinterface (CLI), gebruikt u de opdrachten die hieronder wordt weergegeven. U kunt vervolgens de uitvoer bijvoorbeeld gebruiken voor het definiëren van de installatiekopie van het besturingssysteem in een JSON-sjabloon voor het implementeren van een nieuwe SUSE Linux VM.
Deze PowerShell-opdrachten zijn geldig voor Azure PowerShell-versie 1.0.1 of hoger.

Het is nog steeds mogelijk met de standaard SLES-installatiekopieën voor SAP-installaties, verdient het aanbeveling gebruik te maken van de nieuwe SLES voor SAP-installatiekopieën. Deze installatiekopieën zijn nu beschikbaar in de afbeelding voor Azure-galerie. Meer informatie over deze installatiekopieën vindt u op de bijbehorende [Azure Marketplace pagina]( https://azuremarketplace.microsoft.com/en-us/marketplace/apps/SUSE.SLES-SAP ) of de [webpagina SUSE Veelgestelde vragen over SLES voor SAP]( https://www.suse.com/products/sles-for-sap/frequently-asked-questions/ ).


* Zoek naar de bestaande uitgevers, met inbegrip van SUSE:
  
   ```
   PS  : Get-AzureRmVMImagePublisher -Location "West Europe"  | where-object { $_.publishername -like "*US*"  }
   CLI : azure vm image list-publishers westeurope | grep "US"
   ```
* Zoeken naar bestaande aanbiedingen van SUSE:
  
   ```
   PS  : Get-AzureRmVMImageOffer -Location "West Europe" -Publisher "SUSE"
   CLI : azure vm image list-offers westeurope SUSE
   ```
* Zoek naar de offerings SUSE SLES:
  
   ```
   PS  : Get-AzureRmVMImageSku -Location "West Europe" -Publisher "SUSE" -Offer "SLES"
   PS  : Get-AzureRmVMImageSku -Location "West Europe" -Publisher "SUSE" -Offer "SLES-SAP"
   CLI : azure vm image list-skus westeurope SUSE SLES
   CLI : azure vm image list-skus westeurope SUSE SLES-SAP
   ```
* Zoek naar een specifieke versie van een SKU SLES:
  
   ```
   PS  : Get-AzureRmVMImage -Location "West Europe" -Publisher "SUSE" -Offer "SLES" -skus "12-SP2"
   PS  : Get-AzureRmVMImage -Location "West Europe" -Publisher "SUSE" -Offer "SLES-SAP" -skus "12-SP2"
   CLI : azure vm image list westeurope SUSE SLES 12-SP2
   CLI : azure vm image list westeurope SUSE SLES-SAP 12-SP2
   ```

## <a name="installing-walinuxagent-in-a-suse-vm"></a>WALinuxAgent installeren in een VM SUSE
De agent genoemd WALinuxAgent maakt deel uit van de installatiekopieën SLES in Azure Marketplace. Zie voor informatie over het installeren van deze handmatig (bijvoorbeeld tijdens het uploaden van een besturingssysteem SLES virtuele harde schijf (VHD) on-premises):

* [OpenSUSE](http://software.opensuse.org/package/WALinuxAgent)
* [Azure](../../linux/endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [SUSE](https://www.suse.com/communities/blog/suse-linux-enterprise-server-configuration-for-windows-azure/)

## <a name="sap-enhanced-monitoring"></a>SAP 'uitgebreide bewaking'
SAP 'Verbeterde aan bewaking' is een verplichte vereiste SAP uitvoeren op Azure. Controleer de details in SAP Houd er rekening mee 2191498 'SAP op Linux met Azure: verbeterde Monitoring'.

## <a name="attaching-azure-data-disks-to-an-azure-linux-vm"></a>Azure gegevensschijven koppelen aan een Azure Linux VM
Nooit gegevensschijven koppelen Azure naar een Azure Linux virtuele machine met behulp van de apparaat-ID. Gebruik in plaats daarvan de universally unique identifier (UUID). Wees voorzichtig wanneer u grafische hulpprogramma's voor gegevensschijven koppelen Azure, bijvoorbeeld gebruiken. Controleer de vermeldingen in /etc/fstab.

Het probleem met de apparaat-ID is dat kan worden gewijzigd en vervolgens de virtuele machine in Azure tijdens het opstarten kan vastlopen. Om te beperken van het probleem, kunt u de parameter nofail in /etc/fstab toevoegen. Maar wees voorzichtig met nofail omdat toepassingen voordat het koppelpunt wordt gehost als gebruiken kunnen en naar het root-bestandssysteem schrijft als een externe Azure-gegevensschijf tijdens het opstarten is niet gekoppeld.

De enige uitzondering koppelen via UUID is koppelen van een besturingssysteemschijf voor het oplossen van problemen, zoals beschreven in de sectie die volgen.

## <a name="troubleshooting-a-suse-vm-that-isnt-accessible-anymore"></a>Het oplossen van een SUSE VM die niet meer worden geopend
Mogelijk zijn er situaties waarin een SUSE VM op Azure reageert niet meer in het opstartproces (bijvoorbeeld met een fout met betrekking tot het koppelen van de schijven). U kunt dit probleem controleren via de boot diagnostics-functie voor Azure Virtual Machines v2 in de Azure portal. Zie voor meer informatie [opstarten diagnostics](https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/).

Een manier voor het oplossen van het probleem is de besturingssysteemschijf van de beschadigde VM koppelen aan een andere SUSE VM op Azure. Breng juiste wijzigingen zoals /etc/fstab bewerken of verwijderen van netwerk udev regels, zoals beschreven in de volgende sectie.

Er is een belangrijke factor. Implementeren van verschillende SUSE-virtuele machines van de dezelfde Azure Marketplace-installatiekopie (bijvoorbeeld SLES 11 SP4) zorgt ervoor dat de schijf met het besturingssysteem moet altijd worden gekoppeld door dezelfde UUID. Gebruik daarom de UUID van een besturingssysteemschijf koppelen van een andere virtuele machine die is geïmplementeerd met behulp van de resultaten hetzelfde Azure Marketplace-installatiekopie in twee identieke UUID's. Twee identieke UUID's oorzaak de virtuele machine die wordt gebruikt voor het oplossen van problemen opstarten vanaf een aangesloten en beschadigde schijf met het besturingssysteem in plaats van een schijf met het oorspronkelijke besturingssysteem.

Er zijn twee manieren om problemen te voorkomen:

* Gebruik een andere Azure Marketplace-installatiekopie voor het oplossen van problemen VM (bijvoorbeeld SLES 11 SPx in plaats van SLES 12).
* Niet koppelen beschadigde schijf met het besturingssysteem van een andere virtuele machine met UUID--gebruik iets anders.

## <a name="uploading-a-suse-vm-from-on-premises-to-azure"></a>Uploaden van een VM SUSE on-premises naar Azure
Zie voor een beschrijving van de stappen voor het uploaden van een VM SUSE on-premises naar Azure [een SLES of openSUSE virtuele machine voorbereiden voor Azure](../../linux/suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Als u wilt uploaden van een virtuele machine zonder de identiteitsgegevens stap aan het einde (bijvoorbeeld wilt behouden, een bestaande installatie van de SAP, evenals de hostnaam), controleert u de volgende items:

* Zorg ervoor dat de OS-schijf is gekoppeld met behulp van UUID en niet de apparaat-ID. Wijzigen in UUID NET in /etc/fstab is niet genoeg voor de besturingssysteemschijf. Bovendien Vergeet niet om aan te passen van het opstartlaadprogramma via YaST of door /boot/grub/menu.lst te bewerken.
* Als u de VHDX-indeling voor de besturingssysteemschijf SUSE gebruiken en naar VHD converteren voor het uploaden naar Azure, is het waarschijnlijk dat het netwerkapparaat wordt gewijzigd van eth0 in eth1. Om problemen te voorkomen wanneer u later op Azure bent opgestart, terug te zetten naar eth0 zoals beschreven in [eth0 in hersteld gekloond SLES 11 VMware](https://dartron.wordpress.com/2013/09/27/fixing-eth1-in-cloned-sles-11-vmware/).

Naast het wat in het artikel wordt beschreven, raden we aan dat u dit bestand verwijderen:

   /lib/udev/rules.d/75-persistent-net-generator.rules

U kunt ook de Azure Linux Agent (waagent) om u te helpen te voorkomen dat potentiële problemen, zolang er meerdere NIC's zijn installeren.

## <a name="deploying-a-suse-vm-on-azure"></a>Implementeren van een VM SUSE op Azure
U moet nieuwe SUSE virtuele machines maken met behulp van de sjabloonbestanden JSON in het nieuwe model van Azure Resource Manager. Nadat het bestand JSON-sjabloon is gemaakt, kunt u de virtuele machine kunt implementeren met behulp van de volgende opdracht in de CLI als alternatief voor PowerShell:

   ```
   azure group deployment create "<deployment name>" -g "<resource group name>" --template-file "<../../filename.json>"

   ```
Zie voor meer informatie over de JSON-sjabloonbestanden [Azure Resource Manager-sjablonen samenstellen](../../../resource-group-authoring-templates.md) en [Azure-snelstartsjablonen](https://azure.microsoft.com/documentation/templates/).

Zie voor meer informatie over de CLI en Azure Resource Manager [gebruik van de Azure CLI voor Mac, Linux en Windows Azure Resource Manager](../../../xplat-cli-azure-resource-manager.md).

## <a name="sap-license-and-hardware-key"></a>SAP-licentie en hardware-sleutel
Voor de officiële SAP-Azure-certificering, is een nieuw mechanisme geïntroduceerd voor het berekenen van de SAP-hardware-sleutel die wordt gebruikt voor de SAP-licentie. De kernel SAP moest worden aangepast aan het maken van het nieuwe algoritme gebruiken. Eerdere versies van de SAP kernel voor Linux bevat geen deze code wijzigen. Daarom in bepaalde situaties (bijvoorbeeld Azure VM vergroten/verkleinen), de SAP-hardwaresleutel gewijzigd en de SAP-licentie is niet langer geldig. Een oplossing wordt verstrekt met een meer recente SAP Linux kernels.  De gedetailleerde SAP kernel-patches worden beschreven in SAP-notitie 1928533.

## <a name="suse-sapconf-package--tuned-adm"></a>SUSE sapconf pakket / afgestemd adm
SUSE biedt een pakket genaamd 'sapconf', waarmee een reeks SAP-specifieke instellingen worden beheerd. Zie voor meer informatie over wat dit pakket biedt en het installeren en gebruiken: [sapconf voorbereiden SUSE Linux Enterprise Server SAP-systemen uitvoeren met](https://www.suse.com/communities/blog/using-sapconf-to-prepare-suse-linux-enterprise-server-to-run-sap-systems/) en [wat sapconf is of een SUSE Linux Enterprise voorbereiden Server voor het uitvoeren van SAP-systemen? ](http://scn.sap.com/community/linux/blog/2014/03/31/what-is-sapconf-or-how-to-prepare-a-suse-linux-enterprise-server-for-running-sap-systems).

Er is in de tussentijd een nieuw hulpprogramma die 'sapconf - afgestemd adm' vervangt. Een vindt meer informatie over dit hulpprogramma twee koppelingen te volgen:

- SLES 'afgestemd adm' profiel sap-hana kunt raadplegen voor informatie over [hier](https://www.suse.com/documentation/sles-for-sap-12/book_s4s/data/sec_s4s_configure_sapconf.html) 

- Prestatieafstemming systemen voor SAP-werkbelastingen met 'afgestemd-adm' - vindt [hier](https://www.suse.com/documentation/sles-for-sap-12/pdfdoc/book_s4s/book_s4s.pdf) in hoofdstuk 6.2

## <a name="nfs-share-in-distributed-sap-installations"></a>NFS-sharemachtigingen in gedistribueerde SAP-installaties
Als u hebt een gedistribueerde installatie--bijvoorbeeld waar u de database en de SAP-toepassingsservers in afzonderlijke virtuele machines--installeren kunt u de map /sapmnt via Network File System (NFS) delen. Als u problemen met de installatiestappen hebt nadat u de NFS-share voor /sapmnt maakt, controleert u of 'no_root_squash' is ingesteld voor de share.

## <a name="logical-volumes"></a>Logische volumes
In het verleden desgewenst een een groot volume logische over meerdere Azure data-schijven (bijvoorbeeld voor de SAP-database) is het raadzaam Raid beheerprogramma MDADM niet gebruiken omdat Linux logische Volume Manager (LVM) is niet volledig gevalideerd nog in Azure. Zie voor meer informatie over het instellen van Linux-RAID in Azure met behulp van mdadm, [configureren van software-RAID op Linux](../../linux/configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). In de tussentijd beheer van logische Linux Volume vanaf het begin van mei 2016 kennis wordt volledig ondersteund in Azure en kan worden gebruikt als alternatief voor MDADM. Lees voor meer informatie over LVM op Azure:  
[LVM configureren op een virtuele Linux-machine in Azure](../../linux/configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="azure-suse-repository"></a>Azure SUSE opslagplaats
Als u een probleem met toegang tot de standaard SUSE Azure-opslagplaats hebt, kunt u een opdracht in te stellen. Dergelijke problemen kunnen gebeuren als u een persoonlijke installatiekopie van het besturingssysteem in een Azure-regio maakt en vervolgens de installatiekopie naar een andere Azure-regio kopiëren voor het implementeren van nieuwe virtuele machines die zijn gebaseerd op deze persoonlijke installatiekopie van het besturingssysteem. Voer de volgende opdracht in de virtuele machine:

   ```
   service guestregister restart
   ```

## <a name="gnome-desktop"></a>Gnome bureaublad
Als u het bureaublad Gnome gebruiken wilt voor het installeren van een volledige SAP-demo systeem binnen een enkele VM--met inbegrip van een GUI SAP browser en SAP-beheerconsole--gebruiken deze hint te installeren op de Azure SLES afbeeldingen:

   Voor SLES 11:

   ```
   zypper in -t pattern gnome
   ```

   Voor SLES 12:

   ```
   zypper in -t pattern gnome-basic
   ```

## <a name="sap-support-for-oracle-on-linux-in-the-cloud"></a>SAP-ondersteuning voor Oracle op Linux in de cloud
Er is een beperking van de ondersteuning van Oracle op Linux in gevirtualiseerde omgevingen. Hoewel deze beperking ondersteuning niet gelijk is aan een Azure-specifieke onderwerp, is het belangrijk om te begrijpen. SAP biedt geen ondersteuning voor Oracle op SUSE of Red Hat in een openbare cloud zoals Azure. In de tussentijd wordt uitgevoerd, Oracle-database in Azure wordt volledig ondersteund door SAP op Oracle Linux (Zie SAP-notitie 1928533). Als andere combinaties nodig zijn, rechtstreeks contact opnemen met Oracle.

