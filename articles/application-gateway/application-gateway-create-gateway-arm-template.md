---
title: Een toepassingsgateway met Azure - sjablonen maken | Microsoft Docs
description: Op deze pagina staan instructies voor het maken van een toepassingsgateway met de Azure Resource Manager-sjabloon
documentationcenter: na
services: application-gateway
author: davidmu1
manager: timlt
editor: tysonn
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: davidmu
ms.openlocfilehash: 0aa16e9d7472d2d8c3c251e60a506a7f4223ac1d
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="create-an-application-gateway-by-using-the-azure-resource-manager-template"></a>Een toepassingsgateway maken met de Azure Resource Manager-sjabloon

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-create-gateway-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-gateway-arm.md)
> * [Azure Classic PowerShell](application-gateway-create-gateway.md)
> * [Azure Resource Manager-sjabloon](application-gateway-create-gateway-arm-template.md)
> * [Azure CLI](application-gateway-create-gateway-cli.md)

Azure Application Gateway is een load balancer in laag 7. De gateway biedt opties voor failovers en het routeren van HTTP-aanvragen tussen servers (on-premises en in de cloud). Application Gateway bevat veel ADC-functies (Application Delivery Controller), waaronder HTTP-taakverdeling, op cookies gebaseerde sessieaffiniteit, SSL-offload (Secure Sockets Layer), aangepaste statustests en ondersteuning voor meerdere locaties. Ga voor een volledige lijst met ondersteunde functies naar [Application Gateway-overzicht](application-gateway-introduction.md)

Dit artikel begeleidt u bij het downloaden en wijzigen van een bestaande [Azure Resource Manager-sjabloon](../azure-resource-manager/resource-group-authoring-templates.md) GitHub en implementeren van de sjabloon vanuit GitHub, PowerShell en de Azure CLI.

Als u de sjabloon rechtstreeks vanuit GitHub zonder wijzigingen wilt implementeren, gaat u als u wilt een sjabloon implementeren vanuit GitHub.

## <a name="scenario"></a>Scenario

In dit scenario:

* Een toepassingsgateway maken met web application firewall.
* Maakt u een virtueel netwerk met de naam VirtualNetwork1 met een gereserveerd CIDR-blok van 10.0.0.0/16.
* Maakt u een subnet met de naam Appgatewaysubnet dat gebruikmaakt van 10.0.0.0/28 als CIDR-blok.
* Stelt u twee eerder geconfigureerde back-end-IP-adressen in voor de webservers die load balancing moeten verzorgen voor het verkeer. In deze voorbeeldsjabloon worden de back-end-IP-adressen 10.0.1.10 en 10.0.1.11 gebruikt.

> [!NOTE]
> Deze instellingen zijn de parameters voor deze sjabloon. U kunt regels, de listener, SSL en andere opties in het bestand azuredeploy.json wijzigen voor het aanpassen van de sjabloon.

![Scenario](./media/application-gateway-create-gateway-arm-template/scenario.png)

## <a name="download-and-understand-the-azure-resource-manager-template"></a>De Azure Resource Manager-sjabloon downloaden en begrijpen

U kunt de bestaande Azure Resource Manager-sjabloon downloaden om een virtueel netwerk en twee subnets vanuit GitHub te maken. Vervolgens kunt u wijzigingen aanbrengen en de sjabloon opnieuw gebruiken. Volg hiervoor de volgende stappen:

