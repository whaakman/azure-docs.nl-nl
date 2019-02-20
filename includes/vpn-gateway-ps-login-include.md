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
ms.openlocfilehash: 188f70d17531a3a7da12444ce5f1c97385ea33bc
ms.sourcegitcommit: 79038221c1d2172c0677e25a1e479e04f470c567
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/19/2019
ms.locfileid: "56418038"
---
Voordat u deze configuratie begint, moet u zich aanmelden bij uw Azure-account. De cmdlet vraagt u om de aanmeldingsreferenties voor uw Azure-account. Na het aanmelden, het instellingen van uw account gedownload zodat ze beschikbaar voor Azure PowerShell zijn. Zie [Using Windows PowerShell with Resource Manager](../articles/powershell-azure-resource-manager.md) (Windows PowerShell gebruiken met Resource Manager) voor meer informatie.

Als u wilt aanmelden, opent u de PowerShell-console met verhoogde bevoegdheden en verbinding maken met uw account. Gebruik het volgende voorbeeld als hulp bij het maken van de verbinding:

```powershell
Connect-AzAccount
```

Als u meerdere Azure-abonnementen hebt, controleert u de abonnementen voor het account.

```powershell
Get-AzSubscription
```

Geef het abonnement op dat u wilt gebruiken.

```powershell
Select-AzSubscription -SubscriptionName "Replace_with_your_subscription_name"
 ```
