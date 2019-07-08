---
title: Azure virtuele Machines hoge beschikbaarheid voor SAP NetWeaver op Red Hat Enterprise Linux met Azure NetApp-bestanden | Microsoft Docs
description: Azure virtuele Machines hoge beschikbaarheid voor SAP NetWeaver op Red Hat Enterprise Linux
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: rdeltcheva
manager: juergent
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 06/14/2019
ms.author: radeltch
ms.openlocfilehash: 8679cfe54c8fb2c88b312f67ea9b2d7115cc479e
ms.sourcegitcommit: 837dfd2c84a810c75b009d5813ecb67237aaf6b8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2019
ms.locfileid: "67503576"
---
# <a name="azure-virtual-machines-high-availability-for-sap-netweaver-on-red-hat-enterprise-linux-with-azure-netapp-files-for-sap-applications"></a>Azure virtuele Machines hoge beschikbaarheid voor SAP NetWeaver op Red Hat Enterprise Linux met Azure NetApp-bestanden voor SAP-toepassingen

[dbms-guide]:dbms-guide.md
[deployment-guide]:deployment-guide.md
[planning-guide]:planning-guide.md

[anf-azure-doc]:https://docs.microsoft.com/azure/azure-netapp-files/
[anf-avail-matrix]:https://azure.microsoft.com/global-infrastructure/services/?products=storage&regions=all
[anf-register]:https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-register
[anf-sap-applications-azure]:https://www.netapp.com/us/media/tr-4746.pdf

[2002167]: https://launchpad.support.sap.com/#/notes/2002167
[2009879]: https://launchpad.support.sap.com/#/notes/2009879
[1928533]: https://launchpad.support.sap.com/#/notes/1928533
[2015553]: https://launchpad.support.sap.com/#/notes/2015553
[2178632]: https://launchpad.support.sap.com/#/notes/2178632
[2191498]: https://launchpad.support.sap.com/#/notes/2191498
[2243692]: https://launchpad.support.sap.com/#/notes/2243692
[1999351]: https://launchpad.support.sap.com/#/notes/1999351
[1410736]:https://launchpad.support.sap.com/#/notes/1410736

[sap-swcenter]:https://support.sap.com/en/my-support/software-downloads.html

[template-multisid-xscs]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-xscs-md%2Fazuredeploy.json

[sap-hana-ha]:sap-hana-high-availability-rhel.md
[glusterfs-ha]:high-availability-guide-rhel-glusterfs.md

