---
title: Overzicht van de Firewall van Azure service-tags
description: In dit artikel wordt een overzicht van de Firewall van Azure service-codes.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 6/27/2019
ms.author: victorh
ms.openlocfilehash: d0ac36e415c056dffc9c75d00968ff74c2156e63
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67450175"
---
# <a name="azure-firewall-service-tags"></a>Azure Firewall servicetags

Een servicetag vertegenwoordigt een groep IP-adresvoorvoegsels die het maken van beveiligingsregel vereenvoudigt. U kunt niet uw eigen servicetag maken en ook niet opgeven welke IP-adressen zijn opgenomen in een tag. Microsoft beheert de adresvoorvoegsels die de servicetag omvat en werkt de servicetag automatisch bij wanneer adressen veranderen.

Tags van de service Azure Firewall kunnen worden gebruikt in het veld bestemming van netwerk-regels. U kunt deze gebruiken in plaats van specifieke IP-adressen.

## <a name="supported-service-tags"></a>Servicetags ondersteund

Zie [beveiligingsgroepen](../virtual-network/security-overview.md#service-tags) voor een lijst met servicetags die beschikbaar voor gebruik in Azure-netwerk firewallregels zijn.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Azure-Firewall-regels, [Azure firewallregel verwerking van logica](rule-processing.md).