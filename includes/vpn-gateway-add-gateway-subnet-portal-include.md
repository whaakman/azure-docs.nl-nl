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
ms.openlocfilehash: a4101f3bfe83859eea525370b5eebcaa6e193a2d
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44169100"
---
1. Navigeer in de portal naar het virtuele netwerk waarvoor u een virtuele netwerkgateway wilt maken.
2. Klik in de sectie **Instellingen** van de pagina VNet op **Subnetten** om de pagina Subnetten uit te vouwen.
3. Klik boven aan de pagina **Subnetten** op **+Gatewaysubnet** om de pagina **Subnet toevoegen** te openen.

  ![Het gatewaysubnet toevoegen](./media/vpn-gateway-add-gateway-subnet-portal-include/gateway-subnet.png "Het gatewaysubnet toevoegen")
  
4. Als **naam** voor het subnet wordt automatisch de waarde GatewaySubnet ingevuld. De waarde 'GatewaySubnet' is vereist omdat Azure het subnet anders niet als het gatewaysubnet kan herkennen. Pas de automatisch ingevulde waarden voor de **Adressenreeks** aan overeenkomstig uw configuratievereisten.

  ![Het gatewaysubnet toevoegen](./media/vpn-gateway-add-gateway-subnet-portal-include/add-gateway-subnet.png "Het gatewaysubnet toevoegen")
  
5. Klik onder aan de pagina op **OK** om het subnet te maken.
