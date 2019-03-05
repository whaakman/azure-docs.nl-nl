---
title: Een toepassingsgateway maken met URL-pad gebaseerde regels voor doorsturen - Azure portal
description: Informatie over het maken van URL-pad op basis van routeringsregels voor een application gateway en de virtuele-machineschaalset met behulp van de Azure-portal.
services: application-gateway
author: vhorne
manager: jpconnock
tags: azure-resource-manager
ms.service: application-gateway
ms.topic: article
ms.workload: infrastructure-services
ms.date: 3/26/2018
ms.author: victorh
ms.openlocfilehash: 5d9f973926a46e61d83cba6fb52a543bddf26f99
ms.sourcegitcommit: 3f4ffc7477cff56a078c9640043836768f212a06
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/04/2019
ms.locfileid: "57315857"
---
# <a name="create-an-application-gateway-with-path-based-routing-rules-using-the-azure-portal"></a>Een toepassingsgateway maken met pad gebaseerde routering regels met behulp van de Azure portal

U kunt de Azure-portal gebruiken om te configureren [routeringsregels voor URL-pad gebaseerde](application-gateway-url-route-overview.md) bij het maken van een [toepassingsgateway](application-gateway-introduction.md). In deze zelfstudie maakt u back endpools met behulp van virtuele machines. Vervolgens maakt u regels voor doorsturen die zorg ervoor dat webverkeer aankomt op de juiste servers in de groepen.

In dit artikel leert u het volgende:

> [!div class="checklist"]
> * Een toepassingsgateway maken
> * Virtuele machines voor back-endservers maken
> * Back-endpools maken met de back-endservers
> * Maak een back-end-listener
> * Een regel op pad gebaseerde routering maken

