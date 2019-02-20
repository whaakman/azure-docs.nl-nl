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
ms.openlocfilehash: deabef0c2c3540e515fe72a161710c95a20fa86f
ms.sourcegitcommit: 79038221c1d2172c0677e25a1e479e04f470c567
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/19/2019
ms.locfileid: "56418062"
---
Open de PowerShell-console met verhoogde bevoegdheden.



Als u Azure PowerShell lokaal uitvoert, moet u een verbinding maken met uw Azure-account. De *Connect AzAccount* cmdlet vraagt u om referenties. Na verificatie, het instellingen van uw account gedownload zodat ze beschikbaar voor Azure PowerShell zijn. Als u PowerShell niet lokaal worden uitgevoerd en worden in plaats daarvan met behulp van de Azure Cloud Shell 'Uitproberen' in de browser, kunt u deze eerste stap overslaan. U wordt automatisch verbinding maken met uw Azure-account.

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