---
title: Een toepassingsgateway maken met meerdere hosting-site - Azure-portal | Microsoft Docs
description: Informatie over het maken van een toepassingsgateway die als host fungeert voor meerdere sites met de Azure-portal.
services: application-gateway
author: vhorne
manager: jpconnock
editor: tysonn
ms.service: application-gateway
ms.topic: article
ms.workload: infrastructure-services
ms.date: 01/26/2018
ms.author: victorh
ms.openlocfilehash: 0db4e3af1e355df2c985922012191cdc7d97d5b7
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
ms.locfileid: "34354740"
---
# <a name="create-an-application-gateway-with-multiple-site-hosting-using-the-azure-portal"></a>Een toepassingsgateway maken met meerdere site die als host fungeert voor het gebruik van de Azure-portal

U kunt de Azure portal gebruiken voor het configureren van [hosting van meerdere websites](application-gateway-multi-site-overview.md) bij het maken van een [toepassingsgateway](application-gateway-introduction.md). In deze zelfstudie maakt u back-endpools met behulp van virtuele machines-schaalsets. Configureert u listeners en regels op basis van domeinnamen waarvan u eigenaar om ervoor te zorgen dat het webverkeer binnenkomt op de juiste servers van de groepen. Deze zelfstudie wordt ervan uitgegaan dat u voorbeelden van meerdere domeinen en maakt gebruik van de eigenaar *www.contoso.com* en *www.fabrikam.com*.

In dit artikel leert u het volgende:

> [!div class="checklist"]
> * Een toepassingsgateway maken
> * Maken van virtuele machines voor back-endservers
> * Back-endpools maken met de back-endservers
> * Listeners en routeringsregels maken
> * Een CNAME-record maken in uw domein

![Routering multi-site-voorbeeld](./media/application-gateway-create-multisite-portal/scenario.png)

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

    ![Nieuwe toepassingsgateway maken](./media/application-gateway-create-multisite-portal/application-gateway-create.png)

4. Accepteer de standaardwaarden voor de overige instellingen en klik vervolgens op **OK**.
5. Klik op **Kies een virtueel netwerk**, klikt u op **nieuw**, en voer deze waarden voor het virtuele netwerk:

    - *myVNet* - voor de naam van het virtuele netwerk.
    - *10.0.0.0/16* - voor de adresruimte van het virtuele netwerk.
    - *myAGSubnet* - voor de subnetnaam.
    - *10.0.0.0/24* - voor de adresruimte van het subnet.

    ![Virtueel netwerk maken](./media/application-gateway-create-multisite-portal/application-gateway-vnet.png)

6. Klik op **OK** voor het maken van het virtuele netwerk en subnet.
7. Klik op **Kies een openbaar IP-adres**, klikt u op **nieuw**, en voer vervolgens de naam van het openbare IP-adres. In dit voorbeeld wordt het openbare IP-adres met de naam *myAGPublicIPAddress*. Accepteer de standaardwaarden voor de overige instellingen en klik vervolgens op **OK**.
8. Accepteer de standaardwaarden voor de configuratie van de Listener, laat u de Web application firewall is uitgeschakeld en klik vervolgens op **OK**.
9. Controleer de instellingen op de pagina Samenvatting en klik vervolgens op **OK** om de netwerkbronnen en de toepassingsgateway te maken. Duurt enkele minuten voor de toepassingsgateway worden gemaakt, wacht totdat de implementatie wordt voltooid voordat u doorgaat met de volgende sectie.

### <a name="add-a-subnet"></a>Een subnet toevoegen

1. Klik op **alle resources** in het menu links en klik vervolgens op **myVNet** uit de lijst met resources.
2. Klik op **subnetten**, en klik vervolgens op **Subnet**.

    ![Subnet maken](./media/application-gateway-create-multisite-portal/application-gateway-subnet.png)

3. Voer *myBackendSubnet* voor de naam van het subnet en klik vervolgens op **OK**.

## <a name="create-virtual-machines"></a>Virtuele machines maken

In dit voorbeeld maakt u twee virtuele machines moet worden gebruikt als back-endservers voor de toepassingsgateway. U kunt ook IIS installeren op de virtuele machines om te controleren dat verkeer juist wordt doorgestuurd.

1. Klik op **Nieuw**.
2. Klik op **Compute** en selecteer vervolgens **Windows Server 2016 Datacenter** in de lijst met aanbevolen.
3. Voer deze waarden voor de virtuele machine:

    - *contosoVM* - voor de naam van de virtuele machine.
    - *azureuser* als gebruikersnaam van de beheerder.
    - *Azure123456!* voor het wachtwoord.
    - Selecteer **gebruik bestaande**, en selecteer vervolgens *myResourceGroupAG*.

4. Klik op **OK**.
5. Selecteer **DS1_V2** als grootte van de virtuele machine en klik op **Selecteren**.
6. Zorg ervoor dat **myVNet** is geselecteerd voor het virtuele netwerk en het subnet is **myBackendSubnet**. 
7. Klik op **Uitgeschakeld** om diagnostische gegevens over opstarten uit te schakelen.
8. Klik op **OK**, controleer de instellingen op de overzichtspagina en klik op **Maken**.

### <a name="install-iis"></a>IIS installeren

