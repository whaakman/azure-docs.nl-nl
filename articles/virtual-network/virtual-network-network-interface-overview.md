---
title: Netwerkinterfaces in Azure | Microsoft Docs
description: Meer informatie over Azure-netwerkinterfaces en hoe ze worden gebruikt met virtuele machines.
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f58b503f-18bf-4377-aa63-22fc8a96e4be
ms.service: virtual-network
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/24/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 63f2f6dde56c1b5c4b3ad2591700f43f6542874d
ms.openlocfilehash: 395cff80b3f97b6340e15f370c13f783e2f5dde3
ms.lasthandoff: 02/28/2017


---
# <a name="what-are-network-interfaces"></a>Wat zijn netwerkinterfaces?

Een netwerkinterface (NIC) is de onderlinge verbinding tussen een virtuele machine (VM) en het onderliggende softwarenetwerk. In dit artikel wordt uitgelegd wat een netwerkinterface is en hoe deze wordt gebruikt in het Azure Resource Manager-implementatiemodel.

Microsoft raadt het implementeren van nieuwe resources met het Resource Manager-implementatiemodel aan. U kunt ook VM's met netwerkverbinding implementeren via het [klassieke](virtual-network-ip-addresses-overview-classic.md) implementatiemodel. Als u vertrouwd bent met het klassieke model, zijn er belangrijke verschillen in VM-netwerken in het Resource Manager-implementatiemodel. Meer informatie over de verschillen leest u in het artikel [Virtuele machinenetwerk - klassiek](virtual-network-ip-addresses-overview-classic.md#differences-between-resource-manager-and-classic-deployments).

Een netwerkinterface in Azure:

