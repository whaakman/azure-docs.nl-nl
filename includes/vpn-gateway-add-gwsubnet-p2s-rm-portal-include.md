---
title: bestand opnemen
description: bestand opnemen
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 10/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: bc1e2803a420b95e1abec0886733c5e42a8cfdb4
ms.sourcegitcommit: f6050791e910c22bd3c749c6d0f09b1ba8fccf0c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50026599"
---
1. Navigeer in de [portal](http://portal.azure.com) naar het virtuele netwerk van Resource Manager waarvoor u een gateway wilt maken.
2. Klik in de sectie **Instellingen** van de pagina VNet op **Subnetten** om de pagina **Subnetten** uit te vouwen.
3. Klik op de pagina **Subnetten** op **+Gatewaysubnet** om de pagina **Subnet toevoegen** te openen. 

  ![Het gatewaysubnet toevoegen](./media/vpn-gateway-add-gwsubnet-p2s-rm-portal-include/addgwsubnet.png "Het gatewaysubnet toevoegen")
4. Als **naam** voor het subnet wordt automatisch de waarde GatewaySubnet ingevuld. Deze waarde is vereist. Zonder deze waarde wordt het subnet niet in Azure als het gatewaysubnet herkend. Pas de automatisch ingevulde waarden voor de **Adressenreeks** aan overeenkomstig uw configuratievereisten. Configureer niet routetabel of Service-eindpunten.

  ![Het subnet toevoegen](./media/vpn-gateway-add-gwsubnet-p2s-rm-portal-include/p2sgwsub.png "Het subnet toevoegen")
5.  Klik op **OK** aan de onderkant van de pagina om het subnet te maken.