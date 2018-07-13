---
title: Firewall van Azure met behulp van een sjabloon implementeren
description: Firewall van Azure met behulp van een sjabloon implementeren
services: firewall
author: vhorne
manager: jpconnock
ms.service: firewall
ms.topic: article
ms.date: 7/11/2018
ms.author: victorh
ms.openlocfilehash: 1a732e22d72c36afe11030e42bae529baa35df1a
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38991252"
---
# <a name="deploy-azure-firewall-using-a-template"></a>Firewall van Azure met behulp van een sjabloon implementeren

[!INCLUDE [firewall-preview-notice](../../includes/firewall-preview-notice.md)]

De voorbeelden in de Firewall van Azure-artikelen wordt ervan uitgegaan dat u al de openbare preview van Azure-Firewall hebt ingeschakeld. Zie voor meer informatie, [inschakelen van de openbare preview van Azure-Firewall](public-preview.md).

Deze sjabloon maakt een firewall en een test-netwerkomgeving. Het netwerk heeft een VNet, met drie subnetten: *AzureFirewallSubnet*, *ServersSubnet*, en een *JumpboxSubnet*. De ServersSubnet en JumpboxSubnet hebben een 2-core Windows Server erin.

De firewall is in de AzureFirewallSubnet en is geconfigureerd met een regelverzameling toepassing met een enkele regel waarmee toegang tot www.microsoft.com.

Een gebruiker gedefinieerde route wordt gemaakt die het netwerkverkeer van de ServersSubnet via de firewall waar de firewall-regels worden toegepast.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="template-location"></a>Locatie van de sjabloon

De sjabloon bevindt zich in:

[https://github.com/Azure/azure-quickstart-templates/tree/master/101-azurefirewall-sandbox](https://github.com/Azure/azure-quickstart-templates/tree/master/101-azurefirewall-sandbox)

Lees de introductie en wanneer u klaar bent om te implementeren, klikt u op **implementeren in Azure**.

## <a name="clean-up-resources"></a>Resources opschonen

Bekijk eerst de resources die zijn gemaakt met de firewall, en wanneer niet meer nodig hebt, kunt u de [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) opdracht om de resourcegroep, firewall en alle gerelateerde resources te verwijderen.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup
```
## <a name="next-steps"></a>Volgende stappen

Vervolgens kunt u de Firewall van Azure-logboeken bewaken:

- [Zelfstudie: Monitor Azure Firewall-logboeken](./tutorial-diagnostics.md)

