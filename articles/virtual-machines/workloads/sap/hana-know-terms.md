---
title: Weet u de voorwaarden van SAP HANA op Azure (grote instanties) | Microsoft Docs
description: De voorwaarden van SAP HANA op Azure (grote instanties) op de hoogte.
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
ms.openlocfilehash: 6d8197e3695da8869806e44775f601d95b17fcb2
ms.sourcegitcommit: d211f1d24c669b459a3910761b5cacb4b4f46ac9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/06/2018
ms.locfileid: "44028177"
---
# <a name="know-the-terms"></a>Weet u de voorwaarden

Verschillende gemeenschappelijke definities worden veel gebruikt in de architectuur en technische Implementatiehandleiding. Houd rekening met de volgende voorwaarden en hun betekenis:

- **IaaS**: infrastructuur als een service.
- **PaaS**: Platform als een service.
- **SaaS**: Software als een service.
- **SAP-onderdeel**: een afzonderlijke SAP-toepassing, zoals ERP centraal onderdeel (ECC), Business Warehouse (BW), Manager van de oplossing of Enterprise-Portal (EP). SAP-onderdelen kunnen worden gebaseerd op de traditionele ABAP- of Java-technologieën of een toepassing NetWeaver op basis van zoals zakelijke objecten.
- **SAP-omgeving**: een of meer onderdelen van SAP logisch zijn gegroepeerd om uit te voeren van een zakelijke-functie, zoals ontwikkeling, kwaliteit te waarborgen, training, herstel na noodgevallen of productie.
- **SAP-landschap**: verwijst naar de gehele SAP-elementen in uw IT-landschap. De SAP-landschap bevat alle productie- en niet-productieomgevingen.
- **SAP-systeem**: de combinatie van DBMS-laag en niveau van de toepassing van bijvoorbeeld een systeem voor de ontwikkeling SAP ERP, een testsysteem SAP BW en een SAP CRM productiesysteem. Azure-implementaties bieden geen ondersteuning voor het delen van deze twee lagen tussen on-premises en Azure. Een SAP-systeem is on-premises geïmplementeerd of de geïmplementeerd in Azure. U kunt de verschillende systemen van SAP-landschap dat in Azure of on-premises implementeren. U kunt bijvoorbeeld implementeren van de SAP CRM-ontwikkeling en testen van systemen in Azure, hoewel u de SAP CRM productie system on-premises implementeren. Voor SAP HANA op Azure (grote instanties), dit bedoeld dat u de SAP-toepassingslaag van SAP-systemen in virtuele machines en de bijbehorende SAP HANA-exemplaar op een eenheid in de SAP HANA op Azure (grote instanties) stempel hosten.
- **Grote instantiestempel**: een hardware-infrastructuur-stack die SAP HANA TDI-gecertificeerde en toegewezen aan het uitvoeren van SAP HANA-instanties in Azure.
- **SAP HANA op Azure (grote instanties):** officiële naam voor de aanbieding in Azure om uit te voeren van HANA-exemplaren in op SAP HANA TDI-gecertificeerde hardware die geïmplementeerd in grote instantie stempels in verschillende Azure-regio's. De gerelateerde term *HANA grote instantie* is te kort voor *SAP HANA op Azure (grote instanties)* en wordt veel gebruikt in deze handleiding technische implementatie.
- **Cross-premises**: Beschrijving van een scenario waarbij virtuele machines worden geïmplementeerd naar een Azure-abonnement dat site-naar-site, meerdere sites of Azure ExpressRoute-connectiviteit tussen on-premises datacenters en Azure. In algemene Azure-documentatie en dit soort implementaties worden ook beschreven als cross-premises scenario's. De reden voor de verbinding is om de on-premises domeinen, on-premises Azure Active Directory/OpenLDAP en on-premises DNS uitbreiden naar Azure. Het on-premises-landschap is uitgebreid naar de Azure-assets van de Azure-abonnementen. Met deze uitbreiding, kunnen de virtuele machines deel uitmaken van de on-premises domein. 

   Gebruikers van een domein van de on-premises domein kunnen toegang tot de servers en services op deze VM's (zoals DBMS-services) uitvoeren. On-premises communicatie en naamomzetting tussen VM's geïmplementeerd en virtuele machines die met Azure is mogelijk. In dit scenario is normaal dat van de manier waarop de meeste SAP-activa zijn geïmplementeerd. Zie voor meer informatie, [plannen en ontwerpen voor Azure VPN-Gateway](../../../vpn-gateway/vpn-gateway-plan-design.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) en [een virtueel netwerk met een site-naar-site-verbinding maken met behulp van de Azure-portal](../../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
- **Tenant**: een klant die is geïmplementeerd in HANA grote instantie stempel wordt geïsoleerd in een *tenant.* Een tenant wordt geïsoleerd in de netwerken, opslag en compute-laag van andere tenants. Opslag- en rekencapaciteit eenheden toegewezen aan de verschillende tenants kunnen Zie elkaar of met elkaar communiceren op het niveau van de stempel HANA grote instantie. Een klant kan ervoor kiezen implementaties in verschillende tenants. Zelfs dan is er geen communicatie tussen tenants op het niveau van de stempel HANA grote instantie.
- **SKU-categorie**: voor HANA grote instantie, de volgende twee categorieën van SKU's worden aangeboden:
    - **Type ik klasse**: S72, S72m, S144, S144m, S192, S192m en S192xm
    - **Typ II klasse**: S384, S384m, S384xm, S384xxm, S576m, S576xm, S768m, S768xm en S960m


Een aantal aanvullende resources zijn beschikbaar voor het implementeren van een SAP-workload in de cloud. Als u van plan bent om uit te voeren van een implementatie van SAP HANA in Azure, moet u ervaring hebt met en op de hoogte van de beginselen van Azure IaaS en de implementatie van SAP-workloads op Azure IaaS. Voordat u doorgaat, Zie [gebruik SAP-oplossingen op Azure virtual machines](get-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) voor meer informatie. 

**Volgende stappen**
- Raadpleeg [HLI certificering](hana-certification.md)