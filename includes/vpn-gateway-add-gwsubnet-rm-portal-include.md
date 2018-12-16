---
title: bestand opnemen
description: bestand opnemen
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 11/30/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: eb2555cdc163ee0f88149248b9e7d83a51d34a1b
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/15/2018
ms.locfileid: "53444077"
---
1. In de [Azure-portal](http://portal.azure.com), selecteer het virtuele netwerk van Resource Manager waarvoor u wenst te maken van een virtuele netwerkgateway.

2. In de **instellingen** sectie van uw virtuele netwerk weergeeft, schakelt **subnetten** om uit te breiden de **subnetten** pagina.

3. Op de **subnetten** weergeeft, schakelt **gatewaysubnet** openen de **subnet toevoegen** pagina.

   ![Het gatewaysubnet toevoegen](./media/vpn-gateway-add-gwsubnet-rm-portal-include/addgwsub.png "Het gatewaysubnet toevoegen")

4. De **naam** voor uw subnet automatisch autofilled met de waarde is *GatewaySubnet*. Deze waarde is vereist voor Azure voor het herkennen van het subnet als het gatewaysubnet. Pas de autofilled **adresbereik** vervolgens selecteert u waarden die moeten worden overeenkomstig uw configuratievereisten **OK** om het subnet te maken.

   ![Het subnet toevoegen](./media/vpn-gateway-add-gwsubnet-rm-portal-include/addsubnetgw.png "Het subnet toevoegen")