![Voorbeeld van URL-routering](./media/application-gateway-create-url-route-portal/scenario.png)

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Aanmelden bij Azure Portal op [https://portal.azure.com](https://portal.azure.com)

## <a name="create-an-application-gateway"></a>Een toepassingsgateway maken

U hebt een virtueel netwerk nodig voor communicatie tussen de resources die u maakt. In dit voorbeeld worden twee subnetten gemaakt: één voor de toepassingsgateway en de andere voor de back-endservers. U kunt een virtueel netwerk maken op hetzelfde moment dat u de toepassingsgateway maakt.

1. Klik op **nieuw** gevonden in de linkerbovenhoek van Azure portal.
2. Selecteer **Netwerken** en vervolgens **Application Gateway** in de lijst Aanbevolen.
3. Voer deze waarden in voor de toepassingsgateway:

    - *myAppGateway* als de naam van de toepassingsgateway.
    - *myResourceGroupAG* als de nieuwe resourcegroep.

    ![Nieuwe toepassingsgateway maken](./media/application-gateway-create-url-route-portal/application-gateway-create.png)

4. Accepteer de standaardwaarden voor de overige instellingen en klik op **OK**.
5. Klik op **Een virtueel netwerk kiezen**, klik op **Nieuw maken** en voer deze waarden in voor het virtuele netwerk:

    - *myVnet* als de naam van het virtuele netwerk.
    - *10.0.0.0/16* als de adresruimte van het virtuele netwerk.
    - *myAGSubnet* als de naam van het subnet.
    - *10.0.0.0/24* als de adresruimte van het subnet.

    ![Virtueel netwerk maken](./media/application-gateway-create-url-route-portal/application-gateway-vnet.png)

6. Klik op **OK** om het virtuele netwerk en subnet te maken.
7. Klik op **een openbaar IP-adres kiezen**, klikt u op **nieuw**, en voer de naam van het openbare IP-adres. In dit voorbeeld is de naam van het openbare IP-adres *myAGPublicIPAddress*. Accepteer de standaardwaarden voor de overige instellingen en klik op **OK**.
8. Accepteer de standaardwaarden voor de configuratie van de Listener, laat u de Web application firewall is uitgeschakeld en klik vervolgens op **OK**.
9. Controleer de instellingen op de pagina Samenvatting en klik vervolgens op **OK** om de netwerkbronnen en de application gateway te maken. Het duurt enkele minuten voor de toepassingsgateway worden gemaakt, wacht totdat de implementatie is voltooid voordat u doorgaat met de volgende sectie.

### <a name="add-a-subnet"></a>Een subnet toevoegen

1. Klik in het menu links op **Alle resources** en vervolgens in de lijst met resources op **myVNet**.
2. Klik op **subnetten**, en klik vervolgens op **Subnet**.

    ![Subnet maken](./media/application-gateway-create-url-route-portal/application-gateway-subnet.png)

3. Voer *myBackendSubnet* in als naam van het subnet en klik op **OK**.

## <a name="create-virtual-machines"></a>Virtuele machines maken

In dit voorbeeld maakt u drie virtuele machines die worden gebruikt als back-endservers voor application gateway. U installeert ook IIS op de virtuele machines om te controleren of de toepassingsgateway is gemaakt.

1. Klik op **Nieuw**.
2. Klik op **Compute** en selecteer vervolgens **Windows Server 2016 Datacenter** in de lijst Aanbevolen.
3. Voer deze waarden in voor de virtuele machine:

    - *myVM1* als naam van de virtuele machine.
    - *azureuser* als gebruikersnaam van de beheerder.
    - *Azure123456!* als het wachtwoord.
    - Selecteer **Bestaande gebruiken** en selecteer *myResourceGroupAG*.

4. Klik op **OK**.
5. Selecteer **DS1_V2** als grootte van de virtuele machine en klik op **Selecteren**.
6. Zorg ervoor dat **myVNet** is geselecteerd voor het virtuele netwerk en dat het subnet **myBackendSubnet** is. 
7. Klik op **Uitgeschakeld** om diagnostische gegevens over opstarten uit te schakelen.
8. Klik op **OK**, controleer de instellingen op de overzichtspagina en klik op **Maken**.

### <a name="install-iis"></a>IIS installeren

1. Open de interactieve shell en zorg ervoor dat deze is ingesteld op **PowerShell**.

    ![Aangepaste extensie installeren](./media/application-gateway-create-url-route-portal/application-gateway-extension.png)

2. Voer de volgende opdracht uit om IIS op de virtuele machine te installeren: 

    ```azurepowershell-interactive
    $publicSettings = @{ "fileUris" = (,"https://raw.githubusercontent.com/Azure/azure-docs-powershell-samples/master/application-gateway/iis/appgatewayurl.ps1");  "commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File appgatewayurl.ps1" }
    Set-AzVMExtension `
      -ResourceGroupName myResourceGroupAG `
      -Location eastus `
      -ExtensionName IIS `
      -VMName myVM1 `
      -Publisher Microsoft.Compute `
      -ExtensionType CustomScriptExtension `
      -TypeHandlerVersion 1.4 `
      -Settings $publicSettings
    ```

3. Maak twee meer virtuele machines en IIS installeren met behulp van de stappen die u zojuist hebt voltooid. Typ de namen van *myVM2* en *myVM3* voor de namen en de waarden van VMName in Set-AzVMExtension.

## <a name="create-backend-pools-with-the-virtual-machines"></a>Back-endpools maken met de virtuele machines

1. Klik op **alle resources** en klik vervolgens op **myAppGateway**.
2. Klik op **Back-endpools**. Er is automatisch een standaardpool gemaakt met de toepassingsgateway. Klik op **appGatewayBackendPool**.
3. Klik op **toevoegen doel** om toe te voegen *myVM1* naar appGatewayBackendPool.

    ![Back-endservers toevoegen](./media/application-gateway-create-url-route-portal/application-gateway-backend.png)

4. Klik op **Opslaan**.
5. Klik op **back-endpools** en klik vervolgens op **toevoegen**.
6. Voer een naam in van *imagesBackendPool* en toe te voegen *myVM2* met behulp van **toevoegen doel**.
7. Klik op **OK**.
8. Klik op **toevoegen** opnieuw toe te voegen een andere back-endpool met de naam *videoBackendPool* en toe te voegen *myVM3* toe.

## <a name="create-a-backend-listener"></a>Maak een back-end-listener

1. Klik op **Listeners** en klik op **Basic**.
2. Voer *myBackendListener* voor de naam van de *myFrontendPort* voor de naam van de front-endpoort, en vervolgens *8080* als de poort voor de listener.
3. Klik op **OK**.

## <a name="create-a-path-based-routing-rule"></a>Een regel op pad gebaseerde routering maken

1. Klik op **regels** en klik vervolgens op **op pad gebaseerde**.
2. Voer *regel 2* voor de naam.
3. Voer *installatiekopieën* voor de naam van het eerste pad. Voer */images/* \* voor het pad. Selecteer **imagesBackendPool** voor de back-endpool.
4. Voer *Video* voor de naam van het tweede pad. Voer */video/* \* voor het pad. Selecteer **videoBackendPool** voor de back-endpool.

    ![Een pad gebaseerde regel maken](./media/application-gateway-create-url-route-portal/application-gateway-route-rule.png)

5. Klik op **OK**.

## <a name="test-the-application-gateway"></a>De toepassingsgateway testen

1. Klik op **alle resources**, en klik vervolgens op **myAGPublicIPAddress**.

    ![Registreer het openbare IP-adres van de toepassingsgateway](./media/application-gateway-create-url-route-portal/application-gateway-record-ag-address.png)

2. Kopieer het openbare IP-adres en plak het in de adresbalk van de browser. Bijvoorbeeld http:// http://40.121.222.19.

    ![Basis-URL testen in de toepassingsgateway](./media/application-gateway-create-url-route-portal/application-gateway-iistest.png)

3. Wijzig de URL naar http://&lt;ip-adres&gt;: 8080/images/test.htm, vervangen door &lt;ip-adres&gt; met uw IP-adres, en u ziet er ongeveer als volgt:

    ![Afbeeldingen-URL in toepassingsgateway testen](./media/application-gateway-create-url-route-portal/application-gateway-iistest-images.png)

4. Wijzig de URL naar http://&lt;ip-adres&gt;: 8080/video/test.htm, vervangen door &lt;ip-adres&gt; met uw IP-adres, en u ziet er ongeveer als volgt:

    ![Video-URL testen in de toepassingsgateway](./media/application-gateway-create-url-route-portal/application-gateway-iistest-video.png)

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd hoe u

> [!div class="checklist"]
> * Een toepassingsgateway maken
> * Virtuele machines voor back-endservers maken
> * Back-endpools maken met de back-endservers
> * Maak een back-end-listener
> * Een regel op pad gebaseerde routering maken

Voor meer informatie over Toepassingsgateways en de bijbehorende resources, gaat u naar de artikelen met procedures.
