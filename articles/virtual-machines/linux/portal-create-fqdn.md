---
title: FQDN-naam voor een Linux-VM in Azure portal maken | Microsoft Docs
description: Informatie over het maken van een FQDN-naam of FQDN-naam voor een Resource Manager gebaseerde virtuele machine in Azure portal.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: 2cd6c249-a737-4a0a-b5ba-e1c09e551b30
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 08/15/2018
ms.author: cynthn
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 54d1f77823b982ee8f49122c46b0a01cb27390f2
ms.sourcegitcommit: d2f2356d8fe7845860b6cf6b6545f2a5036a3dd6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/16/2018
ms.locfileid: "42054487"
---
# <a name="create-a-fully-qualified-domain-name-in-the-azure-portal-for-a-linux-vm"></a>Een volledig gekwalificeerde domeinnaam in Azure portal voor een Linux-VM maken

Wanneer u een virtuele machine (VM) maakt de [Azure-portal](https://portal.azure.com), een openbare IP-resource voor de virtuele machine wordt automatisch gemaakt. U kunt dit IP-adres gebruiken voor externe toegang tot de virtuele machine. Hoewel de portal geen maakt een [volledig gekwalificeerde domeinnaam](https://en.wikipedia.org/wiki/Fully_qualified_domain_name), of de FQDN-naam, kunt u een toevoegen nadat de virtuele machine is gemaakt. In dit artikel ziet u de stappen voor het maken van een DNS-naam of FQDN-naam.

## <a name="create-a-fqdn"></a>Een FQDN maken
In dit artikel wordt ervan uitgegaan dat u al een virtuele machine hebt gemaakt. Indien nodig, kunt u [een virtuele machine maken in de portal](quick-create-portal.md) of [met de Azure CLI](quick-create-cli.md). Volg deze stappen nadat uw virtuele machine actief is:

[!INCLUDE [virtual-machines-common-portal-create-fqdn](../../../includes/virtual-machines-common-portal-create-fqdn.md)]

U kunt nu op afstand verbinding met de virtuele machine met behulp van deze DNS-naam, zoals met `ssh azureuser@mydns.westus.cloudapp.azure.com`.

## <a name="next-steps"></a>Volgende stappen
Nu dat uw virtuele machine een openbaar IP-adres en DNS-naam heeft, kunt u implementeren veelvoorkomende toepassingsframeworks of -services zoals nginx, MongoDB, Docker, enzovoort.

U kunt ook meer informatie over [met Resource Manager](../../azure-resource-manager/resource-group-overview.md) voor tips over het bouwen van uw Azure-implementaties.

