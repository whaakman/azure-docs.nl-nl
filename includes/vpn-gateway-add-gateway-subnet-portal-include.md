---
title: bestand opnemen
description: bestand opnemen
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 04/04/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 9132cf438cab518e20e6c2ddfdb7d0928753bd19
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66170086"
---
1. Navigeer in de portal naar het virtuele netwerk waarvoor u een virtuele netwerkgateway wilt maken.
2. Klik in de sectie **Instellingen** van de pagina VNet op **Subnetten** om de pagina Subnetten uit te vouwen.
3. Klik boven aan de pagina **Subnetten** op **+Gatewaysubnet** om de pagina **Subnet toevoegen** te openen.

   ![Het gatewaysubnet toevoegen](./media/vpn-gateway-add-gateway-subnet-portal-include/gateway-subnet.png "Het gatewaysubnet toevoegen")
  
4. Als **naam** voor het subnet wordt automatisch de waarde GatewaySubnet ingevuld. De waarde 'GatewaySubnet' is vereist omdat Azure het subnet anders niet als het gatewaysubnet kan herkennen. Pas de automatisch ingevulde waarden voor de **Adressenreeks** aan overeenkomstig uw configuratievereisten.

   ![Het gatewaysubnet toevoegen](./media/vpn-gateway-add-gateway-subnet-portal-include/add-gateway-subnet.png "Het gatewaysubnet toevoegen")
  
5. Klik onder aan de pagina op **OK** om het subnet te maken.
