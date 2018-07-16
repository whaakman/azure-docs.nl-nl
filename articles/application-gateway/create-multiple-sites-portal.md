---
title: Een toepassingsgateway die als host fungeert voor meerdere web sites - Azure portal maken | Microsoft Docs
description: Informatie over het maken van een application gateway die als host fungeert voor meerdere sites met behulp van de Azure portal.
services: application-gateway
author: vhorne
manager: jpconnock
editor: tysonn
ms.service: application-gateway
ms.topic: article
ms.workload: infrastructure-services
ms.date: 12/28/2017
ms.author: victorh
ms.openlocfilehash: 8f2e2500c39f42ebd7fefb3cec941088edf126f2
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39045016"
---
# <a name="create-and-configure-an-application-gateway-to-host-multiple-web-sites-using-the-azure-portal"></a>Maken en configureren van een toepassingsgateway voor het hosten van meerdere websites met behulp van de Azure-portal

U kunt de Azure portal gebruiken om [configureren met het hosten van meerdere websites](multiple-site-overview.md) bij het maken van een [toepassingsgateway](overview.md). In deze zelfstudie definieert u de back-end-adresgroepen met behulp van virtuele machines. Vervolgens configureert u listeners en regels op basis van domeinen waarvan u eigenaar bent om er zeker van te zijn dat webverkeer bij de juiste servers in de pools binnenkomen. In deze zelfstudie wordt ervan uitgegaan dat u eigenaar bent van meerdere domeinen en voorbeelden gebruikt van *www.contoso.com* en *www.fabrikam.com*.

In dit artikel leert u het volgende:

> [!div class="checklist"]
> * Een toepassingsgateway maken
> * Virtuele machines voor back-endservers maken
> * Back-endpools maken met de back-endservers
> * Back-endlisteners maken
> * Routeringsregels maken
> * Een CNAME-record in uw domein maken

