---
title: SAP NetWeaver testen op Microsoft Azure SUSE Linux VM's | Microsoft Docs
description: SAP NetWeaver testen op Microsoft Azure SUSE Linux VM's
services: virtual-machines-linux
documentationcenter: ''
author: hermanndms
manager: jeconnoc
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: 645e358b-3ca1-4d3d-bf70-b0f287498d7a
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 09/14/2017
ms.author: hermannd
ms.openlocfilehash: 8a16fa9f639a6a4a17d6904d6bc9a0e31f774e0c
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46950043"
---
# <a name="running-sap-netweaver-on-microsoft-azure-suse-linux-vms"></a>SAP NetWeaver uitvoeren op Microsoft Azure SUSE Linux VM's
Dit artikel beschrijft de verschillende dingen die u moet overwegen wanneer u SAP NetWeaver op virtuele Microsoft Azure SUSE Linux-machines (VM's) uitvoert. SAP NetWeaver wordt vanaf 19 mei 2016 officieel ondersteund op SUSE Linux VM's in Azure. Alle gegevens met betrekking tot Linux-versies, SAP-kernel-versies en andere vereisten kunnen u vinden in de SAP-notitie 1928533 "SAP-toepassingen op Azure: ondersteunde producten en typen Azure VM's '.
Meer documentatie over SAP op Linux-VM's vindt u hier: [SAP op virtuele Linux-machines (VM's) met behulp van](get-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

De volgende informatie kunt u enkele mogelijke valkuilen voorkomen.

## <a name="suse-images-on-azure-for-running-sap"></a>SUSE-installatiekopieën in Azure voor het uitvoeren van SAP
Voor het uitvoeren van SAP NetWeaver op Azure, gebruik van SUSE Linux Enterprise Server SLES 12 (SPx) of SLES voor SAP - Zie ook SAP-notitie 1928533. Een speciale SUSE-installatiekopie is in de Azure Marketplace ("SLES 11 SP3 voor SAP CAL'), maar de installatiekopie is niet bedoeld voor algemeen gebruik. Moet u deze installatiekopie niet gebruiken omdat deze bestemd voor de [SAP Cloud Appliance Library](https://cal.sap.com/) oplossing.  

U moet het framework van de implementatie van Azure Resource Manager gebruiken voor alle installaties op Azure. Als u wilt zoeken voor installatiekopieën voor SLES SUSE- en -versies met behulp van Azure PowerShell of de Azure-opdrachtregelinterface (CLI), gebruikt u de opdrachten die hieronder wordt weergegeven. U kunt vervolgens de uitvoer van de bijvoorbeeld gebruiken voor het definiëren van de installatiekopie van het besturingssysteem in een JSON-sjabloon voor het implementeren van een nieuwe SUSE Linux-VM.
Deze PowerShell-opdrachten zijn geldig voor Azure PowerShell-versie 1.0.1 of hoger.

Het is nog steeds mogelijk is met de standaard installatiekopieën voor SLES voor SAP-installaties, is het aanbevolen om het gebruik van de nieuwe SLES voor SAP-installatiekopieën maken. Deze installatiekopieën zijn nu beschikbaar in de galerie met installatiekopieën van Azure. Meer informatie over deze installatiekopieën kan worden gevonden op de bijbehorende [Azure Marketplace-pagina]( https://azuremarketplace.microsoft.com/en-us/marketplace/apps/SUSE.SLES-SAP ) of de [webpagina SUSE Veelgestelde vragen over SLES voor SAP]( https://www.suse.com/products/sles-for-sap/frequently-asked-questions/ ).


* Zoeken naar bestaande uitgevers, inclusief SUSE:
  
   ```
   PS  : Get-AzureRmVMImagePublisher -Location "West Europe"  | where-object { $_.publishername -like "*US*"  }
   CLI : azure vm image list-publishers westeurope | grep "US"
   ```
* Zoeken naar bestaande aanbiedingen van SUSE:
  
   ```
   PS  : Get-AzureRmVMImageOffer -Location "West Europe" -Publisher "SUSE"
   CLI : azure vm image list-offers westeurope SUSE
   ```
* Zoeken naar SUSE SLES-aanbiedingen:
  
   ```
   PS  : Get-AzureRmVMImageSku -Location "West Europe" -Publisher "SUSE" -Offer "SLES"
   PS  : Get-AzureRmVMImageSku -Location "West Europe" -Publisher "SUSE" -Offer "SLES-SAP"
   CLI : azure vm image list-skus westeurope SUSE SLES
   CLI : azure vm image list-skus westeurope SUSE SLES-SAP
   ```
* Zoeken naar een specifieke versie van een SLES-SKU:
  
   ```
   PS  : Get-AzureRmVMImage -Location "West Europe" -Publisher "SUSE" -Offer "SLES" -skus "12-SP2"
   PS  : Get-AzureRmVMImage -Location "West Europe" -Publisher "SUSE" -Offer "SLES-SAP" -skus "12-SP2"
   CLI : azure vm image list westeurope SUSE SLES 12-SP2
   CLI : azure vm image list westeurope SUSE SLES-SAP 12-SP2
   ```

## <a name="installing-walinuxagent-in-a-suse-vm"></a>WALinuxAgent installeren in een SUSE virtuele machine
De agent met de naam WALinuxAgent maakt deel uit van de installatiekopieën voor SLES in de Azure Marketplace. Zie voor informatie over het installeren van deze handmatig (bijvoorbeeld tijdens het uploaden van een besturingssysteem SLES virtuele harde schijf (VHD) van on-premises):

* [openSUSE](http://software.opensuse.org/package/WALinuxAgent)
* [Azure](../../linux/endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [SUSE](https://www.suse.com/communities/blog/suse-linux-enterprise-server-configuration-for-windows-azure/)

## <a name="sap-enhanced-monitoring"></a>SAP-"verbeterde controle"
SAP 'enhanced aan controleren"is een verplichte vereiste om uit te voeren van SAP op Azure. Controleer de details in SAP Houd er rekening mee 2191498 "SAP op Linux met Azure: verbeterde controle'.

## <a name="attaching-azure-data-disks-to-an-azure-linux-vm"></a>Azure-gegevensschijven koppelen aan een virtuele Azure Linux-machine
Nooit gegevensschijven koppelen van Azure met een Azure Linux-VM met behulp van de apparaat-ID. Gebruik in plaats daarvan de universally unique identifier (UUID). Wees voorzichtig wanneer u grafische hulpprogramma's te koppelen van een Azure-gegevensschijven, bijvoorbeeld. Controleer de vermeldingen in/etc/fstab.

Het probleem met de apparaat-ID is dat kan worden gewijzigd, en vervolgens de Azure-VM tijdens het opstarten dat mogelijk. Als u wilt het probleem verhelpen, kunt u de parameter nofail toevoegen in/etc/fstab. Maar wees voorzichtig met nofail omdat toepassingen van het koppelpunt als voordat gebruikmaken kunnen en naar het bestandssysteem hoofdmap schrijven kunnen als een externe Azure-gegevensschijf tijdens het opstarten is niet gekoppeld.

De enige uitzondering koppelen via UUID is bezig met koppelen van de schijf met een besturingssysteem voor het oplossen van problemen, zoals beschreven in de sectie die volgen.

## <a name="troubleshooting-a-suse-vm-that-isnt-accessible-anymore"></a>Het oplossen van een SUSE VM die niet meer toegankelijk is
Mogelijk zijn er situaties waarin een SUSE VM op Azure loopt tijdens het opstarten (bijvoorbeeld met een fout met betrekking vast tot het koppelen van schijven). U kunt dit probleem controleren met behulp van de functie van de diagnostische gegevens over opstarten voor virtuele Machines van Azure-v2 in de Azure-portal. Zie voor meer informatie, [diagnostische gegevens over opstarten](https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/).

Een manier om het probleem te verhelpen, is de besturingssysteemschijf van de beschadigde virtuele machine koppelen aan een andere SUSE-VM op Azure. Vervolgens dienovereenkomstig zoals/etc/fstab bewerken of verwijderen van regels voor network udev, zoals beschreven in de volgende sectie.

Er is een van de belangrijke dingen om te overwegen. Implementeren van verschillende SUSE-VM's uit de dezelfde Azure Marketplace-installatiekopie (bijvoorbeeld SLES 11 SP4) zorgt ervoor dat de besturingssysteemschijf altijd door de dezelfde UUID worden gekoppeld. Gebruik daarom de UUID een besturingssysteemschijf koppelen vanuit een andere virtuele machine die is geïmplementeerd met behulp van de dezelfde resultaten van de Azure Marketplace-installatiekopie in twee identieke UUID's. Twee identieke UUID's oorzaak een de virtuele machine die is gebruikt voor het oplossen van problemen opstarten vanaf de aangesloten en beschadigde OS-schijf in plaats van een schijf met het oorspronkelijke besturingssysteem.

Er zijn twee manieren om problemen te voorkomen:

* Gebruik een andere Azure Marketplace-installatiekopie voor de VM voor probleemoplossing (bijvoorbeeld SLES 11 SPx in plaats van SLES 12).
* Beschadigde schijf met het besturingssysteem niet koppelen vanuit een andere virtuele machine met behulp van UUID--gebruik iets anders.

## <a name="uploading-a-suse-vm-from-on-premises-to-azure"></a>Uploaden van een VM SUSE van on-premises naar Azure
Zie voor een beschrijving van de stappen voor het uploaden van een VM SUSE van on-premises naar Azure, [een SLES of opensuse gebaseerde virtuele machine voor Azure voorbereiden](../../linux/suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Als u wilt uploaden van een virtuele machine zonder de identiteitsgegevens stap aan het einde (bijvoorbeeld naar behouden een bestaande SAP-installatie, evenals de hostnaam), controleert u de volgende items:

* Zorg ervoor dat de besturingssysteemschijf is gekoppeld met behulp van UUID en niet de apparaat-ID. Wijzigen in UUID just-in-/ etc/fstab is niet voldoende voor de besturingssysteemschijf. Ook, vergeet dan niet om aan te passen van het opstartlaadprogramma via YaST of door /boot/grub/menu.lst te bewerken.
* Als u de VHDX-indeling voor de besturingssysteemschijf van SUSE en deze naar VHD converteren voor het uploaden naar Azure, is het waarschijnlijk dat het netwerkapparaat is gewijzigd van eth0 in eth1. Om problemen te voorkomen wanneer u later op Azure hebt opgestart, terug te zetten naar eth0 zoals beschreven in [vaststelling eth0 in SLES 11 VMware gekloond](https://dartron.wordpress.com/2013/09/27/fixing-eth1-in-cloned-sles-11-vmware/).

Naast wat in het artikel wordt beschreven, wordt u aangeraden dat u dit bestand verwijderen:

   /lib/udev/Rules.d/75-persistent-NET-generator.Rules

U kunt ook de Azure Linux Agent (waagent) om u te helpen potentiële problemen te voorkomen, zolang er meerdere NIC's zijn installeren.

## <a name="deploying-a-suse-vm-on-azure"></a>Een SUSE virtuele machine op Azure
U moet nieuwe SUSE virtuele machines maken met behulp van JSON-sjabloon-bestanden in de nieuwe Azure Resource Manager-model. Nadat het bestand met JSON-sjabloon is gemaakt, kunt u de virtuele machine kunt implementeren met behulp van de volgende CLI-opdracht als alternatief voor PowerShell:

   ```
   azure group deployment create "<deployment name>" -g "<resource group name>" --template-file "<../../filename.json>"

   ```
Zie voor meer informatie over JSON-sjabloonbestanden [Authoring Azure Resource Manager-sjablonen](../../../resource-group-authoring-templates.md) en [Azure-snelstartsjablonen](https://azure.microsoft.com/documentation/templates/).

Zie voor meer informatie over Azure klassieke CLI en Azure Resource Manager [gebruiken de klassieke Azure CLI voor Mac, Linux en Windows met Azure Resource Manager](../../../xplat-cli-azure-resource-manager.md).

## <a name="sap-license-and-hardware-key"></a>SAP-licentie en hardware-sleutel
Een nieuwe mechanisme is voor de officiële SAP-Azure-certificering geïntroduceerd voor het berekenen van de SAP-hardware-sleutel die wordt gebruikt voor de SAP-licentie. De SAP-kernel moest worden aangepast om het gebruik van het nieuwe algoritme. Vorige versies van de SAP kernel voor Linux bevat geen deze gewijzigde code. Dus in bepaalde situaties (bijvoorbeeld Azure-VM vergroten of verkleinen), de SAP-hardwaresleutel gewijzigd en de SAP-licentie is niet langer geldig. Een oplossing wordt geleverd met meer recente SAP Linux kernels.  De gedetailleerde SAP-kernel-patches worden beschreven in de SAP-notitie 1928533.

## <a name="suse-sapconf-package--tuned-adm"></a>SUSE sapconf pakket / afgestemd op de adm
SUSE biedt een pakket met de naam 'sapconf', waarmee een set van SAP-specifieke instellingen worden beheerd. Zie voor meer informatie over wat dit pakket doet en hoe u installeert en gebruikt: [sapconf om voor te bereiden van een SUSE Linux Enterprise Server om uit te voeren van SAP-systemen met](https://www.suse.com/communities/blog/using-sapconf-to-prepare-suse-linux-enterprise-server-to-run-sap-systems/) en [wat sapconf is of hoe u voorbereidt een SUSE Linux Enterprise Server voor het uitvoeren van SAP-systemen? ](http://scn.sap.com/community/linux/blog/2014/03/31/what-is-sapconf-or-how-to-prepare-a-suse-linux-enterprise-server-for-running-sap-systems).

Er is in de tussentijd een nieuw hulpprogramma, die wordt vervangen door 'sapconf - afgestemd op de adm'. Een vindt meer informatie over dit hulpprogramma dat de twee koppelingen:

- SLES-documentatie over 'afgestemd op de adm-profiel sap-hana kan worden gevonden [hier](https://www.suse.com/documentation/sles-for-sap-12/book_s4s/data/sec_saptune.html) 

- Afstemmen systemen voor SAP-Workloads met 'afgestemd-adm' - vindt [hier](https://www.suse.com/documentation/sles-for-sap-12/pdfdoc/book_s4s/book_s4s.pdf) in hoofdstuk 6.2

## <a name="nfs-share-in-distributed-sap-installations"></a>NFS-sharemachtigingen in gedistribueerde SAP-installaties
Als u een gedistribueerde installatie--bijvoorbeeld waar u voor het installeren van de database en de SAP-toepassingsservers in afzonderlijke virtuele machines, kunt u de map /sapmnt via Network File System (NFS) delen. Als er zich problemen met de installatiestappen uit nadat u de NFS-share voor /sapmnt hebt gemaakt, controleert u of 'no_root_squash' is ingesteld voor de share.

## <a name="logical-volumes"></a>Logische volumes
In het verleden desgewenst een een logische groot over meerdere Azure-gegevens-schijven (bijvoorbeeld voor de SAP-database), is het raadzaam te gebruiken hulpprogramma Raid MDADM omdat Linux logische Volume Manager (LVM) is niet volledig gevalideerd nog op Azure. Zie voor meer informatie over het instellen van de RAID Linux op Azure met behulp van mdadm, [software-RAID op Linux configureren](../../linux/configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). In de tussentijd Linux Logical Volume Manager vanaf het begin van mei 2016, kennis wordt volledig ondersteund in Azure en kan worden gebruikt als een alternatief voor MDADM. Lees voor meer informatie met betrekking tot LVM op Azure:  
[LVM configureren op een virtuele Linux-machine in Azure](../../linux/configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="azure-suse-repository"></a>SUSE-opslagplaats voor Azure
Als u een probleem met toegang tot de standaard Azure SUSE-opslagplaats hebt, kunt u een opdracht in te stellen. Dergelijke problemen kunnen optreden als u een persoonlijke installatiekopie van het besturingssysteem in een Azure-regio maken en vervolgens de installatiekopie kopiëren naar een andere Azure-regio nieuwe virtuele machines die zijn gebaseerd op deze persoonlijke installatiekopie van het besturingssysteem te implementeren. Voer de volgende opdracht in de virtuele machine:

   ```
   service guestregister restart
   ```

## <a name="gnome-desktop"></a>Gnome desktop
Als u wilt het bureaublad Gnome gebruiken voor het installeren van een complete SAP-demo systeem binnen een enkele virtuele machine, met inbegrip van een GUI SAP browser en SAP management-console--gebruikt deze hint te installeren op de Azure-SLES-installatiekopieën:

   Voor SLES 11:

   ```
   zypper in -t pattern gnome
   ```

   Voor SLES 12:

   ```
   zypper in -t pattern gnome-basic
   ```

## <a name="sap-support-for-oracle-on-linux-in-the-cloud"></a>SAP-ondersteuning voor Oracle op Linux in de cloud
Er is een beperking van de ondersteuning van Oracle op Linux in gevirtualiseerde omgevingen. Hoewel deze beperking ondersteuning niet gelijk is aan een Azure-specifieke onderwerp, is het belangrijk om te begrijpen. SAP biedt geen ondersteuning voor Oracle op SUSE of Red Hat in een openbare cloud, zoals Azure. In de tussentijd uitvoeren, Oracle DB in Azure wordt volledig ondersteund door SAP voor Oracle Linux (Zie SAP-notitie 1928533). Als andere combinaties nodig zijn, rechtstreeks contact op met Oracle.

