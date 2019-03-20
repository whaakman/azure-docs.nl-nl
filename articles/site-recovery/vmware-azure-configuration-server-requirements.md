---
title: Configuratie van serververeisten voor noodherstel van VMware naar Azure met Azure Site Recovery | Microsoft Docs
description: In dit artikel beschrijft vereisten en ondersteuning bij het implementeren van de configuratieserver voor noodherstel van VMware naar Azure met Azure Site Recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
services: site-recovery
ms.topic: article
ms.date: 12/31/2018
ms.author: raynew
ms.openlocfilehash: 92eef714f651ef0ce4e58fcfbb8fad75a38c55c8
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58121329"
---
# <a name="configuration-server-requirements-for-vmware-disaster-recovery-to-azure"></a>Configuratie van serververeisten voor noodherstel van VMware naar Azure

U implementeert een on-premises configuratieserver wanneer u [Azure Site Recovery](site-recovery-overview.md) voor herstel na noodgevallen van virtuele VMware-machines en fysieke servers naar Azure.

- De configuratie van server coördinaten communicatie tussen on-premises VMware en Azure. Deze beheert de gegevensreplicatie.
- De configuratie van server coördinaten communicatie tussen on-premises VMware en Azure. Deze beheert de gegevensreplicatie.
- [Meer informatie](vmware-azure-architecture.md) over de onderdelen van de configuratieserver en processen.

## <a name="configuration-server-deployment"></a>Implementatie van configuratieserver

Voor herstel na noodgevallen van virtuele VMware-machines naar Azure, kunt u de configuratieserver implementeren als een VMware-VM.

- Site Recovery biedt een OVA-sjabloon die u downloaden vanuit de Azure-portal en importeren in de vCenter-Server voor het instellen van de virtuele machine van de configuratieserver.
- Wanneer u de configuratieserver met behulp van het OVA-sjabloon implementeert, wordt de virtuele machine automatisch voldoet aan de vereisten die worden vermeld in dit artikel.
- Het is raadzaam dat u de configuratieserver met behulp van het OVA-sjabloon instellen. Echter, als u herstel na noodgevallen voor VMware-VM's worden ingesteld en de OVA-sjabloon niet gebruiken, kunt u implementeren de configuratie van server met [deze instructies](physical-azure-set-up-source.md).
- Als u de configuratieserver voor herstel na noodgevallen van on-premises fysieke machines naar Azure implementeert, volgt u de instructies in [in dit artikel](physical-azure-set-up-source.md). 


## <a name="hardware-requirements"></a>Hardwarevereisten

**Onderdeel** | **Vereiste** 
--- | ---
CPU-kernen | 8 
RAM | 16 GB
Aantal schijven | 3, met inbegrip van de OS-schijf, cacheschijf proces en bewaarstation voor failback 
Vrije schijfruimte (cache van de processerver) | 600 GB
Vrije schijfruimte (bewaarschijf) | 600 GB

## <a name="software-requirements"></a>Softwarevereisten

**Onderdeel** | **Vereiste** 
--- | ---
Besturingssysteem | Windows Server 2012 R2 <br> Windows Server 2016
Landinstelling van het besturingssysteem | Engels (en-us)
Windows Server-functies | Deze rollen niet worden ingeschakeld: <br> - Active Directory Domain Services <br>- Internet Information Services <br> - Hyper-V 
Groepsbeleid | Geen groepsbeleid voor deze inschakelen: <br> -Toegang tot de opdrachtprompt voorkomen. <br> -Toegang tot registerbewerkingsprogramma's voorkomen. <br> -Logica vertrouwen voor bestandsbijlagen. <br> -Uitvoering van Script inschakelen. <br> [Meer informatie](https://technet.microsoft.com/library/gg176671(v=ws.10).aspx)
IIS | -Er zijn geen bestaande standaardwebsite <br> -Er zijn geen bestaande website/toepassing luistert op poort 443 <br>-Inschakelen [anonieme verificatie](https://technet.microsoft.com/library/cc731244(v=ws.10).aspx) <br> -Inschakelen [FastCGI](https://technet.microsoft.com/library/cc753077(v=ws.10).aspx) instelling 

## <a name="network-requirements"></a>Netwerkvereisten

**Onderdeel** | **Vereiste** 
--- | --- 
Type IP-adres | Statisch 
Toegang tot het internet | De server moet toegang hebben tot deze URL's (rechtstreeks of via proxy): <br> - \*.accesscontrol.windows.net<br> - \*.backup.windowsazure.com <br>- \*.store.core.windows.net<br> - \*.blob.core.windows.net<br> - \*.hypervrecoverymanager.windowsazure.com  <br> -https:\//management.azure.com <br> - *.services.visualstudio.com <br> - time.nist.gov <br> - time.windows.com <br> OVF moet ook toegang hebben tot de volgende URL's: <br> -https:\//login.microsoftonline.com <br> - https:\//secure.aadcdn.microsoftonline-p.com <br> -https:\//login.live.com  <br> -https:\//auth.gfx.ms <br> -https:\//graph.windows.net <br> -https:\//login.windows.net <br> -https:\//www.live.com <br> -https:\//www.microsoft.com <br> - https:\//dev.mysql.com/get/Downloads/MySQLInstaller/mysql-installer-community-5.7.20.0.msi 
Poorten | 443 (Orchestration-besturingselement)<br>9443 (Gegevenstransport) 
Type NIC | VMXNET3 (als de configuratieserver een VMware-VM is)

## <a name="required-software"></a>Vereiste software

**Onderdeel** | **Vereiste** 
--- | ---
VMware vSphere PowerCLI | [PowerCLI versie 6.0](https://my.vmware.com/web/vmware/details?productId=491&downloadGroup=PCLI600R1) moet worden geïnstalleerd als de configuratieserver wordt uitgevoerd op een VMware-VM.
MYSQL | MySQL moet worden geïnstalleerd. U kunt ook handmatig installeren of Site Recovery kunt u deze installeren.

## <a name="sizing-and-capacity-requirements"></a>Grootte en capaciteitsvereisten van opslaggroepen

De volgende tabel geeft een overzicht van vereisten voor hardwarecapaciteit voor de configuratieserver. Als u meerdere virtuele VMware-machines repliceert, wordt u aangeraden de [overwegingen voor capaciteitsplanning](site-recovery-plan-capacity-vmware.md), en voer de [Azure Site Recovery Deployment Planner](site-recovery-deployment-planner.md) hulpprogramma voor VMWare replication.read 

**Onderdeel** | **Vereiste** 
--- | ---

| **CPU** | **Geheugen** | **Cacheschijf** | **Veranderingssnelheid van gegevens** | **Gerepliceerde machines** |
| --- | --- | --- | --- | --- |
| 8 vcpu 's<br/><br/> 2-sockets * 4 kernen \@ 2,5 GHz | 16 GB | 300 GB | 500 GB of minder | Les dan 100 machines |
| 12 vcpu 's<br/><br/> 2 socks * 6 kernen \@ 2,5 GHz | 18 GB | 600 GB | 500 GB-1 TB | 100-150-machines |
| 16 vcpu 's<br/><br/> socks 2 * 8 kernen \@ 2,5 GHz | 32 GB | 1 TB | 1-2 TB | 150 tot 200-machines | 



## <a name="next-steps"></a>Volgende stappen
Instellen van herstel na noodgevallen van [virtuele VMware-machines](vmware-azure-tutorial.md) naar Azure.
