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
ms.openlocfilehash: a8dec00373d991a7aeaf11f1a4d95463ab71d9a2
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
1. Navigeer in de [portal](http://portal.azure.com) naar het virtuele netwerk van Resource Manager waarvoor u een gateway wilt maken.
2. Klik in de sectie **Instellingen** van de pagina VNet op **Subnetten** om de pagina **Subnetten** uit te vouwen.
3. Klik op de pagina **Subnetten** op **+Gatewaysubnet** om de pagina **Subnet toevoegen** te openen. 

  ![Het gatewaysubnet toevoegen](./media/vpn-gateway-add-gwsubnet-p2s-rm-portal-include/addgwsubnet.png "Het gatewaysubnet toevoegen")
4. Als **naam** voor het subnet wordt automatisch de waarde GatewaySubnet ingevuld. Deze waarde is vereist. Zonder deze waarde wordt het subnet niet in Azure als het gatewaysubnet herkend. Pas de automatisch ingevulde waarden voor **Adresbereik** aan, zodat deze overeenkomen met uw configuratie. Klik vervolgens onder aan de pagina op **OK** om het subnet te maken.

  ![Het subnet toevoegen](./media/vpn-gateway-add-gwsubnet-p2s-rm-portal-include/p2sgwsub.png "Het subnet toevoegen")