1. Is een resource die kan worden gemaakt, verwijderd en zijn eigen configureerbare instellingen heeft.
2. Moet worden verbonden met één subnet in één Azure Virtual Network (VNet) wanneer deze wordt gemaakt. Als u niet vertrouwd bent met VNets, kunt u hier meer over lezen in het artikel [Overzicht van Virtual Network](virtual-networks-overview.md). De NIC moet zijn verbonden met een VNet die bestaat op dezelfde Azure-[locatie](https://azure.microsoft.com/regions) en in hetzelfde [abonnement](../azure-glossary-cloud-terminology.md#subscription) als de NIC. Nadat een NIC is gemaakt, kunt u het subnet waarmee de NIC is verbonden, wel wijzigen, maar het VNet niet.
3. Krijgt een naam toegewezen die niet kan worden gewijzigd nadat de NIC is gemaakt. De naam moet uniek zijn binnen een Azure-[resourcegroep](../azure-resource-manager/resource-group-overview.md#resource-groups), maar hoeft niet uniek te zijn binnen het abonnement, de Azure-locatie waar de NIC is gemaakt of het VNet waarmee de NIC is verbonden. Binnen één Azure-abonnement worden vaak meerdere NIC's gemaakt. U doet er daarom verstandig aan niet de standaardnamen te hanteren, maar naamgevingsregels te gebruiken die het beheer van meerdere NIC's gemakkelijker maken. Raadpleeg het artikel [Aanbevolen naamgevingsregels voor Azure-resources](../guidance/guidance-naming-conventions.md) voor suggesties.
4. Kan worden gekoppeld aan een virtuele machine, maar kan alleen worden gekoppeld aan één enkele virtuele machine die zich op dezelfde locatie als de NIC bevindt.
5. Heeft een eigen MAC-adres dat behouden blijft zolang de NIC is gekoppeld aan een virtuele machine. Het MAC-adres blijft behouden, ongeacht of de virtuele machine opnieuw wordt opgestart (van binnen het besturingssysteem) of gestopt (toewijzing ongedaan gemaakt) met Azure Portal, Azure PowerShell of de Azure-opdrachtregelinterface. Als de NIC wordt losgekoppeld van de virtuele machine en aan een andere virtuele machine wordt gekoppeld, ontvangt de NIC een ander MAC-adres. Als de NIC is verwijderd, wordt het MAC-adres toegewezen aan andere NIC's.
6. Moet beschikken over één toegewezen primair statisch of dynamisch **privé**-*IPv4*-IP-adres.
7. Kan aan een of meer IP-adresbronnen zijn gekoppeld; gebruik de documentatie [Meerdere IP-adressen per NIC](virtual-network-multiple-ip-addresses-portal.md) voor meer informatie.
8. Ondersteunt versnelde netwerken met I/O-virtualisatie met één root (SR-IOV) voor specifieke VM-grootten waarop specifieke versies van het besturingssysteem Microsoft Windows Server worden uitgevoerd. Lees het artikel [Versnelde netwerken voor een virtuele machine](virtual-network-accelerated-networking-powershell.md) voor meer informatie over deze PREVIEW-functie.
9. Kan verkeer ontvangen dat niet bedoeld is voor privé-IP-adressen die eraan zijn toegewezen, als Doorsturen via IP is ingeschakeld voor de NIC. Als een virtuele machine bijvoorbeeld firewallsoftware uitvoert, worden pakketten die niet bestemd zijn voor de eigen IP-adressen, gerouteerd. Op de virtuele machine moet nog steeds software worden uitgevoerd die geschikt is voor routering of doorsturen van verkeer. Hiervoor moet u echter Doorsturen via IP inschakelen voor een NIC.
10. Wordt vaak gemaakt in dezelfde resourcegroep waarin de virtuele machine zich bevindt die aan de NIC is gekoppeld of in hetzelfde VNet waarmee de NIC is verbonden. Dit is echter niet vereist.

## <a name="vms-with-multiple-network-interfaces"></a>VM's met meerdere netwerkinterfaces
Aan een virtuele machine kunnen meerdere NIC's worden gekoppeld, maar houd hierbij rekening met het volgende:  

* De VM-grootte moet meerdere NIC's ondersteunen. Meer informatie over de VM-grootten die meerdere NIC's ondersteunen, vindt u in de artikelen [Windows Server VM-grootten](../virtual-machines/virtual-machines-windows-sizes.md) en [Linux VM-grootten](../virtual-machines/virtual-machines-linux-sizes.md).
* De virtuele machine moet worden gemaakt met ten minste twee NIC's. Als de virtuele machine is gemaakt met slechts één NIC, zelfs als de VM-grootte meer dan één NIC ondersteunt, kunt u geen extra NIC's koppelen aan de virtuele machine nadat deze is gemaakt. Als de virtuele machine is gemaakt met ten minste twee NIC's, kunt u extra NIC's koppelen aan de virtuele machine nadat deze is gemaakt, mits de VM-grootte meer dan twee NIC's ondersteunt.  
* U kunt de secundaire NIC's (de primaire NIC kan niet worden losgekoppeld) loskoppelen van een virtuele machine als er ten minste drie NIC's aan de VM zijn gekoppeld. U kunt NIC's niet loskoppelen als er twee of minder NIC's aan de virtuele machine zijn gekoppeld.  
* De volgorde van de NIC's binnen de virtuele machine is willekeurig en kan ook wijzigen in Azure-infrastructuurupdates. De IP-adressen en de bijbehorende ethernet-MAC-adressen blijven echter dezelfde. Stel bijvoorbeeld dat het besturingssysteem Azure NIC1 identificeert als Eth1. Eth1 heeft IP-adres 10.1.0.100 en MAC-adres 00-0D-3A-B0-39-0D. Nadat een Azure-infrastructuur is bijgewerkt en opnieuw is opgestart, kan het besturingssysteem nu Azure NIC1 identificeren als Eth2, maar de IP- en MAC-adressen zullen dezelfde zijn als voorheen (toen het besturingssysteem Azure NIC1 identificeerde als Eth1). Wanneer het opnieuw opstarten door de klant wordt uitgevoerd, blijft de NIC-volgorde binnen het besturingssysteem dezelfde.  
* Als de virtuele machine lid is van een [beschikbaarheidsset](../azure-glossary-cloud-terminology.md#availability-set), moeten alle virtuele machines binnen de beschikbaarheidsset óf één NIC óf meerdere NIC's hebben. Als de virtuele machines meerdere NIC's hebben, hoeft het aantal NIC's per VM niet identiek te zijn, als elke VM maar ten minste twee NIC's heeft.

## <a name="next-steps"></a>Volgende stappen
* Lees het artikel [Een VM maken](../virtual-machines/virtual-machines-windows-hero-tutorial.md) voor meer informatie over het maken van een virtuele machine met één NIC.
* Lees het artikel [Een VM implementeren met meerdere NIC's](virtual-network-deploy-multinic-arm-ps.md) voor meer informatie over het maken van een virtuele machine met meerdere NIC's.
* Lees het artikel [Meerdere IP-adressen voor virtuele Azure-machines](virtual-network-multiple-ip-addresses-powershell.md) voor meer informatie over het maken van een NIC met meerdere IP-configuraties.