In dit artikel wordt beschreven hoe u de virtuele machines implementeren, configureren van de virtuele machines, installeer het framework van het cluster en installeren van een maximaal beschikbare 7,50 van SAP NetWeaver-systeem, met behulp van [Azure NetApp Files](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-introduction/).
In de voorbeeldconfiguraties, enzovoort-opdrachten voor installatie. ASCS-exemplaar is getal 00, het exemplaar INGEN is getal 01, primaire toepassingsexemplaar (Pa's) is 02 en het exemplaar van de toepassing (AAS) is 03. SAP-systeem-ID QAS wordt gebruikt. 

De databaselaag wordt niet gedekt in dit artikel.  

Lees eerst de volgende SAP-opmerkingen en documenten:

* [Documentatie voor Azure NetApp bestanden][anf-azure-doc] 
* SAP-notitie [1928533], heeft:
  * Lijst met Azure-VM-grootten die worden ondersteund voor de implementatie van SAP-software
  * Informatie over belangrijke capaciteit voor Azure VM-grootten
  * Ondersteunde SAP-software en besturingssysteem (OS) en combinaties van database
  * Vereiste kernel van SAP-versie voor Windows en Linux op Microsoft Azure

* SAP-notitie [2015553] bevat vereisten voor SAP-ondersteunde SAP software-implementaties in Azure.
* SAP-notitie [2002167] heeft aanbevolen instellingen voor Red Hat Enterprise Linux OS
* SAP-notitie [2009879] bevat richtlijnen voor SAP HANA voor Red Hat Enterprise Linux
* SAP-notitie [2178632] vindt u meer informatie over alle bewaking metrische gegevens die zijn gerapporteerd voor SAP in Azure.
* SAP-notitie [2191498] heeft de vereiste versie van de SAP-Host-Agent voor Linux in Azure.
* SAP-notitie [2243692] bevat informatie over de licentieverlening voor SAP op Linux in Azure.
* SAP-notitie [1999351] bevat aanvullende informatie over probleemoplossing voor de Azure uitgebreide controle-extensie voor SAP.
* [SAP-Community WIKI](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) bevat alle SAP-opmerkingen voor Linux vereist.
* [Azure virtuele Machines, planning en implementatie van SAP op Linux][planning-guide]
* [Azure Virtual Machines-implementatie voor SAP op Linux][deployment-guide]
* [Azure virtuele Machines DBMS-implementatie voor SAP op Linux][dbms-guide]
* [SAP Netweaver in pacemaker cluster](https://access.redhat.com/articles/3150081)
* Algemene RHEL-documentatie
  * [Overzicht van de invoegtoepassing voor hoge beschikbaarheid](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/high_availability_add-on_overview/index)
  * [Beheer van de invoegtoepassing voor hoge beschikbaarheid](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/high_availability_add-on_administration/index)
  * [Naslaginformatie over hoge beschikbaarheid-invoegtoepassing](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/high_availability_add-on_reference/index)
  * [Configureren van ASCS/INGEN voor SAP Netweaver met zelfstandige resources in RHEL 7.5](https://access.redhat.com/articles/3569681)
  * [SAP S/4HANA ASCS/INGEN met in de wachtrij plaatsen zelfstandige Server, 2 (ENSA2) in Pacemaker op RHEL configureren ](https://access.redhat.com/articles/3974941)
* Azure specifieke RHEL-documentatie:
  * [Ondersteuningsbeleid voor RHEL Clusters met hoge beschikbaarheid - Microsoft Azure Virtual Machines als leden van het Cluster](https://access.redhat.com/articles/3131341)
  * [Installeren en configureren van een Red Hat Enterprise Linux 7.4 (en hoger) Cluster met hoge beschikbaarheid voor Microsoft Azure](https://access.redhat.com/articles/3252491)
* [NetApp-SAP-toepassingen op Microsoft Azure met behulp van Azure NetApp bestanden][anf-sap-applications-azure]

## <a name="overview"></a>Overzicht

Hoge availability(HA) voor SAP Netweaver centrale services vereist gedeelde opslag.
Om dit te bereiken op Red Hat Linux was tot nu toe die nodig zijn voor het bouwen van maximaal beschikbare GlusterFS cluster dat gescheiden is. 

Het is nu mogelijk om te bereiken SAP Netweaver hoge beschikbaarheid met behulp van gedeelde opslag, die zijn geïmplementeerd op Azure NetApp bestanden. Met behulp van Azure NetApp bestanden voor de gedeelde opslag elimineert de noodzaak voor extra [GlusterFS cluster](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-glusterfs). Pacemaker is nog steeds nodig voor hoge beschikbaarheid van de SAP Netweaver centrale services(ASCS/SCS).

![Overzicht van SAP NetWeaver hoge beschikbaarheid](./media/high-availability-guide-rhel/high-availability-guide-rhel-anf.png)

SAP NetWeaver ASCS, SAP NetWeaver SCS INGEN voor SAP NetWeaver en SAP HANA-database gebruiken virtuele hostnaam en virtuele IP-adressen. In Azure, wordt een load balancer overstappen naar een virtueel IP-adres. De volgende lijst bevat de configuratie van de load balancer met afzonderlijke front-end IP-adressen voor (A) SCS- en gebruikers.

> [!IMPORTANT]
> Multi-SID clustering van SAP ASCS/INGEN met Red Hat Linux als gastbesturingssysteem in Azure VM's wordt **niet ondersteund**. Multi-SID clustering beschrijft de installatie van meerdere SAP ASCS/INGEN exemplaren met verschillende SID's in één Pacemaker-cluster.

### <a name="ascs"></a>(A)SCS

* Front-end-configuratie
  * IP-adres 192.168.14.9
* Back-endconfiguratie
  * Verbonden met primaire netwerkinterfaces van alle virtuele machines die deel van de (A uitmaken) SCS/INGEN cluster
* Testpoort
  * Poort 620<strong>&lt;nr&gt;</strong>
* Regels voor taakverdeling
  * 32<strong>&lt;nr&gt;</strong> TCP
  * 36<strong>&lt;nr&gt;</strong> TCP
  * 39<strong>&lt;nr&gt;</strong> TCP
  * 81<strong>&lt;nr&gt;</strong> TCP
  * 5<strong>&lt;nr&gt;</strong>13 TCP
  * 5<strong>&lt;nr&gt;</strong>14 TCP
  * 5<strong>&lt;nr&gt;</strong>16 TCP

### <a name="ers"></a>INGEN

* Front-end-configuratie
  * IP-adres 192.168.14.10
* Back-endconfiguratie
  * Verbonden met primaire netwerkinterfaces van alle virtuele machines die deel van de (A uitmaken) SCS/INGEN cluster
* Testpoort
  * Poort 621<strong>&lt;nr&gt;</strong>
* Regels voor taakverdeling
  * 32<strong>&lt;nr&gt;</strong> TCP
  * 33<strong>&lt;nr&gt;</strong> TCP
  * 5<strong>&lt;nr&gt;</strong>13 TCP
  * 5<strong>&lt;nr&gt;</strong>14 TCP
  * 5<strong>&lt;nr&gt;</strong>16 TCP

## <a name="setting-up-the-azure-netapp-files-infrastructure"></a>Instellen van de infrastructuur van Azure NetApp bestanden 

SAP NetWeaver vereist gedeelde opslag voor de map transport en profiel.  Voordat u doorgaat met de instellingen voor de infrastructuur van Azure NetApp-bestanden, Maak uzelf vertrouwd met de [Azure NetApp Files documentatie][anf-azure-doc]. Controleer of de geselecteerde Azure-regio Azure NetApp bestanden biedt. De volgende koppeling toont de beschikbaarheid van Azure-regio van Azure NetApp bestanden: [Azure NetApp bestanden beschikbaarheid door Azure-regio][anf-avail-matrix].

Azure NetApp bestanden is beschikbaar in verschillende [Azure-regio's](https://azure.microsoft.com/global-infrastructure/services/?products=netapp). Vragen voordat u implementeert Azure NetApp bestanden, onboarding naar Azure NetApp Files, na de [registreren voor Azure NetApp bestanden instructies][anf-register]. 

### <a name="deploy-azure-netapp-files-resources"></a>NetApp-bestanden van Azure-resources implementeren  

De stappen wordt ervan uitgegaan dat u al hebt geïmplementeerd [Azure Virtual Network](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview). De NetApp-bestanden van Azure-resources en de virtuele machines, waar de resources Azure NetApp bestanden wordt gekoppeld moeten worden geïmplementeerd in dezelfde Azure-netwerk of in gekoppelde virtuele netwerken van Azure.  

1. Als u dit nog niet hebt die al klaar, aanvragen [onboarding naar Azure NetApp Files](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-register).  
2. De NetApp-account maken in de geselecteerde Azure-regio na de [instructies voor het maken van de NetApp Account](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-create-netapp-account).  
3. Instellen van Azure NetApp Files capaciteit van toepassingen, na de [instructies over het instellen van Azure NetApp Files capaciteit pool](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-set-up-capacity-pool).  
De architectuur van SAP Netweaver die zijn gepresenteerd in dit artikel maakt gebruik van één Azure NetApp Files capaciteit groep, Premium-SKU. Aangeraden wordt Azure NetApp bestanden Premium SKU voor de werkbelasting van SAP Netweaver-toepassing in Azure.  
4. Een subnet aan Azure NetApp bestanden overdragen zoals beschreven in de [instructies een subnet aan Azure NetApp bestanden overdragen](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-delegate-subnet).  

5. Volumes kunnen implementeren Azure NetApp bestanden, na de [instructies voor het maken van een volume voor Azure NetApp Files](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-create-volumes). Implementeren van de volumes in de aangewezen Azure NetApp bestanden [subnet](https://docs.microsoft.com/rest/api/virtualnetwork/subnets). Houd er rekening mee dat de NetApp-bestanden van Azure-resources en de Azure VM's moeten zich in hetzelfde Azure-netwerk of in gekoppelde virtuele netwerken van Azure. In dit voorbeeld gebruiken we twee Azure NetApp Files volumes: sap<b>QAS</b> en transSAP. De paden die zijn gekoppeld aan de bijbehorende koppelpunten worden /usrsap<b>qas</b>/sapmnt<b>QAS</b>, /usrsap<b>qas</b>/usrsap<b>QAS</b>sys, enzovoort.  

   1. volume sap<b>QAS</b> (nfs://192.168.24.5/usrsap<b>qas</b>/sapmnt<b>QAS</b>)
   2. volume sap<b>QAS</b> (nfs://192.168.24.5/usrsap<b>qas</b>/usrsap<b>QAS</b>ascs)
   3. volume sap<b>QAS</b> (nfs://192.168.24.5/usrsap<b>qas</b>/usrsap<b>QAS</b>sys)
   4. volume sap<b>QAS</b> (nfs://192.168.24.5/usrsap<b>qas</b>/usrsap<b>QAS</b>ingen)
   5. volume transSAP (nfs://192.168.24.4/transSAP)
   6. volume sap<b>QAS</b> (nfs://192.168.24.5/usrsap<b>qas</b>/usrsap<b>QAS</b>Pa's)
   7. volume sap<b>QAS</b> (nfs://192.168.24.5/usrsap<b>qas</b>/usrsap<b>QAS</b>aas)
  
In dit voorbeeld gebruikt we Azure NetApp bestanden voor alle SAP Netweaver-bestandssystemen om te demonstreren hoe Azure NetApp bestanden kunnen worden gebruikt. De SAP-bestandssystemen die niet hoeven te worden gekoppeld via NFS kunnen ook worden geïmplementeerd als [Azure disk-opslag](https://docs.microsoft.com/azure/virtual-machines/windows/disks-types#premium-ssd) . In dit voorbeeld <b>a e</b> moeten zich op Azure NetApp bestanden en <b>f-g</b> (dat wil zeggen, / usr/sap/<b>QAS</b>/D<b>02</b>, /usr/sap/<b>QAS </b>/D<b>03</b>) als Azure disk-opslag kan worden geïmplementeerd. 

### <a name="important-considerations"></a>Belangrijke overwegingen

Wanneer u overweegt Azure NetApp bestanden voor de SAP Netweaver op architectuur voor hoge beschikbaarheid van SUSE, rekening met de volgende belangrijke overwegingen:

- De minimale capaciteit van toepassingen is 4 TiB. De grootte van de capaciteit van toepassingen moet worden gefactureerd in veelvouden van 4 TiB.
- De minimale hoeveelheid is 100 GiB
- Azure Files van NetApp en alle virtuele machines, waar Azure NetApp Files volumes wordt gekoppeld, moeten zich in hetzelfde Azure-netwerk of in [via peering gekoppelde virtuele netwerken](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) in dezelfde regio. Azure NetApp bestanden toegang via VNET-peering in dezelfde regio wordt nu ondersteund. Azure NetApp toegang via het wereldwijde peering wordt nog niet ondersteund.
- Het geselecteerde virtuele netwerk moet een subnet, gedelegeerd naar Azure NetApp bestanden hebben.
- Azure NetApp Files ondersteunt momenteel alleen NFSv3 
- Azure Files met NetApp biedt [beleid exporteren](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-configure-export-policy): u kunt de toegestane clients te bepalen welk toegangstype (lezen en schrijven, alleen-lezen, enz.). 
- Functie van Azure NetApp bestanden is nog niet van de zone op de hoogte. NetApp-bestanden van Azure-functie is niet geïmplementeerd in alle beschikbaarheidszones in een Azure-regio. Houd rekening met de mogelijke gevolgen van de latentie in bepaalde Azure-regio's. 

## <a name="setting-up-ascs"></a>(A) SCS instellen

In dit voorbeeld wordt de resources zijn die handmatig zijn geïmplementeerd de [Azure-portal](https://portal.azure.com/#home).

### <a name="deploy-linux-manually-via-azure-portal"></a>Handmatig implementeren van Linux via Azure portal

U moet eerst de volumes Azure NetApp bestanden maken. Implementeer de virtuele machines. Daarna wordt u een load balancer maken en gebruiken van de virtuele machines in de back endadresgroepen.

1. Maak een Load Balancer (intern)  
   1. De frontend-IP-adressen maken
      1. IP-adres 192.168.14.9 voor de ASCS
         1. De load balancer openen, front-end-IP-adresgroep selecteren en klik op toevoegen
         1. Voer de naam van de nieuwe frontend-IP-adresgroep (bijvoorbeeld **frontend. QAS. ASCS**)
         1. De toewijzing instellen op statisch en geef het IP-adres (bijvoorbeeld **192.168.14.9**)
         1. Klik op OK
      1. IP-adres 192.168.14.10 voor de ASCS ERS
         * Herhaal de stappen hierboven onder "a" maken van een IP-adres voor de gebruikers (bijvoorbeeld **192.168.14.10** en **frontend. QAS. INGEN**)
   1. De back endadresgroepen maken
      1. Een back endadresgroep maken voor de ASCS
         1. Open de load balancer, back-endpools selecteren en klik op toevoegen
         1. Voer de naam van de nieuwe back endpool (bijvoorbeeld **back-end. QAS**)
         1. Klik op een virtuele machine toevoegen.
         1. Selecteer de Beschikbaarheidsset u eerder hebt gemaakt voor ASCS 
         1. Selecteer de virtuele machines van de (A) SCS-cluster
         1. Klik op OK
   1. De statuscontroles maken
      1. Poort 620**00** voor ASCS
         1. De load balancer openen, selecteer statuscontroles en klikt u op toevoegen
         1. Voer de naam van de nieuwe statustest (bijvoorbeeld **health. QAS. ASCS**)
         1. Selecteer TCP als protocol, poort 620**00**, Interval van 5 en de drempelwaarde voor onjuiste status 2
         1. Klik op OK
      1. Poort 621**01** voor ASCS INGEN
            * Herhaal de stappen hierboven onder 'c' te maken van een statustest voor de gebruikers (bijvoorbeeld 621**01** en **health. QAS. INGEN**)
   1. Regels voor taakverdeling
      1. 32**00** TCP voor ASCS
         1. De load balancer openen, Load-balancing regels selecteren en klik op toevoegen
         1. Voer de naam van de nieuwe load balancer-regel (bijvoorbeeld **lb. QAS. ASCS.3200**)
         1. Selecteer de front-end-IP-adres voor ASCS, back-endpool en statustest die u eerder hebt gemaakt (bijvoorbeeld **frontend. QAS. ASCS**)
         1. Houd protocol **TCP**, voer poort **3200**
         1. Time-out voor inactiviteit tot 30 minuten verhogen
         1. **Zorg ervoor dat u zwevend IP inschakelen**
         1. Klik op OK
      1. Extra poorten voor de ASCS
         * Herhaal de stappen hierboven onder "d" voor poorten 36**00**, 39**00**, 81**00**, 5**00**13, 5**00**14, 5**00**16 en TCP-voor de ASCS
      1. Extra poorten voor de ASCS ERS
         * Herhaal de stappen hierboven onder "d" voor poorten 32**01**, 33**01**, 5**01**13, 5**01**14, 5**01**16 en TCP voor de ASCS ERS


> [!IMPORTANT]
> Schakel geen TCP-tijdstempels op Azure VM's achter Azure Load Balancer worden geplaatst. Inschakelen van TCP tijdstempels zorgt ervoor dat de statuscontroles mislukken. Stel de parameter **net.ipv4.tcp_timestamps** naar **0**. Zie voor meer informatie [Load Balancer statuscontroles](https://docs.microsoft.com/azure/load-balancer/load-balancer-custom-probe-overview).

### <a name="create-pacemaker-cluster"></a>Pacemaker-cluster maken

Volg de stappen in [Pacemaker op Red Hat Enterprise Linux in Azure instellen](high-availability-guide-rhel-pacemaker.md) te maken van een eenvoudige Pacemaker-cluster voor dit (A) SCS-server.

### <a name="prepare-for-sap-netweaver-installation"></a>Voorbereiden voor de installatie van SAP NetWeaver

De volgende items worden voorafgegaan door een **[A]** : van toepassing op alle knooppunten **[1]** - alleen van toepassing op knooppunt 1 of **[2]** - alleen van toepassing op knooppunt 2.

1. **[A]**  Omzetten van de hostnaam instellen

   U kunt een DNS-server gebruiken of aanpassen van de/etc/hosts op alle knooppunten. In dit voorbeeld laat zien hoe u het bestand/etc/hosts gebruikt.
   Vervang het IP-adres en de hostnaam in de volgende opdrachten

    ```
    sudo vi /etc/hosts
    ```

   Voeg de volgende regels/etc/hosts. De IP-adres en hostnaam zodat deze overeenkomen met uw omgeving wijzigen

    ```
    # IP address of cluster node 1
    192.168.14.5    anftstsapcl1
    # IP address of cluster node 2
    192.168.14.6     anftstsapcl2
    # IP address of the load balancer frontend configuration for SAP Netweaver ASCS
    192.168.14.9    anftstsapvh
    # IP address of the load balancer frontend configuration for SAP Netweaver ERS
    192.168.14.10    anftstsapers
    ```

1. **[1]**  Maken SAP-mappen in de Azure NetApp Files-volume.  
   Tijdelijk de Azure NetApp Files-volume op een van de virtuele machines koppelen en maken van de SAP-mappen (paden).  

    ```
     #mount temporarily the volume
     sudo mkdir -p /saptmp
     sudo mount -t nfs -o rw,hard,rsize=65536,wsize=65536,vers=3,tcp 192.168.24.5:/sapQAS /saptmp
     # create the SAP directories
     sudo cd /saptmp
     sudo mkdir -p sapmntQAS
     sudo mkdir -p usrsapQASascs
     sudo mkdir -p usrsapQASers
     sudo mkdir -p usrsapQASsys
     sudo mkdir -p usrsapQASpas
     sudo mkdir -p usrsapQASaas
     # unmount the volume and delete the temporary directory
     sudo cd ..
     sudo umount /saptmp
     sudo rmdir /saptmp

1. **[A]** Create the shared directories

   ```
   sudo mkdir -p/sapmnt/QAS sudo mkdir -p /usr/sap/trans sudo mkdir -p /usr/sap/QAS/SYS sudo mkdir -p /usr/sap/QAS/ASCS00 sudo mkdir -p /usr/sap/QAS/ERS01
   
   sudo chattr + ik/sapmnt/QAS sudo chattr + ik /usr/sap/trans sudo chattr + ik /usr/sap/QAS/SYS sudo chattr + ik /usr/sap/QAS/ASCS00 sudo chattr + ik /usr/sap/QAS/ERS01
   ```

1. **[A]** Install NFS client and other requirements

   ```
   sudo yum -y installeren resource-agents resource-agents nfs-utils-sap
   ```

1. **[A]** Check version of resource-agents-sap

   Make sure that the version of the installed resource-agents-sap package is at least 3.9.5-124.el7
   ```
   sudo yum info resource-agents-sap
   
   # <a name="loaded-plugins-langpacks-product-id-search-disabled-repos"></a>Invoegtoepassingen geladen: langpacks, product-id, search-uitgeschakeld-opslagplaatsen
   # <a name="repodata-is-over-2-weeks-old-install-yum-cron-or-run-yum-makecache-fast"></a>Repodata is meer dan twee weken. Yum-cron installeren? Of uit te voeren: yum makecache snel
   # <a name="installed-packages"></a>Geïnstalleerde pakketten
   # <a name="name---------resource-agents-sap"></a>Naam: resource-agents-sap
   # <a name="arch---------x8664"></a>Arch        : x86_64
   # <a name="version------395"></a>Versie: 3.9.5
   # <a name="release------124el7"></a>Release: 124.el7
   # <a name="size---------100-k"></a>Grootte: 100 k
   # <a name="repo---------installed"></a>Opslagplaats: geïnstalleerd
   # <a name="from-repo----rhel-sap-for-rhel-7-server-rpms"></a>Van-opslagplaats: rhel-sap-for-rhel-7-server-rpms
   # <a name="summary------sap-cluster-resource-agents-and-connector-script"></a>Overzicht: SAP-clusteragents resource en het script van de connector
   # <a name="url----------httpsgithubcomclusterlabsresource-agents"></a>URL         : https://github.com/ClusterLabs/resource-agents
   # <a name="license------gplv2"></a>Licentie: GPLv2+
   # <a name="description--the-sap-resource-agents-and-connector-script-interface-with"></a>Beschrijving: De SAP-resource-agents en de connector script interface met
   #          <a name="-pacemaker-to-allow-sap-instances-to-be-managed-in-a-cluster"></a>: Pacemaker om toe te staan van SAP-instanties moeten worden beheerd in een cluster
   #          <a name="-environment"></a>: omgeving.
   ```


1. **[A]** Add mount entries

   ```
   sudo vi/etc/fstab
   
   # <a name="add-the-following-lines-to-fstab-save-and-exit"></a>De volgende regels toevoegen aan fstab, opslaan en sluiten
    192.168.24.5:/sapQAS/sapmntQAS/sapmnt/QAS nfs rw, hard, rsize = 65536, wsize = 65536, bergen 3 192.168.24.5:/sapQAS/usrsapQASsys /usr/sap/QAS/SYS nfs rw, hard, rsize = = 65536, wsize = 65536, bergen = 3 192.168.24.4:/transSAP /usr/sap/trans nfs rw, hard, rsize = 65536, wsize = 65536, bergen = 3
   ```

   Mount the new shares

   ```
   sudo mount - een  
   ```

1. **[A]** Configure SWAP file

   ```
   sudo vi /etc/waagent.conf
   
   # <a name="set-the-property-resourcediskenableswap-to-y"></a>Stel de eigenschap ResourceDisk.EnableSwap op y
   # <a name="create-and-use-swapfile-on-resource-disk"></a>Maak en gebruik van wisselbestand op de bronschijf.
   ResourceDisk.EnableSwap=y
   
   # <a name="set-the-size-of-the-swap-file-with-property-resourcediskswapsizemb"></a>De grootte van het wisselbestand met eigenschap ResourceDisk.SwapSizeMB instellen
   # <a name="the-free-space-of-resource-disk-varies-by-virtual-machine-size-make-sure-that-you-do-not-set-a-value-that-is-too-big-you-can-check-the-swap-space-with-command-swapon"></a>De vrije ruimte van de bronschijf is afhankelijk van de grootte van virtuele machine. Zorg ervoor dat u niet stelt een waarde die is te groot. U kunt de wisselruimte met opdracht swapon controleren
   # <a name="size-of-the-swapfile"></a>Grootte van het wisselbestand.
   ResourceDisk.SwapSizeMB=2000
   ```

   Restart the Agent to activate the change

   ```
   sudo-service waagent opnieuw opstarten
   ```

1. **[A]** RHEL configuration

   Configure RHEL as described in SAP Note [2002167]

### Installing SAP NetWeaver ASCS/ERS

1. **[1]** Create a virtual IP resource and health-probe for the ASCS instance

   ```
   sudo-pc's knooppunt stand-by anftstsapcl2
   
   sudo-pc's-resource maken fs_QAS_ASCS bestandssysteem device='192.168.24.5:/sapQAS/usrsapQASascs' \
     directory='/usr/sap/QAS/ASCS00' fstype='nfs' \
     --groep g-QAS_ASCS
   
   sudo-pc's-resource maken vip_QAS_ASCS IPaddr2 \
     ip=192.168.14.9 cidr_netmask=24 \
     --groep g-QAS_ASCS
   
   sudo-pc's-resource maken nc_QAS_ASCS azure lb-poort 62000 = \
     --groep g-QAS_ASCS
   ```

   Make sure that the cluster status is ok and that all resources are started. It is not important on which node the resources are running.

   ```
   status van sudo-pc 's
   
   # <a name="node-anftstsapcl2-standby"></a>Knooppunt anftstsapcl2: stand-by
   # <a name="online--anftstsapcl1-"></a>Online: [anftstsapcl1]
   #
   # <a name="full-list-of-resources"></a>Volledige lijst met resources:
   #
   # <a name="rscstazure----stonithfenceazurearm------started-anftstsapcl1"></a>rsc_st_azure (stonith:fence_azure_arm):      Gestarte anftstsapcl1
   #  <a name="resource-group-g-qasascs"></a>Resourcegroep: g-QAS_ASCS
   #      <a name="fsqasascs--------ocfheartbeatfilesystem----started-anftstsapcl1"></a>fs_QAS_ASCS (ocf::heartbeat:Filesystem):    Gestarte anftstsapcl1
   #      <a name="ncqasascs--------ocfheartbeatazure-lb------started-anftstsapcl1"></a>nc_QAS_ASCS (ocf::heartbeat:azure-lb):      Gestarte anftstsapcl1
   #      <a name="vipqasascs-------ocfheartbeatipaddr2-------started-anftstsapcl1"></a>vip_QAS_ASCS (ocf::heartbeat:IPaddr2):       Gestarte anftstsapcl1
   ```

1. **[1]** Install SAP NetWeaver ASCS  

   Install SAP NetWeaver ASCS as root on the first node using a virtual hostname that maps to the IP address of the load balancer frontend configuration for the ASCS, for example <b>anftstsapvh</b>, <b>192.168.14.9</b> and the instance number that you used for the probe of the load balancer, for example <b>00</b>.

   You can use the sapinst parameter SAPINST_REMOTE_ACCESS_USER to allow a non-root user to connect to sapinst.

   ```
   # <a name="allow-access-to-swpm-this-rule-is-not-permanent-if-you-reboot-the-machine-you-have-to-run-the-command-again"></a>Toegang tot SWPM toestaan. Deze regel is niet permanent. Als u de computer opnieuw opstarten, hebt u de opdracht opnieuw uitvoeren.
   sudo-firewall-cmd--zone = openbare--toevoegen-port = 4237/tcp
   
   sudo <swpm>/sapinst SAPINST_REMOTE_ACCESS_USER sapadmin SAPINST_USE_HOSTNAME = < virtual_hostname > =
   ```

   If the installation fails to create a subfolder in /usr/sap/**QAS**/ASCS**00**, try setting the owner and group of the ASCS**00** folder and retry.

   ```
   sudo eigenaar qasadm /usr/sap/QAS/ASCS00 sudo chgrp uitvoert sapsys /usr/sap/QAS/ASCS00
   ```

1. **[1]** Create a virtual IP resource and health-probe for the ERS instance

   ```
   sudo-pc's knooppunt unstandby anftstsapcl2 sudo-pc's knooppunt stand-by anftstsapcl1
   
   sudo-pc's-resource maken fs_QAS_AERS bestandssysteem device='192.168.24.5:/sapQAS/usrsapQASers' \
     Directory = '/ usr/sap/QAS/ERS01' fstype = 'nfs' \
    --group g-QAS_AERS

   sudo-pc's-resource maken vip_QAS_AERS IPaddr2 \
     ip=192.168.14.10 cidr_netmask=24 \
    --group g-QAS_AERS
   
   sudo pcs resource create nc_QAS_AERS azure-lb port=62101 \
    --group g-QAS_AERS
   ```
 
   Make sure that the cluster status is ok and that all resources are started. It is not important on which node the resources are running.

   ```
   status van sudo-pc 's
   
   # <a name="node-anftstsapcl1-standby"></a>Knooppunt anftstsapcl1: stand-by
   # <a name="online--anftstsapcl2-"></a>Online: [anftstsapcl2]
   #
   # <a name="full-list-of-resources"></a>Volledige lijst met resources:
   #
   # <a name="rscstazure----stonithfenceazurearm------started-anftstsapcl2"></a>rsc_st_azure (stonith:fence_azure_arm):      Gestarte anftstsapcl2
   #  <a name="resource-group-g-qasascs"></a>Resourcegroep: g-QAS_ASCS
   #      <a name="fsqasascs--------ocfheartbeatfilesystem----started-anftstsapcl2"></a>fs_QAS_ASCS (ocf::heartbeat:Filesystem):    Gestarte anftstsapcl2
   #      <a name="ncqasascs--------ocfheartbeatazure-lb------started-anftstsapcl2"></a>nc_QAS_ASCS (ocf::heartbeat:azure-lb):      Aan de slag anftstsapcl2 <
   #      <a name="vipqasascs-------ocfheartbeatipaddr2-------started-anftstsapcl2"></a>vip_QAS_ASCS (ocf::heartbeat:IPaddr2):       Gestarte anftstsapcl2
   #  <a name="resource-group-g-qasaers"></a>Resourcegroep: g-QAS_AERS
   #      <a name="fsqasaers--------ocfheartbeatfilesystem----started-anftstsapcl2"></a>fs_QAS_AERS (ocf::heartbeat:Filesystem):    Gestarte anftstsapcl2
   #      <a name="ncqasaers--------ocfheartbeatazure-lb------started-anftstsapcl2"></a>nc_QAS_AERS        (ocf::heartbeat:azure-lb):      Gestarte anftstsapcl2
   #      <a name="vipqasaers-------ocfheartbeatipaddr2-------started-anftstsapcl2"></a>vip_QAS_AERS (ocf::heartbeat:IPaddr2):       Gestarte anftstsapcl2
   ```

1. **[2]** Install SAP NetWeaver ERS  

   Install SAP NetWeaver ERS as root on the second node using a virtual hostname that maps to the IP address of the load balancer frontend configuration for the ERS, for example <b>anftstsapers</b>, <b>192.168.14.10</b> and the instance number that you used for the probe of the load balancer, for example <b>01</b>.

   You can use the sapinst parameter SAPINST_REMOTE_ACCESS_USER to allow a non-root user to connect to sapinst.

   ```
   # <a name="allow-access-to-swpm-this-rule-is-not-permanent-if-you-reboot-the-machine-you-have-to-run-the-command-again"></a>Toegang tot SWPM toestaan. Deze regel is niet permanent. Als u de computer opnieuw opstarten, hebt u de opdracht opnieuw uitvoeren.
   sudo-firewall-cmd--zone = openbare--toevoegen-port = 4237/tcp

   sudo <swpm>/sapinst SAPINST_REMOTE_ACCESS_USER sapadmin SAPINST_USE_HOSTNAME = < virtual_hostname > =
   ```

   If the installation fails to create a subfolder in /usr/sap/**QAS**/ERS**01**, try setting the owner and group of the ERS**01** folder and retry.

   ```
   sudo eigenaar qaadm /usr/sap/QAS/ERS01 sudo chgrp uitvoert sapsys /usr/sap/QAS/ERS01
   ```

1. **[1]** Adapt the ASCS/SCS and ERS instance profiles

   * ASCS/SCS profile

   ```
   sudo vi /sapmnt/QAS/profile/QAS_ASCS00_anftstsapvh
   
   # <a name="change-the-restart-command-to-a-start-command"></a>De opdracht opnieuw omzetten in een opdracht starten
   #<a name="restartprogram01--local-en-pfpf"></a>Restart_Program_01 = lokale $(_EN) pf=$(_PF)
   Start_Program_01 = lokale $(_EN) pf=$(_PF)
   
   # <a name="add-the-keep-alive-parameter"></a>De keep alive parameter toevoegen
   enque/encni/set_so_keepalive = true
   ```

   * ERS profile

   ```
   sudo vi /sapmnt/QAS/profile/QAS_ERS01_anftstsapers
   
   # <a name="change-the-restart-command-to-a-start-command"></a>De opdracht opnieuw omzetten in een opdracht starten
   #<a name="restartprogram00--local-er-pfpfl-nrscsid"></a>Restart_Program_00 = lokale $(_ER) pf=$(_PFL) NR=$(SCSID)
   Start_Program_00 = lokale $(_ER) pf=$(_PFL) NR=$(SCSID)
   
   # <a name="remove-autostart-from-ers-profile"></a>automatisch starten van het profiel van gebruikers verwijderen
   # <a name="autostart--1"></a>AutoStart = 1
   ```


1. **[A]** Configure Keep Alive

   The communication between the SAP NetWeaver application server and the ASCS/SCS is routed through a software load balancer. The load balancer disconnects inactive connections after a configurable timeout. To prevent this, you need to set a parameter in the SAP NetWeaver ASCS/SCS profile and change the Linux system settings. Read [SAP Note 1410736][1410736] for more information.

   The ASCS/SCS profile parameter enque/encni/set_so_keepalive was already added in the last step.

   ```
   # <a name="change-the-linux-system-configuration"></a>Wijzigen van de configuratie van de Linux-systeem
   sudo sysctl net.ipv4.tcp_keepalive_time=120
   ```

1. **[A]** Update the /usr/sap/sapservices file

   To prevent the start of the instances by the sapinit startup script, all instances managed by Pacemaker must be commented out from /usr/sap/sapservices file. Do not comment out the SAP HANA instance if it will be used with HANA SR.

   ```
   sudo vi /usr/sap/sapservices
   
   # <a name="on-the-node-where-you-installed-the-ascs-comment-out-the-following-line"></a>Op het knooppunt waar u de ASCS hebt geïnstalleerd, moet u de volgende regel
   # <a name="ldlibrarypathusrsapqasascs00exeldlibrarypath-export-ldlibrarypath-usrsapqasascs00exesapstartsrv-pfusrsapqassysprofileqasascs00anftstsapvh--d--u-qasadm"></a>LD_LIBRARY_PATH=/usr/sap/QAS/ASCS00/exe:$LD_LIBRARY_PATH; export LD_LIBRARY_PATH; /usr/sap/QAS/ASCS00/exe/sapstartsrv pf=/usr/sap/QAS/SYS/profile/QAS_ASCS00_anftstsapvh -D -u qasadm
   
   # <a name="on-the-node-where-you-installed-the-ers-comment-out-the-following-line"></a>Op het knooppunt waar u de gebruikers hebt geïnstalleerd, moet u de volgende regel
   # <a name="ldlibrarypathusrsapqasers01exeldlibrarypath-export-ldlibrarypath-usrsapqasers01exesapstartsrv-pfusrsapqasers01profileqasers01anftstsapers--d--u-qasadm"></a>LD_LIBRARY_PATH=/usr/sap/QAS/ERS01/exe:$LD_LIBRARY_PATH; export LD_LIBRARY_PATH; /usr/sap/QAS/ERS01/exe/sapstartsrv pf=/usr/sap/QAS/ERS01/profile/QAS_ERS01_anftstsapers -D -u qasadm
   ```

1. **[1]** Create the SAP cluster resources

   If using enqueue server 1 architecture (ENSA1), define the resources as follows:

   ```
   sudo-pc's eigenschap onderhoudsmodus ingesteld = true
   
    sudo-pc's-resource maken rsc_sap_QAS_ASCS00 SAPInstance \
    InstanceName=QAS_ASCS00_anftstsapvh START_PROFILE="/sapmnt/QAS/profile/QAS_ASCS00_anftstsapvh" \
    AUTOMATIC_RECOVER = false \
    META-resource-persistentie = 5000 migratie-drempelwaarde = 1 \
    --groep g-QAS_ASCS
   
    sudo-pc's-resource maken rsc_sap_QAS_ERS01 SAPInstance \
    InstanceName=QAS_ERS01_anftstsapers START_PROFILE="/sapmnt/QAS/profile/QAS_ERS01_anftstsapers" \
    AUTOMATIC_RECOVER = false IS_ERS = true \
    --group g-QAS_AERS
      
    sudo-pc's beperking CO-locatie toevoegen g-QAS_AERS met g-QAS_ASCS-5000 sudo-pc's beperking locatie rsc_sap_QAS_ASCS00 regel score = 2000 runs_ers_QAS eq 1 sudo-pc's beperking volgorde g-QAS_ASCS en g-QAS_AERS type = optioneel symmetrisch = false
    
    eigenschap van sudo-pc's van het knooppunt unstandby anftstsapcl1 sudo-pc's instellen onderhoudsmodus = false
    ```

   SAP introduced support for enqueue server 2, including replication, as of SAP NW 7.52. Starting with ABAP Platform 1809, enqueue server 2 is installed by default. See SAP note [2630416](https://launchpad.support.sap.com/#/notes/2630416) for enqueue server 2 support.
   If using enqueue server 2 architecture ([ENSA2](https://help.sap.com/viewer/cff8531bc1d9416d91bb6781e628d4e0/1709%20001/en-US/6d655c383abf4c129b0e5c8683e7ecd8.html)), install resource agent resource-agents-sap-4.1.1-12.el7.x86_64 or newer and define the resources as follows:

    ```
    sudo-pc's eigenschap onderhoudsmodus ingesteld = true
    
    sudo-pc's-resource maken rsc_sap_QAS_ASCS00 SAPInstance \
    InstanceName=QAS_ASCS00_anftstsapvh START_PROFILE="/sapmnt/QAS/profile/QAS_ASCS00_anftstsapvh" \
    AUTOMATIC_RECOVER = false \
    meta resource-stickiness=5000 \
    --groep g-QAS_ASCS
   
    sudo-pc's-resource maken rsc_sap_QAS_ERS01 SAPInstance \
    InstanceName=QAS_ERS01_anftstsapers START_PROFILE="/sapmnt/QAS/profile/QAS_ERS01_anftstsapers" \
    AUTOMATIC_RECOVER = false IS_ERS = true \
    --group g-QAS_AERS
      
    sudo-pc's beperking CO-locatie toevoegen g-QAS_AERS met g-QAS_ASCS-5000 sudo-pc's beperking volgorde g-QAS_ASCS en g-QAS_AERS type = optioneel symmetrisch = false
   
    eigenschap van sudo-pc's van het knooppunt unstandby anftstsapcl1 sudo-pc's instellen onderhoudsmodus = false
    ```

   If you are upgrading from an older version and switching to enqueue server 2, see SAP note [2641322](https://launchpad.support.sap.com/#/notes/2641322). 

   Make sure that the cluster status is ok and that all resources are started. It is not important on which node the resources are running.

    ```
    status van sudo-pc 's
    
    # <a name="online--anftstsapcl1-anftstsapcl2-"></a>Online: [anftstsapcl1 anftstsapcl2]
    #
    # <a name="full-list-of-resources"></a>Volledige lijst met resources:
    #
    # <a name="rscstazure----stonithfenceazurearm------started-anftstsapcl2"></a>rsc_st_azure (stonith:fence_azure_arm):      Gestarte anftstsapcl2
    #  <a name="resource-group-g-qasascs"></a>Resourcegroep: g-QAS_ASCS
    #      <a name="fsqasascs--------ocfheartbeatfilesystem----started-anftstsapcl2"></a>fs_QAS_ASCS (ocf::heartbeat:Filesystem):    Gestarte anftstsapcl2
    #      <a name="ncqasascs--------ocfheartbeatazure-lb------started-anftstsapcl2"></a>nc_QAS_ASCS (ocf::heartbeat:azure-lb):      Gestarte anftstsapcl2
    #      <a name="vipqasascs-------ocfheartbeatipaddr2-------started-anftstsapcl2"></a>vip_QAS_ASCS (ocf::heartbeat:IPaddr2):       Gestarte anftstsapcl2
    #      <a name="rscsapqasascs00-ocfheartbeatsapinstance---started-anftstsapcl2"></a>rsc_sap_QAS_ASCS00 (ocf::heartbeat:SAPInstance):   Gestarte anftstsapcl2
    #  <a name="resource-group-g-qasaers"></a>Resourcegroep: g-QAS_AERS
    #      <a name="fsqasaers--------ocfheartbeatfilesystem----started-anftstsapcl1"></a>fs_QAS_AERS (ocf::heartbeat:Filesystem):    Gestarte anftstsapcl1
    #      <a name="ncqasaers--------ocfheartbeatazure-lb------started-anftstsapcl1"></a>nc_QAS_AERS        (ocf::heartbeat:azure-lb):      Gestarte anftstsapcl1
    #      <a name="vipqasaers-------ocfheartbeatipaddr2-------started-anftstsapcl1"></a>vip_QAS_AERS (ocf::heartbeat:IPaddr2):       Gestarte anftstsapcl1
    #      <a name="rscsapqasers01--ocfheartbeatsapinstance---started-anftstsapcl1"></a>rsc_sap_QAS_ERS01 (ocf::heartbeat:SAPInstance):   Gestarte anftstsapcl1
   ```

1. **[A]** Add firewall rules for ASCS and ERS on both nodes
   Add the firewall rules for ASCS and ERS on both nodes.
   ```
   # <a name="probe-port-of-ascs"></a>Testpoort van ASCS
   sudo-firewall-cmd--zone openbare--toevoegen-port = = 62000/tcp - permanente sudo firewall-cmd--zone = openbare--toevoegen-poort 62000/tcp sudo firewall-cmd--= zone openbare--toevoegen-port = = 3200/tcp - permanente sudo firewall-cmd--zone openbare--toevoegen-port = 3200/tcp sudo = Firewall-cmd--zone openbare--toevoegen-port = = 3600/tcp--permanente sudo firewall-cmd--zone openbare--toevoegen-port = = 3600/tcp sudo firewall-cmd--zone openbare--toevoegen-port = 3900/tcp--= permanente sudo firewall-cmd--zone openbare--toevoegen-port = 3900/tcp sudo = Firewall-cmd--zone openbare--toevoegen-port = 8100/tcp--= permanente sudo firewall-cmd--zone openbare--toevoegen-port = 8100/tcp sudo firewall-cmd--= zone openbare--toevoegen-port = 50013/tcp--= permanente sudo firewall-cmd--zone openbare--toevoegen-port = 50013/tcp sudo = Firewall-cmd--zone openbare--toevoegen-port = 50014/tcp--= permanente sudo firewall-cmd--zone openbare--toevoegen-port = 50014/tcp sudo firewall-cmd--= zone openbare--toevoegen-port = 50016/tcp--= permanente sudo firewall-cmd--zone openbare--toevoegen-port = = 50016/tcp
   # <a name="probe-port-of-ers"></a>Testpoort van gebruikers
   sudo firewall-cmd--zone openbare--toevoegen-port = 62101/tcp--= permanente sudo firewall-cmd--zone openbare--toevoegen-port = 62101/tcp sudo firewall-cmd--= zone openbare--toevoegen-port = 3301/tcp--= permanente sudo firewall-cmd--zone openbare--toevoegen-port = 3301/tcp sudo = Firewall-cmd--zone openbare--toevoegen-port = 50113/tcp--= permanente sudo firewall-cmd--zone openbare--toevoegen-port = 50113/tcp sudo firewall-cmd--= zone openbare--toevoegen-port = 50114/tcp--= permanente sudo firewall-cmd--zone openbare--toevoegen-port = 50114/tcp sudo = Firewall-cmd--zone openbare--toevoegen-port = 50116/tcp--= permanente sudo firewall-cmd--zone openbare--toevoegen-port = = 50116/tcp
   ```

## <a name="2d6008b0-685d-426c-b59e-6cd281fd45d7"></a>SAP NetWeaver application server preparation

   Some databases require that the database instance installation is executed on an application server. Prepare the application server virtual machines to be able to use them in these cases.  

   The steps bellow assume that you install the application server on a server different from the ASCS/SCS and HANA servers. Otherwise some of the steps below (like configuring host name resolution) are not needed.  

   The following items are prefixed with either **[A]** - applicable to both PAS and AAS, **[P]** - only applicable to PAS or **[S]** - only applicable to AAS.  

1. **[A]** Setup host name resolution
   You can either use a DNS server or modify the /etc/hosts on all nodes. This example shows how to use the /etc/hosts file.
   Replace the IP address and the hostname in the following commands:  

   ```
   sudo vi/etc/hosts
   ```

   Insert the following lines to /etc/hosts. Change the IP address and hostname to match your environment.

   ```
   # <a name="ip-address-of-the-load-balancer-frontend-configuration-for-sap-netweaver-ascs"></a>IP-adres van de load balancer-frontend-configuratie voor SAP NetWeaver ASCS
   192.168.14.9 anftstsapvh
   # <a name="ip-address-of-the-load-balancer-frontend-configuration-for-sap-netweaver-ascs-ers"></a>IP-adres van de load balancer-frontend-configuratie voor SAP NetWeaver ASCS INGEN
   192.168.14.10 anftstsapers 192.168.14.7 anftstsapa01 192.168.14.8 anftstsapa02
   ```

1. **[A]** Create the sapmnt directory
   Create the sapmnt directory.
   ```
   sudo mkdir -p/sapmnt/QAS sudo mkdir -p /usr/sap/trans

   sudo chattr + ik/sapmnt/QAS sudo chattr + ik /usr/sap/trans
   ```

1. **[A]** Install NFS client and other requirements  

   ```
   sudo yum -y installeren nfs-utils uuidd
   ```

1. **[A]** Add mount entries  

   ```
   sudo vi/etc/fstab
   
   # <a name="add-the-following-lines-to-fstab-save-and-exit"></a>De volgende regels toevoegen aan fstab, opslaan en sluiten
   192.168.24.5:/sapQAS/sapmntQAS/sapmnt/QAS nfs rw, hard, rsize = 65536, wsize = 65536, bergen = 3 192.168.24.4:/transSAP /usr/sap/trans nfs rw, hard, rsize = 65536, wsize = 65536, bergen = 3
   ```

   Mount the new shares

   ```
   sudo mount - een
   ```

1. **[P]** Create and mount the PAS directory  

   ```
   sudo mkdir -p /usr/sap/QAS/D02 sudo chattr + ik /usr/sap/QAS/D02
   
   sudo vi/etc/fstab
   # <a name="add-the-following-line-to-fstab"></a>Voeg de volgende regel toe fstab
   92.168.24.5:/sapQAS/usrsapQASpas /usr/sap/QAS/D02 nfs rw, hard, rsize = 65536, wsize = 65536, bergen = 3
   
   # <a name="mount"></a>Koppelen
   sudo mount - een
   ```

1. **[S]** Create and mount the AAS directory  

   ```
   sudo mkdir -p /usr/sap/QAS/D03 sudo chattr + ik /usr/sap/QAS/D03
   
   sudo vi/etc/fstab
   # <a name="add-the-following-line-to-fstab"></a>Voeg de volgende regel toe fstab
   92.168.24.5:/sapQAS/usrsapQASaas /usr/sap/QAS/D03 nfs rw, hard, rsize = 65536, wsize = 65536, bergen = 3
   
   # <a name="mount"></a>Koppelen
   sudo mount - een
   ```


1. **[A]** Configure SWAP file
 
   ```
   sudo vi /etc/waagent.conf
   
   # <a name="set-the-property-resourcediskenableswap-to-y"></a>Stel de eigenschap ResourceDisk.EnableSwap op y
   # <a name="create-and-use-swapfile-on-resource-disk"></a>Maak en gebruik van wisselbestand op de bronschijf.
   ResourceDisk.EnableSwap=y
   
   # <a name="set-the-size-of-the-swap-file-with-property-resourcediskswapsizemb"></a>De grootte van het wisselbestand met eigenschap ResourceDisk.SwapSizeMB instellen
   # <a name="the-free-space-of-resource-disk-varies-by-virtual-machine-size-make-sure-that-you-do-not-set-a-value-that-is-too-big-you-can-check-the-swap-space-with-command-swapon"></a>De vrije ruimte van de bronschijf is afhankelijk van de grootte van virtuele machine. Zorg ervoor dat u niet stelt een waarde die is te groot. U kunt de wisselruimte met opdracht swapon controleren
   # <a name="size-of-the-swapfile"></a>Grootte van het wisselbestand.
   ResourceDisk.SwapSizeMB=2000
   ```

   Restart the Agent to activate the change

   ```
   sudo-service waagent opnieuw opstarten
   ```

## Install database

In this example, SAP NetWeaver is installed on SAP HANA. You can use every supported database for this installation. For more information on how to install SAP HANA in Azure, see [High availability of SAP HANA on Azure VMs on Red Hat Enterprise Linux][sap-hana-ha]. For a list of supported databases, see [SAP Note 1928533][1928533].

1. Run the SAP database instance installation

   Install the SAP NetWeaver database instance as root using a virtual hostname that maps to the IP address of the load balancer frontend configuration for the database.

   You can use the sapinst parameter SAPINST_REMOTE_ACCESS_USER to allow a non-root user to connect to sapinst.

   ```
   sudo <swpm>/sapinst SAPINST_REMOTE_ACCESS_USER sapadmin =
   ```

## SAP NetWeaver application server installation

Follow these steps to install an SAP application server.

1. Prepare application server

   Follow the steps in the chapter [SAP NetWeaver application server preparation](high-availability-guide-rhel.md#2d6008b0-685d-426c-b59e-6cd281fd45d7) above to prepare the application server.

1. Install SAP NetWeaver application server

   Install a primary or additional SAP NetWeaver applications server.

   You can use the sapinst parameter SAPINST_REMOTE_ACCESS_USER to allow a non-root user to connect to sapinst.

   ```
   sudo <swpm>/sapinst SAPINST_REMOTE_ACCESS_USER sapadmin =
   ```

1. Update SAP HANA secure store

   Update the SAP HANA secure store to point to the virtual name of the SAP HANA System Replication setup.

   Run the following command to list the entries as \<sapsid>adm

   ```
   hdbuserstore lijst
   ```

   This should list all entries and should look similar to
   ```
   GEGEVENS voor bestand: /home/qasadm/.hdb/anftstsapa01/SSFS_HDB. DAT SLEUTELBESTAND: /home/qasadm/.hdb/anftstsapa01/SSFS_HDB. SLEUTEL
   
   BELANGRIJKSTE STANDAARD ENV: 192.168.14.4:30313 GEBRUIKER: SAPABAP1   DATABASE: QAS
   ```

   The output shows that the IP address of the default entry is pointing to the virtual machine and not to the load balancer's IP address. This entry needs to be changed to point to the virtual hostname of the load balancer. Make sure to use the same port (**30313** in the output above) and database name (**QAS** in the output above)!

   ```
   Su - qasadm hdbuserstore standaard qasdb:30313@QAS SAPABAP1 <password of ABAP schema>
   ```

## Test the cluster setup

1. Manually migrate the ASCS instance

   Resource state before starting the test:

   ```
    rsc_st_azure (stonith:fence_azure_arm):      Aan de slag anftstsapcl1 resourcegroep: g-QAS_ASCS fs_QAS_ASCS (ocf::heartbeat:Filesystem):    Aan de slag anftstsapcl1 nc_QAS_ASCS (ocf::heartbeat:azure-lb):      Gestarte anftstsapcl1 vip_QAS_ASCS (ocf::heartbeat:IPaddr2):       Aan de slag anftstsapcl1 rsc_sap_QAS_ASCS00 (ocf::heartbeat:SAPInstance):   Aan de slag anftstsapcl1 resourcegroep: g-QAS_AERS fs_QAS_AERS (ocf::heartbeat:Filesystem):    Aan de slag anftstsapcl2 nc_QAS_AERS (ocf::heartbeat:azure-lb):      Gestarte anftstsapcl2 vip_QAS_AERS (ocf::heartbeat:IPaddr2):       Aan de slag anftstsapcl2 rsc_sap_QAS_ERS01 (ocf::heartbeat:SAPInstance):   Gestarte anftstsapcl2
   ```

   Run the following commands as root to migrate the ASCS instance.

   ```
   [root@anftstsapcl1 ~] # pc's resourceverplaatsing rsc_sap_QAS_ASCS00
   
   [root@anftstsapcl1 ~] # pc's resource rsc_sap_QAS_ASCS00 wissen
   
   # <a name="remove-failed-actions-for-the-ers-that-occurred-as-part-of-the-migration"></a>Mislukte acties voor de gebruikers die zijn opgetreden als onderdeel van de migratie verwijderen
   [root@anftstsapcl1 ~] # pc's resource opruimen rsc_sap_QAS_ERS01
   ```

   Resource state after the test:

   ```
   rsc_st_azure (stonith:fence_azure_arm):      Aan de slag anftstsapcl1 resourcegroep: g-QAS_ASCS fs_QAS_ASCS (ocf::heartbeat:Filesystem):    Aan de slag anftstsapcl2 nc_QAS_ASCS (ocf::heartbeat:azure-lb):      Gestarte anftstsapcl2 vip_QAS_ASCS (ocf::heartbeat:IPaddr2):       Aan de slag anftstsapcl2 rsc_sap_QAS_ASCS00 (ocf::heartbeat:SAPInstance):   Aan de slag anftstsapcl2 resourcegroep: g-QAS_AERS fs_QAS_AERS (ocf::heartbeat:Filesystem):    Aan de slag anftstsapcl1 nc_QAS_AERS (ocf::heartbeat:azure-lb):      Gestarte anftstsapcl1 vip_QAS_AERS (ocf::heartbeat:IPaddr2):       Aan de slag anftstsapcl1 rsc_sap_QAS_ERS01 (ocf::heartbeat:SAPInstance):   Gestarte anftstsapcl1
   ```

1. Simulate node crash

   Resource state before starting the test:

   ```
   rsc_st_azure (stonith:fence_azure_arm):      Aan de slag anftstsapcl1 resourcegroep: g-QAS_ASCS fs_QAS_ASCS (ocf::heartbeat:Filesystem):    Aan de slag anftstsapcl2 nc_QAS_ASCS (ocf::heartbeat:azure-lb):      Gestarte anftstsapcl2 vip_QAS_ASCS (ocf::heartbeat:IPaddr2):       Aan de slag anftstsapcl2 rsc_sap_QAS_ASCS00 (ocf::heartbeat:SAPInstance):   Aan de slag anftstsapcl2 resourcegroep: g-QAS_AERS fs_QAS_AERS (ocf::heartbeat:Filesystem):    Aan de slag anftstsapcl1 nc_QAS_AERS (ocf::heartbeat:azure-lb):      Gestarte anftstsapcl1 vip_QAS_AERS (ocf::heartbeat:IPaddr2):       Aan de slag anftstsapcl1 rsc_sap_QAS_ERS01 (ocf::heartbeat:SAPInstance):   Gestarte anftstsapcl1
   ```

   Run the following command as root on the node where the ASCS instance is running

   ```
   [root@anftstsapcl2 ~]# echo b > /proc/sysrq-trigger
   ```

   The status after the node is started again should look like this.

   ```
   Online: [anftstsapcl1 anftstsapcl2]
   
   Volledige lijst met resources:
   
   rsc_st_azure (stonith:fence_azure_arm):      Aan de slag anftstsapcl1 resourcegroep: g-QAS_ASCS fs_QAS_ASCS (ocf::heartbeat:Filesystem):    Aan de slag anftstsapcl1 nc_QAS_ASCS (ocf::heartbeat:azure-lb):      Gestarte anftstsapcl1 vip_QAS_ASCS (ocf::heartbeat:IPaddr2):       Aan de slag anftstsapcl1 rsc_sap_QAS_ASCS00 (ocf::heartbeat:SAPInstance):   Aan de slag anftstsapcl1 resourcegroep: g-QAS_AERS fs_QAS_AERS (ocf::heartbeat:Filesystem):    Aan de slag anftstsapcl2 nc_QAS_AERS (ocf::heartbeat:azure-lb):      Gestarte anftstsapcl2 vip_QAS_AERS (ocf::heartbeat:IPaddr2):       Aan de slag anftstsapcl2 rsc_sap_QAS_ERS01 (ocf::heartbeat:SAPInstance):   Gestarte anftstsapcl2
   
   Mislukte acties:
   * rsc_sap_QAS_ERS01_monitor_11000 op anftstsapcl1 'niet wordt uitgevoerd' (7): aanroepen = 45, status = volledig, exitreason ='',
   ```

   Use the following command to clean the failed resources.

   ```
   [root@anftstsapcl1 ~] # pc's resource opruimen rsc_sap_QAS_ERS01
   ```

   Resource state after the test:

   ```
   rsc_st_azure (stonith:fence_azure_arm):      Aan de slag anftstsapcl1 resourcegroep: g-QAS_ASCS fs_QAS_ASCS (ocf::heartbeat:Filesystem):    Aan de slag anftstsapcl1 nc_QAS_ASCS (ocf::heartbeat:azure-lb):      Gestarte anftstsapcl1 vip_QAS_ASCS (ocf::heartbeat:IPaddr2):       Aan de slag anftstsapcl1 rsc_sap_QAS_ASCS00 (ocf::heartbeat:SAPInstance):   Aan de slag anftstsapcl1 resourcegroep: g-QAS_AERS fs_QAS_AERS (ocf::heartbeat:Filesystem):    Aan de slag anftstsapcl2 nc_QAS_AERS (ocf::heartbeat:azure-lb):      Gestarte anftstsapcl2 vip_QAS_AERS (ocf::heartbeat:IPaddr2):       Aan de slag anftstsapcl2 rsc_sap_QAS_ERS01 (ocf::heartbeat:SAPInstance):   Gestarte anftstsapcl2
   ```

1. Kill message server process

   Resource state before starting the test:

   ```
   rsc_st_azure (stonith:fence_azure_arm):      Aan de slag anftstsapcl1 resourcegroep: g-QAS_ASCS fs_QAS_ASCS (ocf::heartbeat:Filesystem):    Aan de slag anftstsapcl1 nc_QAS_ASCS (ocf::heartbeat:azure-lb):      Gestarte anftstsapcl1 vip_QAS_ASCS (ocf::heartbeat:IPaddr2):       Aan de slag anftstsapcl1 rsc_sap_QAS_ASCS00 (ocf::heartbeat:SAPInstance):   Aan de slag anftstsapcl1 resourcegroep: g-QAS_AERS fs_QAS_AERS (ocf::heartbeat:Filesystem):    Aan de slag anftstsapcl2 nc_QAS_AERS (ocf::heartbeat:azure-lb):      Gestarte anftstsapcl2 vip_QAS_AERS (ocf::heartbeat:IPaddr2):       Aan de slag anftstsapcl2 rsc_sap_QAS_ERS01 (ocf::heartbeat:SAPInstance):   Gestarte anftstsapcl2
   ```
   
   Run the following commands as root to identify the process of the message server and kill it.

   ```
   [root@anftstsapcl1 ~]# pgrep ms.sapQAS | xargs kill -9
   ```

   If you only kill the message server once, it will be restarted by `sapstart`. If you kill it often enough, Pacemaker will eventually move the ASCS instance to the other node. Run the following commands as root to clean up the resource state of the ASCS and ERS instance after the test.

   ```
   [root@anftstsapcl1 ~] # pc's resource opschonen rsc_sap_QAS_ASCS00 [root@anftstsapcl1 ~] # pc's resource opruimen rsc_sap_QAS_ERS01
   ```

   Resource state after the test:

   ```
   rsc_st_azure (stonith:fence_azure_arm):      Aan de slag anftstsapcl1 resourcegroep: g-QAS_ASCS fs_QAS_ASCS (ocf::heartbeat:Filesystem):    Aan de slag anftstsapcl2 nc_QAS_ASCS (ocf::heartbeat:azure-lb):      Gestarte anftstsapcl2 vip_QAS_ASCS (ocf::heartbeat:IPaddr2):       Aan de slag anftstsapcl2 rsc_sap_QAS_ASCS00 (ocf::heartbeat:SAPInstance):   Aan de slag anftstsapcl2 resourcegroep: g-QAS_AERS fs_QAS_AERS (ocf::heartbeat:Filesystem):    Aan de slag anftstsapcl1 nc_QAS_AERS (ocf::heartbeat:azure-lb):      Gestarte anftstsapcl1 vip_QAS_AERS (ocf::heartbeat:IPaddr2):       Aan de slag anftstsapcl1 rsc_sap_QAS_ERS01 (ocf::heartbeat:SAPInstance):   Gestarte anftstsapcl1
   ```

1. Kill enqueue server process

   Resource state before starting the test:

   ```
   rsc_st_azure (stonith:fence_azure_arm):      Aan de slag anftstsapcl1 resourcegroep: g-QAS_ASCS fs_QAS_ASCS (ocf::heartbeat:Filesystem):    Aan de slag anftstsapcl2 nc_QAS_ASCS (ocf::heartbeat:azure-lb):      Gestarte anftstsapcl2 vip_QAS_ASCS (ocf::heartbeat:IPaddr2):       Aan de slag anftstsapcl2 rsc_sap_QAS_ASCS00 (ocf::heartbeat:SAPInstance):   Aan de slag anftstsapcl2 resourcegroep: g-QAS_AERS fs_QAS_AERS (ocf::heartbeat:Filesystem):    Aan de slag anftstsapcl1 nc_QAS_AERS (ocf::heartbeat:azure-lb):      Gestarte anftstsapcl1 vip_QAS_AERS (ocf::heartbeat:IPaddr2):       Aan de slag anftstsapcl1 rsc_sap_QAS_ERS01 (ocf::heartbeat:SAPInstance):   Gestarte anftstsapcl1
   ```

   Run the following commands as root on the node where the ASCS instance is running to kill the enqueue server.

   ```
   [root@anftstsapcl2 ~]# pgrep en.sapQAS | xargs kill -9
   ```

   The ASCS instance should immediately fail over to the other node. The ERS instance should also fail over after the ASCS instance is started. Run the following commands as root to clean up the resource state of the ASCS and ERS instance after the test.

   ```
   [root@anftstsapcl2 ~] # pc's resource opschonen rsc_sap_QAS_ASCS00 [root@anftstsapcl2 ~] # pc's resource opruimen rsc_sap_QAS_ERS01
   ```

   Resource state after the test:

   ```
   rsc_st_azure (stonith:fence_azure_arm):      Aan de slag anftstsapcl1 resourcegroep: g-QAS_ASCS fs_QAS_ASCS (ocf::heartbeat:Filesystem):    Aan de slag anftstsapcl1 nc_QAS_ASCS (ocf::heartbeat:azure-lb):      Gestarte anftstsapcl1 vip_QAS_ASCS (ocf::heartbeat:IPaddr2):       Aan de slag anftstsapcl1 rsc_sap_QAS_ASCS00 (ocf::heartbeat:SAPInstance):   Aan de slag anftstsapcl1 resourcegroep: g-QAS_AERS fs_QAS_AERS (ocf::heartbeat:Filesystem):    Aan de slag anftstsapcl2 nc_QAS_AERS (ocf::heartbeat:azure-lb):      Gestarte anftstsapcl2 vip_QAS_AERS (ocf::heartbeat:IPaddr2):       Aan de slag anftstsapcl2 rsc_sap_QAS_ERS01 (ocf::heartbeat:SAPInstance):   Gestarte anftstsapcl2
   ```

1. Kill enqueue replication server process

   Resource state before starting the test:

   ```
   rsc_st_azure (stonith:fence_azure_arm):      Aan de slag anftstsapcl1 resourcegroep: g-QAS_ASCS fs_QAS_ASCS (ocf::heartbeat:Filesystem):    Aan de slag anftstsapcl1 nc_QAS_ASCS (ocf::heartbeat:azure-lb):      Gestarte anftstsapcl1 vip_QAS_ASCS (ocf::heartbeat:IPaddr2):       Aan de slag anftstsapcl1 rsc_sap_QAS_ASCS00 (ocf::heartbeat:SAPInstance):   Aan de slag anftstsapcl1 resourcegroep: g-QAS_AERS fs_QAS_AERS (ocf::heartbeat:Filesystem):    Aan de slag anftstsapcl2 nc_QAS_AERS (ocf::heartbeat:azure-lb):      Gestarte anftstsapcl2 vip_QAS_AERS (ocf::heartbeat:IPaddr2):       Aan de slag anftstsapcl2 rsc_sap_QAS_ERS01 (ocf::heartbeat:SAPInstance):   Gestarte anftstsapcl2
   ```

   Run the following command as root on the node where the ERS instance is running to kill the enqueue replication server process.

   ```
   [root@anftstsapcl2 ~]# pgrep er.sapQAS | xargs kill -9
   ```

   If you only run the command once, `sapstart` will restart the process. If you run it often enough, `sapstart` will not restart the process and the resource will be in a stopped state. Run the following commands as root to clean up the resource state of the ERS instance after the test.

   ```
   [root@anftstsapcl2 ~] # pc's resource opruimen rsc_sap_QAS_ERS01
   ```

   Resource state after the test:

   ```
   rsc_st_azure (stonith:fence_azure_arm):      Aan de slag anftstsapcl1 resourcegroep: g-QAS_ASCS fs_QAS_ASCS (ocf::heartbeat:Filesystem):    Aan de slag anftstsapcl1 nc_QAS_ASCS (ocf::heartbeat:azure-lb):      Gestarte anftstsapcl1 vip_QAS_ASCS (ocf::heartbeat:IPaddr2):       Aan de slag anftstsapcl1 rsc_sap_QAS_ASCS00 (ocf::heartbeat:SAPInstance):   Aan de slag anftstsapcl1 resourcegroep: g-QAS_AERS fs_QAS_AERS (ocf::heartbeat:Filesystem):    Aan de slag anftstsapcl2 nc_QAS_AERS (ocf::heartbeat:azure-lb):      Gestarte anftstsapcl2 vip_QAS_AERS (ocf::heartbeat:IPaddr2):       Aan de slag anftstsapcl2 rsc_sap_QAS_ERS01 (ocf::heartbeat:SAPInstance):   Gestarte anftstsapcl2
   ```

1. Kill enqueue sapstartsrv process

   Resource state before starting the test:

   ```
   rsc_st_azure (stonith:fence_azure_arm):      Aan de slag anftstsapcl1 resourcegroep: g-QAS_ASCS fs_QAS_ASCS (ocf::heartbeat:Filesystem):    Aan de slag anftstsapcl1 nc_QAS_ASCS (ocf::heartbeat:azure-lb):      Gestarte anftstsapcl1 vip_QAS_ASCS (ocf::heartbeat:IPaddr2):       Aan de slag anftstsapcl1 rsc_sap_QAS_ASCS00 (ocf::heartbeat:SAPInstance):   Aan de slag anftstsapcl1 resourcegroep: g-QAS_AERS fs_QAS_AERS (ocf::heartbeat:Filesystem):    Aan de slag anftstsapcl2 nc_QAS_AERS (ocf::heartbeat:azure-lb):      Gestarte anftstsapcl2 vip_QAS_AERS (ocf::heartbeat:IPaddr2):       Aan de slag anftstsapcl2 rsc_sap_QAS_ERS01 (ocf::heartbeat:SAPInstance):   Gestarte anftstsapcl2
   ```

   Run the following commands as root on the node where the ASCS is running.

   ```
   [root@anftstsapcl1 ~] # pgrep -fl ASCS00.*sapstartsrv
   # <a name="59545-sapstartsrv"></a>59545 sapstartsrv
   
   [root@anftstsapcl1 ~]# kill -9 59545
   ```

   The sapstartsrv process should always be restarted by the Pacemaker resource agent as part of the monitoring. Resource state after the test:

   ```
   rsc_st_azure (stonith:fence_azure_arm):      Aan de slag anftstsapcl1 resourcegroep: g-QAS_ASCS fs_QAS_ASCS (ocf::heartbeat:Filesystem):    Aan de slag anftstsapcl1 nc_QAS_ASCS (ocf::heartbeat:azure-lb):      Gestarte anftstsapcl1 vip_QAS_ASCS (ocf::heartbeat:IPaddr2):       Aan de slag anftstsapcl1 rsc_sap_QAS_ASCS00 (ocf::heartbeat:SAPInstance):   Aan de slag anftstsapcl1 resourcegroep: g-QAS_AERS fs_QAS_AERS (ocf::heartbeat:Filesystem):    Aan de slag anftstsapcl2 nc_QAS_AERS (ocf::heartbeat:azure-lb):      Gestarte anftstsapcl2 vip_QAS_AERS (ocf::heartbeat:IPaddr2):       Aan de slag anftstsapcl2 rsc_sap_QAS_ERS01 (ocf::heartbeat:SAPInstance):   Gestarte anftstsapcl2
   ```

## Next steps

* [Azure Virtual Machines planning and implementation for SAP][planning-guide]
* [Azure Virtual Machines deployment for SAP][deployment-guide]
* [Azure Virtual Machines DBMS deployment for SAP][dbms-guide]
* To learn how to establish high availability and plan for disaster recovery of SAP HANA on Azure (large instances), see [SAP HANA (large instances) high availability and disaster recovery on Azure](hana-overview-high-availability-disaster-recovery.md).
* To learn how to establish high availability and plan for disaster recovery of SAP HANA on Azure VMs, see [High Availability of SAP HANA on Azure Virtual Machines (VMs)][sap-hana-ha]