1. Navigeer naar [toepassingsgateway maken met web application firewall ingeschakeld](https://github.com/Azure/azure-quickstart-templates/tree/master/101-application-gateway-waf).
1. Klik op **azuredeploy.json** en vervolgens op **RAW**.
1. Sla het bestand op in een lokale map op uw computer.
1. Als u bekend bent met Azure Resource Manager-sjablonen, kunt u doorgaan naar stap 7.
1. Open het bestand dat u hebt opgeslagen en bekijk de inhoud onder **parameters** in regel
1. Azure Resource Manager-sjabloonparameters bieden een tijdelijke aanduiding voor waarden die kunnen worden ingevuld tijdens de implementatie.

  | Parameter | Beschrijving |
  | --- | --- |
  | **subnetPrefix** |CIDR-blokkering voor het subnet voor de toepassingsgateway. |
  | **applicationGatewaySize** | Grootte van de toepassingsgateway.  WAF kunt alleen middelgrote en grote. |
  | **backendIpaddress1** |IP-adres van de eerste webserver. |
  | **backendIpaddress2** |IP-adres van de tweede webserver. |
  | **wafEnabled** | Instelling om te bepalen of WAF is ingeschakeld.|
  | **wafMode** | Modus van de web application firewall.  Beschikbare opties zijn **preventie** of **detectie**.|
  | **wafRuleSetType** | RuleSet-type voor WAF.  OWASP is momenteel de enige ondersteunde optie. |
  | **wafRuleSetVersion** |RuleSet-versie. OWASP CRS 2.2.9 en 3.0 zijn momenteel ondersteunde opties. |

1. Controleer de inhoud onder **resources** en Let op de volgende eigenschappen:

   * **type**. Het type resource dat door de sjabloon wordt aangemaakt. In dit geval wordt het type is `Microsoft.Network/applicationGateways`, die staat voor een toepassingsgateway.
   * **Naam**. Naam voor de resource. Let op het gebruik van `[parameters('applicationGatewayName')]`, wat betekent dat de naam is opgegeven als invoer door u of door een parameterbestand tijdens de implementatie.
   * **Eigenschappen**. Lijst met eigenschappen voor de resource. Deze sjabloon maakt tijdens het maken van de toepassingsgateway gebruik van het virtuele netwerk en het openbare IP-adres.

1. Ga terug naar [https://github.com/Azure/azure-quickstart-templates/blob/master/101-application-gateway-waf/](https://github.com/Azure/azure-quickstart-templates/blob/master/101-application-gateway-waf).
1. Klik op **azuredeploy-parameters.json**, en klik vervolgens op **RAW**.
1. Sla het bestand op in een lokale map op uw computer.
1. Open het bestand dat u hebt opgeslagen en bewerk de waarden voor de parameters. Gebruik de volgende waarden voor het implementeren van de toepassingsgateway die in ons scenario wordt beschreven.

    ```json
    {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "addressPrefix": {
            "value": "10.0.0.0/16"
            },
            "subnetPrefix": {
            "value": "10.0.0.0/28"
            },
            "applicationGatewaySize": {
            "value": "WAF_Medium"
            },
            "capacity": {
            "value": 2
            },
            "backendIpAddress1": {
            "value": "10.0.1.10"
            },
            "backendIpAddress2": {
            "value": "10.0.1.11"
            },
            "wafEnabled": {
            "value": true
            },
            "wafMode": {
            "value": "Detection"
            },
            "wafRuleSetType": {
            "value": "OWASP"
            },
            "wafRuleSetVersion": {
            "value": "3.0"
            }
        }
    }
    ```

1. Sla het bestand op. U kunt de JSON-sjabloon en parametersjabloon testen met online JSON-validatiehulpprogramma’s zoals [JSlint.com](http://www.jslint.com/).

## <a name="deploy-the-azure-resource-manager-template-by-using-powershell"></a>De Azure Resource Manager-sjabloon implementeren met PowerShell

Als u Azure PowerShell nog nooit hebt gebruikt, gaat u naar: [installeren en configureren van Azure PowerShell](/powershell/azure/overview) en volg de instructies om te melden bij Azure en uw abonnement te selecteren.

1. Meld u aan bij PowerShell

    ```powershell
    Login-AzureRmAccount
    ```

1. Controleer de abonnementen voor het account.

    ```powershell
    Get-AzureRmSubscription
    ```

    U wordt gevraagd om u te verifiëren met uw referenties.

1. Kies welk Azure-abonnement u wilt gebruiken.

    ```powershell
    Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
    ```

1. Indien nodig kunt u een resourcegroep maken met de cmdlet **New-AzureResourceGroup**. In het volgende voorbeeld maakt u een resourcegroep met de naam AppgatewayRG op de locatie VS - oost.

    ```powershell
    New-AzureRmResourceGroup -Name AppgatewayRG -Location "West US"
    ```

1. Voer de cmdlet **New-AzureRmResourceGroupDeployment** uit om het nieuwe virtuele netwerk te implementeren met de vorige sjabloon en parameterbestanden die u hebt gedownload en gewijzigd.
    
    ```powershell
    New-AzureRmResourceGroupDeployment -Name TestAppgatewayDeployment -ResourceGroupName AppgatewayRG `
    -TemplateFile C:\ARM\azuredeploy.json -TemplateParameterFile C:\ARM\azuredeploy-parameters.json
    ```

## <a name="deploy-the-azure-resource-manager-template-by-using-the-azure-cli"></a>De Azure Resource Manager-sjabloon implementeren met de Azure CLI

Volg de volgende stappen uit voor het implementeren van de Azure Resource Manager-sjabloon die u hebt gedownload met Azure CLI:

1. Als u Azure CLI nog nooit hebt gebruikt, raadpleegt u [De Azure CLI installeren en configureren](/cli/azure/install-azure-cli) en volgt u de instructies tot het punt waar u uw Azure-account en -abonnement moet selecteren.

1. Voer indien nodig de `az group create` opdracht voor het maken van een resourcegroep, zoals wordt weergegeven in het volgende codefragment. Hier ziet u de uitvoer van de opdracht. De lijst die na de uitvoer wordt weergegeven, beschrijft de gebruikte parameters. Zie voor meer informatie over resourcegroepen [Overzicht van Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).

    ```azurecli
    az group create --location westus --name appgatewayRG
    ```
    
    **-n (of --name)**. Naam voor de nieuwe resourcegroep. In ons scenario is dit *appgatewayRG*.
    
    **-l (of --location)**. De Azure-regio waar de nieuwe resourcegroep wordt gemaakt. In ons scenario heeft *westus*.

1. Voer de `az group deployment create` cmdlet voor het implementeren van het nieuwe virtuele netwerk met behulp van de sjabloon en de parameterbestanden die u hebt gedownload en gewijzigd in de vorige stap. De lijst die na de uitvoer wordt weergegeven, beschrijft de gebruikte parameters.

    ```azurecli
    az group deployment create --resource-group appgatewayRG --name TestAppgatewayDeployment --template-file azuredeploy.json --parameters @azuredeploy-parameters.json
    ```

## <a name="deploy-the-azure-resource-manager-template-by-using-click-to-deploy"></a>De Azure Resource Manager-sjabloon implementeren met click-to-deploy

U kunt de Azure Resource Manager-sjablonen ook gebruiken via click-to-deploy. Dit is een eenvoudige manier om sjablonen te gebruiken in de Azure Portal.

1. Ga naar [een toepassingsgateway maken met web application firewall](https://azure.microsoft.com/documentation/templates/101-application-gateway-waf/).

1. Klik op **Implementeren in Azure**.

    ![Implementeren in Azure](./media/application-gateway-create-gateway-arm-template/deploytoazure.png)
    
1. Vul de parameters voor de implementatiesjabloon in in de portal en klik op **OK**.

    ![Parameters](./media/application-gateway-create-gateway-arm-template/ibiza1.png)
    
1. Selecteer **ik ga akkoord met de voorwaarden en bepalingen hierboven** en klik op **aankoop**.

1. Klik op de blade voor aangepaste implementatie en klik op **Maken**.

## <a name="providing-certificate-data-to-resource-manager-templates"></a>Certificaat-gegevens voor Resource Manager-sjablonen

Als u SSL met een sjabloon, wordt het certificaat moet worden opgegeven in een base64-tekenreeks in plaats van wordt geüpload. Als u wilt converteren gebruik een .pfx- of .cer naar een base64-tekenreeks een van de volgende opdrachten. Het certificaat converteren de volgende opdrachten naar een base64-tekenreeks kan worden opgegeven voor de sjabloon. De verwachte uitvoer is een tekenreeks die kan worden opgeslagen in een variabele en geplakt in de sjabloon.

### <a name="macos"></a>macOS
```bash
cert=$( base64 <certificate path and name>.pfx )
echo $cert
```

### <a name="windows"></a>Windows
```powershell
[System.Convert]::ToBase64String([System.IO.File]::ReadAllBytes("<certificate path and name>.pfx"))
```

## <a name="delete-all-resources"></a>Alle resources verwijderen

Voor het verwijderen van alle resources in dit artikel hebt gemaakt, voert u een van de volgende stappen uit:

### <a name="powershell"></a>PowerShell

```powershell
Remove-AzureRmResourceGroup -Name appgatewayRG
```

### <a name="azure-cli"></a>Azure-CLI

```azurecli
az group delete --name appgatewayRG
```

## <a name="next-steps"></a>Volgende stappen

Ga naar: [Configure an application gateway for SSL offload](application-gateway-ssl.md) (Een toepassingsgateway voor SSL-offload configureren) als u SSL-offload wilt configureren.

Ga naar: [Create an application gateway with an internal load balancer (ILB)](application-gateway-ilb.md) (Een toepassingsgateway met een interne load balancer (ILB) maken) als u een toepassingsgateway wilt configureren voor gebruik met een interne load balancer.

Als u meer informatie wilt over de algemene opties voor taakverdeling, gaat u naar:

* [Azure Load Balancer](https://azure.microsoft.com/documentation/services/load-balancer/)
* [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)

