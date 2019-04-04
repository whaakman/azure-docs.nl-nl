---
title: Azure Application Gateway configureren met een privé front-end-IP-adres
description: Dit artikel bevat informatie over het configureren van Application Gateway met een privé front-end-IP-adres
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 02/26/2019
ms.author: absha
ms.openlocfilehash: cfc63349e20aa6dbef4e0d31e81842d325bd3ec6
ms.sourcegitcommit: 9f4eb5a3758f8a1a6a58c33c2806fa2986f702cb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58905534"
---
# <a name="configure-an-application-gateway-with-an-internal-load-balancer-ilb-endpoint"></a>Een toepassingsgateway configureren met een interne load balancer (ILB)-eindpunt

Azure Application Gateway kan worden geconfigureerd met een internetgerichte VIP of met een intern eindpunt dat niet wordt blootgesteld aan Internet (met behulp van een privé IP-adres voor de front-end-IP-adres), ook wel bekend als een interne load balancer (ILB)-eindpunt. Configureren van de gateway met behulp van een privé IP-adres voor front-end is handig voor interne line-of-business-toepassingen die niet worden blootgesteld aan Internet. Ook is dit handig als u services en lagen gebruikt in een toepassing met meerdere lagen die zich binnen een beveiligingsgrens bevinden, en als deze toepassing geen toegang heeft tot het internet, maar er wel round-robinbelastingverdeling, sessiepersistentie of SSL-beëindiging (Secure Sockets Layer) vereist is.

In dit artikel begeleidt u bij de stappen voor het configureren van een toepassingsgateway met een front-end privé IP-adres van de Azure-Portal.

In dit artikel leert u hoe u:

- Een privé front-end-IP-configuratie voor een toepassingsgateway maken
- Een toepassingsgateway maken met een privé front-end-IP-configuratie


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Meld u aan bij Azure portal op <https://portal.azure.com>

## <a name="create-an-application-gateway"></a>Een toepassingsgateway maken

Er is een virtueel netwerk nodig voor communicatie tussen de resources die u maakt. U kunt een nieuw virtueel netwerk maken of gebruik een bestaande resourcegroep. In dit voorbeeld maken we een nieuw virtueel netwerk. U kunt een virtueel netwerk maken op hetzelfde moment dat u de toepassingsgateway maakt. Application Gateway-instanties worden gemaakt in afzonderlijke subnetten. In dit voorbeeld maakt u twee subnetten: één voor de toepassingsgateway en één voor de back-endservers.

1. Klik op **nieuw** gevonden in de linkerbovenhoek van Azure portal.
2. Selecteer **Netwerken** en vervolgens **Application Gateway** in de lijst Aanbevolen.
3. Voer *myAppGateway* voor de naam van de toepassingsgateway en *myResourceGroupAG* voor de nieuwe resourcegroep.
4. Accepteer de standaardwaarden voor de overige instellingen en klik op **OK**.
5. Klik op **Een virtueel netwerk kiezen**, klik op **Nieuw maken** en voer deze waarden in voor het virtuele netwerk:
   - myVNet * - voor de naam van het virtuele netwerk.
   - 10.0.0.0/16* - voor de adresruimte van het virtuele netwerk.
   - *myAGSubnet* als de naam van het subnet.
   - *10.0.0.0/24* als de adresruimte van het subnet.  
     ![privé-frontendip-1](./media/configure-application-gateway-with-private-frontend-ip/private-frontendip-1.png)
6. Klik op **OK** om het virtuele netwerk en subnet te maken.
7. Kiest u de Frontend-IP-configuratie als persoonlijk en standaard is de toewijzing van een dynamische IP-adres. Het eerste beschikbare adres van het gekozen Subnet worden toegewezen als de front-end-IP-adres.
8. Als u wilt een privé IP-adres kiezen uit het adresbereik van het subnet (statische toewijzing), klikt u op het vak **kiest u een specifiek privé IP-adres** en geeft u het IP-adres.
   > [!NOTE]
   > Toegewezen, kan het type IP-adres (statische of dynamische) kan niet later worden gewijzigd.
9. Kies uw listenerconfiguratie voor het protocol en de poort, de configuratie van de WAF (indien nodig) en klik op OK.
    ![privé-frontendip-2](./media/configure-application-gateway-with-private-frontend-ip/private-frontendip-2.png)
