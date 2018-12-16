---
title: bestand opnemen
description: bestand opnemen
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 10/17/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 267234cb9ecea1dc097f13739bf98ee11206ad06
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/15/2018
ms.locfileid: "53444065"
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