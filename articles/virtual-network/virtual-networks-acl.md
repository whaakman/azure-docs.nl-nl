---
title: Wat is een netwerk van Azure access control list?
description: Meer informatie over toegangscontrolelijsten in Azure
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 83d66c84-8f6b-4388-8767-cd2de3e72d76
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.openlocfilehash: 9a0c85367968c9b38104012d75b1f3975be82cc1
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="what-is-an-endpoint-access-control-list"></a>Wat is er een eindpunt-ACL's?

> [!IMPORTANT]
> Azure heeft twee verschillende [implementatiemodellen](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) voor het maken en werken met resources: Resource Manager en classic. Dit artikel gaat over het gebruik van het klassieke implementatiemodel. Microsoft raadt aan dat de meeste nieuwe implementaties het Resource Manager-implementatiemodel gebruiken. 

Een eindpunt toegangsbeheerlijst (ACL) is een beveiligingsverbetering beschikbaar voor uw Azure-implementatie. Een ACL biedt de mogelijkheid om selectief toestaan of weigeren van verkeer voor het eindpunt van een virtuele machine. Deze pakketten filteren mogelijkheid biedt een extra beveiligingslaag. U kunt de netwerk-ACL's voor eindpunten alleen opgeven. U kunt een ACL voor een virtueel netwerk of een specifiek subnet opgenomen in een virtueel netwerk niet opgeven. Het is raadzaam netwerkbeveiligingsgroepen (nsg's) gebruiken in plaats van ACL's, indien mogelijk. Zie voor meer informatie over het nsg's, [netwerk beveiligingsoverzicht van groep](virtual-networks-nsg.md)

ACL's kunnen worden geconfigureerd met behulp van PowerShell of de Azure-portal. Zie voor meer informatie over het configureren van een ACL-netwerk met behulp van PowerShell [toegangsbeheerlijsten voor eindpunten met behulp van PowerShell beheren](virtual-networks-acl-powershell.md). Zie voor meer informatie over het configureren van een ACL-netwerk met behulp van de Azure-portal [het instellen van eindpunten aan een virtuele machine](../virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

Netwerk-ACL's gebruikt, kunt u het volgende doen:

* Selectief toestaan of weigeren van inkomend verkeer op basis van een extern subnet IPv4-adresbereik op het invoereindpunt van een virtuele machine.
* Blacklist IP-adressen
* Meerdere regels per eindpunt van de virtuele machine maken
* Gebruik regel ordening om te controleren of de juiste set regels worden toegepast op een opgegeven virtuele machine-eindpunt (laagste en hoogste)
* Geef een ACL voor een specifieke extern subnet IPv4-adres.

Zie de [Azure beperkt](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#networking-limits) artikel voor ACL-limieten.

## <a name="how-acls-work"></a>De werking van ACL 's
Een ACL is een object dat een lijst met regels bevat. Wanneer u een ACL te maken en op het eindpunt van een virtuele machine toepassen, pakket voor het filteren van plaatsvindt op de hostknooppunt van uw virtuele machine. Dit betekent dat het verkeer van externe IP-adressen wordt gefilterd op het hostknooppunt voor de overeenkomende ACL-regels in plaats van op de virtuele machine. Dit voorkomt dat uw virtuele machine de kostbare CPU-cycli uitgaven voor het filteren van netwerkpakketten.

Wanneer een virtuele machine is gemaakt, wordt een standaard ACL plaatsen om alle binnenkomende verkeer blokkeren. Echter, als een eindpunt is gemaakt voor (poort 3389), klikt u vervolgens de standaard ACL wordt gewijzigd zodat al het binnenkomende verkeer voor dat eindpunt. Binnenkomend verkeer van een extern subnet vervolgens naar dat eindpunt is toegestaan en geen firewall inrichting is vereist. Alle andere poorten worden geblokkeerd voor binnenkomend verkeer, tenzij eindpunten voor deze poorten worden gemaakt. Standaard is uitgaand verkeer toegestaan.

**Standaard ACL voorbeeldtabel**

| **Regel #** | **Extern Subnet** | **Eindpunt** | **Toestaan/weigeren** |
| --- | --- | --- | --- |
| 100 |0.0.0.0/0 |3389 |Toestaan |

## <a name="permit-and-deny"></a>Toestaan en weigeren
U kunt selectief toestaan of weigeren voor een virtuele machine-invoereindpunt verkeer door regels die specificeren 'toestaan' of 'niet toestaan' te maken. Het is belangrijk te weten dat standaard, wanneer een eindpunt is gemaakt, al het verkeer is toegestaan voor het eindpunt. Daarom is het belangrijk om te begrijpen hoe toestaan/weigeren regels maken en plaats deze in de juiste volgorde van prioriteit als u wilt dat gedetailleerde controle over het netwerkverkeer dat u wilt toestaan dat het eindpunt van de virtuele machine te bereiken.

Aandachtspunten:

1. **Er is geen ACL-** standaard wanneer een eindpunt is gemaakt, we verlenen voor het eindpunt.
2. **Toestaan -** wanneer u een of meer 'toestaan' bereiken toevoegt, weigert u alle andere bereiken standaard. Alleen de pakketten uit de toegestane IP-adresbereik is mogelijk om te communiceren met het eindpunt van de virtuele machine.
3. **DENY -** wanneer u een of meer 'niet toestaan' bereiken toevoegt, verleent u andere bereiken van het verkeer standaard.
4. **Combinatie van toestaan en weigeren -** kunt u een combinatie van 'toestaan' en 'geweigerd' wanneer u reserveren van een specifiek IP-adresbereik wilt moet worden toegestaan of geweigerd.

## <a name="rules-and-rule-precedence"></a>Regels en de prioriteit van regels
Netwerk-ACL's kunnen worden ingesteld op de eindpunten van specifieke virtuele machine. U kunt bijvoorbeeld een netwerk-ACL voor een RDP-eindpunt gemaakt op een virtuele machine welke vergrendelingen de toegang voor bepaalde IP-adressen opgeven. De onderstaande tabel ziet een manier om toegang te verlenen aan de openbare virtuele IP-adressen (VIP's) van een bepaald bereik om toegang te verlenen voor RDP. Alle andere externe IP-adressen geweigerd. Er volgt een *laagste voorrang* regel volgorde.