![Voorbeeld van routeren voor meerdere sites](./media/create-multiple-sites-portal/scenario.png)

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Aanmelden bij Azure Portal op [http://portal.azure.com](http://portal.azure.com)

## <a name="create-an-application-gateway"></a>Een toepassingsgateway maken

Een virtueel netwerk is vereist voor communicatie tussen de bronnen die u maakt. In dit voorbeeld worden twee subnetten gemaakt: één voor de toepassingsgateway en de andere voor de back-endservers. U kunt een virtueel netwerk maken op hetzelfde moment dat u de toepassingsgateway maakt.

1. Klik op **nieuw** gevonden in de linkerbovenhoek van Azure portal.
2. Selecteer **Netwerken** en vervolgens **Application Gateway** in de lijst Aanbevolen.
3. Voer deze waarden in voor de toepassingsgateway:

    - *myAppGateway* als de naam van de toepassingsgateway.
    - *myResourceGroupAG* als de nieuwe resourcegroep.

    ![Nieuwe toepassingsgateway maken](./media/create-multiple-sites-portal/application-gateway-create.png)

4. Accepteer de standaardwaarden voor de overige instellingen en klik op **OK**.
5. Klik op **een virtueel netwerk kiezen**, klikt u op **nieuw**, en voer vervolgens deze waarden voor het virtuele netwerk:

    - *myVnet* als de naam van het virtuele netwerk.
    - *10.0.0.0/16* als de adresruimte van het virtuele netwerk.
    - *myAGSubnet* als de naam van het subnet.
    - *10.0.0.0/24* als de adresruimte van het subnet.

    ![Virtueel netwerk maken](./media/create-multiple-sites-portal/application-gateway-vnet.png)

6. Klik op **OK** om het virtuele netwerk en subnet te maken.
7. Klik op **een openbaar IP-adres kiezen**, klikt u op **nieuw**, en voer de naam van het openbare IP-adres. In dit voorbeeld is de naam van het openbare IP-adres *myAGPublicIPAddress*. Accepteer de standaardwaarden voor de overige instellingen en klik op **OK**.
8. Accepteer de standaardwaarden voor de configuratie van de Listener, laat u de Web application firewall is uitgeschakeld en klik vervolgens op **OK**.
9. Controleer de instellingen op de pagina Samenvatting en klik vervolgens op **OK** om de netwerkbronnen en de application gateway te maken. Het duurt enkele minuten voor de toepassingsgateway worden gemaakt, wacht totdat de implementatie is voltooid voordat u doorgaat met de volgende sectie.

### <a name="add-a-subnet"></a>Een subnet toevoegen

1. Klik in het menu links op **Alle resources** en vervolgens in de lijst met resources op **myVNet**.
2. Klik op **subnetten**, en klik vervolgens op **Subnet**.

    ![Subnet maken](./media/create-multiple-sites-portal/application-gateway-subnet.png)

3. Voer *myBackendSubnet* in als naam van het subnet en klik op **OK**.

## <a name="create-virtual-machines"></a>Virtuele machines maken

In dit voorbeeld maakt u twee virtuele machines die worden gebruikt als back-endservers voor de toepassingsgateway. U kunt ook IIS installeren op de virtuele machines om te controleren dat verkeer juist wordt doorgestuurd.

1. Klik op **Nieuw**.
2. Klik op **Compute** en selecteer vervolgens **Windows Server 2016 Datacenter** in de lijst met aanbevolen.
3. Voer deze waarden in voor de virtuele machine:

    - *contosoVM* : voor de naam van de virtuele machine.
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

    ![Aangepaste extensie installeren](./media/create-multiple-sites-portal/application-gateway-extension.png)

2. Voer de volgende opdracht uit om IIS op de virtuele machine te installeren: 

    ```azurepowershell-interactive
    $publicSettings = @{ "fileUris" = (,"https://raw.githubusercontent.com/Azure/azure-docs-powershell-samples/master/application-gateway/iis/appgatewayurl.ps1");  "commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File appgatewayurl.ps1" }
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

3. De tweede virtuele machine maken en IIS installeren met behulp van de stappen die u zojuist hebt voltooid. Typ de namen van *fabrikamVM* voor de naam en de waarde van VMName in Set-AzureRmVMExtension.

## <a name="create-backend-pools-with-the-virtual-machines"></a>Back-endpools maken met de virtuele machines

1. Klik op **alle resources** en klik vervolgens op **myAppGateway**.
2. Klik op **back-endpools**, en klik vervolgens op **toevoegen**.
3. Voer een naam in van *contosoPool* en toe te voegen *contosoVM* met behulp van **toevoegen doel**.

    ![Back-endservers toevoegen](./media/create-multiple-sites-portal/application-gateway-multisite-backendpool.png)

4. Klik op **OK**.
5. Klik op **back-endpools** en klik vervolgens op **toevoegen**.
6. Maak de *fabrikamPool* met de *fabrikamVM* met behulp van de stappen die u zojuist hebt voltooid.

## <a name="create-backend-listeners"></a>Back-endlisteners maken

1. Klik op **Listeners** en klik vervolgens op **multi-site**.
2. Voer deze waarden in voor de listener:
    
    - *contosoListener* : voor de naam van de listener.
    - *www.contoso.com* -in dit voorbeeld host-naam vervangen door de domeinnaam van uw.

3. Klik op **OK**.
4. Maak een tweede listener met de naam van *fabrikamListener* en de naam van het tweede domein gebruiken. In dit voorbeeld *www.fabrikam.com* wordt gebruikt.

## <a name="create-routing-rules"></a>Routeringsregels maken

Regels worden afgehandeld in de volgorde waarop ze staan vermeld. Verkeer wordt omgeleid volgens de eerste regel die overeenkomt, ongeacht de specificiteit. Als u bijvoorbeeld een regel hebt die van een basislistener gebruikmaakt en een regel die via dezelfde poort van een listener voor meerdere sites gebruikmaakt, moet de regel voor de listener voor meerdere sites vermeld worden vóór de regel met de basislistener, opdat de regel voor meerdere sites kan functioneren zoals het hoort. 

In dit voorbeeld maakt u twee nieuwe regels en verwijdert u de standaardregel die is gemaakt toen u de toepassingsgateway maakte. 

1. Klik op **regels** en klik vervolgens op **Basic**.
2. Voer *contosoRule* voor de naam.
3. Selecteer *contosoListener* voor de listener.
4. Selecteer *contosoPool* voor de back-endpool.

    ![Een pad gebaseerde regel maken](./media/create-multiple-sites-portal/application-gateway-multisite-rule.png)

5. Klik op **OK**.
6. Maak een tweede regel met behulp van de namen van *fabrikamRule*, *fabrikamListener*, en *fabrikamPool*.
7. Verwijder de standaardregel met de naam *rule1* door erop te klikken en vervolgens te klikken op **verwijderen**.

## <a name="create-a-cname-record-in-your-domain"></a>Een CNAME-record in uw domein maken

Als de toepassingsgateway met het bijbehorende openbare IP-adres is gemaakt, kunt u het DNS-adres ophalen en dit gebruiken om een CNAME-record in uw domein te maken. Het gebruik van A-records wordt niet aanbevolen, omdat de VIP kan veranderen wanneer de toepassingsgateway opnieuw wordt gestart.

1. Klik op **alle resources**, en klik vervolgens op **myAGPublicIPAddress**.

    ![Record application-gateway-adres van DNS](./media/create-multiple-sites-portal/application-gateway-multisite-dns.png)

2. Kopieer het DNS-adres en gebruiken als de waarde voor een nieuwe CNAME-record in uw domein.

## <a name="test-the-application-gateway"></a>De toepassingsgateway testen

1. Voer uw domeinnaam in de adresbalk van de browser in. Bijvoorbeeld http://www.contoso.com.

    ![Contoso-site testen in toepassingsgateway](./media/create-multiple-sites-portal/application-gateway-iistest.png)

2. Wijzig het adres in uw andere domein. U krijgt iets te zien zoals in het volgende voorbeeld:

    ![Fabrikam-site testen in toepassingsgateway](./media/create-multiple-sites-portal/application-gateway-iistest2.png)

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd hoe u:

> [!div class="checklist"]
> * Een toepassingsgateway maken
> * Virtuele machines voor back-endservers maken
> * Back-endpools maken met de back-endservers
> * Back-endlisteners maken
> * Routeringsregels maken
> * Een CNAME-record in uw domein maken
