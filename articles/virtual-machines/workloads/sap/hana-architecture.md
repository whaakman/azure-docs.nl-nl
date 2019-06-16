---
title: Architectuur van SAP HANA op Azure (grote instanties) | Microsoft Docs
description: Architectuur van SAP HANA op Azure (grote instanties) implementeren.
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
ms.date: 05/25/2019
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d077487f85c789bcdfea3d91e29ee0d44ce82de0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66239447"
---
# <a name="sap-hana-large-instances-architecture-on-azure"></a>Architectuur van SAP HANA (grote instanties) op Azure

Op hoog niveau heeft de SAP HANA op Azure (grote instanties)-oplossing het niveau van de SAP-toepassing die in virtuele machines zich bevinden. De databaselaag bevindt zich op SAP TDI-geconfigureerde hardware die zich in een grote instantie stempel in dezelfde Azure-regio die is verbonden met Azure IaaS.

> [!NOTE]
> Implementeer de SAP-toepassingslaag in dezelfde Azure-regio als de SAP DBMS-laag. Deze regel wordt goed gedocumenteerd in gepubliceerde informatie over SAP-workloads op Azure. 

De algehele architectuur van SAP HANA op Azure (grote instanties) biedt een SAP TDI-gecertificeerde hardwareconfiguratie, namelijk een niet-gevirtualiseerde, bare metal, high performance-server voor de SAP HANA-database. Het biedt ook de mogelijkheid om en flexibiliteit van Azure schalen van resources voor de SAP-toepassingslaag om te voldoen aan uw behoeften.

![Overzicht van de architectuur van SAP HANA op Azure (grote instanties)](./media/hana-overview-architecture/image1-architecture.png)

De architectuur die wordt weergegeven is onderverdeeld in drie secties:

- **juiste**: Geeft een on-premises infrastructuur met verschillende toepassingen in data centers zodat eindgebruikers toegang hebben tot LOB-toepassingen, zoals SAP. In het ideale geval deze on-premises infrastructuur is verbonden met Azure met [ExpressRoute](https://azure.microsoft.com/services/expressroute/).

- **Center**: Toont Azure IaaS en, in dit geval, gebruik van virtuele machines voor het hosten van SAP of andere toepassingen die gebruikmaken van SAP HANA als een DBMS-systeem. Kleinere HANA-instanties die werken met het geheugen waarmee u virtuele machines worden geïmplementeerd in virtuele machines samen met het niveau van hun toepassing. Zie voor meer informatie over virtuele machines, [virtuele machines](https://azure.microsoft.com/services/virtual-machines/).

   Netwerk van Azure-services worden gebruikt voor het groeperen van SAP-systemen, samen met andere toepassingen in virtuele netwerken. Deze virtuele netwerken verbinden met on-premises systemen evenals garantie voor SAP HANA op Azure (grote instanties).

   Zie voor SAP NetWeaver-toepassingen en databases die worden ondersteund om uit te voeren in Azure, [SAP Support Opmerking #1928533-SAP-toepassingen op Azure: Ondersteunde producten en typen Azure VM's](https://launchpad.support.sap.com/#/notes/1928533). Zie voor documentatie over het implementeren van SAP-oplossingen op Azure:

  -  [SAP op virtuele machines van Windows gebruiken](../../virtual-machines-windows-sap-get-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
  -  [SAP-oplossingen op Azure virtual machines gebruiken](get-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

- **Links**: Bevat de SAP HANA TDI-gecertificeerde hardware in de Azure Large Instance-stempel. De eenheden HANA grote instantie zijn verbonden met de virtuele netwerken van uw Azure-abonnement met behulp van dezelfde technologie als de connectiviteit van on-premises naar Azure. Vanaf mei 2019, hebt u een optimalisatie geïntroduceerd waarmee voor de communicatie tussen de eenheden HANA grote instantie en de Azure VM's zonder tussenkomst van de ExpressRoute-Gateway. Deze optimalisatie ExpressRoute snel pad met de naam wordt weergegeven in deze architectuur (rode lijnen). 

De Azure Large Instance-stempel zelf combineert de volgende onderdelen:

- **Computing**: Servers die zijn gebaseerd op de andere generatie Intel Xeon-processors die de benodigde computing mogelijkheid bieden en SAP HANA is gecertificeerd.
- **Netwerk**: Een uniforme dankzij het netwerk-fabric die de computing, opslag en LAN-onderdelen met elkaar verbindt.
- **Opslag**: Een opslaginfrastructuur die toegankelijk is via een uniform netwerk-fabric. De specifieke opslagcapaciteit die wordt geleverd, is afhankelijk van de specifieke SAP HANA op Azure (grote instanties)-configuratie die is geïmplementeerd. Meer opslagcapaciteit is beschikbaar op een extra maandelijkse kosten.

Klanten worden binnen de multitenant-infrastructuur van de stempel grote instantie wordt geïmplementeerd als geïsoleerde tenants. Tijdens de implementatie van de tenant, kunt u een Azure-abonnement naam binnen uw Azure-inschrijving. Dit Azure-abonnement is degene die de HANA grote instantie wordt gefactureerd onder. Deze tenants hebben een 1:1-relatie met de Azure-abonnement. Voor een netwerk is het mogelijk voor toegang tot een eenheid HANA grote instantie is geïmplementeerd in een tenant in een Azure-regio van verschillende virtuele netwerken die deel uitmaken van verschillende Azure-abonnementen. Deze Azure-abonnementen moeten behoren tot dezelfde Azure-inschrijving. 

SAP HANA op Azure (grote instanties) is net als bij virtuele machines, beschikbaar in meerdere Azure-regio's. Als u wilt bieden disaster recovery-mogelijkheden, kunt u opt-in. Andere grote instantie stempels binnen één geografisch politieke regio zijn aan elkaar verbonden. Bijvoorbeeld, zijn HANA grote instantie stempels in VS West en VS-Oost verbonden via een speciaal netwerk-koppeling voor disaster recovery-replicatie. 

Net zoals u tussen verschillende VM-typen met Azure Virtual Machines kiezen kunt, kunt u kiezen uit verschillende SKU's van HANA grote instantie die speciaal voor de werkbelasting van de verschillende typen van SAP HANA geschikt zijn. SAP is van toepassing geheugen processor socket-ratio's voor verschillende workloads op basis van de generaties Intel-processor. De volgende tabel ziet u de SKU-typen die worden aangeboden.

U kunt beschikbare SKU's vinden [beschikbare SKU's voor HLI](hana-available-skus.md).

**Volgende stappen**
- Raadpleeg [netwerkarchitectuur van SAP HANA (grote instanties)](hana-network-architecture.md)