10. Controleer de instellingen op de pagina Samenvatting en klik vervolgens op **OK** om de netwerkbronnen en de application gateway te maken. Het duurt enkele minuten voor de toepassingsgateway worden gemaakt, wacht totdat de implementatie is voltooid voordat u doorgaat met de volgende sectie.

## <a name="add-backend-pool"></a>Een back-endpool toevoegen

De back-endpool wordt gebruikt voor het routeren van aanvragen naar de back-endservers die behoeve van de aanvraag. Back-end kan bestaan uit NIC's, virtuele-machineschaalsets, openbare IP-adressen, namen van interne IP-adressen, de volledig gekwalificeerde domeinnaam (FQDN) en multitenant back-ends, zoals Azure Appservice. In dit voorbeeld gebruiken we virtuele machines als de doel-back-end. We kunnen bestaande virtuele machines gebruiken of nieuwe labels maken. In dit voorbeeld maken we twee virtuele machines die Azure als back-endservers voor application gateway gebruikt. Wij zullen u doet dit door:

1. 2 nieuwe virtuele machines maken *myVM* en *myVM2*moet worden gebruikt als back-endservers.
2. IIS installeren op de virtuele machines om te controleren of de application gateway is gemaakt.
3. De back-endservers toevoegen aan de back-endpool.

### <a name="create-a-virtual-machine"></a>Een virtuele machine maken

1. Klik op **Nieuw**.
2. Klik op **Compute** en selecteer vervolgens **Windows Server 2016 Datacenter** in de lijst Aanbevolen.
3. Voer deze waarden in voor de virtuele machine:
   - *myVM* als naam van de virtuele machine.
   - *azureuser* als gebruikersnaam van de beheerder.
   - *Azure123456!* als het wachtwoord.
   - Selecteer **Bestaande gebruiken** en selecteer *myResourceGroupAG*.
4. Klik op **OK**.
5. Selecteer **DS1_V2** voor de grootte van de virtuele machine en klik op **Selecteer**.
6. Zorg ervoor dat **myVNet** is geselecteerd voor het virtuele netwerk en dat het subnet **myBackendSubnet** is.
7. Klik op **Uitgeschakeld** om diagnostische gegevens over opstarten uit te schakelen.
8. Klik op **OK**, controleer de instellingen op de overzichtspagina en klik op **Maken**.

### <a name="install-iis"></a>IIS installeren

1. Open de interactieve shell en zorg ervoor dat deze is ingesteld op **PowerShell**.
    ![privé-frontendip-3](./media/configure-application-gateway-with-private-frontend-ip/private-frontendip-3.png)
2. Voer de volgende opdracht uit om IIS op de virtuele machine te installeren:

   ```azurepowershell
   Set-AzVMExtension `
   
     -ResourceGroupName myResourceGroupAG `
   
     -ExtensionName IIS `
   
     -VMName myVM `
   
     -Publisher Microsoft.Compute `
   
     -ExtensionType CustomScriptExtension `
   
     -TypeHandlerVersion 1.4 `
   
     -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server; powershell Add-Content -Path \"C:\\inetpub\\wwwroot\\Default.htm\" -Value $($env:computername)"}' -Location EastUS  ```



3. Create a second virtual machine and install IIS using the steps that you just finished. Enter myVM2 for its name and for VMName in Set-AzVMExtension.

### Add backend servers to backend pool

1. Click **All resources**, and then click **myAppGateway**.
2. Click **Backend pools**. A default pool was automatically created with the application gateway. Click **appGatewayBackendPool**.
3. Click **Add target** to add each virtual machine that you created to the backend pool.
   ![private-frontendip-4](./media/configure-application-gateway-with-private-frontend-ip/private-frontendip-4.png)
4. Click **Save.**

## Test the application gateway

1. Check your frontend IP that got assigned by clicking the **Frontend IP Configurations** blade in the portal.
    ![private-frontendip-5](./media/configure-application-gateway-with-private-frontend-ip/private-frontendip-5.png)
2. Copy the private IP address, and then paste it into the address bar of your browser of a VM in the same VNet or on-premises which has connectivity to this VNet and try to access the Application Gateway.

## Next steps

In this tutorial, you learned how to:

- Create a private frontend IP configuration for an Application Gateway
- Create an application gateway with private frontend IP configuration

If you want to monitor the health of your backend, see [Application Gateway Diagnostics](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics).
