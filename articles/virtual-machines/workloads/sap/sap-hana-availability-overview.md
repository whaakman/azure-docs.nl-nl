---
title: SAP HANA-beschikbaarheid op Azure Virtual machines - overzicht | Microsoft Docs
description: SAP HANA-bewerkingen op virtuele machines in Azure systeemeigen beschrijft.
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: msjuergent
manager: patfilot
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/05/2018
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7049a4b5159687ab928cda7ddc6b1a35959529ac
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="sap-hana-high-availability-for-azure-virtual-machines"></a>SAP HANA hoge beschikbaarheid voor Azure virtual machines

Talloze mogelijkheden van Azure kunt u essentiële databases zoals SAP HANA op Azure Virtual machines te implementeren. In dit artikel bevat richtlijnen over het bereiken van beschikbaarheid voor SAP HANA-exemplaren die worden gehost in Azure Virtual machines. Het artikel beschrijft de verschillende scenario's waarin u kunt implementeren met behulp van de Azure-infrastructuur om de beschikbaarheid van SAP HANA in Azure te verhogen. 

## <a name="prerequisites"></a>Vereisten

In dit artikel wordt ervan uitgegaan dat u bekend met infrastructuur als een dienst (IaaS) basisbeginselen in Azure bent, met inbegrip van: 

- Klik hier voor meer informatie over het implementeren van virtuele machines of virtuele netwerken via de Azure-portal of PowerShell.
- Met behulp van de Azure platformoverschrijdende opdrachtregelinterface (Azure CLI), inclusief de mogelijkheid om JSON JavaScript Object Notation ()-sjablonen te gebruiken.

In dit artikel wordt ervan uitgegaan dat u bekend bent met het installeren van SAP HANA-instanties, en met het beheren van en het gebruik van SAP HANA-exemplaren. Het is vooral belangrijk dat u bekend bent met de installatie en de bewerkingen van HANA system replicatie. Dit omvat taken, zoals back-up en herstel voor SAP HANA-databases.

Deze artikelen bevatten een overzicht van het gebruik van SAP HANA in Azure:

- [Handmatige installatie van de single instance SAP HANA op Azure Virtual machines](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-get-started)
- [De replicatie voor SAP HANA-systeem in Azure VM's instellen](sap-hana-high-availability.md)
- [Back-up SAP HANA op Azure Virtual machines](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-guide)

Het is ook een goed idee om te worden vertrouwd bent met deze artikelen over SAP HANA:

- [Hoge beschikbaarheid voor SAP HANA](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.02/en-US/6d252db7cdd044d19ad85b46e6c294a4.html)
- [Veelgestelde vragen over: Hoge beschikbaarheid voor SAP HANA](https://archive.sap.com/documents/docs/DOC-66702)
- [Systeem replicatie uit te voeren voor SAP HANA](https://archive.sap.com/documents/docs/DOC-47702)
- [SAP HANA 2.0 SP's 01 wat van nieuwe: hoge beschikbaarheid](https://blogs.sap.com/2017/05/15/sap-hana-2.0-sps-01-whats-new-high-availability-by-the-sap-hana-academy/)
- [Aanbevelingen voor SAP HANA system replicatie](https://www.sap.com/documents/2016/06/18079a1c-767c-0010-82c7-eda71af511fa.html)
- [Replicatie voor SAP HANA-systeem](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.01/en-US/b74e16a9e09541749a745f41246a065e.html)
- [SAP HANA-service automatisch opnieuw opstarten](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.01/en-US/cf10efba8bea4e81b1dc1907ecc652d3.html)
- [SAP HANA system replicatie configureren](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.01/en-US/676844172c2442f0bf6c8b080db05ae7.html)

Dan wordt bekend bent met het implementeren van virtuele machines in Azure, voordat u uw architectuur beschikbaarheid in Azure definiëren, raden we aan dat u leest [de beschikbaarheid van Windows virtuele machines in Azure beheren](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability).

## <a name="service-level-agreements-for-azure-components"></a>Serviceovereenkomsten voor Azure-onderdelen

Azure heeft verschillende beschikbaarheids-Sla's voor andere onderdelen, zoals netwerk-, opslag en virtuele machines. Alle serviceovereenkomsten worden beschreven. Zie voor meer informatie [Microsoft Azure Service Level Agreements](https://azure.microsoft.com/support/legal/sla/). 

[SLA voor virtuele Machines](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_6/) beschrijft twee verschillende Sla's, voor twee verschillende configuraties:

- Een enkele virtuele machine die gebruikmaakt van [Azure Premium-opslag](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage) voor de besturingssysteemschijf en alle gegevensschijven. Deze optie biedt een maandelijkse uptime van 99,9 procent.
- Meerdere (ten minste twee) virtuele machines die zijn geordend in een [Azure beschikbaarheidsset](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets). Deze optie biedt een maandelijkse bedrijfstijd van 99,95%.

Bepaal uw beschikbaarheidsvereiste wordt opgegeven op basis van de serviceovereenkomsten die Azure-onderdelen kunnen bieden. Kies uw scenario's voor SAP HANA als u de vereiste mate van beschikbaarheid.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [SAP HANA-beschikbaarheid binnen één Azure-regio](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-one-region).
- Meer informatie over [SAP HANA-beschikbaarheid volledige Azure-regio's](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-across-regions). 















  


