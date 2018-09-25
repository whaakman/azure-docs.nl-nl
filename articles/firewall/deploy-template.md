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
ms.openlocfilehash: d32e6e29c287d140c28206743e36dc025b26158b
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46991331"
---
# <a name="deploy-azure-firewall-using-a-template"></a>Firewall van Azure met behulp van een sjabloon implementeren

Deze sjabloon maakt een firewall en een test-netwerkomgeving. Het netwerk heeft een VNet, met drie subnetten: *AzureFirewallSubnet*, *ServersSubnet*, en een *JumpboxSubnet*. Het ServersSubnet en het JumpboxSubnet hebben elk één Windows Server met twee kernen.

De firewall bevindt zich in het AzureFirewallSubnet en is geconfigureerd met een toepassingsregelverzameling met één regel die toegang geeft tot www.microsoft.com.

Er wordt een gebruikergedefinieerde route gemaakt die het netwerkverkeer vanuit het ServersSubnet door de firewall leidt, waar de firewallregels worden toegepast.

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

- [Zelfstudie: Azure Firewall-logboeken bewaken](./tutorial-diagnostics.md)

