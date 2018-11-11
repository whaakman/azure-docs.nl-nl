---
title: Distributies van Linux | Microsoft Docs
description: Meer informatie over Linux op door Azure onderschreven distributies, met inbegrip van richtlijnen voor Ubuntu, CentOS, Oracle en SUSE.
services: virtual-machines-linux
documentationcenter: ''
author: szarkos
manager: jeconnoc
editor: tysonn
tags: azure-service-management,azure-resource-manager
ms.assetid: 2777a526-c260-4cb9-a31a-bdfe1a55fffc
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 11/21/2017
ms.author: szark
ms.openlocfilehash: b39ae6b5546e9504b9f37504c486c941ab034399
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51249996"
---
# <a name="linux-on-distributions-endorsed-by-azure"></a>Linux op door Azure onderschreven distributies
-Partners bieden Linux-installatiekopieën in de Azure Marketplace. We werken met verschillende Linux-community's nog meer varianten toevoegen aan de lijst met distributiepunten die zijn goedgekeurd. In de tussentijd voor distributies die niet beschikbaar zijn vanuit de Marketplace, u kunt altijd brengt uw eigen Linux door de richtlijnen op [maken en uploaden van een virtuele harde schijf met het Linux-besturingssysteem](https://docs.microsoft.com/azure/virtual-machines/linux/create-upload-generic).

