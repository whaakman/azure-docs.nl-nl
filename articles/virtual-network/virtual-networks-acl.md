---
title: Wat is een toegangsbeheerlijst voor Azure-netwerk?
description: Meer informatie over toegangscontrolelijsten in Azure
services: virtual-network
documentationcenter: na
author: genlin
manager: cshepard
editor: ''
tags: azure-service-management
ms.assetid: 83d66c84-8f6b-4388-8767-cd2de3e72d76
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: genli
ms.openlocfilehash: 3a7155380a51273d376226c6be7a004f386181ce
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "61035246"
---
# <a name="what-is-an-endpoint-access-control-list"></a>Wat is er een eindpunt-ACL?

> [!IMPORTANT]
> Azure heeft twee verschillende [implementatiemodellen](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) voor het maken van en werken met resources: Resource Manager en klassiek. Dit artikel gaat over het gebruik van het klassieke implementatiemodel. Microsoft raadt aan dat de meeste nieuwe implementaties het Resource Manager-implementatiemodel gebruiken. 

Een eindpunt toegangsbeheerlijst (ACL) is een beveiligingsverbetering in beschikbaar voor uw Azure-implementatie. Een ACL biedt de mogelijkheid selectief toestaan of weigeren van verkeer voor het eindpunt van een virtuele machine. Deze filters gebruiken om functionaliteit voor pakket biedt een extra beveiligingslaag. U kunt de netwerk ACL's voor eindpunten alleen opgeven. U kunt een ACL voor een virtueel netwerk of een specifiek subnet die deel uitmaken van een virtueel netwerk niet opgeven. Het verdient aanbeveling om te gebruiken van netwerkbeveiligingsgroepen (nsg's) in plaats van ACL's, indien mogelijk. Wanneer u nsg's gebruikt, wordt de toegangsbeheerlijst eindpunt vervangen en niet meer afgedwongen. Zie voor meer informatie over nsg's, [overzicht van netwerkbeveiligingsgroepen](security-overview.md)

ACL's kunnen worden geconfigureerd met behulp van PowerShell of Azure portal. Zie voor meer informatie over het configureren van een netwerk ACL met behulp van PowerShell [toegangsbeheerlijsten voor eindpunten met behulp van PowerShell beheren](virtual-networks-acl-powershell.md). Zie voor meer informatie over het configureren van een netwerk ACL met behulp van de Azure-portal [over het instellen van eindpunten aan een virtuele machine](../virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

Met behulp van netwerk-ACL's, kunt u het volgende doen:

* Selectief toestaan of weigeren van inkomend verkeer op basis van een extern subnet IPv4-adresbereik naar een invoereindpunt voor een virtuele machine.
* Zwarte lijst IP-adressen
* Meerdere regels per eindpunt van de virtuele machine maken
* Gebruik regel bestellen om te controleren of de juiste set regels worden toegepast op een eindpunt van de opgegeven virtuele machine (laag naar hoog)
* Geef een ACL voor een specifieke extern subnet IPv4-adres.

Zie de [Azure-limieten](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#networking-limits) artikel voor ACL-limieten.

## <a name="how-acls-work"></a>De werking van ACL 's
Een ACL is een object dat een lijst met regels bevat. Wanneer u een ACL maken en op het eindpunt van een virtuele machine toepassen, duurt het filteren van netwerkpakketten plaats op de hostknooppunt van uw virtuele machine. Dit betekent dat het verkeer van externe IP-adressen wordt gefilterd op de host-knooppunt voor de overeenkomende ACL-regels in plaats van op de virtuele machine. Dit voorkomt dat uw virtuele machine de kostbare CPU-cycli besteed aan het filteren van netwerkpakketten.

Wanneer een virtuele machine wordt gemaakt, is een standaard-ACL opstellen om al het binnenkomende verkeer blokkeren. Echter, als een eindpunt is gemaakt voor (poort 3389), klikt u vervolgens de standaard ACL gewijzigd, zodat al het inkomende verkeer voor dit eindpunt. Inkomend verkeer van een extern subnet vervolgens naar dit eindpunt is toegestaan en geen firewall inrichting is vereist. Alle andere poorten worden geblokkeerd voor binnenkomend verkeer, tenzij de eindpunten voor deze poorten worden gemaakt. Standaard is uitgaand verkeer toegestaan.

**Voorbeeld van de standaard-ACL-tabel**

| **Regel #** | **Extern Subnet** | **Endpoint** | **Toestaan/weigeren** |
| --- | --- | --- | --- |
| 100 |0.0.0.0/0 |3389 |Toestaan |

## <a name="permit-and-deny"></a>Toestaan en weigeren
U kunt selectief toestaan of weigeren van het netwerkverkeer voor een invoereindpunt voor een virtuele machine door regels die specificeren 'toestaan' of 'niet toestaan' te maken. Het is belangrijk te weten dat standaard als een eindpunt is gemaakt, al het verkeer is toegestaan op het eindpunt. Om die reden is het belangrijk om te weten over het maken van regels voor toestaan/weigeren en plaats deze in de juiste volgorde van prioriteit als u wilt dat nauwkeurige controle over het netwerkverkeer dat u wilt toestaan dat het eindpunt van de virtuele machine te bereiken.

Punten om te overwegen:

1. **Er is geen ACL –** standaard als een eindpunt is gemaakt, toegestaan voor het eindpunt.
2. **Toestaan dat -** wanneer u een of meer 'toestaan' bereiken hebt toegevoegd, weigert u alle andere bereiken standaard. Alleen pakketten uit het toegestane IP-adresbereik is mogelijk om te communiceren met het eindpunt van de virtuele machine.
3. **DENY -** wanneer u een of meer "deny" bereiken hebt toegevoegd, verleent u alle andere bereiken van het verkeer standaard.
4. **Combinatie van toestaan en weigeren -** kunt u een combinatie van 'toestaan' en 'geweigerd' wanneer u wilt reserveren van een specifiek IP-adresbereik worden toegestaan of geweigerd.

## <a name="rules-and-rule-precedence"></a>Regels en de prioriteit van regels
Netwerk-ACL's kunnen worden ingesteld op specifieke virtuele machine-eindpunten. U kunt bijvoorbeeld een netwerk ACL voor een RDP-eindpunt gemaakt op een virtuele machine die wordt vergrendeld omlaag toegang voor bepaalde IP-adressen opgeven. De onderstaande tabel ziet u een manier om toegang te verlenen aan openbare virtuele IP-adressen (VIP's) van een bepaald bereik om toegang te verlenen voor RDP. Alle andere externe IP-adressen zijn geweigerd. We volgen een *laagste prioriteit* regel volgorde.

