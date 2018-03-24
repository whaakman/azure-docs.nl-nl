---
title: bestand opnemen
description: bestand opnemen
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: a1fa4d58cefa82e70c036d697957254531042b9c
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
1. Navigeer in de [portal](http://portal.azure.com) naar het virtuele netwerk van Resource Manager waarvoor u een gateway wilt maken.
2. Klik in de sectie **Instellingen** van de pagina VNet op **Subnetten** om de pagina Subnetten uit te vouwen.
3. Klik op de pagina **Subnetten** op **+Gatewaysubnet** om de pagina **Subnet toevoegen** te openen.

  ![Het gatewaysubnet toevoegen](./media/vpn-gateway-add-gwsubnet-rm-portal-include/addgwsub.png "Het gatewaysubnet toevoegen")
4. Als **naam** voor het subnet wordt automatisch de waarde GatewaySubnet ingevuld. Deze waarde is vereist. Zonder deze waarde wordt het subnet niet in Azure als het gatewaysubnet herkend. Pas de automatisch ingevulde waarden voor **Adresbereik** aan, zodat deze overeenkomen met uw configuratie. Klik vervolgens onder aan de pagina op **OK** om het subnet te maken.

  ![Het subnet toevoegen](./media/vpn-gateway-add-gwsubnet-rm-portal-include/addsubnetgw.png "Het subnet toevoegen")