---
title: Overzicht van SAP HANA op Azure (grote instanties) | Microsoft Docs
description: Overzicht van het implementeren van SAP HANA op Azure (grote instanties).
services: virtual-machines-linux
documentationcenter: ''
author: RicksterCDN
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/04/2018
ms.author: saghorpa
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c177dbad1145dee6eda3202d8076997cc7673dfc
ms.sourcegitcommit: d211f1d24c669b459a3910761b5cacb4b4f46ac9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/06/2018
ms.locfileid: "44026918"
---
#  <a name="what-is-sap-hana-on-azure-large-instances"></a>Wat is de SAP HANA op Azure (grote instanties)?

SAP HANA op Azure (grote instanties) is een unieke oplossing naar Azure. Naast het leveren van virtuele machines voor het implementeren en uitvoeren van SAP HANA, biedt Azure u de mogelijkheid voor het uitvoeren en implementeren van SAP HANA op bare-metal-servers die aan u zijn toegewezen. De SAP HANA op Azure (grote instanties)-oplossing is gebaseerd op niet-gedeelde/hostserver bare-metal hardware die aan u is toegewezen. De serverhardware is ingesloten in grotere stempels met compute/server-, netwerk- en opslaginfrastructuur. Als een combinatie is HANA die zijn afgestemd center gegevensintegratie (TDI) gecertificeerd. SAP HANA op Azure (grote instanties) biedt een andere server SKU's of grootten. Eenheden kunnen hebben 36 Intel CPU-kernen en biedt 768 GB van geheugen en gaat u naar maximaal eenheden die maximaal 480 Intel CPU-kernen hebben en maximaal 24 uur per dag TB aan geheugen.

De klantisolatie binnen de stempel infrastructuur wordt uitgevoerd in tenants, die lijkt op:

- **Netwerken**: isolatie van klanten in de infrastructuur-stack via virtuele netwerken per klanttenant toegewezen. Een tenant is toegewezen aan één klant. Een klant kan meerdere tenants hebben. De netwerkisolatie van tenants verbiedt netwerkcommunicatie tussen tenants op het niveau van de stempel infrastructuur, zelfs als de tenants deel uitmaken van dezelfde klant.
- **Opslagonderdelen**: isolatie door middel van opslag virtuele machines met opslagvolumes die zijn toegewezen. Opslagvolumes die kunnen worden toegewezen aan slechts één opslag virtuele machine. Een opslag virtuele machine wordt toegewezen aan één enkele tenant in de SAP HANA TDI-gecertificeerde infrastructuur-stack. Als gevolg hiervan kunnen opslagvolumes die zijn toegewezen aan een virtuele machine voor opslag worden geopend in een specifieke en gerelateerde tenant alleen. Ze zijn niet zichtbaar zijn tussen de verschillende geïmplementeerde tenants.
- **Server of de host**: een server of de host-eenheid niet worden gedeeld tussen klanten of tenants. Een server of de host is geïmplementeerd voor een klant, is een atomische bare metal-compute-eenheid die is toegewezen aan één enkele tenant. *Geen* hardware partitioneren of zachte partitioneren wordt gebruikt dat kan leiden tot u delen van een host of een server met een andere klant. Opslagvolumes die zijn toegewezen aan de opslag virtuele machine van de specifieke tenant zijn gekoppeld aan een dergelijke server. Een tenant kunt beschikken over een aantal eenheden van de server van verschillende SKU's exclusief toegewezen.
- Binnen een SAP HANA op Azure (grote instanties)-infrastructuur stempel, zijn veel verschillende tenants geïmplementeerd en ten opzichte van elkaar geïsoleerd door middel van de tenant-concepten in netwerken, opslag en Computing-niveau. 


Deze eenheden bare metal-server worden ondersteund voor het uitvoeren van SAP HANA alleen. De SAP-toepassingslaag of workload middleware-laag wordt uitgevoerd in virtuele machines. De stempels infrastructuur met SAP HANA op Azure (grote instanties) eenheden zijn verbonden met de Azure-netwerk services wervels. Op deze manier krijgt u connectiviteit met lage latentie tussen virtuele machines en SAP HANA op Azure (grote instanties)-eenheden.

Dit document is een van verschillende documenten die betrekking hebben op SAP HANA op Azure (grote instanties). Dit document worden de basisarchitectuur, verantwoordelijkheden en services die worden geleverd door de oplossing. Op hoog niveau van de mogelijkheden van de oplossing worden ook besproken. Vier andere documenten dekking voor de meeste andere gebieden, zoals netwerk- en -connectiviteit, details en inzoomen op gegevens. De documentatie van SAP HANA op Azure (grote instanties) dekt niet aspecten van de SAP NetWeaver-installatie of implementatie van SAP NetWeaver in virtuele machines. SAP NetWeaver op Azure wordt behandeld in afzonderlijke documenten gevonden in dezelfde Azure-documentatie-container. 


De verschillende documenten van HANA grote instantie richtlijnen omvatten de volgende gebieden:

- [Overzicht van de SAP HANA (grote instanties) en architectuur op Azure](hana-overview-architecture.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [SAP HANA (grote instanties)-infrastructuur en connectiviteit in Azure](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Installeren en configureren van SAP HANA (grote instanties) op Azure](hana-installation.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [SAP HANA (grote instanties) hoge beschikbaarheid en herstel na noodgeval op Azure](hana-overview-high-availability-disaster-recovery.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Het oplossen van SAP HANA (grote instanties) en de controle op Azure](troubleshooting-monitoring.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Hoge beschikbaarheid in SUSE is ingesteld met behulp van de stonith instellen](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/ha-setup-with-stonith)
- [OS-back-up en herstel voor SKU's Type II](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/os-backup-type-ii-skus)

**Volgende stappen**
- Raadpleeg [weet de voorwaarden](hana-know-terms.md)