## <a name="supported-distributions-and-versions"></a>Ondersteunde distributies en versies
De volgende tabel bevat de Linux-distributies en versies die worden ondersteund op Azure. Raadpleeg [ondersteuning voor Linux-installatiekopieën in Microsoft Azure](https://support.microsoft.com/help/2941892/support-for-linux-and-open-source-technology-in-azure) voor meer informatie over ondersteuning voor Linux en open source-technologie in Azure gedetailleerde.

De Linux Integration Services (LIS) stuurprogramma's voor Hyper-V en Azure zijn kernelmodules die deel uitmaakt van Microsoft rechtstreeks naar de upstream-Linux-kernel.  Aantal LIS-stuurprogramma's zijn standaard ingebouwd in kernel van de distributie. Oudere distributies die zijn gebaseerd op Red Hat Enterprise (RHEL) / CentOS zijn beschikbaar als een afzonderlijke download op [Linux Integration Services versie 4.2 voor Hyper-V en Azure](https://www.microsoft.com/en-us/download/details.aspx?id=55106). Zie [vereisten voor Linux-kernel](create-upload-generic.md#linux-kernel-requirements) voor meer informatie over de LIS-stuurprogramma's.

De Azure Linux Agent al vooraf is geïnstalleerd op de Azure Marketplace-installatiekopieën en is meestal beschikbaar vanuit de distributie van pakket-opslagplaats. Broncode is te vinden in [GitHub](https://github.com/azure/walinuxagent).

  
| Distributie | Versie | Stuurprogramma's | Agent |
| --- | --- | --- | --- |
| CentOS |CentOS 6.3+, 7.0+ |CentOS 6.3: [LIS downloaden](https://www.microsoft.com/en-us/download/details.aspx?id=55106)<p>CentOS 6.4 +: In-kernel |Pakket: In [opslagplaats](http://olcentgbl.trafficmanager.net/openlogic/6/openlogic/x86_64/RPMS/) onder "WALinuxAgent" <br/>Broncode: [GitHub](https://github.com/Azure/WALinuxAgent) |
| [CoreOS](https://coreos.com/docs/running-coreos/cloud-providers/azure/) |494.4.0+ |In de kernel |Broncode: [GitHub](https://github.com/coreos/coreos-overlay/tree/master/app-emulation/wa-linux-agent) |
| Debian |Debian 7,9 +, 8.2 + |In de kernel |Pakket: In de opslagplaats onder 'waagent' <br/>Broncode: [GitHub](https://github.com/Azure/WALinuxAgent) |
| Oracle Linux |6.4+, 7.0+ |In de kernel |Pakket: In de opslagplaats onder "WALinuxAgent" <br/>Broncode: [GitHub](https://go.microsoft.com/fwlink/p/?LinkID=250998) |
| Red Hat Enterprise Linux |RHEL 6.7 +, 7.1 + |In de kernel |Pakket: In de opslagplaats onder "WALinuxAgent" <br/>Broncode: [GitHub](https://github.com/Azure/WALinuxAgent) |
| SUSE Linux Enterprise |SLES/SLES voor SAP<br>11 SP4<br>12 SP1+<br>15|In de kernel |Pakket:<p> voor 11 in [Cloud: extra](https://build.opensuse.org/project/show/Cloud:Tools) opslagplaats<br>voor 12 opgenomen in de Module 'Openbare Cloud' onder 'python-azure-agent'<br/>Broncode: [GitHub](https://go.microsoft.com/fwlink/p/?LinkID=250998) |
| openSUSE |openSUSE Leap 42.2 + |In de kernel |Pakket: In [Cloud: extra](https://build.opensuse.org/project/show/Cloud:Tools) opslagplaats onder 'python-azure-agent' <br/>Broncode: [GitHub](https://github.com/Azure/WALinuxAgent) |
| Ubuntu |Ubuntu 12.04 +  **<sup>1</sup>** |In de kernel |Pakket: In de opslagplaats onder "walinuxagent" <br/>Broncode: [GitHub](https://github.com/Azure/WALinuxAgent) |

  - **<sup>1</sup>**  voor Ubuntu 12.04 ondersteuning op Azure raadpleegt u de [EOL kennisgeving](https://azure.microsoft.com/blog/ubuntu-12-04-precise-pangolin-nearing-end-of-life/).


## <a name="partners"></a>Partners

### <a name="coreos"></a>CoreOS
[https://coreos.com/docs/running-coreos/cloud-providers/azure/](https://coreos.com/docs/running-coreos/cloud-providers/azure/)

Vanaf de website van CoreOS:

*CoreOS is ontworpen voor beveiliging, consistentie en betrouwbaarheid. In plaats van de installatie van pakketten via yum of apt, CoreOS Linux-containers gebruikt voor het beheren van uw services op een hoger niveau van abstractie. Een enkele service code en alle afhankelijkheden zijn verpakt in een container die kan worden uitgevoerd op een of meer CoreOS-machines.*

### <a name="credativ"></a>credativ
[http://www.credativ.co.uk/credativ-blog/debian-images-microsoft-azure](http://www.credativ.co.uk/credativ-blog/debian-images-microsoft-azure)

Credativ is een onafhankelijke consulting services bedrijf en die gespecialiseerd in de ontwikkeling en implementatie van professionele oplossingen is met behulp van gratis software. Als toonaangevende open-source-specialisten heeft Credativ internationale opname met veel IT-afdelingen die gebruikmaken van de ondersteuning. In combinatie met Microsoft wordt Credativ momenteel voorbereid bijbehorende Debian installatiekopieën voor Debian 8 (Jessie) en Debian vóór 7 (Wheezy). Beide afbeeldingen zijn speciaal ontworpen om uit te voeren op Azure en eenvoudig kunnen worden beheerd via het platform. Credativ wordt ook ondersteuning voor de lange termijn onderhoud en het bijwerken van de Debian installatiekopieën voor Azure via de Open Source Support Centers.

### <a name="oracle"></a>Oracle
[http://www.oracle.com/technetwork/topics/cloud/faq-1963009.html](http://www.oracle.com/technetwork/topics/cloud/faq-1963009.html)

Van Oracle-strategie bestaat uit het bieden van een breed portfolio van oplossingen voor openbare en privéclouds. De strategie geeft klanten de keuze en flexibiliteit in hoe ze in Oracle clouds Oracle-software en andere clouds implementeren. De partnerschap van Oracle met Microsoft stelt klanten in staat Oracle-software in openbare en besloten clouds van Microsoft te implementeren in de wetenschap dat ze certificeringen en ondersteuning kunnen krijgen van Oracle.  De toezegging en investering in een Oracle-oplossingen voor openbare en privéclouds van Oracle is niet gewijzigd.

### <a name="red-hat"></a>Red Hat
[http://www.redhat.com/en/partners/strategic-alliance/microsoft](http://www.redhat.com/en/partners/strategic-alliance/microsoft)

Toonaangevende provider van de wereld van open source-oplossingen, Red Hat kan meer dan 90% van Fortune 500-bedrijven zakelijke uitdagingen, hun IT uitlijnen en strategieën voor bedrijven, en voor te bereiden voor de toekomst van technologie. Red Hat doet dit door op te geven beveiligde oplossingen via een open bedrijfsmodel en een betaalbare en voorspelbare abonnementsmodel.

### <a name="suse"></a>SUSE
[http://www.suse.com/suse-linux-enterprise-server-on-azure](http://www.suse.com/suse-linux-enterprise-server-on-azure)

SUSE Linux Enterprise Server op Azure is een bewezen platform dat biedt superieure betrouwbaarheid en beveiliging voor de cloud computing. Van SUSE veelzijdige Linux-platform wordt naadloos geïntegreerd met Azure cloudservices bieden een eenvoudige cloudomgeving. Met meer dan 9,200 gecertificeerde toepassingen van meer dan 1800 onafhankelijke softwareleveranciers voor SUSE Linux Enterprise Server, SUSE wordt ervoor gezorgd dat workloads die ondersteund worden uitgevoerd in het datacenter probleemloos kunnen worden geïmplementeerd op Azure.

### <a name="canonical"></a>Canonical
[http://www.ubuntu.com/cloud/azure](http://www.ubuntu.com/cloud/azure)

Canonieke engineering en open community governance station van Ubuntu succes in de client, server en cloud computing, waaronder persoonlijke cloudservices voor consumenten. Van canonical visie van een uniforme, gratis platform in Ubuntu, van telefoon naar de cloud, biedt een reeks samenhangend interfaces voor de telefoon, tablet, tv-programma's en desktop. Deze visie kunt u de eerste keuze voor de diverse instellingen van providers van openbare Clouds voor de makers van consumentenelektronica en een favoriet tussen afzonderlijke technologen Ubuntu.

Met ontwikkelaars en technische datacenters over de hele wereld, Canonical is in zijn soort als u wilt samenwerken met makers van hardware, inhoudsproviders en softwareontwikkelaars om oplossingen voor Ubuntu op de markt voor pc's, servers en mobiele apparaten.
