---
title: bestand opnemen
description: bestand opnemen
services: vpn-gateway
author: WenJason
ms.service: vpn-gateway
ms.topic: include
origin.date: 10/17/2018
ms.date: 03/04/2019
ms.author: v-jay
ms.custom: include file
ms.openlocfilehash: 49f48c2d0bf865cf8c8fde831e7a597f8701d213
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60456347"
---
U kunt controleren of uw verbinding is geslaagd met behulp van de cmdlet 'Get-AzVirtualNetworkGatewayConnection', met of zonder '-fouten opsporen in '. 

1. Gebruik het volgende cmdlet-voorbeeld om de waarden aan te passen aan uw eigen waarden. Selecteer A als dit wordt gevraagd om alles uit te voeren. In het voorbeeld verwijst "-Name" naar de naam van de verbinding die u wilt testen.

   ```azurepowershell
   Get-AzVirtualNetworkGatewayConnection -Name VNet1toSite1 -ResourceGroupName TestRG1
   ```
2. Bekijk de waarden nadat de cmdlet is voltooid. In het onderstaande voorbeeld wordt de verbindingsstatus weergegeven als Verbonden en ziet u inkomende en uitgaande bytes.
   
   ```
   "connectionStatus": "Connected",
   "ingressBytesTransferred": 33509044,
   "egressBytesTransferred": 4142431
   ```