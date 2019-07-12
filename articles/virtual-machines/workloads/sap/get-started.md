---
title: Aan de slag met SAP op Azure Virtual machines | Microsoft Docs
description: Meer informatie over SAP-oplossingen die worden uitgevoerd op virtuele machines (VM's) in Microsoft Azure
services: virtual-machines-linux
documentationcenter: ''
author: msjuergent
manager: bburns
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: ad8e5c75-0cf6-4564-ae62-ea1246b4e5f2
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 07/10/2019
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 27e12b5f33f2e3c0024d5caa6a9bee5e9576a45a
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67805831"
---
# <a name="use-azure-to-host-and-run-sap-workload-scenarios"></a>Gebruik Azure om te hosten en uitvoeren van SAP-workloadscenario 's

Wanneer u Microsoft Azure, kunt u op betrouwbare wijze uw essentiële SAP-workloads en scenario's uitvoeren op een schaalbaar, compatibel en zakelijk bewezen platform. U krijgt de schaalbaarheid, flexibiliteit en kostenbesparingen van Azure. Met de uitgebreide samenwerking tussen Microsoft en SAP, SAP-toepassingen uitvoeren in ontwikkeling en testen en productiescenario's in Azure en volledig wordt ondersteund. Van SAP NetWeaver tot SAP S/4HANA, SAP BI op Linux tot Windows en SAP HANA tot SQL, we hebben allemaal.

Naast het hosten van SAP NetWeaver-scenario's met de verschillende DBMS-systemen in Azure, kunt u andere SAP-workloadscenario's, zoals BI SAP op Azure te hosten. 

De uniekheid van Azure voor SAP HANA is een aanbieding die Azure uit elkaar instelt. Azure biedt om in te schakelen die als host fungeert meer geheugen en CPU-resource veeleisende SAP-scenario's die betrekking hebben op SAP HANA, het gebruik van de klant toegewezen bare-metal hardware. Deze oplossing gebruiken om uit te voeren van SAP HANA-implementaties die tot 24 TB (120 TB scale-out) geheugen voor S/4HANA of andere SAP HANA-werkbelastingen vereisen. 

SAP-workloadscenario's in Azure die als host fungeert kunt ook maken vereisten van identity integration en eenmalige aanmelding. Deze situatie kan optreden wanneer u Azure Active Directory (Azure AD) gebruiken voor verbinding maken met verschillende onderdelen van SAP en SAP-software-as-a-service (SaaS) of platform-as-a-service (PaaS) biedt. Een lijst van dergelijke integration en eenmalige aanmelding in scenario's met Azure AD en SAP entiteiten is beschreven en wordt beschreven in de sectie 'AAD SAP identity integration en eenmalige aanmelding.'

## <a name="latest-changes"></a>Meest recente wijzigingen

- Introductie van nieuwe guide for [IBM Db2 HADR in Red Hat Enterprise Server](high-availability-guide-rhel-ibm-db2-luw.md)
- Release van [hoge beschikbaarheid voor SAP NetWeaver op Red Hat Enterprise Linux met Azure NetApp-bestanden voor SAP-toepassingen](high-availability-guide-rhel-netapp-files.md)
- Introductie van ExpressRoute snel pad en een globaal bereik voor HANA grote instanties in [netwerkarchitectuur van SAP HANA (grote instanties)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-network-architecture) en gerelateerde documenten
- Release van [grote Azure HANA-instanties beheren via de Azure-portal](hana-li-portal.md)
- Release van [hoge beschikbaarheid voor SAP NetWeaver op Azure VM's in SUSE Linux Enterprise Server met Azure NetApp-bestanden voor SAP-toepassingen](high-availability-guide-suse-netapp-files.md)







## <a name="sap-hana-on-azure-large-instances"></a>SAP HANA op Azure (grote exemplaren)

Een reeks documenten leidt u door SAP HANA, op Azure (grote instanties), of kortweg, HANA grote instanties. Voor informatie over de volgende gebieden van HANA grote instanties, Zie:

- [Overzicht van SAP HANA op Azure (grote instanties)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture)
- [Architectuur van SAP HANA op Azure (grote instanties)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-architecture)
- [Infrastructuur en connectiviteit voor SAP HANA op Azure (grote instanties)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-infrastructure-connectivity)
- [Installeren van SAP HANA op Azure (grote instanties)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-installation)
- [Hoge beschikbaarheid en herstel na noodgevallen van SAP HANA op Azure (grote instanties)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-high-availability-disaster-recovery)
- [Probleemoplossing en bewaking van SAP HANA op Azure (grote instanties)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/troubleshooting-monitoring)

Volgende stappen:

- Lezen [overzicht en architectuur van SAP HANA op Azure (grote instanties)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture)



## <a name="sap-hana-on-azure-virtual-machines"></a>SAP HANA op Azure virtual machines
Deze sectie van de documentatie bevat informatie over de verschillende aspecten van SAP HANA. Als een vereiste moet u bekend bent met de principal-services van Azure die elementaire services van Azure IaaS te kunnen leveren. U moet dus kennis van Azure compute, opslag en netwerken. Veel van deze onderwerpen worden verwerkt in de SAP NetWeaver-gerelateerde [Azure handleiding voor capaciteitsplanning](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide). 

Zie voor informatie over HANA op Azure, de volgende artikelen en hun subarticles:

- [Snelstart: Handmatige installatie van één exemplaar SAP HANA op Azure Virtual machines](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-get-started)
- [SAP S/4HANA of BW/4HANA op Azure implementeren](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/cal-s4h)
- [Configuraties en bewerkingen van SAP HANA-infrastructuur in Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations)
- [SAP HANA met hoge beschikbaarheid voor Azure virtual machines](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-overview)
- [SAP HANA-beschikbaarheid binnen één Azure-regio](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-one-region)
- [Beschikbaarheid van SAP HANA op Azure-regio 's](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-across-regions)
- [Hoge beschikbaarheid van SAP HANA op Azure virtual machines](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-high-availability)
- [Back-uphandleiding voor SAP HANA op Azure virtual machines](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-guide)
- [SAP HANA Azure back-up op bestandsniveau](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-file-level)
- [SAP HANA-back-up op basis van opslagmomentopnamen](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-storage-snapshots)

 

## <a name="sap-netweaver-deployed-on-azure-virtual-machines"></a>SAP NetWeaver op Azure virtual machines geïmplementeerd
Deze sectie vindt u documentatie bij de planning en implementatie voor SAP NetWeaver en Business One on Azure. De documentatie is gericht op de basisbeginselen en het gebruik van niet-HANA-databases met een SAP-workloads op Azure. De documenten en artikelen voor hoge beschikbaarheid zijn ook de basis vormt voor HANA met hoge beschikbaarheid in Azure, zoals:

- [SAP Business One op Azure virtual machines](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/business-one-azure)
- [SAP IDES EHP7 SP3 implementeren voor SAP ERP 6.0 op Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/cal-ides-erp6-erp7-sp3-sql)
- [SAP NetWeaver uitvoeren op Microsoft Azure SUSE Linux VM 's](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/suse-quickstart)
- [Azure virtuele Machines, planning en implementatie van SAP NetWeaver](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide)
- [Azure Virtual Machines-implementatie voor SAP NetWeaver](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/deployment-guide)
- [De implementatie van een met meerdere lagen SAP NetWeaver-toepassingen beveiligen met behulp van Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-sap)
- [SAP LaMa-connector voor Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/lama-installation)

Zie voor informatie over niet-HANA-databases onder de werkbelasting van een SAP op Azure:

- [Overwegingen voor Azure Virtual Machines DBMS-implementatie voor de werkbelasting van SAP](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_general)
- [SQL Server Azure virtuele Machines DBMS-implementatie voor SAP NetWeaver](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_sqlserver)
- [DBMS-implementatie voor SAP-werkbelasting in virtuele Azure-machines voor Oracle](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_oracle)
- [DBMS-implementatie voor SAP-werkbelasting in virtuele Azure-machines voor IBM DB2](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_ibm)
- [DBMS-implementatie voor SAP-werkbelasting in virtuele Azure-machines voor SAP ASE](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_sapase)
- [SAP-MaxDB, Cache voor Live en implementatie van de Server voor webinhoud op Azure Virtual machines](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_maxdb)

Voor informatie over SAP HANA-databases op Azure, Zie de sectie "SAP HANA op Azure virtual machines."

Zie voor informatie over maximale beschikbaarheid van een SAP-workloads op Azure:

- [Azure virtuele Machines hoge beschikbaarheid voor SAP NetWeaver](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-start)

Dit document verwijst naar verschillende andere architectuur en scenario documenten. In latere scenario documenten vindt u koppelingen naar gedetailleerde technische documenten waarin wordt uitgelegd van de implementatie en configuratie van de verschillende methoden voor hoge beschikbaarheid. De verschillende documenten die laten hoe zien het tot stand brengen en configureren van hoge beschikbaarheid voor de werkbelasting van een SAP NetWeaver betrekking hebben op Linux en Windows-besturingssystemen.


Zie voor informatie over integratie tussen Azure Active Directory (Azure AD) en SAP-services en eenmalige aanmelding:

- [Zelfstudie: Azure Active Directory-integratie met SAP Cloud voor klanten](https://docs.microsoft.com/azure/active-directory/saas-apps/sap-customer-cloud-tutorial?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [Zelfstudie: Azure Active Directory-integratie met SAP Cloud Platform-identiteitsverificatie](https://docs.microsoft.com/azure/active-directory/saas-apps/sap-hana-cloud-platform-identity-authentication-tutorial?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [Zelfstudie: Azure Active Directory-integratie met SAP Cloud Platform](https://docs.microsoft.com/azure/active-directory/saas-apps/sap-hana-cloud-platform-tutorial?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [Zelfstudie: Azure Active Directory-integratie met SAP NetWeaver](https://docs.microsoft.com/azure/active-directory/saas-apps/sap-netweaver-tutorial?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [Zelfstudie: Azure Active Directory-integratie met SAP Business ByDesign](https://docs.microsoft.com/azure/active-directory/saas-apps/sapbusinessbydesign-tutorial?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [Zelfstudie: Azure Active Directory-integratie met SAP HANA](https://docs.microsoft.com/azure/active-directory/saas-apps/saphana-tutorial?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [Uw omgeving met S/4HANA: Fiori Launchpad SAML eenmalige aanmelding met Azure AD](https://blogs.sap.com/2017/02/20/your-s4hana-environment-part-7-fiori-launchpad-saml-single-sing-on-with-azure-ad/)

Zie voor informatie over de integratie van Azure-services in SAP-onderdelen:

- [SAP HANA gebruiken in Power BI Desktop](https://docs.microsoft.com/power-bi/desktop-sap-hana)
- [DirectQuery en SAP HANA](https://docs.microsoft.com/power-bi/desktop-directquery-sap-hana)
- [De SAP BW-connector gebruiken in Power BI Desktop](https://docs.microsoft.com/power-bi/desktop-sap-bw-connector) 
- [Azure Data Factory biedt integratie van SAP HANA- en Business Warehouse-gegevens](https://azure.microsoft.com/blog/azure-data-factory-offer-sap-hana-and-business-warehouse-data-integration)