1. Open de interactieve shell en zorg ervoor dat deze is ingesteld op **PowerShell**.

    ![Aangepaste extensie installeren](./media/application-gateway-create-multisite-portal/application-gateway-extension.png)

2. Voer de volgende opdracht voor het installeren van IIS op de virtuele machine: 

    ```azurepowershell-interactive
    $publicSettings = @{ "fileUris" = (,"https://raw.githubusercontent.com/davidmu1/samplescripts/master/appgatewayurl.ps1");  "commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File appgatewayurl.ps1" }
    Set-AzureRmVMExtension `
      -ResourceGroupName myResourceGroupAG `
      -Location eastus `
      -ExtensionName IIS `
      -VMName contosoVM `
      -Publisher Microsoft.Compute `
      -ExtensionType CustomScriptExtension `
      -TypeHandlerVersion 1.4 `
      -Settings $publicSettings
    ```

3. De tweede virtuele machine maken en IIS installeren met behulp van de stappen die u zojuist hebt voltooid. Typ de namen van *fabrikamVM* voor de naam en de waarde van VMName in Set AzureRmVMExtension.

## <a name="create-backend-pools-with-the-virtual-machines"></a>Back-endpools maken met de virtuele machines

1. Klik op **alle resources** en klik vervolgens op **myAppGateway**.
2. Klik op **back-endpools**, en klik vervolgens op **toevoegen**.
3. Voer een naam in van *contosoPool* en voeg *contosoVM* met **toevoegen doel**.

    ![Back-endservers toevoegen](./media/application-gateway-create-multisite-portal/application-gateway-multisite-backendpool.png)

4. Klik op **OK**.
5. Klik op **back-endpools** en klik vervolgens op **toevoegen**.
6. Maak de *fabrikamPool* met de *fabrikamVM* met behulp van de stappen die u zojuist hebt voltooid.

## <a name="create-listeners-and-routing-rules"></a>Listeners en routeringsregels maken

1. Klik op **Listeners** en klik vervolgens op **multi-site**.
2. Voer deze waarden voor de listener:
    
    - *contosoListener* - voor de naam van de listener.
    - *www.contoso.com* -in dit voorbeeld host-naam vervangen door de domeinnaam van uw.

3. Klik op **OK**.
4. Maak een tweede listener met de naam van *fabrikamListener* en de naam van het tweede domein gebruiken. In dit voorbeeld *www.fabrikam.com* wordt gebruikt.

Regels worden verwerkt in de volgorde waarin ze worden weergegeven, en verkeer wordt omgeleid met behulp van de eerste regel die overeenkomt met ongeacht specifieke karakter. Bijvoorbeeld, als u een regel met behulp van een basic-listener en een regel met meerdere sites listener beide op dezelfde poort hebt, kan de regel met de listener voor meerdere locaties moet worden vermeld voordat u de regel met de basic-listener in volgorde voor de regel voor meerdere locaties werken zoals verwacht. 

In dit voorbeeld moet u twee nieuwe regels maken en verwijderen van de standaardregel die is gemaakt tijdens het maken van de toepassingsgateway. 

1. Klik op **regels** en klik vervolgens op **Basic**.
2. Voer *contosoRule* voor de naam.
3. Selecteer *contosoListener* voor de listener.
4. Selecteer *contosoPool* voor de back-endpool.

    ![Maak een regel op basis van het pad](./media/application-gateway-create-multisite-portal/application-gateway-multisite-rule.png)

5. Klik op **OK**.
6. Een tweede regel maken met de namen van *fabrikamRule*, *fabrikamListener*, en *fabrikamPool*.
7. Verwijderen van de standaardregel met de naam *rule1* door erop te klikken en vervolgens te klikken op **verwijderen**.

## <a name="create-a-cname-record-in-your-domain"></a>Een CNAME-record maken in uw domein

Nadat de toepassingsgateway is gemaakt met het openbare IP-adres, kunt u ophalen van de DNS-serveradres en een CNAME-record maken in uw domein. Het gebruik van A-records wordt niet aanbevolen omdat het VIP kan worden gewijzigd wanneer de toepassingsgateway opnieuw wordt opgestart.

1. Klik op **alle resources**, en klik vervolgens op **myAGPublicIPAddress**.

    ![Toepassingsgateway record DNS-adres](./media/application-gateway-create-multisite-portal/application-gateway-multisite-dns.png)

2. Kopieer de DNS-serveradres en gebruiken als de waarde voor een nieuwe CNAME-record in uw domein.

## <a name="test-the-application-gateway"></a>Testen van de toepassingsgateway

1. Voer de domeinnaam van uw in de adresbalk van uw browser. Zoals http://www.contoso.com.

    ![Testen van contoso-site in de toepassingsgateway](./media/application-gateway-create-multisite-portal/application-gateway-iistest.png)

2. Wijzig het adres in het andere domein en u ziet ongeveer het volgende voorbeeld:

    ![Fabrikam site in de toepassingsgateway testen](./media/application-gateway-create-multisite-portal/application-gateway-iistest2.png)

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd hoe:

> [!div class="checklist"]
> * Een toepassingsgateway maken
> * Maken van virtuele machines voor back-endservers
> * Back-endpools maken met de back-endservers
> * Listeners en routeringsregels maken
> * Een CNAME-record maken in uw domein

> [!div class="nextstepaction"]
> [Meer informatie over wat u met application gateway doen kunt](application-gateway-introduction.md)