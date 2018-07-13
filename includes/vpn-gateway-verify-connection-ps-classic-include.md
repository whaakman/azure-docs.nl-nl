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
ms.openlocfilehash: 591da67e6411d0e859076f0a3c3c38afc1ebe1f5
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38765914"
---
U kunt controleren of uw verbinding is geslaagd met de cmdlet 'Get-AzureVNetConnection'.

1. Gebruik het volgende cmdlet-voorbeeld om de waarden aan te passen aan uw eigen waarden. De naam van het virtuele netwerk moet tussen aanhalingstekens als deze spaties bevat.

  ```powershell
  Get-AzureVNetConnection "Group ClassicRG ClassicVNet"
  ```
2. Bekijk de waarden nadat de cmdlet is voltooid. In het volgende voorbeeld wordt de status van de verbinding weergegeven als 'Verbonden' en kunt u inkomende en uitgaande bytes zien.

        ConnectivityState         : Connected
        EgressBytesTransferred    : 181664
        IngressBytesTransferred   : 182080
        LastConnectionEstablished : 1/7/2016 12:40:54 AM
        LastEventID               : 24401
        LastEventMessage          : The connectivity state for the local network site 'RMVNetLocal' changed from Connecting to
                                    Connected.
        LastEventTimeStamp        : 1/7/2016 12:40:54 AM
        LocalNetworkSiteName      : RMVNetLocal