---
title: 'Zelfstudie: een toepassingsgateway maken met URL-pad gebaseerde regels voor doorsturen - Azure portal'
description: In deze zelfstudie leert u over het maken van URL-pad op basis van routeringsregels voor een application gateway en de virtuele-machineschaalset met behulp van de Azure-portal.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: tutorial
ms.date: 4/18/2019
ms.author: victorh
ms.openlocfilehash: 5307f7674635fd33241e1faba9bb0b7c0432d10b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "66134746"
---
# <a name="tutorial-create-an-application-gateway-with-path-based-routing-rules-using-the-azure-portal"></a>Zelfstudie: Een toepassingsgateway maken met pad gebaseerde routering regels met behulp van de Azure portal

U kunt de Azure-portal gebruiken om te configureren [routeringsregels voor URL-pad gebaseerde](url-route-overview.md) bij het maken van een [toepassingsgateway](overview.md). In deze zelfstudie maakt u back endpools met behulp van virtuele machines. Vervolgens maakt u regels voor doorsturen die zorg ervoor dat webverkeer aankomt op de juiste servers in de groepen.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een toepassingsgateway maken
> * Virtuele machines voor back-endservers maken
> * Back-endpools maken met de back-endservers
> * Maak een back-end-listener
> * Een regel op pad gebaseerde routering maken

![Voorbeeld van URL-routering](./media/create-url-route-portal/scenario.png)

