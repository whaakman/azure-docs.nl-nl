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
ms.openlocfilehash: 601819756b78ffe8762bdfbfd5f802bc2d76e9c5
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35268050"
---
**Configuratieproces/server-vereisten**

**Onderdeel** | **Vereiste** 
--- | ---
**HARDWARE-INSTELLINGEN** | 
CPU-kernen | 8 
RAM | 16 GB
Aantal schijven | 3, met inbegrip van de OS-schijf, proces server cache schijf en bewaarstation voor failback 
Vrije schijfruimte (cache proces server) | 600 GB
Vrije schijfruimte (bewaarschijf) | 600 GB
 | 
**INSTELLINGEN VOOR SOFTWARE** | 
Besturingssysteem | Windows Server 2012 R2 <br> Windows Server 2016
Landinstelling van het besturingssysteem | Engels (en-us)
Windows Server-functies | Schakel niet in deze rollen: <br> - Active Directory Domain Services <br>- Internet Information Services <br> - Hyper-V 
Groepsbeleid | Geen groepsbeleid voor deze inschakelen: <br> -Toegang tot de opdrachtprompt voorkomen. <br> -Toegang tot het register bewerkingsopties voorkomen. <br> -Logica voor bestandsbijlagen vertrouwen. <br> -Uitvoering van Script inschakelen. <br> [Meer informatie](https://technet.microsoft.com/library/gg176671(v=ws.10).aspx)
IIS | -Er zijn geen bestaande standaardwebsite <br> -Er zijn geen bestaande website/toepassing geluisterd op poort 443 <br>-Inschakelen [anonieme verificatie](https://technet.microsoft.com/library/cc731244(v=ws.10).aspx) <br> -Inschakelen [FastCGI](https://technet.microsoft.com/library/cc753077(v=ws.10).aspx) instelling 
| 
**NETWERKINSTELLINGEN** | 
Type IP-adres | Statisch 
Toegang tot het internet | De server heeft toegang tot deze URL's (rechtstreeks of via proxy) <br> - \*.accesscontrol.windows.net<br> - \*.backup.windowsazure.com <br>- \*.store.core.windows.net<br> - \*.blob.core.windows.net<br> - \*.hypervrecoverymanager.windowsazure.com <br> - https://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi (als u een configuratieserver instelt) <br> - time.nist.gov <br> - time.windows.com 
Poorten | 443 (Orchestration-besturingselement)<br>9443 (Gegevenstransport) 
NIC-type | VMXNET3 (als de configuratieserver een VM VMware is)
 | 
**SOFTWARE INSTALLEREN** | 
VMware vSphere PowerCLI | [PowerCLI versie 6.0](https://my.vmware.com/web/vmware/details?productId=491&downloadGroup=PCLI600R1) moet worden geïnstalleerd als de configuratieserver wordt uitgevoerd op een VMware-VM.
MYSQL | MySQL moet worden geïnstalleerd. U kunt ook handmatig installeren of Site Recovery kunt u deze installeren.

**Configuratie/processerver sizing vereisten**

**CPU** | **Geheugen** | **Cache-schijf** | **Snelheid van de gegevens wijzigen** | **Gerepliceerde machines**
--- | --- | --- | --- | ---
8 vcpu 's<br/><br/> 2-sockets * 4 kernen @ 2,5 GHz | 16GB | 300 GB | 500 GB of minder | < 100 machines
12 vcpu 's<br/><br/> 2 socks * 6 kernen @ 2,5 GHz | 18 GB | 600 GB | 500 GB - 1 TB | 100-150 machines
16 vcpu 's<br/><br/> 2 socks * 8 kernen @ 2,5 GHz | 32 GB | 1 TB | 1-2 TB | 150 -200-machines

