---
title: Een toepassingsgateway maken met URL-pad gebaseerde routeringsregels - Azure-portal | Microsoft Docs
description: Informatie over het maken van de URL op basis van een pad routeringsregels voor een toepassingsgateway en virtuele-machineschaalset ingesteld met de Azure-portal.
services: application-gateway
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.service: application-gateway
ms.topic: article
ms.workload: infrastructure-services
ms.date: 01/26/2018
ms.author: davidmu
ms.openlocfilehash: 62063c42ab15a071a4500417a5d8adf6bfeac97f
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/24/2018
---
# <a name="create-an-application-gateway-with-path-based-routing-rules-using-the-azure-portal"></a>Een toepassingsgateway maken met routering regels op basis van een pad met de Azure portal

U kunt de Azure portal gebruiken voor het configureren van [routeringsregels voor URL-pad gebaseerde](application-gateway-url-route-overview.md) bij het maken van een [toepassingsgateway](application-gateway-introduction.md). In deze zelfstudie maakt u back-endpools gebruik van virtuele machines. Vervolgens maakt u routeringsregels die zorg ervoor dat het webverkeer binnenkomt op de juiste servers van de groepen.

In dit artikel leert u hoe:

> [!div class="checklist"]
> * Een toepassingsgateway maken
> * Maken van virtuele machines voor back-endservers
> * Back-endpools maken met de back-endservers
> * Maak een back-end-listener
> * Maken van een pad op basis van een regel voor doorsturen

