---
title: Een toepassingsgateway maken met URL-pad gebaseerde routeringsregels - Azure-portal | Microsoft Docs
description: Informatie over het maken van de URL op basis van een pad routeringsregels voor een toepassingsgateway en virtuele-machineschaalset ingesteld met de Azure-portal.
services: application-gateway
author: vhorne
manager: jpconnock
editor: tysonn
tags: azure-resource-manager
ms.service: application-gateway
ms.topic: article
ms.workload: infrastructure-services
ms.date: 01/26/2018
ms.author: victorh
ms.openlocfilehash: ecb8a46c57d31c8a19f3a7b75306e42a7d3981bd
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "36335734"
---
# <a name="create-an-application-gateway-with-path-based-routing-rules-using-the-azure-portal"></a>Een toepassingsgateway maken met routering regels op basis van een pad met de Azure portal

U kunt de Azure portal gebruiken voor het configureren van [routeringsregels voor URL-pad gebaseerde](url-route-overview.md) bij het maken van een [toepassingsgateway](overview.md). In deze zelfstudie maakt u back-endpools gebruik van virtuele machines. Vervolgens maakt u routeringsregels die zorg ervoor dat het webverkeer binnenkomt op de juiste servers van de groepen.

In dit artikel leert u het volgende:

> [!div class="checklist"]
> * Een toepassingsgateway maken
> * Maken van virtuele machines voor back-endservers
> * Back-endpools maken met de back-endservers
> * Maak een back-end-listener
> * Maken van een pad op basis van een regel voor doorsturen

