---
title: bestand opnemen
description: bestand opnemen
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: include
ms.date: 09/03/2018
ms.author: raynew
ms.custom: include file
ms.openlocfilehash: afeae4af9b41bf434b26833a3bd927118a4697ae
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58887012"
---
**Configuratieproces/server-vereisten voor fysieke servers repliceren**

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
IIS | -Er zijn geen bestaande standaardwebsite <br> -Er zijn geen bestaande website/toepassing luistert op poort 443 <br>-Inschakelen [anonieme verificatie](https://technet.microsoft.com/library/cc731244(v=ws.10).aspx) <br> -Inschakelen [FastCGI](https://technet.microsoft.com/library/cc753077(v=ws.10).aspx) instelling.
Type IP-adres | Statisch 
| 
**TOEGANGSINSTELLINGEN** | 
MYSQL | MySQL moet worden geïnstalleerd op de configuratieserver. U kunt ook handmatig installeren of Site Recovery kan worden geïnstalleerd tijdens de implementatie. Voor Site Recovery te installeren, controleert u dat de machine kan bereiken http://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi.
URL 's | De configuratieserver moet toegang hebben tot deze URL's (rechtstreeks of via proxy):<br/><br/> Azure AD: `login.microsoftonline.com`; `login.microsoftonline.us`; `*.accesscontrol.windows.net`<br/><br/> Overdracht van replicatiegegevens: `*.backup.windowsazure.com`; `*.backup.windowsazure.us`<br/><br/> Replicatie: `*.hypervrecoverymanager.windowsazure.com`; `*.hypervrecoverymanager.windowsazure.us`; `https://management.azure.com`; `*.services.visualstudio.com`<br/><br/> Toegang tot opslag: `*.blob.core.windows.net`; `*.blob.core.usgovcloudapi.net`<br/><br/> Tijdsynchronisatie: `time.nist.gov`; `time.windows.com`<br/><br/> Telemetrie (optioneel): `dc.services.visualstudio.com`
Firewall | IP-adressen gebaseerde firewallregels moeten communicatie met Azure-URL's toestaan. Om te vereenvoudigen en het IP-adresbereiken beperken, wordt u aangeraden URL-filtering.<br/><br/>**Voor commerciële IP-adressen:**<br/><br/>-Sta de [Azure Datacenter IP-adresbereiken](https://www.microsoft.com/download/confirmation.aspx?id=41653), en de poort HTTPS (443).<br/><br/> -IP-adresbereiken voor de VS-West (gebruikt voor toegangsbeheer en identiteitsbeheer) toestaan.<br/><br/> -Sta de IP-adresbereiken voor de Azure-regio van uw abonnement, ter ondersteuning van de URL's die nodig zijn voor Azure Active Directory, back-up, replicatie en opslag.<br/><br/> **Voor de overheid IP-adressen:**<br/><br/> -Kunnen de regering IP-adresbereiken voor Azure Datacenter en de poort HTTPS (443).<br/><br/> -Sta de IP-adresbereiken voor alle VS (overheid)-regio's (Virginia, Texas, Arizona en Iowa), ter ondersteuning van de URL's die nodig zijn voor Azure Active Directory, back-up, replicatie en opslag.
Poorten | 443 (orchestration-besturingselement) toestaan<br/><br/> 9443 (gegevenstransport) toestaan 


**Configuratie-/ processerver formaat vereisten**

**CPU** | **Geheugen** | **Cacheschijf** | **Veranderingssnelheid van gegevens** | **Gerepliceerde machines**
--- | --- | --- | --- | ---
8 vcpu 's<br/><br/> 2-sockets * 4 kernen \@ 2,5 GHz | 16GB | 300 GB | 500 GB of minder | < 100 machines
12 vcpu 's<br/><br/> 2 socks * 6 kernen \@ 2,5 GHz | 18 GB | 600 GB | 500 GB-1 TB | 100-150-machines
16 vcpu 's<br/><br/> socks 2 * 8 kernen \@ 2,5 GHz | 32 GB | 1 TB | 1-2 TB | 150 -200-machines