### <a name="multiple-rules"></a>Meerdere regels
In het onderstaande voorbeeld als u wilt toegang tot het RDP-eindpunt toestaan alleen vanaf twee openbare IPv4-adresbereiken (65.0.0.0/8 en 159.0.0.0/8), u kunt dit doen door op te geven dat twee *toestaan* regels. In dit geval aangezien RDP standaard voor een virtuele machine wordt gemaakt, kunt u toegang tot de RDP-poort op basis van een extern subnet vergrendelen. Het volgende voorbeeld ziet een manier om toegang te verlenen aan de openbare virtuele IP-adressen (VIP's) van een bepaald bereik om toegang te verlenen voor RDP. Alle andere externe IP-adressen geweigerd. Dit werkt omdat netwerk ACL's kan worden ingesteld voor een specifieke virtuele machine-eindpunt en standaard toegang is geweigerd.

**Voorbeeld: meerdere regels**

| **Regel #** | **Extern Subnet** | **Eindpunt** | **Toestaan/weigeren** |
| --- | --- | --- | --- |
| 100 |65.0.0.0/8 |3389 |Toestaan |
| 200 |159.0.0.0/8 |3389 |Toestaan |

### <a name="rule-order"></a>De regelvolgorde van de
Omdat meerdere regels kunnen worden opgegeven voor een eindpunt, moet er een manier te organiseren regels om te bepalen welke regels voorrang. De regelvolgorde geeft prioriteit. Netwerk-ACL's Volg een *laagste voorrang* regel volgorde. Het eindpunt op poort 80 is in het onderstaande voorbeeld selectief toegang tot alleen bepaalde IP-adresbereiken. Voor het configureren van dit hebben we een regel voor weigeren (regel \# 100) voor adressen in de ruimte 175.1.0.1/24. Een tweede regel is vervolgens met prioriteit 200 dat toegang tot alle adressen onder 175.0.0.0/8 biedt opgegeven.

**Voorbeeld: de prioriteit van regels**

| **Regel #** | **Extern Subnet** | **Eindpunt** | **Toestaan/weigeren** |
| --- | --- | --- | --- |
| 100 |175.1.0.1/24 |80 |Weigeren |
| 200 |175.0.0.0/8 |80 |Toestaan |

## <a name="network-acls-and-load-balanced-sets"></a>Netwerk-ACL's en taakverdeling sets laden
Netwerk-ACL's kunnen worden opgegeven op een eindpunt van de set taakverdeling. Als een ACL voor een set met gelijke taakverdeling is opgegeven, wordt het netwerk ACL toegepast op alle virtuele machines in die set met gelijke taakverdeling. Bijvoorbeeld, als een met gelijke taakverdeling set is gemaakt met 'Poort 80' en de set met gelijke taakverdeling 3 VM's, het netwerk ACL gemaakt op eindpunt 'Poort 80' van een die virtuele machine automatisch op de andere virtuele machines toegepast wordt.

![Netwerk-ACL's en taakverdeling sets laden](./media/virtual-networks-acl/IC674733.png)

## <a name="next-steps"></a>Volgende stappen
[Toegangsbeheerlijsten voor eindpunten met behulp van PowerShell beheren](virtual-networks-acl-powershell.md)

