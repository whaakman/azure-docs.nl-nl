---
title: Zelf studie-een toepassings gateway maken met behulp van op URL-pad gebaseerde routerings regels-Azure Portal
description: In deze zelf studie leert u hoe u op URL-pad gebaseerde routerings regels maakt voor een toepassings gateway en een schaalset voor virtuele machines met behulp van de Azure Portal.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: tutorial
ms.date: 07/27/2019
ms.author: victorh
ms.openlocfilehash: 63a1faa79374e72eabfbee4ece454728c3b4cc05
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/29/2019
ms.locfileid: "68597591"
---
# <a name="tutorial-create-an-application-gateway-with-path-based-routing-rules-using-the-azure-portal"></a>Zelfstudie: Een toepassings gateway met op pad gebaseerde routerings regels maken met behulp van de Azure Portal

U kunt de Azure Portal gebruiken om op [URL-pad gebaseerde routerings regels](url-route-overview.md) te configureren wanneer u een [toepassings gateway](overview.md)maakt. In deze zelf studie maakt u back-endservers met virtuele machines. Vervolgens maakt u routerings regels die ervoor zorgen dat webverkeer binnenkomt op de juiste servers in de groepen.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een toepassingsgateway maken
> * Virtuele machines maken voor back-endservers
> * Back-endservers maken met de back-endservers
> * Een backend-listener maken
> * Een op pad gebaseerde routerings regel maken

![Voorbeeld van URL-routering](./media/create-url-route-portal/scenario.png)

