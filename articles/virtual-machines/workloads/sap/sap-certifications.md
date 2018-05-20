---
title: Microsoft Azure certificeringen voor SAP | Microsoft Docs
description: Bijgewerkte lijst met huidige configuraties en certificeringen van SAP op de Azure-platform.
services: virtual-machines-linux
documentationcenter: ''
author: RicksterCDN
manager: timlt
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/11/2018
ms.author: rclaus
ms.custom: ''
ms.openlocfilehash: 1038f0c8a967c05ebf07849619268f22b1eb0c4b
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/14/2018
---
# <a name="sap-certifications-and-configurations-running-on-microsoft-azure"></a>SAP certificeringen en configuraties die worden uitgevoerd op Microsoft Azure

SAP en Microsoft hebben een lang geschiedenisgegevens samen te werken in een sterke relatie met wederzijdse voordelen voor hun klanten. Het platform wordt voortdurend bijgewerkt door Microsoft en nieuwe Certificeringsgegevens SAP verzenden om ervoor te zorgen Microsoft Azure is het beste platform waarop de werkbelasting van uw SAP uitgevoerd. De volgende tabellen overzicht Azure ondersteunde configuraties en lijst met groeiende SAP certificeringen. 

## <a name="sap-hana-certifications"></a>SAP HANA-certificeringen
Referenties:

- [SAP HANA gecertificeerd IaaS platforms](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure) voor SAP HANA-ondersteuning voor systeemeigen Azure VM's en grote HANA-exemplaren.

| SAP-product | Ondersteund besturingssysteem | Aanbiedingen voor Azure |
| --- | --- | --- |
| SAP HANA-ontwikkelaarsversie (met inbegrip van de clientsoftware HANA bestaan uit SQLODBC, ODBO-alleen Windows, ODBC, JDBC-stuurprogramma's, HANA studio en HANA-database) | Red Hat Enterprise Linux SUSE Linux Enterprise | D-reeks VM-serie |
| Een op HANA Business | SUSE Linux Enterprise | DS14_v2 |
| S/4-HANA SAP | Red Hat Enterprise Linux SUSE Linux Enterprise | Gecontroleerde beschikbaarheid voor GS5, M64s, M64ms, M128s, M128ms, SAP HANA in Azure (grote exemplaren) |
| Suite op HANA, OLTP | Red Hat Enterprise Linux SUSE Linux Enterprise | GS5 voor niet-productieve scenario's, M64s, M64ms, M128s, M128ms, SAP HANA in Azure (grote exemplaren) |
| HANA Enterprise voor BW, OLAP | Red Hat Enterprise Linux SUSE Linux Enterprise | GS5, M64s, M64ms, M128s, M128ms, SAP HANA in Azure (grote exemplaren) |
| SAP BW/4 HANA | Red Hat Enterprise Linux SUSE Linux Enterprise | GS5, M64s, M64ms, M128s, M128ms, SAP HANA in Azure (grote exemplaren) |

Alle Azure VM's zijn gecertificeerd voor SAP HANA scale-up van de tot nu toe.

## <a name="sap-netweaver-certifications"></a>SAP NetWeaver-certificeringen
Microsoft Azure is gecertificeerd voor de volgende SAP-producten, met volledige ondersteuning van Microsoft en SAP.
Referenties:

- [1928533 - SAP-toepassingen in Azure: typen ondersteunde producten en de virtuele machine van Azure](https://launchpad.support.sap.com/#/notes/1928533) voor alle SAP NetWeaver op basis van toepassingen, zoals SAP TREX, SAP LiveCache en SAP-inhoudsserver. En alle databases, met uitzondering van SAP HANA.


| SAP-product | Gastbesturingssysteem | RDBMS | Typen virtuele machines |
| --- | --- | --- | --- |
| SAP Business Suite-software | Windows, SUSE Linux Enterprise, Red Hat Enterprise Linux, Oracle Linux |SQL Server, Oracle (Windows en alleen voor Oracle Linux), DB2, SAP-as-omgeving |A5 naar A11, D11 naar D14, DS11 naar DS14, DS11_v2 naar DS15_v2, GS1 naar GS5, D2s_v3 naar D64s_v3, E2s_v3 naar E64s_v3, M64s naar M128ms |
| SAP Business All-in-One | Windows, SUSE Linux Enterprise, Red Hat Enterprise Linux, Oracle Linux |SQL Server, Oracle (Windows en alleen voor Oracle Linux), DB2, SAP-as-omgeving |A5 naar A11, D11 naar D14, DS11 naar DS14, DS11_v2 naar DS15_v2, GS1 naar GS5, D2s_v3 naar D64s_v3, E2s_v3 naar E64s_v3, M64s naar M128ms |
| SAP Business Objects BI | Windows |N/A |A5 naar A11, D11 naar D14, DS11 naar DS14, DS11_v2 naar DS15_v2, GS1 naar GS5, D2s_v3 naar D64s_v3, E2s_v3 naar E64s_v3, M64s naar M128ms |
| SAP NetWeaver | Windows, SUSE Linux Enterprise, Red Hat Enterprise Linux, Oracle Linux |SQL Server, Oracle (Windows en alleen voor Oracle Linux), DB2, SAP-as-omgeving |A5 naar A11, D11 naar D14, DS11 naar DS14, DS11_v2 naar DS15_v2, GS1 naar GS5, D2s_v3 naar D64s_v3, E2s_v3 naar E64s_v3, M64s naar M128ms |

## <a name="other-sap-workload-supported-on-azure"></a>Andere SAP-werkbelasting ondersteund in Azure

| SAP-product | Gastbesturingssysteem | RDBMS | Typen virtuele machines |
| --- | --- | --- | --- |
| SAP Business één op SQL Server | Windows  | SQL Server | Alle NetWeaver gecertificeerd VM-typen |
| SAP BITS PER KANAAL 10.01 MS SP08 | Windows en Linux | | Alle NetWeaver gecertificeerd VM-typen<br /> SAP-notitie #2451795 |
| SAP Business objecten BI-platform | Windows en Linux | | SAP-notitie #2145537 |
| SAP Data Services 4.2 | | | SAP-notitie #2288344 |
| SAP Hybris Commerce Platform 5.x en 6.x | Windows | SQL Server, Oracle | Alle NetWeaver gecertificeerd VM-typen<br /> [Hybris Wiki](https://wiki.hybris.com/display/SUP/Using+the+hybris+Platform+with+the+Cloud) |
