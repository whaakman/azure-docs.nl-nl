---
title: Linux-distributies goedgekeurde | Microsoft Docs
description: Meer informatie over Linux op Azure goedgekeurde distributies, met inbegrip van de richtlijnen voor Ubuntu, CentOS, Oracle en SUSE.
services: virtual-machines-linux
documentationcenter: 
author: szarkos
manager: timlt
editor: tysonn
tags: azure-service-management,azure-resource-manager
ms.assetid: 2777a526-c260-4cb9-a31a-bdfe1a55fffc
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/02/2017
ms.author: szark
ms.openlocfilehash: 39cb2464eb593a29c4436afb5c14419b704ebff4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="linux-on-distributions-endorsed-by-azure"></a>Linux op door Azure goedgekeurde distributies
Partners bieden Linux-installatiekopieën in Azure Marketplace. We werken met verschillende Linux-community's nog meer versies toevoegen aan de lijst met goedgekeurde distributie. In de tussentijd voor distributies die niet beschikbaar in de Marketplace, u kunt altijd brengt uw eigen Linux door de richtlijnen op [maken en uploaden van een virtuele harde schijf met het Linux-besturingssysteem](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

## <a name="supported-distributions-and-versions"></a>Ondersteunde distributies en versies
De volgende tabel bevat de Linux-distributies en versies die worden ondersteund in Azure. Raadpleeg [ondersteuning voor Linux-installatiekopieën in Microsoft Azure](https://support.microsoft.com/en-us/kb/2941892) voor meer informatie gedetailleerde.

De Linux Integration Services (LIS) stuurprogramma's voor Hyper-V en Azure zijn kernelmodules die Microsoft rechtstreeks aan de upstream Linux-kernel bijdraagt.  Sommige LIS stuurprogramma's zijn ingebouwd in de distributie kernel standaard. Oudere distributies die zijn gebaseerd op Red Hat Enterprise (RHEL) / CentOS zijn beschikbaar als een afzonderlijke download op [Linux Integration Services versie 4.1 voor Hyper-V](http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409). Zie [Linux kernel vereisten](create-upload-generic.md#linux-kernel-requirements) voor meer informatie over de LIS stuurprogramma's.

De Azure Linux Agent al vooraf is geïnstalleerd op de Azure Marketplace-installatiekopieën en van de distributie pakket opslagplaats doorgaans beschikbaar is. Broncode vindt u op [GitHub](https://github.com/azure/walinuxagent).

| Distributie | Versie | Stuurprogramma's | Agent |
| --- | --- | --- | --- |
| CentOS |CentOS 6.3 + 7.0 + |CentOS 6.3: [LIS downloaden](http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409)<p>CentOS 6.4 +: In de kernel |Pakket: In [opslagplaats](http://olcentgbl.trafficmanager.net/openlogic/6/openlogic/x86_64/RPMS/) onder 'WALinuxAgent' <br/>Broncode: [GitHub](https://github.com/Azure/WALinuxAgent) |
| [CoreOS](https://coreos.com/docs/running-coreos/cloud-providers/azure/) |494.4.0+ |In de kernel |Broncode: [GitHub](https://github.com/coreos/coreos-overlay/tree/master/app-emulation/wa-linux-agent) |
| Debian |Debian 7,9 +, 8.2 + |In de kernel |Pakket: In de opslagplaats onder 'waagent' <br/>Broncode: [GitHub](https://github.com/Azure/WALinuxAgent) |
| Oracle Linux |6.4+, 7.0+ |In de kernel |Pakket: In de opslagplaats onder 'WALinuxAgent' <br/>Broncode: [GitHub](http://go.microsoft.com/fwlink/p/?LinkID=250998) |
| Red Hat Enterprise Linux |RHEL 6.7 +, 7.1 + |In de kernel |Pakket: In de opslagplaats onder 'WALinuxAgent' <br/>Broncode: [GitHub](https://github.com/Azure/WALinuxAgent) |
| SUSE Linux Enterprise |SLES/SLES voor SAP<br>11 SP4<br>12 SP1 +|In de kernel |Pakket:<p> voor 11 in [Cloud: extra](https://build.opensuse.org/project/show/Cloud:Tools) opslagplaats<br>voor 12 opgenomen in de Module 'Openbare Cloud' onder 'python-azure-agent'<br/>Broncode: [GitHub](http://go.microsoft.com/fwlink/p/?LinkID=250998) |
| openSUSE |openSUSE Leap 42,1 + |In de kernel |Pakket: In [Cloud: extra](https://build.opensuse.org/project/show/Cloud:Tools) opslagplaats onder 'python-azure-agent' <br/>Broncode: [GitHub](https://github.com/Azure/WALinuxAgent) |
| Ubuntu |Ubuntu 12.04, 14.04, 16.04, 16,10 |In de kernel |Pakket: In de opslagplaats onder 'walinuxagent' <br/>Broncode: [GitHub](https://github.com/Azure/WALinuxAgent) |

## <a name="partners"></a>Partners

### <a name="coreos"></a>CoreOS
[https://coreos.com/Docs/Running-coreos/cloud-providers/Azure/](https://coreos.com/docs/running-coreos/cloud-providers/azure/)

Van de virtuele CoreOS-website:

*Virtuele CoreOS is ontworpen voor beveiliging, consistentie en betrouwbaarheid. In plaats van de installatie van pakketten via yum of apt, virtuele CoreOS Linux containers gebruikt voor het beheren van uw services op een hoger niveau van abstractie. Een enkele service code en alle afhankelijkheden zijn verpakt in een container die kan worden uitgevoerd op een of meer virtuele CoreOS-machines.*

### <a name="credativ"></a>Credativ
[http://www.credativ.co.uk/credativ-blog/debian-images-Microsoft-Azure](http://www.credativ.co.uk/credativ-blog/debian-images-microsoft-azure)

Credativ is een onafhankelijke advies en dienstverlener die gespecialiseerd in de ontwikkeling en implementatie van professional-oplossingen is met behulp van gratis software. Als de toonaangevende open source-specialisten heeft Credativ internationale herkenning met veel IT-afdelingen die gebruikmaken van hun ondersteuning. In combinatie met Microsoft wordt Credativ momenteel voorbereid bijbehorende Debian installatiekopieën voor Debian 8 (Jessie) en Debian vóór 7 (Wheezy). Beide afbeeldingen zijn speciaal ontworpen voor het uitvoeren op Azure en kunnen eenvoudig worden beheerd via het platform. Credativ wordt ook ondersteuning voor de lange termijn onderhoud en bijwerken van de Debian installatiekopieën voor Azure via de Open Source Support Centers.

### <a name="oracle"></a>Oracle
[http://www.Oracle.com/technetwork/Topics/cloud/FAQ-1963009.HTML](http://www.oracle.com/technetwork/topics/cloud/faq-1963009.html)

Van Oracle-strategie is een breed scala aan oplossingen voor openbare en persoonlijke clouds te bieden. De strategie biedt klanten keuze en flexibiliteit in hoe ze Oracle-software in Oracle-clouds en andere clouds implementeren. De Oracle-samen met Microsoft kan klanten implementeren Oracle-software in Microsoft openbare en persoonlijke clouds met het vertrouwen van de certificeringsinstantie en ondersteuning van Oracle.  Het streven en de investeringen in Oracle openbare en persoonlijke cloud-oplossingen van Oracle blijft ongewijzigd.

### <a name="red-hat"></a>Red Hat
[http://www.RedHat.com/en/partners/Strategic-Alliance/Microsoft](http://www.redhat.com/en/partners/strategic-alliance/microsoft)

De wereld toonaangevende leverancier van oplossingen voor open-source, Red Hat helpt meer dan 90% van Fortune 500 bedrijven zakelijke problemen oplossen, hun IT align en business-strategieën en voorbereiden van de toekomst van technologie. Red Hat doet dit door het bieden van beveiligde oplossingen via een open bedrijfsmodel en een betaalbare, voorspelbare abonnementsmodel.

### <a name="suse"></a>SUSE
[http://www.SUSE.com/SUSE-Linux-Enterprise-Server-on-Azure](http://www.suse.com/suse-linux-enterprise-server-on-azure)

SUSE Linux Enterprise Server in Azure is een beproefde platform waarmee hogere betrouwbaarheid en -beveiliging voor cloudcomputing. De SUSE veelzijdige Linux-platform biedt naadloos geïntegreerd met Azure-cloud-services voor het leveren van een eenvoudig beheerbare cloudomgeving. Met meer dan 9,200 gecertificeerde toepassingen van meer dan 1800 onafhankelijke softwareleveranciers voor SUSE Linux Enterprise Server en SUSE zorgt ervoor dat werkbelastingen ondersteunde in het datacenter probleemloos kunnen worden geïmplementeerd in Azure.

### <a name="canonical"></a>Canonical
[http://www.Ubuntu.com/cloud/Azure](http://www.ubuntu.com/cloud/azure)

Canonieke engineering en open community governance station Ubuntu van succes in client-, server- en cloud computing, waaronder persoonlijke cloudservices voor consumenten. Canonieke de visie van een uniforme, gratis platform in Ubuntu, via de telefoon in de cloud, biedt een serie van samenhangende interfaces voor de telefoon, tablet, tv- en bureaublad. Deze visie kunt u de eerste keuze voor diverse instellingen van providers van openbare Clouds de besluitvormers van consumer electronics en een favoriet tussen afzonderlijke technologen Ubuntu.

Met ontwikkelaars en engineering centers over de hele wereld, wordt een unieke Canonical geplaatst als u wilt samenwerken met hardware besluitvormers, inhoudsproviders en softwareontwikkelaars Ubuntu oplossingen op de markt voor pc's, servers en mobiele apparaten te brengen.