![Voorbeeld van de URL-routering](./media/application-gateway-create-url-route-portal/scenario.png)

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Aanmelden bij de Azure portal op [http://portal.azure.com](http://portal.azure.com)

## <a name="create-an-application-gateway"></a>Een toepassingsgateway maken

Een virtueel netwerk is vereist voor de communicatie tussen de bronnen die u maakt. Twee subnetten worden gemaakt in dit voorbeeld: één voor de toepassingsgateway, en de andere voor de back-endservers. U kunt een virtueel netwerk maken op hetzelfde moment dat u de toepassingsgateway maakt.

1. Klik op **nieuw** gevonden in de linkerbovenhoek van de Azure portal.
2. Selecteer **Networking** en selecteer vervolgens **Application Gateway** in de lijst met aanbevolen.
3. Voer deze waarden voor de toepassingsgateway:

    - *myAppGateway* - voor de naam van de toepassingsgateway.
    - *myResourceGroupAG* - voor de nieuwe resourcegroep.

    ![Nieuwe toepassingsgateway maken](./media/application-gateway-create-url-route-portal/application-gateway-create.png)

4. Accepteer de standaardwaarden voor de overige instellingen en klik vervolgens op **OK**.
5. Klik op **Kies een virtueel netwerk**, klikt u op **nieuw**, en voer deze waarden voor het virtuele netwerk:

    - *myVNet* - voor de naam van het virtuele netwerk.
    - *10.0.0.0/16* - voor de adresruimte van het virtuele netwerk.
    - *myAGSubnet* - voor de subnetnaam.
    - *10.0.0.0/24* - voor de adresruimte van het subnet.

    ![Virtueel netwerk maken](./media/application-gateway-create-url-route-portal/application-gateway-vnet.png)

6. Klik op **OK** voor het maken van het virtuele netwerk en subnet.
7. Klik op **Kies een openbaar IP-adres**, klikt u op **nieuw**, en voer vervolgens de naam van het openbare IP-adres. In dit voorbeeld wordt het openbare IP-adres met de naam *myAGPublicIPAddress*. Accepteer de standaardwaarden voor de overige instellingen en klik vervolgens op **OK**.
8. Accepteer de standaardwaarden voor de configuratie van de Listener, laat u de Web application firewall is uitgeschakeld en klik vervolgens op **OK**.
9. Controleer de instellingen op de pagina Samenvatting en klik vervolgens op **OK** om de netwerkbronnen en de toepassingsgateway te maken. Duurt enkele minuten voor de toepassingsgateway worden gemaakt, wacht totdat de implementatie wordt voltooid voordat u doorgaat met de volgende sectie.

### <a name="add-a-subnet"></a>Een subnet toevoegen

1. Klik op **alle resources** in het menu links en klik vervolgens op **myVNet** uit de lijst met resources.
2. Klik op **subnetten**, en klik vervolgens op **Subnet**.

    ![Subnet maken](./media/application-gateway-create-url-route-portal/application-gateway-subnet.png)

3. Voer *myBackendSubnet* voor de naam van het subnet en klik vervolgens op **OK**.

## <a name="create-virtual-machines"></a>Virtuele machines maken

In dit voorbeeld maakt u drie virtuele machines moet worden gebruikt als back-endservers voor de toepassingsgateway. U kunt ook IIS installeren op de virtuele machines om te controleren of de toepassingsgateway is gemaakt.

1. Klik op **Nieuw**.
2. Klik op **Compute** en selecteer vervolgens **Windows Server 2016 Datacenter** in de lijst met aanbevolen.
3. Voer deze waarden voor de virtuele machine:

    - *myVM1* - voor de naam van de virtuele machine.
    - *azureuser* - voor de gebruikersnaam van de beheerder.
    - *Azure123456!* voor het wachtwoord.
    - Selecteer **gebruik bestaande**, en selecteer vervolgens *myResourceGroupAG*.

4. Klik op **OK**.
5. Selecteer **DS1_V2** voor de grootte van de virtuele machine en klik op **Selecteer**.
6. Zorg ervoor dat **myVNet** is geselecteerd voor het virtuele netwerk en het subnet is **myBackendSubnet**. 
7. Klik op **uitgeschakelde** diagnostische gegevens over opstarten uitschakelen.
8. Klik op **OK**, Controleer de instellingen op de pagina Samenvatting en klik vervolgens op **maken**.

### <a name="install-iis"></a>IIS installeren

1. Open de interactieve shell en zorg ervoor dat deze is ingesteld op **PowerShell**.

    ![Aangepaste extensie installeren](./media/application-gateway-create-url-route-portal/application-gateway-extension.png)

2. Voer de volgende opdracht voor het installeren van IIS op de virtuele machine: 

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
2. Klik op **back-endpools**. Een standaardgroep is automatisch gemaakt met de toepassingsgateway. Klik op **appGatewayBackendPool**.
3. Klik op **toevoegen doel** om toe te voegen *myVM1* naar appGatewayBackendPool.

    ![Back-endservers toevoegen](./media/application-gateway-create-url-route-portal/application-gateway-backend.png)

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
3. Voer *installatiekopieën* voor de naam van het eerste pad. Voer */images/** voor het pad. Selecteer **imagesBackendPool** voor de back-endpool.
4. Voer *Video* voor de naam van het tweede pad. Voer */video/** voor het pad. Selecteer **videoBackendPool** voor de back-endpool.

    ![Maak een regel op basis van het pad](./media/application-gateway-create-url-route-portal/application-gateway-route-rule.png)

5. Klik op **OK**.

## <a name="test-the-application-gateway"></a>Testen van de toepassingsgateway

1. Klik op **alle resources**, en klik vervolgens op **myAGPublicIPAddress**.

    ![Record application gateway openbaar IP-adres](./media/application-gateway-create-url-route-portal/application-gateway-record-ag-address.png)

2. Het openbare IP-adres Kopieer en plak deze in de adresbalk van uw browser. Zoals http://http: / / 40.121.222.19.

    ![Basis-URL te testen in de toepassingsgateway](./media/application-gateway-create-url-route-portal/application-gateway-iistest.png)

3. Wijzig de URL naar http://&lt;IP-adres&gt;: 8080/video/test.htm, vervangen door &lt;IP-adres&gt; met uw IP-adres en u ziet ongeveer het volgende voorbeeld:

    ![Test-URL voor afbeeldingen in de toepassingsgateway](./media/application-gateway-create-url-route-portal/application-gateway-iistest-images.png)

4. Wijzig de URL naar http://&lt;IP-adres&gt;: 8080/video/test.htm, vervangen door &lt;IP-adres&gt; met uw IP-adres en u ziet ongeveer het volgende voorbeeld:

    ![Video-URL te testen in de toepassingsgateway](./media/application-gateway-create-url-route-portal/application-gateway-iistest-video.png)

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd hoe u

> [!div class="checklist"]
> * Een toepassingsgateway maken
> * Maken van virtuele machines voor back-endservers
> * Back-endpools maken met de back-endservers
> * Maak een back-end-listener
> * Maken van een pad op basis van een regel voor doorsturen

Blijven de artikelen voor meer informatie over Toepassingsgateways en de bijbehorende resources.