Als u liever, u kunt deze zelfstudie volgen met behulp [Azure CLI](tutorial-url-route-cli.md) of [Azure PowerShell](tutorial-url-route-powershell.md).

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij de Azure Portal op [https://portal.azure.com](https://portal.azure.com).

## <a name="create-an-application-gateway"></a>Een toepassingsgateway maken

Een virtueel netwerk is vereist voor communicatie tussen de bronnen die u maakt. In dit voorbeeld worden twee subnetten gemaakt: één voor de toepassingsgateway en de andere voor de back-endservers. U kunt een virtueel netwerk maken op hetzelfde moment als die u de toepassingsgateway maakt.

1. Selecteer **nieuw** gevonden in de linkerbovenhoek van Azure portal.
2. Selecteer **Netwerken** en vervolgens **Application Gateway** in de lijst Aanbevolen.
3. Voer deze waarden in voor de toepassingsgateway:

   - *myAppGateway* als de naam van de toepassingsgateway.
   - *myResourceGroupAG* als de nieuwe resourcegroep.

     ![Nieuwe toepassingsgateway maken](./media/create-url-route-portal/application-gateway-create.png)

4. Accepteer de standaardwaarden voor de overige instellingen en selecteer **OK**.
5. Selecteer **een virtueel netwerk kiezen**, selecteer **nieuw**, en voer vervolgens deze waarden voor het virtuele netwerk:

   - *myVnet* als de naam van het virtuele netwerk.
   - *10.0.0.0/16* als de adresruimte van het virtuele netwerk.
   - *myAGSubnet* als de naam van het subnet.
   - *10.0.0.0/24* als de adresruimte van het subnet.

     ![Virtueel netwerk maken](./media/create-url-route-portal/application-gateway-vnet.png)

6. Selecteer **OK** te maken van het virtuele netwerk en subnet.
7. Selecteer **een openbaar IP-adres kiezen**, selecteer **nieuw**, en voer de naam van het openbare IP-adres. In dit voorbeeld is de naam van het openbare IP-adres *myAGPublicIPAddress*. Accepteer de standaardwaarden voor de overige instellingen en selecteer **OK**.
8. Accepteer de standaardwaarden voor de configuratie van de Listener, laat u de Web application firewall is uitgeschakeld en selecteer vervolgens **OK**.
9. Controleer de instellingen op de pagina Samenvatting en selecteer vervolgens **OK** om de netwerkbronnen en de application gateway te maken. Het duurt enkele minuten voor de toepassingsgateway worden gemaakt, wacht totdat de implementatie is voltooid voordat u doorgaat met de volgende sectie.

### <a name="add-a-subnet"></a>Een subnet toevoegen

1. Selecteer **alle resources** in het menu links, en selecteer vervolgens **myVNet** in de lijst met resources.
2. Selecteer **subnetten**, en selecteer vervolgens **Subnet**.

    ![Subnet maken](./media/create-url-route-portal/application-gateway-subnet.png)

3. Voer *myBackendSubnet* voor de naam van het subnet en selecteer vervolgens **OK**.

## <a name="create-virtual-machines"></a>Virtuele machines maken

In dit voorbeeld maakt u drie virtuele machines die worden gebruikt als back-endservers voor application gateway. U kunt ook IIS installeren op de virtuele machines om te controleren of dat de application gateway is gemaakt.

1. Selecteer **Nieuw**.
2. Selecteer **Compute** en selecteer vervolgens **Windows Server 2016 Datacenter** in de lijst Aanbevolen.
3. Voer deze waarden in voor de virtuele machine:

    - *myVM1* als naam van de virtuele machine.
    - *azureuser* als gebruikersnaam van de beheerder.
    - *Azure123456!* als het wachtwoord.
    - Selecteer **Bestaande gebruiken** en selecteer *myResourceGroupAG*.

4. Selecteer **OK**.
5. Selecteer **DS1_V2** voor de grootte van de virtuele machine en selecteer **Selecteer**.
6. Zorg ervoor dat **myVNet** is geselecteerd voor het virtuele netwerk en dat het subnet **myBackendSubnet** is. 
7. Selecteer **Uitgeschakeld** om diagnostische gegevens over opstarten uit te schakelen.
8. Selecteer **OK**, controleer de instellingen op de overzichtspagina en selecteer **Maken**.

### <a name="install-iis"></a>IIS installeren

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

1. Open de interactieve shell en zorg ervoor dat deze ingesteld op **PowerShell**.

    ![Aangepaste extensie installeren](./media/create-url-route-portal/application-gateway-extension.png)

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

1. Selecteer **alle resources** en selecteer vervolgens **myAppGateway**.
2. Selecteer **back-endpools**. Er is automatisch een standaardpool gemaakt met de toepassingsgateway. Selecteer **appGatewayBackendPool**.
3. Selecteer **toevoegen doel** om toe te voegen *myVM1* naar appGatewayBackendPool.

    ![Back-endservers toevoegen](./media/create-url-route-portal/application-gateway-backend.png)

4. Selecteer **Opslaan**.
5. Selecteer **back-endpools** en selecteer vervolgens **toevoegen**.
6. Voer een naam in van *imagesBackendPool* en toe te voegen *myVM2* met behulp van **toevoegen doel**.
7. Selecteer **OK**.
8. Selecteer **toevoegen** opnieuw toe te voegen een andere back-endpool met de naam *videoBackendPool* en toe te voegen *myVM3* toe.

## <a name="create-a-backend-listener"></a>Maak een back-end-listener

1. Selecteer **Listeners** en selecteer **Basic**.
2. Voer *myBackendListener* voor de naam van de *myFrontendPort* voor de naam van de front-endpoort, en vervolgens *8080* als de poort voor de listener.
3. Selecteer **OK**.

## <a name="create-a-path-based-routing-rule"></a>Een regel op pad gebaseerde routering maken

1. Selecteer **regels** en selecteer vervolgens **op pad gebaseerde**.
2. Voer *regel 2* voor de naam.
3. Voer *installatiekopieën* voor de naam van het eerste pad. Voer */images/* \* voor het pad. Selecteer **imagesBackendPool** voor de back-endpool.
4. Voer *Video* voor de naam van het tweede pad. Voer */video/* \* voor het pad. Selecteer **videoBackendPool** voor de back-endpool.

    ![Een pad gebaseerde regel maken](./media/create-url-route-portal/application-gateway-route-rule.png)

5. Selecteer **OK**.

## <a name="test-the-application-gateway"></a>De toepassingsgateway testen

1. Selecteer **alle resources**, en selecteer vervolgens **myAGPublicIPAddress**.

    ![Registreer het openbare IP-adres van de toepassingsgateway](./media/create-url-route-portal/application-gateway-record-ag-address.png)

2. Kopieer het openbare IP-adres en plak het in de adresbalk van de browser. Bijvoorbeeld http://40.121.222.19.

    ![Basis-URL testen in de toepassingsgateway](./media/create-url-route-portal/application-gateway-iistest.png)

3. Wijzig de URL naar http://&lt;ip-adres&gt;: 8080/images/test.htm, vervangen &lt;ip-adres&gt; met uw IP-adres, en u ziet er ongeveer als volgt:

    ![Afbeeldingen-URL in toepassingsgateway testen](./media/create-url-route-portal/application-gateway-iistest-images.png)

4. Wijzig de URL naar http://&lt;ip-adres&gt;: 8080/video/test.htm, vervangen &lt;ip-adres&gt; met uw IP-adres, en u ziet er ongeveer als volgt:

    ![Video-URL testen in de toepassingsgateway](./media/create-url-route-portal/application-gateway-iistest-video.png)

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u de bij de toepassingsgateway gemaakte resources niet meer nodig hebt, verwijdert u de resourcegroep. Als u de resourcegroep verwijdert, worden ook de toepassingsgateway en alle gerelateerde resources verwijderd. 

Ga als volgt te werk om de resourcegroep te verwijderen:

1. Selecteer **Resourcegroepen** in het linkermenu van de Azure-portal.
2. Zoek en selecteer **myResourceGroupAG** in de lijst op de pagina **Resourcegroepen**.
3. Selecteer **Resourcegroep verwijderen** op de **pagina van de resourcegroep**.
4. Voer *myResourceGroupAG* voor **TYPE de naam van de RESOURCEGROEP** en selecteer vervolgens **verwijderen**.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over wat u met Azure Application Gateway doen kunt](application-gateway-introduction.md)
