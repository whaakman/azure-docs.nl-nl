---
title: Microsoft Azure certificeringen voor SAP | Microsoft Docs
description: Bijgewerkte lijst met huidige configuraties en certificeringen van SAP op de Azure-platform.
services: virtual-machines-linux
documentationcenter: 
author: RicksterCDN
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 10/12/2017
ms.author: rclaus
ms.custom: 
ms.openlocfilehash: 865fa54c908481b3f4c211f12293538c617b6129
ms.sourcegitcommit: a036a565bca3e47187eefcaf3cc54e3b5af5b369
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/17/2017
---
# <a name="sap-certifications-and-configurations-running-on-microsoft-azure"></a>SAP certificeringen en configuraties die worden uitgevoerd op Microsoft Azure

SAP en Microsoft hebben een lang geschiedenisgegevens samen te werken in een sterke relatie met wederzijdse voordelen voor hun klanten. Het platform wordt voortdurend bijgewerkt door Microsoft en nieuwe Certificeringsgegevens SAP verzenden om ervoor te zorgen Microsoft Azure is het beste platform waarop de werkbelasting van uw SAP uitgevoerd. De volgende tabellen worden onze ondersteunde configuraties en de lijst met groeiende certificeringen. 

## <a name="sap-hana-certifications"></a>SAP HANA-certificeringen
Referenties:

- [SAP-notitie 2316233 - SAP HANA op Microsoft Azure (grote exemplaren)](https://launchpad.support.sap.com/#/notes/2316233) die betrekking hebben op grote exemplaren HANA met betrekking tot SAP HANA-ondersteuning.
- [SAP HANA gecertificeerd IaaS platforms](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Amazon%20Web%20Services%2CMicrosoft%20Azure) voor SAP HANA-ondersteuning voor systeemeigen Azure VM's.

| SAP-product | Ondersteund besturingssysteem | Aanbiedingen voor Azure |
| --- | --- | --- |
| SAP HANA-ontwikkelaarsversie (met inbegrip van de clientsoftware HANA bestaan uit SQLODBC, ODBO-alleen Windows, ODBC, JDBC-stuurprogramma's, HANA studio en HANA-database) | Red Hat Enterprise Linux SUSE Linux Enterprise | D-reeks VM-serie |
| Een op HANA Business | SUSE Linux Enterprise | DS14_v2 |
| S/4-HANA SAP |Red Hat Enterprise Linux SUSE Linux Enterprise | Gecontroleerde beschikbaarheid voor GS5, SAP HANA in Azure (grote exemplaren) |
| Suite op HANA, OLTP | Red Hat Enterprise Linux SUSE Linux Enterprise | GS5 voor één knooppunt implementaties voor scenario's met niet-productieve SAP HANA in Azure (grote exemplaren) |
| HANA Enterprise voor BW, OLAP | Red Hat Enterprise Linux SUSE Linux Enterprise | GS5 voor implementaties met één knooppunt, SAP HANA in Azure (grote exemplaren) |
| SAP BW/4 HANA | Red Hat Enterprise Linux SUSE Linux Enterprise | GS5 voor implementaties met één knooppunt, SAP HANA in Azure (grote exemplaren) |

## <a name="sap-netweaver-certifications"></a>SAP NetWeaver-certificeringen
Microsoft Azure is gecertificeerd voor de volgende SAP-producten, met volledige ondersteuning van Microsoft en SAP.
Referenties:

- [1928533 - SAP-toepassingen in Azure: typen ondersteunde producten en de virtuele machine van Azure](https://launchpad.support.sap.com/#/notes/1928533) voor alle SAP NetWeaver op basis van toepassingen, waaronder SAP TREX, SAP LiveCache en SAP-inhoudsserver. En alle databases, met uitzondering van SAP HANA.


| SAP-product | Gastbesturingssysteem | RDBMS | Typen virtuele machines |
| --- | --- | --- | --- |
| SAP Business Suite-software |Windows, SUSE Linux Enterprise, Red Hat Enterprise Linux, Oracle Linux |SQL Server, Oracle (Windows en alleen voor Oracle Linux), DB2, SAP-as-omgeving |A5 naar A11, D11 naar D14, DS11 naar DS14, DS11_v2 naar DS15_v2, GS1 naar GS5, M-serie |
| SAP Business All-in-One |Windows, SUSE Linux Enterprise, Red Hat Enterprise Linux |SQL Server, Oracle (Windows en alleen voor Oracle Linux), DB2, SAP-as-omgeving |A5 naar A11, D11 naar D14, DS11 naar DS14, DS11_v2 naar DS15_v2, GS1 naar GS5, M-serie |
| SAP Business Objects BI |Windows |N.v.t. |A5 naar A11, D11 naar D14, DS11 naar DS14, DS11_v2 naar DS15_v2, GS1 naar GS5, M-serie |
| SAP NetWeaver |Windows, SUSE Linux Enterprise, Red Hat Enterprise Linux |SQL Server, Oracle (Windows en alleen voor Oracle Linux), DB2, SAP-as-omgeving |A5 naar A11, D11 naar D14, DS11 naar DS14, DS11_v2 naar DS15_v2, GS1 naar GS5, M-serie |

## <a name="other-sap-workload-supported-on-azure"></a>Andere SAP-werkbelasting ondersteund in Azure

| SAP-product | Gastbesturingssysteem | RDBMS | Typen virtuele machines |
| --- | --- | --- | --- |
| SAP Business één op SQL Server | Windows  | SQL Server | Alle NetWeaver gecertificeerd VM-typen |
| SAP BITS PER KANAAL 10.01 MS SP08 | Windows | | Alle NetWeaver gecertificeerd VM-typen<br /> SAP-notitie #2451795 |
| SAP Business objecten BI-platform | Windows | | SAP-notitie #2145537 |
| SAP-gegevensservices 4.2 | | | SAP-notitie #2288344 |
| SAP Hybris Commerce Platform 5.x en 6.x | Windows | SQL Server, Oracle | Alle NetWeaver gecertificeerd VM-typen<br /> [Hybris Wiki](https://wiki.hybris.com/display/SUP/Using+the+hybris+Platform+with+the+Cloud) |
