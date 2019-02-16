---
title: Beschikbaarheid van SAP HANA op Azure Virtual machines - overzicht | Microsoft Docs
description: Beschrijft de bewerkingen die SAP HANA op Azure systeemeigen VM's.
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
ms.openlocfilehash: 1db56ad31991b85ffad415818c7c67f0ee30808d
ms.sourcegitcommit: d2329d88f5ecabbe3e6da8a820faba9b26cb8a02
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/16/2019
ms.locfileid: "56328075"
---
# <a name="sap-hana-high-availability-for-azure-virtual-machines"></a>SAP HANA met hoge beschikbaarheid voor Azure virtual machines

Talloze mogelijkheden van Azure kunt u databases die essentiële, zoals SAP HANA op Azure VM's implementeren. In dit artikel biedt richtlijnen voor hoe voor beschikbaarheid voor SAP HANA-instanties die worden gehost in Azure VM's. Het artikel beschrijft de verschillende scenario's die u kunt implementeren met behulp van de Azure-infrastructuur om de beschikbaarheid van SAP HANA in Azure te vergroten. 

## <a name="prerequisites"></a>Vereisten

In dit artikel wordt ervan uitgegaan dat u bekend met infrastructuur als een service (IaaS)-bewerkingen in Azure bent, met inbegrip van: 

- Klik hier voor meer informatie over het implementeren van virtuele machines of virtuele netwerken via de Azure portal of PowerShell.
- Met behulp van de Azure platformoverschrijdende opdrachtregelinterface (Azure CLI), met inbegrip van de optie voor het gebruik van sjablonen voor JavaScript Object Notation (JSON).

In dit artikel wordt ook van uitgegaan dat u bekend bent met het installeren van SAP HANA-instanties, en met het beheren en gebruiken van SAP HANA-instanties. Het is met name belangrijk om vertrouwd te raken met de installatie en bewerkingen van HANA-systeemreplicatie. Dit omvat taken zoals het back-up en herstel voor SAP HANA-databases.

Deze artikelen bevatten een goed overzicht van het gebruik van SAP HANA in Azure:

- [Handmatige installatie van één exemplaar SAP HANA op Azure Virtual machines](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-get-started)
- [SAP HANA-systeemreplicatie in Azure VM's instellen](sap-hana-high-availability.md)
- [Back-up van SAP HANA op Azure Virtual machines](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-guide)

Het is ook een goed idee om vertrouwd te raken met deze artikelen over SAP HANA:

- [Hoge beschikbaarheid voor SAP HANA](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.02/en-US/6d252db7cdd044d19ad85b46e6c294a4.html)
- [Veelgestelde vragen: Hoge beschikbaarheid voor SAP HANA](https://archive.sap.com/documents/docs/DOC-66702)
- [-Systeemreplicatie voor SAP HANA uitvoeren](https://archive.sap.com/documents/docs/DOC-47702)
- [SAP HANA 2.0 SP's 01 wat van nieuwe: hoge beschikbaarheid](https://blogs.sap.com/2017/05/15/sap-hana-2.0-sps-01-whats-new-high-availability-by-the-sap-hana-academy/)
- [Aanbevelingen voor netwerken voor SAP HANA-systeemreplicatie](https://www.sap.com/documents/2016/06/18079a1c-767c-0010-82c7-eda71af511fa.html)
- [SAP HANA-systeemreplicatie](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.01/en-US/b74e16a9e09541749a745f41246a065e.html)
- [SAP HANA-service automatisch opnieuw opstarten](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.01/en-US/cf10efba8bea4e81b1dc1907ecc652d3.html)
- [SAP HANA-systeemreplicatie configureren](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.01/en-US/676844172c2442f0bf6c8b080db05ae7.html)

Dan wordt bekend bent met het implementeren van virtuele machines in Azure, voordat u de architectuur van de beschikbaarheid in Azure definiëren, raden we aan dat u leest [de beschikbaarheid van Windows virtuele machines in Azure beheren](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability).

## <a name="service-level-agreements-for-azure-components"></a>Serviceovereenkomsten voor Azure-onderdelen

Azure heeft verschillende beschikbaarheids-Sla's voor verschillende onderdelen, zoals netwerken, opslag en virtuele machines. Alle Sla's worden beschreven. Zie voor meer informatie, [Microsoft Azure Service Level Agreements](https://azure.microsoft.com/support/legal/sla/). 

[SLA voor virtuele Machines](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_6/) twee verschillende Sla's, voor de twee verschillende configuraties worden beschreven:

- Een enkele virtuele machine die gebruikmaakt van [Azure premium SSD's](../../windows/disks-types.md) voor schijf met het besturingssysteem en alle gegevensschijven. Deze optie biedt een maandelijkse beschikbaarheid van 99,9 procent.
- Meerdere (ten minste twee) virtuele machines die zijn ingedeeld in een [Azure-beschikbaarheidsset](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets). Deze optie biedt een maandelijkse beschikbaarheid van 99,95 procent.

Meten van de vereiste beschikbaarheid op basis van de SLA's die Azure-onderdelen kunnen bieden. Kies vervolgens de scenario's voor SAP HANA uw vereiste niveau van beschikbaarheid te bereiken.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [SAP HANA-beschikbaarheid binnen één Azure-regio](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-one-region).
- Meer informatie over [beschikbaarheid van SAP HANA op Azure-regio's](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-across-regions). 















  


