---
title: Voorbeeldsjablonen van Azure Policy | Microsoft Docs
description: Voorbeeldsjablonen van Azure Policy voor virtueel netwerk.
services: virtual-network
documentationcenter: ''
author: jimdial
manager: jeconnoc
editor: ''
tags: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: ''
ms.workload: ''
ms.date: 05/02/2018
ms.author: jdial
ms.custom: mvc
ms.openlocfilehash: 2b8766a5353b015030872176e9032034afb7cb9d
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/03/2018
---
# <a name="azure-policy-sample-templates-for-virtual-network"></a>Voorbeeldsjablonen van Azure Policy voor virtueel netwerk

De volgende tabel bevat links naar voorbeeldsjablonen van [Azure Policy](../azure-policy/azure-policy-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json). De voorbeelden staan in [opslagplaats met voorbeelden voor Azure Policy](https://github.com/Azure/azure-policy).

| | |
|---|---|
|**Netwerk**||
| [NSG X on every NIC](../azure-policy/scripts/nsg-on-nic.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (NSG X op elke NIC) | Vereist dat een specifieke netwerkbeveiligingsgroep wordt gebruikt met elke interface van een virtueel netwerk. U geeft de id op van de netwerkbeveiligingsgroep die u wilt gebruiken. |
| [NSG X on every subnet](../azure-policy/scripts/nsg-on-subnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (NSG X in elk subnet) | Vereist dat een specifieke netwerkbeveiligingsgroep wordt gebruikt met elk virtueel subnet. U geeft de id op van de netwerkbeveiligingsgroep die u wilt gebruiken. |
| [No route table](../azure-policy/scripts/no-user-def-route-table.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (Geen routetabel)  |Voorkomt dat virtuele netwerken worden geïmplementeerd met een routetabel. |
| [Use approved subnet for VM network interfaces](../azure-policy/scripts/use-approved-subnet-vm-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (Goedgekeurd subnet gebruiken voor VM-netwerkinterfaces) | Vereist dat netwerkinterfaces een goedgekeurd subnet gebruiken. U geeft de id op van het goedgekeurde subnet. |
| [Use approved vNet for VM network interfaces](../azure-policy/scripts/use-approved-vnet-vm-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (Goedgekeurd vNet gebruiken voor VM-netwerkinterfaces) | Vereist dat netwerkinterfaces een goedgekeurd virtueel netwerk gebruiken. U geeft de id op van het goedgekeurde virtuele netwerk. |
|**Controle**||
| [Audit diagnostic setting](../azure-policy/scripts/audit-diag-setting.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (Diagnostische instelling voor controleren) | Controleert of er geen diagnostische instellingen zijn ingeschakeld voor de opgegeven resourcetypen. U geeft een matrix van resourcetypen op waarvoor u wilt controleren of er diagnostische instellingen zijn ingeschakeld. |
|**Naam- en tekstconventies**||
| [Allow multiple name patterns](../azure-policy/scripts/allow-multiple-name-patterns.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (Meerdere naamgevingspatronen toestaan) | Toestaan dat verschillende naamgevingspatronen mogen worden gebruikt voor resources. |
| [Require like pattern](../azure-policy/scripts/enforce-like-pattern.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (Like-patroon vereisen) | Resourcenamen moeten voldoen aan de voorwaarde *like* voor een patroon. |
| [Require match pattern](../azure-policy/scripts/enforce-match-pattern.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (Match-patroon vereisen) | Resourcenamen moeten overeenkomen met een opgegeven naamgevingspatroon. |
| [Require tag match pattern](../azure-policy/scripts/enforce-tag-match-pattern.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (Tag-match-patroon vereisen) | Een tagwaarde moet overeenkomen met een tekstpatroon. |
|**Tags**||
| [Billing Tags Policy Initiative](../azure-policy/scripts/billing-tags-policy-init.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (Factureringstagbeleidinitiatief) | Vereist opgegeven tagwaarden voor kostenplaats en productnaam. Er wordt gebruikgemaakt van ingebouwde beleidsregels om vereiste tags toe te passen en af te dwingen. U geeft de vereiste waarden voor de tags op.  |
| [Enforce tag and its value on resource groups](../azure-policy/scripts/enforce-tag-rg.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (Tag met de bijbehorende waarde afdwingen voor resourcegroepen) | Vereist een tag en waarde voor een resourcegroep. U geeft de vereiste naam en waarde voor de tag op.  |
| [Enforce tag and its value](../azure-policy/scripts/enforce-tag-val.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (Tag en waarde afdwingen) | Vereist een opgegeven naam en waarde voor de tag. U geeft de naam en waarde op voor de tag die moet worden afgedwongen.  |
| [Apply tag and its default value](../azure-policy/scripts/apply-tag-def-val.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (Tag met standaardwaarde toepassen) | Hiermee voegt u de opgegeven naam en waarde voor een tag toe, als die tag niet is opgegeven. U geeft de naam en waarde op voor de tag die moet worden toegepast.  |
|**Algemeen**||
| [Allowed locations](../azure-policy/scripts/allowed-locs.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (Toegestane locaties) | Vereist dat alle resources worden geïmplementeerd naar de goedgekeurde locaties. U geeft een matrix van goedgekeurde locaties op.  |
| [Allowed resource types](../azure-policy/scripts/allowed-res-types.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (Toegestane resourcetypen) | Zorgt ervoor dat alleen goedgekeurde resourcetypen worden geïmplementeerd. U geeft een matrix met resourcetypen op die zijn toegestaan.  |
| [Not allowed resource types](../azure-policy/scripts/not-allowed-res-type.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (Niet-toegestane resourcetypen) | Voorkomt de implementatie van de opgegeven resourcetypen. U geeft een matrix met resourcetypen op die niet zijn toegestaan.  |