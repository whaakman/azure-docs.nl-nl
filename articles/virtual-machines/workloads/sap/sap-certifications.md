---
title: Microsoft Azure-certificeringen voor SAP | Microsoft Docs
description: Bijgewerkte lijst met huidige configuraties en certificeringen van SAP op het Azure-platform.
services: virtual-machines-linux
documentationcenter: ''
author: RicksterCDN
manager: jeconnoc
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 07/12/2018
ms.author: rclaus
ms.custom: ''
ms.openlocfilehash: d1d2bd74a5118b48f7ed79a77ca2d7b6545607c6
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/17/2018
ms.locfileid: "39072377"
---
# <a name="sap-certifications-and-configurations-running-on-microsoft-azure"></a>SAP-certificeringen en configuraties die worden uitgevoerd op Microsoft Azure

SAP en Microsoft hebben een lange geschiedenis samen te werken in een sterke samenwerking met wederzijdse voordelen voor hun klanten. Microsoft wordt voortdurend bijgewerkt voor het platform en het indienen van nieuwe Certificeringsgegevens SAP om ervoor te zorgen van Microsoft Azure is het beste platform waarop u uw SAP-workloads uitvoeren. De volgende tabellen overzicht Azure ondersteunde configuraties en SAP-certificeringen groeiende lijst. 

## <a name="sap-hana-certifications"></a>SAP HANA-certificeringen
Referenties:

- [SAP HANA-gecertificeerde IaaS-platformen](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure) voor SAP HANA-ondersteuning voor systeemeigen Azure-VM's en HANA grote instanties.

| SAP-product | Ondersteund besturingssysteem | Aanbiedingen voor Azure |
| --- | --- | --- |
| SAP HANA Developer Edition (inclusief de HANA-clientsoftware bestaat uit SQLODBC, ODBO-Windows alleen, ODBC, JDBC-stuurprogramma's, HANA studio en HANA database) | Red Hat Enterprise Linux, SUSE Linux Enterprise | Uit de D-serie VM-reeks |
| Zakelijke één op HANA | SUSE Linux Enterprise | DS14_v2 |
| SAP S/4 HANA | Red Hat Enterprise Linux, SUSE Linux Enterprise | Gecontroleerde beschikbaarheid voor GS5, M64s, M64ms, M128s, M128ms, SAP HANA op Azure (grote instanties) |
| Suite op HANA, OLTP | Red Hat Enterprise Linux, SUSE Linux Enterprise | GS5 voor niet-productie scenario's, M64s, M64ms, M128s, M128ms, SAP HANA op Azure (grote instanties) |
| HANA Enterprise voor BW, OLAP | Red Hat Enterprise Linux, SUSE Linux Enterprise | GS5, M64s, M64ms, M128s, M128ms, SAP HANA op Azure (grote instanties) |
| SAP BW/4 HANA | Red Hat Enterprise Linux, SUSE Linux Enterprise | GS5, M64s, M64ms, M128s, M128ms, SAP HANA op Azure (grote instanties) |

Alle Azure-virtuele machines zijn gecertificeerd voor SAP HANA omhoog tot nu toe.

## <a name="sap-netweaver-certifications"></a>SAP NetWeaver-certificeringen
Microsoft Azure is gecertificeerd voor de volgende SAP-producten, met volledige ondersteuning van Microsoft en SAP.
Referenties:

- [1928533 - SAP-toepassingen op Azure: ondersteunde producten en Azure-VM-typen](https://launchpad.support.sap.com/#/notes/1928533) voor alle SAP NetWeaver op basis van toepassingen, waaronder SAP TREX LiveCache SAP en SAP-Server voor webinhoud. En alle databases, met uitzondering van SAP HANA.


| SAP-product | Gastbesturingssysteem | RDBMS | Typen virtuele machines |
| --- | --- | --- | --- |
| SAP Business Suite-software | Windows, SUSE Linux Enterprise, Red Hat Enterprise Linux, Oracle Linux |SQL Server, Oracle (Windows en alleen voor Oracle Linux), DB2, SAP ASE |A5 tot A11, D11 tot D14, DS11 tot DS14, DS11_v2 naar DS15_v2, GS1 tot GS5, D2s_v3 naar D64s_v3, E2s_v3 naar E64s_v3, M64s naar M128ms |
| SAP Business All-in-One | Windows, SUSE Linux Enterprise, Red Hat Enterprise Linux, Oracle Linux |SQL Server, Oracle (Windows en alleen voor Oracle Linux), DB2, SAP ASE |A5 tot A11, D11 tot D14, DS11 tot DS14, DS11_v2 naar DS15_v2, GS1 tot GS5, D2s_v3 naar D64s_v3, E2s_v3 naar E64s_v3, M64s naar M128ms |
| SAP Business Objects BI | Windows |N/A |A5 tot A11, D11 tot D14, DS11 tot DS14, DS11_v2 naar DS15_v2, GS1 tot GS5, D2s_v3 naar D64s_v3, E2s_v3 naar E64s_v3, M64s naar M128ms |
| SAP NetWeaver | Windows, SUSE Linux Enterprise, Red Hat Enterprise Linux, Oracle Linux |SQL Server, Oracle (Windows en alleen voor Oracle Linux), DB2, SAP ASE |A5 tot A11, D11 tot D14, DS11 tot DS14, DS11_v2 naar DS15_v2, GS1 tot GS5, D2s_v3 naar D64s_v3, E2s_v3 naar E64s_v3, M64s naar M128ms |

## <a name="other-sap-workload-supported-on-azure"></a>Andere SAP werkbelasting die wordt ondersteund op Azure

| SAP-product | Gastbesturingssysteem | RDBMS | Typen virtuele machines |
| --- | --- | --- | --- |
| SAP Business een op SQL Server | Windows  | SQL Server | Alle NetWeaver gecertificeerde VM-typen<br /> [SAP-notitie #928839](https://launchpad.support.sap.com/#/notes/928839) |
| SAP BITS PER KANAAL 10.01 MS SP08 | Windows en Linux | | Alle NetWeaver gecertificeerde VM-typen<br /> SAP-notitie #2451795 |
| SAP Business objecten BI-platform | Windows en Linux | | SAP-notitie #2145537 |
| SAP Data Services 4.2 | | | SAP-notitie #2288344 |
| SAP Hybris Commerce Platform 5.x en 6.x | Windows | SQL Server, Oracle | Alle NetWeaver gecertificeerde VM-typen<br /> [Hybris Wiki](https://wiki.hybris.com/display/SUP/Using+the+hybris+Platform+with+the+Cloud) |
