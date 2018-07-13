---
title: bestand opnemen
description: bestand opnemen
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/29/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 508685954e23a357fa5fc48b0e89c5e7daedb5c6
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38765913"
---
U kunt met de cmdlet 'Get-AzureRmVirtualNetworkGatewayConnection', met of zonder -Debug, verifiëren of de verbinding is geslaagd. 

1. Gebruik het volgende cmdlet-voorbeeld om de waarden aan te passen aan uw eigen waarden. Selecteer A als dit wordt gevraagd om alles uit te voeren. In het voorbeeld verwijst "-Name" naar de naam van de verbinding die u wilt testen.

  ```azurepowershell-interactive
  Get-AzureRmVirtualNetworkGatewayConnection -Name VNet1toSite1 -ResourceGroupName TestRG1
  ```
2. Bekijk de waarden nadat de cmdlet is voltooid. In het onderstaande voorbeeld wordt de verbindingsstatus weergegeven als Verbonden en ziet u inkomende en uitgaande bytes.
   
  ```
  "connectionStatus": "Connected",
  "ingressBytesTransferred": 33509044,
  "egressBytesTransferred": 4142431
  ```