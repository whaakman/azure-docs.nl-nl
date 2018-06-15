---
title: FQDN-naam voor een virtuele machine van Windows in de Azure portal maken | Microsoft Docs
description: Informatie over het maken van een FQDN-naam of FQDN-naam voor een Resource Manager gebaseerde virtuele machine in de Azure-portal.
services: virtual-machines-windows
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: a2ae5887-76df-485e-ae19-0efd96df8600
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 12/13/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 8ebc1ef89b24a9aa21f39e5b05051c16351f08cd
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2017
ms.locfileid: "26707406"
---
# <a name="create-a-fully-qualified-domain-name-in-the-azure-portal-for-a-windows-vm"></a>Een volledig gekwalificeerde domeinnaam in de Azure portal maken voor een virtuele machine van Windows

Wanneer u een virtuele machine (VM) maakt in de [Azure-portal](https://portal.azure.com), een openbare IP-resource voor de virtuele machine automatisch wordt gemaakt. U kunt dit IP-adres gebruiken voor externe toegang tot de virtuele machine. Hoewel de portal geen maakt een [volledig gekwalificeerde domeinnaam](https://en.wikipedia.org/wiki/Fully_qualified_domain_name), of de FQDN-naam, kunt u een zodra de virtuele machine is gemaakt. In dit artikel ziet u de stappen voor het maken van een DNS-naam of FQDN-naam.

## <a name="create-a-fqdn"></a>Maken van een FQDN-naam
In dit artikel wordt ervan uitgegaan dat u al een virtuele machine hebt gemaakt. Indien nodig, kunt u [maken van een virtuele machine in de portal](quick-create-portal.md) of [met Azure PowerShell](quick-create-powershell.md). Volg deze stappen nadat uw virtuele machine actief is:

[!INCLUDE [virtual-machines-common-portal-create-fqdn](../../../includes/virtual-machines-common-portal-create-fqdn.md)]

U kunt nu op afstand verbinding met de virtuele machine met behulp van deze DNS-naam, zoals voor Remote Desktop Protocol (RDP).

## <a name="next-steps"></a>Volgende stappen
Nu dat uw virtuele machine een openbare IP-adres en DNS-naam heeft, kunt u algemene toepassingsframeworks of services, zoals IIS, SQL en SharePoint kunt implementeren.

U kunt ook meer informatie over [met Resource Manager](../../azure-resource-manager/resource-group-overview.md) voor tips over het bouwen van uw Azure-implementaties.

