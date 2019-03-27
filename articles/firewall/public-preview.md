---
title: De openbare preview van Azure-Firewall inschakelen
description: Azure PowerShell gebruiken om in te schakelen van de openbare preview van Azure-Firewall
author: vhorne
ms.service: firewall
services: firewall
ms.topic: article
ms.date: 7/11/2018
ms.author: victorh
ms.openlocfilehash: fe1b8f9d56b0f4faa0baa25463b2aa29a59715cb
ms.sourcegitcommit: f24fdd1ab23927c73595c960d8a26a74e1d12f5d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/27/2019
ms.locfileid: "58499587"
---
# <a name="enable-the-azure-firewall-public-preview"></a>De openbare preview van Azure-Firewall inschakelen

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [firewall-preview-notice](../../includes/firewall-preview-notice.md)]

## <a name="enable-using-azure-powershell"></a>Inschakelen met behulp van Azure PowerShell

Als u wilt de openbare preview van Azure-Firewall inschakelen, gebruikt de volgende Azure PowerShell-opdrachten:

```PowerShell
Register-AzProviderFeature -FeatureName AllowRegionalGatewayManagerForSecureGateway -ProviderNamespace Microsoft.Network
Register-AzProviderFeature -FeatureName AllowAzureFirewall -ProviderNamespace Microsoft.Network
```

Het duurt maximaal 30 minuten voor de functieregistratie te voltooien. U kunt de registratiestatus van uw controleren door het uitvoeren van de volgende Azure PowerShell-opdrachten:

```powershell

Get-AzProviderFeature -FeatureName AllowRegionalGatewayManagerForSecureGateway -ProviderNamespace Microsoft.Network

Get-AzProviderFeature -FeatureName AllowAzureFirewall -ProviderNamespace Microsoft.Network
```
Nadat de registratie voltooid is, voert u de volgende opdracht uit:

```powershell
Register-AzResourceProvider -ProviderNamespace Microsoft.Network
```

## <a name="next-steps"></a>Volgende stappen

- [Zelfstudie: Azure Firewall implementeren en configureren met Azure Portal](tutorial-firewall-deploy-portal.md)