### <a name="multiple-rules"></a>Meerdere regels
In het onderstaande voorbeeld als u wilt toegang tot de RDP-eindpunt toestaan alleen vanaf twee openbare IPv4-adresbereiken (65.0.0.0/8 en 159.0.0.0/8), u kunt dit doen door op te geven twee *toestaan* regels. In dit geval, aangezien RDP standaard voor een virtuele machine is gemaakt, kunt u toegang tot de RDP-poort op basis van een extern subnet vergrendelen. Het volgende voorbeeld ziet een manier om toegang te verlenen aan openbare virtuele IP-adressen (VIP's) van een bepaald bereik om toegang te verlenen voor RDP. Alle andere externe IP-adressen zijn geweigerd. Dit werkt omdat netwerk ACL's kan worden ingesteld voor het eindpunt van een specifieke virtuele machine en de toegang is standaard niet toegestaan.

**Voorbeeld: meerdere regels**

| **Regel #** | **Extern Subnet** | **Endpoint** | **Toestaan/weigeren** |
| --- | --- | --- | --- |
| 100 |65.0.0.0/8 |3389 |Toestaan |
| 200 |159.0.0.0/8 |3389 |Toestaan |

### <a name="rule-order"></a>Regelvolgorde
Omdat meerdere regels kunnen worden opgegeven voor een eindpunt, moet er een manier om te organiseren van regels om te bepalen welke regel voorrang. De regelvolgorde geeft prioriteit aan. ACL's volgen het netwerk een *laagste prioriteit* regel volgorde. In het volgende voorbeeld wordt is het eindpunt op poort 80 selectief toegang verleend tot slechts bepaalde IP-adresbereiken. Voor het configureren van dit, hebben we een regel voor weigeren (regel \# 100) voor adressen in de ruimte 175.1.0.1/24. Een tweede regel wordt vervolgens opgegeven met de prioriteit van 200 dat toegang tot alle andere adressen onder 175.0.0.0/8 biedt.

**Voorbeeld: de prioriteit van regels**

| **Regel #** | **Extern Subnet** | **Endpoint** | **Toestaan/weigeren** |
| --- | --- | --- | --- |
| 100 |175.1.0.1/24 |80 |Weigeren |
| 200 |175.0.0.0/8 |80 |Toestaan |

## <a name="network-acls-and-load-balanced-sets"></a>Netwerk-ACL's en sets met gelijke taakverdeling
Netwerk-ACL's kunnen worden opgegeven voor een eindpunt met gelijke instellen. Als een ACL voor een set met gelijke is opgegeven, wordt de netwerk ACL toegepast op alle virtuele machines in deze set met gelijke. Bijvoorbeeld, als een gelijke set wordt gemaakt met 'Poort 80' en de set met gelijke bevat 3 VM's, het netwerk-ACL gemaakt op eindpunt 'Poort 80' van een die virtuele machine automatisch op de andere VM's toegepast wordt.

![Netwerk-ACL's en sets met gelijke taakverdeling](./media/virtual-networks-acl/IC674733.png)

## <a name="next-steps"></a>Volgende stappen
[Toegangsbeheerlijsten voor eindpunten met behulp van PowerShell beheren](virtual-networks-acl-powershell.md)

