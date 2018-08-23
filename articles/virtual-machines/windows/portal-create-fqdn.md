---
title: FQDN-naam voor een Windows-VM in Azure portal maken | Microsoft Docs
description: Informatie over het maken van een FQDN-naam of FQDN-naam voor een Resource Manager gebaseerde virtuele machine in Azure portal.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: a2ae5887-76df-485e-ae19-0efd96df8600
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 08/15/2018
ms.author: cynthn
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 885003863b8d5a5a81adc7f0310bbf2238edc68e
ms.sourcegitcommit: d2f2356d8fe7845860b6cf6b6545f2a5036a3dd6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/16/2018
ms.locfileid: "42060781"
---
# <a name="create-a-fully-qualified-domain-name-in-the-azure-portal-for-a-windows-vm"></a>Een volledig gekwalificeerde domeinnaam in Azure portal voor een Windows-VM maken

Wanneer u een virtuele machine (VM) maakt de [Azure-portal](https://portal.azure.com), een openbare IP-resource voor de virtuele machine wordt automatisch gemaakt. U kunt dit IP-adres gebruiken voor externe toegang tot de virtuele machine. Hoewel de portal geen maakt een [volledig gekwalificeerde domeinnaam](https://en.wikipedia.org/wiki/Fully_qualified_domain_name), of de FQDN-naam, kunt u maken nadat de virtuele machine is gemaakt. In dit artikel ziet u de stappen voor het maken van een DNS-naam of FQDN-naam.

## <a name="create-a-fqdn"></a>Een FQDN maken
In dit artikel wordt ervan uitgegaan dat u al een virtuele machine hebt gemaakt. Indien nodig, kunt u [een virtuele machine maken in de portal](quick-create-portal.md) of [met Azure PowerShell](quick-create-powershell.md). Volg deze stappen nadat uw virtuele machine actief is:

[!INCLUDE [virtual-machines-common-portal-create-fqdn](../../../includes/virtual-machines-common-portal-create-fqdn.md)]

U kunt nu op afstand verbinding maken met de virtuele machine met behulp van deze DNS-naam, zoals voor Remote Desktop Protocol (RDP).

## <a name="next-steps"></a>Volgende stappen
Nu dat uw virtuele machine een openbaar IP-adres en DNS-naam heeft, kunt u veelvoorkomende toepassingsframeworks of services, zoals IIS, SQL of SharePoint kunt implementeren.

U kunt ook meer informatie over [met Resource Manager](../../azure-resource-manager/resource-group-overview.md) voor tips over het bouwen van uw Azure-implementaties.

