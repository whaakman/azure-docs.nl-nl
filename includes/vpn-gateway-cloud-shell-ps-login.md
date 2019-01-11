---
title: bestand opnemen
description: bestand opnemen
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 11/30/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: fee97b29f24d8bb4f50a2929c3ceb33af85a5e21
ms.sourcegitcommit: d4f728095cf52b109b3117be9059809c12b69e32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2019
ms.locfileid: "54201479"
---
Open de PowerShell-console met verhoogde bevoegdheden.



Als u Azure PowerShell lokaal uitvoert, moet u een verbinding maken met uw Azure-account. De *Connect-AzureRmAccount* cmdlet vraagt u om referenties. Na verificatie, het instellingen van uw account gedownload zodat ze beschikbaar voor Azure PowerShell zijn. Als u PowerShell niet lokaal worden uitgevoerd en worden in plaats daarvan met behulp van de Azure Cloud Shell 'Uitproberen' in de browser, kunt u deze eerste stap overslaan. U wordt automatisch verbinding maken met uw Azure-account.

```azurepowershell
Connect-AzureRmAccount
```

Als u meer dan één abonnement hebt, krijgt u een lijst met uw Azure-abonnementen.

```azurepowershell-interactive
Get-AzureRmSubscription
```

Geef het abonnement op dat u wilt gebruiken.

```azurepowershell-interactive
Select-AzureRmSubscription -SubscriptionName "Name of subscription"
```