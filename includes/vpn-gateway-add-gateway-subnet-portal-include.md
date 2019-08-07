---
title: bestand opnemen
description: bestand opnemen
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 08/02/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 52ab0413dffeee59cb9d34c6276a0c806a4d0322
ms.sourcegitcommit: 6cbf5cc35840a30a6b918cb3630af68f5a2beead
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/05/2019
ms.locfileid: "68780162"
---
Het gateway-subnet bevat de gereserveerde IP-adressen die door de Gateway Services van het virtuele netwerk worden gebruikt. Maak een gateway-subnet.

1. Navigeer in de portal naar het virtuele netwerk waarvoor u een virtuele netwerkgateway wilt maken.
2. Klik op de pagina virtueel netwerk op **subnetten** om de pagina **VNet1-subnetten** uit te vouwen.
3. Klik bovenaan op **+ Gateway-subnet** om de pagina **subnet toevoegen** te openen.

   ![Het gatewaysubnet toevoegen](./media/vpn-gateway-add-gateway-subnet-portal-include/gateway-subnet.png "Het gatewaysubnet toevoegen")
4. De **naam** voor het subnet wordt automatisch gevuld met de vereiste waarde ' GatewaySubnet '. Wijzig het automatisch ingevulde **adres bereik (CIDR-blok)** zodat dit overeenkomt met de volgende waarde:

   **Adres bereik (CIDR-blok)** : 10.1.255.0/27

   ![Het gatewaysubnet toevoegen](./media/vpn-gateway-add-gateway-subnet-portal-include/add-gateway-subnet1.png "Het gatewaysubnet toevoegen")
5. Laat de overige instellingen ongewijzigd als de standaard waarde **geen** of **0 is geselecteerd**. Klik vervolgens op **OK** om het gateway-subnet te maken.