![Voorbeeld van URL-routering](./media/create-url-route-portal/scenario.png)

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Aanmelden bij Azure Portal op [http://portal.azure.com](http://portal.azure.com)

## <a name="create-an-application-gateway"></a>Een toepassingsgateway maken

Een virtueel netwerk is vereist voor de communicatie tussen de bronnen die u maakt. In dit voorbeeld worden twee subnetten gemaakt: één voor de toepassingsgateway en de andere voor de back-endservers. U kunt een virtueel netwerk maken op hetzelfde moment dat u de toepassingsgateway maakt.

1. Klik op **nieuw** gevonden in de linkerbovenhoek van de Azure portal.
2. Selecteer **Netwerken** en vervolgens **Application Gateway** in de lijst Aanbevolen.
3. Voer deze waarden in voor de toepassingsgateway:

    - *myAppGateway* als de naam van de toepassingsgateway.
    - *myResourceGroupAG* als de nieuwe resourcegroep.

    ![Nieuwe toepassingsgateway maken](./media/create-url-route-portal/application-gateway-create.png)

4. Accepteer de standaardwaarden voor de overige instellingen en klik op **OK**.
5. Klik op **Kies een virtueel netwerk**, klikt u op **nieuw**, en voer deze waarden voor het virtuele netwerk:

    - *myVnet* als de naam van het virtuele netwerk.
    - *10.0.0.0/16* als de adresruimte van het virtuele netwerk.
    - *myAGSubnet* als de naam van het subnet.
    - *10.0.0.0/24* als de adresruimte van het subnet.

    ![Virtueel netwerk maken](./media/create-url-route-portal/application-gateway-vnet.png)

6. Klik op **OK** om het virtuele netwerk en subnet te maken.
7. Klik op **Kies een openbaar IP-adres**, klikt u op **nieuw**, en voer vervolgens de naam van het openbare IP-adres. In dit voorbeeld is de naam van het openbare IP-adres *myAGPublicIPAddress*. Accepteer de standaardwaarden voor de overige instellingen en klik op **OK**.
8. Accepteer de standaardwaarden voor de configuratie van de Listener, laat u de Web application firewall is uitgeschakeld en klik vervolgens op **OK**.
9. Controleer de instellingen op de pagina Samenvatting en klik vervolgens op **OK** om de netwerkbronnen en de toepassingsgateway te maken. Duurt enkele minuten voor de toepassingsgateway worden gemaakt, wacht totdat de implementatie wordt voltooid voordat u doorgaat met de volgende sectie.

### <a name="add-a-subnet"></a>Een subnet toevoegen

1. Klik in het menu links op **Alle resources** en vervolgens in de lijst met resources op **myVNet**.
2. Klik op **subnetten**, en klik vervolgens op **Subnet**.

    ![Subnet maken](./media/create-url-route-portal/application-gateway-subnet.png)

3. Voer *myBackendSubnet* in als naam van het subnet en klik op **OK**.

## <a name="create-virtual-machines"></a>Virtuele machines maken

In dit voorbeeld maakt u drie virtuele machines moet worden gebruikt als back-endservers voor de toepassingsgateway. U installeert ook IIS op de virtuele machines om te controleren of de toepassingsgateway is gemaakt.

1. Klik op **Nieuw**.
2. Klik op **Compute** en selecteer vervolgens **Windows Server 2016 Datacenter** in de lijst met aanbevolen.
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

    ![Aangepaste extensie installeren](./media/create-url-route-portal/application-gateway-extension.png)

2. Voer de volgende opdracht uit om IIS op de virtuele machine te installeren: 

    ```azurepowershell-interactive
    $publicSettings = @{ "fileUris" = (,"https://raw.githubusercontent.com/davidmu1/samplescripts/master/appgatewayurl.ps1");  "commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File appgatewayurl.ps1" }
    Set-AzureRmVMExtension `
      -ResourceGroupName myResourceGroupAG `
      -Location eastus `
      -ExtensionName IIS `
      -VMName myVM1 `
      -Publisher Microsoft.Compute `
      -ExtensionType CustomScriptExtension `
      -TypeHandlerVersion 1.4 `
      -Settings $publicSettings
    ```

3. Maak twee meer virtuele machines en IIS installeren met behulp van de stappen die u zojuist hebt voltooid. Typ de namen van *myVM2* en *myVM3* voor de namen en de waarden van VMName in Set AzureRmVMExtension.

## <a name="create-backend-pools-with-the-virtual-machines"></a>Back-endpools maken met de virtuele machines

1. Klik op **alle resources** en klik vervolgens op **myAppGateway**.
2. Klik op **Back-endpools**. Er is automatisch een standaardpool gemaakt met de toepassingsgateway. Klik op **appGateayBackendPool**.
3. Klik op **toevoegen doel** om toe te voegen *myVM1* naar appGatewayBackendPool.

    ![Back-endservers toevoegen](./media/create-url-route-portal/application-gateway-backend.png)

4. Klik op **Opslaan**.
5. Klik op **back-endpools** en klik vervolgens op **toevoegen**.
6. Voer een naam in van *imagesBackendPool* en voeg *myVM2* met **toevoegen doel**.
7. Klik op **OK**.
8. Klik op **toevoegen** om opnieuw toe te voegen met een naam van een andere back-endpool *videoBackendPool* en voeg *myVM3* aan.

## <a name="create-a-backend-listener"></a>Maak een back-end-listener

1. Klik op **Listeners** en klik op **Basic**.
2. Voer *myBackendListener* voor de naam *myFrontendPort* voor de naam van de frontend-poort, en vervolgens *8080* als de poort voor de listener.
3. Klik op **OK**.

## <a name="create-a-path-based-routing-rule"></a>Maken van een pad op basis van een regel voor doorsturen

1. Klik op **regels** en klik vervolgens op **op basis van het pad**.
2. Voer *regel 2* voor de naam.
3. Voer *installatiekopieën* voor de naam van het eerste pad. Voer */images/* \* voor het pad. Selecteer **imagesBackendPool** voor de back-endpool.
4. Voer *Video* voor de naam van het tweede pad. Voer */video/* \* voor het pad. Selecteer **videoBackendPool** voor de back-endpool.

    ![Maak een regel op basis van het pad](./media/create-url-route-portal/application-gateway-route-rule.png)

5. Klik op **OK**.

## <a name="test-the-application-gateway"></a>De toepassingsgateway testen

1. Klik op **alle resources**, en klik vervolgens op **myAGPublicIPAddress**.

    ![Registreer het openbare IP-adres van de toepassingsgateway](./media/create-url-route-portal/application-gateway-record-ag-address.png)

2. Kopieer het openbare IP-adres en plak het in de adresbalk van de browser. Bijvoorbeeld http://40.121.222.19.

    ![Basis-URL testen in de toepassingsgateway](./media/create-url-route-portal/application-gateway-iistest.png)

3. Wijzig de URL naar http://&lt;IP-adres&gt;: 8080/video/test.htm, vervangen door &lt;IP-adres&gt; met uw IP-adres en u ziet ongeveer het volgende voorbeeld:

    ![Afbeeldingen-URL in toepassingsgateway testen](./media/create-url-route-portal/application-gateway-iistest-images.png)

4. Wijzig de URL naar http://&lt;IP-adres&gt;: 8080/video/test.htm, vervangen door &lt;IP-adres&gt; met uw IP-adres en u ziet ongeveer het volgende voorbeeld:

    ![Video-URL testen in de toepassingsgateway](./media/create-url-route-portal/application-gateway-iistest-video.png)

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd hoe u

> [!div class="checklist"]
> * Een toepassingsgateway maken
> * Maken van virtuele machines voor back-endservers
> * Back-endpools maken met de back-endservers
> * Maak een back-end-listener
> * Maken van een pad op basis van een regel voor doorsturen

Blijven de artikelen voor meer informatie over Toepassingsgateways en de bijbehorende resources.
