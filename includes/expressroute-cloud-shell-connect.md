---
title: bestand opnemen
description: bestand opnemen
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 02/01/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 1aca39a7ff162aa3c42fdb3ca5999c71091ec02e
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/23/2019
ms.locfileid: "66119396"
---
 Als u de Azure Cloud Shell gebruikt, aanmelden u bij uw Azure-account automatisch na het klikken op 'Uitproberen'. Voor lokaal aanmelden, opent u de PowerShell-console met verhoogde bevoegdheden en voer de cmdlet uit om verbinding te maken.

```azurepowershell
Connect-AzAccount
```

Als u meer dan één abonnement hebt, krijgt u een lijst met uw Azure-abonnementen.

```azurepowershell-interactive
Get-AzSubscription
```

Geef het abonnement op dat u wilt gebruiken.

```azurepowershell-interactive
Select-AzSubscription -SubscriptionName "Name of subscription"
```