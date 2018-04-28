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
ms.openlocfilehash: cab04a7eafbc21e0d26cd5a287f3dbee8d3d22b7
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2018
---
## <a name="scenario"></a>Scenario
Dit document helpt bij een implementatie met meerdere NIC's in virtuele machines in een specifiek scenario. In dit scenario hebt u een IaaS twee lagen werkbelasting gehost in Azure. Elke laag wordt geïmplementeerd in een eigen subnet in een virtueel netwerk (VNet). De front-laag bestaat uit verschillende webservers, gegroepeerd in een load balancer ingesteld voor hoge beschikbaarheid. De back-end-laag bestaat uit meerdere databaseservers. De database-servers zijn geïmplementeerd met twee NIC's, één voor toegang tot de database, de andere voor beheer. Het scenario omvat ook Netwerkbeveiligingsgroepen (nsg's) om te bepalen welk verkeer is toegestaan voor elk subnet en NIC in de implementatie. De volgende afbeelding ziet de basisarchitectuur van dit scenario:

![MultiNIC scenario](./media/virtual-network-deploy-multinic-scenario-include/Figure1.png)

