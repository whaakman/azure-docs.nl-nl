---
title: bestand opnemen
description: bestand opnemen
services: virtual-network
author: genlin
ms.service: virtual-network
ms.topic: include
ms.date: 04/13/2018
ms.author: genli
ms.custom: include file
ms.openlocfilehash: d20ef44fd5c117e4e3a568542bb022c451ac23fc
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/23/2019
ms.locfileid: "66170824"
---
## <a name="scenario"></a>Scenario
Dit document begeleidt bij een implementatie met meerdere NIC's in virtuele machines in een specifiek scenario. In dit scenario hebt u een IaaS twee lagen werkbelasting die wordt gehost in Azure. Elke laag wordt geïmplementeerd in een eigen subnet in een virtueel netwerk (VNet). De front-endlaag bestaat uit meerdere webservers, gegroepeerd in een load balancer-set voor hoge beschikbaarheid. De back-end-laag bestaat uit verschillende database-servers. De database-servers worden geïmplementeerd met twee NIC's, één voor toegang tot de database, de andere voor beheer. Het scenario omvat ook Netwerkbeveiligingsgroepen (nsg's) om te bepalen welk verkeer is toegestaan op elk subnet en NIC in de implementatie. De volgende afbeelding ziet u de basisarchitectuur van dit scenario:

![MultiNIC scenario](./media/virtual-network-deploy-multinic-scenario-include/Figure1.png)

