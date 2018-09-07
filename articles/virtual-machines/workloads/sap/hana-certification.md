---
title: Certificering van SAP HANA op Azure (grote instanties) | Microsoft Docs
description: Certificering van SAP HANA op Azure (grote instanties).
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
ms.openlocfilehash: 1346018d99a1c26290ad8928d9794f3b55050910
ms.sourcegitcommit: d211f1d24c669b459a3910761b5cacb4b4f46ac9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/06/2018
ms.locfileid: "44028109"
---
# <a name="certification"></a>Certificering

SAP vereist naast de NetWeaver-certificering, een speciale certificering voor SAP HANA voor de ondersteuning van SAP HANA op bepaalde infrastructuur, zoals Azure IaaS.

De core SAP-notitie op NetWeaver en een certificaat van de SAP HANA mate is [SAP Opmerking #1928533-SAP-toepassingen op Azure: ondersteunde producten en typen Azure VM's](https://launchpad.support.sap.com/#/notes/1928533).

De CA-records voor SAP HANA op Azure (grote instanties)-eenheden kunnen u vinden in de [SAP HANA-gecertificeerde IaaS-platformen](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure) site. 

De SAP HANA op Azure (grote instanties)-typen, waarnaar in de SAP HANA-gecertificeerde site IaaS-platformen, biedt Microsoft en SAP-klanten de mogelijkheid tot het implementeren van grote SAP Business Suite, SAP BW, BW/4HANA, s/4 HANA of andere SAP HANA-workloads in Azure. De oplossing is gebaseerd op het stempel SAP HANA-gecertificeerde toegewezen hardware ([SAP HANA op maat center gegevensintegratie – TDI](https://scn.sap.com/docs/DOC-63140)). Als u een oplossing voor SAP HANA TDI-geconfigureerde uitvoert, worden alle SAP HANA-toepassingen (zoals SAP Business Suite op SAP HANA, SAP BW op HANA SAP S4/HANA en BW4/HANA) werkt op de hardware-infrastructuur.

Vergeleken met het uitvoeren van SAP HANA in virtuele machines, is deze oplossing een voordeel. Het biedt voor veel grotere volumes voor geheugen. Als u wilt deze oplossing inschakelt, moet u inzicht in de volgende belangrijke aspecten:

- De SAP-toepassing-laag en niet-SAP-toepassingen uitvoeren in virtuele machines die worden gehost in de gebruikelijke Azure hardware stempels.
- Klanten on-premises datacenters, infrastructuur en implementaties van toepassingen zijn verbonden met het cloud-platform via ExpressRoute (aanbevolen) of een virtueel particulier netwerk (VPN). Active Directory en DNS worden ook uitgebreid naar Azure.
- De SAP HANA-database-instantie voor HANA-werkbelasting wordt uitgevoerd op SAP HANA op Azure (grote instanties). De stempel grote instantie is verbonden met het Azure-netwerken, zodat de software die wordt uitgevoerd in virtuele machines kan communiceren met de HANA-instantie die wordt uitgevoerd in HANA grote instantie.
- De hardware van SAP HANA op Azure (grote instanties) is specifieke hardware die zijn opgegeven in een IaaS met SUSE Linux Enterprise Server of Red Hat Enterprise Linux vooraf is geïnstalleerd. Net als bij virtuele machines, verdere is updates en onderhoud van het besturingssysteem uw verantwoordelijkheid.
- Installatie van HANA of eventuele extra onderdelen die nodig zijn voor het uitvoeren van SAP HANA op eenheden van HANA grote instantie is uw verantwoordelijkheid. Alle betreffende lopende bewerkingen en het beheer van SAP HANA op Azure zijn ook uw eigen verantwoordelijkheid.
- Naast de oplossingen die hier worden beschreven, kunt u andere onderdelen installeren op uw Azure-abonnement dat verbinding met SAP HANA op Azure (grote instanties maakt). Voorbeelden zijn onderdelen waarmee communicatie met of rechtstreeks naar de SAP HANA-database, zoals Vliegende servers, RDP-servers, SAP HANA Studio, SAP-gegevensservices voor SAP BI-scenario's, of een bewakingsoplossingen.
- Als in Azure biedt HANA grote instantie ondersteuning voor hoge beschikbaarheid en herstel na noodgevallen.

**Volgende stappen**
- Raadpleeg [beschikbare SKU's voor HLI](hana-available-skus.md) 