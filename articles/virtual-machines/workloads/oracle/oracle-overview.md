---
title: Oracle-oplossingen op Microsoft Azure | Microsoft Docs
description: Meer informatie over opties voor het implementeren van Oracle-toepassingen en oplossingen op Microsoft Azure, waaronder volledig op Azure-infrastructuur of bij cross-cloud-connectiviteit met Oracle Cloud Infrastructure (OCI).
services: virtual-machines-linux
documentationcenter: ''
author: romitgirdhar
manager: gwallace
tags: azure-resource-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 06/04/2019
ms.author: rogirdh
ms.openlocfilehash: b8bfa0dfa82f73cad010a608150eac48c7f3d4c8
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67707453"
---
# <a name="overview-of-oracle-applications-and-solutions-on-azure"></a>Overzicht van Oracle-toepassingen en oplossingen op Azure

Dit artikel bevat de mogelijkheden voor het uitvoeren van Oracle-oplossingen met behulp van Azure-infrastructuur. Zie ook gedetailleerde inleiding over beschikbare [Oracle VM-installatiekopieën](oracle-vm-solutions.md) in de Azure Marketplace en de preview-functie op [interconnect van Azure met Oracle Cloud Infrastructure (OCI)](oracle-oci-overview.md).

## <a name="oracle-databases-on-azure-infrastructure"></a>Oracle-databases op Azure-infrastructuur

Oracle-databases op Azure met behulp van Linux-installatiekopieën die beschikbaar in de Azure Marketplace-infrastructuur worden uitgevoerd:

* Oracle Database 12.1 12.2 en 18,3 Enterprise Edition 

* Oracle Database 12.1 12.2 en 18,3 Standard Edition 

U kunt ook een oplossing gebaseerd op een aangepaste installatiekopie u maken in Azure of een aangepaste installatiekopie uploaden vanaf uw on-premises omgeving.

(Optioneel) configureren met meerdere gekoppelde schijven en prestaties van de database verbeteren door het installeren van Oracle geautomatiseerde Storage Management (ASM).

## <a name="applications-on-oracle-linux-and-weblogic-server"></a>Toepassingen op en WebLogic Server, Oracle Linux

Zakelijke toepassingen uitvoeren in Azure op ondersteunde Oracle-besturingssystemen. De volgende installatiekopieën zijn beschikbaar in de Azure Marketplace:

* Oracle WebLogic Server 12.1.2

* Oracle Linux (UEK) 6,8, 6,9 6.10, 7.3, 7.4, 7.5 en 7,6

## <a name="high-availability-and-disaster-recovery-options"></a>Hoge beschikbaarheid en noodherstel herstelopties

* Oracle Data Guard, actief Data Guard en GoldenGate configureren op Azure-infrastructuur in combinatie met [Beschikbaarheidszones](../../../availability-zones/az-overview.md) voor hoge beschikbaarheid.

* Gebruik [Azure Site Recovery](../../../site-recovery/site-recovery-overview.md) te coördineren en beheren van herstel na noodgevallen voor uw virtuele Oracle Linux-machines in Azure en uw on-premises of fysieke servers. 

* Oracle echte toepassing Clusters (RAC) inschakelen in Azure met [FlashGrid SkyCluster](https://www.flashgrid.io/oracle-rac-in-azure/).

## <a name="integration-of-azure-with-oci-preview"></a>Integratie van Azure met OCI (preview)

Oracle-toepassingen uitvoeren in Azure-infrastructuur, die zijn verbonden met de back-enddatabases in Oracle Cloud Infrastructure (OCI). Deze oplossing maakt gebruik van de volgende mogelijkheden: 

* **Netwerken binnen de cloud** -gebruik de directe interconnect beschikbaar is tussen Azure ExpressRoute- en Oracle FastConnect tot stand brengen van verbindingen van hoge bandbreedte, particuliere en lage latentie tussen de toepassing en de databaselaag.
* **Geïntegreerde identiteit** -instellen van federatieve identiteiten tussen Azure AD en Oracle IDCS te maken van een bron één identiteit voor de oplossingen. Schakel eenmalige aanmelding voor het beheren van resources in OCI en Azure.

### <a name="deploy-oracle-applications-on-azure"></a>Oracle-toepassingen op Azure implementeren

Gebruik Terraform sjablonen van Azure-infrastructuur en Oracle-toepassingen gevalideerd en wordt ondersteund om uit te voeren in de configuratie van cross-cloud installeren:

* E-Business Suite
* JD Edwards EnterpriseOne
* PeopleSoft
* Oracle Retail-toepassingen
* Oracle Hyperion financieel beheer

Ook toepassingen die verbinding met OCI en andere Azure-services maken in Azure implementeren.

### <a name="set-up-oracle-databases-in-oci"></a>Oracle-databases in OCI instellen

Oracle Database Cloud Services (zelfstandige Database, RAC, Exadata, DBaaS, één knooppunt) gebruiken in combinatie met Oracle-toepassingen uitvoeren in Azure. Meer informatie over [OCI databaseopties](https://docs.cloud.oracle.com/iaas/Content/Database/Concepts/databaseoverview.htm). 
 

## <a name="licensing"></a>Licentieverlening

Implementatie van Oracle-toepassingen in Azure is gebaseerd op een 'bring your own license'-model. Ervan wordt uitgegaan dat u goed een licentie voor het gebruik van Oracle-software en dat u beschikt over een huidige ondersteuningscontract bij Oracle. Oracle is mobiliteit van licenties gegarandeerd van on-premises naar Azure. Zie de Oracle-Azure [Veelgestelde vragen over](https://www.oracle.com/cloud/technologies/oracle-azure-faq.html).

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over het implementeren van [Oracle VM-installatiekopieën](oracle-vm-solutions.md) in Azure-infrastructuur.

* Meer informatie over het [interconnect van Azure met OCI](oracle-oci-overview.md).