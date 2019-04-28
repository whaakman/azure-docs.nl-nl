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
ms.openlocfilehash: 0e009354e66ab13cdb9fbc3cf9e4b37e904bdfd1
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60411778"
---
U kunt controleren of uw verbinding is geslaagd met behulp van de opdracht [az network vpn-connection show](/cli/azure/network/vpn-connection). In het voorbeeld verwijst "--name" naar de naam van de verbinding die u wilt testen. Zolang het tot stand brengen van de verbinding bezig is, is de verbindingsstatus "Verbinding maken". Wanneer de verbinding tot stand is gebracht, verandert de status in "Verbonden".

```azurecli
az network vpn-connection show --name VNet1toSite2 --resource-group TestRG1
```
