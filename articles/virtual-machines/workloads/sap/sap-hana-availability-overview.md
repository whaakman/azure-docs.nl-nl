---
title: SAP HANA beschikbaarheid op Azure Virtual machines - overzicht | Microsoft Docs
description: Bewerkingen van SAP HANA op virtuele machines in Azure systeemeigen
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: 
author: msjuergent
manager: patfilot
editor: 
tags: azure-resource-manager
keywords: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/05/2018
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 9b22f89750234a4715d2b7fd2df6ad8740b1d085
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/08/2018
---
# <a name="sap-hana-high-availability-guide-for-azure-virtual-machines"></a>Hoge beschikbaarheid voor SAP HANA-handleiding voor Azure virtual machines
Azure biedt talloze mogelijkheden waarmee missie kritieke databases zoals SAP HANA in Azure VM's implementeren. Dit document biedt richtlijnen voor het bereiken van beschikbaarheid voor SAP HANA-exemplaren die worden gehost in Azure Virtual Machines. In het document worden beschreven van verschillende scenario's die kunnen worden geïmplementeerd op Azure-infrastructuur om de beschikbaarheid van SAP HANA in Azure te verhogen. 

## <a name="prerequisites"></a>Vereisten
Deze handleiding wordt ervan uitgegaan dat u bekend met dergelijke infrastructuur als een dienst (IaaS) basisbeginselen op Azure als bent: 

- Klik hier voor meer informatie over het implementeren van virtuele machines of virtuele netwerken via de Azure-portal of PowerShell.
- De Azure platformoverschrijdende opdrachtregelinterface (CLI), inclusief de mogelijkheid om JSON JavaScript Object Notation ()-sjablonen te gebruiken.

Deze handleiding wordt ervan uitgegaan dat u bekend bent met SAP HANA-exemplaren installeren en beheren en operationele SAP HANA-exemplaren. Met name instellen en bewerkingen rond HANA System Replication of taken, zoals back-up en herstel van SAP HANA-databases.

Er zijn andere artikelen waarmee een goed overzicht op SAP HANA-onderwerpen in Azure:

- [Handmatige installatie van de single instance SAP HANA op Azure Virtual machines](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-get-started)
- [SAP HANA System replicatie in virtuele machines in Azure instellen](sap-hana-high-availability.md)
- [Back-handleiding voor SAP HANA op Azure Virtual Machines](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-guide)

Artikel en inhoud die u moet over SAP HANA vertrouwd kunnen worden weergegeven zoals:

- [Hoge beschikbaarheid voor SAP HANA](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.02/en-US/6d252db7cdd044d19ad85b46e6c294a4.html)
- [Veelgestelde vragen over: Hoge beschikbaarheid voor SAP HANA](https://archive.sap.com/documents/docs/DOC-66702)
- [Het systeem replicatie uit te voeren voor SAP HANA](https://archive.sap.com/documents/docs/DOC-47702)
- [SAP HANA 2.0 SP's 01 wat is er nieuw: hoge beschikbaarheid](https://blogs.sap.com/2017/05/15/sap-hana-2.0-sps-01-whats-new-high-availability-by-the-sap-hana-academy/)
- [Aanbevelingen voor SAP HANA System replicatie](https://www.sap.com/documents/2016/06/18079a1c-767c-0010-82c7-eda71af511fa.html)
- [SAP HANA System replicatie](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.01/en-US/b74e16a9e09541749a745f41246a065e.html)
- [SAP HANA Service Auto-Restart](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.01/en-US/cf10efba8bea4e81b1dc1907ecc652d3.html)
- [SAP HANA System replicatie configureren](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.01/en-US/676844172c2442f0bf6c8b080db05ae7.html)


Afgezien van wordt bekend bent met het implementeren van virtuele machines in Azure, wordt ook aangeraden lezen van het artikel [de beschikbaarheid van Windows virtuele machines in Azure beheren](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability) eerste voordat u doorgaat met het definiëren van uw architectuur beschikbaarheid in Azure.

## <a name="service-level-agreements-for-different-azure-components"></a>Service Level agreements voor verschillende Azure-onderdelen
Azure heeft verschillende beschikbaarheids-Sla's voor andere onderdelen, zoals netwerk-, opslag en virtuele machines. Alle deze SLA's worden beschreven en vindt u beginnen met de [Microsoft Azure Service Level Agreement](https://azure.microsoft.com/support/legal/sla/) pagina. Als u het selectievakje uit de [SLA voor virtuele Machines](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_6/), zult u ontdekken dat Azure twee verschillende Sla's met twee verschillende configuraties biedt. De serviceovereenkomsten worden gedefinieerd zoals:

- Een enkele virtuele machine met [Azure Premium-opslag](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage) biedt een maandelijks percentage van de beschikbaarheid van 99,9% voor het besturingssysteem schijf en alle gegevensschijven
- Meerdere (ten minste twee) virtuele machines die zijn geordend in een [Azure Beschikbaarheidsset](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets) bieden een maandelijks percentage van de beschikbaarheid van 99,95%

Meting uw vereisten van beschikbaarheid op basis van de onderdelen van de Azure Sla's bieden en vervolgens besluit op de verschillende scenario's die u wilt implementeren met SAP HANA de beschikbaarheid van de bereiken die u nodig te bieden.

## <a name="next-steps"></a>Volgende stappen
Blijven de documenten:

- [SAP HANA beschikbaarheid binnen één Azure-regio](https://docs.microsoft.com/en-us/azure/virtual-machines/workloads/sap/sap-hana-availability-one-region)
- [Beschikbaarheid van de SAP HANA volledige Azure-regio 's](https://docs.microsoft.com/en-us/azure/virtual-machines/workloads/sap/sap-hana-availability-across-regions) 















  


