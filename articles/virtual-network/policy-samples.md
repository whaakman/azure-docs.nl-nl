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
ms.openlocfilehash: b0b3b65ee2360a5cec32235f2ee5bf8d4839cc8b
ms.sourcegitcommit: fcb674cc4e43ac5e4583e0098d06af7b398bd9a9
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/18/2019
ms.locfileid: "56342074"
---
# <a name="azure-policy-sample-templates-for-virtual-network"></a>Voorbeeldsjablonen van Azure Policy voor virtueel netwerk

De volgende tabel bevat koppelingen naar voorbeelden voor [Azure Policy](../governance/policy/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json). De voorbeelden staan in [opslagplaats met voorbeelden voor Azure Policy](https://github.com/Azure/azure-policy).

| | |
|---|---|
|**Netwerk**||
| [NSG X on every NIC](../governance/policy/samples/nsg-on-nic.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (NSG X op elke NIC) | Vereist dat een specifieke netwerkbeveiligingsgroep wordt gebruikt met elke interface van een virtueel netwerk. U geeft de id op van de netwerkbeveiligingsgroep die u wilt gebruiken. |
| [NSG X on every subnet](../governance/policy/samples/nsg-on-subnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (NSG X in elk subnet) | Vereist dat een specifieke netwerkbeveiligingsgroep wordt gebruikt met elk virtueel subnet. U geeft de id op van de netwerkbeveiligingsgroep die u wilt gebruiken. |
| [No route table](../governance/policy/samples/no-user-defined-route-table.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (Geen routetabel)  |Voorkomt dat virtuele netwerken worden geïmplementeerd met een routetabel. |
| [Use approved subnet for VM network interfaces](../governance/policy/samples/use-approved-subnet-vm-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (Goedgekeurd subnet gebruiken voor VM-netwerkinterfaces) | Vereist dat netwerkinterfaces een goedgekeurd subnet gebruiken. U geeft de id op van het goedgekeurde subnet. |
| [Use approved vNet for VM network interfaces](../governance/policy/samples/use-approved-vnet-vm-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (Goedgekeurd vNet gebruiken voor VM-netwerkinterfaces) | Vereist dat netwerkinterfaces een goedgekeurd virtueel netwerk gebruiken. U geeft de id op van het goedgekeurde virtuele netwerk. |
|**Controle**||
| [Audit diagnostic setting](../governance/policy/samples/audit-diagnostic-setting.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (Diagnostische instelling voor controleren) | Controleert of er geen diagnostische instellingen zijn ingeschakeld voor de opgegeven resourcetypen. U geeft een matrix van resourcetypen op waarvoor u wilt controleren of er diagnostische instellingen zijn ingeschakeld. |
|**Naam- en tekstconventies**||
| [Allow multiple name patterns](../governance/policy/samples/allow-multiple-name-patterns.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (Meerdere naamgevingspatronen toestaan) | Toestaan dat verschillende naamgevingspatronen mogen worden gebruikt voor resources. |
| [Require like pattern](../governance/policy/samples/enforce-like-pattern.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (Like-patroon vereisen) | Resourcenamen moeten voldoen aan de voorwaarde *like* voor een patroon. |
| [Require match pattern](../governance/policy/samples/enforce-match-pattern.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (Match-patroon vereisen) | Resourcenamen moeten overeenkomen met een opgegeven naamgevingspatroon. |
| [Require tag match pattern](../governance/policy/samples/enforce-tag-match-pattern.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (Tag-match-patroon vereisen) | Een tagwaarde moet overeenkomen met een tekstpatroon. |
|**Tags**||
| [Billing Tags Policy Initiative](../governance/policy/samples/billing-tags-policy-initiative.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (Factureringstagbeleidinitiatief) | Vereist opgegeven tagwaarden voor kostenplaats en productnaam. Er wordt gebruikgemaakt van ingebouwde beleidsregels om vereiste tags toe te passen en af te dwingen. U geeft de vereiste waarden voor de tags op.  |
| [Enforce tag and its value on resource groups](../governance/policy/samples/enforce-tag-on-resource-groups.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (Tag met de bijbehorende waarde afdwingen voor resourcegroepen) | Vereist een tag en waarde voor een resourcegroep. U geeft de vereiste naam en waarde voor de tag op.  |
| [Enforce tag and its value](../governance/policy/samples/enforce-tag-value.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (Tag en waarde afdwingen) | Vereist een opgegeven naam en waarde voor de tag. U geeft de naam en waarde op voor de tag die moet worden afgedwongen.  |
| [Apply tag and its default value](../governance/policy/samples/apply-tag-default-value.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (Tag met standaardwaarde toepassen) | Hiermee voegt u de opgegeven naam en waarde voor een tag toe, als die tag niet is opgegeven. U geeft de naam en waarde op voor de tag die moet worden toegepast.  |
|**Algemeen**||
| [Allowed locations](../governance/policy/samples/allowed-locations.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (Toegestane locaties) | Vereist dat alle resources worden geïmplementeerd naar de goedgekeurde locaties. U geeft een matrix van goedgekeurde locaties op.  |
| [Allowed resource types](../governance/policy/samples/allowed-resource-types.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (Toegestane resourcetypen) | Zorgt ervoor dat alleen goedgekeurde resourcetypen worden geïmplementeerd. U geeft een matrix met resourcetypen op die zijn toegestaan.  |
| [Not allowed resource types](../governance/policy/samples/not-allowed-resource-types.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (Niet-toegestane resourcetypen) | Voorkomt de implementatie van de opgegeven resourcetypen. U geeft een matrix met resourcetypen op die niet zijn toegestaan.  |