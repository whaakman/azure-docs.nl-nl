---
title: SAP gebruiken op Windows virtuele machines | Microsoft Docs
description: Schakel over het gebruik van SAP op virtuele machines (VM's) in Microsoft Azure
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: MSSedusch
manager: timlt
editor: 
tags: azure-service-management
keywords: 
ms.assetid: 1b455be4-c02f-43e3-8d39-c2d5f216e646
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: campaign-page
ms.tgt_pltfrm: vm-windows
ms.workload: na
ms.date: 10/04/2016
ms.author: sedusch
ms.openlocfilehash: 1d6326d5f79ea4e975abd1aa90b0d4a635f4c31a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="using-sap-on-windows-virtual-machines-in-azure"></a>SAP gebruiken op Windows virtuele machines in Azure
Cloud computing is een veelgebruikte term die in de IT-sector steeds belangrijker wordt, zowel voor kleine bedrijven als voor grote ondernemingen en multinationals. Microsoft Azure is het cloudserviceplatform van Microsoft dat een groot aantal nieuwe mogelijkheden biedt. Klanten kunnen toepassingen nu snel als cloudservices inrichten en de inrichting weer ongedaan maken, zodat technische of financiële beperkingen geen rol spelen. In plaats van tijd en geld te investeren in de hardware-infrastructuur kunnen bedrijven zich nu richten op de toepassing, bedrijfsprocessen en de voordelen voor klanten en gebruikers.

Met Microsoft Azure virtuele machines biedt Microsoft een uitgebreide infrastructuur als een Service (IaaS)-platform. SAP NetWeaver-toepassingen worden ondersteund op virtuele Azure-machines (IaaS). De whitepapers die hieronder wordt beschreven hoe plannen en implementeren van toepassingen voor SAP NetWeaver gebaseerd op Windows virtuele machines in Azure. U kunt ook SAP NetWeaver op basis van toepassingen implementeren op [virtuele Linux-machines](../../linux/classic/sap-get-started.md).

[!INCLUDE [virtual-machines-common-classic-sap-get-started](../../../../includes/virtual-machines-common-classic-sap-get-started.md)]

## <a name="sap-netweaver-on-azure---ha"></a>SAP NetWeaver op Azure - HA
Titel: SAP NetWeaver op Azure - Clustering SAP ASC's / SCS exemplaren die gebruikmaken van Windows Server Failover Cluster in Azure met SIOS DataKeeper

Overzicht: ' dit document wordt beschreven hoe u SIOS DataKeeper gebruikt voor het instellen van een maximaal beschikbare configuratie SAP ASC's / SCS op Azure. SAP beveiligt hun storingspunt fout onderdelen, zoals SAP ASC's / SCS of in de wachtrij plaatsen replicatie Services met Windows Server Failover Cluster-configuraties waarvoor de gedeelde schijven. Deze SAP-onderdelen zijn essentieel voor de functionaliteit van een SAP-systeem. Functionaliteit voor hoge beschikbaarheid moet daarom nemen om ervoor te zorgen dat deze onderdelen een storing van een server of een virtuele machine als u doen met Windows clusterconfiguraties voor bare metal- en Hyper-V-omgevingen kunnen tolereren. Vanaf augustus 2015 kan geen Azure op zichzelf leveren gedeelde schijven die vereist voor de Windows zijn op basis van maximaal beschikbare configuraties die nodig zijn voor deze kritieke SAP-onderdelen. Met behulp van het product DataKeeper door SIOS kunnen echter Failover-Cluster van Windows Server-configuraties die nodig zijn voor SAP ASC's / SCS worden gebaseerd op het Azure IaaS-platform. Dit artikel wordt beschreven in een benadering stap voor stap het installeren van een Failover-Cluster van Windows Server-configuratie met gedeelde schijf geleverd door SIOS Datakeeper in Azure. Het artikel wordt uitgelegd van details in configuraties aan de kant Azure, Windows en SAP, waardoor de configuratie voor hoge beschikbaarheid in een optimale wijze werken. Het artikel is een aanvulling op de SAP installatie documentatie en -opmerkingen SAP staan voor de primaire resources voor installaties en -implementaties van SAP-software op gegeven platforms.

Bijgewerkte: Augustus 2015

[Deze handleiding nu downloaden](http://go.microsoft.com/fwlink/?LinkId=613056)

