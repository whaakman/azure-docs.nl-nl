---
title: De openbare preview van Azure-Firewall inschakelen
description: Azure PowerShell gebruiken om in te schakelen van de openbare preview van Azure-Firewall
author: vhorne
ms.service: firewall
services: firewall
ms.topic: article
ms.date: 7/11/2018
ms.author: victorh
ms.openlocfilehash: 263b16a419b5ff20a9b6d62860385f92c2a18f9c
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38991220"
---
# <a name="enable-the-azure-firewall-public-preview"></a>De openbare preview van Azure-Firewall inschakelen

[!INCLUDE [firewall-preview-notice](../../includes/firewall-preview-notice.md)]

## <a name="enable-using-azure-powershell"></a>Inschakelen met behulp van Azure PowerShell

Als u wilt de openbare preview van Azure-Firewall inschakelen, gebruikt de volgende Azure PowerShell-opdrachten:

```PowerShell
Register-AzureRmProviderFeature -FeatureName AllowRegionalGatewayManagerForSecureGateway -ProviderNamespace Microsoft.Network

Register-AzureRmProviderFeature -FeatureName AllowAzureFirewall -ProviderNamespace Microsoft.Network
```

Het duurt maximaal 30 minuten voor de functieregistratie te voltooien. U kunt de registratiestatus van uw controleren door het uitvoeren van de volgende Azure PowerShell-opdrachten:

```PowerShell

Get-AzureRmProviderFeature -FeatureName AllowRegionalGatewayManagerForSecureGateway -ProviderNamespace Microsoft.Network

Get-AzureRmProviderFeature -FeatureName AllowAzureFirewall -ProviderNamespace Microsoft.Network
```
Nadat de registratie voltooid is, voert u de volgende opdracht uit:

```PowerShell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
```

## <a name="next-steps"></a>Volgende stappen

- [Zelfstudie: Implementeren en configureren van de Firewall van Azure met behulp van de Azure portal](tutorial-firewall-deploy-portal.md)

