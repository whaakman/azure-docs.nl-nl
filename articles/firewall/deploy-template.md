---
title: Firewall van Azure met behulp van een sjabloon implementeren
description: Firewall van Azure met behulp van een sjabloon implementeren
services: firewall
author: vhorne
manager: jpconnock
ms.service: firewall
ms.topic: article
ms.date: 12/01/2018
ms.author: victorh
ms.openlocfilehash: 86fdbbacf3e8064afe0aaaaebea1d6ef6c25f9d4
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52865821"
---
# <a name="deploy-azure-firewall-using-a-template"></a>Firewall van Azure met behulp van een sjabloon implementeren

De [sjabloon maken AzureFirewall sandbox-setup](https://github.com/Azure/azure-quickstart-templates/tree/master/101-azurefirewall-sandbox) maakt u een test-netwerkomgeving met een firewall. Het netwerk heeft één virtueel netwerk (VNet) met drie subnetten: *AzureFirewallSubnet*, *ServersSubnet*, en *JumpboxSubnet*. De *ServersSubnet* en *JumpboxSubnet* subnet elke hebben een één, twee core-Windows Server-machine.

De firewall wordt de *AzureFirewallSubnet* subnet, en heeft een regelverzameling toepassing met een enkele regel waarmee toegang tot *www.microsoft.com*.

Een door de gebruiker gedefinieerde route wijst netwerkverkeer van de *ServersSubnet* subnet via de firewall, waarbij de firewall-regels worden toegepast.

Zie voor meer informatie over Azure-Firewall [implementeren en configureren van de Firewall van Azure met behulp van de Azure-portal](tutorial-firewall-deploy-portal.md).

## <a name="use-the-template-to-deploy-azure-firewall"></a>Gebruik de sjabloon voor het implementeren van Azure-Firewall

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

**Installeren en de Firewall van Azure met behulp van de sjabloon te implementeren:**

1. Toegang tot de sjabloon op [ https://github.com/Azure/azure-quickstart-templates/tree/master/101-azurefirewall-sandbox ](https://github.com/Azure/azure-quickstart-templates/tree/master/101-azurefirewall-sandbox).
   
1. Lees de inleiding en wanneer u klaar bent om te implementeren, selecteer **implementeren in Azure**.
   
1. Meld u bij Azure portal indien nodig. 

1. In de portal op de **maken van een sandbox-installatie van AzureFirewall** pagina typt of selecteert u de volgende waarden:
   
   - **Resourcegroep**: Selecteer **nieuw**, typ een naam voor de resourcegroep en selecteer **OK**. 
   - **Virtuele-netwerknaam**: Typ een naam voor de nieuwe VNet. 
   - **Admin Username**: Typ een gebruikersnaam voor het beheerdersaccount van de gebruiker.
   - **Beheerderswachtwoord**: Typ een administrator-wachtwoord. 
   
1. Lees de voorwaarden en bepalingen, en selecteer vervolgens **ik ga akkoord met de voorwaarden en bepalingen bovenstaande**.
   
1. Selecteer **Aankoop**.
   
   Het duurt een paar minuten om de resources te maken. 
   
1. Verken de resources die zijn gemaakt met de firewall. 

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u deze niet meer nodig, kunt u de resourcegroep, firewall en alle gerelateerde resources verwijderen door het uitvoeren van de [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) PowerShell-opdracht. Verwijderen van een resourcegroep met de naam *MyResourceGroup*, uitvoeren: 

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name MyResourceGroup
```
Niet verwijdert de resourcegroep en de firewall nog, als u van plan bent om door te gaan naar de firewall voor bewaking van de zelfstudie. 

## <a name="next-steps"></a>Volgende stappen

Vervolgens kunt u de Firewall van Azure-logboeken bewaken:

> [!div class="nextstepaction"]
> [Zelfstudie: Azure Firewall-logboeken bewaken](./tutorial-diagnostics.md)
