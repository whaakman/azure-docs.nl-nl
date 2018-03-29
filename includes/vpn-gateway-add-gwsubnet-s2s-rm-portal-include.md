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
ms.openlocfilehash: 3e02242822283ec8597a7be217228cb95e748761
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
1. Navigeer in de portal naar het virtuele netwerk waarvoor u een virtuele netwerkgateway wilt maken.
2. Klik in de sectie **Instellingen** van de pagina VNet op **Subnetten** om de pagina Subnetten uit te vouwen.
3. Klik boven aan de pagina **Subnetten** op **+Gatewaysubnet** om de pagina **Subnet toevoegen** te openen.

  ![Het gatewaysubnet toevoegen](./media/vpn-gateway-add-gwsubnet-s2s-rm-portal-include/addgwsub.png "Het gatewaysubnet toevoegen")
4. Als **naam** voor het subnet wordt automatisch de waarde GatewaySubnet ingevuld. De waarde 'GatewaySubnet' is vereist omdat Azure het subnet anders niet als het gatewaysubnet kan herkennen. Pas de automatisch ingevulde waarden voor de **Adressenreeks** aan overeenkomstig uw configuratievereisten.

  ![Het gatewaysubnet toevoegen](./media/vpn-gateway-add-gwsubnet-s2s-rm-portal-include/addsub2.png "Het gatewaysubnet toevoegen")
5. Klik onder aan de pagina op **OK** om het subnet te maken.