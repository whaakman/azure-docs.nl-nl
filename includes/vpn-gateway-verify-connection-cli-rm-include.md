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
ms.openlocfilehash: 1c3103889bd11df45710cc0d6afaeeba022c9ace
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38763002"
---
U kunt controleren of uw verbinding is geslaagd met behulp van de opdracht [az network vpn-connection show](/cli/azure/network/vpn-connection#show). In het voorbeeld verwijst "--name" naar de naam van de verbinding die u wilt testen. Zolang het tot stand brengen van de verbinding bezig is, is de verbindingsstatus "Verbinding maken". Wanneer de verbinding tot stand is gebracht, verandert de status in "Verbonden".

```azurecli
az network vpn-connection show --name VNet1toSite2 --resource-group TestRG1
```