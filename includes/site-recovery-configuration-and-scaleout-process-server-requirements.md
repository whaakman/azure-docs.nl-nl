---
title: bestand opnemen
description: bestand opnemen
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: include
ms.date: 06/10/2018
ms.author: raynew
ms.custom: include file
ms.openlocfilehash: f6053c4ef1fe9ce7cc3f06804822afa98055a645
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37967678"
---
**Configuratieproces/server-vereisten**

**Onderdeel** | **Vereiste** 
--- | ---
**HARDWARE-INSTELLINGEN** | 
CPU-kernen | 8 
RAM | 16 GB
Aantal schijven | 3, met inbegrip van de OS-schijf, cacheschijf proces en bewaarstation voor failback 
Vrije schijfruimte (cache van de processerver) | 600 GB
Vrije schijfruimte (bewaarschijf) | 600 GB
 | 
**INSTELLINGEN VOOR SOFTWARE** | 
Besturingssysteem | Windows Server 2012 R2 <br> Windows Server 2016
Landinstelling van het besturingssysteem | Engels (en-us)
Windows Server-functies | Deze rollen niet worden ingeschakeld: <br> - Active Directory Domain Services <br>- Internet Information Services <br> - Hyper-V 
Groepsbeleid | Geen groepsbeleid voor deze inschakelen: <br> -Toegang tot de opdrachtprompt voorkomen. <br> -Toegang tot registerbewerkingsprogramma's voorkomen. <br> -Logica vertrouwen voor bestandsbijlagen. <br> -Uitvoering van Script inschakelen. <br> [Meer informatie](https://technet.microsoft.com/library/gg176671(v=ws.10).aspx)
IIS | -Er zijn geen bestaande standaardwebsite <br> -Er zijn geen bestaande website/toepassing luistert op poort 443 <br>-Inschakelen [anonieme verificatie](https://technet.microsoft.com/library/cc731244(v=ws.10).aspx) <br> -Inschakelen [FastCGI](https://technet.microsoft.com/library/cc753077(v=ws.10).aspx) instelling 
| 
**NETWERKINSTELLINGEN** | 
Type IP-adres | Statisch 
Toegang tot het internet | De server moet toegang hebben tot deze URL's (rechtstreeks of via proxy) <br> - \*.accesscontrol.windows.net<br> - \*.backup.windowsazure.com <br>- \*.store.core.windows.net<br> - \*.blob.core.windows.net<br> - \*.hypervrecoverymanager.windowsazure.com  <br> - https://management.azure.com <br> -*. services.visualstudio.com <br> - time.nist.gov <br> - time.windows.com <br> OVF moet ook toegang tot de volgende URL 's <br> - https://login.microsoftonline.com <br> - https://secure.aadcdn.microsoftonline-p.com <br> - https://login.live.com  <br> - https://auth.gfx.ms <br> - https://graph.windows.net <br> - https://login.windows.net <br> - https://www.live.com <br> - https://www.microsoft.com <br> - https://dev.mysql.com/get/Downloads/MySQLInstaller/mysql-installer-community-5.7.20.0.msi 
Poorten | 443 (Orchestration-besturingselement)<br>9443 (Gegevenstransport) 
Type NIC | VMXNET3 (als de configuratieserver een VMware-VM is)
 | 
**SOFTWARE TE INSTALLEREN** | 
VMware vSphere PowerCLI | [PowerCLI versie 6.0](https://my.vmware.com/web/vmware/details?productId=491&downloadGroup=PCLI600R1) moet worden geïnstalleerd als de configuratieserver wordt uitgevoerd op een VMware-VM.
MYSQL | MySQL moet worden geïnstalleerd. U kunt ook handmatig installeren of Site Recovery kunt u deze installeren.

**Configuratie-/ processerver formaat vereisten**

**CPU** | **Geheugen** | **Cacheschijf** | **Veranderingssnelheid van gegevens** | **Gerepliceerde machines**
--- | --- | --- | --- | ---
8 vcpu 's<br/><br/> 2-sockets * 4 kernen @ 2,5 GHz | 16GB | 300 GB | 500 GB of minder | < 100 machines
12 vcpu 's<br/><br/> 2 socks * 6 kernen @ 2,5 GHz | 18 GB | 600 GB | 500 GB - 1 TB | 100-150-machines
16 vcpu 's<br/><br/> socks 2 * 8 kernen @ 2,5 GHz | 32 GB | 1 TB | 1-2 TB | 150 -200-machines