Als u wilt, kunt u deze zelf studie volt ooien met behulp van [Azure cli](tutorial-url-route-cli.md) of [Azure PowerShell](tutorial-url-route-powershell.md).

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij de Azure Portal op [https://portal.azure.com](https://portal.azure.com).

## <a name="create-an-application-gateway"></a>Een toepassingsgateway maken

1. Selecteer **Een resource maken** in het linkermenu van de Azure-portal. Het venster **Nieuw** wordt weergegeven.

2. Selecteer **Netwerken** en vervolgens **Application Gateway** in de lijst **Aanbevolen**.

### <a name="basics-tab"></a>Tabblad basis beginselen

1. Voer op het tabblad **basis beginselen** deze waarden in voor de volgende instellingen voor de toepassings gateway:

   - **Resourcegroep**: Selecteer **myResourceGroupAG** als de resourcegroep. Als deze nog niet bestaat, selecteert u **Nieuwe maken** om deze te maken.
   - **Naam van de toepassings gateway**: Typ *myAppGateway* als naam voor de toepassingsgateway.

     ![Nieuwe toepassings gateway maken: Basics](./media/application-gateway-create-gateway-portal/application-gateway-create-basics.png)

2.  Er is een virtueel netwerk nodig voor communicatie tussen de resources die u maakt. U kunt een nieuw virtueel netwerk maken of een bestaande gebruiken. In dit voor beeld maakt u een nieuw virtueel netwerk op het moment dat u de toepassings gateway maakt. Application Gateway exemplaren worden in afzonderlijke subnetten gemaakt. In dit voorbeeld maakt u twee subnetten: één voor de toepassingsgateway en één voor de back-endservers.

    Onder **virtueel netwerk configureren**selecteert u **nieuwe maken** om een nieuw virtueel netwerk te maken. In het venster **virtueel netwerk maken** dat wordt geopend, voert u de volgende waarden in om het virtuele netwerk en twee subnetten te maken:

    - **Naam**: Typ *myVnet* als naam voor het virtuele netwerk.

    - **Subnetnaam** (Application Gateway subnet): In  het raster subnetten wordt een subnet met de naam *standaard*weer gegeven. Wijzig de naam van dit subnet in *myAGSubnet*.

      Het subnet van de toepassingsgateway kan alleen bestaan uit toepassingsgateways. Andere resources zijn niet toegestaan.

    - **Subnetnaam** (subnet van back-endserver): Voer in de tweede rij van  het raster subnetten *myBackendSubnet* in de kolom **subnet name** in.

    - **Adres bereik** (subnet van back-endserver): Voer in de tweede rij van  het raster subnetten een adres bereik in dat niet overlapt met het adres bereik van *myAGSubnet*. Als het adres bereik van *myAGSubnet* bijvoorbeeld 10.0.0.0/24 is, voert u *10.0.1.0/24* in voor het adres bereik van *myBackendSubnet*.

    Selecteer **OK** om het venster **virtueel netwerk maken** te sluiten en de instellingen voor het virtuele netwerk op te slaan.

     ![Nieuwe toepassings gateway maken: virtueel netwerk](./media/application-gateway-create-gateway-portal/application-gateway-create-vnet.png)
    
3. Accepteer op het tabblad **basis beginselen** de standaard waarden voor de overige instellingen en selecteer **vervolgens volgende: Front-end.**

### <a name="frontends-tab"></a>Tabblad front-ends

1. Controleer op  het tabblad frontends of het **frontend-IP-adres type** is ingesteld op **openbaar**. <br>U kunt de frontend-IP zo configureren dat deze openbaar of privé is volgens uw use-case. In dit voor beeld kiest u een openbaar frontend-IP.
   > [!NOTE]
   > Voor de SKU van Application Gateway v2 kunt u alleen de **open bare** frontend-IP-configuratie kiezen. De persoonlijke frontend-IP-configuratie is op dit moment niet ingeschakeld voor deze v2-SKU.

2. Kies **Nieuw maken** voor het **open bare IP-adres** en voer *myAGPublicIPAddress* in als naam voor het open bare IP-adres en selecteer vervolgens **OK**. 

     ![Nieuwe toepassings gateway maken: front-end](./media/application-gateway-create-gateway-portal/application-gateway-create-frontends.png)

3. Selecteer **Volgende: Back-end.**

### <a name="backends-tab"></a>Tabblad back-ends

De back-end-groep wordt gebruikt voor het routeren van aanvragen naar de back-endservers die de aanvraag behandelen. Back-endservers kunnen Nic's zijn, virtuele-machine schaal sets, open bare Ip's, interne Ip's, FQDN-namen (Fully Qualified Domain names) en back-ends met meerdere tenants, zoals Azure App Service. In dit voor beeld maakt u een lege back-end-pool met uw toepassings gateway en voegt u vervolgens de back-endservers toe aan de back-end-groep.

1. Selecteer op het tabblad **back** -end **+ een back-end-groep toevoegen**.

2. In het venster **een back-Endadresgroep toevoegen** dat wordt geopend, voert u de volgende waarden in om een lege back-end-groep te maken:

    - **Naam**: Voer *appGatewayBackendPool* in als de naam van de back-end-groep.
    - **Back-end-groep zonder doelen toevoegen**: Selecteer **Ja** als u een back-end-groep zonder doelen wilt maken. U voegt back-endservers toe nadat u de toepassings gateway hebt gemaakt.

3. Selecteer in het venster **een back-Endadresgroep toevoegen** de optie **toevoegen** om de back-endadresgroep op te slaan en terug te keren naar het tabblad **back-end** .
4. Voeg nu twee back-endservers toe met de naam *imagesBackendPool*en *videoBackendPool*.

     ![Nieuwe toepassings gateway maken: back-end](./media/create-url-route-portal/backends.png)

4. Op het tabblad **back-end** selecteert **u volgende: Configuratie**.

### <a name="configuration-tab"></a>Tabblad Configuratie

Op het tabblad **configuratie** verbindt u de front-end-en back-endservers die u hebt gemaakt met behulp van routerings regels.

1. Selecteer **een regel toevoegen** in de kolom **routerings regels** .
2. Voer in het venster **een regel voor de route ring toevoegen** die wordt geopend, *firewallregel1* in als naam van de **regel**.
3. Een routerings regel vereist een listener. Voer op het tabblad **listener** in het venster **een regel voor de route ring toevoegen** de volgende waarden in voor de listener:

    - **Naam van listener**: Voer *DefaultListener* in als de naam van de listener.
    - **Frontend-IP**: Selecteer **openbaar** om het open bare IP-adres te kiezen dat u hebt gemaakt voor de front-end.

   Accepteer de standaard waarden voor de overige instellingen op het tabblad **listener** en selecteer vervolgens het tabblad **backend-doelen** om de rest van de routerings regel te configureren.
4. Op het tabblad **backend-doelen** selecteert u **AppGatewayBackendPool** voor het back- **end-doel**.

5. Voor de **http-instelling**selecteert u **Nieuw maken** om een nieuwe http-instelling te maken. De HTTP-instelling bepaalt het gedrag van de routerings regel. In het venster **een HTTP-instelling toevoegen** dat wordt geopend, voert u *myHTTPSetting* in voor de naam van de **http-instelling**. Accepteer de standaard waarden voor de overige instellingen in het venster **een HTTP-instelling toevoegen** en selecteer vervolgens **toevoegen** om terug te gaan naar het venster een regel voor het routeren van **een route ring toevoegen** . 

6. Selecteer in het venster **een routerings regel toevoegen** de optie **toevoegen** om de routerings regel op te slaan en terug te keren naar het tabblad **configuratie** .



1. Selecteer **een regel toevoegen** in de kolom **routerings regels** .

2. Voer in het venster **een regel voor de route ring toevoegen** die wordt geopend, *firewallregel2* in als naam van de **regel**.

3. Een routerings regel vereist een listener. Voer op het tabblad **listener** in het venster **een regel voor de route ring toevoegen** de volgende waarden in voor de listener:

    - **Naam van listener**: Voer *myBackendListener* in als de naam van de listener.
    - **Frontend-IP**: Selecteer **openbaar** om het open bare IP-adres te kiezen dat u hebt gemaakt voor de front-end.
    - **Poort**: 8080

   Onder **aanvullende instellingen**:
   - **Type listener**: Basic

   Accepteer de standaard waarden voor de overige instellingen op het tabblad **listener** en selecteer vervolgens het tabblad **backend-doelen** om de rest van de routerings regel te configureren.

4. Op het tabblad **backend-doelen** selecteert u **AppGatewayBackendPool** voor het back- **end-doel**.

5. Selecteer *myHTTPSetting*voor de **http-instelling**. Accepteer de standaard waarden voor de overige instellingen in het venster **een HTTP-instelling toevoegen** en selecteer vervolgens **toevoegen** om terug te gaan naar het venster een regel voor het routeren van **een route ring toevoegen** . 

1. Onder **route ring op basis van paden**selecteert **u meerdere doelen toevoegen om een op pad gebaseerde regel te maken**.
2. Voer in het venster **een regel voor het toevoegen van een pad** de volgende waarden in voor de padregel:

   - **Pad**: */images/\**
   - **Naam van padregel**: *Installatiekopieën*
   - **Http-instelling**: Selecteer *myHTTPSetting*
   - **Back-end-doel**: *imagesBackendPool*
9. Selecteer **Toevoegen**.
10. Voeg nog een padregel met de naam *video*toe, met een */video/\**  -pad en *videoBackendPool*.
11. Selecteer **wijzigingen opslaan en ga terug naar routerings regels**.

    ![Een regel voor doorsturen toevoegen](media/create-url-route-portal/add-routing-rule.png)

12. Selecteer **Toevoegen**.

7. Selecteer **Volgende: Tags** en vervolgens **volgende: Bekijk + maken**.

### <a name="review--create-tab"></a>Tabblad controleren en maken

Controleer de instellingen op het tabblad **beoordelen en maken** en selecteer vervolgens **maken** om het virtuele netwerk, het open bare IP-adres en de toepassings gateway te maken. Het kan enkele minuten duren om de toepassingsgateway te maken in Azure.

Wacht totdat de implementatie is voltooid voordat u doorgaat met de volgende sectie.


## <a name="create-virtual-machines"></a>Virtuele machines maken

In dit voor beeld maakt u drie virtuele machines die worden gebruikt als back-endservers voor de toepassings gateway. U installeert ook IIS op de virtuele machines om te controleren of de toepassings gateway is gemaakt.

1. Selecteer **Een resource maken** in de Azure-portal. Het venster **Nieuw** wordt weergegeven.
2. Selecteer **Compute** en selecteer vervolgens **Windows Server 2016 Data Center** in de lijst **populair** . De pagina **Een virtuele machine maken** wordt weergegeven.

   Application Gateway kunt verkeer routeren naar elk type virtuele machine dat wordt gebruikt in de back-endadresgroep. In dit voor beeld gebruikt u een Windows Server 2016 Data Center.
1. Voer deze waarden in op het tabblad **Basisinformatie** voor de volgende instellingen voor de virtuele machine:

    - **Resourcegroep**: Selecteer **myResourceGroupAG** als naam van de resourcegroep.
    - **Naam van virtuele machine**: Voer *myVM1* in als de naam van de virtuele machine.
    - **Gebruikersnaam**: Typ *azureuser* als gebruikersnaam van de beheerder.
    - **Wachtwoord**: Typ *Azure123456!* als beheerderswachtwoord.
4. Accepteer de overige standaardwaarden en klik op **Volgende: Schijven**.  
5. Accepteer de standaardwaarden op het tabblad **Schijven** en selecteer **Volgende: Netwerken**.
6. Zorg ervoor dat, op het tabblad **Netwerken**, **myVNet** is geselecteerd bij **Virtueel netwerk** en dat **Subnet** is ingesteld op **myBackendSubnet**. Accepteer de overige standaardwaarden en klik op **Volgende: Beheer**.

   Application Gateway kunt communiceren met exemplaren buiten het virtuele netwerk waarin deze zich bevindt, maar u moet ervoor zorgen dat er een IP-verbinding is.
1. Op het tabblad **Beheer** stelt u **Diagnostische gegevens over opstarten** in op **Uit**. Accepteer de overige standaardwaarden en selecteer **Beoordelen en maken**.
2. Controleer de instellingen op het tabblad **Beoordelen en maken**, corrigeer eventuele validatiefouten en selecteer vervolgens **Maken**.
3. Wacht tot de virtuele machine is gemaakt voordat u verder gaat.

### <a name="install-iis"></a>IIS installeren

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

1. Open de interactieve shell en controleer of deze is ingesteld op **Power shell**.

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

3. Maak twee meer virtuele machines en Installeer IIS met behulp van de stappen die u zojuist hebt voltooid. Gebruik *myVM2* en *myVM3* voor de namen van virtuele machines en voor de **VMName** -waarden in set-AzVMExtension.

## <a name="add-backend-servers-to-backend-pools"></a>Back-endservers toevoegen aan back-end-Pools

1. Selecteer **Alle resources** en vervolgens **myAppGateway**.

2. Selecteer **Back-endpools** in het linkermenu.

3. Selecteer **appGatewayBackendPool**.

4. Onder **Doelen** selecteert u **Virtuele machine** in de vervolgkeuzelijst.

5. Onder **virtuele machine** en **netwerk interfaces**selecteert u de virtuele machine **myVM1** en de netwerk interface is gekoppeld aan de vervolg keuzelijst.

    ![Back-endservers toevoegen](./media/create-url-route-portal/backend-pool.png)

6. Selecteer **Opslaan**.
7. Herhaal dit om *myVM2* en interface toe te voegen aan de *imagesBackendPool*, en klik vervolgens *MyVM3* en interface aan de *videoBackendPool*.

Wacht tot de implementatie is voltooid voordat u verdergaat met de volgende stap.

## <a name="test-the-application-gateway"></a>De toepassingsgateway testen

1. Selecteer **alle resources**en selecteer vervolgens **myAGPublicIPAddress**.

    ![Registreer het openbare IP-adres van de toepassingsgateway](./media/create-url-route-portal/application-gateway-record-ag-address.png)

2. Kopieer het openbare IP-adres en plak het in de adresbalk van de browser. Bijvoorbeeld http://40.121.222.19.

    ![Basis-URL testen in de toepassingsgateway](./media/create-url-route-portal/application-gateway-iistest.png)

3. Wijzig de URL in http://&lt;IP-adres&gt;: 8080/images/test.htm, vervang &lt;het IP-&gt; adres door uw IP-adres en u ziet iets als in het volgende voor beeld:

    ![Afbeeldingen-URL in toepassingsgateway testen](./media/create-url-route-portal/application-gateway-iistest-images.png)

4. Wijzig de URL in http://&lt;IP-adres&gt;: 8080/video/test.htm, vervang &lt;het IP-&gt; adres door uw IP-adres en u ziet iets als in het volgende voor beeld:

    ![Video-URL testen in de toepassingsgateway](./media/create-url-route-portal/application-gateway-iistest-video.png)

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u de bij de toepassingsgateway gemaakte resources niet meer nodig hebt, verwijdert u de resourcegroep. Als u de resourcegroep verwijdert, worden ook de toepassingsgateway en alle gerelateerde resources verwijderd. 

Ga als volgt te werk om de resourcegroep te verwijderen:

1. Selecteer **Resourcegroepen** in het linkermenu van de Azure-portal.
2. Zoek en selecteer **myResourceGroupAG** in de lijst op de pagina **Resourcegroepen**.
3. Selecteer **Resourcegroep verwijderen** op de **pagina van de resourcegroep**.
4. Voer *myResourceGroupAG* in bij **Typ de naam van de resource groep** en selecteer vervolgens **verwijderen**.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over wat u kunt doen met Azure-toepassing gateway](application-gateway-introduction.md)
